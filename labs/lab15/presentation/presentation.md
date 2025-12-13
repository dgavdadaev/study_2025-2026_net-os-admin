---
## Front matter
lang: ru-RU
title: Администрирование сетевых подсистем
subtitle: Лабораторная работа №15
author:
  - Авдадаев Джамал Геланиевич
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 15 декабря 2025

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

# Цель работы

## Основная цель

Получить практические навыки настройки сетевого журналирования  
с использованием rsyslog на сервере и клиенте.

# Настройка сервера rsyslog

## Создание конфигурационного файла

![Конфигурация netlog-server.conf](Screenshot_1.png){ width=70% }

## Включение TCP-приёма сообщений

![Проверка портов rsyslog](Screenshot_2.png){ width=70% }

## Настройка firewall

# Настройка клиента rsyslog

## Создание и заполнение конфигурации

![netlog-client.conf](Screenshot_3.png){ width=70% }

# Просмотр журналов

## Проверка получения логов сервером

![Входящие сообщения в messages](Screenshot_4.png){ width=70% }

## Просмотр в графической утилите

![gnome-system-monitor](Screenshot_5.png){ width=70% }

## Установка консольного просмотрщика

![Ошибка установки lnav](Screenshot_6.png){ width=70% }

# Автоконфигурация Vagrant: сервер

## Экспорт конфигурации

![Создание структуры provisioning](Screenshot_7.png){ width=70% }

## Скрипт автоматизации

![Скрипт netlog.sh (server)](Screenshot_8.png){ width=70% }

# Автоконфигурация Vagrant: клиент

## Экспорт конфигурации

![Экспорт client netlog](Screenshot_9.png){ width=70% }

## Скрипт автоматизации

![Скрипт netlog.sh (client)](Screenshot_10.png){ width=70% }

# Выводы

## Итог лабораторной работы

– Настроены сервер и клиент rsyslog для сетевого журналирования.  
– Обеспечена передача сообщений по TCP-порту 514.  
– Реализованы provisioning-скрипты для автоматизации развёртывания.  
– Проверена работа логирования и взаимодействие journald ↔ rsyslog.  
– Лабораторный стенд полностью функционирует и соответствует заданию.
