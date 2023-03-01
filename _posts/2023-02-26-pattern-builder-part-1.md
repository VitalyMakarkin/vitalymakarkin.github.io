---
layout: post
title: "Паттерн проектирования - Builder. Часть 1"
date: 2023-02-26 12:00:00 +0300
tags: [pattern,kotlin]
---
Часто в работе приходится сталкиваться с различными подходами программирования. Паттерн Builder, он же Строитель, один из наиболее используемых. С его помощью можно эффективно создавать объекты со специфичной конфигурацией. Именно поэтому этот паттерн часто используется для создания объектов конфигурации.\
\
Пример:

Необходимо создать объект, который представляет собой реальное физическое устройство с определенными характеристиками.
Такое устройство удобно описать с помощью класса Device:

{% highlight kotlin %}
data class Device(
    val name: String,
    val totalWeight: Float = 0F,
    val screenSize: Float? = null
) {

    override fun toString() = StringBuilder("$name - total weight: $totalWeight")
        .apply { screenSize?.let { append(", screen size: $screenSize") } }
        .toString()
}
{% endhighlight %}

Класс обладает всего тремя свойствами:
1. name - наименование;
2. totalWeight - общий вес;
3. screenSize - размер экрана.

Однако, не исключено, что этот класс может приобрести десяток, а то и сотню других новых свойств,
часть из которых может быть определено по умолчанию или являться опциональными.\
Переопределенный метод toString() сразу предпологает возможное отсутсвие свойства screenSize.\
\
Для удобства создания экземляров Device определяется класс DeviceBuilder:

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

Метод build() включает в себя немного бизнес-логики, т.е. предполагает обязательное задание свойства 'name' (имени устройства) , 
в противном случае устройство не будет создано.\
\
Теперь создание устройств управляется простой конфигурацией:

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

В результате создаются устройства только с возможными для них характеристиками:

{% highlight kotlin %}
Macbook Air - total weight: 1.29, screen size: 13.3 
IPad Air - total weight: 0.46, screen size: 10.9
Pencil - total weight: 0.021
Air Pods - total weight: 0.042
{% endhighlight %}