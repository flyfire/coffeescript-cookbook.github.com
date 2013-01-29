---
layout: recipe
title: 类变量
chapter: Classes and Objects
---
## 问题

你需要一个类变量。

## 方法

{% highlight coffeescript %}
class Zoo
  @MAX_ANIMALS: 50
  MAX_ZOOKEEPERS: 3

Zoo.MAX_ANIMALS
# => 50

Zoo.MAX_ZOOKEEPERS
# => undefined (it is an instance variable)

zoo = new Zoo
zoo.MAX_ZOOKEEPERS
# => 3
{% endhighlight %}

## 详解

CoffeeScript会把这些值直接存储在这个对象（构造函数）中，而不是其原型上（即也不会再独立的实例上），这样不但节省了内存，还提供了一个中心，用于存储类级别的值。
