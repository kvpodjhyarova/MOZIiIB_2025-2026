---
## Front matter
title: "Лабораторная работа №1. Шифры простой замены"
author: "Подъярова Ксения Витальевна"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: false # List of figures
lot: false # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: IBM Plex Serif
romanfont: IBM Plex Serif
sansfont: IBM Plex Sans
monofont: IBM Plex Mono
mathfont: STIX Two Math
mainfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
romanfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
sansfontoptions: Ligatures=Common,Ligatures=TeX,Scale=MatchLowercase,Scale=0.94
monofontoptions: Scale=MatchLowercase,Scale=0.94,FakeStretch=0.9
mathfontoptions:
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Цель работы

Целью данной работы является изучение алгоритмов шифрования Цезарь и Атбаш,
принцип его работы, реализация на Julia.

# Задание

1. Реализовать шифр Цезаря с произвольным ключем k.
2. Реализовать шифр Атбаш.

# Выполнение лабораторной работы

## Шифр Цезаря

Суть шифра Цезаря заключается в том, что происходит смещение всех букв по
алфавиту в сообщении на некоторый коеффициент k. Декодирование происходим путем
смещения в обратную сторону.

Далее приведена реализация как для русского так и для английского алфавита
одновременно

```julia
result = ""
for c in msg
    if 1041 < Int(c) < 1104
	base = (uppercase(c) == c) ? codepoint('А') : codepoint('а')
	# 31 - так как в ASCII ё -- пропущена в списке
	t = base + (Int(Char(c)) % base + key) % 31
    else
	base = (uppercase(c) == c) ? codepoint('A') : codepoint('a')
	t = base + (Int(Char(c)) % base + key) % 26
    end
    key_rot = Char(t)
    result = result * key_rot
end
```

В качестве параметров скрипт принимает:
 
 - `<enc>` --- (Тип: Char) Расшивровать или шифровать сообщение (Возможные
значения: `d`, `e`).

 - `<msg>` --- (Тип: String) Сообщение, с которым нужно прозвести действие.
 
 - `<key>` --- (Тип: Int) Значение сдвига в шифре Цезаря. (Для русского алфавита
в промежутке `[0, 31]`, для английского алфавита в промежутке `[0, 26]`)

```
$ julia caesar.jl e test 3
whvw

$ julia caesar.jl d whvw 3
test
```

## Шифр Атбаш

Шифр Атбаш, отчасти, похож на шифр Цезаря, но в данном алгоритме
разворачивается весь алфавит, а не происходит сдвиг.

```julia
function atbash(msg, alp, rev)
        result = ""
        for i in msg
                c = rev[findfirst(i, alp)]
                result = result * c
        end
        result
end
```
В качестве параметров скрипт принимает:

 - `<enc>` --- (Тип: Char) Расшивровать или шифровать сообщение (Возможные
 значения: `d`, `e`).

 - `<msg>` --- (Тип: String) Сообщение, с которым нужно прозвести действие.

 - `<alp>` --- (Тип: String) Словарь из которого, можно составить данное
 сообщение.

```
$ julia atbash.jl e "test test" " abcdefghijklmnopqrstuvwxyz"
fugfzfugf
test test
```


# Выводы

В данной лабораторной работе были изучены два алгоритма шифрования: Цезарь и Атбаш, оба алгоритма были реализованы на языке Julia и работают корректно.

