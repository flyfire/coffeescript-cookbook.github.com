---
layout: recipe
title: 过滤数组 Filtering Arrays
chapter: Arrays
---
## 问题 Problem

你想基于某个布尔条件来过滤数组。

You want to be able to filter arrays based on a boolean condition.

## 方法 Solution

使用`Array.filter`（ECMAScript 5）:

Use Array.filter (ECMAScript 5):

{% highlight coffeescript %}
array = [1..10]

array.filter (x) -> x > 5
# => [6,7,8,9,10]
{% endhighlight %}

在pre-EC5的实现中，给Array的原型扩展一个过滤函数，这个函数接受一个回调函数，并迭代整个数组，收集所有调用过滤函数的为true的元素。在复写这个函数的之前需要先确保这个是否已经实现了：

In pre-EC5 implementations, extend the Array prototype to add a filter function which will take a callback and perform a comprension over itself, collecting all elements for which the callback is true. Be sure to check if the function is already implemented before overwriting it:

{% highlight coffeescript %}
# Extending Array's prototype
unless Array::filter
  Array::filter = (callback) ->
    element for element in this when callback(element)

array = [1..10]

# Filter odd elements
filtered_array = array.filter (x) -> x % 2 == 0
# => [2,4,6,8,10]

# Filter elements less than or equal to 5:
gt_five = (x) -> x > 5
filtered_array = array.filter gt_five
# => [6,7,8,9,10]
{% endhighlight %}

## 讨论 Discussion

这个方法的用法与Ruby中Array的select方法类似。

This is similar to using ruby's Array#select method.
