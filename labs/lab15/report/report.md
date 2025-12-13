---
## Front matter
title: "Отчёт по лабораторной работе 15"
subtitle: "Настройка сетевого журналирования"
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

Получение навыков по работе с журналами системных событий.

# Процесс работы


## Настройка сервера сетевого журнала

### Создание конфигурационного файла

Для включения сетевого приёма журналов по TCP-порту 514 на сервере был создан конфигурационный файл `netlog-server.conf`:

В файл внесены параметры загрузки модуля и запуска TCP-сервера:

```
$ModLoad imtcp
$InputTCPServerRun 514
```

![Создание конфигурации netlog-server.conf](Screenshot_1.png){ #fig:001 width=80% }

### Перезапуск rsyslog и проверка открытых портов

После обновления конфигурации служба rsyslog была перезапущена, а список слушающих портов проверен с помощью утилиты `lsof`.

![Порты rsyslog после запуска TCP-сервера](Screenshot_2.png){ #fig:002 width=80% }

Разрешение входящих TCP-соединений на порт 514.

## Настройка клиента сетевого журнала

### Создание файла конфигурации

На клиенте был создан файл `netlog-client.conf`.

В файл `netlog-client.conf` добавлена строка для отправки всех сообщений на сервер:

```
*.* @@server.dgavdadaev.net:514
```

![Конфигурация клиента netlog-client.conf](Screenshot_3.png){ #fig:003 width=80% }

Служба rsyslog была перезапущена для применения изменений.

## Просмотр логов

### Просмотр системного журнала на сервере

Для наблюдения за входящими записями использовался просмотр файла `/var/log/messages`.

![Пример входящих сообщений в /var/log/messages](Screenshot_4.png){ #fig:004 width=80% }

### Просмотр журналов через графическую утилиту

На сервере под пользователем `user` была запущена программа `gnome-system-monitor`.

![Просмотр ресурсов и журнала в gnome-system-monitor](Screenshot_5.png){ #fig:005 width=80% }

### Установка консольного просмотрщика lnav

Попытка установки пакета `lnav` из стандартных репозиториев завершилась ошибкой: пакет не найден.

![Ошибка установки lnav](Screenshot_6.png){ #fig:006 width=80% }

## Подготовка окружения Vagrant для автоконфигурации

### Сервер: экспорт конфигураций и создание скрипта

На сервере была создана директория `/vagrant/provision/server/netlog/etc/rsyslog.d`, в которую помещён файл `netlog-server.conf`.

![Создание структуры netlog для сервера](Screenshot_7.png){ #fig:007 width=80% }

Затем создан исполняемый файл `netlog.sh`, содержащий автоматизацию копирования конфигураций, настройки firewall и перезапуска rsyslog.

![Скрипт провижининга сервера](Screenshot_8.png){ #fig:008 width=80% }

### Клиент: экспорт конфигураций и создание скрипта

На клиенте была создана структура `/vagrant/provision/client/netlog/etc/rsyslog.d`, куда помещён файл `netlog-client.conf`.

![Экспорт конфигурации клиента](Screenshot_9.png){ #fig:009 width=80% }

Создан скрипт `netlog.sh`, выполняющий установку дополнительных утилит, копирование конфигураций и перезапуск rsyslog.

![Скрипт провижининга клиента](Screenshot_10.png){ #fig:010 width=80% }

# Итоги

## Вывод  

В ходе работы была выполнена настройка серверной и клиентской части rsyslog, обеспечена передача журналов по TCP-порту 514, а также подготовлены provisioning-скрипты для автоматизации конфигурации в среде Vagrant. Проверена корректная работа логирования, взаимодействие journald и rsyslog, а также механизмы просмотра журналов. Лабораторный стенд успешно функционирует и соответствует требованиям задания.

## Контрольные вопросы

**1. Какой модуль rsyslog вы должны использовать для приёма сообщений от journald?**  
Используется модуль **imjournal**, обеспечивающий прямую интеграцию rsyslog с journald.

**2. Как называется устаревший модуль, который можно использовать для включения приёма сообщений журнала в rsyslog?**  
Устаревший модуль — **imuxsock**, основанный на механизме сокетов `/dev/log`.

**3. Чтобы убедиться, что устаревший метод приёма сообщений из journald в rsyslog не используется, какой дополнительный параметр следует использовать?**  
Следует установить параметр **`IMJournalIgnorePreviousMessages="on"`**, что исключает подачу в rsyslog старых сообщений, перенаправленных journald через сокет.

**4. В каком конфигурационном файле содержатся настройки, которые позволяют вам настраивать работу журнала?**  
Основные параметры журнала определяются в файле **`/etc/rsyslog.conf`** и включаемых файлах в директории **`/etc/rsyslog.d/`**.

**5. Каким параметром управляется пересылка сообщений из journald в rsyslog?**  
Пересылку определяет параметр **`ForwardToSyslog=`** в файле *journald.conf*.

**6. Какой модуль rsyslog вы можете использовать для включения сообщений из файла журнала, не созданного rsyslog?**  
Для чтения произвольных файлов используется модуль **imfile**.

**7. Какой модуль rsyslog вам нужно использовать для пересылки сообщений в базу данных MariaDB?**  
Для работы с СУБД MariaDB применяется модуль **ommysql**.

**8. Какие две строки вам нужно включить в rsyslog.conf, чтобы позволить текущему журнальному серверу получать сообщения через TCP?**  

```
$ModLoad imtcp
$InputTCPServerRun 514
```

**9. Как настроить локальный брандмауэр, чтобы разрешить приём сообщений журнала через порт TCP 514?**  
Необходимо открыть порт:

```
firewall-cmd --add-port=514/tcp
firewall-cmd --add-port=514/tcp --permanent
```

И затем перезагрузить правила:

```
firewall-cmd --reload
```
