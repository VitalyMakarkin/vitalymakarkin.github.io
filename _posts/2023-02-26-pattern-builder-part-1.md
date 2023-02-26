---
layout: post
title: "Паттерн проектирования - Builder (Строитель). Часть 1"
date: 2023-02-26 12:00:00 +0300
tags: [pattern,kotlin]
---
[Статья в разработке]\
\
Часто в работе приходится сталкиваться с различными подходами программирования. Паттерн Builder один из наиболее используемых. С его помощью можно эффективно создавать объекты со специфичной конфигурацией. Именно поэтому этот паттерн часто испоьзуется для создания объектов конфигурации.\
\
Простой случай на примере создания экземпляра класса Device:

{% highlight kotlin %}
val notebook = Device()
    .setName("Notebook")
    .setSize(13.3F)
    .setWeight(1.5F)
    .build()
{% endhighlight %}

Здесь описано некое устройство с наименованием Notebook, размером 13.3 дюймов и весом 1.5 кг.\
\
Реализация класса Device:

{% highlight kotlin %}
class Device {

    private var name: String = ""
    private var size: Float = 0F
    private var weight: Float = 0F

    fun setName(value: String): Device {
        name = value
        return this
    }

    fun setSize(value: Float): Device {
        size = value
        return this
    }

    fun setWeight(value: Float): Device {
        weight = value
        return this
    }

    fun build(): Device {
        return this
    }

    override fun toString(): String {
        return "$name (size: $size, weight: $weight)"
    }
}
{% endhighlight %}

Вывод результата работы программы, переопределенного ранее метода toString:

{% highlight kotlin %}
print(notebook)
{% endhighlight %}

Результат:

{% highlight kotlin %}
Notebook (size: 13.3, weight: 1.5)
{% endhighlight %}