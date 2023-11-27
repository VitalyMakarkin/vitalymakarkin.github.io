---
layout: post
title: "LeetCode - Problem 389. Find the Difference"
date: 2023-11-27 12:00:00 +0300
tags: [leetcode,kotlin]
---

Условие:

Даны две строковых переменных s и t. 
Строка t генерируется случайной перетасовкой строки s с добавлением еще одной буквы в случайную позицию.\
Необходимо вернуть букву, добавленную к строке t.

Пример 1:\
Входные параметры: s = "draft", t = "adfgtr"\
Результат: "g"

Пример 2:\
Входные параметры: s = "had", t = "head"\
Результат: "e"

В качетсве решения предлагается реализовать функцию findTheDifference:
{% highlight kotlin %}
fun findTheDifference(s: String, t: String): Char {
    ...
}
{% endhighlight %}

Решение:

Из условия можно понять, что одна из букв будет встречаться чаще остальных. Это можно определить подсчитав сколько раз встречается та или иная буква.

{% highlight kotlin %}
fun findTheDifference(s: String, t: String): Char {
    // Подсчет букв в s
    val sMap = mutableMapOf<Char, Int>()
    s.forEach { char ->
        val value = sMap[char] ?: 0
        sMap[char] = value + 1
    }

    // Подсчет букв в t
    val tMap = mutableMapOf<Char, Int>()
    t.forEach { char ->
        val value = (tMap[char] ?: 0) + 1
        tMap[char] = value
    }

    // Сравниваем количество повторений каждой буквы в s и t
    sMap.forEach { (char, count) ->
        val tCount = tMap[char] ?: 0
        // Если буква из t встречается чаще - возвращаем ее
        if (tCount > count) return char
    }

    // Если не нашли совпадение по буквам, значит эта буква вовсе не была в s,
    // т.е. отличие имено в последней букве в t, которая отличается от всех букв в s
    return t.last()
}
{% endhighlight %}

Немного оптимизации, результат возвращается в момент подсчета в t:

{% highlight kotlin %}
fun findTheDifference(s: String, t: String): Char {
    val sMap = mutableMapOf<Char, Int>()
    s.forEach { char ->
        val value = sMap[char] ?: 0
        sMap[char] = value + 1
    }

    val tMap = mutableMapOf<Char, Int>()
    t.forEach { char ->
        val value = (tMap[char] ?: 0) + 1
        val sValue = sMap[char] ?: 0
        tMap[char] = value
        if (value > sValue) return char
    }
    return t.last()
}
{% endhighlight %}

[ Сейчас в доработке наиболее оптимальные решения ]