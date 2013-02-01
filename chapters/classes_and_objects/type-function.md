---
layout: recipe
title: CoffeeScript式的Type函数
chapter: Classes and Objects
---
## 问题

不借助`typeof`获取一个对象的类型（关于为什么`typeof`不给力的原因请参考http://javascript.crockford.com/remedial.html，）。

## 方法

使用下面这个函数：

{% highlight coffeescript %}
type = (obj) ->
  if obj == undefined or obj == null
    return String obj
  classToType = new Object
  for name in "Boolean Number String Function Array Date RegExp".split(" ")
    classToType["[object " + name + "]"] = name.toLowerCase()
  myClass = Object.prototype.toString.call obj
  if myClass of classToType
    return classToType[myClass]
  return "object"
{% endhighlight %}

## 详解

这个函数参考了jQuery的`$.type`函数。（http://api.jquery.com/jQuery.type/）

注意，作为类型检测的替代，在某些场景下，你还可以使用鸭子类型结合存在操作符来避免对某个对象类型的检测。例如，下面是一段不会发生异常的代码，如果`myArray`确实是一个数组时（或者为类数组，，有`push`方法），把元素放到数组中，否则什么都不做。

{% highlight coffeescript %}
myArray?.push? myValue
{% endhighlight %}
