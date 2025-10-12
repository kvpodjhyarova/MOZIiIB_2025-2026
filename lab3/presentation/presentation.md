---
## Front matter
lang: ru-RU
title: Шифры простой замены
author:
  - Подъярова Ксения Витальевна.
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 08 сентябр 2024

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
mainfont: DejaVu Serif
romanfont: DejaVu Serif
sansfont: DejaVu Sans
monofont: DejaVu Sans Mono
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
---

## Цель работы

Целью данной работы является изучение алгоритмов шифрования Цезарь и Атбаш,
принцип его работы, реализация на Julia.

## Задание

1. Реализовать шифр Цезаря с произвольным ключем k.
2. Реализовать шифр Атбаш.

# Выполнение лабораторной работы

## Шифр Цезаря

Суть шифра Цезаря заключается в том, что происходит смещение всех букв по
алфавиту в сообщении на некоторый коеффициент k. Декодирование происходим путем
смещения в обратную сторону.

Далее приведена реализация как для русского так и для английского алфавита
одновременно

## Шифр Цезаря

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

## Шифр Цезаря

В качестве параметров скрипт принимает:
 
 - `<enc>` --- (Тип: Char) Расшивровать или шифровать сообщение (Возможные
значения: `d`, `e`).

 - `<msg>` --- (Тип: String) Сообщение, с которым нужно прозвести действие.
 
 - `<key>` --- (Тип: Int) Значение сдвига в шифре Цезаря. (Для русского алфавита
в промежутке `[0, 31]`, для английского алфавита в промежутке `[0, 26]`)

## Шифр Цезаря

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

## Шифр Атбаш

В качестве параметров скрипт принимает:

 - `<enc>` --- (Тип: Char) Расшивровать или шифровать сообщение (Возможные
 значения: `d`, `e`).

 - `<msg>` --- (Тип: String) Сообщение, с которым нужно прозвести действие.

 - `<alp>` --- (Тип: String) Словарь из которого, можно составить данное
 сообщение.

## Шифр Атбаш

```
$ julia atbash.jl e "test test" " abcdefghijklmnopqrstuvwxyz"
fugfzfugf
test test
```

## Выводы

В данной лабораторной работе были изучены два алгоритма шифрования: Цезарь и
Атбаш, оба алгоритма были реализованы на языке Julia и работают корректно.

