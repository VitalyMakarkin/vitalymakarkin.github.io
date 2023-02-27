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
val device = DeviceBuilder()
    .setName("Notebook")
    .setWeight(1.5F)
    .setSize(13.3F)
    .build()
{% endhighlight %}

Здесь описано некое устройство с наименованием Notebook, размером 13.3 дюймов и весом 1.5 кг.\
\
Реализация класса данных Device:

{% highlight kotlin %}
class Device {

    var name: String = ""
    var weight: Float = 0F
    var size: Float = 0F

    override fun toString(): String {
        return "$name - weight: $weight, size: $size"
    }
}
{% endhighlight %}

Реализация класса-строителя DeviceBuilder:

{% highlight kotlin %}
class DeviceBuilder {

    private var name: String = ""
    private var weight: Float = 0F
    private var size: Float = 0F

    fun setName(value: String): DeviceBuilder {
        name = value
        return this
    }

    fun setWeight(value: Float): DeviceBuilder {
        weight = value
        return this
    }

    fun setSize(value: Float): DeviceBuilder {
        size = value
        return this
    }

    fun build(): Device {
        val device = Device()
        device.name = name
        device.weight = weight
        device.size = size
        return device
    }
}
{% endhighlight %}

Вывод результата работы программы, переопределив ранее метод toString у класса Device:

{% highlight kotlin %}
print(notebook)
{% endhighlight %}

Результат:

{% highlight kotlin %}
Notebook - weight: 1.5, size: 13.3
{% endhighlight %}

Паттерн реализован, однако он не расскрывает свои преимущества.\
\
Расширенный вариант:

{% highlight kotlin %}
data class Device(
    val name: String,
    val totalWeight: Float,
    val screenSize: Float? = null
) {

    override fun toString() = StringBuilder("$name - total weight: $totalWeight")
        .apply { screenSize?.let { append(", screen size: $screenSize") } }
        .toString()
}
{% endhighlight %}

{% highlight kotlin %}
class DeviceBuilder {

    private var name: String? = null
    private var totalWeight: Float = 0F
    private var screenSize: Float? = null

    fun setName(value: String) = apply {
        this.name = value
    }

    fun setTotalWeight(value: Float) = apply {
        this.totalWeight = value
    }

    fun setScreenSize(value: Float) = apply {
        this.screenSize = value
    }

    fun build(): Device {
        return name
            ?.let { Device(it, totalWeight, screenSize) }
            ?: throw Exception("Property 'name' is not implemented")
    }
}
{% endhighlight %}

{% highlight kotlin %}
val firstDevice = DeviceBuilder()
    .setName("Macbook Air")
    .setTotalWeight(1.29F)
    .setScreenSize(13.3F)
    .build()

print(firstDevice)

val secondDevice = DeviceBuilder()
    .setName("IPad Air")
    .setTotalWeight(0.46F)
    .setScreenSize(10.9F)
    .build()

print(secondDevice)

val thirdDevice = DeviceBuilder()
    .setName("Pencil")
    .setTotalWeight(0.021F)
    .build()

print(thirdDevice)


val fourthDevice = DeviceBuilder()
    .setName("Air Pods")
    .setTotalWeight(0.042F)
    .build()

print(fourthDevice)
{% endhighlight %}

Результат:

{% highlight kotlin %}
Macbook Air - total weight: 1.29, screen size: 13.3 
IPad Air - total weight: 0.46, screen size: 10.9
Pencil - total weight: 0.021
Air Pods - total weight: 0.042
{% endhighlight %}