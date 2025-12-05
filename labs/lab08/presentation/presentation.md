---
lang: ru-RU
title: Настройка SMTP-сервера Postfix
subtitle: Лабораторная работа №8
author: Авдадаев Джамал Геланиевич
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 1 декабря 2025
theme: metropolis
aspectratio: 169
slide_level: 2
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
---

# Цель работы

## Основная цель

Приобретение практических навыков по установке и конфигурированию SMTP-сервера Postfix.

# Ход выполнения

## Установка Postfix и настройка firewall

![Установка Postfix и настройка firewall](Screenshot_1.png){width=85%}

## Настройка myorigin и mydomain

![Изменение параметров myorigin и mydomain](Screenshot_2.png){width=85%}

## Продолжение настройки параметров

![Изменение параметров myorigin и mydomain](Screenshot_3.png){width=85%}

## Мониторинг лога и отправка локального письма

![Мониторинг лога и отправка локального письма](Screenshot_4.png){width=85%}

## Продолжение мониторинга

![Мониторинг лога и отправка локального письма](Screenshot_5.png){width=85%}

## Установка Postfix на клиенте

![Установка Postfix и настройка IPv4 на клиенте](Screenshot_6.png){width=85%}

## Настройка inet_interfaces и mynetworks

![Настройка inet_interfaces и mynetworks](Screenshot_7.png){width=85%}

## Успешная доставка после изменения mynetworks

![Успешная доставка письма после настройки mynetworks](Screenshot_8.png){width=85%}

## Письмо, отправленное на доменный адрес без MX

![Полученное письмо с клиента перед настройкой MX](Screenshot_9.png){width=85%}

## Очередь Postfix при ошибке доставки

![Очередь сообщений Postfix при ошибке доставки](Screenshot_10.png){width=85%}

## Логи о недоставке

![Мониторинг maillog при ошибке доставки](Screenshot_11.png){width=85%}

## Файл прямой зоны с MX-записью

![Файл прямой DNS-зоны с MX-записью](Screenshot_12.png){width=85%}

## Файл обратной зоны

![Файл обратной DNS-зоны](Screenshot_13.png){width=85%}

## Логи успешной доставки

![Успешная доставка письма после настройки DNS и Postfix](Screenshot_14.png){width=85%}

## Полученное письмо после исправления MX

![Полученное письмо test2 после настройки MX](Screenshot_15.png){width=85%}

# Итоги

## Вывод
- Настроена почтовая система Postfix для локальной сети и доменной доставки  
- Реализована отправка писем между клиентом и сервером  
- Настроены MX и PTR-записи  
- Проверена доставка писем и работа очереди Postfix  
- Добавлена автоматизация конфигурации через Vagrant provisioning
