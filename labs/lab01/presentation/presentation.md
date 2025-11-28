---
## Front matter
lang: ru-RU
title: Администрирование сетевых подсистем
subtitle: Лабораторная работа №1
author:
  - Авдадаев Джамал Геланиевич
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 25 ноября 2025

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

Приобретение практических навыков подготовки стенда Rocky Linux, автоматизации установки и развёртывания виртуальных машин с помощью Packer и Vagrant.

# Ход выполнения

## Установка плагинов Vagrant

![Установка vagrant-vbguest](Screenshot_1.png){ #fig:001 width=70% }

## Добавление box-образа

![Добавление box-файла](Screenshot_2.png){ #fig:002 width=70% }

## Настройка provisioning: hostname

![Скрипт изменения hostname](Screenshot_3.png){ #fig:003 width=70% }

## Настройка provisioning: создание пользователя

![Скрипт user](Screenshot_4.png){ #fig:004 width=70% }

## Подключение provisioning в Vagrantfile

![Vagrantfile provisioning](Screenshot_5.png){ #fig:005 width=70% }

## Запуск виртуальной машины Server

![vagrant up server](Screenshot_6.png){ #fig:006 width=75% }

## Проверка hostname в GUI

![hostname после запуска](Screenshot_7.png){ #fig:007 width=70% }

## Подключение по SSH

![SSH вход под пользователем](Screenshot_8.png){ #fig:008 width=70% }

# Выводы

## Итог лабораторной работы

В ходе работы сформирован образ Rocky Linux с помощью Packer, добавлен в Vagrant и использован для развёртывания виртуальной машины.  
Provisioning-скрипты автоматизировали назначение hostname и создание пользователя.  
Проверены корректный запуск VM, работа SSH и смена имени хоста. Лабораторный стенд подготовлен полностью и соответствует заданию.
