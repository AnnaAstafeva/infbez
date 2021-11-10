---
# Front matter
lang: ru-RU
title: "Отчет по лабораторной работе №5"
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

Изучить механизмы изменения идентификаторов, применение SetUID- и Sticky-битов. Получить практические навыки работы в консоли с дополнительными атрибутами. Рассмотреть работу механизма смены идентификатора процессов пользователей, а также влияние бита Sticky на запись и удаление файлов. 

# Теоретическое описание

В Linux, как и в любой многопользовательской системе, абсолютно естественным образом возникает задача разграничения доступа субъектов — пользователей к объектам — файлам дерева каталогов.

Setuid, Setgid и Sticky Bit - это специальные типы разрешений позволяют задавать расширенные права доступа на файлы или каталоги. 
Setuid – это бит разрешения, который позволяет пользователю запускать исполняемый файл с правами владельца этого файла. Другими словами, использование этого бита позволяет нам поднять привилегии пользователя в случае, если это необходимо. 
Принцип работы Setgid очень похож на setuid с отличием, что файл будет запускаться пользователем от имени группы, которая владеет файлом. 
Последний специальный бит разрешения – это Sticky Bit . В случае, если этот бит установлен для папки, то файлы в этой папке могут быть удалены только их владельцем. 


# Выполнение лабораторной работы

1. Вошла в систему от имени пользователя guest,  создала программу simpleid.c. (рис. -@fig:001), (рис. -@fig:002). 

![Создание файла simpleid.c](image/1.png){ #fig:001 width=70% height=70% }

![Написание программы simpleid.c](image/2.png){ #fig:002 width=70% height=70% }

2. Скомплировала программу и убедилась, что файл программы создан: gcc simpleid.c -o simpleid. Выполнила программу simpleid: ./simpleid. Выполнила системную программу id. И сравнила полученный результат с данными предыдущего пункта задания. (Данные одинаковы)(рис. -@fig:003). 

![Компиляция, выполнение программы](image/3.png){ #fig:003 width=70% height=70% }

3. Усложнила программу, добавив вывод действительных идентификаторов. (рис. -@fig:004). 

![Создание программы simpleid2.c](image/4.png){ #fig:004 width=70% height=70% }

4. Скомпилировала и запустила simpleid2.c: gcc simpleid2.c -o simpleid2; ./simpleid2 (рис. -@fig:005). 

![Компиляция, выполнение программы](image/5.png){ #fig:005 width=70% height=70% }

5. От имени суперпользователя выполнила команды: chown root:guest /home/guest/simpleid2; chmod u+s /home/guest/simpleid2. (рис. -@fig:006).

![Выполнение](image/6.png){ #fig:006 width=70% height=70% }

С помощью первой команды для файла simpleid2 мы поменяли пользователя и группу на root и guest соответственно. С помощью второй установили разрешение для владельца на выполнение с разрешением суперпользователя.

6. Выполнила проверку правильности установки новых атрибутов и смены владельца файла simpleid2: ls -l simpleid2. Запустила simpleid2 и id.  (рис. -@fig:007).

![Выполнение](image/7.png){ #fig:007 width=70% height=70% }

7. Создала и откомпилировала программу readfile.c: (рис. -@fig:008). 

![Создание и компиляция программы readfile.c](image/8.png){ #fig:008 width=50% height=50% }

8. Сменила владельца у файла readfile.c и изменила права так, чтобы только суперпользователь (root) мог прочитать его, a guest не мог. (рис. -@fig:009). 

![Изменение владельца и прав](image/9.png){ #fig:009 width=50% height=50% }

9. Проверила, что пользователь guest не может прочитать файл readfile.c. (рис. -@fig:0010). 

![Проверка](image/10.png){ #fig:0010 width=50% height=50% }

10. Сменила у программы readfile владельца и установила SetU’D-бит. (рис. -@fig:0011). 

![Изменение для программы readfile](image/11.png){ #fig:0011 width=50% height=50% }

11. Проверила, может ли программа readfile прочитать файл readfile.c (может), проверила, может ли программа readfile прочитать файл /etc/shadow (может).  (рис. -@fig:0012). (рис. -@fig:0013). 

![Проверка](image/12.png){ #fig:0012 width=50% height=50% }

![Проверка](image/13.png){ #fig:0013 width=50% height=50% }

12. Выяснила, установлен ли атрибут Sticky на директории /tmp, для чего выполнила команду: ls -l / | grep tmp. От имени пользователя guest создала файл file01.txt в директории /tmp со словом test: echo "test" > /tmp/file01.txt. Просмотрела атрибуты у только что созданного файла и разрешила чтение и запись для категории пользователей «все остальные»: ls -l /tmp/file01.txt, chmod o+rw /tmp/file01.txt, ls -l /tmp/file01.txt. (рис. -@fig:0014). 

![Выполение](image/14.png){ #fig:0014 width=50% height=50% }

13. От пользователя guest2 (не являющегося владельцем) попробовала прочитать файл /tmp/file01.txt: cat /tmp/file01.txt, попробовала дозаписать в файл /tmp/file01.txt слово test2 командой: echo "test2" >> /tmp/file01.txt. Проверила содержимое файла командой: cat /tmp/file01.txt. Также попробовала записать в файл /tmp/file01.txt
слово test3, стерев при этом всю имеющуюся в файле информацию командой: echo "test3" > /tmp/file01.txt
От пользователя guest2 попробовала удалить файл /tmp/file01.txt командой: rm /tmp/fileOl.txt. (Все действия, кроме удаления файла, выполнить удалось). (рис. -@fig:0015). 

![Выполение и проверка от пользователя guest2](image/15.png){ #fig:0015 width=50% height=50% }

14. От суперпользователя выполнила команду, снимающую атрибут t (Sticky-бит) с директории /tmp: chmod -t /tmp. (рис. -@fig:0016). 

![Снятие атрибута "t" с директории /tmp](image/16.png){ #fig:0016 width=50% height=50% }

15. От пользователя guest2 проверила, что атрибута t у директории /tmp нет: ls -l / | grep tmp. Повторила предыдущие шаги. Нам удалось удалить файл от имени пользователя, не являющегося его владельцем, также получилось выполнить дозапись в файл и замену текста в файле. (рис. -@fig:0017). 

![Проверка](image/17.png){ #fig:0017 width=50% height=50% }

16. От суперпользователя вернула атрибут t на директорию /tmp: chmod +t /tmp. (рис. -@fig:0018). 

![Добавление атрибута "t" на директорию /tmp](image/18.png){ #fig:0018 width=50% height=50% }



# Выводы

На основе проделанной работы я изучила механизмы изменения идентификаторов, применение SetUID- и Sticky-битов. Получла практические навыки работы в консоли с дополнительными атрибутами. Рассмотрела работу механизма смены идентификатора процессов пользователей, а также влияние бита Sticky на запись и удаление файлов.