---
## Front matter
title: "Отчёт по лабораторной работе 5"
subtitle: "Расширенная настройка HTTP-сервера Apache"
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

## Конфигурирование HTTP-сервера для работы через протокол HTTPS

В каталоге `/etc/pki/tls/private` был создан приватный ключ и самоподписанный сертификат для домена **www.dgavdadaev.net**. Использовалась команда генерации сертификата, после чего были введены параметры: RU, Russia, Moscow, dgavdadaev, dgavdadaev, dgavdadaev.net, dgavdadaev@dgavdadaev.net.

![Генерация ключа и сертификата](Screenshot_1.png){ #fig:001 width=80% }

Сертификат и ключ были переименованы и размещены в соответствующих каталогах. Далее сертификат был скопирован в `/etc/ssl/certs/`.

Был отредактирован файл `/etc/httpd/conf.d/www.dgavdadaev.net.conf`, после чего он получил следующий вид:

![Конфигурация Apache](Screenshot_2.png){ #fig:002 width=80% }

### Построчное пояснение конфигурации

- `<VirtualHost *:80>` — начало конфигурации виртуального хоста для HTTP.  
- `ServerAdmin webmaster@dgavdadaev.net` — адрес администратора.  
- `DocumentRoot /var/www/html/www.dgavdadaev.net` — корневая директория сайта.  
- `ServerName` и `ServerAlias` — доменное имя сайта.  
- `ErrorLog` и `CustomLog` — файлы логирования ошибок и обращений.  
- `RewriteEngine on` — включение механизма переписывания ссылок.  
- `RewriteRule ^(.*)$ https://%{HTTP_HOST}$1 [R=301,L]` — принудительное перенаправление всех запросов на HTTPS.  
- `</VirtualHost>` — завершение блока HTTP.

- `<IfModule mod_ssl.c>` — проверка наличия модуля SSL.  
- `<VirtualHost *:443>` — начало конфигурации HTTPS.  
- `SSLEngine on` — включение SSL.  
- Повторяются параметры администратора, домена, корневого каталога и логов.  
- `SSLCertificateFile` — путь к сертификату.  
- `SSLCertificateKeyFile` — путь к приватному ключу.  
- `</VirtualHost>` — конец конфигурации HTTPS.  
- `</IfModule>` — завершение условного блока.

После перезапуска веб-сервера сайт стал доступен по защищённому протоколу HTTPS.

![Работа сайта по HTTPS](Screenshot_3.png){ #fig:003 width=80% }

Просмотр сведений о сертификате показал корректность введённых параметров.

![Просмотр сертификата](Screenshot_4.png){ #fig:004 width=80% }

## Конфигурирование HTTP-сервера для работы с PHP

В каталоге сайта был создан файл `index.php` с функцией `phpinfo()`.

![Файл index.php](Screenshot_5.png){ #fig:005 width=60% }

После выполнения всех настроек веб-сервер корректно обработал PHP-страницу.

![Отображение phpinfo](Screenshot_6.png){ #fig:006 width=80% }

## Внесение изменений во внутреннее окружение виртуальной машины

В каталоге `/vagrant/provision/server/http` были размещены актуальные конфигурации веб-сервера. Для этого скопированы файлы Apache, веб-контента и SSL-материалов:

- конфигурации `/etc/httpd/conf.d/*` перенесены в `.../etc/httpd/conf.d/`;
- содержимое веб-каталога `/var/www/html/*` — в `.../var/www/html/`;
- созданы каталоги для ключей и сертификатов `etc/pki/tls/private` и `etc/pki/tls/certs`;
- файл приватного ключа и файл сертификата перенесены в соответствующие пути внутри `provision`.

![Копирование конфигурационных файлов](Screenshot_7.png){ #fig:007 width=80% }

В скрипт `/vagrant/provision/server/http.sh` добавлены команды для установки PHP и правил межсетевого экрана, позволяющих работать через HTTPS. Это обеспечивает автоматическую подготовку окружения при развёртывании.

![Изменения в скрипте http.sh](Screenshot_8.png){ #fig:008 width=80% }

# Итоги

## Вывод  
В процессе работы был настроен веб-сервер для функционирования через HTTPS, создан самоподписанный сертификат, выполнено перенаправление с HTTP на HTTPS и добавлена поддержка PHP. Конфигурации и SSL-материалы перенесены во внутреннее окружение виртуальной машины для автоматического развёртывания. Проверена работа сертификата и корректное выполнение PHP-скриптов.

## Контрольные вопросы  

**1. В чём отличие HTTP от HTTPS?**  
HTTP передаёт данные в открытом виде, тогда как HTTPS использует шифрование (TLS/SSL), обеспечивая защищённый канал между сервером и клиентом.

**2. Каким образом обеспечивается безопасность контента веб-сервера при работе через HTTPS?**  
Безопасность достигается за счёт шифрования трафика, проверки подлинности сервера с помощью сертификата и защиты данных от перехвата и изменения.

**3. Что такое сертификационный центр? Приведите пример.**  
Сертификационный центр (CA) — организация, выпускающая и подписывающая цифровые сертификаты, подтверждая подлинность владельца.  
Примеры: LetsEncrypt, DigiCert, GlobalSign.
