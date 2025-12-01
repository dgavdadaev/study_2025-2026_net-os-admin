---
## Front matter
lang: ru-RU
title: Администрирование сетевых подсистем
subtitle: Лабораторная работа №4
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

# Цель работы

## Основная цель

Приобретение практических навыков по установке, настройке и анализу работы HTTP-сервера Apache, а также конфигурированию виртуального хостинга и подготовке окружения для автоматизации развёртывания.

# Ход выполнения

## Установка пакетов Apache

![Установка пакетов Apache](Screenshot_1.png){ width=70% }

## Тестирование с клиентской машины

![Тестовая страница Apache](Screenshot_2.png){ width=70% }

![Мониторинг access_log](Screenshot_3.png){ width=70% }

## DNS: прямая зона

![Файл прямой зоны](Screenshot_4.png){ width=70% }

## DNS: обратная зона

![Файл обратной зоны](Screenshot_5.png){ width=70% }

## server.dgavdadaev.net

![Конфигурация server](Screenshot_6.png){ width=70% }

## www.dgavdadaev.net

![Конфигурация www](Screenshot_7.png){ width=70% }

## Веб-контент и структура каталогов

![Каталоги веб-контента](Screenshot_8.png){ width=70% }

## Страницы виртуальных хостов

![server.dgavdadaev.net](Screenshot_9.png){ width=70% }

![www.dgavdadaev.net](Screenshot_10.png){ width=70% }

## Создание структуры provision

![Копирование конфигураций](Screenshot_11.png){ width=70% }

## Скрипт автоматизации http.sh

![Содержимое http.sh](Screenshot_12.png){ width=70% }

# Выводы

## Итог работы

- Установлен и настроен HTTP-сервер Apache.  
- Настроены файлы конфигурации и межсетевой экран.  
- Проведён анализ логов и тестирование работы сервера.  
- Реализован виртуальный хостинг для двух доменных имён.  
- Подготовлена структура для автоматизированного provisioning в Vagrant.  
- Созданы тестовые веб-страницы и проверена корректность их работы.
