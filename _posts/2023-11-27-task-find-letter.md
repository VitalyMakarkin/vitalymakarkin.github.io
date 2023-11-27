---
layout: post
title: "Задача - Найти добавленную букву"
date: 2023-11-27 12:00:00 +0300
tags: [kotlin]
---

Условие:

Даны две строковых переменных a и b. 
Строка a генерируется случайной перетасовкой строки b с добавлением еще одной буквы в случайную позицию.\
Необходимо вернуть букву, добавленную к строке b.

Пример 1:\
Входные параметры: a = "draft", b = "adfgtr"\
Результат: "g"

Пример 2:\
Входные параметры: a = "had", b = "head"\
Результат: "e"

В качетсве решения предлагается реализовать функцию findLetter:
{% highlight kotlin %}
fun findLetter(a: String, b: String): Char {
    ...
}
{% endhighlight %}

Решение:

Из условия можно понять, что одна из букв будет встречаться чаще остальных. Это можно определить подсчитав сколько раз встречается та или иная буква.

{% highlight kotlin %}
fun findLetter(a: String, b: String): Char {
    // Подсчет букв в a
    val aMap = mutableMapOf<Char, Int>()
    a.forEach { char ->
        val aValue = aMap[char] ?: 0
        aMap[char] = aValue + 1
    }

    // Подсчет букв в b
    val bMap = mutableMapOf<Char, Int>()
    b.forEach { char ->
        val bValue = bMap[char] ?: 0
        bMap[char] = bValue + 1
    }

    // Сравниваем количество повторений каждой буквы в a и b
    aMap.forEach { (char, aCount) ->
        val bCount = bMap[char] ?: 0
        // Если буква из b встречается чаще - возвращаем ее
        if (bCount > aCount) return char
    }

    // Если не нашли совпадение по буквам, значит эта буква вовсе не была в a,
    // т.е. отличие имено в последней букве в b, которая отличается от всех букв в a
    return b.last()
}
{% endhighlight %}

Немного оптимизации, результат возвращается в момент подсчета в b:

{% highlight kotlin %}
fun findLetter(a: String, b: String): Char {
    val aMap = mutableMapOf<Char, Int>()
    a.forEach { char ->
        val aValue = aMap[char] ?: 0
        aMap[char] = aValue + 1
    }

    val bMap = mutableMapOf<Char, Int>()
    b.forEach { char ->
        val aValue = aMap[char] ?: 0
        val bValue = (bMap[char] ?: 0) + 1
        bMap[char] = bValue
        if (bValue > aValue) return char
    }
    return b.last()
}
{% endhighlight %}

[ Сейчас в доработке наиболее оптимальные решения ]