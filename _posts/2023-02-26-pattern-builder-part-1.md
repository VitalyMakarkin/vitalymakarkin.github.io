---
layout: post
title: "Паттерн проектирования - Builder. Часть 1"
date: 2023-02-26 12:00:00 +0300
tags: [pattern,kotlin]
---
[Статья в разработке]\
\
Часто в работе приходится сталкиваться с различными подходами программирования. Паттерн Builder, он же Строитель, один из наиболее используемых. С его помощью можно эффективно создавать объекты со специфичной конфигурацией. Именно поэтому этот паттерн часто испоьзуется для создания объектов конфигурации.\
\
Простой случай на примере создания экземпляра класса Device:

{% highlight kotlin %}
val notebook = DeviceBuilder()
    .setName("Notebook")
    .setSize(13.3F)
    .setWeight(1.5F)
    .build()
{% endhighlight %}

Здесь описано некое устройство с наименованием Notebook, размером 13.3 дюймов и весом 1.5 кг.\
\
Реализация классов Device и DeviceBuilder:

{% highlight kotlin %}
class Device {
    var name: String = ""
    var size: Float = 0F
    var weight: Float = 0F

    override fun toString(): String {
        return "$name (size: $size, weight: $weight)"
    }
}

class DeviceBuilder {

    private var name: String = ""
    private var size: Float = 0F
    private var weight: Float = 0F

    fun setName(value: String): DeviceBuilder {
        name = value
        return this
    }

    fun setSize(value: Float): DeviceBuilder {
        size = value
        return this
    }

    fun setWeight(value: Float): DeviceBuilder {
        weight = value
        return this
    }

    fun build(): Device {
        val device = Device()
        device.name = name
        device.size = size
        device.weight = weight
        return device
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