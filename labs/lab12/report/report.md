---
## Front matter
title: "Отчёт по лабораторной работе 12"
subtitle: "Синхронизация времени"
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

Получение навыков по управлению системным временем и настройке синхронизации времени.

# Процесс работы

## Проверка текущих параметров времени

### Команда timedatectl

На сервере и клиенте были просмотрены параметры системного времени с помощью команды `timedatectl`.  
Обе машины работают в часовой зоне **UTC (UTC +0000)**.  
Сетевая синхронизация активна, служба NTP работает.

Результат выполнения команды на сервере:

![Параметры времени на сервере](Screenshot_1.png){ #fig:101 width=80% }

Результат на клиенте:

![Параметры времени на клиенте](Screenshot_2.png){ #fig:102 width=80% }

### Просмотр текущего системного времени

Системное время выводилось командой `date`.  
Оно совпадает с показаниями timedatectl.

### Просмотр аппаратного времени

Команда `hwclock` показала текущее аппаратное время, соответствующее системному.

## Управление синхронизацией времени

## Проверка источников времени

Команда `chronyc sources` показала список доступных NTP-источников:

![Источники времени сервера](Screenshot_3.png){ #fig:104 width=80% }

**Пояснение результатов:**

- Символ `^*` — активный источник, используемый для синхронизации.
- `LastRx` — время с момента последнего получения пакета.
- `Last sample` — точность последней коррекции.
- `Stratum` — уровень удалённости NTP-сервера.

До настройки клиент использовал внешние NTP-серверы:

![Источники времени клиента](Screenshot_4.png){ #fig:105 width=80% }

### Разрешение доступа внутренней сети

В файл `/etc/chrony.conf` на сервере добавлена строка разрешения подсети:

```
allow 192.168.0.0/16
```

![Правка chrony.conf на сервере](Screenshot_5.png){ #fig:106 width=80% }

После внесения изменений служба синхронизации была перезапущена.

### Настройка межсетевого экрана

Для разрешения NTP-трафика на сервере были добавлены правила Firewall:

```
firewall-cmd --add-service=ntp --permanent
firewall-cmd --reload
```

### Настройка NTP-клиента

На клиенте в файле `/etc/chrony.conf` были удалены все старые директивы `server`  
и добавлена строка для синхронизации с локальным сервером:

```
server server.dgavdadaev.net iburst
```

![Редактирование chrony.conf на клиенте](Screenshot_6.png){ #fig:107 width=80% }

После обновления конфигурации служба была перезапущена.

### Проверка работы NTP после настройки

Команда `chronyc sources` показала, что клиент теперь использует локальный сервер:

![Источники времени после настройки](Screenshot_7.png){ #fig:108 width=80% }

## Настройка внутреннего окружения виртуальных машин

В каталоге `/vagrant/provision/server` был создан каталог `ntp/etc`, куда скопирован актуальный конфигурационный файл:

```
mkdir -p /vagrant/provision/server/ntp/etc
cp -R /etc/chrony.conf /vagrant/provision/server/ntp/etc/
```

Был создан и заполнен provisioning-скрипт `ntp.sh`:

![Скрипт ntp.sh сервера](Screenshot_8.png){ #fig:109 width=80% }

Аналогичная подготовка была выполнена в каталоге клиента:

```
mkdir -p /vagrant/provision/client/ntp/etc
cp -R /etc/chrony.conf /vagrant/provision/client/ntp/etc/
```

Создан скрипт:

![Скрипт ntp.sh клиента](Screenshot_9.png){ #fig:110 width=80% }

# Итоги

## Вывод
В процессе выполнения работы была произведена настройка служб синхронизации времени на сервере и клиенте. Изучены механизмы работы `timedatectl`, системного и аппаратного времени. Настроены службы `chronyd` на сервере и клиенте, выполнена перенастройка брандмауэра, а также подготовлены provisioning-скрипты для автоматизации конфигурации в среде Vagrant. Клиент успешно начал синхронизировать время с локальным NTP-сервером, что подтверждено выводом команд chrony. Лабораторный стенд функционирует корректно и соответствует требованиям задания.

## Контрольные вопросы

**1. Почему важна точная синхронизация времени для служб баз данных?**  
Потому что многие операции используют временные метки: транзакции, репликация, блокировки, журналы. Несовпадение времени может привести к нарушению целостности данных, ошибкам согласованности или сбоям в репликации.

**2. Почему служба проверки подлинности Kerberos сильно зависит от правильной синхронизации времени?**  
Kerberos использует временные метки в билетах безопасности. Если время клиента и сервера отличается более чем на допустимый интервал, билеты считаются недействительными, и аутентификация не происходит.

**3. Какая служба используется по умолчанию для синхронизации времени на RHEL 7?**  
По умолчанию используется служба `chronyd` (пакет `chrony`).

**4. Какова страта по умолчанию для локальных часов?**  
Страта локальных часов обычно равна **10**, так как они считаются низкоприоритетным источником времени.

**5. Какой порт брандмауэра должен быть открыт, если вы настраиваете свой сервер как одноранговый узел NTP?**  
Необходимо открыть порт **UDP 123**, используемый протоколом NTP.

**6. Какую строку нужно включить в конфигурационный файл chrony, чтобы быть сервером времени, даже если внешние источники недоступны?**  
Строку:  
`local stratum 10`  
Она разрешает использовать локальные часы как источник времени.

**7. Какую страту имеет хост, если нет текущей синхронизации времени NTP?**  
Он использует страту, указанную в директиве `local` (по умолчанию 10), что означает отсутствие точного внешнего источника.

**8. Какую команду вы бы использовали на сервере с chrony, чтобы узнать, с какими серверами он синхронизируется?**  
Команду:  
`chronyc sources`

**9. Как получить подробную статистику текущих настроек времени для процесса chrony на сервере?**  
Использовать команду:  
`chronyc tracking`
