---
## Front matter
title: "Отчёт по лабораторной работе 4"
subtitle: "Базовая настройка HTTP-сервера Apache"
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

Приобретение практических навыков по установке и базовому конфигурированию HTTP-сервера Apache.

# Процесс работы

## Установка HTTP-сервера

После входа на виртуальную машину server и перехода в режим суперпользователя была выполнена установка стандартного веб-сервера Apache HTTPD через группу пакетов Basic Web Server. В ходе установки система загрузила основной пакет httpd, дополнительные модули, документацию и зависимые компоненты.

![Установка пакетов веб-сервера](Screenshot_1.png){ #fig:001 width=80% }

После завершения установки были изучены каталоги /etc/httpd/conf и /etc/httpd/conf.d.  
Первый содержит основной конфигурационный файл httpd.conf и файл magic, определяющий правила обработки типов данных.  
Во втором расположены модульные конфигурации, управляющие обработкой CGI, SSL, пользовательских директорий, автоматической индексацией каталогов и стандартной приветственной страницы.

Для разрешения входящих HTTP-подключений были добавлены правила в конфигурацию межсетевого экрана firewalld. Это позволило обеспечить доступ к веб-серверу по порту 80 как из локальной сети, так и извне.

Был запущен мониторинг системного журнала с целью отслеживания возможных ошибок и состояния служб. Затем служба httpd была активирована и запущена. После этого в выводе журнала появились сообщения, подтверждающие успешный запуск веб-сервера Apache.

## Анализ работы HTTP-сервера

На сервере были открыты два терминала:  
в одном отслеживались ошибки веб-сервера, во втором — обращения клиентов.

Затем на виртуальной машине client был открыт браузер и введён адрес 192.168.1.1.  
В результате на экране появилась стандартная тестовая страница Apache HTTP Server Test Page, подтверждающая корректную работу HTTP-сервера.  
Параллельно в журнале access_log фиксировались обращения клиента.

![Стандартная тестовая страница Apache](Screenshot_2.png){ #fig:002 width=80% }

![Мониторинг логов веб-сервера](Screenshot_3.png){ #fig:003 width=80% }

## Настройка виртуального хостинга

Для корректной работы виртуального хостинга были добавлены новые записи в файлы прямой и обратной DNS-зон.

В файл прямой зоны /var/named/master/fz/dgavdadaev.net была добавлена запись, сопоставляющая имя www.dgavdadaev.net с IP-адресом 192.168.1.1.

![Файл прямой DNS-зоны](Screenshot_4.png){ #fig:004 width=80% }

В файл обратной зоны /var/named/master/rz/192.168.1 была добавлена запись сопоставления IP-адреса 192.168.1.1 с именем www.dgavdadaev.net.

![Файл обратной DNS-зоны](Screenshot_5.png){ #fig:005 width=80% }

В каталоге /etc/httpd/conf.d были созданы два файла: server.dgavdadaev.net.conf и www.dgavdadaev.net.conf.

Файл server.dgavdadaev.net.conf содержит настройки виртуального хоста, включающие адрес администратора, директорию DocumentRoot, имя хоста и пути к логам ошибок и обращений.

![Конфигурация server.dgavdadaev.net](Screenshot_6.png){ #fig:006 width=80% }

Файл www.dgavdadaev.net.conf содержит аналогичные параметры, но для доменного имени www.dgavdadaev.net и соответствующей директории.

![Конфигурация www.dgavdadaev.net](Screenshot_7.png){ #fig:007 width=80% }

В каталоге /var/www/html были созданы отдельные директории для двух виртуальных веб-серверов: server.dgavdadaev.net и www.dgavdadaev.net.  
В каждую директорию помещена тестовая страница index.html, содержащая приветственный текст.
После создания каталоги с веб-контентом были переданы пользователю apache, что необходимо для корректной работы HTTP-сервера.  
Далее был восстановлен контекст SELinux для каталогов /etc, /var/named и /var/www, после чего служба httpd была перезапущена.  

![каталоги с веб-контентом](Screenshot_8.png){ #fig:008 width=80% }

Для серверного хоста server.dgavdadaev.net была создана страница с текстом:

![Тестовая страница server.dgavdadaev.net](Screenshot_9.png){ #fig:009 width=80% }

Для веб-хоста www.dgavdadaev.net создана аналогичная страница:

![Тестовая страница www.dgavdadaev.net](Screenshot_10.png){ #fig:010 width=80% }

## Подготовка конфигурации для внутреннего окружения виртуальной машины

Для обеспечения воспроизводимой конфигурации при последующих развёртываниях были сформированы каталоги и файлы в структуре /vagrant/provision/server.

В каталоге /vagrant/provision/server был создан подкаталог http, содержащий разделы:

- http/etc/httpd/conf.d — копии конфигурационных файлов виртуальных хостов;
- http/var/www/html — копии веб-страниц, размещённых на сервере.

![Копирование конфигурационных файлов HTTP](Screenshot_11.png){ #fig:011 width=80% }

В подкаталоге dns были размещены актуальные файлы DNS-зон, включая прямую и обратную зоны, необходимые для работы виртуального хостинга.

В корневом каталоге /vagrant/provision/server был создан исполняемый файл http.sh — сценарий автоматизированного развёртывания, включающий установку веб-сервера, копирование конфигураций, изменение прав доступа, настройку SELinux, добавление правил firewalld и запуск сервиса httpd.

![Содержимое сценария http.sh](Screenshot_12.png){ #fig:012 width=80% }

# Итоги

## Вывод  
В ходе работы был установлен и сконфигурирован HTTP-сервер Apache на виртуальной машине server. Настроены базовые параметры сервера, разрешён HTTP-трафик в межсетевом экране, проверена работоспособность через журнал системных сообщений.  
Были изучены конфигурационные файлы в каталогах /etc/httpd/conf и /etc/httpd/conf.d, проанализированы логи доступа и ошибок.  
Настроен виртуальный хостинг по именам server.dgavdadaev.net и www.dgavdadaev.net, созданы тестовые веб-страницы, восстановлены контексты SELinux и обеспечена корректная работа веб-ресурсов.  
Дополнительно подготовлены каталоги и provisioning-скрипт http.sh для автоматизации настройки в среде Vagrant.

## Контрольные вопросы  

**1. Через какой порт по умолчанию работает Apache?**  
Apache по умолчанию принимает HTTP-запросы на порту 80, который является стандартным портом для незашифрованного веб-трафика.

**2. Под каким пользователем запускается Apache и к какой группе относится этот пользователь?**  
В Rocky Linux (как и в большинстве дистрибутивов на базе RHEL) Apache работает от имени пользователя *apache*, который относится к группе *apache*. Это обеспечивает изоляцию доступа и безопасность веб-процессов.

**3. Где располагаются лог-файлы веб-сервера? Что можно по ним отслеживать?**  
Логи Apache находятся в каталоге */var/log/httpd/*.  
Здесь располагаются:  
– error_log — журнал ошибок, фиксирующий сбои, нарушения прав доступа и проблемы при обработке запросов;  
– access_log — журнал запросов клиентов, отображающий IP-адреса, запрашиваемые ресурсы, коды ответов, время обращения.  
По логам удобно отслеживать работу сервера, активность пользователей и возможные атаки.

**4. Где по умолчанию содержится контент веб-серверов?**  
Статический веб-контент Apache хранится в каталоге */var/www/html*.  
При использовании виртуального хостинга создаются дополнительные подкаталоги внутри /var/www/html, соответствующие каждому доменному имени.

**5. Каким образом реализуется виртуальный хостинг? Что он даёт?**  
Виртуальный хостинг реализуется через конфигурационные файлы в каталоге /etc/httpd/conf.d, где для каждого доменного имени создаётся отдельный блок VirtualHost.  
Он позволяет одному экземпляру Apache обслуживать несколько независимых сайтов на одном IP-адресе, выделяя каждому собственный DocumentRoot, логи и настройки.
