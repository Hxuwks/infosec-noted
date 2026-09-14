## 📊 Общая статистика

- Всего семестров: 7 (семестры 3–9) + Pre-Security (лето)
- Всего часов: ориентир 3400+ (включая новые блоки и Семестр 9)
- Тем: 45+ основных блоков и десятки подтем в списках «Что изучать»
- Фокус: Сети, сетевая безопасность, Linux, C, C++, Go, Python, пентест, Red Team, проектирование безопасных сетей, Highload сетевые сервисы
- Доля Red Team контента: около 55% от всей roadmap
- Доля программирования (C/Rust/Go/Python/Bash): около 25% от всей roadmap
- Баланс: теория/практика 30/70
- Контрольные точки и книги по блокам: в конце семестров 2–3 и в общем списке в конце файла

---

## ☀️ Pre-Security (Лето перед Семестр 3)

**Время:** ~150–200 часов (июнь-август)

**Цель:** Получить базовые навыки работы с сетями и Linux перед интенсивным погружением в семестре 3. Повторение — мать учения.

---

### 1. Базовые сети и Containerlab

**Время:** 80 часов

**Что изучать:**

- Модель OSI: 7 уровней, их назначение, примеры протоколов на каждом уровне
- Физический уровень: кабели (витая пара, оптоволокно), типы соединений
- Канальный уровень: Ethernet, MAC-адреса, коммутаторы (switch),broadcast домен
- Сетевой уровень: IP-адреса, маски подсетей (/24, /16, /8), основы маршрутизации
- Транспортный уровень: TCP vs UDP, порты (1–65535), различия
- Прикладной уровень: HTTP, HTTPS, DNS, FTP, SSH — на уровне понимания
- IP адресация: IPv4, частные адреса (10.x, 192.168.x, 172.16.x), публичные vs приватные
- Подсети: простой subnetting, /24, /30, /16 — без сложного VLSM
- Основы маршрутизации: статические маршруты, default gateway
- ARP: понятие, MAC-адрес vs IP-адрес
- DNS: что такое DNS, A-записи, nslookup
- DHCP: понятие, получение IP автоматически
- **Теория про лабораторную среду:** что такое декларативная топология (YAML), роли узлов в Containerlab (`linux`, `srl`, `frr` и др.), чем L3-лабора на Linux отличается от «кликовой» схемы в симуляторе Cisco; ограничения: **классический L2 (VLAN 802.1Q, STP, VTP) и Wi‑Fi** в Containerlab не воспроизводятся как в учебнике CCNA — эти темы закреплять на **управляемом коммутаторе**, **GNS3** или **EVE-NG** с подходящими образами

**Containerlab (практика):**

- Установка **Docker** и **Containerlab**, проверка `containerlab version`, права и группы для работы без лишнего `sudo`, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Первая топология: файл `*.clab.yml`, команды `containerlab deploy`, `destroy`, `inspect`, `graph`; понимание линков и имён интерфейсов у контейнеров, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Простая сеть: два **linux**-хоста в одной подсети (или с явными `links` в YAML), назначение адресов (`ip addr`), проверка L2/L3, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Ping и трассировка между хостами, просмотр ARP и маршрутов (`ip neigh`, `ip route`), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- **DHCP:** развернуть **dnsmasq** или **Kea** в контейнере Linux, пул адресов, аренда на клиенте (`dhclient` / journal), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- **Статическая маршрутизация:** два (и более) узла **FRRouting (FRR)** или Linux с включённым forwarding, статические маршруты через `vtysh` / `ip route`, проверка `show ip route` и end-to-end ping, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- **Сегментация без классического «коммутатора PT»:** несколько подсетей и отдельные `links` в YAML, маршрутизация между сегментами на FRR/Linux (логический аналог разнесённых VLAN по смыслу «разные подсети»), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- **Политики доступа (аналог простых ACL):** базовые правила **nftables** или **iptables** (filter INPUT/FORWARD, разрешить/запретить TCP/UDP по портам и подсетям), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- **NAT:** схема inside/outside на Linux-шлюзе, **MASQUERADE** в таблице `nat`, проверка выхода во «внешнюю» подсеть лаборатории, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- **Wi‑Fi / домашний роутер:** в Containerlab не моделируется как в PT; зафиксировать конфигурацию **точки доступа или домашнего маршрутизатора** отдельно (железо) или перенести в **EVE-NG/GNS3**; в отчёте — схема и параметры SSID/безопасности на уровне концепций

**Проекты:**

- Репозиторий с топологией `*.clab.yml`: 3–5 **linux**-хостов, адресация, ping, скриншоты/`graph`, README с диаграммой, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Два маршрутизатора (**FRR** или Linux) со статической маршрутизацией и сохранённым конфигом (`/etc/frr/frr.conf` или скрипт деплоя), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Две изолированные подсети в одной топологии Containerlab, хосты в разных сегментах, маршрутизация и политика на шлюзе (**nftables/iptables**), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Внутренняя сеть за Linux-NAT шлюзом (`MASQUERADE`), проверка с внешнего узла лаборатории, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** [Containerlab](https://containerlab.dev/), документация **FRRouting**, курс CCNA (Intro — теория сетей), David Bombal — CCNA (YouTube), Networks Explained (видео), при необходимости L2: GNS3 / EVE-NG

---

### 2. Базовый Linux и сетевые утилиты

**Время:** 70 часов

**Что изучать:**

- Установка Linux: Ubuntu Server или Debian в VirtualBox
- Базовая работа в терминале: ls, cd, pwd, mkdir, rmdir, rm, cp, mv
- Файловая система: /home, /etc, /var, /opt, /usr — понимание структуры
- Права доступа: chmod (символьный и восьмеричный), chown, базовые права (rwx)
- Пользователи: useradd, passwd, sudo, работа с /etc/passwd
- Процессы: ps, top, kill — базовое управление
- Работа с файлами: cat, less, head, tail, nano, vim (основы)
- Поиск: find, grep, locate
- Текстовые утилиты: cut, sort, uniq, wc
- Архивация: tar, gzip, zip

**Сетевые утилиты (базовые):**

- ip addr: просмотр и настройка IP-адресов
- ping: проверка доступности
- netstat / ss: просмотр открытых портов и соединений
- curl / wget: скачивание файлов, простые HTTP-запросы
- ssh: подключение к удалённому серверу
- scp: копирование файлов по SSH
- telnet: простая проверка порта
- dig / nslookup: DNS-запросы
- iptables (базовые примеры): блокировка/разрешение порта
- traceroute / mtr: трассировка маршрута
- nmap (базовый): простое сканирование портов

**Практика:**

- Поднятие Ubuntu Server в VirtualBox, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Настройка статического IP на сервере, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Установка пакетов через apt, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Настройка SSH-сервера и подключение с хоста, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Настройка простого файрвола через iptables, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Создание простого скрипта для бэкапа, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- Linux-сервер в VirtualBox с SSH и настроенным файрволом, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Скрипт мониторинга: ping до 8.8.8.8, уведомление если недоступен, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Простой сканер портов на bash, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** Linux Journey ( beginner section), The Linux Command Line (первые главы), OverTheWire Bandit (первые 10 уровней), видео: Level1Linux (YouTube)

---

## Семестр 3 - Фундамент (Сентябрь - Декабрь)

**Время:** ~320–390 часов

### 1. Сети: TCP/IP стек и фундаментальные протоколы

**Время:** 100 часов

**Что изучать:**

- Модель OSI и соответствие реальным протоколам. Уровни: физический, канальный, сетевой, транспортный, прикладной
- Ethernet: MAC-адреса, кадры, FCS, типы кадров (Ethernet II, 802.3), коммутация (switching), Broadcast домен, коллизионный домен
- ARP: ARP Request/Reply, ARP таблица, ARP poisoning на уровне понимания, Gratuitous ARP, Proxy ARP
- VLAN: 802.1Q, тегирование, Trunk, Native VLAN, Voice VLAN, VTP на уровне концепций
- IPv4: формат заголовка (version, IHL, TTL, протокол, ID фрагмента), маски и CIDR (/8 до /30), подсети, VLSM
- Path MTU Discovery: фрагментация, DF бит, ICMP Destination Unreachable (Fragmentation Needed)
- ICMP: Echo Request/Reply (ping), Destination Unreachable, Time Exceeded, Redirect, параметры проблемы
- TCP: установка соединения (three-way handshake), разрыв (FIN/ACK, RST), скользящее окно (sliding window)
- TCP машина состояний: LISTEN, SYN_SENT, SYN_RECEIVED, ESTABLISHED, FIN_WAIT, CLOSE_WAIT, LAST_ACK, TIME_WAIT
- Контроль перегрузки (congestion control): slow start, congestion avoidance, fast retransmit, fast recovery
- Контроль потока: Window Size, Window Scale опция, Zero Window
- Опции TCP: MSS, Window Scale, SACK, Timestamp, TCP Fast Open
- UDP: когда уместен, диапазоны портов (системные 1–1023, зарегистрированные 1024–49151, динамические 49152–65535)
- DNS: рекурсия и итерация, резолверы, корневые серверы, TLD, зоны, повторные запросы
- DNS записи: A, AAAA, CNAME, MX, TXT, NS, SOA, PTR, SRV, TTL, DNSSEC на уровне концепций
- Утилиты DNS: dig, nslookup, host, resolvectl, настройка /etc/resolv.conf
- HTTP: методы (GET, POST, PUT, DELETE, OPTIONS, HEAD, PATCH), заголовки, коды ответа (1xx–5xx), keep-alive, chunked transfer
- HTTP/2: multiplex, server push, header compression HPACK, binary framing — на уровне концепций
- HTTP/3: QUIC, 0-RTT — на уровне концепций без деталей реализации
- HTTPS: TLS 1.2/1.3 рукопожатие, сертификаты, CA, цепочка доверия
- SMTP: отправка почты, команды MAIL, RCPT, DATA, MIME, DKIM на уровне концепций
- POP3/IMAP: получение почты, различия протоколов, команды RETR, DELE, LIST
- FTP: активный и пассивный режимы, PORT/EPRT, PASV/EPSV, команды
- Сокеты: socket, bind, listen, accept, connect. Семейства AF_INET, AF_INET6
- Сокеты TCP: send, recv, shutdown, close. Порядок байтов сети (htons, htonl, ntohs, ntohl)
- Сокеты UDP: sendto, recvfrom, connect для UDP (одностороннее)
- setsockopt: SO_REUSEADDR, SO_REUSEPORT, TCP_NODELAY, SO_KEEPALIVE, SO_RCVBUF, SO_SNDBUF
- Блокирующий и неблокирующий режимы: O_NONBLOCK, F_SETFL
- Мультиплексирование: select, poll, epoll (level-triggered и edge-triggered)
- RAW sockets: ручная сборка заголовков, отправка сырых пакетов
- Wireshark и tshark: фильтры отображения (display filters), фильтры захвата (capture filters), BPF
- Следование TCP-потоку (Follow TCP Stream), анализ отдельных пакетов
- QoS на уровне концепций: DSCP, IP Precedence, queueing disciplines (fifo, pfifo, sfq)
- Мультикаст: IGMP, адреса 224.0.0.0–239.255.255.255, MAC-маппинг

**Проекты:**

- Эхо-сервер и эхо-клиент на TCP: несколько клиентов, обработка отключений, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Минимальный HTTP-сервер на C с раздачей статических файлов, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Сканер портов: TCP SYN scan, TCP Connect scan, UDP scan. Потоки для ускорения, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Простой чат на несколько клиентов через broadcast или unicast, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Передача файлов по собственному протоколу поверх TCP, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Резолвер DNS с итерацией по корневым серверам, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Сниффер пакетов на libpcap с разбором Ethernet, IP, TCP, UDP, ICMP, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Пингующий инструмент (ICMP Echo Request) без использования системного вызова, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Замер скорости линка в локальной сети через iperf-подобную утилиту, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Traceroute на основе ICMP Time Exceeded или UDP, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Высокопроизводительный HTTP/1.1 сервер на C с epoll, поддержка статики, базовый CGI, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Мини-анализатор трафика на libpcap с разбором протоколов до прикладного уровня, TUI, фильтры, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Игровой сервер с собственным бинарным протоколом и компрессией, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** *TCP/IP Illustrated* том 1 (Stevens), *Computer Networks* (Tanenbaum), *Unix Network Programming* (Stevens), Beej's Guide to Network Programming. Инструменты: Wireshark, tcpdump, iperf3, mtr, netstat, ss, ip. **Containerlab** (+ при необходимости FRR/Linux-узлы) для моделирования L3-топологий и сервисов уровня CCNA; классический коммутаторный L2 — на стенде или в GNS3/EVE-NG

---

### 2. Linux: Глубокое погружение

**Время:** 90 часов

**Что изучать:**

- Файловая система POSIX: chmod (символьный и восьмеричный), chown, chgrp, umask, sticky bit, setuid, setgid
- ACL: setfacl, getfacl, расширенные атрибуты (xattr)
- Процессы: ps, top, htop, pidof, pgrep. Состояния: Running, Sleeping (Interruptible/Uninterruptible), Stopped, Zombie
- Сигналы: SIGTERM, SIGKILL, SIGHUP, SIGINT, SIGUSR. kill, killall, pkill
- nice и renice: приоритеты -20 (высший) до +19 (низший)
- Фоновые задачи: bg, fg, jobs, nohup, disown, screen, tmux
- Файловая система /proc: /proc/[pid]/cmdline, /proc/[pid]/mem, /proc/[pid]/fd, /proc/cpuinfo, /proc/meminfo
- Файловая система /sys: /sys/class/net, /sys/devices
- strace и ltrace: трассировка системных вызовов, фильтрация по событиям
- Пакеты: apt, apt-cache, dpkg, yum, dnf, rpm. Сборка из исходников (configure, make, make install)
- Журналы: journalctl, /var/log, rsyslog, systemd-journald, logrotate. Разбор через grep, awk, sed
- systemd: systemctl, unit-файлы (.service, .socket, .timer), targets, dependencies, WantedBy, RequiredBy
- Таймеры systemd: замена cron для точного времени запуска
- Сеть: ip addr, ip link, ip route, ip rule, ss, netstat, tcpdump, nmap
- firewalld и iptables: базовые правила, таблицы filter/nat/mangle, цепочки INPUT/OUTPUT/FORWARD
- nftables: таблицы, цепочки, наборы — как более новый вариант
- Пользователи: useradd, usermod, userdel. /etc/passwd, /etc/shadow, /etc/group. sudo через /etc/sudoers
- PAM: на уровне концепций что это и почему важно для безопасности
- Диски: mount, fstab, lsblk, blkid, fdisk, parted. mdadm для RAID
- LVM: Physical Volume, Volume Group, Logical Volume, снапшоты
- Bash: переменные, массивы, if/case, for/while, функции, регулярные выражения
- Parameter expansion: ${var:-default}, ${var:=default}, ${var#pattern}, ${var%pattern}
- trap: обработка сигналов в скриптах
- Shellcheck: проверка скриптов перед запуском
- cgroups: ограничение CPU, памяти, IO через filesystem cgroup v1/v2
- Namespaces: pid, net, mnt, uts, ipc, user — на уровне концепций
- SELinux: контексты (user:role:type), режимы (Enforcing, Permissive, Disabled), базовые политики
- sysctl: /proc/sys/net/, настройка ядра на лету (/proc/sys)
- Hardening SSH: PubkeyAuthentication, PermitRootLogin no, PasswordAuthentication no, порт не 22

**Проекты:**

- Скрипт бэкапов с ротацией: инкременты, полные копии, шифрование GPG, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Собственный демон: systemd unit, корректные сигналы, логирование в journal, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Файрвол: iptables/nftables для базового сервера (INPUT, OUTPUT, FORWARD правила), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Монитор здоровья сервера: CPU, RAM, диск, сеть, отправка алертов, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Скрипт hardening Linux по чек-листу CIS Benchmarks (упрощённо), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Разбор дампа tcpdump: извлечение данных из дампа через bash, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- System Monitor Dashboard: мониторинг в реальном времени, TUI (ncurses), история метрик, алерты, экспорт JSON
- Smart Backup Tool: инкременты, шифрование, расписание через timers, политика хранения, restore, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**Open Source Strategy:**

- Этикет: MIT, GPL, Apache, BSD — понимание различий
- Репозитории для чтения: coreutils, busybox, tmux, htop, iproute2
- Первые вклады: исправления документации, мелкие issue
- Подписка на good first issue
- README профиля с проектами

**📚 Ресурсы:** *The Linux Command Line* (Shotts), *How Linux Works* (Ward), *Linux Basics for Hackers*. Сайт Linux Journey, варгейм OverTheWire Bandit, курс Linux Foundation на edX. **Containerlab** для практики сетевых концепций Linux в многоконтейнерных топологиях (`ip`, `ss`, маршрутизация, nftables)

---

### 3. C: Основы системного программирования

**Время:** 80 часов

**Что изучать:**

- Компиляция: gcc и clang, флаги (-Wall, -Wextra, -O2, -g). Стандарты C11, C17
- Препроцессор: #define, #include, #ifdef, макросы с аргументами
- Типы данных: int, long, short, char, float, double, void, size_t, ptrdiff_t
- Указатели: арифметика указателей, void\*, указатели на функции, callback-и
- Массивы и указатели: почему array[i] тождественно \*(array+i)
- Выравнивание структур: #pragma pack, атрибут packed
- Куча: malloc, calloc, realloc, free. Утечки памяти
- valgrind: memcheck для поиска утечек и выходов за границы
- ASan и MSan: санитайзеры компилятора
- Строки: strcpy, strncpy, strcat, strncat, sprintf, snprintf
- Безопасность строк: переполнения буфера и как их избегать
- Файлы высокий уровень: fopen, fread, fwrite, fprintf, fclose, fseek, ftell
- Файлы низкий уровень: open, read, write, lseek, close, fcntl, ioctl
- dup и dup2: перенаправление stdout/stderr
- stdin, stdout, stderr как файловые дескрипторы 0,1,2
- Битовые операции: &, |, ^, ~, \<<, >>
- Битовые маски: установка, сброс, проверка битов
- Структуры и union: sizeof, псевдонимы типов через typedef
- Вариативные функции: va_start, va_arg, va_end через stdarg.h
- Сигналы в C: signal, sigaction
- Makefile: цели, зависимости, автоматические переменные ($@, $\<, $^)
- CMake на вводном уровне
- Отладка gdb: breakpoints (b), run (r), next (n), step (s), continue (c), print (p), backtrace (bt)

**Проекты:**

- Реализация утилиты cat: поддержка -n, -v, нескольких файлов, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Реализация grep: базовые регулярные выражения, -i, -r, -l, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Реализация wc: -l, -w, -c, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Простой парсер конфигов: INI или TOML, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Аллокатор памяти: first-fit или best-fit, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Hex-редактор в консоли: чтение/запись, навигация, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Custom Memory Allocator: buddy, slab, стратегии, визуализация, тесты на производительность, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Поиск по бинарникам (ripgrep-подобный): regex, hex, mmap для больших файлов, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** *The C Programming Language* (K&R), *Expert C Programming* (van der Linden), *Modern C* (Gustedt). Курс CS50, Exercism C track. Чтение кода: redis, sqlite, nginx

---

### 4. Python: Скриптинг и автоматизация

**Время:** 60 часов

**Что изучать:**

- Базовые типы: int, float, str, bool, list, tuple, set, dict, None, frozenset
- Управление потоком: if/elif/else, for/while, break/continue/pass, ternary operator
- Функции: def, args, kwargs, \*args, \*\*kwargs, lambda,losures, nonlocal, global
- Декораторы: @staticmethod, @classmethod, @property, кастомные декораторы с параметрами
- Классы: class, \_\_init\_\_, \_\_str\_\_, \_\_repr\_\_, \_\_eq\_\_, \_\_hash\_\_, \_\_enter\_\_/\_\_exit\_\_
- Наследование: MRO, super(), multiple inheritance, composition vs inheritance
- Контекстные менеджеры: with, contextlib.contextmanager, contextlib.ExitStack
- Работа с файлами: open, read, write, with...as, io.StringIO, io.BytesIO, tempfile
- Модули и пакеты: import, from...import, \_\_name\_\_=="\_\_main\_\_", \_\_init\_\_.py, relative vs absolute imports
- Стандартная библиотека: os, sys, subprocess, json, time, datetime, re, pathlib, dataclasses, enum, typing
- Аргументы командной строки: argparse (subcommands, mutually exclusive groups), sys.argv
- Виртуальные окружения: venv, virtualenv, pip, pip-tools, poetry (basics)
- Обработка ошибок: try/except/else/finally, пользовательские исключения, except Exception as e
- Type hints: базовые, Optional, Union, List, Dict, Tuple, Callable, Protocol
- Сети: socket (TCP/UDP), http.client, urllib.request, ssl.wrap_socket
- HTTP-клиенты: requests (GET/POST, headers, session, timeout, retry), aiohttp
- Парсинг HTML/XML: BeautifulSoup4, lxml, re для простых случаев
- Работа с JSON: json.loads/dumps, jsonlines, orjson
- Работа с CSV: csv.reader/writer, DictReader, DictWriter
- subprocess: run, Popen, PIPE, capture_output, check=True
- Logging: basicConfig, levels, handlers, formatters, named loggers
- Многопоточность: threading, Thread, Lock, Event, Queue, ThreadPoolExecutor
- Многопроцессорность: multiprocessing, Process, Pool, ProcessPoolExecutor
- asyncio: event loop, coroutine, await, gather, create_task, asyncio.Queue, aiofiles
- Декомпиляция и интроспекция: dis, inspect, sys.getsizeof
- Шаблоны проектирования: Singleton, Factory, Observer, Strategy на Python
- Метапрограммирование: \_\_getattr\_\_, \_\_setattr\_\_, descriptors, \_\_init_subclass\_\_

**Проекты:**

- Скрипт сбора информации о системе: CPU, RAM, диски, сеть, процессы, top-5 по CPU, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Асинхронный сканер портов на asyncio: 1000+ портов за секунды, progress bar, export в JSON/CSV, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- HTTP-клиент с sessions, retry, timeout, auth, download с progress, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Парсер логов Apache/Nginx: извлечение IP, status codes, user agents, топ-10 IP по количеству запросов, ошибки 5xx, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Автоматизатор SSH-сессий: parallel exec на 10+ хостах, сбор вывода, diff конфигов, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- REST API scraper: парсинг JSON API, pagination, rate limiting, export в SQLite, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Утилита мониторинга: ping sweep сети, проверка HTTP endpoints, отправка алертов в Telegram/webhook, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Конвертер данных: JSON ↔ CSV ↔ XML ↔ YAML, с CLI interface, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Генератор паролей и менеджер секретов: генерация, шифрование (Fernet), поиск по базе, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Простой TCP-чат на несколько клиентов: threading, broadcast, команды /users /quit, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Network Automation Toolkit: ping sweep, port scan, service detection, config backup (SSH), report generation (HTML/PDF), modular architecture с плагинами, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Async Web Crawler: BFS/DFS, robots.txt respect, rate limiting, deduplication, link extraction, export в JSON, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** *Automate the Boring Stuff with Python*, *Python Crash Course* (Matthes), Python official docs, Real Python, *Fluent Python* (Ramalho), *Effective Python* (Slater)

---

### 5. Виртуализация

**Время:** 60 часов

**Что изучать:**

- Виртуализация vs контейнеризация: полная виртуализация, паравиртуализация, аппаратная (hardware-assisted VT-x/AMD-V)
- Гипервизоры Type 1 vs Type 2: KVM/QEMU, VMware ESXi, Hyper-V, Xen, bhyve
- VirtualBox: настройка, NAT/bridged/host-only/internal сети, shared folders, snapshots, linked clones, headless mode
- VMware Workstation/Fusion: сети, snapshot, linked clones, VMnet адаптеры, командная строка vmrun
- KVM/QEMU: virsh (list, edit, dominfo, snapshot-create, migrate), virt-install, libvirt, bridge networking (brctl, nmcli)
- libvirt: virt-manager, virsh net-*, pool-*, volume-*, autostart, dumpxml/edit
- Vagrant: Boxes (hashicorp atlas), provisioning (shell, ansible, docker), multi-machine, synced folders, forwarded ports, Snapshots
- Nested virtualization: включение VT-x в VM, KVM-in-KVM, VMware nested, ограничения производительности
- Сети в виртуальных средах: NAT, bridged, host-only, internal, NAT Network, promiscuous mode
- Виртуальные коммутаторы: Linux bridge (brctl), Open vSwitch (ovs-vsctl, ovs-ofctl), OVN
- SR-IOV: проброс PCIe/NIC устройств в VM, VF (Virtual Function), PF (Physical Function)
- Виртуальные VLAN: тегирование в VM, 802.1Q на Linux bridge, trunk порты в OVS
- Диски в VM: qcow2 (copy-on-write, snapshots, compression), VMDK, VDI, raw, disk thin provisioning
- Live migration: миграция работающих VM между хостами, pre-copy vs post-copy, downtime minimization
- Управление ресурсами: CPU pinning, cgroups в VM, IO throttle, memory ballooning, huge pages
- QEMU device models: virtio (net, disk, scsi), e1000, rtl8139 — производительность и совместимость
- VM Templates и Clones: golden image, sysprep/cloud-init, linked clone vs full clone
- Cloud-init: initial cloud instance setup, user-data scripts, metadata service, first-boot config
- Типы дисковой подсистемы: IDE, SATA, SCSI, virtio-scsi, NVMe passthrough
- Snapshot Management: инкрементальные, дерево снапшотов, merge, rollback, snapshot=checkpoint для security testing
- Дисковый I/O: fio benchmarks, iotop, alignment, discard/trim для SSD

**Практика:**

- VirtualBox lab с несколькими VM: Kali, Metasploitable, Windows, Ubuntu Server — настройка сетей, shared folders, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Настройка KVM с libvirt в Linux: virsh commands, bridge networking, autostart, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Vagrantfile для пентест-лаборатории: multi-machine, provisioning, snapshots, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Настройка Open vSwitch: создание bridge, портов, VLAN trunk, flow rules через ovs-ofctl, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Nested virtualization: запуск KVM внутри KVM/VMware, проверка работоспособности, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Live migration: миграция VM между двумя KVM хостами через libvirt, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Cloud-init: создание golden image Ubuntu, первичная настройка через user-data, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- Автоматизация создания VM через Vagrant: Multi-machine pentalab (Kali, 2 targets, DC), provisioning ansible, snapshot management, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Виртуальная сеть с несколькими VM и firewall: Linux bridge + OVS, NAT, port forwarding, ACL, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Nested virtualization lab: запуск контейнеров/docker-in-docker внутри VM, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Бенчмарк дисковой подсистемы: fio на разных типах дисков (qcow2 vs raw vs virtio-scsi), сравнение IOPS, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- QEMU network lab: 3 VM с разными NIC моделями (e1000 vs virtio), benchmark throughput через iperf3, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Fully Automated Pentest Lab: Vagrant + Ansible + libvirt, полный стек от нуля до рабочей лаборатории с AD/Windows/Linux, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- VM Migration & HA Setup: 2 KVM хоста, live migration, shared storage (NFS), failover, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** VirtualBox docs, KVM/QEMU docs, Vagrant docs, Open vSwitch docs, libvirt docs, *Mastering KVM Virtualization*

---

### 6. Git и версионирование

**Время:** 40 часов

**Что изучать:**

- Внутренности: объекты blob, tree, commit, tag. .git директория (objects, refs, HEAD, config, hooks)
- Хеширование: SHA-1/SHA-256, content-addressable storage, zlib compression
- refs: branches (refs/heads), tags (refs/tags), HEAD, ORIG_HEAD, stash
- Базовые команды: init, clone, add, commit, push, pull, fetch, merge, rebase
- add: -p (patch mode), --interactive, --all, --sparse, add --patch partial staging
- commit: -m, -a, --amend, --no-edit, Signed-off-by, GPG signing
- push: -u (upstream), --force-with-lease (безопасный force), --tags, push options
- pull: --rebase (rebase вместо merge), --ff-only, --squash
- fetch: --all, --prune, fetch + diff, fetch + log comparison
- Ветки: branch, checkout, switch, restore, cherry-pick, revert, reset, stash
- branch: -d/-D, -m, -a (remote tracking), --list, branch命名 convention
- switch vs checkout: switch -c, switch -, switch --discard-changes
- stash: push, pop, apply, drop, list, stash branch, include untracked
- merge vs rebase: fast-forward, three-way merge, conflicts resolution, rebase --onto
- cherry-pick: -n (no-commit), --signoff, conflict handling
- revert vs reset: revert (создаёт новый commit), reset --soft/--mixed/--hard
- reflog: восстановление после ошибок, dangling commits, git fsck
- Интерактивный rebase: squash, fixup, reword, edit, drop — изменение истории
- Git flow: feature branches, release, hotfix, develop — workflow
- GitHub flow: PR-based workflow, main + feature branches
- Conventional Commits: type(scope): description, semantic versioning trigger
- .gitignore: паттерны, negation (!), directory-specific, global gitignore
- Теги: annotated (-a) vs lightweight, signed tags, tag descriptions
- Удалённые репозитории: remote, remote add, remote show, tracking branches
- Pull requests: создание, ревью, reviewers, merge strategies (merge/squash/rebase)
- SSH ключи для Git: ed25519 (предпочтительно), rsa 4096, ssh-agent, multiple keys
- CI: GitHub Actions (workflow, jobs, steps, matrix, caching), GitLab CI (pipeline, stages, artifacts)
- Git hooks: pre-commit, commit-msg, post-merge, prepare-commit-msg, husky
- large files: git-lfs, .gitattributes, large file storage
- Git worktrees: работа с несколькими ветками одновременно
- Bisect: бинарный поиск баг-коммита
- blame: авторство строк, --since, --ignore-rev
- shortlog: статистика коммитов по авторам
- Git submodules: add, update, deinit, submodule vs subtree
- Git aliases: полезные сокращения

**Практика:**

- Репозиторий с ветвлением и конфликтами: создание веток, merge, resolve conflicts, rebase, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Pre-commit хуки: shellcheck, линтеры, auto-formatter, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- CI pipeline для Python/C проекта: test, lint, build, deploy stages, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Git bisect: найти баг-коммит в автоматическом режиме (git bisect run), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Interactive rebase: история из 20 коммитов → 5 чистых, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Git LFS: хранение больших pcap-файлов в репозитории, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Worktrees: работа над bugfix и feature одновременно в одном репозитории, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- Репозиторий с ветвлением, конфликтами и rebase workflow, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Pre-commit хуки: shellcheck, линтеры, auto-formatter, commit-msg линтер, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- CI/CD pipeline: Python проект → test → lint → build → Docker image → push to registry, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Release workflow: tag → changelog generation → release notes → artifacts, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** Pro Git (издание на русском), Learn Git Branching, Oh Shit, Git!?, *Git in Practice*

---

### 7. Алгоритмы и структуры данных

**Время:** 60 часов

**Что изучать:**

- Асимптотика: O, Omega, Theta, амортизация. Worst/average/best case. Master theorem для рекуррентностей
- Динамический массив: стратегия роста capacity (×2), амортизация O(1), реаллокация и копирование
- Связные списки: односвязные, двусвязные, вставка, удаление, реверс, two-pointer technique
- Стеки и очереди: реализация на массивах и списках, monotonic stack, deque
- Хеш-таблицы: хеш-функции, открытая адресация (linear/quadratic probing), цепочки (chaining), рехэширование, load factor, collision resolution strategies
- Хеш-функции для безопасности: MD5 (сломан), SHA-1 (сломан), SHA-256, HMAC
- Деревья: BST (вставка, удаление, поиск), обходы (preorder, inorder, postorder, level-order), lowest common ancestor
- Балансировка: AVL дерево (вращения LL/RR/LR/RL), красно-чёрные деревья (properties, recoloring, rotation), B-tree (concept)
- Бинарная куча: heapify (build-heap O(n)), heap sort, priority queue, min-heap/max-heap
- Графы: представление (матрица смежности, список рёбер, adjacency list), взвешенные/невзвешенные
- BFS: shortest path в невзвешенном графе, level-order traversal, bipartiteness check
- DFS: обход, cycle detection, topological sort, connected components
- Топологическая сортировка: алгоритм Kahn (BFS-based), DFS-based
- Кратчайшие пути: Dijkstra (min-heap O((V+E)logV)), Bellman-Ford (negative weights O(VE)), Floyd-Warshall (all-pairs O(V³))
- Минимальное остовное дерево: Prim (O(E logV)), Kruskal (O(E logE)) с Union-Find
- Union-Find: path compression, union by rank, offline algorithms
- Сортировки: quicksort (randomized, O(n log n) average), mergesort (stable O(n log n)), heapsort (in-place O(n log n)), counting sort (O(n+k)), radix sort (O(d·n)), bucket sort
- Двоичный поиск: на отсортированном массиве, по предикату (binary search on answer), bisect в Python
- Строки: KMP (O(n+m) поиск подстроки), Rabin-Karp (хеш-based), Z-function, префиксное дерево (trie), суффиксное дерево (concept)
- Жадные алгоритмы: activity selection, Huffman coding, greedy exchange argument
- Динамическое программирование: memoization, tabulation, knapsack (0/1 и unbounded), LCS, LIS, coin change
- Графы: network flow (Ford-Fulkerson/Edmonds-Karp), bipartite matching
- Нетривиальные структуры: segment tree, fenwick tree (BIT), skip list
- Bit manipulation: &, |, ^, ~, <<, >>. Masking, bit counting, power-of-two checks, swap without temp
- rekурсия vs итерация: stack depth, tail recursion, TCO (tail call optimization)

**Практика:**

- Реализация на C: динамический массив (дженерик через void* или макросы), linked list, hash map, binary search tree, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Визуализатор: анимация сортировок (mergesort, quicksort, heapsort) через终端 output или curses, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- BFS/DFS визуализатор: обход графа с отрисовкой в консоли, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- Библиотека структур данных на C: dynamic array, linked list, hash table, BST, heap, queue, stack — все с unit tests (check.h), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- LeetCode: 30–40 задач уровня Easy и Medium, распределение: arrays(10), strings(5), hash(5), trees(5), graphs(5), dp(5), sorting(5), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Визуализатор алгоритмов сортировки: live terminal animation, comparison/swap counters, time complexity display, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Калькулятор выражений: парсинг, обратная польская нотация, поддержка переменных и функций, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Trie-поиск: префиксный поиск по файлам, автодополнение, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- LRU Cache: хеш-таблица + двусвязный список, O(1) get/put, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** *Introduction to Algorithms* (CLRS), *The Algorithm Design Manual* (Skiena), *Algorithm Design* (Kleinberg & Tardos), *Algorithms* (Sedgewick), курс Algorithms на Coursera (Princeton/Stanford), VisuAlgo

---

### 8. Open Source Security Contributions 

**Время:** 40 часов

**Что изучать:**

- Стратегия OSS вклада: поиск `good first issue`, чтение contribution guidelines, quality pull requests
- Лицензии: MIT, GPL, Apache, BSD — понимание различий, restrictions, compatibility
- Базовые contribution направления для security инженера: rulesets, parsers, tooling wrappers, docs with PoC examples
- Практика code review: lint/test/security checklist перед PR, review etiquette
- Этика и disclosure: responsible disclosure, coordinated vulnerability disclosure, CVE process
- Портфолио вклада: как превращать PR в измеримый карьерный результат, link to PRs в резюме
- GitHub advanced: topics, releases, discussions, codeowners, branch protection rules
- Git extras: conventional commits, semantic-release, changelog generation

**Проекты:**

- 5–8 PR в профильные репозитории (Suricata rules, Zeek scripts, сетевые утилиты), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Подготовка `security-improvements` issue + PR с измеримым улучшением, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- OSS Security Pack: 10 PR в 3 разных проектах (минимум 5 accepted) + технический отчёт по impact, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** GitHub Docs (Contributing), Suricata GitHub, Zeek docs, OSS Guide (opensource.guide)

---

### 9. Продвинутый Wireshark/tshark и BPF фильтры 

**Время:** 45 часов

**Что изучать:**

- Capture filters (BPF) vs display filters: где использовать каждый тип, синтаксис, ограничения
- BPF: host, net, port, proto, src/dst, tcp/udp/icmp, and/or/not, direction
- Продвинутые display filters: `tcp.analysis.retransmission`, `http2`, `quic`, `tls.record`, `dns.flags.rcode`, `tcp.analysis.flags`
- _Display filter pieces: ip.addr, tcp.port, http.request.method, tcp.flags.syn==1 && tcp.flags.ack==0
- tshark automation: extraction полей (-T fields -e), статистика (-z), экспорт в JSON/CSV/PCAP
- Correlation pipelines: tcpdump + tshark + jq для быстрой triage аналитики
- Выявление beaconing и low-and-slow трафика по временным паттернам
- IO Graphs: визуализация throughput, protocol distribution, conversation statistics
- Follow stream: TCP, UDP, TLS, HTTP — reconstruction сессий
- Packet details: раскрытие всех уровней, дерево полей,抜粋ить raw data
- coloring rules: кастомные правила раскраски для быстрого визуального анализа
- Expert information: warnings, errors, notes — интерпретация
- Statistics: protocol hierarchy, conversations, endpoints, I/O graph, round trip time
- tls decryption: (Pre)-Master-Secret log file, SSLKEYLOGFILE, RSA key logging (deprecated)
- DNS analysis: queries/responses, latency, failures, cache poisoning indicators
- HTTP analysis: request/response pairs, chunked encoding, keep-alive, websocket upgrade
- Conversation filtering: filter by conversation, follow stream, export objects
- Temporal analysis: time between packets, retransmission timing, response time distribution

**Практика:**

- Анализ pcap-файла атаки: поиск SYN flood, port scan, DNS exfil через tshark, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- tshark pipeline: извлечение HTTP requests из pcap → фильтрация по user-agent → экспорт в JSON, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- TLS inspection: расшифровка HTTPS через SSLKEYLOGFILE в Wireshark, анализ handshake, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Beacon detection: анализ интервалов DNS запросов для поиска C2, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- IO graph: построение графика throughput по времени, выявление spike/pattern, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- Набор BPF/display фильтров для 15 частых troubleshooting/incident кейсов (SYN scan detection, DNS tunneling, slowloris, RDP brute force, SMB lateral movement), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- CLI-утилита анализа pcap: top talkers, suspicious intervals, DNS anomalies, HTTP errors, TLS failures, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- tshark automation script: batch-анализ 100 pcap файлов, генерация summary report, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Wireshark Detection Cookbook: 30 production-ready фильтров + auto-tshark pipeline +omaly detection script, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Network Triage CLI: tshark-based tool для автоматического анализа pcap, JSON export, severity scoring, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** Wireshark docs, tshark man, tcpdump man, *Wireshark Network Analysis* (Laura Chappell), Wireshark Wiki

---

### 10. BPF основы для сетевой диагностики 

**Время:** 50 часов

**Что изучать:**

- Классический BPF и eBPF: отличия, bytecode, register model, program types
- bpftrace one-liners:BEGIN, END, probe types (kprobe, kretprobe, uprobe, tracepoint, profile)
- Наблюдение TCP retransmits, drops, connect latency через tracepoints/kprobes
- bpftrace syntax: filter expressions, builtins (comm, pid, tid, nsecs, kstack, ustack), printf, @ histograms
- Наблюдение DNS: kprobe на udp_sendmsg, tcp_connect, tracepoint:tcp/tcp_retransmit_skb
- Наблюдение socket operations: connect, accept, bind, close — latency analysis
- Наблюдение файлового ввода-вывода: block I/O latency, disk usage by process
- Наблюдение контекстных переключений: sched:sched_switch, off-CPU analysis
- Наблюдение syscall'ов: syscall entry/exit, duration, failure rates
- Безопасность и ограничения eBPF verifier: permissions, helper functions, bounded loops, stack size
- Контроль доступа к eBPF: CAP_BPF, CAP_NET_ADMIN, kernel.unprivileged_bpf_disabled
- kprobes и kretprobe: kernel function instrumentation, аргументы, return value
- uprobes: user-space function instrumentation (libc, openssl, custom binaries)
- CO-RE (Compile Once Run Everywhere): BTF, BTF-defined maps, CO-RE relocs, vmlinux.h
- Отладка eBPF: bpftool prog show/map dump, cat /sys/kernel/debug/tracing, BPF verifier logs
- bpfcc tools: install, python bindings, bpftrace под капотом
- libbpf: загрузка eBPF программ из C, skeleton, maps interaction, perf buffer, ring buffer
- timeline eBPF: от простых скриптов до production monitoring agents

**Практика:**

- Установка bpftrace и bpfcc tools, проверка доступа, hello world, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Наблюдение TCP connect latency через kprobe на tcp_v4_connect: histogram, percentile (p50/p95/p99), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Наблюдение TCP retransmissions: kprobe на tcp_retransmit_skb, подсчёт по dst IP, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Наблюдение DNS: отслеживание DNS запросов и ответов через кинетические tracepoint, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Наблюдение disk latency: blk_io_within, по process, histogram, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Off-CPU analysis: kprobe на sched:sched_switch, off-CPU time по process, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- Набор bpftrace скриптов для диагностики latency spikes: 10 скриптов для разных сценариев (TCP/DNS/disk/CPU), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- mini dashboard: сбор kernel network metrics и экспорт в Prometheus через exposition format, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Network connection tracer: eBPF программа на C, которая логирует все TCP connect() с latency и destination, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- BPF Troubleshooting Kit: 20 диагностических скриптов + runbook для SRE/NetSec команды + automated latency report, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- eBPF Network Monitor: libbpf-based agent, TCP connect/accept/close tracking, DNS visibility, Prometheus exporter, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** bpftrace docs, eBPF.io, Brendan Gregg blog, *BPF Performance Tools* (Gregg), *Learning eBPF* (Liz Rice), Linux kernel bpf docs

---

## Семестр 4 - Углубление (Февраль - Июнь)

**Время:** ~340–380 часов

### 1. Операционные системы: Внутреннее устройство

**Время:** 80 часов

**Что изучать:**

- Процессы: fork, exec, copy-on-write, wait, waitpid. Зомби и сироты
- Семейные отношения: группы процессов, сессии, терминал управления
- Демоны: double fork, закрытие стандартных потоков, работа в /var/run
- Потоки POSIX: pthread_create, pthread_join, pthread_detach
- Синхронизация потоков: pthread_mutex, pthread_rwlock, условные переменные, семафоры
- Гонки (race conditions) и взаимные блокировки (deadlocks)
- Межпроцессное взаимодействие: pipes, FIFO, очереди сообщений (SysV, POSIX)
- Разделяемая память: shmget, shmat, shmctl
- Сокеты Unix domain: AF_UNIX для IPC
- Сигналы: sigaction, sigprocmask, обработка async-signal-safe функциями
- Виртуальная память: страницы, page fault, таблица страниц, TLB
- mmap: MAP_PRIVATE, MAP_SHARED, MAP_ANONYMOUS
- brk/sbrk: управление кучей процесса
- mprotect: защита страниц, NX bit
- Планирование CPU: FCFS, SJF, Round Robin, CFS в Linux
- Файловые системы: ext4, inode, VFS
- Жёсткие и символические ссылки
- Mount namespaces
- Системные вызовы: таблица sys_call, strace в деталях
- Модули ядра: module_init, module_exit, printk, сборка, insmod/rmmod

**Проекты:**

- Многопоточный веб-сервер с пулом потоков, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Оболочка (shell) с пайпами: ls | grep | wc, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Чат через shared memory или message queue, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Симулятор простого планировщика процессов, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Modern Shell: лексер, парсер, встроенные команды, пайпы, перенаправления, job control, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Mini Container Runtime: namespaces, cgroups, chroot, сеть между контейнерами, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** OSTEP (Operating Systems: Three Easy Pieces — онлайн бесплатно), *The Linux Programming Interface* (Kerrisk), *APUE* (Stevens), MIT 6.828 (xv6)

---

### 2. Основы безопасности и криптографии

**Время:** 70 часов

**Что изучать:**

- Симметричное шифрование: AES (ECB, CBC, CTR, GCM mode), режимы работы
- Асимметричное шифрование: RSA, ECC (Curve25519)
- Хеширование: MD5 (устарел), SHA-1 (устарел), SHA-256, SHA-3
- HMAC: код аутентичности сообщения на основе хеша
- Пароли: соль, Argon2, bcrypt, scrypt — как правильно хранить
- OpenSSL: командная строка (genrsa, req, x509), библиотеки libcrypto, libssl
- TLS/SSL: рукопожатие, сертификаты X.509, цепочки CA, валидация
- TLS 1.3: упрощённое рукопожатие, 0-RTT
- Криптографические библиотеки: libsodium, sodium_add, TweetNaCl
- Атаки на криптографию: словарные атаки на пароли, collision attacks
- Entropy: /dev/urandom vs /dev/random

**Проекты:**

- Генерация ключей и сертификатов для тестового TLS, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Шифрование файлов: AES-GCM, сохранение nonce и tag, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Проверка пароля: Argon2id verify с правильными параметрами, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Простой secure chat: шифрование сообщений, обмен ключами, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- TLS-сервер и TLS-клиент с верификацией сертификата, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** *Cryptography Engineering* (Ferguson, Schneier), *The Handbook of Applied Cryptography* (онлайн), OpenSSL Cookbook

---

### 3. Сетевые инструменты анализа и мониторинга

**Время:** 70 часов

**Что изучать:**

- tcpdump: фильтры BPF (-f), -i (interface), -w (write pcap), -r (read), -v/-vv/-vvv, -n/-nn, -c (count), -s (snaplen)
- tcpdump advanced: -X (hex+ascii), -A (ascii), -e (link-level), -tttt (human time), ring buffer (-C/-W)
- Wireshark: фильтры отображения, Follow stream, IO Graphs, Statistics, Expert info, coloring rules, tshark automation
- tshark: CLI-версия Wireshark для автоматизации, -T fields -e для extraction, -z статистика, -Y display filter
- nmap: сканирование (-sS, -sT, -sU), определение ОС (-O), скрипты (-sC, --script), -A (aggressive), -T0-T5
- nmap advanced: NULL/FIN/Xmas scans, idle scan (-sI), bounce scan, version intensity, script categories
- netstat: состояния соединений (-t, -u, -x), слушающие (-l), process info (-p), continuous (-c)
- ss: замена netstat, -t/-u/-x/-w, -a (all), -p (process), -i (internal), -m (memory), -e (extended), -K (kill), -s (summary)
- iperf3: тестирование пропускной способности, -s (server), -c (client), -P (parallel), -R (reverse), -b (bandwidth), -t (time), -J (JSON)
- mtr: комбинированный traceroute + ping, -r (report), -c (count), -n (no DNS), -w (wide)
- traceroute/tracert: методы (ICMP (-I), UDP, TCP SYN (-T)), -n (no DNS), probe types
- ethtool: настройка и диагностика NIC, -S (statistics), speed/duplex, offload features, ring buffer size
- ip: addr, link, route, rule, neigh (ARP), monitor (real-time), -s (statistics)
- bandwhich: визуализация использования сети по процессам, band top/trace
- iftop: мониторинг трафика по хостам в реальном времени, -f (filter), -n (no DNS)
- iptraf-ng: текстовый мониторинг интерфейсов, detailed stats, conversation monitoring
- vnstat: мониторинг трафика за периоды (hourly, daily, monthly), -l (live), database management
- bmon: настройка и скорость на интерфейсах, grouping,前世port
- ngrep: поиск по сетевым пакетам (grep для пакетов), -q (quiet), -d (interface), pattern matching
- tc (traffic control): qdisc (pfifo, sfq, tbf), classful shaping, netem (emulation: delay, loss, reorder, duplicate)
- dnstop: анализ DNS трафика в реальном времени, top queries, top clients
- tcpflow: потоковая реконструкция TCP соединений, файлы per-flow
- tcpick: TCP connection sniffer, stream visualization
- arpwatch/arp-scan: ARP monitoring, new host detection, ARP table dump
- netcat/nc: port listening (-l), file transfer, reverse shell, port scanning, -v (verbose), -z (scan mode)
- socat: advanced netcat, SSL, UNIX sockets, port forwarding, relay
- ncat: nmap's netcat, --ssl, --ssl-verify, --sh-exec

**Практика:**

- Анализ дампа сетевого трафика: поиск аномалий в pcap файлах (port scan, DNS exfil, suspicious beacons), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Скрипт автоматического мониторинга сети: алерты при изменении состояний (host down, port open/close, DNS failure), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Собственная утилита traceroute с выбором метода (ICMP/UDP/TCP), comparison с оригинальным traceroute, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Эмуляция сети через tc netem: задержки (50ms), потери (5%), reorder — тестирование приложений в degraded conditions, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Логгер использования сети по процессам: top talkers с process attribution, bandwidth accounting, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- iperf3 benchmark: throughput test между двумя хостами, TCP/UDP comparison, parallel streams, MTU impact analysis, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- nmap deep dive: 5 scan types comparison, timing, script usage, output formats, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- arp-scan network discovery: scan subnet, detect new hosts, flag anomalies, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- Network monitoring script suite: 5 скриптов для разных сценариев (ping check, port monitor, DNS check, bandwidth monitor, ARP watch), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- pcap analysis report: автоматический анализ pcap → report (top talkers, protocols, anomalies, statistics), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Network benchmark toolkit: iperf3 + mtr + ping combined benchmark suite, JSON output, comparison reports, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- tc netem test harness: скрипт для эмуляции 10 network conditions + validation of application behavior, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Network Monitoring Dashboard: realtime metrics, TUI (ncurses) + web (flask), history, алерты, export, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Анализатор pcap с автоматическим определением протоколов и аномалий: protocol classification, threat scoring, JSON report, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** *The Practice of Network Security Monitoring* (Bejtlich), Wireshark Docs, Nmap Documentation, tc man, *Network Tools* by Daniel Miessler

---

### 4. Отладка сетевых приложений

**Время:** 60 часов

**Что изучать:**

- strace: трассировка системных вызовов (-f, -e, -p, -c, -T), формат вывода, timestamp опция
- strace для сетевых вызовов: socket, connect, accept, send, recv, sendto, recvfrom, close
- strace для диагностики TCP/UDP: отслеживание connect failures, timeout, unexpected closes, ECONNREFUSED, ETIMEDOUT
- strace + фильтрация по сигналам: обработка SIGPIPE, SIGURG, SIGIO (async I/O)
- tcpdump + strace связка: корреляция системных вызовов с пакетами в сети, timestamp alignment
- Wireshark + strace: совместный анализ дампа и системных вызовов, pipeline analysis
- Анализ TIME_WAIT, CLOSE_WAIT через /proc и strace: мониторинг состояний socket'ов
- Диагностика DNS через strace: резолвинг, NSSwitch, getaddrinfo, /etc/nsswitch.conf
- Анализ TLS handshake через strace: openssl calls, certificate loading, SSL_read/SSL_write
- Диагностика high load: strace для выявления блокирующих вызовов (blocked on futex, poll, epoll_wait)
- Отладка race conditions в сетевых приложениях через strace -f: fork/thread interleaving
- ltrace: отладка библиотечных вызовов для сетевых приложений (libc, libssl, libpcap)
- core dumps: ulimit -c unlimited, gdb для анализа crash в сетевом сервисе, stack trace
- addr2line: преобразование адресов в файлы/строки, useful для stripped binaries
- /proc/net/tcp, /proc/net/udp: просмотр состояний, inode mapping, file descriptor tracking
- lsof -i: поиск open network connections по процессам, port ownership
- ss internals: -K (kill socket), -a (all), -m (memory), -i (internal info), -e (extended), -p (process)
- netstat -w: wide output, -p для process info, -c для continuous updates
- /proc/[pid]/fd: file descriptor tracking для network sockets, socket inode
- auditd: мониторинг сетевых syscall'ов через audit subsystem
- ftrace: function_graph tracer для kernel network functions
- crash: offline analysis of kernel crash dumps (basics)

**Практика:**

- Диагностика "connection refused" через strace + tcpdump: пошаговый разбор от syscall до пакета, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Анализ TLS handshake ошибок: strace + Wireshark, пошаговый разбор protocol и system level, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Отладка утечки сокетов: strace для поиска open/close дисбаланса, lsof -i, ss -s, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Анализ медленного DNS резолвинга: strace + tcpdump + dig, tracing request→response, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Поиск причин high latency в HTTP сервере через strace: определение blocking syscall, timing analysis, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Анализ ephemeral port exhaustion через /proc/net/tcp и ss -s, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- ltrace для анализа SSL library вызовов: TLS handshake sequence, certificate verification steps, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- Diagnostic toolkit: скрипт, который автоматически собирает strace + tcpdump + ss + lsof для проблемного сервиса, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Network debugging playbook: 10 сценариев (connection refused, timeout, slow dns, port exhaustion, etc.) с пошаговой командной процедурой, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Automated network health check: скрипт проверки ports, services, DNS, latency, routing, конфигурация, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Network Debugging Toolkit: полный CLI toolkit для автоматической диагностики (strace + tcpdump + ss + lsof + perf), severity scoring, HTML report, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** strace man, ltrace man, *The Linux Programming Interface*, *Systems Performance* (Brendan Gregg), Brendan Gregg's USE method

---

### 5. C++: Современный стандарт и системное программирование

**Время:** 70 часов

**Что изучать:**

- Классы: конструкторы, деструктор, RAII
- Наследование и полиморфизм: virtual функции, override
- Умные указатели: unique_ptr, shared_ptr, weak_ptr
- Перемещение: rvalue-ссылки, std::move, perfect forwarding
- Шаблоны: полная и частичная специализация
- STL контейнеры: vector, map, unordered_map, set
- STL алгоритмы: std::sort, std::find, std::transform
- Лямбды: захват, mutable, generic (auto)
- Исключения: try/catch, noexcept
- Параллелизм: std::thread, std::mutex, std::condition_variable
- Атомики: std::atomic, порядки памяти (memory order)
- C++20: concepts, ranges, coroutines — на уровне концепций
- CMake: find_package, target_link_libraries

**Проекты:**

- Собственный vector: шаблонный контейнер с итераторами, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Thread pool: пул потоков для параллельного выполнения задач, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Простой JSON-парсер: без внешних библиотек, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Key-value хранилище в памяти с индексацией, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Потокобезопасный логгер, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- HTTP/2 сервер: http-parser, multiplex, header compression, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Key-value база: B+ tree, journal, транзакции, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Game engine: SDL2/OpenGL, ECS, физика, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** *A Tour of C++*, *Effective Modern C++*, *C++ Concurrency in Action*, cppreference.com

---

### 6. Проектирование сетей

**Время:** 70 часов

**Что изучать:**

- Топологии: звезда, кольцо, шина, полносвязная, ячеистая — преимущества, недостатки, применения
- Модель Клиент-Сервер: распределение ролей, масштабирование, fault tolerance
- Модель Peer-to-Peer: DHT, BitTorrent, advantages/decentralization trade-offs
- Сетевая архитектура:三层模型 (Core/Distribution/Access), tiers и scalability
- VLAN: дизайн VLAN (numbering, naming), management VLAN, native VLAN, voice VLAN, data VLAN, guest VLAN
- STP: 802.1D, RSTP ( Rapid Spanning Tree), MSTP (Multiple STP), root bridge election, port states (blocking/listening/learning/forwarding)
- EtherChannel: LACP (active/passive), PAgP, static — negotiation, hash algorithms (src-ip, dst-ip, src-dst-ip)
- Маршрутизация: статическая vs динамическая, IGP vs EGP, administrative distance, metric
- OSPF: areas (backbone, stub, NSSA), LSA types (1-5, 7), DR/BDR election, cost calculation, adjacency states
- BGP: AS numbers, eBGP/iBGP, path attributes (AS_PATH, LOCAL_PREF, MED), route reflectors, communities, prefix filtering
- EIGRP: DUAL algorithm, K-values, stub routing — на уровне концепций
- NAT: SNAT, DNAT, PAT (overload), hairpin NAT, static NAT, PAT pool
- Firewall: zones (trust/untrust/DMZ), policies (permit/deny/log), inspection levels (stateful/stateless/application)
- DMZ: проектирование демилитаризованной зоны, placement of web/mail/DNS servers, dual-firewall vs single-firewall
- High Availability: FHRP (HSRP/VRRP/GLBP), load balancing (round-robin, least connections, hash)
- Redundancy: link redundancy, device redundancy, path redundancy, BFD (Bidirectional Forwarding Detection)
- QoS: DSCP (EF, AF classes, BE), priority queuing (PQ), weighted fair queuing (WFQ), traffic shaping vs policing
- IPv6: addressing (link-local, ULA, global unicast), SLAAC, DHCPv6, NDP, dual-stack, tunneling (6to4, teredo)
- MPLS: labels, LDP, VPN (L3VPN, L2VPN), traffic engineering — на уровне концепций
- SD-WAN concepts: overlay vs underlay, controller-based, application-aware routing
- Network segmentation: microsegmentation, VRF, ACL-based, zone-based
- Wireless fundamentals: 802.11 standards, SSID, WPA2/WPA3, enterprise auth (802.1X), WLC concepts
- Automation basics: Ansible для network devices (module ios_config, nxos_config), playbook structure
- GitOps для сетевых конфигов: version control конфигов, change management, rollback

**Практика (Containerlab):**

- Топология `*.clab.yml`: статическая маршрутизация между тремя узлами FRR, проверка таблиц маршрутов и полной связности, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- DHCP: пул на dnsmasq/Kea, relay, клиенты получают адреса, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- VLAN / trunk / VTP: дизайн VLAN + логическая изоляция отдельными подсетями ИЛИ выполнение на реальном коммутаторе / GNS3/EVE-NG, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Политики доступа: nftables/iptables (ACL equivalent), логирование и счётчики, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- NAT: PAT (MASQUERADE), статические DNAT, проверка с «внешней» стороны, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Сеть организации: несколько сегментов, выделенный firewall (Linux), DMZ-подсеть, маршруты и политики, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- OSPF между 3 маршрутизаторами: areas, route redistribution, passive interfaces, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- HSRP/VRRP: два шлюза с keepalived, failover testing, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- Проектирование сети офиса: 3 отдела, серверная, firewall, management VLAN — **артефакт:** диаграмма (draw.io/d2) + рабочий `*.clab.yml` + README, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Проектирование сети дата-центра: подключение к провайдерам, 2 tiers (core/distribution), redundancy, BGP/OSPF — **модель в Containerlab**, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Отказоустойчивость: два шлюза (FRR) с VRRP (keepalived) + link redundancy + failover testing, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- QoS design: классификация трафика (voice, video, data, best-effort), queueing, policing — Containerlab simulation, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Ansible playbook для автоматизации конфигурации 5 сетевых устройств (Linux routers): config push, backup, diff, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Enterprise Network Blueprint: полная архитектура (3 floors, DMZ, server farm, remote access VPN, wireless), draw.io diagrams + Containerlab topology + Ansible automation + README, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** [Containerlab](https://containerlab.dev/), FRRouting, CCNA/CCNP материалы (теория), CBT Nuggets, David Bombal (YouTube); для L2/Wi-Fi — GNS3 / EVE-NG, *Network Warrior* (Donahue)

---

### 7. Физическая лаборатория (реальное оборудование)

**Время:** 40 часов

**Что изучать:**

- Оборудование: 2 managed switches (Zyxel/MikroTik), 2 роутера
- Консольный кабель (USB-to-Serial, RJ45-to-DB9)
- Подключение через console: PuTTY, screen, minicom
- Настройка IP адресов на свитчах/роутерах
- VLAN: создание, присвоение портам, тегирование
- Trunk: 802.1Q, настройка trunk портов
- LACP: объединение портов в trunk (Zyxel/MikroTik)
- STP: включение, настройка приоритета
- Basic routing: статические маршруты
- DHCP server на свитче/роутере
- Port security: MAC limiting, shutdown
- Management VLAN: выделенный VLAN для управления
- Backup конфигураций: сохранение/восстановление

**Практика:**

- Поднятие свитчей из "коробки", с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Настройка VLAN: 3 VLAN (management, data, voice), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Настройка trunk между свитчами, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- LACP etherchannel, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Настройка DhCP relay, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Port security с shutdown, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- Сеть офиса на реальном железе, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Настройка отказоустойчивости с STP, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Резервное канал, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**Оборудование (минимум):**

- 2 x MikroTik CRS112 или Zyxel GS1900
- 2 x MikroTik RB951 или аналоги
- Консольный кабель
- PAT (4-портовый свитч для подключения к роутеру провайдера)

**📚 Ресурсы:** MikroTik docs, Zyxel docs, Console cable tutorials

---

### 8. Введение в пентест

**Время:** 60 часов

**Что изучать:**

- Методология: reconnaissance → scanning → enumeration → exploitation → post-exploitation → reporting
- PTES (Penetration Testing Execution Standard): phases, deliverables, legal framework
- Passive reconnaissance: WHOIS, ARIN, BGPView, DNS history (SecurityTrails, PassiveTotal), Shodan, Censys
- OSINT: TheHarvester (emails, subdomains), amass (subdomain enum), subfinder, crt.sh (certificate transparency)
- Active reconnaissance: Nmap (-sS, -sT, -sU, -O, -sV, --script, -p-, --min-rate)
- Nmap scripting engine (NSE): default, safe, vuln categories, writing custom scripts
- Service enumeration: enum4linux (SMB), ldapsearch (LDAP), snmpwalk (SNMP), smtp-user-enum, dnsenum
- Vulnerability scanning: OpenVAS/Greenbone, Nessus (educational), nmap --script vuln
- Enumeration: NetBIOS, SMB (smbclient, smbmap), SNMP (community strings), DNS (zone transfer, subdomain brute-force), SMTP (VRFY, EXPN)
- Web enumeration: gobuster/ffuf (directory/file brute-force), nikto (web scanner), whatweb (technology detection), wpscan (WordPress)
- Exploitation: Metasploit framework (msfconsole, msfvenom), searchexploit, manual exploitation
- Payload types: reverse_shell, bind_shell, meterpreter, staged vs stageless, encoding
- Post-exploitation: enumeration (system info, network, users, processes), privilege escalation vectors
- Privilege escalation: kernel exploits, SUID binaries, sudo misconfig, writable paths, cron jobs, capabilities
- Lateral movement basics: pass-the-hash (concept), pivoting, port forwarding (ssh -L, ssh -R, chisel)
- Parol hashing: NTLM, bcrypt, argon2 — offline cracking (john the ripper, hashcat modes)
- Brute force: Hydra (SSH, FTP, HTTP), Medusa, ncrack — wordlists (rockyou, seclists)
- Веб-уязвимости: SQLi (error-based, blind), XSS (reflected, stored), CSRF — на уровне концепций + простые примеры
- OWASP Top 10 overview: injection, broken auth, sensitive data exposure, XXE, SSRF, broken access control
- Reporting: structure (executive summary, methodology, findings, recommendations), severity (CVSS), evidence (screenshots, logs)
- Безопасная среда: изолированные VM, HackTheBox, TryHackMe, VulnHub — правила безопасности

**Практика:**

- HackTheBox: 5–10 машин начального уровня с пошаговым writeup, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- TryHackMe: Complete Beginner, Pre-Security, Introductory Networking pathways, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- VulnHub: загружаемые образы (Kioptrix, Mr.Robot, Hackfile), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Nmap deep dive: 10 разных сканирований с разбором вывода, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Metasploit practice: exploit через msfconsole → meterpreter → post-exploitation → report, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Инструменты:** Nmap, Metasploit, Netcat, Wireshark, Burp Suite (Community), john, hashcat, Hydra, gobuster, ffuf, enum4linux, bloodhound (concept)

**Проекты:**

- Writeup 5机器: каждый machine с phases (recon → enum → exploit → privesc → proof.txt), screenshots, lessons learned, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Automated recon script: bash/python скрипт для автоматического сбора информации (nmap, gobuster, enum4linux) с форматированным выводом, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Vulnerability report: для 3靶机 отчёт в формате PTES с CVSS scoring, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** *The Ethical Hacker's Handbook*, PTES (Penetration Testing Execution Standard), HackTheBox Academy, TCM Security (Practical Ethical Hacking), *Hacking: The Art of Exploitation* (Erickson), payloadsallthethings

---

### 9. Rust для сетевого программирования (60 часов)

**Время:** 60 часов

**Что изучать:**

- Ownership/borrowing в контексте сетевых буферов: lifetime аннотации для &str/&[u8], borrowed data в async contexts
- References vs ownership: move semantics, Copy/Clone traits, borrow checker в сетевом коде
- Async networking: tokio runtime, `TcpListener`, `UdpSocket`, `tokio::io::AsyncRead/AsyncWrite`
- Tokio internals: current_thread vs multi_thread runtime, task spawning, cooperative scheduling
- Channels в tokio: mpsc, broadcast, oneshot, watch — для communication между tasks
- Безопасная обработка неблагонадёжного ввода без UB и data races: proper error handling, Result propagation
- Библиотеки: bytes crate (Bytes, BytesMut), tokio-util (codec, framing), pin-project
- FFI с C библиотеками (libpcap, libnetfilter_queue) для гибридных сетевых инструментов
- error handling: thiserror, anyhow, custom error types, Backtrace для debugging
- Серийализация/десериализация: serde, serde_json, toml, bincode для сетевых протоколов
- Логирование: tracing crate (spans, events), env_logger, log macros
- Конфигурация: clap (derive macro), toml config files
- Инструменты производительности: `cargo bench`, `criterion`, `tokio-console`, flamegraph
- Тестирование: #[tokio::test], mockall для мока, proptest для property-based тестов
- Process management: ctrlc crate, graceful shutdown patterns, signal handling
- Файловая система: tokio::fs, async read/write, tempdir для temporary files
- HTTP: reqwest (async client), axum/warp (async server), tower middleware
- Генерация кода: prost (protobuf), tonic (gRPC), build.rs

**Практика:**

- Установка Rust: rustup, cargo, rust-analyzer, clippy, rustfmt, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- TCP echo server на tokio: async/await, multi-client, graceful shutdown, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- UDP broadcast chat: multicast, message framing, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- HTTP client на reqwest: GET/POST, retry, timeout, JSON parsing, streaming download, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- FFI with libpcap: привязка через unsafe, packet capture callback, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Простой HTTP сервер на axum: routing, handlers, JSON response, middleware, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- Асинхронный TCP proxy на Rust с метриками и graceful shutdown: request/response logging, connection pooling, timeout, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Безопасный parser бинарного сетевого протокола с fuzz-тестами: bytes crate, custom framing, proptest, cargo-fuzz, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Async DNS resolver: upstream forwarding, caching, UDP/TCP fallback, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Simple HTTP load tester: concurrent connections, latency percentiles, JSON report, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Rust Network Toolkit: proxy + packet parser + pcap analyzer + metrics exporter с профилированием, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Async HTTP service framework: axum-based, middleware, auth, rate limiting, JSON, WebSocket support, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** *Rust for Rustaceans* (Jon Gjengset), *The Rust Programming Language* (book), Tokio docs, Rust Async Book, *Zero To Production In Rust* (Luca Palmieri)

---

### 10. Продвинутая отладка сетевых приложений (60 часов)

**Время:** 60 часов

**Что изучать:**

- `rr` record/replay: установка, record session, replay, share session, Time-Travel debugging для детерминированного разбора race-condition
- `perf` основы: stat, record, report, top, sleep profiling, flame graph generation
- `perf` для сетевых: perf top -g (hot spots), net:[*] tracepoints, software events
- Flame graphs: FlameGraph toolkit, Brendan Gregg's script,差分 flame graphs (differential flamegraphs)
- uprobes/kprobes для корреляции user/kernel сетевой активности: uprobe на ssl_write, kprobe на tcp_sendmsg
- Диагностика lock contention: perf lock, contention between threads, mutex profiling
- Syscall stalls: perf sched latency, off-CPU analysis, blocked function identification
- Packet processing bottlenecks: throughput vs latency profiling, CPU affinity, RSS/RFS
- Postmortem debugging: core dumps (gcore, ulimit -c), gdb offline analysis в production-like окружении
- ASan/TSan/UBSan: AddressSanitizer, ThreadSanitizer, UndefinedBehaviorSanitizer для поиска memory/race bugs в сетевом коде
- valgrind --tool=helgrind: race condition detection в threaded network server
- Memory profiling: valgrind massif, heaptrack, alloc tracking
- CPU profiling: gprof, perf record, simpleperf (Android), pprof (Go), cargo-flamegraph (Rust)
- Latency analysis: histograms, percentiles (p50/p95/p99), HdrHistogram
- Latencyheatmap: визуализация latency по времени (ASCII или plotting)
- Debugging TLS: SSLKEYLOGFILE + Wireshark, strace + gdb combo
- Debugging DNS: strace on resolver, tcpdump on port 53, dig tracing
- Debugging HTTP/2: nghttp, Wireshark HTTP2 filter, h2load profiling
- Thread dump analysis: jstack (Java), gdb attach, /proc/[pid]/task, thread naming conventions
- Deadlock detection: strace -f + backtrace analysis, lockdep (kernel)

**Практика:**

- Разбор 3 сложных багов (latency spike, packet loss, deadlock) с `rr`/`perf`: пошаговый workflow от发现问题到定位root cause, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Автоматизация профилирования в CI для сетевого сервиса: perf record → flamegraph → regression check, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Thread sanitizer для C/Go сетевого сервера: TSAN report analysis, fix, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Flame graph generation и анализ: permalink для comparison, hotspot identification, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Off-CPU analysis: bpftrace/perf скрипт для определения где thread блокируется, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- Network Performance Forensics Suite: rr/perf pipeline + авто-генерация отчёта с flame graphs, latency histograms, hotspot report, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Automated regression profiling: CI integration для detection performance regressions в network services, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Debug playbook: 10 типичных проблем с пошаговым workflow (strace → perf → flamegraph → fix), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Network Performance Forensics Suite: rr/perf/bpftrace pipeline + automated report generation + CI integration, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** perf docs, rr docs, Brendan Gregg Flame Graphs, *Systems Performance* (Brendan Gregg), *How Linux Works* (Ward)

---

### 11. Дизайн и реверс кастомных протоколов (60 часов)

**Время:** 60 часов

**Что изучать:**

- Протокол design principles: separation of concerns, backward/forward compatibility, versioning strategy
- Проектирование бинарных протоколов: framing (length-prefixed, delimiter-based, fixed-length), versioning, header/body separation
- Текстовые протоколы: SMTP, HTTP/1.1 style — command-response, parsing strategies
- Бинарные форматы: endianness (network byte order), field alignment, padding, reserved fields
- Безопасность протоколов: replay protection (nonce, timestamp, sequence numbers), auth (HMAC, digital signatures), integrity, anti-downgrade
- Сериализация: Protocol Buffers, MessagePack, Cap'n Proto, FlatBuffers — сравнение
- Кодирование: varint, fixed-width, base64/hex, LZ4/snappy/zstd compression
- Формальная спецификация: protocol state machines, diagram notation (Mermaid), test vectors
- Тестирование совместимости: multi-implementation testing, fuzzing, property-based testing
- Написание Wireshark dissector для собственного протокола: Lua plugin, Lua protocol registration, item Tree, topic/group
- Фаззинг протоколов: Sulley/Peach/Boofuzz сценарии, mutation vs generation подходы, coverage feedback и crash triage
- car案crafting: Scapy для генерации тестовых пакетов,pcrafting for protocol testing
- Протокольные state machines: формальное описание через state diagram, проверка на finite state automat
- Retry/backoff: exponential backoff, jitter, circuit breaker, timeout на уровне protocol
- Flow control: credit-based, window-based, rate limiting на уровне application
- Группировка: multiplexing/demultiplexing, stream vs datagram semantics
- Метаданные протокола: headers, trailers, TLV (Type-Length-Value) patterns
- Примеры реальных протоколов: QUIC (RFC 9000), gRPC (HTTP/2 + protobuf), WireGuard (simple but secure)

**Практика:**

- Проектирование бинарного протокола для chat application: message types, framing, auth, encryption — спецификация + reference implementation на C/Rust, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Реверс неизвестного протокола из pcap: Wireshark + Scapy analysis → inference packet structure → write dissector, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Фаззинг протокола: boofuzz сценарий для binary protocol, crash triage, минимизация repro, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Написание Wireshark dissector на Lua для кастомного протокола, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Протокольный fuzz test: property-based тестирование (proptest/custom) на соответствие спецификации, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- Разработать собственный secure протокол для telemetry/control канала: спецификация + 2 реализации (C/Rust) + fuzz report, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Написать dissector + negative test suite + fuzz report для кастомного протокола, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Protocol state machine: specification + implementation + verification against invalid inputs, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Custom Secure Protocol Stack: RFC-style specification + 2 implementations (C/Rust) + Wireshark dissector + fuzz campaign + compatibility test suite, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** RFC 9000 (QUIC), protobuf docs, Wireshark dissector docs, *The Design and Implementation of the FreeBSD Operating System* (networking chapters), *TCP/IP Illustrated* том 1

---

## Семестр 5 - Специализация (Февраль - Июнь)

**Время:** ~320 часов

### Общие темы

#### 1. Продвинутая сетевая безопасность

**Время:** 80 часов

**Что изучать:**

- **Атаки на Network Layer:**
  - IP spoofing: source address forgery, BCP38/BCP84 (uRPF), edge filtering
  - ICMP redirect: man-in-the-middle via routing manipulation, mitigation (ignore ICMP redirects)
  - ARP spoofing/poisoning: L2 mitm, gratuitous ARP injection,ettercap/bettercap demonstration
  - ICMP flood: ping flood, smurf attack amplification, rate limiting mitigation
  - IP fragmentation attacks: Teardrop, fragmentation overlap, overlapping fragment reassembly exploit

- **Атаки на Transport Layer:**
  - TCP SYN flood: SYN cookies (net.ipv4.tcp_syncookies), SYN proxy, SYN backlog tuning
  - TCP state exhaustion: large number of half-open connections, TIME_WAIT accumulation
  - UDP flood: amplification (DNS/NTP/Memcached/SSDP), reflected amplification attack chains
  - TCP RST injection: forged RST to terminate connections, sequence prediction
  - TCP sequence prediction: blind spoofing, ISN randomization

- **Атаки на Application Layer:**
  - HTTP flood: GET/POST flood, layer 7 DDoS, behavioral fingerprinting
  - Slowloris: keep connections open with slow headers, connection pool exhaustion
  - R-U-Dead-Yet (RUDY): slow POST body, thread starvation
  - DNS amplification: request to open resolver with spoofed src, 50-100x amplification
  - NTP monlist amplification: reflected NTP DDoS
  - Memcached amplification: UDP-based, extreme amplification factor (10,000x+)

- **DDoS:**
  - Types: volumetric (L3/L4), protocol attacks (L3/L4), application layer (L7)
  - Botnets: Mirai, Emotet — concepts, IoT botnet recruitment
  - Mitigation: blackhole routing (RTBH), scrubbing centers, CDN absorption, cloud-based (CloudFlare, AWS Shield)
  - Detection: traffic baseline, anomaly detection, flow analysis (NetFlow/sFlow/IPFIX)

- **IDS/IPS:**
  - Snort: rule structure (action proto src dst), content keywords (content, nocase, depth, offset, distance, within), flowbits, pcre, threshold
  - Snort rules: 完整规则范例 для portscan, C2 beaconing, data exfil, malware download, brute force
  - Suricata: architecture (multi-threaded), eve.json, fast.log, stats.log, alerts
  - Suricata rules: same format as Snort + suricata-specific (file_data, http_uri, tls_cert_subject)
  - Suricata YAML: runmodes, stream config, logging output, lua scripting
  - Suricata update: signature management, custom rules,سورس集管理

- **Network detection:**
  - Signature-based: pattern matching, trade-offs (speed vs evasion)
  - Anomaly-based: baseline deviation, ML approaches, false positive challenges
  - Behavior-based: protocol anomaly, traffic pattern analysis, Encrypted traffic analysis (JA3/JA3S, JARM)

- **Zeek (formerly Bro):**
  - Architecture: sensor, manager, logging framework
  - Connection logs: conn.log, dns.log, http.log, ssl.log, files.log
  - Zeek scripts: event-driven, signature-based detection, custom log writers
  - Zeek + Suricata integration: complement approaches
  - Writing custom Zeek scripts for specific detection

- **NetFlow/sFlow/IPFIX:**
  - Architecture: exporter → collector → analyzer
  - Analysis: top talkers, traffic matrix, anomaly detection, long-lived connections
  - Tools: nfdump, nfsen, ElastiFlow, Kentik

- **Packet crafting:**
  - Scapy: packet creation, layers (Ether/IP/TCP/UDP), send/receive, sniffing
  - Packet crafting for testing: generating attack traffic for IDS validation
  - Hping3: crafting custom packets, SYN scan, ICMP flood, traceroute with spoofed source

- **Firewall deep dive:**
  - iptables: tables (filter/nat/mangle/raw), chains (INPUT/OUTPUT/FORWARD/PREROUTING/POSTROUTING), stateful tracking (NEW/ESTABLISHED/RELATED/INVALID), string matching, time-based rules
  - nftables: tables/sets/maps, dynamic sets, flow offload, CT (connection tracking) helpers, meter-based rate limiting
  - firewalld: zones, rich rules, direct rules, lockdown, ipset integration
  - fail2ban: filter definitions, jail configuration, actions (ban/unban), recidive, trajectory analysis
  - CrowdSec: community-driven, scenario/collection/decisions, Bouncer architecture, behavioral analysis

- **Honeypots:**
  - Cowrie: SSH/Telnet honeypot, session recording, command logging, interaction profiles
  - Dionaea: low-interaction, captures malware binaries, protocol emulation (SMB, HTTP, FTP)
  - T-Pot: multi-honeypot platform, ELK stack integration, attack visualization
  - Sebek: kernel-level honeypot (kernel module), keystroke capture
  - Honeypot deployment: placement, risk management, legal considerations, data handling

**Практика:**

- ARP spoofing lab:ettercap/bettercap mitm, HTTP/DNS credential capture, detection via ARP table monitoring, DAI mitigation, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- TCP SYN flood attack + defense: hping3 flood → SYN cookies enabled → connection count verification, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- DNS cache poisoning + DNSSEC: attack with spoofed responses → DNSSEC validation failure → signed zone verification, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Suricata rules authoring: 15 production rules для разных атак (portscan, C2, exfil, brute force), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- False positive analysis: Suricata + легитимный трафик → tuning suppression rules, thresholding, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Zeek custom script: detection for SSH brute force + HTTP SQLi attempts, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Scapy packet crafting: generate custom packets for testing IDS rules, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- fail2ban deployment: jail для SSH + HTTP + DNS, ban actions, trajectory analysis, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Cowrie honeypot: deployment, interaction logging, analysis of captured commands, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- iptables/nftables deep: 10 правил для разных сценариев (rate limit, string match, time-based, stateful), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- Обнаружение сканирования портов через анализ аномалий в логах: anomaly detection pipeline, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Набор Suricata rules для 10 атак: port scan, C2 beaconing, data exfil, brute force, malware download, DNS tunneling — с validation на replay traffic, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Zeek + Suricata + Elasticsearch pipeline: logs → parsing → dashboard → alerting, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Анализ дампа атаки: полный разбор pcap с атаками → identification of all indicators → defensive recommendations, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Honeypot deployment: Cowrie + Dionaea + ELK → analysis of captured data → threat intelligence extraction, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Network IDS/IPS: Suricata + Zeek + custom rules + Elasticsearch dashboard + alerting pipeline + honeypot integration, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- DDoS mitigation tool: обнаружение аномального трафика (baseline deviation) + автоматическая фильтрация + NetFlow analysis, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** Suricata docs, Snort manual, Zeek docs, *The Practice of Network Security Monitoring* (Bejtlich), *Network Security Monitoring* (httpcs), Snort Cookbook

---

#### 2. Файрволы и сетевая защита

**Время:** 60 часов

**Что изучать:**

- **Файрволы Stateful:** отслеживание соединений (conntrack), state table, connection tracking в Linux kernel (nf_conntrack)
- **Файрволы stateless:** ACL only — use cases, limitations
- **Фильтрация:** по адресам источника/назначения, портам, протоколам, TCP flags, ICMP types, time-based rules
- **Фильтрация по состоянию:** NEW, ESTABLISHED, RELATED, INVALID, UNTRACKED
- **NAT и PAT:** SNAT (source NAT), DNAT (destination NAT), overload (PAT), masquerade, hairpin NAT, static NAT, NAT tables (PREROUTING, POSTROUTING)
- **Port forwarding:** проброс портов наружу-внутрь, DNAT rules, multi-port forwarding, reflection
- **Proxy:** application-level gateway (ALG), transparent proxy (iptables REDIRECT/TPROXY), explicit proxy (squid)
- **Proxy protocols:** HTTP CONNECT, SOCKS4/SOCKS5, MITM TLS proxy
- **Web Application Firewall (WAF):**
  - ModSecurity: OWASP CRS (Core Rule Set),SecRule, phase, action (deny, pass, log), anomaly scoring
  - NAXSI: nginx WAF, whitelisting, learning mode
  - Встроенные WAF: CloudFlare, AWS WAF, Azure WAF — managed rules, custom rules
- **DNS firewall:** RPZ (Response Policy Zone), Response Policy Zone triggers, DNS sinkholing
- **DMZ:** размещение сервисов с разными уровнями доверия, dual-firewall DMZ, single-firewall with sub-interfaces
- **VPN (введение):**
  - IPSec: AH (Authentication Header), ESP (Encapsulating Security Payload), tunnel vs transport mode
  - IKEv1/IKEv2: SA negotiation, Phase 1/Phase 2, Diffie-Hellman groups, PFS (Perfect Forward Secrecy)
  - OpenVPN: TLS-based, TUN/TAP, certificate management, client profiles
  - WireGuard: Curve25519, ChaCha20-Poly1305, simple configuration, key management
  - SSL VPN: AnyConnect/OpenConnect, posture checks, split tunneling
- **NAT Traversal:** ICE, STUN, TURN, hole punching, UPnP (security risk)
- **Split tunneling:** плюсы и минусы с точки зрения безопасности, DNS leakage, bypass controls
- **Connection tracking:** conntrack tools (conntrack -L, conntrack -E), zone isolation, helper modules (ftp, sip, h323)
- **Advanced nftables:** sets (named/anonymous), maps, meters, flow offload, CT helpers, SYNPROXY, rate limiting per IP
- **Advanced iptables:** string matching (-m string), time matching (-m time), recent module, hashlimit, geoip
- **Firewall hardening:** default deny, logging dropped packets, rate limiting management access, jump rules

**Инструменты (с командами):**

- iptables: написание правил для разных сценариев, таблицы filter/nat/mangle, chain management
- nftables: современная замена iptables, atomic rule changes, sets, maps
- firewalld: динамическое управление зонами (public, internal, drop), rich rules, direct rules
- UFW: uncomplicated firewall для Ubuntu, Application profiles
- pfSense/OPNsense: web-based firewall на VM,.package installation, VPN configuration
- OPNsense: IDS/IPS модуль (Suricata integration), HA proxy

**Практика:**

- iptables/nftables: 20 правил для разных сценариев (web server protection, SSH rate limit, anti-spoof, DNS server, mail server), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- pfSense lab: install, configure WAN/LAN, NAT, firewall rules, VPN (OpenVPN/WireGuard), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- ModSecurity + OWASP CRS: install on nginx, test with SQLi/XSS payloads, false positive tuning, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- VPN lab: WireGuard server/client setup, IPSec site-to-site (strongSwan), OpenVPN with certificate auth, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- conntrack analysis: track connection states, identify SYN flood by conntrack table size, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Transparent proxy: squid + iptables REDIRECT для HTTP traffic, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- Полноценный файрвол для веб-сервера: iptables/nftables rules + fail2ban + connection tracking + logging, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- pfSense/OPNsense deployment: WAN/LAN/DMZ zones, NAT, firewall rules, OpenVPN remote access,_IDS, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- WAF configuration: ModSecurity + OWASP CRS + nginx, custom rules, exception handling, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- VPN deployment comparison: WireGuard vs IPSec vs OpenVPN benchmark (throughput, latency, setup complexity), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** iptables-tutorial (Oskar Andreasson), nftables wiki, pfSense docs, OPNsense docs, *Linux Firewalls* (Hartig)

---

### **Дорожка A: Enterprise Red Team (Classic AD / Windows / EDR Evasion)**

#### 1. Python для автоматизации пентеста

**Время:** 50 часов

**Что изучать:**

- Scapy: создание пакетов, отправка, сниффинг
- asyncio для сетевых операций: aiohttp, asyncio sockets
- Парсинг PCAP/PCAPNG: dpkt, pyshark, scapy
- Network automation: netmiko, NAPALM, paramiko
- Потоковый сканер портов: asyncio + masscan
- Асинхронный брутфорс: medusa, hydra (или asyncio-версия)
- Автоматизация nmap: python-nmap
- Создание кастомных инструментов анализа
- Интеграция с Metasploit через pymetasploit
- Веб-сканер: requests + BeautifulSoup + selenium

**Проекты:**

- Сканер портов на asyncio, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Анализатор PCAP с визуализацией, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Автоматизированный инструмент рекогнито, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Брутфорсер с async, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT:**

- Automated pentest toolkit: рекогнито, сканер, эксплуататор, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** Scapy docs, asyncio documentation, Python for Security Professionals

---

#### 2. Red Team Infrastructure 

**Время:** 50 часов

**Что изучать:**

- **C2 (Command & Control) фреймворки:**
  - Cobalt Strike: Team Server, Beacon, Malleable C2 profiles, listeners (HTTP/HTTPS/DNS/SMB), staging/execute-assembly
  - Havoc: бесплатная альтернатива, демо-структура, расширяемость
  - Mythic: Python-based, агенты, плагины, перезапускаемые payloads
  - Sliver: Bishop Fox, стабильный, cross-platform, minicampal
  - Empire: PowerShell + Python post-exploitation
  - Сравнение: возможности, лицензии, активность сопровождения, evasive capability

- **Malleable C2 профили (Cobalt Strike):**
  - Профиль структура: http-get/http-post blocks, headers, URIs, user-agent, netbios
  - Transformation blocks: base64, base64url, netbios, prepend/append, mask
  - Сетевые сигнатуры: характерные паттерны, детекты IPS/NDR, fingerprint (JA3/JARM)
  - Правильный выбор: имитация легитимного HTTP, избежание известных сигнатур
  - Тестирование профиля: surfaceto, HTTP fingerprints, реальные детекты

- **Domain Fronting & CDN hiding:**
  - Domain Fronting: использование CDN как прокси, HTTP Host header замаскирован
  - Реализация: nginx + TLS SNI vs Host mismatch, CDN support
  - Альтернативы: DomainFronting снижение, использование cloud functions (AWS Lambda, Cloudflare Workers)
  - Redirectors: Azure CDN, Cloudflare Workers, AWS Lambda, customnginx

- **Redirectors:**
  - Nginx redirector: proxy_pass, SSL termination, per-domain redirects
  - Redirector types: TCP/SSL passthrough, HTTP forward, domain fronting
  - Мульти-редиректор: LAG (Load Balancing), failover, random access

- **OpSec для Red Team:**
  - Чистые VM на каждый проект: изоляция, snapshots, no cross-contamination
  - Аутентификация: без личных аккаунтов, VPN на входе, MFA
  - Инструменты: без обфускации не использовать, custom profiles
  - Инфраструктурное hygiene: password manager, keys, креды зашифрованы
  - Журналирование: вести лог операций, artifact tracking

- **Инфраструктурное развертывание:**
  - Автоматизация: Terraform/Ansible для деплоя C2, профили, окружения
  - Мониторинг: следить за доступностью, health checks телеграм/email
  - Recovery: бэкапы, restore runbook, emergency kill switch (fail-safe)

**Практика:**

- Развернуть Sliver/Mythic C2 server на VPS с HTTPS, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Настроить nginx redirector базовый (proxy_pass), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Реализовать domain fronting через CDN (Cloudflare Workers/AWS Lambda) — учебный кейс, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Написать Malleable C2 профиль, протестировать HTTP fingerprint и детекты, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Автоматизировать деплой инфраструктуры через Terraform/Ansible, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- Развернуть полную инфраструктуру Red Team: C2 (Mythic/Sliver) + Redirector (nginx) + Domain Fronting (CDN) + отчет об OPSEC, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Создать Malleable C2 профиль для имитации легитимного трафика, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Построить multi-layer config: VPS → CDN → redirector → teamserver → operator, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Red Team C2 Infrastructure-as-Code: полностью автоматизированное развертывание инфраструктуры (Terraform) + OPSEC checklist + recovery runbook + traffic fingerprint analysis, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** Cobalt Strike docs (Malleable C2), Sliver docs, Mythic docs, nginx docs, *Red Team Field Manual*

---

#### 3. Active Directory: Атаки и защита (50 часов)

В Network Roadmap уже есть AD, но нужно дать конкретику для Red Team:

**Что добавить (конкретные атаки):**

```yaml
Initial Access (Kerberos):
  - AS-REP Roasting (нет пред-аутентификации)
  - Kerberoasting (запросить билет на сервис)
  - Pass-the-Ticket (использовать билет из другой сессии)
  - Golden Ticket (создать билет администратора)
  - Silver Ticket (создать билет на конкретный сервис)
  - Diamond Ticket (легитимно выданный, но модифицированный)

Lateral Movement:
  - Overpass-the-Hash (pass the hash to Kerberos)
  - Pass-the-Hash (NTLM)
  - Pass-the-Certificate (с сертификатом)
  - DCOM/WMI/PSEXEC/WinRM атаки
  - PtH (Pass the Hash)

Privilege Escalation:
  - ACLs abuse (WriteProperty, WriteOwner, GenericAll)
  - GPO abuse (групповые политики)
  - AD CS атаки (ESC1-ESC8 из Certified Pre-Owned)
  - Shadow Credentials (Key Credential Link)
  - RBCD (Resource-Based Constrained Delegation)

Persistence:
  - AdminSDHolder (права на группу админов)
  - Skeleton Key (маппинг на DC)
  - DSRM (Directory Services Restore Mode)
  - Group Policy persistence
```

**Инструменты:**

- Impacket (секретное оружие для AD!), с акцентом на реальные команды, ограничения инструмента и использование в troubleshooting/incident response сценариях.
  - `secretsdump.py` — дамп хешей
  - `getTGT.py` — получить билет
  - `wmiexec.py` — выполнение по сети
- Rubeus (работа с Kerberos), с акцентом на реальные команды, ограничения инструмента и использование в troubleshooting/incident response сценариях.
- PowerView (часть PowerSploit), с акцентом на реальные команды, ограничения инструмента и использование в troubleshooting/incident response сценариях.
- BloodHound (карта связей и атак), с акцентом на реальные команды, ограничения инструмента и использование в troubleshooting/incident response сценариях.
- SharpHound (сбор данных для BloodHound), с акцентом на реальные команды, ограничения инструмента и использование в troubleshooting/incident response сценариях.

**Практика:**

```bash
# Пример атаки на AD в лаборатории
BloodHound → найти путь к Domain Admin (DACL abuse) 
→ Rubeus: AS-REP Roasting → получить хеш 
→ CrackMapExec: pass-the-hash → доступ
→ Mimikatz: Golden Ticket → полный контроль
```

**Проект:**

> "Dominate the Domain" — автоматизировать захват домена через цепочку из 3-5 атак без использования известных уязвимостей (только misconfigurations)

---

#### 4. EDR/AV Evasion (40 часов) 

Критично для Red Team: современный пентест без обхода EDR (CrowdStrike, SentinelOne, Microsoft Defender) бесполезен.

**Что изучать:**

- **Как работает EDR:**

  - ETW (Event Tracing for Windows) — основной источник телеметрии
  - Kernel callbacks (Process/Thread/Image Load)
  - AMSI (Anti-Malware Scan Interface) для PowerShell/Script
  - User Mode Hooks (ntdll.dll — Export Address Table hooks)

- **Методы обхода:**

  - **Direct Syscalls** — вызовы ядра без ntdll (Hell's Gate, Halos Gate)
  - **DLL Unhooking** — перезагрузить чистую ntdll
  - **Process Injection:**
    - Classic: CreateRemoteThread (детектится)
    - Advanced: Process Hollowing, Atom Bombing, Early Bird APC
  - **PPID Spoofing** — подделать родительский процесс (explorer.exe)
  - **Block DLLs** — заблокировать загрузку DLL мониторинга
  - **Inline Hooking** — захукать функции API
  - **Obfuscation** — обфускация кода (не строк!)

- **Beacon Object Files (BOF)** — как писать маленькие C-функции для выполнения в Beacon

**Инструменты:**

- **pe_to_shellcode** — превратить PE в шелл-код, с акцентом на реальные команды, ограничения инструмента и использование в troubleshooting/incident response сценариях.
- **donut** — создание position-independent code, с акцентом на реальные команды, ограничения инструмента и использование в troubleshooting/incident response сценариях.
- **ScareCrow** — загрузка Cobalt Strike beacon через EDR, с акцентом на реальные команды, ограничения инструмента и использование в troubleshooting/incident response сценариях.
- **Shoggoth** (свой проект) — универсальный обход, с акцентом на реальные команды, ограничения инструмента и использование в troubleshooting/incident response сценариях.

**Проект:**

> "AV/EDR Evasion Loader" — программа на C/C++, которая загружает Meterpreter через direct syscalls + отключает ETW и AMSI

---

#### 5. OSINT и External Recon (30 часов)

Red Team начинается с внешней разведки.

**Время:** 30 часов

**Что изучать:**

- **Пассивный OSINT (без взаимодействия с целью):**
  - Shodan: поиск по тегам (product, port, ssl.cert.org, vuln), сетевое картографирование, экспорт
  - Censys: расширенные запросы, cert domains, порты, сервисы
  - BinaryEdge: risk score, data leak DB, экспонированные сервисы
  - Shodan host search: протоколы, banners, эксплуатируемые сервисы
  - SecurityTrails: история DNS, subdomains, associated domains
  - PassiveTotal (RiskIQ): passive DNS, SSL/TLS certificate transparency, associated infrastructure
  - Certificate Transparency logs: crt.sh, Censys cert search — исторические сертификаты/хосты
  - GitHub Dorking: поиск коммитов с секретами, .env, tokens (query: path:.env, extension:env, language:shell)
  - Google Dorks: site:, intitle:, inurl:, filetype:, "index of", intext — осмысленные запросы для утечек
  - Wayback Machine: исторический контент, устаревшие endpoints, старые версии
  - DNS reflection: SPF/DMARC/DKIM записи для email-инфраструктуры

- **Активный OSINT (с осторожностью!):**
  - The Harvester: email + домены через пассивные источники, перекрёстная проверка
  - Amass: subdomain enumeration (passive + active + brute), синонимы для доменов
  - Sublist3r: быстрый subdomain scan
  - subfinder: пассивные источники (cert, DNS brute)
  - EyeWitness: скриншоты веб-сервисов, fingerprinting
  - httpx: HTTP probing, tech detection, status codes
  - Nuclei: template-based vulnerability scanning (recon phase)

- **Социальная инженерия (OSINT на сотрудников):**
  - LinkedInt: сбор данных из LinkedIn (должности, тех-стек, контакты)
  - Hunter.io: поиск email по домену, validation
  - Holehe: проверка email регистраций на сервисах
  - DeHashed: утечки паролей, где скомпрометирован email
  - Google: поиск сотрудников, публичные презентации, конференции
  - Заключение: из каких данных можно сформировать phishing target

- **Инфраструктурная карта:**
  - Сопоставление: домены → IP ranges (ASN), cloud providers, hosting/CDN
  - Технологический fingerprint: WAF (Cloudflare, Akamai), CMS (WordPress, Drupal), серверы
  - Определение целей: mail server, VPN, remote access, dev/staging

- **Методология:**
  - Passive first: собрать максимум без контакта (Shodan, crt.sh, history)
  - Active second: целенаправленный contact (aims, ports)
  - Документирование: структурированный report с mapped infra, targets, risk ranking

**Практика:**

- Пассивная разведка домена: subdomains, certs, DNS history, Shodan — собрать полную карту, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- GitHub Dorking: поиск утечек секретов по GPU-запросам, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Employee OSINT: email patterns, контакты, LinkedIn — построить phishing target profile, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Инфраструктурная карта: сайт → ASN → hosting → WAF/CDN → веб-сервисы, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- OSINT автоматизатор на Python: по домену собирает поддомены → IP → порты → email сотрудников → утечки паролей → выдаёт отчёт, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Полный recon report по реальной лабораторной цели: пассивный + активный + phishing-ready профиль, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Full External Recon Toolkit: автоматический сбор всех фаз (passive → active → mapping) с картой инфраструктуры и приоритизацией целей, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** Shodan docs, Amass docs, TheHarvester docs, OSINT framework (online), *Open Source Intelligence Techniques* (Bazzell)

---

#### 6. Custom C2 Framework Engineering (60 часов) 

**Время:** 60 часов

**Что изучать:**

- Архитектура C2: teamserver, redirectors, agents, encrypted tasking
- OpSec для C2 инфраструктуры: jitter, domain rotation, traffic shaping, kill-switch
- Malleable C2 профили и сетевые сигнатуры защитных систем
- Реализация task queue, file transfer, operator audit trail
- Защита C2 инфраструктуры от takeover и утечки артефактов

**Проекты:**

- Мини C2 на Go/Rust: registration, heartbeat, encrypted command execution, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Построение redirector цепочки: VPS -> CDN -> redirector -> teamserver, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Red Team C2 Pipeline: отказоустойчивая инфраструктура + opsec-checklist + recovery runbook, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** Sliver docs, Mythic docs, Cobalt Strike docs

---

#### 7. Advanced AD Attacks: ESC1-ESC8 и делегации (70 часов) 

**Время:** 70 часов

**Что изучать:**

- AD CS атаки: ESC1-ESC8 (Certified Pre-Owned)
- Shadow Credentials и Key Credential Link abuse
- RBCD и цепочки эскалации
- ACL abuse advanced: WriteOwner/GenericAll/DCSync pathing
- Detection/mitigation рекомендации после каждой техники

**Проекты:**

- Лабораторная цепочка: low-priv user -> certificate abuse -> domain compromise, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- AD attack graph: BloodHound + приоритетный remediation plan, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- AD Domination Enterprise Lab: 5 атак (ESC + RBCD + Shadow Creds + DCSync + persistence), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** *Certified Pre-Owned*, SpecterOps blogs, BloodHound docs, Impacket docs

---

#### 8. EDR/AV Evasion Engineering (60 часов)

**Время:** 60 часов

**Что изучать:**

- ETW, AMSI, user-mode hooks, kernel callbacks
- Direct syscalls (Hell's Gate/FreshyCalls), unhooking ntdll, PPID spoofing
- Process injection: Hollowing, Early Bird APC, Section mapping
- Beacon Object Files (BOF): компактные C-модули
- Безопасное тестирование только в изолированных стендах

**Проекты:**

- Direct syscall loader в лаборатории с telemetry comparison, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- ETW/AMSI bypass PoC (контролируемое окружение), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Universal EDR Evasion Framework: syscalls + unhooking + AMSI bypass + BOF execution, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** *Evading EDR* (Matt Hand), SysWhispers, BOF docs, Outflank research

---

### **Дорожка B: Cloud Native Red Team — Kubernetes & Cloud Security**

#### 1. Go: Высоконагруженные сетевые сервисы

**Время:** 70 часов

**Что изучать:**

- Go basics: пакеты, импорт, функции, методы
- Типы данных: int, float, string, bool, array, slice, map, struct
- Управление потоком: if, switch, for, range
- Goroutines: go func(), каналы, channel directions
- Каналы: make channels, select, close, buffered/unbuffered
- Синхронизация: sync.Mutex, sync.WaitGroup, sync.Once
- Пакет context: context.WithCancel, timeout, deadline
- net/http: Handler, ResponseWriter, Request, mux
- JSON: encoding/json, Marshal, Unmarshal
- HTTP/2: enable http2 в библиотеке
- gRPC: protobuf, protoc, generation, unary/streaming
- Таймауты и retry: time.Ticker, retry logic
- Graceful shutdown: сигналы, остановка сервера
- Производительность: pprof, benchmarks, профилирование

**Проекты:**

- HTTP API с middleware: логирование, аутентификация, rate limiting, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Микросервис на gRPC с protobuf, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Простой балансировщик нагрузки на Go, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Pastebin сервис: создание и получение ссылок, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- URL shortener: редирект, статистика кликов, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- API Gateway: маршрутизация, аутентификация, rate limiting, логирование, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Высоконагруженный HTTP сервис с тысячами RPS: бенчмарки, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Real-time сервис: WebSocket или Server-Sent Events, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**Open Source Strategy:**

- Репозитории Go для вклада: teleport, docker, cockroachdb
- Issues по Go: теги good first issue

**📚 Ресурсы:** *The Go Programming Language* (Donovan & Kernighan), Go by Example, Go docs

---

#### 2. Docker и контейнеризация

**Время:** 50 часов

**Что изучать:**

- Контейнеры vs VM: overlay filesystem, union mount
- Docker architecture: daemon, client, registry
- Images: слои, Dockerfile инструкции (FROM, RUN, COPY, EXPOSE, CMD, ENTRYPOINT)
- Volumes: named volumes, bind mounts
- Networks: bridge, host, overlay, none
- Docker-compose: yaml, services, volumes, networks
- Multi-stage builds: оптимизация размера образа
- .dockerignore: исключение файлов
- Безопасность контейнеров: не запускать от root, read-only rootfs
- User namespace: изоляция UID/GID
- cgroups: ограничение ресурсов (CPU, memory)
- SELinux/AppArmor profiles для Docker
- Docker Bench Security: CIS Docker Benchmark

**Проекты:**

- Docker-compose для стека: nginx + app + DB, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Multi-stage образ Go приложения, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Утилита управления контейнерами, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- CI/CD pipeline с Docker, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT:**

- Docker-compose development environment: dev, test, prod профили, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** Docker docs, Dockerfile best practices, Docker Security

---

#### 3. Kubernetes Networking

**Время:** 60 часов

**Что изучать:**

- Kubernetes архитектура: control plane (API server, etcd, scheduler, controller manager), data plane (kubelet, kube-proxy, container runtime)
- Container Network Interface (CNI): CNI specification, container network namespace, veth pairs, CNI plugin execution flow
- CNI plugins comparison: Flannel (VXLAN/host-gw), Calico (BGP/VXLAN), Cilium (eBPF), Weave Net (mesh), Multus (multi-NIC)
- Pod networking: CNI plugins implementation, pod-to-pod communication, cross-node networking, hairpin mode
- Service types: ClusterIP (internal), NodePort (external via node), LoadBalancer (cloud LB), ExternalName (CNAME), headless (None)
- kube-proxy: iptables mode ( DNAT rules per Service), IPVS mode (hash tables, better performance), userspace mode (deprecated)
- DNS в k8s: CoreDNS architecture, service discovery (<svc>.<ns>.svc.cluster.local), headless services, ExternalName, custom DNS config
- Ingress controller: nginx ingress, Traefik, HAProxy, IngressClass, Ingress rules, path-based routing, host-based routing
- TLS termination: cert-manager, Let's Encrypt integration, wildcard certificates, mTLS between services
- NetworkPolicies: ingress/egress rules, pod selectors, namespace selectors, CIDR blocks, default deny implementation
- NetworkPolicy примеры: изоляция namespaces, allow specific ports, egress control, DNS egress allowance
- CNI и security: сетевая изоляция в k8s, microsegmentation, east-west traffic control
- eBPF в Kubernetes: Cilium CNI, kube-proxy replacement, transparent encryption, bandwidth management
- ServiceMesh: Istio/Linkerd concepts — sidecar proxy, mTLS, traffic management, observability
- CNI diagnostic: kubectl exec, tcpdump в pod, Wireshark для k8s, network policy debugging
- NetworkPolicy debugging: connectivity tests, DNS resolution issues, policy conflicts
- Multi-tenancy networking: namespace isolation, shared vs dedicated CNI, resource quotas
- Kubernetes network concepts: ServiceAccount, RBAC for network policies, admission controllers
- Container network performance: MTU considerations, encapsulation overhead (VXLAN vs direct routing)
- Dual-stack: IPv4/IPv6 networking in k8s, Service dual-stack, Pod dual-stack
- Network policies as code: OPA/Gatekeeper constraints, Kyverno policies, testing before apply

**Практика:**

- Настройка k8s кластера (k3s/minikube) с Flannel/Calico, проверка pod-to-pod connectivity через ping/nc, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- NetworkPolicies: default deny all → allow specific pods/ports → test connectivity, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Ingress с TLS termination: cert-manager + Let's Encrypt + nginx ingress, host/path routing, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Диагностика pod-to-pod connectivity: tcpdump в pod, nslookup, curl, network policy conflicts, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- kube-proxy comparison: iptables vs IPVS mode, performance test (service endpoint scaling), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Cilium CNI setup + Network Policies: L3/L4/L7 policies, Hubble observability, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- Kubernetes Network Security Lab: k3s/minikube + CNI + NetworkPolicies + monitoring + Ingress с TLS, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Multi-tier application с NetworkPolicy: frontend → backend → database isolation, egress restrictions, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Service mesh basics: Istio/Linkerd installation + mTLS + traffic management demo, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Kubernetes Network Security Blueprint: production-ready кластер с Cilium, NetworkPolicies, Ingress, monitoring, logging, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** Kubernetes docs (Networking), Cilium docs, Calico docs, *Kubernetes in Action* (Marko Lukša), *Kubernetes Networking* (Rosenbaum)

---

#### 4. Базы данных для сетевых приложений

**Время:** 40 часов

**Что изучать:**

- **PostgreSQL:**
  - Архитектура: процессы (postmaster, backend, checkpointer, WAL writer), табличное пространство
  - Типы данных: numeric, JSON/JSONB, arrays, hstore, range types, UUID
  - Запросы: SELECT/INSERT/UPDATE/DELETE, JOIN, GROUP BY, window functions, CTE (WITH)
  - Индексы: B-tree, Hash, GIN, GiST, BRIN, partial indexes, covering indexes, explain/analyze
  - Транзакции: ACID, isolation levels (READ COMMITTED, REPEATABLE READ, SERIALIZABLE), MVCC, deadlocks
  - Подключение: libpq, PQconnectdb, prepared statements, PgX (Go driver), database/sql
  - Репликация: streaming replication, WAL, read replicas, synchronous/asynchronous
  - Шардирование: partitioning (declarative partitioning), Citus — на уровне концепций

- **MySQL/MariaDB:**
  - Основные отличия от PostgreSQL: storage engines (InnoDB, MyISAM), locking model
  - Репликация: binlog, statement vs row-based, GTID

- **Redis:**
  - Типы данных: string, list, hash, set, sorted set, bitmap, HyperLogLog
  - Команды: GET/SET, LPUSH/LRANGE, HSET/HGET, SADD, ZADD, INCR, EXPIRE, TTL
  - Pub/Sub: subscribe/publish, channels, patterns
  - Ключи и сроки жизни: TTL, eviction policies (noeviction, allkeys-lru, etc.)
  - Персистентность: RDB snapshots, AOF, hybrid persistence

- **SQL injection:**
  - Основы: конкатенация строк, параметризованные запросы (prepared statements), ORM bind variables
  - Продвинутые: UNION-based, boolean-based blind, time-based blind, error-based, stacked queries
  - Защита: prepared statements, input validation, least-privilege DB accounts
  - Инструменты: sqlmap (detection, exploitation), Burp Suite repeater

- **Схема данных для logging:**
  - Таблицы событий безопасности: event_id, timestamp, source_ip, dest_ip, protocol, action
  - Индексирование по timestamp и IP, партиционирование по времени
  - Временные ряды: TimeScaleDB, best practices

- **Connection pooling:**
  - PgBouncer: транзакционный/сессионный режим, multi-host setup, конфигурация

- **Безопасность БД:**
  - Аутентификация: password, scram-sha-256, pg_hba.conf, SSL/TLS подключения
  - Авторизация: roles, grants, schema-level privileges, row-level security (RLS)
  - Шифрование: Transparent Data Encryption (TDE), column-level encryption, pgcrypto
  - Аудит: логирование запросов, pg_stat_statements, extension

**Практика:**

- Настройка PostgreSQL + создание схемы для security logging, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- SQL injection тест: параметризованные vs конкатенация, sqlmap на учебном приложении, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Репликация master-slave: настройка streaming replication, failover тест, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Redis pub/sub + cache layer: интеграция с Go приложением, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- PgBouncer: установка, конфигурация, тест connection pooling с несколькими клиентами, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- Логирование событий безопасности в PostgreSQL: схема, индексы, партиционирование, CRUD API, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Redis cache layer перед PostgreSQL: кэш, TTL, инвалидация, нагрузочное тестирование, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Простое API на Go с БД (PostgreSQL + prepared statements) + авторизация, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Secure SIEM-lite на PostgreSQL: лог-агрегатор + schema + индексы + API + базовые детекты SQL injection, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** PostgreSQL docs, *The Art of PostgreSQL*, SQL Injection Prevention (OWASP), Redis docs

---

#### 5. Производительность сетевых приложений

**Время:** 40 часов

**Что изучать:**

- **Бенчмаркинг:**
  - wrk: HTTP benchmark, threads/connections, latency percentiles
  - hey: простой HTTP load generator, запрос в секунду, обработка ошибок
  - ab (apache benchmark): традиционный, keep-alive, concurrency
  - k6: скриптовый нагрузочный тестировщик, SLO проверки, scriptable scenarios
  - Load testing best practices: реалистичные сценарии, как масштабировать, метрики p95/p99

- **Профилирование:**
  - pprof (Go): CPU profile, heap, goroutine, mutex; go tool pprof, web UI, flamegraph
  - perf (Linux): perf stat, perf record/report, hardware counters
  - flamegraph: Brendan Gregg, горизонт анализа, hot path
  - bpftrace/off-CPU: анализ задержек, блокировок

- **Многопоточность/async:**
  - epoll: edge/level triggered, EPOLLOUT/EPOLLIN, event loop дизайн
  - kqueue (BSD/macOS): аналог epoll
  - IOCP (Windows): Completion Ports, async I/O
  - io_uring: современная async I/O модель Linux, SQ/CQ rings, liburing

- **Zero-copy:**
  - sendfile: передача между fd без копирования через userspace
  - splice: стыковка двух fd, peformance
  - vmsplice: userspace page registration для избежания копий
  - io_uring: передача данных без syscall на каждый I/O

- **Buffering:**
  - Размер буферов: влияние на throughput/latency, буферизация на TCP/kernel/приложение уровне
  - Buffer pools: предварительное выделение, избежание аллокаций на горячем пути

- **Keepalive:**
  - TCP keepalive: net.ipv4.tcp_keepalive_*, серверные параметры
  - HTTP keep-alive: connection reuse, влияние на latency/throughput

- **HTTP/2 и HTTP/3 multiplexing:**
  - HTTP/2: stream multiplexing, HPACK, server push, влияние на latency
  - HTTP/3: QUIC поверх UDP, 0-RTT, connection migration
  - Сравнение производительности: HTTP/1.1 vs HTTP/2 vs HTTP/3 под нагрузкой

- **CDN и кэширование:**
  - Varnish: cache, ESI, grace mode, hit ratio
  - nginx proxy_cache: конфигурация, invalidation, microcaching

- **Балансировщики:**
  - HAProxy: L4/L7, алгоритмы (round-robin, leastconn, source), health checks, stickiness
  - nginx upstream: прокси, health checks, weighted load balancing

- **Rate limiting:**
  - Token bucket: допуск капасити, replenish rate
  - Leaky bucket: выходной буфер, backpressure
  - Реализация: nginx limit_req, HAProxy stick table, custom (Redis)

- **Backpressure:**
  - Механизмы предотвращения перегрузки: bounded queues, semaphores, demand-based flow control
  - Применение: gRPC, message brokers, HTTP/2 flow control

**Практика:**

- Бенчмарк HTTP сервера на C (epoll) vs Go vs Rust: wrk/k6, сравнение p50/p95/p99 и RPS, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Профилирование Go сервиса: pprof CPU/heap, flamegraph, найти hot path, оптимизировать, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Zero-copy тест: sendfile vs userspace read/write, сравнение throughput, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Rate limiter: реализация token bucket, тест допустимых/отвергнутых запросов, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- HAProxy настройка: L4 TCP + L7 HTTP, алгоритмы, health checks, graceful failover, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- Бенчмарк HTTP сервера на разных технологиях (C/C++/Go/Rust): метрики, отчет, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Профилирование и оптимизация горячего пути: найти bottleneck, устранить, измерить улучшение (before/after), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Rate limiter + backpressure: токенный ведро + bounded queue в сетевом сервисе, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Highload Network Service: эндпоинт с 100k RPS, профилирование и оптимизация горячего пути, сравнение с альтернативами, документация и бенчмарк, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** *Systems Performance* (Brendan Gregg), k6 docs, wrk docs, HAProxy docs, io_uring docs

---

#### 6. Мониторинг сети: Prometheus + Grafana

**Время:** 40 часов

**Что изучать:**

- **Prometheus:**
  - Архитектура: pull-модель, retention, TSDB, scrape interval, storage/compact
  - Пуш-паттерн: PushGateway (избегать, если можно pull), когда использовать
  - AlertManager: rules, routes, receivers (email, slack, webhook), grouping, inhibition
  - node_exporter: CPU (node_cpu_seconds_total), RAM (node_memory_*), disk (node_disk_*), network (node_network_*)
  - blackbox_exporter: протоколы (icmp, http, tcp, dns), probes, успех/провал
  - snmp_exporter: мониторинг свитчей/роутеров через SNMP, MIB walks, OIDs
  - ПромQL: векторные/скалярные метрики, selectors, aggregation (sum, avg, max), rate/irate, histogram_quantile
  - Service Discovery: static config, file_sd, dns_sd, kubernetes_sd, ec2_sd
  - Экспортеры: стандартные (node, blackbox, snmp), кастомные (клиентская библиотека prometheus/client_golang)
  - Конфигурация: prometheus.yml, scrape_configs, relabeling, honor_labels

- **Grafana:**
  - Дашборды: панели (graph, stat, gauge, table), переменные, аннотации
  - Источники данных: Prometheus, Loki, Elasticsearch, InfluxDB
  - Alerting: правила, уведомления, связь с Prometheus AlertManager
  - Визуализация: heatmap, histogram, гео-мапы, transformations
  - Права пользователей, folders, provisioning (as code)

- **Loki (опционально):**
  - Агрегация логов, LogQL, связь с Prometheus метриками (correlations)

- **Мониторинг сетевого оборудования:**
  - SNMP: v1/v2c/v3, community strings, OIDs, MIBs
  - Интерфейсы: speed, errors, drops, utilization
  - Devices: MikroTik, Cisco, generic via snmp_exporter

**Практика:**

- Настройка node_exporter на сервере: метрики CPU/RAM/disk/network, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- blackbox_exporter для HTTP/TCP/ICMP/DNS проверок, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Grafana dashboard для сети: панели, переменные, alerts, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- ПромQL запросы: rate, histogram_quantile, topk, агрегации — практические задачи, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- AlertManager: правила, группы, receiver для email/slack/webhook, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- snmp_exporter: мониторинг сетевого устройства (MikroTik/virtual), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- Network monitoring stack: Prometheus + Grafana + node/blackbox/snmp exporters в docker-compose, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Dashboard для сетевых метрик: ingress/egress, latency, ошибки, доступность — с переменными и алертами, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Alerting на недоступные хосты и высокую нагрузку: правила, receiver, эскалация, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Full Network Observability Stack: Prometheus + Grafana + exporters + Loki + AlertManager + dashboards + SLO метрики, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** Prometheus docs, Grafana docs, node_exporter docs, blackbox_exporter docs, Prometheus Up & Running

---

## Семестр 6 - Продвинутые темы (Сентябрь - Декабрь)

**Время:** ~320 часов

**Выбрать дорожку A или B:**

---

### Общие разделы для обеих дорожек

**Что изучать:**

- Data exfiltration: DNS tunneling (TXT/NULL/long-subdomain каналы), ICMP tunneling (echo payload C2), HTTP(S) exfil через легитимные домены и cloud storage abuse.
- Covering tracks: очистка и подмена журналов (Windows Event Logs, bash history, PowerShell logs), timestomping (MACE), tampering следов в SIEM и артефактов EDR.

**Практика:**

- HackTheBox Pro Lab: Offshore, Dripping, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- TryHackMe: AD компьютеры, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- VulnHub: AD-style VMs, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**📚 Ресурсы:** Rubeus docs, PowerShell Empire docs, *Attacking Microsoft Active Directory*, *Red Team Field Manual*

---

#### 1. Облачные сети и безопасность

**Время:** 50 часов

**Что изучать:**

- AWS networking: VPC, public/private subnets, route tables, IGW, NAT Gateway, egress-only IGW, multi-AZ сегментация и blast-radius дизайн.
- AWS security groups: stateful filtering, inbound/outbound матрицы, least-privilege для app tiers, rule shadowing и troubleshooting deny-by-default моделей.
- AWS NACLs: stateless subnet firewall, порядок правил, ephemeral ports, сценарии блокировки lateral movement между подсетями.
- VPC Peering: non-transitive маршрутизация, ограничения перекрытия CIDR, DNS resolution across VPC и типичные ошибки асимметричного трафика.
- Transit Gateway: hub-and-spoke топологии, route domains, segmentation между environments (prod/dev), centralized egress/inspection patterns.
- AWS security controls: IAM roles и trust policies, SCP guardrails, Security Hub findings aggregation, Access Analyzer для external exposure.
- Azure networking: VNet, subnet delegation, NSG flow rules, UDR, Azure Firewall policies, приватные endpoints и service endpoints.
- Cloud security model: shared responsibility по IaaS/PaaS/SaaS, misconfiguration ownership, compensating controls и governance процесс.
- Serverless: Lambda execution chaining (EventBridge->S3->EC2), Function URLs RCE, IAM role abuse patterns; Cloud Functions trigger hijacking, Cloud Run container escapes, cross-service privilege escalation.
- Container orchestration: ECS/EKS/AKS network policies, pod-to-pod segmentation, metadata endpoint hardening, image provenance и runtime controls.
- Cloud security tools: Prowler/ScoutSuite/CloudSploit для baseline аудита, triage critical findings, false-positive tuning и remediation tracking.
- Hardening: CIS Benchmarks для AWS/Azure/GCP, baseline templates, drift detection через IaC и регулярный compliance scanning.
- VPC Flow Logs: анализ east-west и north-south трафика, выявление beaconing/C2, deny events, enrichment через Athena/Elastic.
- PrivateLink/Private Endpoint: закрытый доступ к PaaS-сервисам без публичного интернета, контроль DNS split-horizon и data exfil ограничения.
- Hybrid connectivity: Site-to-Site VPN/Direct Connect/ExpressRoute, route leakage риски, BGP policy, отказоустойчивость каналов.

**Проекты:**

- AWS VPC lab: public/private subnets, centralized egress, flow logs и сегментация трафика между tiers.
- Security groups + NACL policy matrix: least-privilege правила, validation via reachability tests и deny-case проверки.
- AWS Site-to-Site VPN (учебный): туннель с BGP/route failover, monitoring состояния и troubleshooting MTU issues.
- EKS networking hardening: NetworkPolicy, pod identity controls, egress restrictions и audit логирование сетевых событий.

**📚 Ресурсы:** AWS docs (Networking), AWS Well-Architected Framework, Azure docs

---

#### 2. Web Application Firewall (CloudFlare и др.)

**Время:** 40 часов

**Что изучать:**

- WAF концепции: signature-based, anomaly-based, behavioral detection, positive security model и virtual patching для zero-day mitigation.
- CloudFlare WAF: managed rules lifecycle, sensitivity tuning, exception handling, false-positive triage и per-path policy.
- Custom rules: expression language, geo/IP/ASN controls, bot-score filters, adaptive rate limiting по URI/user/session.
- CloudFlare Workers: edge validation, JWT pre-auth, header normalization, anti-abuse middleware и безопасные egress-запросы.
- Bot management: JS challenge, CAPTCHA, device/browser fingerprinting, sequence rules и API abuse detection.
- DDoS protection: L3/L4/L7 mitigation pipelines, always-on mode, autoscale thresholds, upstream coordination и origin shielding.
- TLS/SSL: Full (strict), certificate lifecycle, OCSP stapling, HSTS, TLS versions/ciphers hardening, mTLS для admin/API.
- Access policy: MFA, identity-aware access, IP/device posture checks, short-lived access tokens для чувствительных маршрутов.
- Log fields: RayID, colo, ASN, bot score, WAF action, matched rule ID; нормализация полей для SIEM корреляции.
- Challenge responses: managed challenge стратегия, bypass для trusted clients, user-friction баланс и наблюдаемость конверсии.
- OWASP Top 10 mapping: соответствие managed/custom правил на SQLi, XSS, SSRF, LFI/RFI, command injection, auth bypass.
- SIEM integration: Logpush в S3/Kafka/BigQuery, real-time alerting, корреляция с origin logs и SOAR playbooks.

**Практика:**

- Настройка CloudFlare WAF rules: managed + custom rulesets, staged rollout и проверка false-positive на легитимном трафике.
- Блокировка по GeoIP/ASN: risk-based policies, исключения для trusted traffic и мониторинг bypass попыток.
- Rate limiting для API: per-token/per-IP thresholds, burst control и graceful responses without breaking clients.
- Custom expressions для SQLi/XSS: tailored rules по endpoint behavior, validation на test payloads и tuning noise.

**Проекты:**

- Полная настройка WAF для веб-приложения, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Dashboard с графиками атак, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Настройка боевого режима, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** CloudFlare docs, OWASP Top 10, ModSecurity docs

---

#### 3. Беспроводные сети и безопасность

**Время:** 30 часов

**Что изучать:**

- Wi-Fi стандарты: 802.11 a/b/g/n/ac/ax/be, PHY differences, practical throughput vs theoretical rate, roaming behavior.
- Частоты: 2.4/5/6 GHz planning, interference sources, client compatibility, signal propagation в офисных и публичных средах.
- Каналы: overlapping/co-channel interference, channel width (20/40/80/160), DFS ограничения и выбор каналов под нагрузку.
- Режимы: managed/monitor/master/ad-hoc/mesh, use-cases для аудита, атак и защиты беспроводного сегмента.
- WPA2 security: 4-way handshake, PTK/GTK derivation, PMKID leakage, downgrade риски и hardening AP клиентов.
- WPA3 security: SAE/Dragonfly internals, anti-offline-dictionary преимущества, transition mode pitfalls и совместимость legacy клиентов.
- WEP legacy risks: IV reuse, RC4 weaknesses, быстрый key recovery и причины полного запрета в enterprise среде.
- MAC filtering bypass: spoofing techniques, OUI impersonation, необходимость 802.1X вместо MAC-based допусков.
- Rogue AP/Evil Twin: captive portal phishing, deauth-assisted attacks, detection через WIDS/WIPS и RF fingerprinting.
- Wi-Fi jamming: RF noise sources, detection baselines, incident response playbook и правовые ограничения.
- DFS operations: radar event handling, channel switch announcements, влияние на стабильность voice/video трафика.
- 802.11n/ac/ax features: MIMO, MU-MIMO, beamforming, OFDMA, BSS coloring и их влияние на безопасность и мониторинг.

**Инструменты:**

- airmon-ng + Wireshark: monitor mode capture, frame-level analysis и фильтрация management/control/data трафика.
- bully/reaver: controlled WPS assessment, lockout detection и защита от brute-force сценариев в лаборатории.
- hcxtools/hcxdumptool: PMKID/handshake capture workflow, quality checks и безопасное хранение хешей.
- cowpatty/hashcat: password recovery workflow, rules/masks optimization и оценка стойкости passphrase политики.
- wpa_supplicant: secure client profiles, EAP method tuning и certificate validation practices, с акцентом на реальные команды, ограничения инструмента и использование в troubleshooting/incident response сценариях.
- hostapd: secure AP setup, WPA2/WPA3 configs, rogue AP detection hooks и логирование событий, с акцентом на реальные команды, ограничения инструмента и использование в troubleshooting/incident response сценариях.

**Проекты:**

- Создание Rogue AP с логированием credentials, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Анализ handshake, брутфорс, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Detecting rogue APs в сети, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** aircrack-ng docs, *802.11 Wireless Networks*

---

#### 4. Анализ вредоносного ПО

**Время:** 50 часов

**Что изучать:**

- Статический анализ: strings/entropy/sections, PE imports/exports, suspicious APIs, resource anomalies, signed-binary trust checks.
- Динамический анализ: API hooking, process injection traces, registry/file activity, sandbox evasion indicators, network behavior profiling.
- Форматы файлов: PE/ELF/Mach-O internals, loaders, section flags, entry point logic и anti-analysis трюки по платформам.
- Упаковщики: UPX/custom packers detection, unpacking workflows, memory dumping, OEP recovery и повторная реконструкция образца.
- Обфускация: XOR/base64/custom crypto layers, string decoding pipelines, control-flow flattening и anti-disassembly patterns.
- Сетевая активность malware: C2 protocol families (HTTP/DNS/TCP custom), jittered beaconing, domain generation algorithms, exfil channels.
- Sandbox pipeline: Cuckoo/CAPE/Any.Run setup, IOC extraction, ATT&CK mapping и автоматизация triage отчётов.
- YARA engineering: устойчивые сигнатуры, atom quality, rule performance, false-positive suppression и CI-тестирование правил.
- Декомпиляция: Ghidra/IDA/Binary Ninja/GDB workflows, function recovery, data-flow analysis, API call graph и behavior reconstruction.

**Практика:**

- Анализ образцов из MalwareBazaar: triage, static/dynamic behavior mapping и extraction IOC/TTP в структурированном виде.
- Создание YARA правил: high-signal signatures, anti-evasion refinements и performance tests against clean corpus.
- Отчёт по анализу: technical narrative, ATT&CK mapping, confidence level и practical detection/remediation recommendations.

**📚 Ресурсы:** *Practical Malware Analysis*, *Malware Analyst's Cookbook*, Ghidra docs

---

#### 5. Reverse Engineering сетевых протоколов

**Время:** 40 часов

**Что изучать:**

- Протокол reverse workflow: pcap carving, session reconstruction, message boundary inference, state machine extraction.
- Binary protocol format: packet framing, field types, endianness, checksum logic, versioning и backward compatibility patterns.
- Шифрование в протоколах: handshake fingerprinting, key exchange clues, TLS wrapping detection и слабые custom crypto конструкции.
- Фаззинг протоколов: Sulley/Peach/Boofuzz сценарии, mutation vs generation подходы, coverage feedback и crash triage.
- Инструменты: Wireshark/tshark/NetworkMiner/Scapy, custom dissectors, Lua plugins и automation pipeline для массового анализа.
- Реверс proprietary протоколов: игровые клиенты, IoT, мессенджеры; correlation network trace \<-> binary execution paths.
- IDA/Ghidra integration: function-to-packet mapping, parser code recovery, command handlers и auth/crypto path tracing.
- Packet engineering: ручная сборка/реплей пакетов, session hijack simulation, protocol tampering и validation bypass testing.

**Проекты:**

- Реверс неизвестного протокола из pcap: inference packet structure/state machine и документирование формата сообщений.
- Фаззер для бинарного протокола: generation + mutation modes, crash triage и минимизация repro кейсов.
- Reconstruction диссектора в Wireshark: Lua/C parser implementation, field decoding и validation на реальных traces.

**📚 Ресурсы:** *Practical Reverse Engineering*, *The IDA Pro Book*

---

#### 6. SD-WAN и SASE

**Время:** 30 часов

**Что изучать:**

- SD-WAN architecture: control/data plane split, edge nodes, orchestrator, policy-based routing, overlay tunnels и SLA-aware path selection.
- SD-WAN platforms: Cisco Viptela, VMware VeloCloud, Versa; differences в policy model, observability, security stack и automation API.
- SASE model: convergence SD-WAN + SSE (SWG/CASB/ZTNA/FWaaS), PoP design, latency trade-offs и service chaining.
- WAN optimization: deduplication, compression, caching, TCP acceleration, QoS class mapping для latency-sensitive приложений.
- SD-WAN security: встроенный NGFW/IPS/URL filtering, segmentation VRF, secure internet breakout и branch hardening baseline.
- Zero Trust integration: identity-aware policy, device posture signals, continuous verification, least-privilege access across branches.
- Multi-cloud connectivity: direct cloud connect/on-ramp patterns, route control, inter-region resilience и egress governance.
- Controller operations: centralized policy rollout, config drift detection, staged deployments, rollback strategy и audit trail.

**На уровне концепций:** принципы работы, отличия от традиционного WAN

**📚 Ресурсы:** Viptela docs, Versa docs, Gartner reports

---

#### 7. VPN (углублённо)

**Время:** 40 часов

**Что изучать:**

- IPSec fundamentals: IKEv1/v2 exchange, ISAKMP states, ESP/AH transport vs tunnel mode, PFS, rekey и SA lifetime tuning.
- IPSec deployment: site-to-site и remote-access топологии, policy-based vs route-based tunnels, NAT-T и failover design.
- OpenVPN internals: TLS handshake, TUN/TAP behavior, certificate pinning, cipher suites, auth plugins и client profile hygiene.
- WireGuard operations: Curve25519 + ChaCha20-Poly1305, key rotation, roaming peers, hub-and-spoke/full-mesh topology decisions.
- SSL VPN ecosystem: AnyConnect/Pulse/OpenVPN post-auth controls, posture checks, split/full tunnel policy и session security.
- VPN servers: strongSwan/OpenVPN/WireGuard hardening, HA setup, logging strategy, config backup и secret management.
- VPN clients: certificate lifecycle, OCSP/CRL checks, mobile endpoint constraints, secure bootstrap and revocation workflows.
- Split tunneling risks: bypass of monitoring, DNS leakage, local LAN pivoting; compensating controls через ACL, ZTNA, DNS policy.
- MFA for VPN: TOTP/WebAuthn/Push integration, step-up auth для admin access, fallback risks и account lockout strategy.
- VPN troubleshooting: IKE negotiation debug, MTU/MSS issues, asymmetric routing, dead peer detection и packet capture methodology.
- Hairpin NAT scenarios: remote users to on-prem services via same gateway, policy routing and NAT reflection pitfalls.
- IPv6 over VPN: dual-stack routing, RA/DHCPv6 behavior, leak prevention, firewall parity между IPv4 и IPv6 правилами.

**Практика:**

- Настройка WireGuard сервера/клиентов: key management, role-based access, kill-switch policies и logging.
- IPSec VPN между двумя сетями: resilient tunnel with rekey/failover testing и validation of encrypted routing paths.
- OpenVPN с OTP/YubiKey: MFA integration, certificate revocation workflow и hardened auth policy.

**Проекты:**

- WireGuard VPNaaS для малого офиса, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- IPSec VPN с two-factor auth, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** WireGuard docs, strongSwan docs, OpenVPN docs

---

#### 8. eBPF для сетевой безопасности и мониторинга

**Время:** 50 часов

**Что изучать:**

- eBPF fundamentals: program types, attach points, verifier constraints, helper functions, map lifecycle и безопасность загрузки.
- bpftrace workflows: kernel/user probes, histogram/latency scripts, safe observability in production without full packet capture.
- XDP use-cases: early drop/redirect/pass decisions, DDoS mitigation pipelines, CPU budget optimization и NIC offload awareness.
- tc with eBPF: ingress/egress policy enforcement, traffic shaping hooks, mark/classify flows и chaining с iptables/nftables.
- eBPF in C: clang/llvm toolchain, libbpf skeletons, userspace control plane, pinned maps и event streaming.
- Tracepoints: TCP retransmits/drops/error hotspots, correlation with kernel network stack regressions и SLO impact.
- kprobe/kretprobe: kernel function instrumentation, syscall-level network audit, overhead control и safe rollout.
- uprobes: user-space TLS/socket libraries tracing, app-level visibility без изменения исходного кода.
- Network observability: latency, throughput, retransmissions, top talkers, queue depth, drop reasons per interface/pod/node.
- Security analytics: anomaly detection via connection patterns, suspicious egress, port scan fingerprints и lateral movement signals.
- bpftrace script engineering: reusable script catalog, parameterization, CI linting и controlled execution in incident response.
- CO-RE portability: BTF reliance, version-agnostic builds, compatibility testing across kernel matrix and distro variants.

**Практика:**

- Написание eBPF программы на C: attach points, map interactions, verifier constraints и userspace exporter.
- bpftrace скрипт для TCP мониторинга: retransmits/latency histograms, interface breakdown и low-overhead execution.
- XDP фильтр пакетов: stateless + state-aware rules, benchmark under traffic bursts и rollback safety controls.

**Проекты:**

- Мониторинг TCP drops в ядре, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- XDP firewall для DDoS protection, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Latency мониторинг через bpftrace, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** eBPF.io, bpftrace docs, Linux kernel bpf-docs

---

### Дорожка A: Red Team (Атакующий пентест)

#### 1. Атаки и эксплуатация сетей

**Время:** 70 часов

**Что изучать:**

- **ARP spoofing/poisoning:**
  - ettercap: unified sniffing, ARP poisoning, DNS spoofing plugin, Wireshark integration
  - bettercap: caplet-based automation, ARP spoofer, DNS spoofer, net.probe, net.sniff
  - arpspoof (dsniff suite): simple ARP spoofing tool
  - Детекция: ARP table monitoring, static ARP entries, DAI (Dynamic ARP Inspection), arpwatch

- **IP spoofing:** проверка на уровне edge router (BCP38/uRPF), source address validation

- **DNS атаки:**
  - DNS cache poisoning: Kaminsky attack (birthday paradox for txn ID), source port randomization bypass
  - DNS spoofing: ложные ответы, lcache poisoning, tools (dnsspoof,Responder)
  - DNS tunneling: data exfil через TXT/CNAME/NULL records, iodine, dnscat2
  - DNS rebinding: TLD tricks, DNS-o-Matic, CSRF via DNS rebinding
  - DNSSEC bypass и attack vectors

- **Man-in-the-Middle:**
  - ettercap: ARP + DNS + HTTPS mitm, plugin system, logging
  - BetterCAP: 2.0 architecture, caplet scripting, BLE mitm, WiFi mitm
  - sslstrip: HTTPS→HTTP downgrade, HSTS bypass ( sslstrip+)
  - mitmproxy: intercepting proxy, scripting, transparent mode
  - WiFi mitm: Evil Twin, Karma attack, deauth + capture handshake

- **VLAN атаки:**
  - Switch spoofing: DTP negotiation, VLAN hopping
  - Double tagging: 802.1Q tag stacking, native VLAN exploitation
  - Mitigation: disable DTP, native VLAN 999, explicit VLAN assignment

- **VoIP атаки:**
  - RTP hijacking: media stream interception
  - SIP invite flooding / registration hijacking
  - toll fraud: unauthorized call routing

- **SMB Relay / NTLM Relay:**
  - NTLM Relay: Responder + ntlmrelayx, Targets: LDAP, HTTP, SMB, WPAD
  - Pass-the-Hash: overpass-the-hash (Kerberos), hash capture via Responder
  - Mitigation: SMB signing, EPA (Extended Protection for Authentication), LDAP signing

- **Wireless атаки:**
  - Monitor mode: airmon-ng, wireless interface configuration
  - Deauth attack: aireplay-ng, deauth frames,pmk capture
  - Handshake capture: airodump-ng, 4-way handshake, PMKID (clientless)
  - WPA2 cracking: aircrack-ng, wordlist attack, rule-based, mask attack
  - WPA3 considerations: SAE, Dragonfly, downgrade attacks (concept)
  - Rogue AP: hostapd, evil twin, credential harvesting, captive portal phishing
  - WiFi Pineapple: rogue AP platform, karma attack, logging
  - Bluetooth attacks: Bluesnarfing, Bluejacking, BLE MITM (concept)

- **Rogue services:**
  - Rogue DHCP server: dhcpstarve, yersinia, unauthorized DHCP responses
  - Rogue DNS: dnsmasq redirect, DNS spoofing to attacker-controlled resolver
  - Rogue LDAP: NTLM relay to LDAP, certificate abuse
  - LLMNR/NBT-NS poisoning: Responder, credential capture

- **SSL stripping и downgrade attacks:**
  - sslstrip / sslstrip+: HTTPS→HTTP downgrade
  - HSTS bypass: subdomain attacks, certificate transparency logs
  - TLS downgrade attacks: POODLE, DROWN (concept), FREAK
  - Certificate substitution: fraudulent certificates, CA compromise scenarios

- **Network pivoting:**
  - SSH port forwarding: -L (local), -R (remote), -D (dynamic SOCKS proxy)
  - Chisel: TCP/UDP tunnel over HTTP, client-server architecture
  - Proxychains: routing traffic through SOCKS/HTTP proxy, DNS resolution through proxy
  - sshuttle: VPN-like tunnel through SSH, transparent proxy
  - socat: port forwarding, relays, complex socket operations
  - ligolo-ng: tunneling for pentesters, proxy setup

**Практика:**

- ARP spoofing lab:ettercap mitm → HTTP credential capture → detection via ARP monitoring → DAI mitigation, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- DNS spoofing + cache poisoning:ettercap DNS spoof → validation → defense (DNSSEC, DNS over HTTPS), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- NTLM relay chain: Responder (capture) → ntlmrelayx (relay to LDAP/SMB) → access, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- WiFi attack lab: monitor mode → deauth → handshake capture → crack (isolated lab!), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- SSL stripping demo: mitmproxy setup → HTTPS downgrade → credential capture → HSTS bypass discussion, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Pivoting chain: compromised host → chisel tunnel → access internal network → lateral movement, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Rogue DHCP + DNS: unauthorized DHCP server → DNS redirect → credential capture, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Инструменты:** bettercap, ettercap, arpspoof, Responder, ntlmrelayx, sslstrip, aircrack-ng suite, hostapd, dnsspoof, mitmproxy, chisel, proxychains, ligolo-ng

**Проекты:**

- Full MITM toolkit: ARP + DNS + SSL stripping + credential capture → comprehensive attack report, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Wireless attack lab documentation: step-by-step guide для WiFi атак с mitigation recommendations, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Network pivot playbook: 5 different pivoting techniques с comparison, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** *The Practice of Penetration Testing*, *Network Security Essentials* (Kaufman), *Hacking Exposed* series, bettercap docs, Responder docs

---

#### 2. Продвинутый пентест и Red Team

**Время:** 80 часов

**Что изучать:**

- **Active Directory атаки:**
  - Kerberoasting: SPN requesting, TGS offline cracking, hashcat modes
  - AS-REP Roasting: pre-auth disabled accounts, AS-REP extraction, offline cracking
  - DCSync: mimikatz dcsync, replication protocol abuse, credential extraction
  - Pass-the-Hash: NTLM hash use, crackmapexec, secretsdump
  - Pass-the-Ticket: Kerberos ticket reuse, Rubeus, ticket extraction
  - Overpass-the-Hash: NTLM hash → Kerberos ticket, Kerberos as NTLM relay
  - Golden Ticket: KRBTGT hash, domain compromise, undetectable persistence
  - Silver Ticket: service-specific hash, service impersonation
  - Diamond Ticket: legitimate TGT modification, stealth technique
  - Unconstrained/Constrained/Resource-Based Constrained Delegation abuse
  - AdminSDHolder persistence, Skeleton Key, DSRM backdoor
  - GPO abuse: Gpp-triage, GPP password, scheduled tasks via GPO
  - PrintSpooler/SpoolSample: coercing authentication, printer bug
  - ADCS attacks: ESC1-ESC8 (Certified Pre-Owned), certificate abuse
  - Shadow Credentials: Key Credential Link, msDS-KeyCredentialLink manipulation
  - ACL abuse: WriteProperty, WriteOwner, GenericAll, DCSync pathing

- **Lateral movement:**
  - WinRM: evil-winrm, wsman, PowerShell remoting
  - WMI: wmiexec.py, wmic, remote process creation
  - PsExec: sysinternals, psexec.py (Impacket), service creation
  - DCOM: mmc20.application, shellwindows, exectMethod
  - SMB: smbexec.py, psexec.py, atexec.py
  - Overpass-the-Hash: Kerberos ticket from NTLM hash
  - Pass-the-Certificate: certificate authentication, Schannel

- **Privilege escalation:**
  - Kernel exploits: Windows (PrintNightmare, HiverSleep, PetitPotam), Linux (DirtyPipe, DirtyCow)
  - Misconfigured permissions: unquoted service paths, weak folder permissions, DLL hijacking
  - Token impersonation: SeImpersonatePrivilege, potato attacks (Hot/Juicy/Rotten)
  - Group membership: Domain Admins, Enterprise Admins, built-in groups
  - Credential hunting: SAM database, LSA secrets, group Managed Service Accounts

- **Persistence:**
  - Userland: Registry Run keys, Scheduled Tasks, Startup folder, Services
  - AdminSDHolder: ACL inheritance, protection bypass
  - Skeleton Key: password filtering on DC, patching authentication
  - DSRM: Directory Services Restore Mode backdoor
  - Group Policy persistence: GPO modification, scheduled tasks via GPO
  - Golden/Diamond Ticket: Kerberos-based persistence
  - Certificate persistence: ADCS enrollment, auto-enrollment

- **PowerShell атаки:**
  - PowerShell Empire: post-exploitation framework, listeners, agents
  - PowerShell: AMSI bypass (concept), logging evasion, script block logging
  - Cobalt Strike: beacon, stagers, malleable C2 profiles
  - C2 frameworks: Mythic, Covenant, Sliver, Havoc

- **Pivot и tunneling:**
  - proxychains: SOCKS/HTTP proxy routing, DNS through proxy
  - socks proxy: ssh -D, chisel, ligolo-ng
  - Port forwarding: ssh -L/-R, socat, rinetd
  - Pivoting: from compromised host to internal network segments
  - DNS tunneling: iodine, dnscat2 — command channel over DNS
  - ICMP tunneling: icmpsh, ptunnel — data over ICMP

- **Data exfiltration:**
  - DNS tunneling: TXT/CNAME records, data encoding, chunking
  - ICMP tunneling: payload in echo request/reply
  - HTTPS: encrypted exfil over legitimate connections
  - Cloud storage abuse: upload to legit cloud services
  - Steganography: image/file embedding

- **Covering tracks:**
  - Windows Event Logs: clearing (wevtutil), specific log manipulation
  - Bash history: HISTFILE manipulation,shred
  - Timestomping: timestomp, CFF Explorer
  - File deletion: secure delete (sdelete), unlink, wiped
  - Log tampering: log rotation abuse, log injection

- **Living Off The Land (LOLBAS):**
  - Windows LOLBAS: certutil, mshta, rundll32, regsvr32, wmic, bitsadmin, powershell
  - Linux LOLBins: curl/wget, python, perl, nc, base64
  - Signed binary proxy execution: abuse of legitimate tools

**Практика:**

- HackTheBox Pro Lab: Offshore, Dante — full attack chain с writeup, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- TryHackMe: Active Directory rooms (Kerberos, Lateral Movement, Persistence), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- VulnHub: AD-style VMs (DC + member servers), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Mimikatz deep dive: 10 different credential extraction techniques, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- PowerShell Empire attack: listener + agent + post-exploitation chain, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Impacket toolkit: 10 different scripts (secretsdump, getTGT, wmiexec, smbexec, etc.) practical usage, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Cobalt Strike basics: beacon deployment, lateral movement, data collection (educational lab only), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Инструменты:** Rubeus, Mimikatz, PowerView, BloodHound/SharpHound, Impacket suite, CrackMapExec/NetExec, Evil-WinRM, CrackMapExec, Cobalt Strike (educational), Mythic, Sliver

**📚 Ресурсы:** Rubeus docs, PowerShell Empire docs, *Attacking Microsoft Active Directory* (HackTricks), *Red Team Field Manual*, *Certified Pre-Owned* (SpecterOps), *Hacking Active Directory* (IppSec)

---

#### 3. Custom Malware Development (40 часов) 

Red Team должен писать свой имплант под конкретную задачу.

**Что изучать:**

- **Архитектура импланта:**

  - Коммуникация: HTTP/S, DNS, ICMP (C2 каналы)
  - Криптография: шифрование трафика (AES-GCM, ChaCha20)
  - Beaconing: jitter, sleep, kill date

- **Обход защиты на файловой системе:**

  - Reflective DLL injection (загрузка DLL без выхода на диск)
  - Execute from memory (RunPE, Process Hollowing)
  - Fileless techniques: WMI, Registry, scheduled tasks

- **Persistence (закрепление в системе):**

  - Userland: Registry Run keys, Scheduled Tasks, Startup folder
  - Kernel: Bootkits (только обзорно, сложно)
  - Firmware: UEFI/BIOS persistence (экзотика)

- **Data exfiltration:**

  - DNS tunneling (dnscat2)
  - ICMP tunneling (icmpsh)
  - HTTPS with random jitter

**Практика на Rust/C++:**

```rust
// Пример импланта на Rust (безопасный и быстрый)
use reqwest;
use std::thread;
use std::time::Duration;

fn beacon(c2_url: &str) {
    loop {
        let response = reqwest::blocking::get(c2_url);
        // Выполнить команду, зашифровать результат
        thread::sleep(Duration::from_secs(60));
    }
}
```

**Проект:**

> Custom C2 Implant на Rust: HTTPS Beacon + AES шифрование + загрузка команд + выполнение PowerShell + report результатов

---

#### 4. Phishing и Watering Hole (40 часов)

Социалочка — самый эффективный вектор в Red Team.

**Время:** 40 часов

**Что изучать:**

- **Phishing кампании:**
  - GOPhish: запуск своей платформы, campaigns, landing pages, sending profiles, tracking (open/click/credential), групповая рассылка
  - Evilginx2: 2FA фишинг (проксирование реального сайта), session cookie theft, reverse proxy, subdomain setup
  - Modlishka: 2FA bypass, reverse proxy, session hijacking
  - Email spoofing: SPF/DKIM/DMARC обход, From header manipulation, spoofing detection mitigations
  - Payload delivery: HTML smuggling, multi-stage, document-based, URL-only

- **Watering Hole:**
  - Взлом легитимного сайта, который посещают сотрудники (target-based)
  - Drive-by download через уязвимости браузера/плагинов
  - Compromise assessment: как определить посещаемость и подобрать вектор

- **Макросы в документах:**
  - VBA макросы с обходом защиты: VBA Stomping, P-Code remnants, obfuscation
  - Word/Excel с отключённым макросом, но с внешней ссылкой (DDE, OLE)
  - Excel 4.0 macros (старые, но детектятся хуже)
  - OLE objects, DDE (Dynamic Data Exchange), RTF exploitation

- **Прикрепление файлов:**
  - ISO, IMG, VHD (Windows может монтировать — обход Mark-of-the-Web)
  - LNK (ярлыки) + скрытые команды (PowerShell one-liners)
  - CHM (Windows Help) + исполнение кода
  - MSI packages, script files (WSF, VBS, JS)

- **Инфраструктура фишинга:**
  - Redirector topology: email → link → redirector → phishing server (domain hygiene)
  - Домены: lookalike domains, typo-squatting, matching SSL certs, headers
  - Email infrastructure: SMTP relays, DKIM/SPF alignment, sending reputation, warm-up
  - Reporting and metrics: open rate, click rate, credential capture rate, conversion analysis

- **Аналитика:**
  - Метрики: sent, delivered, opened, clicked, submitted credentials
  - A/B testing: теми письма, адреса, время отправки, плотность ссылок
  - ROI-анализ: как оценить эффективность кампании

- **OPSEC и легальность:**
  - Только санкционированные симуляции, rules of engagement, в рамках договорённостей
  - Не использовать ресурсы реальных пользователей, изолированная тестовая среда
  - Очистка после теста: удалить письма, отозвать доступ, собрать evidence

**Практика:**

- Развернуть GOPhish, создать кампанию против собственной тестовой среды, измерить open/click rate, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Создать фишинговую landing page (клон входной страницы почты) и отследить отправку кредов, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Развернуть Evilginx2 в легальном lab режиме против собственного приложения, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Создать email с обходом anti-spoofing (SPF/DKIM) в тестовой среде, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Создать документ с макросом и payload'ом (в lab), протестировать детекты AV/EDR, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- "2FA Phishing Framework": прокси на Python, который редиректит на легитимный сайт, но крадёт 2FA токен (lab), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Полная фишинговая кампания против собственной lab-инфраструктуры: инфраструктура, отправка, измерение метрик, отчёт, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Enterprise Phishing Simulation Platform: landing pages + metrics + lessons learned + reporting, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** GOPhish docs, Evilginx2 docs, Microsoft anti-phishing guidance, OWASP Phishing resources

---

#### 5. Physical и Wireless Red Team (40 часов)

Пентест WiFi и физических помещений.

**Время:** 40 часов

**Что изучать:**

- **Physical Red Team:**
  - BadUSB: Rubber Ducky (DuckyScript, payloads), Bash Bunny, Flipper Zero (BadUSB mode)
  - Rogue Device Drop: оставить флешку/USB-питание с backdoor в холле (HID attack, ethernet drop)
  - Lockpicking: базовые навыки (pick, rake, bump key) — для проникновения в серверные
  - RFID access control: Proxmark3, low-frequency (HID/EM4100) клоны, high-frequency (MIFARE) attacks
  - Tailgating/Piggybacking: следование за сотрудником, social engineering на охране
  - Dumpster diving: сбор документов, паролей на стикерах, старых дисков

- **Wireless Red Team:**
  - WPA2/WPA3 Enterprise атаки: PEAP (часто с самоподписанными сертификатами), EAP-TTLS, attack on certificate validation
  - KARMA attack: rogue AP с известным SSID (client probing), Mana attack
  - Deauthentication attack: выбить клиента, перехватить handshake
  - PMKID capture: атака без клиента в сети
  - Evil Twin: поддельная точка доступа с перехватом трафика
  - WPA3: Dragonfly downgrade, SAE timing attacks (concept)
  - WiFi Pineapple: rogue AP платформа, karma, logging

- **Bluetooth/BLE:**
  - BlueBorne: remote code execution over BT
  - BLE recon: блеск-скан устройств, GATT service enumeration
  - Bluesnarfing, Bluejacking (concept)
  - Flipper Zero Bluetooth: BLE spam, HID attacks

- **Методология:**
  - Recon: изучение здания, беспроводной coverage, точки входа
  - Attack: выбор вектора (physical/electronic/social)
  - Persistence: leave access (backdoor, rogue AP), keep covert
  - Cleanup: убрать оборудование, собрать evidence

- **Юридические рамки:**
  - Только санкционированное тестирование, письменное согласие
  - Соблюдение законов о физическом доступе, изолированные тестовые участки
  - Не нарушать безопасность других зданий/сетей

**Практика:**

- BadUSB payload: написать и протестировать DuckyScript для автоматической типизации команд, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Rogue AP: ESP32/Flipper Zero создаёт точку доступа, captive portal, перехват credentials (изолированная среда), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- WPA2 Enterprise attack: настройка rogue AP с PEAP, перехват хешей → cracking (lab), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Deauth + handshake capture: aireplay-ng capture PMKID в изолированной среде, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- RFID: Proxmark3 клонирование и считывание карты доступа (в lab с разрешения), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- "Rogue AP с MITM": ESP32/Flipper Zero создаёт точку доступа с вашим SSID, перехватывает трафик (только lab), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Physical entry simulation runbook: MVST scenario, шаг за шагом, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Physical-to-Network Kill Chain: badge clone simulation + rogue device drop + network foothold → AD pivot → SOC validation, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** Hak5 (Rubber Ducky, Bash Bunny), Flipper Zero docs, Proxmark3 docs, *The Hardware Hacking Handbook*, aircrack-ng docs

---

#### 6. Purple Team и Документирование (20 часов)

Как документировать и как работать с защитой (синей командой).

**Что изучать:**

- **Purple Team методология:**

  - MITRE ATT&CK mapping (какая техника используется)
  - Atomic Red Team — тестирование детектов (найти, что пропускает SOC)
  - Caldera + Sandcat (автоматизация Red Team)

- **Отчет для Blue Team:**

  - Что было не замечено
  - Где сработала защита (и где нет)
  - Рекомендации с приоритетом (Critical, High, Medium, Low)
  - Timeline атаки (день X — что сделали)

- **Metrics:**

  - Time to Detect (TTD) — через сколько нас заметили
  - Time to Respond (TTR) — через сколько нас остановили
  - Coverage of MITRE ATT&CK — какие техники сработали

**Проект:**

> "Purple Team Automation" — скрипт, который запускает атаку (например, Pass-the-Hash) и проверяет, заметил ли SOC (по логам SIEM)

---

#### 7. Custom Malware Development на Rust/C (60 часов) 
**Время:** 60 часов

**Что изучать:**

- Архитектура импланта: transport, tasking, crypto, persistence modules
- Rust как memory-safe основа для agent development, C для low-level компонентов
- In-memory execution паттерны и ограничение артефактов на диске
- Kill-date, operator OPSEC, anti-analysis checks
- Безопасный жизненный цикл разработки offensive tooling

**Проекты:**

- Rust beacon с AES-GCM transport и jitter, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Модульный loader на C для controlled lab execution, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Custom Implant Framework: Rust agent + C loader + encrypted C2 + operator console, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** Rust docs, *Practical Malware Analysis*, offensive research blogs

---

#### 8. Phishing Infrastructure & 2FA Bypass (50 часов) 
**Время:** 50 часов

**Что изучать:**

- GoPhish кампании и метрики конверсии
- Evilginx2/Modlishka в легальном lab режиме
- Email infrastructure: SPF/DKIM/DMARC и anti-spoofing
- Redirector topology и доменная гигиена инфраструктуры
- Аналитика: open/click/credential capture rates

**Проекты:**

- Фишинговая симуляция для 3 ролей пользователей, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- 2FA bypass tabletop exercise и отчёт для Blue Team, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Enterprise Phishing Simulation Platform: landing pages + metrics + lessons learned, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** GoPhish docs, Evilginx2 docs, Microsoft anti-phishing guidance

---

#### 9. Physical/Wireless Red Team Operations (50 часов) 

**Время:** 50 часов

**Что изучать:**

- BadUSB workflows: Flipper Zero / ESP32 payload chains
- Rogue AP и credential harvesting detection gaps
- RFID/BLE attack surface: Proxmark3 и BLE recon
- Юридические рамки physical testing
- Связка physical access -> network foothold -> AD pivot

**Проекты:**

- Controlled rogue AP exercise в изолированной среде, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Physical entry simulation runbook с MITRE mapping, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Physical-to-Network Kill Chain Lab: badge clone simulation + rogue device drop + SOC validation

**📚 Ресурсы:** Flipper docs, Proxmark docs, Hak5 Academy

---

### Дорожка B: Blue Team (Защита и реагирование)

#### 1. Защита и обнаружение вторжений (расширенная)

**Время:** 60 часов

**Что изучать:**

- SIEM архитектура: ingestion pipelines, parsing/normalization, storage tiers, search layer, correlation engine и alerting workflows.
- Wazuh deep dive: decoders/rules hierarchy, active response safeguards, custom rulesets, MITRE mapping и lifecycle обновлений агентов.
- Elastic Stack для security: ECS нормализация, index templates/ILM, hot-warm storage, detection queries в Kibana и alert actions.
- Network monitoring: Zeek/Suricata deployment patterns, sensor placement, packet loss control, encrypted traffic visibility и tuning signatures.
- Интеграция IDS + SIEM: reliable log shipping, enrichment (asset/user/context), deduplication и correlation across host/network telemetry.
- Host-based detection: OSSEC/FIM policies, rootkit checks, baseline deviations, suspicious process/service persistence detection.
- EDR fundamentals: Falcon/Cortex event model, telemetry coverage gaps, detection content quality и incident triage workflows.
- Honeypots: Cowrie/Dionaea/T-Pot deployment, realistic deception scenarios, attacker TTP capture и legal/operational boundaries.
- Threat intelligence: STIX/TAXII feeds, MISP/OTX curation, IOC expiration policy, confidence scoring и automated enrichment.
- Log management: centralized collection, retention strategy, secure transport/storage, tamper resistance and compliance-driven policies.
- Alert tuning: false-positive reduction, threshold tuning, suppression logic, multi-signal correlation and detection debt tracking.
- Incident response: host isolation, evidence preservation, volatile data capture, chain of custody integrity и handoff процедуру.
- Splunk concepts: SPL search patterns, field extraction, accelerated data models и basic detection engineering practices.
- QRadar concepts: log source onboarding, offense correlation, rule tuning and workflow alignment with SOC operations.

**Проекты:**

- Развёртывание Wazuh: server + agents + custom rules, onboarding лог-источников и проверка end-to-end alert flow.
- Настройка Zeek: custom logs/scripts для DNS/HTTP/SMB, baseline профили и детект аномальных паттернов.
- Анализ реальных incident reports: реконструкция kill chain, выделение detection gaps и remediation backlog.
- Honey pot deployment: Cowrie/T-Pot с безопасной изоляцией, сбором IOC и автоматической отправкой событий в SIEM.

**🌟 FLAGSHIP PROJECT:**

- Complete SIEM: Wazuh + Elasticsearch + Kibana + custom rules + alerting, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** Wazuh docs, Elastic docs, OSSEC docs

---

#### 2. DFIR - Цифровая криминалистика и реагирование на инциденты

**Время:** 60 часов

**Что изучать:**

- Chain of custody: формальные процедуры документирования, hashing evidence, access logs, handoff checkpoints и юридическая пригодность артефактов.
- Live response: безопасный сбор volatile artifacts (processes, net connections, RAM indicators), минимизация contamination и scripted triage.
- Dead response: disk imaging (dd/E01), verification hashes, write blockers, partition carving и recovery deleted artifacts.
- Memory forensics: Volatility/Rekall workflows, process tree anomalies, injected code indicators, credential residue и kernel module checks.
- Network forensics: pcap reconstruction, session pivoting по timestamps, beacon correlation, C2/exfil detection и lateral movement tracing.
- Timeline analysis: unified timeline из host/network/cloud logs, timezone normalization, causality mapping и incident narrative building.
- Windows forensics: Registry hives, Event Logs, Prefetch, ShimCache, AmCache, MFT/USN Journal для persistence и execution evidence.
- Linux forensics: auditd/journald/syslog/bash history, cron/systemd artifacts, SSH traces, suspicious binaries и privilege escalation footprints.
- Mobile forensics: acquisition constraints, app artifact extraction, backup analysis и chain-of-custody ограничения на уровне практики.
- E-mail forensics: header path validation, SPF/DKIM/DMARC checks, attachment detonation, phishing campaign clustering.
- Steganography analysis: detection heuristics, LSB/content anomaly checks, extraction workflows и validation false positives.
- Incident response playbooks: ransomware/BEC/credential theft сценарии, decision trees, escalation matrix и communication templates.
- DFIR tools: Autopsy/FTK/CAINE/Velociraptor, coverage boundaries, automation opportunities и repeatable case workflow.

**Практика:**

- Исследование инцидента с нуля: intake -> triage -> evidence collection -> analysis -> final report с chain-of-custody.
- Извлечение данных из дампа памяти: подозрительные процессы, injected regions, network artifacts и credential traces.
- Timeline analysis реального инцидента: объединение host/network/cloud событий в единую временную линию и root cause.
- Анализ вредоносного трафика в pcap: C2 beaconing, exfil паттерны, lateral movement indicators и IOC extraction.

**Проекты:**

- Полное расследование инцидента: от сбора улик и верификации артефактов до итогового технического отчёта.
- Криминалистический дамп памяти с анализом, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Анализ сетевого дампа на предмет компрометации, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** *Digital Forensics*, *The Art of Digital Forensics*, SANS DFIR materials, Volatility docs

---

#### 3. Threat Hunting с Sigma-правилами

**Время:** 50 часов

**Что изучать:**

- Threat Hunting methodology: hypothesis-driven подход, data requirements, validation criteria, hunt backlog и ретроспектива эффективности.
- MITRE ATT&CK usage: mapping observed behaviors to tactics/techniques, gap analysis и приоритизация coverage по бизнес-рискам.
- Sigma rules internals: fields/operators/condition grammar, backend compatibility constraints и quality checks перед продом.
- Sigma authoring: platform-specific detections для Windows/Linux/network logs, anti-noise filters и confidence scoring.
- Sigma to SIEM pipeline: conversion в Splunk/Elastic/QRadar/Wazuh, field mapping pitfalls, test replay и regression checks.
- Hunting loop execution: indicator -> hypothesis -> search -> validate -> report -> tuning, с обязательной фиксацией learnings.
- TTP-based hunting: поиск поведенческих паттернов ATT&CK, chaining multiple weak signals и escalation criteria.
- YARA in hunting: malware family traits, rule hardening against evasion и integration with sandbox/EDR telemetry.
- OpenCTI operations: enrichment workflows, relationship graphing, campaign context и приоритизация actionable intelligence.
- MISP operations: IOC sharing quality, tagging/taxonomies, expiration policy и trust boundaries with external communities.
- EDR diagnostics: advanced queries, timeline pivoting, process ancestry analysis, remote response actions и triage automation.

**Проекты:**

- Написание 10+ Sigma-правил: coverage для privilege escalation, persistence, lateral movement, exfiltration и defense evasion.
- Hunting dashboard: KPI hunts, coverage heatmap, false-positive trends и investigation drill-down для аналитиков.
- Интеграция Sigma с Wazuh/Elastic: CI-конвертация, replay test events и проверка стабильности детектов после обновлений.
- Active hunting: hypothesis-based поиск угроз на реальных логах с валидацией находок и оформлением отчёта.

**🌟 FLAGSHIP PROJECT:**

- Threat Hunting Lab: Sigma + Wazuh + активный hunting, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** Sigma docs, MITRE ATT&CK, *The Practice of Threat Hunting*

---

#### 4. Advanced SIEM Engineering (50 часов) 

**Время:** 50 часов

**Что изучать:**

- Корреляционные правила: multi-event, time-window, suppression
- Sigma engineering lifecycle: authoring, tuning, versioning, coverage mapping
- Нормализация логов: ECS/CEF/LEEF
- Detection-as-code и тестирование правил в CI
- Метрики SIEM качества: precision, recall, MTTD

**Проекты:**

- Реализовать 20 production-ready Sigma rules по ATT&CK, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Detection CI pipeline: lint + unit tests + sample events replay, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Detection Engineering Platform: Sigma repo + auto validation + deployment в Wazuh/Elastic, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** SigmaHQ, Elastic detection rules, Splunk security content

---

#### 5. DFIR Toolkit Automation (40 часов) 

**Время:** 40 часов

**Что изучать:**

- Автоматизация triage: сбор логов, memory artifacts, network captures
- Скрипты первичной оценки компрометации для Windows/Linux
- Timeline merging из host/network/cloud источников
- Стандартизация IR артефактов и chain of custody templates
- Post-incident review и lessons learned

**Проекты:**

- DFIR collector toolkit (Python/Bash) для live response, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Автоматическая генерация incident timeline из набора логов, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Rapid DFIR Kit: единый CLI для triage + evidence packaging + initial severity scoring, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** Velociraptor docs, Volatility docs, SANS IR cheat sheets

---


## Семестр 7 - Enterprise Security Engineering (Февраль - Июнь)

**Время:** ~340–380 часов

### 1. eBPF/XDP Security Engineering (enterprise)

**Время:** 100 часов

**Что изучать:**

- **eBPF fundamentals:**
  - Program types: XDP, TC, socket filter, cgroup, tracepoint, kprobe, perf event
  - Attach points: netdev (XDP), ingress/egress (TC), kernel functions (kprobe), user functions (uprobe)
  - Verifier: instruction limits, loop bounds, memory access checks, helper function restrictions, 错误信息解读
  - Helper functions: bpf_map_lookup_elem, bpf_map_update_elem, bpf_ktime_get_ns, bpf_get_smp_processor_id
  - Map lifecycle: creation, update, lookup, delete, pinning, sharing between programs
  - BPF maps: hash, array, LRU hash, LRU per-CPU, ring buffer, perf event array, stack trace

- **XDP (eXpress Data Path):**
  - XDP program types: XDP_PASS, XDP_DROP, XDP_REDIRECT, XDP_TX, XDP_ABORTED
  - Native XDP vs offloaded XDP vs generic XDP — performance characteristics
  - NIC support: driver-dependent features, multi-buffer XDP
  - XDP use-cases: DDoS mitigation, load balancing (XDP sponsored), NAT, firewalling
  - XDP interaction with TC: chaining XDP + TC programs

- **TC with eBPF:**
  - Ingress/egress hooks: classful qdisc, BPF filter, action
  - TC classifier-action: filter → action (pass/drop/mirror)
  - Integration with iptables/nftables: chaining and interaction

- **eBPF programming in C:**
  - clang/llvm toolchain: compiling BPF programs, -target bpf
  - libbpf: skeleton (skeleton.h), program loading, map interaction, CO-RE helpers
  - vmlinux.h: kernel type definitions, BTF reliance
  - Userspace control plane: loading programs, updating maps, reading events

- **CO-RE (Compile Once Run Everywhere):**
  - BTF (BPF Type Format): kernel type information, relocations
  - CO-RE relocs: field offset adjustments across kernel versions
  - bpftool: prog show, map dump, btf show, net show
  - Compatibility matrix: kernel versions, distro support

- **Observability with eBPF:**
  - Network metrics: latency per function, packet drops, retransmits, connection states
  - TCP observability: retransmission reasons, RTT estimation, congestion window, zero-window
  - DNS observability: query/response time, failures, SERVFAIL analysis
  - HTTP observability: request/response time, status codes, latency distribution
  - Disk I/O observability: block device latency, queue depth, IOPS per process
  - Process scheduling: off-CPU time, context switch frequency, run queue latency

- **Security with eBPF:**
  - Network policy enforcement: L3/L4 filtering, connection tracking, rate limiting
  - DDoS mitigation: early drop, blacklisting, SYN proxy, rate limiting per source
  - Audit: syscall monitoring, file access, process execution, network connections
  - Runtime security: container escape detection, suspicious process behavior

- **eBPF performance:**
  - Profiling eBPF programs: perf, bpftool prog profile, self-programmed stats
  - Verifier logs: reading and interpreting verifier output, complexity analysis
  - Map performance: hash vs array, per-CPU vs shared, batching operations
  - Program overhead: instruction count, memory usage, cache behavior

- **eBPF for cloud-native:**
  - Cilium datapath: how Cilium uses eBPF, replacement of kube-proxy
  - Service mesh: sidecar-less service mesh via eBPF, transparent encryption
  - Multi-cluster networking: ClusterMesh, global network policies

- **eBPF ecosystem:**
  - bpftrace: high-level tracing language, probes, one-liners, scripts
  - bcc tools: pre-built tools, python bindings
  - Pixie: auto-instrumentation, eBPF-based observability platform
  - Falco: eBPF-based runtime security, rules, alerts

- **Debugging eBPF:**
  - bpftool: prog show, map dump, btf dump
  - perf: profile eBPF programs, benchmark
  - libbpf_print: debugging output, verifier log parsing
  - Test frameworks: bpftest, kselftest for BPF

**Практика:**

- XDP hello world: simplest XDP program, attach, verify, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- XDP DDoS mitigation: drop rules based on source IP, rate limiting, blacklisting, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- TC eBPF: ingress filtering, packet modification (ecn marking), logging, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- bpftrace for network debugging: 10 scripts (TCP latency, DNS failures, socket operations), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- libbpf program: write C eBPF program with map interaction, userspace control plane, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- eBPF observability agent: collect TCP/DNS metrics, export to Prometheus, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Performance benchmark: XDP vs iptables vs nftables throughput test, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- XDP firewall для DDoS protection: rules + metrics + dynamic blacklisting + Prometheus exporter, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- eBPF telemetry exporter: TCP/DNS/disk latency metrics → Prometheus → Grafana, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- eBPF network IDS: XDP drop + TC logging + alerting pipeline, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Network IDS с eBPF: XDP mitigation (DDoS) + TC logging (detection) + Prometheus metrics + Grafana dashboard + alerting, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- eBPF-based Service Mesh: transparent mTLS, load balancing, observability without sidecars, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** eBPF.io, Cilium docs, kernel bpf docs, Brendan Gregg материалы, *BPF Performance Tools* (Gregg), *Learning eBPF* (Liz Rice), libbpf-bootstrap

---

### 2. Высоконагруженные сетевые сервисы на Go/Rust

**Время:** 90 часов

**Что изучать:**

- Go: context propagation, graceful shutdown, middleware chains, pprof
- Rust: async runtime (tokio), ownership в сетевом коде, memory safety
- Zero-copy подходы: sendfile, буферизация и пул буферов
- Backpressure, retries, circuit breaker, bulkhead patterns
- API gateway паттерны: auth, rate limiting, request tracing
- TLS termination, mTLS между сервисами
- Нагрузочное тестирование: k6/wrk + SLO/SLA метрики

**Проекты:**

- Async Go API Gateway (auth + rate limiting + logging), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Rust network worker с bounded memory, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Сравнение latency/throughput между C epoll, Go, Rust, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT:**

- Async API Gateway 100k RPS (в лаборатории): нагрузочные профили, профилирование, оптимизация горячего пути

**📚 Ресурсы:** Go docs, Tokio docs, *Systems Performance*, k6 docs

---

### 3. Cloud-native Security Architecture

**Время:** 80 часов

**Что изучать:**

- EKS security baseline: IRSA, pod security boundaries, SG for pods, restricted node roles и секреты только через managed services.
- Kubernetes admission control: OPA/Gatekeeper/Kyverno policies, deny-by-default rules, exception workflow и policy unit tests.
- Runtime security в k8s: Falco/Tetragon sensors, syscall and behavior detection, response hooks и noise reduction tuning.
- CSPM program: Prowler/ScoutSuite checks, risk-based triage, ownership mapping и remediation SLA governance.
- HTTP/3/QUIC security: handshake visibility limits, abuse patterns, edge mitigation strategy и telemetry requirements.
- WireGuard architecture: secure admin overlay, key lifecycle management, segmentation by role and emergency access controls.
- Threat modeling for cloud-native: trust boundaries, attack paths through control plane/data plane, abuse cases и mitigation backlog.

**Проекты:**

- Secure EKS baseline с NetworkPolicy и admission policies, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Cloud posture audit: AWS аккаунт с чек-листом критичных рисков, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- WireGuard hub-and-spoke для лаборатории, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT:**

- Secure Cloud Blueprint: IaC baseline + security controls + threat model + remediation backlog, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** AWS Security docs, EKS Best Practices, WireGuard docs, OWASP Threat Modeling

---

### 4. Purple Team Engineering и метрики

**Время:** 70 часов

**Что изучать:**

- Purple Team loop: controlled emulation -> detection validation -> tuning -> retest, с фиксированием evidence и owner assignments.
- ATT&CK coverage mapping: technique-by-technique heatmap, detection depth scoring и quarterly gap closure plan.
- Maturity metrics: TTD/TTR, precision/recall, false-positive burden, analyst effort per alert и trend tracking.
- Atomic Red Team/Caldera operations: scheduled adversary simulations, safety guardrails, repeatability и benchmark comparisons.
- Reporting model: executive summary для руководства + technical deep dive для SOC/engineering команд с remediation actions.
- Remediation prioritization: Critical/High/Medium/Low matrix based on exploitability, exposure, business impact and fix complexity.

**Проекты:**

- Purple Team runbook для 10 техник ATT&CK, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Автоматическая проверка детектов по расписанию, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Dashboard метрик TTD/TTR по техникам, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT:**

- Purple Team Lab: автоматизированные тесты техник + Sigma/Suricata правила + отчёт по покрытию, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** MITRE ATT&CK, Atomic Red Team, Sigma docs, Wazuh docs

---

### 5. Leadership Track для Red Team Lead / Architect

**Время:** 40 часов

**Что изучать:**

- Scoping and planning: rules of engagement, objective hierarchy, legal constraints, success criteria and deconfliction process.
- Stakeholder communication: CISO/SOC/infra alignment, expectation management, risk communication cadence и escalation channels.
- Risk reporting: technical findings -> business impact translation, likelihood/impact scoring и scenario-based prioritization.
- Remediation roadmap: actionable fixes with owners, deadlines, dependencies, verification gates и residual risk tracking.
- Team leadership: mentoring juniors, quality review of findings, consistency in methodology и delivery standards.
- Security design review: network/cloud architecture critique, threat-informed improvements и defensible security trade-offs.

**Проекты:**

- Подготовка и защита архитектурного security review перед mock-комиссией, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Создание lead-level отчёта по полной Red Team симуляции, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT:**

- Security Architecture Review: аудит 2 систем (on-prem + cloud) и защита плана улучшений, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** NIST CSF, MITRE ATT&CK Navigator, OWASP SAMM

---

### 6. Cilium и eBPF Security Platform (60 часов) 

**Время:** 60 часов

**Что изучать:**

- Cilium datapath internals: eBPF policy enforcement path, identity model, service load-balancing and kube-proxy replacement implications.
- L3/L4/L7 policy design: default deny, namespace/workload scoping, DNS/FQDN rules и Hubble observability correlations.
- Threat-aware segmentation: zero-trust microsegmentation, east-west blast-radius reduction и policy simulation before enforcement.
- Cilium performance tuning: conntrack pressure, policy scale impacts, datapath profiling and latency under load.
- SIEM/SOAR integration: exporting flow/security events, enrichment with workload identity and automated response playbooks.

**Проекты:**

- Развернуть k8s lab с Cilium + Hubble + security policies, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Реализовать policy pack для 3-tier приложения с тестами, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Cilium Security Blueprint: policy-as-code, observability и incident drills, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** Cilium docs, Hubble docs, Kubernetes security docs

---

### 7. Highload Rust Networking (60 часов) 

**Время:** 60 часов

**Что изучать:**

- Async Rust highload patterns: Tokio scheduler behavior, cooperative tasks, bounded queues and backpressure propagation.
- Lock-free/low-lock data structures: queues/maps/ring buffers, contention profiling and correctness under concurrent load.
- Zero-copy parsing: bytes crate, slice-based protocol parsing, memory footprint budgeting и allocation hotspot reduction.
- Benchmarking/profiling: criterion microbenchmarks, flamegraph/perf analysis, p99/p999 focus and regression baselines.
- Reliability engineering: retries with jitter, circuit breaker/bulkhead, graceful degradation and overload protection strategy.

**Проекты:**

- Rust TCP/HTTP service с нагрузкой 50k+ RPS, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Сравнение latency p99 между Rust и Go реализациями, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Highload Rust Gateway: p99 latency tuning + load report + failover, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** Tokio docs, Rust Performance Book, criterion docs

---

### 8. Cloud-native Security Operations (50 часов)

**Время:** 50 часов

**Что изучать:**

- Runtime detection in k8s: Falco/Tetragon rule engineering, syscall/context enrichment, actionable severity mapping and noise control.
- Cloud IR scenarios: IAM credential compromise, token abuse, suspicious automation, container breakout and blast-radius containment.
- Supply chain security: image signing/verification, SBOM validation, provenance checks and admission policy enforcement.
- WireGuard admin overlay: isolated management plane, MFA-gated access paths, key rotation and audit logging.
- Continuous posture management: drift detection against IaC baseline, risk triage workflow and remediation automation loops.

**Проекты:**

- Настроить cloud-native SOC mini stack, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Реализовать incident playbooks для 3 cloud сценариев, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Cloud-native Security Operations Lab: detections + posture scans + response automation, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** Falco docs, Tetragon docs, AWS IR guide, CNCF security papers

---

## Семестр 8 - Финализация (Сентябрь - Декабрь)

**Время:** ~320 часов

### 1. Финальный проект

**Время:** 120 часов

**Что изучать:**

- Выбор темы: Network IDS/IPS, Security Dashboard, Pentest framework, vulnerability scanner или monitoring tool с измеримыми целями и scope.
- Проектирование: C4/DFD диаграммы, trust boundaries, threat model, data classification и non-functional requirements.
- Реализация: production-like прототип с ограничениями, telemetry hooks, error budget и clear assumptions.
- Тестирование: unit/integration/security/performance tests, negative cases, fuzzing points и reproducible test plan.
- Документация: architecture decision records, API contracts, deployment guide, user/ops runbook и known limitations.
- Репозиторий: clean commit history, CI gates, lint/security checks, issue templates and release notes discipline.

**Проекты:**

- Network IDS с современным UI, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Pentest automation framework, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Vulnerability scanner для сети, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Security operations center (SOC) simulator, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Network traffic anomaly detector с ML, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Secure proxy/VPN с enhanced logging, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Presentation:** Демо, слайды, Q&A практика

---

### 2. Code Review чужого сетевого проекта

**Время:** 40 часов

**Что изучать:**

- Code review процесс: подход, цели, критерии оценки
- Поиск open source проектов: простые TCP-серверы, эхо-серверы, чат-серверы
- Типичные баги в сетевых проектах: race conditions, buffer overflow, resource leaks
- Анализ безопасности: проверка ввода, обработка ошибок, управление памятью
- Best practices: структура кода, документация, тесты
- Написание code review отчёта: найденные баги, рекомендации, severity

**Проекты:**

- Code review 2–3 простых TCP-серверов (эхо-чат, простой HTTP сервер), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Поиск и документирование багов с severity levels, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Рекомендации по исправлению уязвимостей, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT:**

- Полный code review: найти 5+ багов, написать рекомендации, предложить исправления, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** Code review guides, *The Art of Code Review*

---

### 3. Глубокая специализация (выбрать направление)

**Время:** 80 часов

**Направления:**

- Network Forensics: pcap/NetFlow reconstruction, attacker timeline, evidentiary standards и courtroom-ready documentation подход.
- Malware Analysis: static/dynamic workflows, C2 behavior reverse, unpacking/obfuscation handling и IOC/TTP extraction.
- Cloud Security: multi-cloud governance, CSPM + CIEM, identity attack paths и continuous hardening program.
- OT/ICS Security: Modbus/DNP3/SCADA network visibility, legacy constraints, segmentation and safety-first response.
- IoT Security: firmware extraction/emulation, embedded protocol abuse, hardware attack surface and secure update chain.
- Zero Trust architecture: identity-centric policy, continuous verification, microsegmentation and adaptive access controls.
- Red Teaming: end-to-end adversary emulation, campaign planning, detection validation and executive-level risk reporting.

**Проекты:**

- Один глубокий проект по выбранной нише, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Техническая статья или доклад, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Вклад в open source в области, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

---

### 4. Поиск работы

**Время:** 50 часов

**Что изучать:**

- Резюме под network security роли: impact-driven bullets, quantified outcomes, stack/tools evidence и role-specific tailoring.
- LinkedIn strategy: профиль как landing page, endorsements/recommendations, регулярный technical content и outreach cadence.
- Target companies/roles: SOC/Blue/Red/SecEng tracks, job description gap analysis и application prioritization matrix.
- Professional networking: конференции, meetups, CTF/community presence, warm introductions and follow-up discipline.
- Interview prep: technical drills (packets/ACL/troubleshooting) + behavioral STAR stories, whiteboard scenarios и mock interviews.
- Certification path: Security+/CEH/OSCP/CISSP sequencing по целевой роли, effort budgeting и exam ROI evaluation.
- Portfolio quality: reproducible demos, architecture docs, threat model artifacts и concise case studies on outcomes.

**Проекты:**

- Отклики: 50+, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Технических интервью: 10+, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Оффер: минимум один, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

---

### 5. Разбор 20 реальных сетевых кейсов для собеседований

**Время:** 30 часов

**Кейсы:**

1. **Компьютер не получает IP по DHCP** - диагностика, типичные причины
2. **Нет доступа к интернету через роутер** - NAT, DNS, routes
3. **Не работает VPN подключение** - MTU,Firewall, протоколы
4. **Высокий latency на одном хосте** - проверка link negotiation
5. **VLAN трафик не ходит между свитчами** - trunk, native VLAN
6. **DNS резолвит медленно** - /etc/resolv.conf, firewall
7. **SMB шаринг недоступен** - ports, authentication
8. **STP блокирует нужный порт** - port-priority, root bridge
9. **Роутер не маршрутизирует** - ip forward, routing table
10. **HTTPS не работает только на certain domains** - TLS 1.3, CIPHER mismatch
11. **SSL certificate error в приложении** - cert chain, hostname
12. **ARP таблица переполнена** - ARP spoofing, static entries
13. **MAC address на двух портах** - MAC flapping, loops
14. **NAT loopback не работает** - Hairpin NAT
15. **MTU issues в туннеле** - Path MTU Discovery
16. **VPN drop при heavy traffic** - fragmentation
17. **WiFi клиент не видит сеть** - broadcast, channel
18. **ACL не работает как ожидается** - implicit deny, order
19. **Firewall block loopback interface** - zone-based
20. **Proxy PAC файл не работает** - JavaScript errors, WPAD

**Формат разбора:**

- Симптомы
- Диагностика (какие команды)
- Root cause
- Решение

**Практика:**

- Разбор каждого кейса с командами диагностики, гипотезами, подтверждением root cause и финальным remediation plan.
- Самостоятельная диагностика, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**📚 Ресурсы:** Network Engineer interview questions, NetworkWorld кейсы

---

### 6. Дополнительные темы

**Время:** 70 часов

**Темы на выбор:**

- DNSSEC: signing chain, KSK/ZSK rotation, validation failures, NSEC/NSEC3 nuances и operational pitfalls.
- IPv6 security: NDP/RA abuse, SLAAC hardening, privacy extensions trade-offs и dual-stack attack surface.
- QUIC/HTTP3 security: encrypted transport visibility gaps, protocol abuse patterns и edge detection controls.
- Tor network: onion routing internals, hidden services opsec, deanonymization risks и defensive monitoring strategies.
- Blockchain security: smart contract basics, common vuln classes, wallet/key custody и infra attack vectors.
- Hardware security: TPM attestation, secure/measured boot chains, firmware trust anchors and supply-chain concerns.
- Voice security: VoIP/SIP interception risks, SS7 weaknesses, fraud patterns and detection constraints.
- Bluetooth/BLE security: pairing weaknesses, MITM/sniffing vectors, GATT abuse and device hardening practices.
- 5G security: core/RAN threat model, slicing risks, signaling abuse and telecom-specific defense controls.

**Проекты:**

- Исследовательский проект по одной теме, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Статья или пост, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

---

### 7. Advanced Code Review Mastery (50 часов) 

**Время:** 50 часов

**Что изучать:**

- Security code review методология для C/C++/Go/Rust/Python
- Поиск memory safety багов, race conditions, auth bypass, SSRF/RCE
- Threat-model driven review: trust boundaries и data flow
- Автоматизация review чеков (SAST + custom scripts)
- Подготовка review отчётов уровня senior/principal

**Проекты:**

- Провести review 5 сетевых open source проектов с severity grading, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Подготовить patch proposals и отправить минимум 3 PR с исправлениями, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Security Review Portfolio: 20+ найденных дефектов/уязвимостей с fix-патчами, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** CodeQL docs, Semgrep docs, OWASP Code Review Guide

---

### 8. Анализ реальных инцидентов (60 часов) 

**Время:** 60 часов

**Что изучать:**

- Разбор публичных breach reports: kill chain, root cause, detection gaps
- Correlation host/network/cloud сигналов в единую timeline
- Оценка технического и бизнес-ущерба
- Формирование remediation roadmap после инцидента
- Подготовка executive brief и технического appendix

**Проекты:**

- 10 разборов реальных инцидентов (APT/ransomware/cloud compromise), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Создание шаблона post-incident report для Security Lead, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Incident Knowledge Base: 20 кейсов с ATT&CK mapping и defensive controls, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** CISA advisories, Mandiant reports, CrowdStrike reports, MITRE ATT&CK

---

### 9. Advanced Certification Prep (50 часов) 

**Время:** 50 часов

**Что изучать:**

- Подготовка к OSEP/OSWE/CRTO/AWS Security Specialty по слабым зонам
- Лабораторные sprint-и с таймбоксом под формат экзамена
- Exam strategy: time management, report quality, objective selection
- Карта пробелов компетенций и план закрытия до интервью
- Связь сертификаций с ролью Red Team Lead/Security Architect

**Проекты:**

- 6 exam-like симуляций (8-12 часов каждая), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Персональная competency matrix с еженедельным апдейтом, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Certification War Room: дашборд прогресса, симуляции, ретроспективы, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** OffSec docs, Zero-Point Security materials, AWS prep guide, HTB Pro Labs

---

## Семестр 9 - Red Team Mastery & Architecture (Финальный уровень)

**Время:** ~400 часов

### 1. Enterprise Red Team Operations (80 часов)
**Время:** 80 часов

**Что изучать:**

- **Planning & Scoping:**
  - Rules of Engagement (ROE): legal boundaries, scope definition, IP ranges, domains, exclusion lists
  - Threat model integration: mapping to MITRE ATT&CK for target organization
  - Objective hierarchy: primary objectives, secondary goals, success criteria,禁区 areas
  - Infrastructure planning: C2 setup, redirectors, VPN, domain fronting, OPSEC requirements
  - Team roles: operator, infrastructure, lead, analyst, medical/security officer
  - Deconfliction process: blue team awareness, safety signals, emergency shutdown procedures

- **Operational Phases:**
  - Reconnaissance: OSINT, passive DNS, technology fingerprinting, employee enumeration
  - Initial Access: phishing, web exploit, physical, supply chain, trusted relationship
  - Establishment: foothold persistence, C2 installation, beacon configuration
  - Discovery: internal recon, network enumeration, AD enumeration, trust mapping
  - Privilege Escalation: local privesc, domain escalation, certificate abuse
  - Lateral Movement: credential reuse, relay, delegation abuse, pivot techniques
  - Collection: data staging, targeted exfil preparation, screenshot/keylog
  - Exfiltration: DNS/HTTPS/ICMP channels, encryption, compression, chunking
  - Cleanup: evidence removal, log clearing, persistence removal (or leave for realism)
  - Reporting: technical timeline, executive summary, detection validation

- **Metrics & Measurement:**
  - Dwell time: time from initial access to detection
  - Time to Detect (TTD): how quickly blue team identified activity
  - Time to Respond (TTR): how quickly blue team contained threat
  - Coverage: percentage of ATT&CK techniques tested
  - Objective completion: primary/secondary goals achieved
  - Impact assessment: what could have been compromised if unchecked

- **Long-Running Operations:**
  - Multi-week campaigns: maintaining persistence, avoiding detection
  - Infrastructure rotation: C2 channel switching, domain rotation, IP changes
  - Operator OPSEC: clean VMs, VPN discipline, behavioral consistency, traffic patterns
  - Task scheduling: managing multiple operators, coordination, task delegation

- **Communication & Reporting:**
  - Stakeholder communication: CISO updates, SOC coordination, legal counsel
  - Executive briefings: risk communication, business impact, board-level presentation
  - Technical deep-dive: evidence presentation, detection gap analysis, remediation roadmap
  - Debrief: lessons learned, playbook improvement, blue team feedback integration

- **Compliance & Legal:**
  - Legal frameworks: Computer Fraud and Abuse Act, international considerations
  - Third-party coordination: ISP, cloud provider, hosting company notifications
  - Evidence handling: preservation, chain of custody, expert witness preparation
  - Insurance considerations: cyber insurance, liability, coverage requirements

**Практика:**

- Operation planning: создание полного operation plan для enterprise lab с 5 бизнес-юнитами (written scenario, ROE, objectives), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Reconnaissance campaign: полная внешняя разведка организации (OSINT, DNS, Shodan, employee enumeration), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Initial Access: 3 different vectors (phishing, web, physical) testing, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Multi-week campaign: 2-week operation с persistent access, rotation, cleanup, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Executive report preparation: создание presentation для management + technical report для SOC/engineering, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- Полный operation plan: ROE + objectives + infrastructure plan + team roles + timeline + metrics, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- 2 emulation campaigns: разные сценарии (APT29-style + ransomware group) с метриками TTD/TTR, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Enterprise Red Team Program-in-a-Box: planning templates + execution playbooks + governance + metrics framework, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** MITRE ATT&CK Evaluations, NIST SP 800-115, *Red Team Development and Operations* (Bader et al.), *The Art of Intrusion* (Mitnick)

---

### 2. Advanced C2 Engineering (60 часов)

**Время:** 60 часов

**Что изучать:**

- Distributed C2 architecture: multi-tenant teamservers, redundant redirectors
- Traffic profile randomization, fallback channels, channel failover
- OPSEC hardening C2: secret rotation, infra hygiene, blast radius limitation
- Detection-aware C2 design и сигнатурная устойчивость
- Telemetry-safe testing и rollback при ошибках

**Проекты:**

- Построить C2 с multi-channel transport (HTTPS + DNS fallback) в lab, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Реализовать автоматическое переключение каналов при потере связи, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Adaptive C2 Fabric: модульная инфраструктура с failover, jitter profiles и observability, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** Sliver docs, Mythic docs, C2 Matrix, SANS red team papers

---

### 3. Custom Implant Development (80 часов) 

**Время:** 80 часов

**Что изучать:**

- **Имплант архитектура:**
  - Tasking model: operator commands → implant execution → result return
  - Module system: core commands + loadable modules (mimikatz-like, collection, lateral movement)
  - Encrypted transport: AES-GCM/ChaCha20-Poly1305, key exchange (ECDH), session keys
  - Beaconing: jitter (randomized sleep), sleep time, kill date, check-in intervals
  - Staging: encrypted staging (key exchange before payload delivery), stageless vs staged
  - C2 channels: HTTPS, DNS, ICMP, custom protocols — channel switching capability

- **Rust/C development patterns:**
  - Rust для memory-safe core: reqwest для HTTP, tokio для async, ring/rustcrypto для crypto
  - C для low-level компонентов: syscalls, PE manipulation, process injection primitives
  - FFI boundary: Rust ↔ C interface, unsafe blocks, proper error propagation
  - Build systems: cargo (Rust), cmake/make (C), cross-compilation (x86_64-pc-windows-msvc)
  - Obfuscation: string encryption, control flow flattening, dead code insertion
  - Payload generation: position-independent code, shellcode generation, reflective loading

- **In-memory execution:**
  - Reflective DLL injection: loading DLL without writing to disk, PE header manipulation
  - Process hollowing: RunPE, suspended process creation, memory replacement
  - Process injection: CreateRemoteThread, APC queuing, thread hijacking, section mapping
  - Syscall evasion: direct syscalls (SysWhispers), Hell's Gate, Halos Gate
  - Unhooking: refreshing ntdll.dll from disk, clearing userland hooks

- **Anti-analysis techniques:**
  - Anti-VM: VM detection (registry keys, MAC addresses, CPUID, timing)
  - Anti-debug: IsDebuggerPresent, NtQueryInformationProcess, timing checks
  - Anti-sandbox: behavioral checks, environmental keying, delayed execution
  - Code obfuscation: opaque predicates, control flow flattening, string encryption
  - Encryption: runtime decryption of strings/code, key derivation from environment

- **Persistence mechanisms:**
  - Userland: Registry Run keys, Scheduled Tasks, Startup folder, Services, COM objects
  - Kerberos: Golden Ticket, Diamond Ticket (persistent domain access)
  - ADCS: certificate-based persistence, auto-enrollment abuse
  - GPO: scheduled tasks, startup scripts via Group Policy
  - WMI: event subscriptions for persistence

- **Data handling:**
  - Collection: screen capture, keylogging, clipboard monitoring, file collection
  - Staging: local compression, encryption, temporary storage
  - Exfiltration: chunked transfer, protocol mixing (DNS for small, HTTPS for large), encryption
  - Cleanup: secure deletion, log clearing, evidence removal

- **OPSEC:**
  - Traffic profiling: mimicking legitimate traffic patterns, user-agent rotation
  - Timezone awareness: operating during business hours
  - Beacon jitter: avoiding pattern detection, randomizing sleep intervals
  - Infrastructure hygiene: domain reputation, SSL certificates, IP clean status

- **Testing & validation:**
  - EDR testing: deploying in lab with Defender/SentinelOne/CrowdStrike, comparison
  - Network detection: testing against Suricata/Zeek/Snort rules
  - Sandbox evasion: testing against popular sandboxes (Joe Sandbox, Any.Run, Cuckoo)

**Практика:**

- Basic implant on Rust: HTTP beacon + AES encryption + command execution + result exfil, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Process injection lab: CreateRemoteThread + reflective DLL loading in controlled environment, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Syscall evasion: direct syscalls implementation comparison (with/without hooks), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- DNS C2 channel: DNS-based command channel (TXT records), chunked data transfer, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- EDR detection test: deploy implant against Defender in lab, compare detection with/without obfuscation, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- Custom Implant Suite v3: Rust agent (HTTP/DNS channels) + C low-level modules (injection, persistence) + test harness + detection evasion report, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- C2 framework basics: Go/Rust teamserver + implant + encrypted comms + operator console, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Detection evasion benchmark: test against 3 EDR solutions, measure detection rates, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Custom Implant Framework: Rust agent + C loader + encrypted multi-channel C2 + operator console + evasion techniques + detection report, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** Rust docs, Offensive Rust materials, *Practical Malware Analysis*, *Evading EDR* (Matt Hand), SysWhispers, Outflank research

---

### 4. Purple Team Engineering at Scale (60 часов)

**Время:** 60 часов

**Что изучать:**

- Continuous validation pipeline: Atomic tests -> detections -> feedback loop
- ATT&CK coverage engineering и gap-driven development
- Detection quality metrics: precision/recall/MTTD/MTTR
- Планирование совместных red/blue exercise на квартальном цикле
- Executive reporting по улучшению security posture

**Проекты:**

- Запуск purple pipeline для 25 ATT&CK техник с регулярными прогонами, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Автоматизация отчётов по покрытиям и неотработанным сценариям, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Purple Engineering Platform: test orchestration + detections QA + management dashboard, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** Atomic Red Team, ATT&CK Navigator, Sigma docs, detection engineering blogs

---

### 5. Security Architecture & Zero Trust (80 часов) 
**Время:** 80 часов

**Что изучать:**

- **Security architecture patterns:**
  - Defense in depth: layered controls, redundancy, fail-safe defaults
  - Least privilege: RBAC, ABAC, just-in-time access, standing vs ephemeral privileges
  - Separation of duties: dual control, split knowledge, independent verification
  - Zero Standing Privileges: ephemeral credentials, time-bound access, approval workflows
  - Secure-by-default: deny-all, explicit allow, fail-closed design

- **Zero Trust Architecture (ZTA):**
  - NIST SP 800-207: principles, logical components, deployment models
  - Identity-centric security: identity as new perimeter, continuous verification
  - Device posture: endpoint assessment, compliance checks, certificate-based auth
  - Microsegmentation: workload isolation, east-west traffic control, identity-based policies
  - Software-Defined Perimeter (SDP): connect-after-authenticate, single-packet authorization
  - Continuous monitoring: behavioral analytics, anomaly detection, risk scoring
  - ZTA in practice: Google BeyondCorp, Microsoft Zero Trust, Zscaler/ZPA

- **Threat modeling:**
  - STRIDE: Spoofing, Tampering, Repudiation, Information disclosure, DoS, Elevation of privilege
  - LINDDUN: privacy threat modeling (Linkability, Identifiability, Non-repudiation, Detectability, Disclosure, Unawareness, Non-compliance)
  - Attack trees: root goal → attack paths → leaf nodes → probability/cost estimation
  - Data flow diagrams (DFD): processes, data stores, external entities, trust boundaries
  - Abuse cases: misuse stories, security requirements derivation
  - Tools: Microsoft Threat Modeling Tool, OWASP Threat Dragon, pytm
  - PASTA: Process for Attack Simulation and Threat Analysis (7-step methodology)

- **Architecture by environment:**
  - On-premises: segmented networks, DMZ, internal firewalls, jump servers
  - Cloud-native: VPC design, security groups, NACLs, WAF, API gateway, serverless
  - Hybrid: site-to-site VPN, direct connect, identity federation, split-brain DNS
  - Multi-cloud: consistent policies, CSPM, unified identity, network connectivity
  - Container/K8s: admission control, network policies, runtime security, image scanning

- **IAM architecture:**
  - Authentication: MFA (TOTP, WebAuthn/FIDO2, push), passwordless, certificate-based
  - Authorization: RBAC, ABAC, policy engines (OPA, Cedar)
  - Federation: SAML 2.0, OIDC, OAuth 2.0 flows, trust relationships
  - Privileged Access Management (PAM): vault, credential rotation, session recording
  - Identity governance: access reviews, lifecycle management, compliance mapping

- **Network security architecture:**
  - Segmentation: VLAN/VRF, microsegmentation, firewall zones, DMZ
  - Encryption: TLS everywhere, mTLS between services, VPN for remote access
  - Detection: IDS/IPS placement, network tap/SPAN, network detection and response (NDR)
  - DNS security: DNSSEC, DNS filtering, DNS monitoring, DoH/DoT

- **Cloud security architecture:**
  - AWS: VPC design, SG/NACL, WAF, GuardDuty, Security Hub, Config Rules
  - Azure: VNet, NSG, Azure Firewall, Defender for Cloud, Sentinel
  - GCP: VPC, Cloud Armor, SCC, Chronicle SIEM
  - Shared responsibility model: IaaS/PaaS/SaaS ownership boundaries

- **Security governance:**
  - Frameworks: NIST CSF 2.0, ISO 27001/27002, SOC 2, CIS Controls
  - Risk management: risk assessment, risk treatment, residual risk acceptance
  - Compliance: PCI-DSS, HIPAA, GDPR — security controls mapping
  - Security review process: architecture review board, security champion model

- **IR & resilience architecture:**
  - IR plan: roles, communication, escalation, evidence handling
  - SIEM architecture: log sources, parsing, correlation, alerting, retention
  - SOAR: playbook automation, incident enrichment, response actions
  - BC/DR: RTO/RPO, backup strategy, failover design

- **Metrics & reporting:**
  - Security metrics: MTTD, MTTR, vulnerability remediation time, patch compliance
  - Risk quantification: FAIR model, annual loss expectancy (ALE)
  - Executive reporting: risk dashboards, board presentation, compliance status

**Практика:**

- STRIDE threat model: для 3 систем (web app, API, k8s cluster) — DFD + threat list + mitigations, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Zero Trust design: для enterprise (500 users) — identity, device, network, application layers, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Cloud security baseline: AWS account hardening — SG/NACL/IAM/GuardDuty/CloudTrail, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- PAM: HashiCorp Vault setup for credential management, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Security architecture review: аудит 2 систем (on-prem + cloud) — найти gaps, рекомендовать controls, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- Security Architecture Blueprint: enterprise (3 DC, multi-cloud) — segmentation, IAM, detection, IR, resilience, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Roadmap внедрения controls на 12 месяцев с приоритизацией и зависимостями, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Threat model report: STRIDE/LINDDUN для 3 систем + mitigation backlog, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Enterprise Security Architecture Blueprint: полная архитектура (segmentation, IAM, detection, IR, resilience, compliance mapping) + threat model + implementation roadmap, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** NIST SP 800-207, NIST CSF 2.0, SABSA, AWS Well-Architected Security Pillar, Microsoft CAF Security, *Enterprise Security Architecture*, *Threat Modeling* (Shostack)

---

### 6. Leadership и управление Red Team программой (50 часов) 

**Время:** 50 часов

**Что изучать:**

- **Программное планирование:**
  - Формирование Red Team backlog: приоритизация по бизнес-риску, coverage gaps, stakeholder requests
  - Квартальный план проверок: цели, scope, ресурсы, dependency и timing
  - Бюджетирование: tools, infrastructure, licensing, contractor/vendor costs, ROI justification
  - Staffing: требуемые роли (operators, infrastructure, analysts), навыки и growth plan, hiring criteria
  - Tooling selection: build vs buy, коммерческие (Cobalt Strike, C2) vs OSS, EDR interoperability

- **KPI программы:**
  - Coverage: процент ATT&CK techniques протестированных, gap analysis
  - Remediation adoption: как быстро blue/eng teams закрывают найденные пробелы
  - Business risk reduction: количественная оценка снижения экспозиции (FAIR)
  - Quality: false positive rate, severity distribution, actionability of findings
  - Efficiency: cost per test, time per operation, analyst effort per finding

- **Коммуникации с C-level:**
  - Decision-ready материалы: executive summary, risk posture, prioriteed recommendations
  - Презентация результатов совету директоров: темы, визуализация, narrative
  - Согласование бюджета и ресурсов с бизнесом: business case, KPIs, expected value
  - Кризисная коммуникация: при экстренных событиях, escalation protocols

- **Командное развитие:**
  - Coaching: индивидуальные планы, mentoring, technical growth paths
  - Развитие до senior operators: создание opportunities, autonomous operations, knowledge transfer
  - Распределение задач: matching skill to task, stretch assignments, review loop
  - Performance management: цели, регулярная обратная связь, доска оценки навыков

- **Операционная дисциплина:**
  - Quality assurance: стандарты операций, checklists, peer review процессов
  - Knowledge management: playbooks, lessons learned, операционные стандарты
  - Compliance: соблюдение legal, ethical, regulatory границ операций
  - Continuous improvement: ретроспективы, внедрение улучшений в playbooks

**Проекты:**

- 2 квартальных плана Red Team программы: цели, scope, ресурсы, KPI, budget estimate, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Mock steering committee: презентация программы и защита перед board, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Staffing & development plan: роли, навыки, hiring criteria, growth path для команды 5 человек, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Red Team Program Operating Model: governance, cadence, KPI, budgets, staffing, коммуникационные шаблоны, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** NIST CSF 2.0, ISACA governance guides, CISO playbooks, FAIR analysis, *The Manager's Path* (Fournier)

---

### 7. Interview & Career Mastery (60 часов) 

**Время:** 60 часов

**Что изучать:**

- **Interview prep по направлениям:**
  - Архитектурные кейсы: system design (network/AD/cloud), trade-offs, capacity planning
  - Offensive кейсы: как бы ты атаковал, threat modeling на лету, TTP выбор
  - Detection engineering: как построить детект, анализ логов, корреляции
  - Troubleshooting на живых сетях: диагностика, гипотезы, root cause
  - Бихевиоральные: STAR (Situation, Task, Action, Result), конфликтные сценарии, лидерство

- **Storytelling по проектам:**
  - Структура: context → challenge → action → measurable result → reflection
  - Квантификация: числа, проценты, временные рамки, бизнес-эффект
  - Адаптация к роли: акцент на offensive/detection/architecture в зависимости от типа
  - WOW-эффект: демо, интерактивные примеры, живой код, метрики

- **Зарплатные переговоры:**
  - Позиционирование экспертизы: уровни (mid/senior/lead), компенсационные опции
  - Рынок: benchmarks (levels.fyi, Glassdoor), внешние офферы, конкурентные переговоры
  - Package negotiation: base, bonus, equity, benefits, remote, обучение
  - Тактика: раскрытие цифр, BATNA, декомпозиция зачем/сколько, тайминг

- **Публичный профиль:**
  - GitHub: качественные репозитории, документация, benchmarks, README
  - Статьи: технические deep dives (Habr/Medium/eng матерials), регулярность
  - Доклады: конференции, meetups, технический контент для community
  - Mentorship: публичное менторство, вклад в обучение сообщества

- **Выбор ролей:**
  - Red Team Lead: операционное лидерство, операции, team management
  - Principal Network Security Engineer: архитектура, сетевые стандарты, EOL стратегии
  - Security Architect: enterprise-level дизайн, governance, compliance
  - Сравнение: ответственность, карьерный рост, компенсация, stress/сore

- **Карьерные треки:**
  - IC (Individual Contributor) vs Management: путь, оплата, автономия, влияние
  - Специализация depth vs breadth: эксперт vs универсал в enterprise
  - Переход между red/blue: взаимная ценность, типичные переходы, skills transfer

**Практика:**

- 20 mock интервью (technical + leadership) с разбором, записью и рефлексией, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Зарплатный negotiation script: подготовка и репетиция 5 сценариев, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Публичная презентация: доклад по flagship проекту перед аудиторией, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Case study: разбор 10 реальных интервью кейсов (network/AD/cloud/arch), с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- 20 mock interviews (technical + leadership) с письменными разборами и планом улучшений, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Обновление портфолио: каждый flagship проект с измеримыми результатами, репозиторий, документацией, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Personal career roadmap: целевые роли, gap analysis по навыкам, timeline, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Offer Conversion Engine: пакет материалов (resume, LinkedIn, interview scripts, negotiation playbook, portfolio) для выхода на senior/lead офферы, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Результаты:** интервью, оффер, публичный контент

**📚 Ресурсы:** Interviewing.io guides, levels.fyi, security leadership podcasts, *Cracking the PM Interview* (адаптация), negotiation workshops

---

### 8. Финальная интеграция и APT эмуляция (60 часов) 

**Время:** 60 часов

**Что изучать:**

- **Интеграция компетенций:**
  - Сборка всех навыков в единую операцию: network/offensive/cloud/detection/architecture
  - Cross-domain execution: network attack → AD compromise → cloud pivot → detection validation
  - Управление всей операцией: планирование, ресурсы, тайминг, дефис
  - Координация с blue team: detection response, TTPS, deconfliction

- **Планирование adversary simulation:**
  - Моделирование APT-групп: TTP fingerprinting (APT29, APT28, FIN7, Lazarus), attribution techniques
  - Сценарии: ransomware, data exfil, long-term persistence, supply chain, cloud compromise
  - Целевые objectives: кража данных, доступ к критическим системам, повреждение
  - Timeframe: длительные (multi-week) операции, постоянный доступ

- **Глубокая документация:**
  - Technical timeline: точное пошаговое описание атаки (что, когда, как, какие сигнатуры)
  - Executive summary: бизнес-риск, критичность, рекомендуемые действия
  - Remediation plan: конкретные fixes, owners, deadlines, verification
  - MITRE ATT&CK mapping: техники, методы, coverage gaps
  - Detection recommendations: как улучшить видимость и время реакции

- **Пост-операционный разбор:**
  - Lessons learned: что работало, что не работало, что улучшить
  - Playbook improvement: обновление operation templates на основе опыта
  - Blue team feedback loop: передача знаний, совместная работа над устранением
  - Метрики операции: TTD/TTR, goals achieved, coverage, dwell time

- **Итоговая подготовка к роли:**
  - Синтез: portfolio демонстрация полного цикла ИБ operaций
  - Leadership readiness: управление операциями, коммуникация на всех уровнях
  - Continuous improvement: личный growth plan, следующие цели после программы

**Практика:**

- Полная 4-недельная Red Team кампания против hardened enterprise lab, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Создание полного отчёта: technical timeline + executive summary + remediation + ATT&CK mapping, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Purple remediation cycle: blue team закрывает gaps → retest → валидация устранения, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.
- Post-op debrief: lessons learned + improvement план для следующих операций, с обязательной фиксацией шагов, команд, ожидаемых артефактов и разбором типичных ошибок после выполнения.

**Проекты:**

- Итоговая 4-недельная Red Team кампания против hardened enterprise lab: планирование, исполнение, отчетность, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Отчёты для CISO, Blue Team и инженерных лидов: три разных view на одну операцию, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Post-incident playbook: обновлённые шаблоны на основе реального опыта, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**🌟 FLAGSHIP PROJECT (выбрать один):**

- Full APT Simulation против hardened enterprise lab: initial access → AD domination → cloud pivot → exfiltration simulation → purple remediation cycle, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

**📚 Ресурсы:** MITRE ATT&CK, Caldera docs, Mandiant APT reports, enterprise IR playbooks, Atomic Red Team

---

## 📚 Ресурсы

### Сертификации (жесткий дедлайн-план)

- Семестр 3: CompTIA Network+, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Семестр 4: CompTIA Security+, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Семестр 5: OSCP (критично), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Семестр 6: CRTO (или CRTP, если CRTO недоступен), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Семестр 7: OSEP, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Семестр 8: AWS Security Specialty, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

### Обязательные лаборатории

- Detection Lab: AD + Sysmon + EDR + SIEM, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- HackTheBox Pro Labs: Offshore, Dante, RastaLabs, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- AWS/Azure pentest labs в sandbox-среде, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Physical lab: MikroTik + Flipper Zero + ESP32 + изолированный Wi-Fi сегмент, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

### Измеримые карьерные KPI

- 120+ машин HackTheBox/TryHackMe/VulnLab, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- 50+ принятых PR в OSS, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- 10+ технических статей (Habr/Medium), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- 10+ технических видео с разбором flagship-проектов, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- 8–12 крупных проектов в портфолио (минимум 6 flagship), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

### Сети

- *TCP/IP Illustrated* том 1 (Stevens), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- *Computer Networks* (Tanenbaum), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- *Unix Network Programming* (Stevens), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- *Computer Networking: A Top-Down Approach* (Kurose & Ross), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Cisco CCNA/CCNP Materials, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- David Bombal YouTube, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- NetworkChuck YouTube, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

### Безопасность

- *The Practice of Network Security Monitoring* (Bejtlich), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- *Penetration Testing* (Weidman), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- *The Art of Exploitation* (Jon Erickson), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- PTES (Penetration Testing Execution Standard), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- OWASP Top 10, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- HackTheBox Academy, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

### Linux

- *The Linux Command Line* (Shotts), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- *How Linux Works* (Ward), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- *Linux Programming Interface* (Kerrisk), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- OSTEP (бесплатно онлайн), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

### Go

- *The Go Programming Language* (Donovan & Kernighan), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Go by Example, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

### Python

- *Automate the Boring Stuff*, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Real Python, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

### C/C++

- *The C Programming Language* (K&R), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- *Effective Modern C++*, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- *C++ Concurrency in Action*, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

### Инструменты (практика)

- Wireshark, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- tcpdump, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Nmap, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Metasploit, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Suricata, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Zeek, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- BetterCAP, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Containerlab, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- GNS3, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Eve-NG, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- VirtualBox/VMware, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Docker, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- HackTheBox, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- TryHackMe, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- VulnHub, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

### Варгеймы

- OverTheWire (Bandit, Narnia, Behemoth, Drifter), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- SmashTheStack, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Root Me, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

---

## 🎯 Финальная цель

К концу шестого семестра ты претендуешь на позиции:

- Network Security Engineer, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Security Operations Center (SOC) Analyst, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Network Penetration Tester, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- **Red Team Specialist / Red Team Lead** ⭐, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Security Architect (entry-level), с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.
- Infrastructure Security Specialist, с измеримыми критериями готовности, документацией архитектурных решений и итоговым техническим разбором результатов.

Ты понимаешь как работают сети на всех уровнях OSI, умеешь проектировать безопасные сети, писать сетевые сервисы на C/C++/Go, проводить пентест, настраивать IDS/IPS, обнаруживать и анализировать атаки, **моделировать атаки APT-групп, обходить EDR, работать с C2-инфраструктурой**, и у тебя есть портфолио проектов и вкладов в open source.

**Red Team в РФ сейчас — это:**

- Зарплата сеньора: **400-700 тыс руб** (в 1.5-2 раза выше обычного пентеста)
- Спрос: очень высокий (крупные банки, нефтегаз, ритейл, телеком)
- Дефицит: реально крутых Red Team спецов единицы

Ты думаешь не «как найти работу», а «в какую компанию пойти» — потому что у тебя есть фундамент, навыки и уверенность.

С таким расширением твой Network Roadmap становится дорожной картой к позиции **Red Team Lead за 3-4 года**.

---

## 📚 Red Team Books (обязательны к прочтению)

| Книга | Темы |
|-------|------|
| *Red Team Field Manual (RTFM)* | Основы, шпаргалка |
| *Operator Handbook* | Еще шпаргалка |
| *Hacking: The Art of Exploitation* | Binary exploitation |
| *Evading EDR* (2024, Matt Hand) | **Святая книга по EDR evasion** |
| *Certified Pre-Owned* (AD атаки) | Бесплатно онлайн, библия AD |
| *The Hardware Hacking Handbook* | Для physical и embedded |

---

## 💎 Red Team Certification Path

После изучения материала:

1. **OSCP** (Offensive Security) — базовый пентест (сдать нужно обязательно)
2. **CRTP** (Certified Red Team Professional) — AD атаки, практический
3. **OSEP** (Evasion and Persistence) — EDR bypass, продвинутый
4. **OSED** (Exploit Development) — эксплойты на ассемблере (если нравится embedded)

---

## 🛠️ Hardware для Red Team (бюджетно)

```yaml
Обязательно (10-15 тыс руб):
  - VPS (DigitalOcean/VScale): ~1000 руб/мес
  - Flipper Zero: 15k руб (один раз)
  - ESP32-S3 dev board: 2000 руб

Для физического тестирования:
  - Bash Bunny / Rubber Ducky: 15k руб
  - Proxmark3 Easy: 6k руб

Для WiFi:
  - Alfa AWUS036ACH (чипсет Realtek) : 5k руб (packet injection)
  - TP-Link TL-WN722N v1: 2k руб (дешевый вариант)
```

---

## 🎯 Финальный Red Team проект (вместо обычного в семестре 6)

**"Full Red Team Simulation"**

**Цель:** Сымитировать APT-группу (типа APT29) против собственной лаборатории.

**Лаборатория:**

- 5 VM: AD, клиент Windows, сервер приложений, Firewall, SOC с Wazuh
- EDR: установить Microsoft Defender for Endpoint (пробная версия 90 дней)

**За 4 недели пройти:**

```text
Week 1: OSINT + Phishing → Initial Access
Week 2: EDR Evasion (direct syscalls) → C2 установка
Week 3: AD атаки (Kerberoasting + Golden Ticket) → DA доступ
Week 4: Exfiltration данных (DNS tunneling) + отчет для Blue Team
```

**Документация:**

- Report for management (executive summary)
- Report for Blue Team (TTD, TTR, MITRE ATT&CK mapping)
- Timeline of attack (что когда делалось)
- Code of custom tools (GitHub)

---

## 🎯 Лаборатории для практики Red Team

### Бесплатные (прогрессия по сложности):

1. **HackTheBox — Pro Labs** (Offshore, Dante, RastaLabs) — имитация реальной сети
2. **TryHackMe** — Red Team track (много теории)
3. **VulnLab** — AD лаборатория
4. **Detection Lab** (создать свою) — развернуть Windows + EDR и тестировать обход

### Платные (стоят того):

- **HTB Pro Lab — Offshore** (€40/мес) — лучшее приближение к реальному AD
- **Pentester Academy — Red Team Lab** ($39/мес) — много контента
- **GoDaddy AD Lab** (бесплатно с Azure credits) — можно на месяц

---

## ⚡ Важные напоминания

1. **Иди по порядку**: не перескакивай темы без практики
2. **Делай проекты**: теория без кода забывается
3. **Практикуй на реальных инструментах**: Wireshark, nmap, tcpdump, Metasploit, Suricata — каждый день
4. **Containerlab + GNS3 + Eve-NG**: моделируй сети (L3 и сервисы в CLab; полноценный L2/Cisco IOS — в GNS3/EVE-NG), настраивай оборудование
5. **VulnHub/HackTheBox/TryHackMe**: регулярно — минимум 2 лабы в неделю
6. **Читай RFC**: первоисточники важнее туториалов
7. **Веди заметки**: что изучил, какие проекты сделал
8. **Вклад в OSS**: начиная с четвёртого семестра — стабильно
9. **Алгоритмы**: 3–4 задачи LeetCode в неделю — параллельно основному
10. **Спи и отдыхай**: это марафон

Удачи в пути к цели! 🚀
	