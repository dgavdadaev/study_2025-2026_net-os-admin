---
## Front matter
lang: ru-RU
title: Администрирование сетевых подсистем
subtitle: Лабораторная работа №10
author:
  - Авдадаев Джамал Геланиевич
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 1 декабря 2025

## Formatting pdf
toc: false
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}

---

# Цели и задачи

## Цель работы
Приобретение практических навыков настройки SMTP-сервера:  
• LMTP  
• SMTP AUTH (SASL)  
• SMTP over TLS  
• интеграция Postfix и Dovecot  

---

# Процесс выполнения

## Добавление протокола LMTP

![Screenshot](Screenshot_2.png){ width=70% }

## Настройка сервиса LMTP

![Screenshot](Screenshot_1.png){ width=70% }

## Формат логинов для аутентификации

![Screenshot](Screenshot_3.png){ width=70% }

## Логи доставки LMTP

![Screenshot](Screenshot_4.png){ width=100% }

## Проверка Maildir

![Screenshot](Screenshot_5.png){ width=70% }

## Настройка службы auth в Dovecot

![Screenshot](Screenshot_6.png){ width=70% }

## Настройка ограничений SMTP

![Screenshot](Screenshot_7.png){ width=100% }

## Изменения в master.cf

![Screenshot](Screenshot_8.png){ width=100% }

## Проверка SMTP AUTH

![Screenshot](Screenshot_9.png){ width=70% }

## Настройка сертификатов и TLS в Postfix

![Screenshot](Screenshot_10.png){ width=100% }

## Изменения master.cf для submission (587)

![Screenshot](Screenshot_11.png){ width=100% }

## Настройка firewall

![Screenshot](Screenshot_12.png){ width=100% }

## Проверка подключения через openssl

![Screenshot](Screenshot_13.png){ width=70% }


## Отправка через Evolution

![Screenshot](Screenshot_14.png){ width=70% }

## Анализ логов сервера

![Screenshot](Screenshot_15.png){ width=100% }

## Maildir после тестирования

![Screenshot](Screenshot_16.png){ width=70% }

# Итоги

## Вывод
Были настроены:  
• LMTP-доставка Dovecot  
• SMTP AUTH (SASL)  
• защищённый SMTP-submission (587/TLS)  
• TLS-шифрование канала  
• автоматизация конфигурации в Vagrant  

SMTP-сервер работает корректно, письма доставляются успешно.

