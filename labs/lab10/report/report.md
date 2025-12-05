---
## Front matter
title: "Отчёт по лабораторной работе 10"
subtitle: "Расширенные настройки SMTP-сервера"
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

Приобретение практических навыков по конфигурированию SMTP-сервера в части настройки аутентификации.

# Процесс работы

## Настройка LMTP в Dovecot и интеграция с Postfix

### Добавление протокола LMTP в конфигурацию Dovecot

В файле `/etc/dovecot/dovecot.conf` был расширен список поддерживаемых протоколов: добавлен LMTP. Это необходимо для доставки почты через внутренний транспорт Dovecot.

![Фрагмент dovecot.conf с добавленным lmtp](Screenshot_2.png){ #fig:001 width=80% }

Строка конфигурации была изменена на:
protocols = imap pop3 lmtp

### Настройка сервиса LMTP в Dovecot

В файле `/etc/dovecot/conf.d/10-master.conf` была изменена секция `service lmtp`. Определён unix-сокет для связи Postfix и Dovecot, а также задан пользователь, группа и права доступа.

![Настройка сервиса lmtp в 10-master.conf](Screenshot_1.png){ #fig:002 width=80% }

Эта конфигурация создаёт сокет `/var/spool/postfix/private/dovecot-lmtp` с владельцем postfix и правами 0600. Postfix получает возможность передавать сообщения в DovecotLMTP.

### Настройка Postfix для передачи писем через LMTP

Postfix был настроен на использование транспорта LMTP через unix-сокет. Передача сообщений больше не выполняется напрямую в локальный Maildir, а осуществляется через DovecotLMTP.

### Задание формата имени пользователя в Dovecot

В файле `/etc/dovecot/conf.d/10-auth.conf` установлен формат логина без доменной части.

![auth\_username\_format](Screenshot_3.png){ #fig:003 width=80% }

Настройка:
auth_username_format = %Ln
Это гарантирует, что логин интерпретируется в виде локального имени пользователя.

### Перезапуск служб и отправка письма

После изменения конфигурации Postfix и Dovecot были перезапущены. Затем было отправлено тестовое письмо для проверки работы LMTP. В процессе мониторинга через `tail -f /var/log/maillog` наблюдалась успешная передача сообщения от Postfix к DovecotLMTP.

![Логи доставки письма через LMTP](Screenshot_4.png){ #fig:004 width=100% }

**Анализ логов:**
Postfix принимает письмо от клиента → передаёт его LMTP-транспорту → DovecotLMTP принимает письмо через сокет → успешно сохраняет его в INBOX пользователя. Лог содержит строку «saved mail to INBOX», подтверждающую доставку.

### Проверка почтового ящика

После доставки письмо было просмотрено через команду чтения Maildir.

![Письмо доставлено в Maildir](Screenshot_5.png){ #fig:005 width=80% }

Письмо с темой «LMTP test» присутствует в Maildir, а значит LMTP-доставка настроена корректно.

## Настройка SMTP-аутентификации

### Определение службы auth в Dovecot

В файле `/etc/dovecot/conf.d/10-master.conf` определена служба аутентификации, включающая два unix-слушателя: один для Postfix, другой для внутренней базы пользователей Dovecot.

![Секция service auth в 10-master.conf](Screenshot_6.png){ #fig:006 width=80% }

**Построчное объяснение:**
• service auth — объявление службы аутентификации
• unix_listener /var/spool/postfix/private/auth — сокет, к которому подключается Postfix для SASL-аутентификации
• group = postfix и user = postfix — разрешения на доступ для Postfix
• mode = 0660 — сокет доступен владельцу и группе
• unix_listener auth-userdb — внутренний сокет Dovecot для работы с его базой пользователей
• mode = 0600 — доступ только для Dovecot
• user = dovecot — владелец сокета

### Настройка Postfix для использования SASL

Postfix был перенастроен на использование Dovecot как SASL-провайдера и получил путь к соответствующему сокету.

### Настройка ограничений приёма почты

Были заданы параметры smtpd_recipient_restrictions, блокирующие попытки использовать сервер как open relay.

![postconf smtpd\_recipient\_restrictions](Screenshot_7.png){ #fig:007 width=100% }

**Комментарии к опциям:**
• reject_unknown_recipient_domain — отклонение несуществующих доменов
• permit_mynetworks — разрешение доверенным хостам
• reject_non_fqdn_recipient — отклонение адресов без доменной части
• reject_unauth_destination — защита от пересылки писем на внешние домены
• reject_unverified_recipient — проверка существования адресата
• permit — завершение цепочки успешным разрешением

### Ограничение сети, которой разрешена отправка

Сервер принимает почту только с локального диапазона:
mynetworks = 127.0.0.0/8

### Изменения в master.cf

В `master.cf` был включён smtpd с поддержкой SASL.

![Фрагмент master.cf](Screenshot_8.png){ #fig:008 width=100% }

### Проверка SMTP-аутентификации через telnet

На клиенте была получена base64-строка для AUTH PLAIN и выполнено подключение к smtp-службе. Аутентификация прошла успешно, что подтверждает корректную настройку SASL.

![Тест AUTH PLAIN через telnet](Screenshot_9.png){ #fig:009 width=80% }

## Настройка SMTP over TLS

### Копирование временных сертификатов и настройка TLS в Postfix

Для включения TLS были использованы временные сертификаты Dovecot. Файлы сертификата и ключа были скопированы в каталоги `/etc/pki/tls/certs/` и `/etc/pki/tls/private/`, чтобы избежать нарушений SELinux. Далее в Postfix были указаны пути к сертификату и ключу, а также включено кэширование TLS-сессий и задан уровень безопасности.

![Настройка сертификатов и параметров TLS в Postfix](Screenshot_10.png){ #fig:010 width=100% }

Настройки TLS позволяют Postfix принимать защищённые SMTP-соединения и использовать сертификат Dovecot для шифрования канала.

### Настройка службы submission в master.cf

Для запуска SMTP-сервера на порту 587 была изменена конфигурация файла `/etc/postfix/master.cf`. Блок стандартного сервиса SMTP был сокращён до одной строки, а затем добавлен новый блок `submission` с обязательным использованием TLS и аутентификации.

![Фрагмент master.cf с добавленным сервисом submission](Screenshot_11.png){ #fig:011 width=100% }

Секция `submission` запускает демон smtpd на порту 587 и требует TLS (`smtpd_tls_security_level=encrypt`) и SASL-аутентификацию, что соответствует рекомендациям по безопасной отправке почты.

### Настройка межсетевого экрана

Для работы сервиса submission необходимо разрешить соответствующую службу в firewalld. Добавление правила, его сохранение и перезагрузка межсетевого экрана были выполнены успешно.

![Команды настройки firewall для smtp-submission](Screenshot_12.png){ #fig:012 width=100% }

После применения правил порт 587 стал доступен для клиентов для защищённой отправки почты.

### Проверка подключения через openssl и тестирование аутентификации

На клиентской машине было выполнено подключение через openssl с использованием STARTTLS. Сервер корректно представился, указав использование самоподписанного сертификата. После отправки команды EHLO был доступен список расширений SMTP. Аутентификация прошла успешно, что подтверждает правильную работу SASL через TLS.

![Проверка соединения и аутентификации через openssl](Screenshot_13.png){ #fig:013 width=80% }

### Проверка отправки почты через Evolution

На клиенте был настроен почтовый клиент Evolution: для SMTP указан порт 587, протокол STARTTLS и обычная парольная аутентификация. После этого была выполнена отправка тестового письма, которое успешно доставилось на сервер.

![Отправленные письма в Evolution](Screenshot_14.png){ #fig:014 width=70% }

### Анализ логов сервера при использовании SMTP over TLS

Логи на сервере показывают успешное прохождение IMAP-входа, подключение SMTP-клиента, выполнение аутентификации, передачу сообщения через LMTP и доставку письма в INBOX пользователя.

![Логи доставки письма через SMTP over TLS и LMTP](Screenshot_15.png){ #fig:015 width=100% }

Запись *saved mail to INBOX* подтверждает успешную доставку письма.

### Проверка содержимого Maildir на сервере

Проверка почтового ящика пользователя показала, что новое письмо `test3` было успешно доставлено и находится в Maildir.

![Maildir с полученными письмами](Screenshot_16.png){ #fig:016 width=70% }

## Внесение изменений в настройки внутреннего окружения виртуальной машины

Для сохранения результатов настройки серверных служб конфигурационные файлы Dovecot и Postfix были скопированы в соответствующие каталоги внутри среды Vagrant: `/vagrant/provision/server/mail/etc/dovecot/` и `/vagrant/provision/server/mail/etc/postfix/`.

![Копирование конфигурационных файлов в окружение Vagrant](Screenshot_17.png){ #fig:017 width=100% }

Это позволяет автоматически применять ту же конфигурацию при будущих развёртываниях виртуальной машины.

Был изменён файл `/vagrant/provision/server/mail.sh`, в который добавлена автоматическая установка необходимых пакетов, копирование конфигурационных файлов, настройка firewall и конфигурирование Postfix, включая параметры сети, протоколов и домена.

![Расширенная версия скрипта mail.sh](Screenshot_18.png){ #fig:018 width=100% }

Для клиентской машины была создана упрощённая версия скрипта, содержащая минимальный набор команд: установка Postfix, почтового клиента s-nail, Evolution и telnet, настройка сетевых протоколов и запуск Postfix.

![Упрощённый вариант mail.sh для клиента](Screenshot_19.png){ #fig:019 width=70% }

# Итоги

## Вывод

В ходе выполнения работы была выполнена настройка TLS, SASL-аутентификации и защищённого SMTP-submission на базе Postfix и Dovecot. Реализовано использование временного сертификата Dovecot, настроены сервисы LMTP, IMAP и SMTP, обеспечена передача писем по безопасному каналу. Проведено тестирование подключения через openssl и telnet, подтверждена корректная работа аутентификации и доставка писем через LMTP. Настройки были перенесены в окружение Vagrant для автоматизации будущих развёртываний. Все задачи выполнены успешно.

## Контрольные вопросы

**1. Приведите пример задания формата аутентификации пользователя в Dovecot в форме логина с указанием домена.**
Чтобы задать формат логина пользователя вместе с доменной частью, используется параметр:
auth_username_format = %n
В таком формате логин не преобразуется и учитывает домен, например: *[user@example.net](mailto:user@example.net)*.

**2. Какие функции выполняет почтовый Relay-сервер?**
Relay-сервер принимает сообщения от клиентов или других SMTP-серверов и пересылает их дальше к конечному серверу назначения. Он выполняет транзитную доставку почты, не являясь конечной точкой хранения сообщений. Часто используется в корпоративных сетях для фильтрации, маршрутизации или централизованного выхода почты в Интернет.

**3. Какие угрозы безопасности могут возникнуть в случае настройки почтового сервера как Relay-сервера?**
Некорректно настроенный relay превращается в *open relay*, что позволяет злоумышленникам отправлять через него спам и вредоносные сообщения. Это ведёт к перегрузке сервера, включению в DNS-блок-листы, блокировке домена и IP-адреса, снижению репутации почтового домена и рискам отказа в обслуживании.
