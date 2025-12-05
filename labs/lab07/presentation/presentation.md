---
lang: ru-RU
title: Администрирование сетевых подсистем
subtitle: Лабораторная работа №7
author:
  - Авдадаев Джамал Геланиевич
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 28 ноября 2025

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

# Цели и задачи работы

## Цель лабораторной работы

Получение практических навыков настройки межсетевого экрана firewalld, создания пользовательских служб, перенаправления портов и включения Masquerading.

# Ход выполнения

## Создание пользовательской службы firewalld

![Создание файла ssh-custom.xml](Screenshot_1.png){ width=70% }

## Изменение порта и описания службы

![Редактирование ssh-custom.xml](Screenshot_2.png){ width=70% }

## Проверка списка служб и активация

![Активация службы](Screenshot_3.png){ width=70% }

## Настройка перенаправления порта

![Настройка перенаправления 2022→22](Screenshot_4.png){ width=70% }

## Подключение клиента по новому порту

![SSH через порт 2022](Screenshot_5.png){ width=70% }

## Включение IPv4 forwarding и masquerading

![Включение forwarding и masquerading](Screenshot_6.png){ width=70% }

## Подготовка конфигурации для Vagrant provision

![Копирование конфигураций в provision](Screenshot_7.png){ width=70% }

## Скрипт автоматизации firewall

![firewall.sh](Screenshot_8.png){ width=70% }

# Выводы

## Итог лабораторной работы

Была создана пользовательская служба firewalld с новым портом, выполнено перенаправление трафика, включено пересылание IPv4-пакетов и настроен маскарадинг.  
SSH-подключение по новому порту проверено с клиентской машины.  
Конфигурации перенесены в директорию Vagrant provisioning, создан автоматический скрипт firewall.sh.  
Итоговая среда успешно функционирует и готова для повторного развёртывания.
