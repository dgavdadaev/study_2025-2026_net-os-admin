---
## Front matter
lang: ru-RU
title: Администрирование сетевых подсистем
subtitle: Лабораторная работа №12
author:
  - Авдадаев Джамал Геланиевич
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 04 декабря 2025

## Formatting pdf
toc: false
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
---

# Цель работы

## Основная цель

Получение навыков по управлению системным временем и настройке синхронизации времени  
в Linux-системах с использованием службы **chrony**.

# Выплнение работы

## Команды date и hwclock

![server](Screenshot_1.png){ width=70% }

## Команды date и hwclock

![client](Screenshot_2.png){ width=70% }

## chronyc sources — сервер

![chrony sources server](Screenshot_3.png){ width=75% }

## chronyc sources — клиент (до настройки)

![chrony sources client old](Screenshot_4.png){ width=75% }

## Разрешение доступа внутренней сети

![chrony.conf server](Screenshot_5.png){ width=75% }

## Редактирование chrony.conf

![chrony.conf client](Screenshot_6.png){ width=75% }

## Проверка синхронизации

![chrony sources client new](Screenshot_7.png){ width=75% }

## Скрипт сервера ntp.sh

![ntp.sh server](Screenshot_8.png){ width=75% }

## Скрипт клиента ntp.sh

![ntp.sh client](Screenshot_9.png){ width=75% }

# Выводы

## Итоги работы

- Исследованы инструменты управления временем: `timedatectl`, `date`, `hwclock`.  
- Настроена синхронизация времени на сервере и клиенте с использованием chrony.  
- Разрешён NTP-трафик на сервере, клиент привязан к локальному NTP.  
- Подготовлены provisioning-скрипты, автоматизирующие процесс конфигурации.  
- Клиент успешно синхронизируется с сервером, лабораторный стенд работает корректно.
