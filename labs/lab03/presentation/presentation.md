---
## Front matter
lang: ru-RU
title: Администрирование сетевых подсистем
subtitle: Лабораторная работа №3
author:
  - Авдадаев Джамал Геланиевич
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 26 ноября 2025

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

Приобретение практических навыков по установке, конфигурированию и интеграции DHCP-сервера Kea с DNS-сервером Bind9, включая настройку динамических обновлений DDNS.

# Ход выполнения

## Установка DHCP-сервера Kea

![Установка DHCP](Screenshot_1.png){ width=70% }

## Резервное копирование конфигурации

![Бэкап конфигурации](Screenshot_2.png){ width=70% }

## Настройка DNS-параметров

![Изменение DNS-настроек в kea-dhcp4.conf](Screenshot_2.png){ width=70% }

## Настройка подсети DHCP

![Настройка подсети](Screenshot_3.png){ width=70% }

## Проверка и активация DHCP

![Проверка kea-dhcp4](Screenshot_4.png){ width=70% }

# Настройка DNS

## Прямая зона

![Прямая зона DNS](Screenshot_5.png){ width=70% }

## Обратная зона

![Обратная зона DNS](Screenshot_6.png){ width=70% }

## Проверка резолвинга

![ping dhcp](Screenshot_7.png){ width=70% }

## Настройка firewall и SELinux

![firewall + SELinux](Screenshot_8.png){ width=70% }

# Настройка DHCP-клиента

## Скрипт маршрутизации

![routing script](Screenshot_9.png){ width=70% }

## Получение адреса клиентом

![ifconfig eth1](Screenshot_10.png){ width=70% }

# Настройка DDNS

## Генерация TSIG-ключа

![TSIG key](Screenshot_12.png){ width=70% }

## Настройка update-policy в Bind9

![update-policy](Screenshot_13.png){ width=70% }

## Файл tsig-keys.json

![tsig JSON](Screenshot_14.png){ width=70% }

## Настройка kea-dhcp-ddns

![kea-dhcp-ddns.conf](Screenshot_15.png){ width=70% }

## Статус службы Kea DDNS

![kea-dhcp-ddns.service](Screenshot_16.png){ width=70% }

## Разрешение DDNS в Kea DHCP

![kea-dhcp4 DDNS](Screenshot_17.png){ width=70% }

## Перезапуск DHCP

![restart kea-dhcp4](Screenshot_18.png){ width=70% }

# Проверка DDNS

## Проверка записи клиента через dig

![dig client](Screenshot_19.png){ width=70% }

# Подготовка provisioning

## Копирование конфигураций DHCP/DNS

![provisioning copy](Screenshot_20.png){ width=70% }

## Скрипт автоматической настройки

![dhcp.sh](Screenshot_21.png){ width=70% }

# Выводы

## Итог лабораторной работы

В ходе работы был установлен и настроен DHCP-сервер Kea, интегрированный с Bind9 для динамических DNS-обновлений.  
Созданы TSIG-ключи, настроены правила обновления прямой и обратной зоны, включено автоматическое формирование A и PTR-записей.  
Проверено получение адреса клиентом и корректное появление DNS-записей.  
Структура provisioning подготовлена для автоматизации развёртывания стенда.
