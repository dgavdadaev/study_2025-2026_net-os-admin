---
## Front matter
lang: ru-RU
title: Администрирование сетевых подсистем
subtitle: Лабораторная работа №16
author:
  - Авдадаев Джамал Геланиевич
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 11 декабря 2025

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

# Цели и задачи

## Цель лабораторной работы

Получить навыки по настройке Fail2ban  
для защиты от атак типа «brute force».

# Установка и запуск Fail2ban

## Установка пакета

![Установка Fail2ban](Screenshot_1.png){ width=70% } :contentReference[oaicite:0]{index=0}

## Первичный запуск

![Запуск Fail2ban](Screenshot_1.png){ width=70% } :contentReference[oaicite:1]{index=1}

# Первичная проверка работы

## Просмотр журнала

![Просмотр лога](Screenshot_2.png){ width=70% } :contentReference[oaicite:2]{index=2}

# Настройка Fail2ban

## Включение защиты SSH

![Конфигурация SSH](Screenshot_3.png){ width=70% } :contentReference[oaicite:3]{index=3}

## Запуск jail-ов SSH

![Запуск SSH jail](Screenshot_4.png){ width=70% } :contentReference[oaicite:4]{index=4}

# Защита HTTP-служб

## Включение HTTP jail-ов

![HTTP jail](Screenshot_5.png){ width=70% } :contentReference[oaicite:5]{index=5}

## Запуск HTTP jail-ов

![Работа HTTP jail](Screenshot_6.png){ width=70% } :contentReference[oaicite:6]{index=6}

# Защита почтовых служб

## Включение почтовых jail-ов

![Mail jail](Screenshot_7.png){ width=70% } :contentReference[oaicite:7]{index=7}

## Запуск почтовых jail-ов

![Работа почтовых jail](Screenshot_8.png){ width=70% } :contentReference[oaicite:8]{index=8}

# Проверка защиты SSH

## Общий статус Fail2ban

![Статус Fail2ban](Screenshot_9.png){ width=70% } :contentReference[oaicite:9]{index=9}

## Блокировка при ошибке входа

![Блокировка IP](Screenshot_10.png){ width=70% } :contentReference[oaicite:10]{index=10}

## Разблокировка IP

![Unban](Screenshot_10.png){ width=70% } :contentReference[oaicite:11]{index=11}

## Добавление IP в ignoreip

![ignoreip](Screenshot_11.png){ width=70% } :contentReference[oaicite:12]{index=12}

## Проверка игнорирования IP

![Игнорирование IP](Screenshot_12.png){ width=70% } :contentReference[oaicite:13]{index=13}

# Вывод

## Основные результаты

- Настроена защита SSH, HTTP и почтовых служб  
- Проверена блокировка и разблокировка IP  
- Настроен механизм ignoreip  
- Подготовлен provisioning Fail2ban  
- Fail2ban успешно защищает сервер от brute-force атак
