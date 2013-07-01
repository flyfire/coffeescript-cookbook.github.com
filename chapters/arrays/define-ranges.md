---
layout: recipe
title: 定义区间数组 Define Ranges Array
chapter: Arrays
---
## 问题 Problem

你想在一个数组中定义一个区间。

You want to define a range in an array.

## 方案 Solution

在CoffeeScript中，有两种方法定义一系列的数组元素。

There are two ways to define a range of array elements in CoffeeScript.

{% highlight coffeescript %}

myArray = [1..10]
# => [ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 ]

{% endhighlight %}

{% highlight coffeescript %}

myArray = [1...10]
# => [ 1, 2, 3, 4, 5, 6, 7, 8, 9 ]

{% endhighlight %}

我可以使用下面的写法转置一系列的元素。

We can also reverse the range of element by writing it this way.

{% highlight coffeescript %}

myLargeArray = [10..1]
# => [ 10, 9, 8, 7, 6, 5, 4, 3, 2, 1 ]

{% endhighlight %}

{% highlight coffeescript %}
myLargeArray = [10...1]
# => [ 10, 9, 8, 7, 6, 5, 4, 3, 2 ]

{% endhighlight %}

## 讨论 Discussion

闭区间通常使用`..`操作符来定义。

Inclusive range always define by '..' operator.

开区间使用`...`操作符来定义，通常会省略掉最后一个值。

Exclusive range define by '...', and always omit the last value. 
