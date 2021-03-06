# Сетевые утилиты
---
### NetCat
---
**Синтаксис**
 > *ncat [OPTIONS...] [hostname] [port]*



**Описание**
     Netcat, она же nc — простая, но очень удобная консольная утилита, предназначенная для работы с портами и TCP и/или UDP трафиком между узлами сети.

**Полезные ключи:**

 - `-l` — указать какой порт будет слушать nc для приёма входящих соединений;
 - `-n` — не использовать преобразование IP в имена хостов;
 - `-k` — продолжать принимать соединения на указанном с ключём -l порту после того, как завершится активная сессия;
 - `-s` — указать локальный IP интерфейса, который будет использоваться для отправки пакетов;
 - `-v` — использовать подробный режим;
 - `-u` — использовать UDP вместо TCP, который используется по-умолчанию;
 - `-w` — таймаут, после которого сессия будет закрыта, если не активна;
 - `-X` — указать тип прокси, который использоваться, иначе — используется SOCK5;
 - `-x` — адрес:порт прокси-сервера, через который необходимо отправлять трафик;

**Пример использования**  

На стороне «сервера» (машина, которая будет принимать трафик) запускаем nc для приёма входящих пакетов на порт `12345`:

    $ nc -l 12345 > ../temp.log

На стороне «клиента» — выполняем чтение файла и пересылаем содержимое на «сервер»:

    $ tail -f /var/log/zabbix/zabbix_agentd.log | nc 10.249.140.239 12345

Переслать файл можно так. На стороне сервера — запустим `nc` для приёма соединений на порту `12346` и укажем выводить содержимое принятого файла в локальный файл:

    # nc -l 12346 > post-install.log

На стороне клиента — запускаем nc и считываем в него содержимое файла `post-install.log`:


    # nc 10.249.140.239 12346 < post-install.log

Интересная возможность — сканирование портов, хотя и далеко от возможностей `nmap`:

    # nc -z -v 10.249.140.239 8080-8085
    nc: connect to 10.249.140.239 port 8080 (tcp) failed: Connection refused
    Connection to 10.249.140.239 8081 port [tcp/tproxy] succeeded!
    nc: connect to 10.249.140.239 port 8082 (tcp) failed: Connection refused
    nc: connect to 10.249.140.239 port 8083 (tcp) failed: Connection refused
    nc: connect to 10.249.140.239 port 8084 (tcp) failed: Connection refused
    nc: connect to 10.249.140.239 port 8085 (tcp) failed: Connection refused

---
###NMap
---
nmap - Утилита для исследования сети и сканер портов

**Синтаксис**
 > *nmap [Scan Type...] [Options] {target specification}*

 **Описание**
       *Nmap (“Network Mapper”)* это утилита с открытым исходным кодом для исследования сети и проверки безопасности. Она была разработана для быстрого сканирования больших сетей, хотя прекрасно справляется и с единичными целями. Nmap использует сырые IP пакеты оригинальными способами, чтобы определить какие хосты доступны в сети, какие службы (название приложения и версию) они предлагают, какие операционные системы (и версии ОС) они используют, какие типы пакетных фильтров/брандмауэров используются и еще дюжины других характеристик. В тот время как Nmap обычно используется для проверки безопасности, многие сетевые и системные администраторы находят ее полезной для обычных задач, таких как контролирование структуры сети, управление расписаниями запуска служб и учет времени работы хоста или службы.

**Полезные ключи:**

 - `-sT` (scan TCP) - использовать метод TCP connect(). Наиболее общий метод сканирования TCP-портов.
 - `-sS` (scan SYN) - использовать метод TCP SYN. Этот метод часто называют "полуоткрытым" сканированием, поскольку при этом полное TCP-соединение с портом сканируемой машины не устанавливается.
 - `-sF,-sX,-sN` (scan FIN, scan Xmas, scan NULL) - "невидимое" FIN, Xmas Tree и NULL-сканирование. Эти методы используются в случае, если SYN-сканирование по каким-либо причинам оказалось неработоспособным.
 - `-sP` (scan Ping) - ping-"сканирование". Иногда вам необходимо лишь узнать адреса активных хостов в сканируемой сети. Nmap может сделать это, послав ICMP-сообщение "запрос эха" на каждый IP-адрес, указанный вами.
 - `-sV` (scan Version) - включение режима определения версий служб, за которыми закреплены сканируемые порты.
 - `-sU` (scan UDP) - сканировать UDP-порты.
 - `-sO` (scan Open protocol) - сканирование протоколов IP.
 - `-sI`(scan Idle) - позволяет произвести абсолютно невидимое сканирование портов. Атакующий может просканировать цель, не посылая при этом пакетов от своего IP-адреса.

**Пример использования**  

Запустим командой:

    # nmap scanme.nmap.org
    Interesting ports on scanme.nmap.org (74.207.244.221):
    Not shown: 998 closed ports
    PORT STATE SERVICE
    22/tcp open ssh
    80/tcp open http

Ничего необычного, ssh на стандартном порту и http на 80. Nmap распознаёт следующие состояния портов: open, filtered, closed, или unfiltered. Open означает, что приложение на целевой машине готово для принятия пакетов на этот порт. Filtered означает, что брандмауэр, фильтр, или что-то другое в сети блокирует порт, так что Nmap не может определить, является ли порт открытым или закрытым. Closed — не связанны в данный момент ни с каким приложением, но могут быть открыты в любой момент. Unfiltered порты отвечают на запросы Nmap, но нельзя определить, являются ли они открытыми или закрытыми.

    # nmap -sV example.com example2.com
    PORT STATE SERVICE VERSION
    22/tcp open ssh OpenSSH 5.3p1 Debian 3ubuntu7 (protocol 2.0)
    80/tcp open http Apache httpd 2.2.14 ((Ubuntu))
    Service Info: OS: Linux
Прогресс налицо — мы узнали точные названия используемых служб и даже их версии, а заодно узнали точно, какая операционная система стоит на сервере. С расшифровкой никаких проблем не возникает, все вполне понятно.

 Aгрессивное сканирование можно провести, указав ключ -A

    # nmap -A scanme.nmap.org

Nmap выведет очень много информации, я не стану приводить пример. Сканирование может длится довольно долго, занимая несколько минут.

В локальных сетях или просто имея на руках диапазон ip адресов, удобно проверить их на занятость с помощью ключей -sP:

    # nmap -sP 192.168.1.0/24

Cканирование проходит довольно быстро, так как по сути это обычный ping-тест, отвечает ли хост на ping. Следует учесть, что хост может не отвечать на ping из-за настроек фаерволла. Если нужный участок сети нельзя ограничить маской, можно указать диапазон адресов, с какого и по какой надо провести сканирование. Например, есть диапазон адресов с 192.168.1.2 до 192.168.1.5. Тогда выполним:

    # nmap -sP 192.168.1.2-5
    Host 192.168.1.2 is up (0.0023s latency)
    Host 192.168.1.3 is up (0.0015s latency)
    Host 192.168.1.4 is up (0.0018s latency)
    Host 192.168.1.5 is up (0.0026s latency)

В моем случае все ip в данный момент были в сети.


---
###wget
---
**Синтаксис**
 > *wget [option]... [URL]...*

 **Описание**
 *Wget* - это открыто распостраняемая утилита для загрузки файлов из интернет. Она поддерживает протоколы HTTP, HTTPS, и FTP, загрузку с серверов прокси по протоколу HTTP.

**Полезные ключи:**

 - `-np, –no-parent` — не подниматься выше начального адреса при рекурсивной загрузке.
 - `-r, –recursive` — включить рекурсивный просмотр каталогов и подкаталогов на удаленном сервере.
 - `-l <depth>, –level=<depth>` — определить максимальную глубину рекурсии равной depth при просмотре каталогов на удалённом сервере. По умолчанию depth=5.
 - `-np, –no-parent` — не переходить в родительский каталог во время поиска файлов. Это очень полезное свойство, поскольку оно
   гарантирует, что будут копироваться только те файлы, которые
   расположены ниже определённой иерархии.
 - `-A <acclist>, –accept <acclist>, -R <rejlist>, –reject <rejlist>` — список имен файлов, разделенных запятыми, которые следует (accept) или не следует (reject) загружать. Разрешается задание имен файлов по маске.
 - `-k, –convert-links` — превратить абсолютные ссылки в HTML документе в относительные ссылки. Преобразованию подвергнутся только те ссылки, которые указывают на реально загруженные страницы; остальные не будут преобразовываться. Заметим, что лишь в конце работы wget сможет узнать какие страницы были реально загружены. Следовательно, лишь в конце работы wget будет выполняться окончательное преобразование.
 - `–http-user=<user>, –http-passwd=<password>` — указать имя пользователя и пароль на HTTP-сервере.
 - `-H, –span-hosts` — разрешает посещать любые сервера, на которые есть ссылка.
 - `-p, –page-requisites` — загружать все файлы, которые нужны для отображения страниц HTML. Например: рисунки, звук, каскадные стили (CSS). По умолчанию такие файлы не загружаются. Параметры -r и -l, указанные вместе могут помочь, но т.к. wget не различает внешние и внутренние документы, то нет гарантии, что загрузится все требуемое.

**Пример использования**  

Предположим, вы хотите скопировать на свою машину удалeнный файл, с определeнным URL.

    $ wget http://google.com/
    --2015-10-01 02:23:40--  http://google.com/
    Распознаётся google.com (google.com)… 80.254.96.251, 80.254.96.216, 80.254.96.241, ...
    Подключение к google.com (google.com)|80.254.96.251|:80... соединение установлено.
    HTTP-запрос отправлен. Ожидание ответа... 302 Found
    Адрес: http://www.google.ru/?gfe_rd=cr&ei=_W4MVsbCDYzHYMWbjeAK [переход]
    --2015-10-01 02:23:41--  http://www.google.ru/?gfe_rd=cr&ei=_W4MVsbCDYzHYMWbjeAK
    Распознаётся www.google.ru (www.google.ru)… 80.254.96.231, 80.254.96.241, 80.254.96.247, ...
    Повторное использование соединения с google.com:80.
    HTTP-запрос отправлен. Ожидание ответа... 200 OK
    Длина: нет данных [text/html]
    Сохранение в: «index.html»

    index.html                 [                <=>          ] 154,43K  25,1KB/s   за 6,1s   

    2015-10-01 02:23:47 (25,1 KB/s) - «index.html» сохранён [158141]

Если сервер не отвечает можно использовать флаг `-w1h` и тогда попытки скачивания будут происходить раз в час.

Если соединение было прервано использование флага `-c` продолжит докачивание файла.

Чтобы выкачать файлы из списка, содержащего прямые ссылки:

    $ wget -i pupkinlist.txt

или

    $ wget --input-file=pupkinlist.txt

Здесь указывается только файл, в котором содержатся ссылки. Файл может так же быть HTML-страницей, в которой есть ссылки. Они будут выкачаны указанной выше командой.

Копирование сайта для локального просмотра (с заменой интернет-ссылок на локальные адреса скачанных страниц):

    $ wget -r -l0 -k http://www.vasyapupkin.com/

При этом будет включена рекурсивная выгрузка (ключ -r, –recursive), замена абсолютных ссылок в HTML документе в относительные ссылки (ключ -k) и определенна максимальная глубина рекурсии (ключ -l).

---
###ssh
---
**Синтаксис**
 > ssh [-1246AaCfGgKkMNnqsTtVvXxYy] [-b bind_address] [-c cipher_spec]
         [-D [bind_address:]port] [-E log_file] [-e escape_char] [-F configfile]
         [-I pkcs11] [-i identity_file] [-L [bind_address:]port:host:hostport]
         [-l login_name] [-m mac_spec] [-O ctl_cmd] [-o option] [-p port]
         [-Q cipher | cipher-auth | mac | kex | key | protocol-version]
         [-R [bind_address:]port:host:hostport] [-S ctl_path] [-W host:port]
         [-w local_tun[:remote_tun]] [user@]hostname [command]

 **Описание**
 ssh (SSH клиент) - это программа для регистрации на удаленной машине и выполнения на ней команд. Она предназначена для замены rlogin и rsh и осуществления безопасного шифрованного соединения между двумя независимыми компьютерами через незащищенную сеть. Подключения Х11 и произвольные ТСР/IP порты также могут быть перенаправлены через защищенный канал.
ssh подключается и регистрируется к указанному *имени_машины*. Пользователь должен доказать свою подлинность удаленной машине одним из нескольких способов в зависимости от используемой версии протокола.

**Флаги**

 - `-a` Отключает перенаправление соединения агента аутентификации
 - `-A` Включает перенаправление соединения агента аутентификации.
 - `-c blowfish|3des` Выбор пароля для шифрования сеанса. 3des (тройной-des) это кортеж вида encrypt-decrypt-encrypt с тремя различными ключами. Подразумевается, что это более безопасно, чем пароль des более не поддерживаемый в ssh. blowfish это быстрый блочный шифр, это кажется безопаснее и намного быстрее чем 3des. blowfish используется по умолчанию.
 - `-c cipher_spec` Дополнительно, для протокола версии 2, может быть указан через запятую список шифров в порядке их предпочтения. Для получения дополнительной информации смотри Ciphers.
 - `-e ch|^ch|none` Устанавливает управляющие символы для сеансов с псевдо-терминалом (по умолчанию: "~"). Управляющие символы распознаются только в начале строки. Управляющий символ следующий за точкой (".") завершит соединение, следующий за control-Z приостановит соединение и следующий сам за собой означает один управляющий символ. Установка символа в значение ``none'' отключает любые управляющие символы и делает сеанс полностью прозрачным.
 - `-f` Запросит ssh перейти в фоновый режим только перед выполнением команды. Это полезно если ssh собирается запросить пароль или парольную фразу, но пользователь хочет сделать это в фоновом режиме.
 - `-g` Позволяет удаленным машинам обращаться к локальным перенаправленным портам.
 - `-i` файл_идентификации Указывает файл из которого считывается идентификация (приватный ключ) для RSA или DSA аутентификации.
 - `-k` Отключает пересылку Kerberos tickets и AFS лексем.
 - `-l` имя_регистрации Указывает имя регистрации пользователя в системе, используемое для подключения к удаленной машине.
 - `-m mac_spec` Дополнительно, для протокола версии 2 может быть указан разделенный через запятую список MAC (код подлинности сообщения) алгоритмов в порядке предпочтения.
 - `-n` Перенаправляет стандартный ввод из /dev/null (фактически, предотвращает чтение из стандартного ввода). Это должно использоваться когда ssh выполняется в фоновом режиме.
 - `-N` Не выполнять удаленную команду. Это полезно если вы хотите только перенаправить порты (только протокол версии 2).
 - `-p` порт Порт для связи с удаленной машиной.
 - `-P` Использовать не привилегированные порты для исходящих соединений.
 - `-q` Тихий режим. Подавляет все предупреждения и диагностические сообщения. Будут отображены только фатальные ошибки.
 - `-T` Отменить переназначение терминала.
 - `-v` Режим отладки. Принудит ssh вывести отладочную информацию о его деятельности.
 - `-C` Включит сжатие всех данных (включая stdin, stdout, stderr и данные для перенаправленных Х11 и TCP/IP соединений).
 - `-L порт:машина:порт_машины` Определяет заданный порт на локальной (клиентской) машине который будет перенаправлен к заданной машине и порт на удаленной машине.
 - `-R порт:машина:порт_машины` Указывает заданный порт на удаленной машине (сервере) который будет перенаправлен к заданной машине и локальному порту.
 - `-4` Принуждает ssh использовать только IPv4 адреса.
 - `-6` Принуждает ssh использовать только IPv6 адреса.

---
###ping
---

**Синтаксис**

> ping [-aAbBdDfhLnOqrRUvV][-c count][-F flowlabel][-i interval][-I interface][-l preload][-m mark][-M pmtudisc_option][-N nodeinfo_option][-w  deadline][-W  timeout][-p  pattern][-Q  tos][-s packetsize][-S sndbuf][-t ttl][-T timestamp option][hop ...] destination

**Описание**
*Ping* — утилита для проверки соединений в сетях на основе TCP/IP

**Флаги**

 - `-t` Проверка связи с указанным узлом до прекращения.
 - `-n <число>` Число отправляемых запросов эха.
 - `-l <размер>` Размер буфера отправки.
 - `-f` Установка в пакете флага, запрещающего фрагментацию (только IPv4).
 - `-i <TTL>` Задание срока жизни пакетов.
