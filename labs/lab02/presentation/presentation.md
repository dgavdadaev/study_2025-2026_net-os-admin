---
## Front matter
lang: ru-RU
title: Администрирование сетевых подсистем
subtitle: Лабораторная работа №2
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
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
---

# Цели и задачи работы

## Цель лабораторной работы  
Приобретение практических навыков установки, конфигурирования и тестирования DNS-сервера, а также понимания принципов работы системы доменных имён.

# Ход выполнения

## Проверка работы DNS-клиента  
![dig www.yandex.ru](Screenshot_1.png){ width=70% }

## Анализ конфигурации DNS  
![Конфигурация DNS](Screenshot_2.png){ width=70% }  

## Анализ конфигурации DNS  
![Root-зона named.ca](Screenshot_3.png){ width=70% }  

## Анализ конфигурации DNS  

![localhost и loopback зоны](Screenshot_4.png){ width=70% }

## Сравнение работы внешнего и локального DNS  
![dig через внешний и локальный сервер](Screenshot_5.png){ width=70% }

## Настройка локального DNS сервера  
![Настройка NetworkManager](Screenshot_6.png){ width=70% }

## Изменение named.conf  
![Правки named.conf](Screenshot_7.png){ width=70% }

## Проверка работы UDP-порта 53  
![Проверка UDP 53](Screenshot_8.png){ width=70% }

## Создание файла зон  
![Создание user.net](Screenshot_9.png){ width=70% }

## Настройка прямой зоны  
![Прямая зона](Screenshot_10.png){ width=70% }

## Настройка обратной зоны  
![Обратная зона](Screenshot_11.png){ width=70% }

## Перезапуск службы named  
![Статус named](Screenshot_12.png){ width=70% }

## Проверка зоны dig  
![dig ns.user.net](Screenshot_13.png){ width=70% }

## Проверка работы host  
![host проверка](Screenshot_14.png){ width=70% }

## Копирование конфигурации DNS  
![Файлы в /vagrant](Screenshot_15.png){ width=70% }

## Создание provisioning-скрипта dns.sh  
![dns.sh](Screenshot_16.png){ width=70% }

# Выводы

## Итог лабораторной работы  
Настроены прямая и обратная DNS-зоны, изменены параметры BIND, восстановлены права и контекст SELinux, проверена корректность работы DNS-службы.  
С помощью dig и host подтверждено функционирование сервера имён.  
Конфигурация перенесена в каталог Vagrant для автоматизированного развёртывания.  
