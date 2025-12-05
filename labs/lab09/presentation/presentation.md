---
## Front matter
lang: ru-RU
title: Администрирование сетевых подсистем
subtitle: Лабораторная работа №9
author:
  - Авдадаев Джамал Геланиевич
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 1 декабря 2025

## i18n babel
babel-lang: russian
babel-otherlangs: english

## Formatting pdf
toc: false
toc-title: Содержание
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
---

# Цели и задачи работы

## Цель лабораторной работы

Приобретение практических навыков установки, конфигурирования и проверки работы POP3/IMAP-сервера на базе Postfix и Dovecot.

# Ход выполнения

## Установка и настройка Dovecot

![Установка пакетов dovecot и telnet](Screenshot_1.png){ #fig:001 width=70% }

## Настройка протоколов

![dovecot.conf — IMAP/POP3](Screenshot_2.png){ #fig:002 width=70% }

## Настройка аутентификации

![10-auth.conf](Screenshot_3.png){ #fig:003 width=70% }

## Источники данных пользователей

![auth-system.conf.ext](Screenshot_4.png){ #fig:004 width=70% }

## Настройка почтовых ящиков и Postfix

![10-mail.conf](Screenshot_5.png){ #fig:005 width=70% }

## FirewallD: разрешение почтовых служб

![Firewall настройки](Screenshot_6.png){ #fig:006 width=85% }

## Тестирование с клиента через Evolution

![Отправка письма](Screenshot_7.png){ #fig:007 width=70% }

## Тестирование с клиента через Evolution

![Получение письма](Screenshot_8.png){ #fig:008 width=85% }

## Анализ журнала почтовой службы

![maillog: отправка и получение писем](Screenshot_9.png){ #fig:009 width=90% }

## Проверка почты на сервере

![Maildir через mail](Screenshot_10.png){ #fig:010 width=70% }

## Проверка POP3 через Telnet

![POP3 через telnet](Screenshot_11.png){ #fig:011 width=90% }

# Выводы

## Итог лабораторной работы

Почтовая система на базе Postfix и Dovecot успешно установлена и настроена.  
Проверена работа протоколов IMAP и POP3, отправка и получение писем в Evolution, доступ к сообщениям через `mail`, `doveadm` и Telnet.  
Конфигурации интегрированы в Vagrant-окружение, а provisioning-скрипты дополнены автоматизацией настройки почтовых служб.  
Работа почтового сервера подтверждена на всех уровнях тестирования.
