---
## Front matter
lang: ru-RU
title: Администрирование сетевых подсистем
subtitle: Лабораторная работа №5
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

Получение навыков настройки веб-сервера Apache для работы по HTTPS, создания самоподписанного сертификата, настройки PHP и подготовки автоматизированного развёртывания через Vagrant.

# Ход выполнения

## Генерация SSL-ключа и сертификата

![Генерация ключа и сертификата](Screenshot_1.png){ #fig:001 width=75% }

## Конфигурация Apache: HTTP → HTTPS редирект

![Конфигурация Apache](Screenshot_2.png){ #fig:002 width=75% }

## Проверка работы HTTPS

![Работа сайта по HTTPS](Screenshot_3.png){ #fig:003 width=80% }

## Проверка информации о сертификате

![Просмотр сертификата](Screenshot_4.png){ #fig:004 width=75% }

## Настройка PHP и создание index.php

![Файл index.php](Screenshot_5.png){ #fig:005 width=60% }

## Проверка обработки PHP-страниц

![Отображение phpinfo](Screenshot_6.png){ #fig:006 width=80% }

## Подготовка окружения для автоматического развёртывания

![Копирование конфигурационных файлов](Screenshot_7.png){ #fig:007 width=80% }

## Изменения в скрипте http.sh

![Изменения в скрипте http.sh](Screenshot_8.png){ #fig:008 width=80% }

# Выводы

## Итоги лабораторной работы

Настроен веб-сервер Apache для работы по HTTPS, создан самоподписанный сертификат, включён редирект с HTTP, добавлена поддержка PHP.  
Конфигурации и SSL-материалы подготовлены для автоматической установки при развёртывании через Vagrant.  
Все элементы функционируют корректно, сайт доступен по защищённому протоколу и выполняет PHP-скрипты.
