---
## Front matter
lang: ru-RU
title: Администрирование сетевых подсистем
subtitle: Лабораторная работа №13  
author:
  - Авдадаев Джамал Геланиевич
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 06 декабря 2025

## i18n babel
babel-lang: russian
babel-otherlangs: english

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

## Цель лабораторной работы
Получение практических навыков настройки сервера и клиента NFS, публикации каталогов, назначения прав доступа и автоматизации конфигурации средствами provisioning.

# Ход выполнения

## Установка ПО и подготовка каталога

![Редактирование exports](Screenshot_1.png){ width=70% }

## Настройка SELinux и firewall

![Настройка firewalld](Screenshot_2.png){ width=70% }

## Проверка экспортов с клиента

![Проверка showmount](Screenshot_4.png){ width=70% }

## Подключение дерева NFS

![Монтирование NFS](Screenshot_5.png){ width=70% }

## Автоматическое монтирование

![remote-fs.target](Screenshot_7.png){ width=70% }

## Публикация веб-каталога

![Экспорт www](Screenshot_8.png){ width=70% }

## Автоматизация bind-монтирования

![fstab www](Screenshot_9.png){ width=70% }

## Автоматизация bind-монтирования

![Проверка www](Screenshot_10.png){ width=70% }

## Настройка каталога пользователя

![Каталог common](Screenshot_12.png){ width=70% }

## Экспорт и bind-монтирование

![export home](Screenshot_13.png){ width=70% }

## Экспорт и bind-монтирование

![fstab home](Screenshot_14.png){ width=70% }

## Проверка работы с клиентской стороны

![Работа пользователя](Screenshot_16.png){ width=70% }

## Проверка работы с клиентской стороны

![Появление файла](Screenshot_17.png){ width=70% }

# Выводы

## Итог лабораторной работы

- Настроены экспортируемые каталоги NFS  
- Реализовано bind-монтирование системных и пользовательских каталогов  
- Обеспечены корректные права доступа и безопасность  
- Настроено автоматическое монтирование  
- Созданы provisioning-скрипты для полной автоматизации настройки сервера и клиента  
