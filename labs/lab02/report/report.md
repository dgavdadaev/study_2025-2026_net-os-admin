---
## Front matter
title: "Отчёт по лабораторной работе 2"
subtitle: "Настройка DNS-сервера"
author: "Авдадаев Джамал Геланиевич"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
lot: true # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: IBM Plex Serif
romanfont: IBM Plex Serif
sansfont: IBM Plex Sans
monofont: IBM Plex Mono
mathfont: STIX Two Math
mainfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
romanfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
sansfontoptions: Ligatures=Common,Ligatures=TeX,Scale=MatchLowercase,Scale=0.94
monofontoptions: Scale=MatchLowercase,Scale=0.94,FakeStretch=0.9
mathfontoptions:
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Введение

## Цель работы  

Приобретение практических навыков по установке и конфигурированию DNSсервера, усвоение принципов работы системы доменных имён.

# Процесс работы

## Проверка работы DNS-клиента

### Запрос DNS-записи *www.yandex.ru* с помощью dig

На виртуальной машине был выполнен запрос `dig www.yandex.ru`.  
Команда обратилась к DNS-серверу, указанному в `/etc/resolv.conf`, и вернула:

- HEADER — параметры запроса, статус и флаги.
- QUESTION SECTION — доменное имя и тип записи.
- ANSWER SECTION — три A-записи `www.yandex.ru`.
- SERVER — адрес DNS-сервера, который обслужил запрос.
- Query time — время обработки.
- MSG SIZE — размер пакета.

![Результат запроса dig www.yandex.ru](Screenshot_1.png){ #fig:001 width=80% }

## Анализ конфигурационных файлов DNS

### Файл `/etc/resolv.conf`

Файл содержит:

- домен поиска: `dgavdadaev.net`
- DNS-сервер: `10.0.2.3`

### Файл `/etc/named.conf`

Основной конфигурационный файл BIND.  
Содержит:

- параметры интерфейсов (`listen-on`)
- пути к служебным файлам
- настройку рекурсии
- список разрешённых клиентов (`allow-query`)

![Файл named.conf](Screenshot_2.png){ #fig:002 width=80% }

### Файл `/var/named/named.ca`

Это корневой файл DNS.  
Он включает:

- NS-записи корневых серверов
- их IPv4 и IPv6-адреса
- дату последнего обновления корневой зоны

![Файл named.ca](Screenshot_3.png){ #fig:003 width=80% }

### Файлы `/var/named/named.localhost` и `/var/named/named.loopback`

Оба файла содержат:

- SOA-запись
- A-запись 127.0.0.1
- AAAA-запись ::1  
- PTR-запись `localhost`

![Файлы зон localhost](Screenshot_4.png){ #fig:004 width=80% }

### Сравнение DNS-запросов через внешний и локальный сервер

Были выполнены два запроса:

1. `dig www.yandex.ru` — использован внешний DNS.
2. `dig @127.0.0.1 www.yandex.ru` — использован локальный BIND.

Отличия:

- первый запрос обрабатывается провайдерским DNS
- второй — локальным сервером
- локальный сервер сначала выдал timeout, затем успешно вернул те же A-записи
- при обращении к 127.0.0.1 время ответа выше из-за отсутствия кэша

![Сравнение dig через внешний и локальный DNS](Screenshot_5.png){ #fig:005 width=80% }

## Настройка DNS по умолчанию

Интерфейс `eth0` был настроен так, чтобы использовать локальный DNS-сервер.  
После изменения параметров NetworkManager в `/etc/resolv.conf` появился DNS-сервер `127.0.0.1`.

![Изменения в настройках NetworkManager](Screenshot_6.png){ #fig:006 width=80% }

В `/etc/named.conf` внесены изменения:

- разрешено прослушивание всех IPv4-интерфейсов

- разрешены запросы от сети `192.168.0.0/16`

![Изменённый named.conf](Screenshot_7.png){ #fig:007 width=80% }

Для разрешения DNS были добавлены правила:

- разрешение сервиса `dns` на текущий сеанс
- разрешение сервиса `dns` на постоянной основе

Команда `lsof` показала, что процесс `named` слушает UDP-порт 53, что подтверждает корректную работу сервера.

![Результат lsof | grep UDP](Screenshot_8.png){ #fig:008 width=80% }

## Конфигурирование первичного DNS-сервера

### Создание собственного файла зон

Был скопирован шаблон `named.rfc1912.zones` и переименован в файл доменной зоны

В конфигурации присутствуют две зоны:

- прямая зона `dgavdadaev.net`
- обратная зона `1.168.192.in-addr.arpa`

![Файл user.net](Screenshot_9.png){ #fig:010 width=80% }

### Настройка прямой DNS-зоны

В каталоге `/var/named/master/fz/` создан файл прямой зоны `dgavdadaev.net`.  
В нём определены:

- SOA-запись с серийным номером в формате ГГГГММДДВВ
- NS-запись
- A-записи сервера и имени `ns`

![Файл прямой зоны](Screenshot_10.png){ #fig:011 width=80% }

### Настройка обратной DNS-зоны

В каталоге `/var/named/master/rz/` создан файл обратной зоны `192.168.1`.  
В файле определены:

- SOA-запись
- NS-запись
- PTR-записи, соответствующие IP-адресам

![Файл обратной зоны](Screenshot_11.png){ #fig:012 width=80% }

После правки конфигурации DNS-сервер был перезапущен.  
Служба запущена и работает корректно:

![Статус службы named](Screenshot_12.png){ #fig:013 width=80% }

### Проверка работы DNS-зоны

Запрос вида: `dig ns.dgavdadaev.net`

вернул A-запись, что подтверждает корректность настройки прямой зоны.

![Результат dig ns.dgavdadaev.net](Screenshot_13.png){ #fig:014 width=80% }

Для анализа зоны использовались команды:

- `host -l dgavdadaev.net`
- `host -a dgavdadaev.net`
- `host -t A dgavdadaev.net`
- `host -t PTR 192.168.1.1`

Все результаты корректны — зона обслуживается сервером, записи совпадают с данными файла зон.

![Проверка командой host](Screenshot_14.png){ #fig:015 width=80% }

## Подготовка окружения Vagrant для автоматического развёртывания DNS

В каталоге `/vagrant` создана структура для provisioning:

- директория `dns/etc/named`
- директория `dns/var/named/master`

В неё были перенесены:

- конфигурационные файлы из `/etc/named/`
- файлы зон из `/var/named/master/`

![Копирование файлов в /vagrant](Screenshot_15.png){ #fig:016 width=80% }

Создан исполняемый файл `dns.sh`

![Скрипт dns.sh](Screenshot_16.png){ #fig:017 width=80% }

# Итоги

## Вывод

В результате была настроена первичная DNS-зона и обратная зона, скорректированы конфигурационные файлы BIND, восстановлены права и метки SELinux, а DNS-служба успешно перезапущена. Проверка с помощью `dig` и `host` подтвердила корректность работы прямых и обратных записей. Конфигурация перенесена в каталог Vagrant для последующего автоматизированного развёртывания.

## Контрольные вопросы

**1. Что такое DNS?**
Иерархическая система, преобразующая доменные имена в IP-адреса и обратно.

**2. Каково назначение кэширующего DNS-сервера?**
Сохранять результаты DNS-запросов, ускоряя последующие обращения и уменьшая нагрузку на внешние серверы.

**3. Чем отличается прямая зона от обратной?**
Прямая зона переводит доменные имена в IP-адреса, обратная — IP-адреса в доменные имена.

**4. Где располагаются настройки DNS-сервера?**

* `/etc/named.conf` — основной конфигурационный файл BIND.
* `/etc/named/` — файлы включаемых зон и шаблонов.
* `/var/named/` — рабочие файлы прямых и обратных зон.

**5. Что указывается в файле resolv.conf?**
DNS-серверы, домены поиска и параметры резолвера.

**6. Какие типы записей есть в DNS?**
A — IPv4-адрес; AAAA — IPv6-адрес; NS — серверы зоны; SOA — начало зоны; CNAME — алиасы; MX — почтовые серверы; PTR — обратное разрешение.

**7. Для чего используется домен in-addr.arpa?**
Для построения обратных DNS-зон IPv4.

**8. Для чего нужен демон named?**
Запускает службу DNS, обрабатывает запросы, обслуживает зоны и обновляет кэш.

**9. В чём разница между master-сервером и slave-сервером?**
Master хранит оригинальные файлы зон; slave получает их копии по механизму зон-трансфера.

**10. Какие параметры отвечают за время обновления зоны?**
Поля SOA-записи: serial, refresh, retry, expire, minimum.

**11. Как защитить зону от скачивания?**
Ограничить зон-трансфер с помощью `allow-transfer` и настроить ACL.

**12. Какая запись используется для почтовых серверов?**
MX.

**13. Как протестировать работу DNS-сервера?**
Командами `dig`, `host`, `nslookup`.

**14. Как управлять службой?**
`systemctl start|stop|restart|status имя_службы`.

**15. Как посмотреть отладочную информацию сервиса?**
`journalctl -u имя_службы` или `systemctl status`.

**16. Где хранится отладочная информация?**
В системном журнале, доступном через `journalctl`.

**17. Как посмотреть, какие файлы использует процесс?**
Через `lsof -p PID` или `lsof | grep имя_процесса`.

**18. Примеры изменения сетевого соединения с помощью nmcli:**

* `nmcli connection show`
* `nmcli connection edit eth0`
* `nmcli connection modify eth0 ipv4.dns 127.0.0.1`

**19. Что такое SELinux?**
Механизм мандатного контроля доступа, обеспечивающий дополнительную безопасность системы.

**20. Что такое контекст SELinux?**
Метка безопасности, определяющая права процесса или файла.

**21. Как восстановить контекст SELinux?**
Командой `restorecon -R путь`.

**22. Как создать правила SELinux на основе логов?**
Использовать `audit2allow` для генерации модулей политики.

**23. Что такое булевый переключатель SELinux?**
Настройки, включающие или отключающие определённые функции политики.

**24. Как посмотреть список переключателей?**
`getsebool -a`.

**25. Как изменить значение переключателя?**
`setsebool имя on|off` или `setsebool -P` для постоянного изменения.
