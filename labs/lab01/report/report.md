---
## Front matter
title: "Отчёт по лабораторной работе 1"
subtitle: "Подготовка лабораторного стенда"
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

Целью данной работы является приобретение практических навыков установки Rocky Linux на виртуальную машину с помощью инструмента Vagrant.

# Процесс работы

## Установка плагинов и подготовка окружения

### Установка плагина *vagrant-vbguest*

Для корректной работы с общими папками VirtualBox был установлен плагин `vagrant-vbguest`.  
В каталоге проекта была выполнена команда:

`vagrant plugin install --plugin-clean-sources --plugin-source [https://rubygems.org](https://rubygems.org) vagrant-vbguest`

В результате система загрузила необходимые зависимости и установила плагин.

![Установка плагина vagrant-vbguest](Screenshot_1.png){ #fig:001 width=80% }

### Добавление box-образа в Vagrant

После формирования box-файла Rocky Linux образ был зарегистрирован локально в Vagrant:

`vagrant box add rockylinux10 vagrant-virtualbox-rockylinux10-x86_64.box`

Vagrant автоматически распаковал содержимое box-файла и сообщил об успешной установке образа.

![Добавление box-образа Rocky Linux](Screenshot_2.png){ #fig:002 width=80% }

### Скрипт назначения hostname

В каталоге *provision/default* был создан скрипт, формирующий hostname по шаблону:  
**текущее_имя_хоста.логин.net**

Пример содержимого скрипта:

![Скрипт изменения hostname](Screenshot_3.png){ #fig:003 width=80% }

### Скрипт создания пользователя

Был подготовлен скрипт, создающий пользователя, назначающий ему пароль, добавляющий в группу *wheel* и настраивающий приглашение командной строки.

![Скрипт создания пользователя](Screenshot_4.png){ #fig:004 width=80% }

### Подключение provisioning-скриптов в Vagrantfile

В конфигурационный файл Vagrant были добавлены блоки, вызывающие общие provisioning-скрипты:

![Фрагмент Vagrantfile с provisioning](Screenshot_5.png){ #fig:005 width=80% }

### Запуск VM *server*

В командной строке была выполнена команда:

`vagrant up server`

Машина успешно развернулась, применены provisioning-скрипты и выполнена первичная настройка.

![Результат запуска VM server](Screenshot_6.png){ #fig:006 width=80% }

### Проверка hostname в графическом окружении

После входа в систему под пользователем *dgavdadaev* hostname был успешно изменён согласно шаблону:

**server.dgavdadaev.net**

![Hostname после запуска системы](Screenshot_7.png){ #fig:007 width=80% }

### SSH-подключение к серверу

Для удалённого подключения использовалась команда:

`vagrant ssh server`

После ввода пароля *vagrant* выполнен переход к созданному пользователю:

![SSH-подключение и вход под пользователем](Screenshot_8.png){ #fig:008 width=80% }

# Итоги

## Вывод  
В ходе выполнения работы был сформирован образ Rocky Linux с помощью Packer и зарегистрирован в Vagrant. Созданные provisioning-скрипты автоматизировали назначение hostname и создание пользователя. Виртуальная машина успешно развернулась в VirtualBox, применив все этапы конфигурации. Проверено подключение по SSH, корректность смены имени хоста и вход под созданным пользователем. Лабораторный стенд был полностью подготовлен и функционирует согласно заданию.

## Контрольные вопросы  

**1. Для чего предназначен Vagrant?**  
Инструмент для автоматизированного развёртывания и управления виртуальными машинами, обеспечивающий воспроизводимые и переносимые рабочие среды.

**2. Что такое box-файл? В чём назначение Vagrantfile?**  
Box-файл — это готовый образ виртуальной машины, из которого Vagrant создаёт новые экземпляры ВМ.  
Vagrantfile — конфигурационный файл проекта, описывающий параметры виртуальных машин, сеть, provisioning и используемые box-образы.

**3. Приведите описание и примеры вызова основных команд Vagrant.**  
- `vagrant init` — создание Vagrantfile в каталоге проекта.  
- `vagrant up` — запуск и развёртывание виртуальной машины.  
- `vagrant halt` — остановка работающей ВМ.  
- `vagrant ssh` — подключение к ВМ по SSH.  
- `vagrant destroy` — удаление созданной ВМ.  
- `vagrant box add` — добавление нового box-образа.  

**4. Дайте построчные пояснения содержания файлов vagrant-rocky.pkr.hcl, ks.cfg, Vagrantfile, Makefile.**  
- `vagrant-rocky.pkr.hcl` — задаёт параметры сборки образа Packer: тип билдера, используемый ISO-файл, настройки виртуальной машины и вызов kickstart-файла.  
- `ks.cfg` — kickstart-конфигурация автоматической установки ОС: разметка диска, настройки сети, параметры пользователя, пакеты и службы.  
- `Vagrantfile` — описание инфраструктуры Vagrant: выбор box-файла, настройки провайдера VirtualBox, конфигурация сети и подключение provisioning-скриптов.  
- `Makefile` — автоматизирует запуск часто используемых команд (например, сборка образа Packer, добавление box-файла или развертывание ВМ).
