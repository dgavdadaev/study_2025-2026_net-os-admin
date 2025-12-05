---
lang: ru-RU
title: Администрирование сетевых подсистем
subtitle: Лабораторная работа №11  
author:
  - Авдадаев Джамал Геланиевич
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 04 декабря 2025

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
Приобретение практических навыков по настройке безопасного удалённого доступа к серверу по протоколу SSH.

# Ход выполнения

## Попытка входа под root

![Попытка входа под root](Screenshot_1.png){ width=65% }

## Настройка запрета root-доступа

![sshd_config — запрет root](Screenshot_2.png){ width=65% }

## Проверка после запрета

![Отказ после запрета root](Screenshot_3.png){ width=65% }

## Успешный вход обычного пользователя

![Успешный вход пользователя](Screenshot_4.png){ width=65% }

## Настройка AllowUsers

![AllowUsers vagrant](Screenshot_5.png){ width=65% }

## Блокировка пользователя dgavdadaev

![Отказ пользователя dgavdadaev](Screenshot_6.png){ width=65% }

## Расширение AllowUsers

![Добавление пользователя](Screenshot_7.png){ width=65% }

## Расширение AllowUsers

![Успешный вход](Screenshot_8.png){ width=65% }

## Добавление второго порта

![Добавление порта](Screenshot_9.png){ width=65% }

## Ошибка SELinux

![Ошибка привязки](Screenshot_10.png){ width=65% }
## Разрешение порта в SELinux и firewall

![SELinux и firewall](Screenshot_11.png){ width=65% }

## Подключение по порту 2022

![Подключение по 2022](Screenshot_12.png){ width=65% }

## Включение PubkeyAuthentication

![Параметр PubkeyAuthentication](Screenshot_13.png){ width=65% }

## Вход без пароля

![Безпарольный вход](Screenshot_14.png){ width=65% }

## Создание туннеля

![TCP после туннеля](Screenshot_15.png){ width=65% }

## Проверка в браузере

![localhost:8080](Screenshot_16.png){ width=65% }

## Команды по SSH

![Просмотр почты](Screenshot_18.png){ width=65% }

## Включение X11Forwarding

![X11Forwarding](Screenshot_19.png){ width=65% }

## Ошибка при запуске Firefox

![Ошибка Firefox](Screenshot_20.png){ width=65% }

## Скрипт ssh.sh

![ssh.sh](Screenshot_21.png){ width=65% }

# Вывод

## Итоги работы

- Запрещён вход root  
- Ограничены доступные пользователи  
- Настроены несколько портов SSH  
- Разрешён нестандартный порт в SELinux/firewalld  
- Настроена аутентификация по ключам  
- Созданы SSH-туннели  
- Изучены консольные и графические приложения SSH  
