## Введение

В рамках данного материала рассматривается практический сетевой анализ, позволяющий изучить перемещение пакетов в инфраструктуре, их служебную информацию и модификацию при прохождении через узлы связи. Цикл включает разбор модели OSI снизу вверх, анализ базовых протоколов и элементы сетевого программирования. Перед началом работы рекомендуется ознакомиться с базовым синтаксисом команд `cd`, `ls`, конфигурации `containerlab` и утилиты `ip`. Основное внимание уделяется углубленному разбору Ethernet-кадров, их ручному формированию, передаче по виртуальному линку, перехвату и побайтовому анализу.

## Канальный уровень и структура Ethernet-кадра

Ethernet-кадры функционируют на канальном уровне (L2) модели OSI. Передача данных внутри локального сегмента сети опирается на MAC-адреса.

* MAC-адрес представляет собой 48-битный (6 байт) уникальный идентификатор сетевого интерфейса, назначаемый производителем для адресации отправителя и получателя во фрейме, а также в целях безопасности.
* Структура OUI (Organizationally Unique Identifier) определяет производителя в первых трех байтах, где первый бит нулевого байта задает тип рассылки (0 — unicast, 1 — multicast), а второй бит указывает тип администрирования (0 — глобально уникальный от завода, 1 — локально измененный программно).
* Широковещательный (Broadcast) адрес (`FF:FF:FF:FF:FF:FF`) используется для рассылки кадров всем узлам в пределах широковещательного домена.

Базовая структура кадра Ethernet II, используемая в работе:

```plaintext
╔══════════════════════════════════════════════════════════════════════╗
║                      КАДР ETHERNET II (Data Link)                    ║
╠══════════════════════════════════════════════════════════════════════╣
║ PREAMBLE (7) │ SFD │      DST MAC (6)      │      SRC MAC (6)        ║
║  10101010..  │0xAB │  AA:BB:CC:DD:EE:FF    │  11:22:33:44:55:66      ║
╠══════════════════════════════════════════════════════════════════════╣
║ TYPE/LEN (2) │                                                       ║
║   0x0800     ↓                                                       ║
║  (IPv4)    ┌─────────────────────────────────────────────────────┐   ║
║            │              DATA (46 ─ 1500 байт)                  │   ║
║            │   (IP-пакет, ARP, или данные верхнего уровня)       │   ║
║            │   "Hello, канальный уровень! 01001010..."           │   ║
║            └─────────────────────────────────────────────────────┘   ║
╠══════════════════════════════════════════════════════════════════════╣
║ FCS (4) │ CRC-32 (проверка целостности)                              ║
╚══════════════════════════════════════════════════════════════════════╝

```

## Поле EtherType и стандартные идентификаторы

Поле EtherType занимает 2 байта в заголовке Ethernet II и определяет протокол верхнего уровня либо указывает размер кадра. Основные стандартизированные значения EtherType:

* `0x0800` — Internet Protocol version 4 (IPv4).
* `0x0806` — Address Resolution Protocol (ARP).
* `0x86DD` — Internet Protocol version 6 (IPv6).
* `0x8100` — IEEE 802.1Q VLAN-тегирование.
* `0x88B5` — Экспериментальный диапазон для тестирования проприетарных протоколов канального уровня.

## Разбор и запуск топологии

Роль виртуального сетевого стенда выполняет топология из двух хостов с образами `wbitt/network-multitool:3.22.2`. Сеть реализована с помощью инструмента Containerlab на основе конфигурации `*.clab.yml`. Образ выбран за счет легковесности на базе ядра Alpine и наличия необходимых сетевых утилит.

Конфигурация топологии (`01-two-hosts.clab.yml`):

```yaml
name: 01-two-hosts 

topology:
  nodes: 
    pc-01: 
      kind: linux 
      image: wbitt/network-multitool:3.22.2 
      exec:
        - ip addr add 192.168.1.2/24 dev eth1
        - ip link set eth1 up

    pc-02:
      kind: linux
      image: wbitt/network-multitool:3.22.2 
      exec:
        - ip addr add 192.168.1.3/24 dev eth1
        - ip link set eth1 up

  links:
    - endpoints: ["pc-01:eth1", "pc-02:eth1"]

```

Состояние развернутой топологии:

| Name | Kind / Image | State | IPv4 Address |
| --- | --- | --- | --- |
| `clab-01-two-hosts-pc-01` | linux `wbitt/network-multitool:3.22.2` | running | `172.20.20.2` |
| `clab-01-two-hosts-pc-02` | linux `wbitt/network-multitool:3.22.2` | running | `172.20.20.3` |

## Вывод сетевой конфигурации хостов

Перед отправкой кадра необходимо определить IP и MAC-адреса хостов для корректной адресации получателя.

Проверка параметров интерфейса `eth1` на `pc-01`:

```bash
/ # ip addr show eth1 | grep inet
    inet 192.168.1.2/24 scope global eth1
    inet6 fe80::a8c1:abff:feb9:d1c6/64 scope link proto kernel_ll 

/ # ip link show eth1
30: eth1@if29: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9500 qdisc noqueue state UP mode DEFAULT group default 
    link/ether aa:c1:ab:b9:d1:c6 brd ff:ff:ff:ff:ff:ff link-netnsid 1

```

Проверка параметров на `pc-02`:

```bash
/ # ip addr show eth1 | grep inet
    inet 192.168.1.3/24 scope global eth1
    inet6 fe80::a8c1:abff:fe63:f754/64 scope link proto kernel_ll 

/ # ip link show eth1
29: eth1@if30: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9500 qdisc noqueue state UP mode DEFAULT group default 
    link/ether aa:c1:ab:63:f7:54 brd ff:ff:ff:ff:ff:ff link-netnsid 1

```

Сводная таблица параметров локального сегмента:

| Хост | IPv4-адрес | MAC-адрес (link/ether) |
| --- | --- | --- |
| **pc-01** | `192.168.1.2` | `aa:c1:ab:b9:d1:c6` |
| **pc-02** | `192.168.1.3` | `aa:c1:ab:63:f7:54` |

## Реализация скриптов на Python

### Использование стандартных сокетов (`socket.AF_PACKET`)

Сырые сокеты (`SOCK_RAW`) с семейством адресов `AF_PACKET` применяются в Linux для обеспечения прямого доступа к сетевому интерфейсу в обход стека маршрутизации ОС.

Скрипт-отправитель (`send_frame.py` на `pc-01`):

```python
import socket

IFACE = "eth1"

dst_mac = b"\xaa\xc1\xab\x63\xf7\x54"
src_mac = b"\xaa\xc1\xab\xb9\d1\xc6"
ethertype = b"\x88\xb5"
payload = b"Hello from raw socket payload!"

frame = dst_mac + src_mac + ethertype + payload

s = socket.socket(socket.AF_PACKET, socket.SOCK_RAW)
s.bind((IFACE, 0))

s.send(frame)
print("[+] Сырой Ethernet-кадр успешно отправлен с pc-01 на pc-02!")

```

Скрипт-приемник (`recv_frame.py` на `pc-02`):

```python
import socket

IFACE = "eth1"

s = socket.socket(socket.AF_PACKET, socket.SOCK_RAW)
s.bind((IFACE, 0))

print("[*] Ожидание входящих Ethernet-кадров на интерфейсе eth1...")

while True:
    frame, addr = s.recvfrom(65535)

    dst_mac = frame[0:6]
    src_mac = frame
    ethertype = frame
    payload = frame[14:]

    if ethertype == b"\x88\xb5":
        dst_str = ":".join(f"{b:02x}" for b in dst_mac)
        src_str = ":".join(f"{b:02x}" for b in src_mac)

        print("\n[+] Успешно перехвачен кадр со своим EtherType!")
        print(f"    Dst MAC   : {dst_str}")
        print(f"    Src MAC   : {src_str}")
        print(f"    EtherType : 0x{ethertype.hex()}")
        print(f"    Payload   : {payload.decode('utf-8', errors='ignore')}")
        break

```

### Использование библиотеки Scapy

Библиотека Scapy предоставляет объектно-ориентированный интерфейс для автоматизации сетевых задач и упрощения работы с кадрами. Установка в контейнерах выполняется командой: `apk add --no-cache py3-scapy`.

Отправка кадра через Scapy (`send_frame.py`):

```python
from scapy.all import Ether, sendp

frame = (
    Ether(dst="aa:c1:ab:63:f7:54", src="aa:c1:ab:b9:d1:c6", type=0x88B5)
    / "Hello from Scapy L2 raw frame!"
)

sendp(frame, iface="eth1", verbose=False)
print("[+] Кадр успешно отправлен через Scapy!")

```

Перехват кадра через Scapy (`recv_frame.py`):

```python
from scapy.all import Ether, sniff

def packet_callback(packet):
    if packet.haslayer(Ether) and packet[Ether].type == 0x88B5:
        print("\n[+] Перехвачен кадр через Scapy Sniffer!")
        print(f"    Dst MAC   : {packet[Ether].dst}")
        print(f"    Src MAC   : {packet[Ether].src}")
        print(f"    EtherType : {packet[Ether].type:04x}")
        print(f"    Payload   : {packet.payload}")

print("[*] Запуск сниффера Scapy на интерфейсе eth1...")
sniff(iface="eth1", prn=packet_callback, filter="ether proto 0x88B5")

```

## Запуск скриптов и анализ трафика (tcpdump и Wireshark)

Теперь, когда у нас есть готовые скрипты для отправки и приема кадров как на стандартных сокетах, так и через библиотеку Scapy, проверим их работу в реальных условиях нашего виртуального стенда. Для этого мы запустим приемник на pc-02, отправим наш кастомный кадр с pc-01, а затем перехватим и детально разберем трафик с помощью системных утилит анализа.

### 1. Проверка работы скриптов

Откройте терминал первого контейнера и запустите скрипт-приемник:

```bash
docker exec -it clab-01-two-hosts-pc-02 sh
python3 recv_frame.py

```

Скрипт перейдет в режим блокирующего ожидания (`[*] Ожидание входящих Ethernet-кадров...`).

В соседнем окне терминала зайдите в контейнер отправителя и выполните скрипт генерации кадра:

```bash
docker exec -it clab-01-two-hosts-pc-01 sh
python3 send_frame.py

```

В терминале pc-02 мгновенно появится подтверждение успешного перехвата с расшифрованными параметрами:

```plaintext
[+] Успешно перехвачен кадр со своим EtherType!
    Dst MAC   : aa:c1:ab:63:f7:54
    Src MAC   : aa:c1:ab:b9:d1:c6
    EtherType : 0x88b5
    Payload   : Hello from raw socket payload!

```

### 2. Отлов трафика с помощью tcpdump

Чтобы убедиться на уровне операционной системы, что кадр действительно передается по физическому линку между контейнерами, запустим на pc-02 классический консольный анализатор `tcpdump`. Настроим его на прослушивание интерфейса `eth1`, фильтрацию по нашему кастомному протоколу и вывод шестнадцатеричного дампа (`-XX`):

```bash
tcpdump -i eth1 -nn -XX 'ether proto 0x88b5'

```

Повторите отправку кадра с pc-01. В выводе `tcpdump` появится следующая структура:

```bash
hxuwks@thinkpad:~$ sudo tcpdump -i eth1 -nn -XX 'ether proto 0x88b5'
[sudo] password for hxuwks: 
12:34:56.789012 aa:c1:ab:b9:d1:c6 > aa:c1:ab:63:f7:54, ethertype Experimental (0x88b5), length 46:
    0x0000:  aac1 ab63 f754 aac1 abb9 d1c6 88b5 4865  ..c.tT........He
    0x0010:  6c6c 6f20 6672 6f6d 2072 6177 2073 6f63  llo from raw soc
    0x0020:  6b65 7420 7061 796c 6f61 6421            ket payload!

```

### 3. Побайтовый анализ структуры кадра

Сопоставим полученный шестнадцатеричный дамп с теоретической моделью заголовка Ethernet II:

* **`aa c1 ab 63 f7 54`** (байты 0–5) — Destination MAC (`aa:c1:ab:63:f7:54`, принадлежащий pc-02).
* **`aa c1 ab b9 d1 c6`** (байты 6–11) — Source MAC (`aa:c1:ab:b9:d1:c6`, принадлежащий pc-01).
* **`88 b5`** (байты 12–13) — EtherType (`0x88B5`, указывающий на наш экспериментальный протокол).
* **`48 65 6c 6c 6f ...`** (начиная с 14-го байта) — Payload (полезная нагрузка, содержащая нашу текстовую строку "Hello from raw socket payload!").

> **Примечание:** Если размер полезной нагрузки меньше минимально допустимого стандартом Ethernet (46 байт), драйвер сетевой карты автоматически дополняет кадр нулями (padding) до достижения минимальной длины.

### 4. Сохранение и детальный анализ через Wireshark / tshark

Для глубокого ретроспективного анализа трафик можно сохранить в стандартный `.pcap` файл с помощью `tcpdump`:

```bash
tcpdump -i eth1 -w l2_frame.pcap 'ether proto 0x88b5'

```

Полученный файл можно открыть в графическом интерфейсе Wireshark для визуального изучения структуры полей, либо проанализировать в консоли с помощью `tshark` с ключом детального вывода (`-V`):

```bash
tshark -r l2_frame.pcap -V

```

Команда выведет послойное дерево пакета, подтверждая корректность формирования кадра на канальном уровне без привлечения стеков IP/TCP.

## Справочные материалы (Sheet-lists)

### 1. Справочник флагов `tcpdump`

| Флаг / Опция | Краткий смысл | Пример использования |
| --- | --- | --- |
| `-i` | Указание сетевого интерфейса. | `tcpdump -i eth1` |
| `-n` / `-nn` | Отключение резолва: `-n` (только IP), `-nn` (IP и порты). | `tcpdump -i eth1 -nn` |
| `-c` | Лимит количества захватываемых пакетов. | `tcpdump -i eth1 -c 5` |
| `-w` | Сохранение сырого трафика в `.pcap`. | `tcpdump -i eth1 -w traffic.pcap` |
| `-r` | Чтение пакетов из `.pcap` файла. | `tcpdump -r traffic.pcap -nn` |
| `-v` / `-vv` / `-vvv` | Уровень verbose: `-v` (базовый IP), `-vv` (TCP-опции), `-vvv` (максимум). | `tcpdump -i eth1 -vv -nn` |
| `-X` / `-XX` | HEX/ASCII payload: `-X` (L3+), `-XX` (включая L2 Ethernet-заголовок). | `tcpdump -i eth1 -XX 'ether proto 0x88b5'` |
| `-S` | Вывод абсолютных (вместо относительных) TCP Sequence Numbers. | `tcpdump -i eth1 -S tcp` |
| `-q` | Тихое отображение (сокращенный вывод шапки). | `tcpdump -i eth1 -q icmp` |
| `-p` | Отключение promiscuous mode (только свой трафик). | `tcpdump -i eth1 -p` |

### 2. Справочник флагов `tshark`

| Флаг / Опция | Краткий смысл | Пример использования |
| --- | --- | --- |
| `-i` | Выбор интерфейса для захвата «на лету». | `tshark -i eth1` |
| `-c` | Остановка после заданного числа пакетов. | `tshark -i eth1 -c 10` |
| `-f` | BPF-фильтр захвата (синтаксис `tcpdump`). | `tshark -i eth1 -f "tcp port 443"` |
| `-Y` | Фильтр отображения (Display Filter Wireshark). | `tshark -r traffic.pcap -Y "http.request"` |
| `-r` | Чтение пакетов из файла. | `tshark -r traffic.pcap` |
| `-w` | Экспорт потока в `.pcap` в реальном времени. | `tshark -i eth1 -w output.pcap` |
| `-x` | Послойный HEX/ASCII дамп пакета. | `tshark -i eth1 -c 1 -x` |
| `-V` | Полная детализация дерева протоколов (как панель Details). | `tshark -r traffic.pcap -V` |
| `-T fields` + `-e` | Извлечение конкретных полей пакета для парсинга. | `tshark -r traffic.pcap -T fields -e ip.src` |
| `-z` | Генерация встроенных статистических отчетов. | `tshark -r traffic.pcap -z conv,ip` |

