---
## Front matter
title: "Отчёт по лабораторной работе 7"
subtitle: "Расширенные настройки межсетевого экрана"
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

Получить навыки настройки межсетевого экрана в Linux в части переадресации портов и настройки Masquerading.

# Процесс работы

## Создание пользовательской службы firewalld

### Копирование и подготовка файла службы

На сервере был создан файл пользовательской службы на основе стандартного описания SSH.  
После копирования и перехода в каталог `/etc/firewalld/services/` было просмотрено содержимое файла `ssh-custom.xml`.

![Просмотр оригинального файла ssh-custom.xml](Screenshot_1.png){ #fig:001 width=80% }

Структура файла представляет собой XML-документ:

- Заголовок XML включает версию и кодировку.
- Корневой элемент `<service>` объединяет параметры одной службы.
- Элемент `<short>` содержит краткое имя сервиса.
- Элемент `<description>` описывает назначение службы.
- В блоке `<port>` указан протокол и номер порта, который следует открыть.

### Модификация службы

Файл описания был открыт в редакторе.  
В него внесены изменения:

- порт изменён на 2022;
- описание дополнено, чтобы указать, что файл является модифицированным.

![Редактирование ssh-custom.xml](Screenshot_2.png){ #fig:002 width=80% }

### Проверка списка служб FirewallD

Выведен список всех доступных служб.  
Пользовательская служба ещё не отображается в перечне.

После перезагрузки конфигурации отображены обновлённые списки доступных и активных служб.  
Новая служба стала видимой, но пока не активной.

![Перезагрузка и проверка](Screenshot_3.png){ #fig:003 width=80% }

### Активация пользовательской службы

Служба была добавлена в набор активных.  
После этого она стала отображаться среди включённых.  
Затем служба была добавлена в постоянную конфигурацию и правила firewall были перезагружены.

## Настройка перенаправления порта

На сервере настроено перенаправление трафика с порта 2022 на стандартный порт SSH — 22.

![Настройка forward-port](Screenshot_4.png){ #fig:004 width=80% }

### Проверка работы с клиента

С клиентской машины выполнено подключение к серверу по SSH через порт 2022.  
Соединение успешно установлено, что подтверждает корректность перенаправления.

![Подключение к серверу через порт 2022](Screenshot_5.png){ #fig:005 width=80% }

## Включение IPv4 forwarding и маскарадинга

Проверено состояние параметра пересылки пакетов.  
Далее был включён IPv4 forwarding и создан конфигурационный файл, содержащий этот параметр.  
После применения настроек активирован маскарадинг и перезагружена конфигурация firewall.

![Настройка forwarding и masquerading](Screenshot_6.png){ #fig:006 width=80% }

## Подготовка конфигурации для Vagrant

В каталоге `/vagrant/provision/server/` создана структура директорий для хранения конфигураций FirewallD и системных параметров:

- директория для служб FirewallD;
- директория для файлов sysctl.

В эти каталоги были помещены:

- файл службы `ssh-custom.xml`;
- файл `90-forward.conf`.

Также был создан файл `firewall.sh`.

![Подготовка структуры каталогов](Screenshot_7.png){ #fig:007 width=80% }

### Содержимое файла firewall.sh

Скрипт включает операции копирования конфигураций, регистрации службы, настройки перенаправления порта, включения masquerading и восстановления контекстов SELinux.

![Содержимое firewall.sh](Screenshot_8.png){ #fig:008 width=80% }

# Итоги

## Вывод  
В процессе выполнения работы была создана пользовательская служба firewalld, настроено перенаправление порта, включено пересылание IPv4-пакетов и маскарадинг. Проверена работа SSH-доступа через новый порт. Конфигурационные файлы были вынесены в директорию Vagrant provisioning, а для автоматизации создан скрипт firewall.sh. Полученная конфигурация успешно функционирует и может применяться при повторном развёртывании виртуальной машины.

## Контрольные вопросы  

**1. Где хранятся пользовательские файлы firewalld?**  
В каталоге `/etc/firewalld/`, включая подкаталоги `services/`, `zones/` и другие, где пользователь может размещать собственные конфигурации.

**2. Какую строку надо включить в пользовательский файл службы, чтобы указать порт TCP 2022?**  
Строку с определением порта в XML-формате:  
`<port protocol="tcp" port="2022"/>`

**3. Какая команда позволяет вам перечислить все службы, доступные в настоящее время на вашем сервере?**  
Команда для вывода списка всех доступных служб:  
`firewall-cmd --get-services`

**4. В чем разница между трансляцией сетевых адресов (NAT) и маскарадингом (masquerading)?**  
NAT — общий механизм преобразования адресов и портов при прохождении пакетов между сетями.  
Маскарадинг — разновидность NAT, при которой внешний IP выбирается автоматически, обычно используется при динамическом IP-адресе интерфейса.

**5. Какая команда разрешает входящий трафик на порт 4404 и перенаправляет его в службу ssh по IP-адресу 10.0.0.10?**  
Команда перенаправления входящего трафика:  
`firewall-cmd --add-forward-port=port=4404:proto=tcp:toaddr=10.0.0.10:toport=22`

**6. Какая команда используется для включения маскарадинга IP-пакетов для всех пакетов, выходящих в зону public?**  
Команда активации маскарадинга:  
`firewall-cmd --zone=public --add-masquerade --permanent`
