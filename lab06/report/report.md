---
# Front matter
lang: ru-RU
title: "Отчет по лабораторной работе №6"
subtitle: "Информационная безопасноть"
author: "Астафьева Анна Андреевна НПИбд-01-18"

# Formatting
toc-title: "Содержание"
toc: true # Table of contents
toc_depth: 2
lof: true # List of figures
fontsize: 12pt
linestretch: 1.5
papersize: a4paper
documentclass: scrreprt
polyglossia-lang: russian
polyglossia-otherlangs: english
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase
indent: true
pdf-engine: lualatex
header-includes:
  - \linepenalty=10 # the penalty added to the badness of each line within a paragraph (no associated penalty node) Increasing the υalue makes tex try to haυe fewer lines in the paragraph.
  - \interlinepenalty=0 # υalue of the penalty (node) added after each line of a paragraph.
  - \hyphenpenalty=50 # the penalty for line breaking at an automatically inserted hyphen
  - \exhyphenpenalty=50 # the penalty for line breaking at an explicit hyphen
  - \binoppenalty=700 # the penalty for breaking a line at a binary operator
  - \relpenalty=500 # the penalty for breaking a line at a relation
  - \clubpenalty=150 # extra penalty for breaking after first line of a paragraph
  - \widowpenalty=150 # extra penalty for breaking before last line of a paragraph
  - \displaywidowpenalty=50 # extra penalty for breaking before last line before a display math
  - \brokenpenalty=100 # extra penalty for page breaking after a hyphenated line
  - \predisplaypenalty=10000 # penalty for breaking before a display
  - \postdisplaypenalty=0 # penalty for breaking after a display
  - \floatingpenalty = 20000 # penalty for splitting an insertion (can only be split footnote in standard LaTeX)
  - \raggedbottom # or \flushbottom
  - \usepackage{float} # keep figures where there are in the text
  - \usepackage{amsmath}
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Цель работы

Развить навыки администрирования ОС Linux. Получить первое практическое знакомство с технологией SELinux. Проверить работу SELinx на практике совместно с веб-сервером Apache.

# Теоретическое описание

SELinux — набор технологий расширения системы безопасности Linux. Сегодня основу набора составляют три технологии: мандатный контроль доступа, ролевой доступ RBAC и система типов (доменов). Apache – это свободное программное обеспечение для размещения веб-сервера. Он хорошо показывает себя в работе с масштабными проектами, поэтому заслуженно считается одним из самых популярных веб-серверов. Кроме того, Apache очень гибок в плане настройки, что даёт возможность реализовать все особенности размещаемого веб-ресурса.

# Подготовка лабораторного стенда:

1. В конфигурационном файле /etc/httpd/httpd.conf  задала параметр ServerName. (рис. -@fig:001). 

![Параметр ServerName](image/1.png){ #fig:001 width=60% height=60% }

2. Также проследила, чтобы пакетный фильтр был отключён или в своей рабочей конфигурации позволял подключаться к 80-у и 81-у портам протокола tcp. Отключила фильтр командами: iptables -F, iptables -P INPUT ACCEPT iptables -P OUTPUT ACCEPT. Так же добавила разрешающие правила.
(рис. -@fig:002), (рис. -@fig:003). 

![Отключение фильтра](image/2.png){ #fig:002 width=70% height=70% }

![Добавление разрешающих правил](image/3.png){ #fig:003 width=70% height=70% }

# Выполнение лабораторной работы

1. Вошла в систему с полученными учётными данными и убедилась, что SELinux работает в режиме enforcing политики targeted с помощью команд getenforce и sestatus.(рис. -@fig:004). 

![Проверка](image/4.png){ #fig:004 width=70% height=70% }

2. Обратилась с помощью браузера к веб-серверу, запущенному на компьютере, и убедилась, что последний работает: service httpd status(рис. -@fig:005), (рис. -@fig:006).

![Обращение через браузер](image/5.png){ #fig:005 width=70% height=70% }

![Проверка](image/6.png){ #fig:006 width=70% height=70% }

3. Нашла веб-сервер Apache в списке процессов, определила его контекст безопасности (рис. -@fig:007). 

![веб-сервер Apache](image/7.png){ #fig:007 width=70% height=70% }

4. Посмотрела текущее состояние переключателей SELinux для Apache с помощью команды: sestatus -bigrep httpd. Обратила внимание, что многие из них находятся в положении «off». (рис. -@fig:008). 

![Просмотр состояние переключателей SELinux для Apache](image/8.png){ #fig:008 width=70% height=70% }

5. Посмотрела статистику по политике с помощью команды seinfo, также определила множество пользователей(8), ролей(14), типов(4793). Определила тип файлов и поддиректорий, находящихся в директории /var/www, с помощью команды: ls -lZ /var/www. Определила тип файлов, находящихся в директории /var/www/html: ls -lZ /var/www/html. Определила круг пользователей, которым разрешено создание файлов в директории /var/www/html. (рис. -@fig:009), (рис. -@fig:010). 

![Получение информации](image/9.png){ #fig:009 width=70% height=70% }

![Получение информации](image/10.png){ #fig:010 width=70% height=70% }

6. Создала от имени суперпользователя (так как в дистрибутиве после установки только ему разрешена запись в директорию) html-файл /var/www/html/test.html(рис. -@fig:011). 

![Создание файла](image/11.png){ #fig:011 width=70% height=70% }

7. Проверила контекст созданного файла. httpd_sys_content_t (рис. -@fig:012). 

![Проверка](image/12.png){ #fig:012 width=70% height=70% }

8. Обратилась к файлу через веб-сервер, введя в браузере адрес http://127.0.0.1/test.html. Убедилась, что файл был успешно отображён. (рис. -@fig:013). 

![Получение доступа к файлу через браузер](image/13.png){ #fig:013 width=50% height=50% }

9. Изменила контекст файла /var/www/html/test.html с httpd_sys_content_t на samba_share_t. После этого проверила, что контекст поменялся. Попробовала ещё раз получить доступ к файлу через веб-сервер, введя в браузере адрес http://127.0.0.1/test.html. Получили сообщение об ошибке. (рис. -@fig:014).

![Получение доступа к файлу через браузер](image/14.png){ #fig:014 width=50% height=50% }

12. Проанализировала ситуацию. Файл не был отображён потому что мы изменили контекст файла. Просмотрела log-файлы веб-сервера Apache. Также просмотрела системный лог-файл: tail /var/log/messages (рис. -@fig:015), (рис. -@fig:016). 

![Просмотр системного лог-файла](image/15.png){ #fig:015 width=50% height=50% }

![Просмотр системного лог-файла](image/16.png){ #fig:016 width=50% height=50% }

13. Попробовала запустить веб-сервер Apache на прослушивание ТСР-порта 81 (а не 80, как рекомендует IANA и прописано в /etc/services). Для этого в файле /etc/httpd/httpd.conf нашла строчку Listen 80 и замените её на Listen 81.(рис. -@fig:017). 

![Изменеие порта 80 на 81](image/17.png){ #fig:017 width=50% height=50% }

14. Проанализиировала лог-файлы. Просмотрела файлы /var/log/http/error_log, /var/log/http/access_log и /var/log/audit/audit.log. (рис. -@fig:018), (рис. -@fig:019), (рис. -@fig:020).

![Анализ лог-файла](image/18.png){ #fig:018 width=50% height=50% }

![Анализ файла](image/19.png){ #fig:019 width=50% height=50% }

![Анализ файла](image/20.png){ #fig:020 width=50% height=50% }

15. Выполнила команду: semanage port -a -t http_port_t -р tcp 81. После этого проверила список портов командой: semanage port -l | grep http_port_t. Убедилась, что порт 81 появился в списке. (рис. -@fig:021).

![Выполнение и проверка](image/21.png){ #fig:021 width=50% height=50% }

16. Вернула контекст httpd_sys_cоntent__t к файлу /var/www/html/test.html: chcon -t httpd_sys_content_t /var/www/html/test.html. После этого попробовала получить доступ к файлу через веб-сервер, введя в браузере адрес http://127.0.0.1:81/test.html. Увидели содержимое файла — слово «test». (рис. -@fig:022), (рис. -@fig:023).

![Возвращение контекста](image/22.png){ #fig:022 width=50% height=50% }

![Получение доступа к файлу через браузер](image/23.png){ #fig:023 width=50% height=50% }

17. Исправила обратно конфигурационный файл apache, вернув Listen 80. (рис. -@fig:024).

![Исправление конфигурационного файл apache](image/24.png){ #fig:024 width=50% height=50% }

18. Удалила привязку http_port_t к 81 порту. (рис. -@fig:025).

![Удалние привязки http_port_t к 81 порту](image/25.png){ #fig:025 width=50% height=50% }

19. Удалила файл /var/www/html/test.html. (рис. -@fig:026).

![Удаление файла /var/www/html/test.html](image/26.png){ #fig:026 width=50% height=50% }


# Выводы
На основе проделанной работы развила навыки администрирования ОС Linux. Получила первое практическое знакомство с технологией SELinux. Проверила работу SELinx на практике совместно с веб-сервером Apache.