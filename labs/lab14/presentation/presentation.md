---
## Front matter
lang: ru-RU
title: Администрирование сетевых подсистем
subtitle: Лабораторная работа №14
author:
  - Авдадаев Джамал Геланиевич
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 2025

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

## Основная цель

Получение практических навыков настройки файловых служб Samba и разграничения доступа пользователей к общим ресурсам.

# Ход выполнения

## Установка пакетов и подготовка каталога

![Установка пакетов Samba](Screenshot_1.png){ width=70% }

## Настройка smb.conf

![Редактирование smb.conf](Screenshot_2.png){ width=70% }

## Запуск службы и проверка

![Проверка smbclient](Screenshot_3.png){ width=70% }

## Правила firewalld и SELinux

![Настройки firewall и SELinux](Screenshot_5.png){ width=70% }

## Добавление пользователя

![Добавление SMB-пользователя](Screenshot_7.png){ width=70% }

## Установка и настройка

![Установка клиента](Screenshot_9.png){ width=70% }

## Проверка smbclient

![Просмотр ресурсов](Screenshot_11.png){ width=70% }

## Монтирование SMB-ресурса

![Монтирование SMB](Screenshot_12.png){ width=70% }

## Тестирование записи

![Создание файла на клиенте](Screenshot_13.png){ width=70% }

## Файл учётных данных и fstab

![Файл smbusers](Screenshot_14.png){ width=70% }

## Файл учётных данных и fstab

![fstab](Screenshot_15.png){ width=70% }

# Итоги

## Основные результаты

- Развёрнут Samba-сервер на Rocky Linux.  
- Настроены firewall, SELinux и права доступа.  
- Созданы пользователь и SMB-учётная запись.  
- Настроен клиент: smbclient, CIFS-монтирование, автоматизация через fstab.  
- Подготовлены provisioning-скрипты для автоматического развёртывания.

