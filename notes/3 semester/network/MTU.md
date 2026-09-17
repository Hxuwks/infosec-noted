# Введение в проблему

Представьте: вы сетевой администратор, и к вам приходят с тикетом *«не открывается сайт»*. Вы начинаете первичную диагностику и видите странную картину: ICMP-запросы доходят за считанные миллисекунды, сервер исправен, порт открыт, а в логах нет ни единой явной ошибки. Для начинающего специалиста такая заявка быстро превращается в головную боль — ведь привычные утилиты говорят, что «сеть работает».

Однако в сетевом взаимодействии есть параметры, отсутствие видимых сбоев в которых вовсе не гарантирует передачу данных. И один из главных таких параметров — **MTU (Maximum Transmission Unit)**.

Сегодня мы разберем, как незаметное расхождение в размерах кадров может «уронить» передачу трафика, что такое *PMTUD Black Hole* и почему неправильная настройка MTU приводит к коварным проблемам в сетях.

---

# Что такое MTU и на что он влияет

**MTU (Maximum Transmission Unit)** — максимальный размер IP-пакета в байтах, который может быть передан через сетевой интерфейс без деления на части. В свою очередь, **TCP MSS (Maximum Segment Size)** определяет максимальный объем полезной нагрузки (Payload) на транспортном уровне в рамках одного TCP-сегмента, за исключением накладных расходов на IP- и TCP-заголовки.

Но почему данные не передаются единым непрерывным потоком за один раз?

Передача монолитного пакета размером в сотни мегабайт или гигабайт создала бы критическую нагрузку на буферную память и вычислительные ресурсы сетевого оборудования. Более того, это привело бы к блокировке очереди вывода (Head-of-Line Blocking): весь остальной трафик был бы вынужден ожидать завершения передачи гигантского кадра. В результате чувствительные к задержкам интерактивные сессии (например, SSH или VoIP) обрывались бы по таймауту. Именно для предотвращения подобных коллапсов и обеспечения мультиплексирования трафика применяется **IP-фрагментация**.

Поэтапный механизм работы IP-фрагментации:

1. **Инициация фрагментации.** На маршрутизатор поступает IP-пакет, размер которого превышает значение MTU исходящего сетевого интерфейса. Если параметры пакета разрешают деление, маршрутизатор принимает решение о его фрагментации.


2. **Формирование фрагментов.** Исходный пакет разделяется на несколько частей. При этом ядро маршрутизатора дублирует базовый IP-заголовок для каждого фрагмента и модифицирует в нем следующие служебные поля:


* **Identification (IP ID):** уникальный идентификатор исходного пакета. Он одинаков у всех фрагментов одной серии, что позволяет конечному узлу идентифицировать их принадлежность к единой датаграмме.


* **Fragment Offset (Смещение):** указывает позицию полезной нагрузки конкретного фрагмента относительно начала исходного пакета (измеряется в 8-байтовых блоках).




3. **Сборка на целевом узле.** Применив полученное смещение, конечное устройство-получатель восстанавливает исходную последовательность данных и собирает фрагменты в первоначальный пакет.



Помимо идентификатора и смещения, ключевую роль в управлении процессом играют два флага в IP-заголовке:

* **DF (Don't Fragment):** запрещает фрагментацию. Если пакет с установленным флагом $DF=1$ превышает MTU интерфейса, маршрутизатор отбрасывает его и возвращает отправителю служебное сообщение `ICMP Type 3 Code 4` (*Fragmentation Needed*).


* **MF (More Fragments):** указывает на наличие последующих фрагментов. Значение $MF=1$ информирует получателя о том, что данный фрагмент не является завершающим. У последнего фрагмента серии этот флаг сбрасывается в $MF=0$.



---

# Практика

## Создание проблемных условий

Сформируем тестовый стенд Containerlab со следующей топологией:

```yaml
name: mtu-lab

topology:
  nodes:
    rtr-01:
      kind: linux
      image: quay.io/frrouting/frr:10.5.1
      exec:
        - sysctl -w net.ipv4.ip_forward=1
        - ip link set eth1 up
        - ip link set eth2 up
        - ip link set dev eth2 mtu 1300
        - ip addr add 192.168.1.1/24 dev eth1
        - ip addr add 10.0.0.1/30 dev eth2
        - ip route add 192.168.2.0/24 via 10.0.0.2 dev eth2

    rtr-02:
      kind: linux
      image: frrouting/frr:latest
      exec:
        - sysctl -w net.ipv4.ip_forward=1
        - ip link set eth1 up
        - ip link set eth2 up
        - ip addr add 192.168.2.1/24 dev eth2
        - ip addr add 10.0.0.2/30 dev eth1
        - ip route add 192.168.1.0/24 via 10.0.0.1 dev eth1

    pc-01:
      kind: linux
      image: alpine:latest
      exec:
        - ip link set eth1 up
        - ip addr add 192.168.1.2/24 dev eth1
        - ip route replace default via 192.168.1.1 dev eth1

    pc-02:
      kind: linux
      image: nginx:alpine
      exec:
        - ip link set eth1 up
        - ip addr add 192.168.2.2/24 dev eth1
        - ip route replace default via 192.168.2.1 dev eth1

  links:
    - endpoints: ["pc-01:eth1", "rtr-01:eth1"]
    - endpoints: ["rtr-01:eth2", "rtr-02:eth1"]
    - endpoints: ["rtr-02:eth2", "pc-02:eth1"]

```

Для первичной проверки связности авторизуемся на узле `pc-01` и убедимся в доступности веб-сервера `pc-02` с помощью ICMP-запросов базового размера (56 байт полезной нагрузки) и тестового HTTP-запроса:

```bash
/ # ping -c 3 192.168.2.2
PING 192.168.2.2 (192.168.2.2): 56 data bytes
64 bytes from 192.168.2.2: seq=0 ttl=62 time=0.200 ms
64 bytes from 192.168.2.2: seq=1 ttl=62 time=0.172 ms
64 bytes from 192.168.2.2: seq=2 ttl=62 time=0.176 ms

--- 192.168.2.2 ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 0.172/0.182/0.200 ms

/ # wget http://192.168.2.2
Connecting to 192.168.2.2 (192.168.2.2:80)
saving to 'index.html'
index.html           100% |**********************************************************|   896  0:00:00 ETA
'index.html' saved

```

Базовая сетевая связность и доступность L7-сервиса подтверждены.

Однако проследим поведение системы при попытке передачи датаграммы, размер которой заведомо превышает MTU транзитного участка пути. При попытке отправить ICMP-пакет с полезной нагрузкой 1400 байт (итоговый размер IP-пакета с учетом заголовков — 1428 байт) и явно установленным флагом запрета фрагментации ($DF=1$) стековая утилита возвращает системную ошибку о невозможности локальной отправки пакета столь большого объема:

```bash
/ # ping -c 2 -M do -s 1400 192.168.2.2
PING 192.168.2.2 (192.168.2.2) 1400(1428) bytes of data.
ping: sendmsg: Message too large
ping: sendmsg: Message too large

--- 192.168.2.2 ping statistics ---
2 packets transmitted, 0 received, +2 errors, 100% packet loss, time 1030ms

```

Для локализации «узкого горлышка» и динамического определения значения Path MTU на сетевом маршруте выполним трассировку с помощью утилиты `tracepath`:

```bash
/ # tracepath 192.168.2.2
 1?: [LOCALHOST]                      pmtu 9500
 1:  192.168.1.1                                           0.186ms
 1:  192.168.1.1                                           0.139ms
 2:  192.168.1.1                                           0.167ms pmtu 1300
 2:  10.0.0.2                                              0.227ms
 3:  192.168.2.2                                           0.290ms reached
     Resume: pmtu 1300 hops 3 back 3

```

Результат анализа показывает, что на втором хопе (интерфейс `eth2` маршрутизатора `rtr-01` с IP-адресом `192.168.1.1`) происходит снижение PMTUD до 1300 байт. Устройство сгенерировало служебное уведомление `ICMP Type 3 Code 4` (*Fragmentation Needed and DF set*), указав максимальный допустимый размер датаграммы для данного транзитного сегмента.

Для устранения возникшего ограничения в сетевой инженерии применяются два основных подхода:

1. **Прямое увеличение MTU** на лимитирующем физическом или виртуальном интерфейсе сетевого устройства.


2. **Использование механизмов межсетевого экрана (MSS Clamping)**, позволяющих модифицировать значение поля `TCP MSS` в служебных пакетах `SYN` «на лету» для корректного согласования размера сегментов между конечными узлами.



---

## Решение проблемы: Перехват и модификация TCP MSS (MSS Clamping)

Когда прямое увеличение MTU на транзитном оборудовании невозможно (например, из-за ограничений провайдера или использования туннелей GRE, IPsec, VXLAN), эффективным решением становится **MSS Clamping**.

Механизм перехватывает TCP-пакеты с установленным флагом `SYN` на стадии рукопожатия (TCP 3-Way Handshake) и принудительно перезаписывает значение опции `Maximum Segment Size`. В результате клиент и сервер заранее договариваются об уменьшенном размере полезной нагрузки, что предотвращает появление пакетов, превышающих Path MTU.

Для реализации этого решения в Linux используется межсетевой экран `iptables` и таблица `mangle`, специально предназначенная для модификации заголовков пакетов.

Правило подменяет MSS в `SYN`-пакетах, проходящих через маршрутизатор, рассчитывая его автоматически исходя из MTU исходящего интерфейса:

```bash
iptables -t mangle -A FORWARD -p tcp --tcp-flags SYN,RST SYN -j TCPMSS --clamp-mss-to-pmtu

```

**Как это работает:**

1. Флаги `SYN,RST SYN` отбирают только служебные пакеты установки TCP-соединения.


2. Таргет `-j TCPMSS` пересчитывает опцию MSS: $MSS = MTU - 20 \text{ (IP Header)} - 20 \text{ (TCP Header)}$.


3. Для интерфейса `eth2` с MTU 1300 байт значение MSS автоматически усекается до **1260 байт**.



---

## Верификация решения

> **Примечание:** Обратите внимание, что правило `TCPMSS` работает исключительно на L4-уровне для TCP-трафика. Протокол ICMP (`ping`) не имеет поля MSS в своем заголовке, поэтому проверки через `ping -s` с превышением MTU по-прежнему будут сбрасываться. Для проверки работы подмены MSS используются L7-запросы (например, `curl` / `wget`).

#### 1. Проверка передачи данных с клиента `pc-01`

Выполним повторный запрос к веб-серверу `pc-02` для скачивания ресурсов. Теперь TCP-сессия успешно устанавливается, а объемные сегменты автоматически нарешены под рамки MTU и корректно проходят через лимитированный участок:

```bash
/ # curl 192.168.2.2
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, nginx is successfully installed and working.
Further configuration is required for the web server, reverse proxy,
API gateway, load balancer, content cache, or other features.</p>

<p>For online documentation and support please refer to
<a href="https://nginx.org/">nginx.org</a>.<br/>
To engage with the community please visit
<a href="https://community.nginx.org/">community.nginx.org</a>.<br/>
For enterprise grade support, professional services, additional
security features and capabilities please refer to
<a href="https://f5.com/nginx">f5.com/nginx</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

```

#### 2. Анализ сетевых дампов на `rtr-01`

Для подтверждения работы подмены заголовков запустим перехват пакетов с помощью `tcpdump` на маршрутизаторе `rtr-01` во время инициализации сессии:

```bash
/ # tcpdump -i any -nn -v 'tcp[tcpflags] & (tcp-syn) != 0'
tcpdump: WARNING: any: That device doesn't support promiscuous mode
(Promiscuous mode not supported on the "any" device)
tcpdump: listening on any, link-type LINUX_SLL2 (Linux cooked v2), snapshot length 262144 bytes
18:10:26.344930 eth1  In  IP (tos 0x0, ttl 64, id 31203, offset 0, flags [DF], proto TCP (6), length 60)
    192.168.1.2.49958 > 192.168.2.2.80: Flags [S], cksum 0x4d4f (correct), seq 1965823575, win 56760, options [mss 9460,sackOK,TS val 2913172674 ecr 0,nop,wscale 10], length 0
18:10:26.344995 eth2  Out IP (tos 0x0, ttl 63, id 31203, offset 0, flags [DF], proto TCP (6), length 60)
    192.168.1.2.49958 > 192.168.2.2.80: Flags [S], cksum 0x6d57 (correct), seq 1965823575, win 56760, options [mss 1260,sackOK,TS val 2913172674 ecr 0,nop,wscale 10], length 0
18:10:26.345101 eth2  In  IP (tos 0x0, ttl 63, id 0, offset 0, flags [DF], proto TCP (6), length 60)
    192.168.2.2.80 > 192.168.1.2.49958: Flags [S.], cksum 0xbbb3 (correct), seq 1637359239, ack 1965823576, win 56688, options [mss 9460,sackOK,TS val 2031130786 ecr 2913172674,nop,wscale 10], length 0
18:10:26.345110 eth1  Out IP (tos 0x0, ttl 62, id 0, offset 0, flags [DF], proto TCP (6), length 60)
    192.168.2.2.80 > 192.168.1.2.49958: Flags [S.], cksum 0xdbbb (correct), seq 1637359239, ack 1965823576, win 56688, options [mss 1260,sackOK,TS val 2031130786 ecr 2913172674,nop,wscale 10], length 0
^C
4 packets captured
4 packets received by filter
0 packets dropped by kernel
/ #

```

Анализ захваченного трафика наглядно показывает работу правила `iptables`:

1. Входящий `SYN`-пакет от `pc-01` на интерфейсе `eth1` анонсирует `options [mss 9460]` (так как на хосте включен Jumbo Frame).


2. При пересылке в интерфейс `eth2` маршрутизатор `rtr-01` перезаписывает поле на `options [mss 1260]` ($1300 \text{ MTU} - 40 \text{ bytes Header}$).


3. Для ответного пакета `SYN-ACK` от сервера `pc-02` происходит аналогичная подмена при транзите с `eth2` на `eth1`.



#### 3. Контрольный эксперимент (без фаервола)

Для подтверждения гипотезы сбросим созданное правило с таблицы `mangle` и повторно зафиксируем 3-Way Handshake:

```bash
/ # iptables -t mangle -F FORWARD
/ # tcpdump -i any -nn -v 'tcp[tcpflags] & (tcp-syn) != 0'
tcpdump: WARNING: any: That device doesn't support promiscuous mode
(Promiscuous mode not supported on the "any" device)
tcpdump: listening on any, link-type LINUX_SLL2 (Linux cooked v2), snapshot length 262144 bytes
18:13:00.971778 eth1  In  IP (tos 0x0, ttl 64, id 65242, offset 0, flags [DF], proto TCP (6), length 60)
    192.168.1.2.39228 > 192.168.2.2.80: Flags [S], cksum 0x690e (correct), seq 168175844, win 56760, options [mss 9460,sackOK,TS val 1704799117 ecr 0,nop,wscale 10], length 0
18:13:00.971832 eth2  Out IP (tos 0x0, ttl 63, id 65242, offset 0, flags [DF], proto TCP (6), length 60)
    192.168.1.2.39228 > 192.168.2.2.80: Flags [S], cksum 0x690e (correct), seq 168175844, win 56760, options [mss 9460,sackOK,TS val 1704799117 ecr 0,nop,wscale 10], length 0
18:13:00.971921 eth2  In  IP (tos 0x0, ttl 63, id 0, offset 0, flags [DF], proto TCP (6), length 60)
    192.168.2.2.80 > 192.168.1.2.39228: Flags [S.], cksum 0x1f32 (correct), seq 1904059355, ack 168175845, win 56688, options [mss 9460,sackOK,TS val 1552865323 ecr 1704799117,nop,wscale 10], length 0
18:13:00.971928 eth1  Out IP (tos 0x0, ttl 62, id 0, offset 0, flags [DF], proto TCP (6), length 60)
    192.168.2.2.80 > 192.168.1.2.39228: Flags [S.], cksum 0x1f32 (correct), seq 1904059355, ack 168175845, win 56688, options [mss 9460,sackOK,TS val 1552865323 ecr 1704799117,nop,wscale 10], length 0
^C
4 packets captured
4 packets received by filter
0 packets dropped by kernel
/ #

```

Без действующих правил межсетевого экрана маршрутизатор транслирует пакеты без изменений — значение остается `options [mss 9460]`. В результате узлы пытаются передавать полноразмерные сегменты, что при MTU 1300 на транзитном участке приводит к сбросу пакетов с флагом `DF=1` и неработоспособности соединения.

---

# Заключение

Проблема «подвисания» соединений при исправном ICMP — классический симптом расхождения Path MTU и возникновения PMTUD Black Hole. Внедрение **MSS Clamping** на транзитном узле позволяет нивелировать негативные последствия узких горлышек в сетевой инфраструктуре без необходимости перенастройки конечных хостов.