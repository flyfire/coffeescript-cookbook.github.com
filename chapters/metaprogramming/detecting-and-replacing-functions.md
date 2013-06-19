---
layout: recipe
title: 检测并创建缺失的函数 Detecting and Creating Missing Functions
chapter: Metaprogramming
---
## 问题 Problem

你想检测一个函数是否存在，如果不存在就创建这个函数（例如在Internet Explorer 8中的ECMAScript 5函数）。

You want to detect if a function exists and create it if it does not (such as an ECMAScript 5 function in Internet Explorer 8).

## 方案 Solution

使用`::`来检测这个函数，如果不存在就赋值上这个函数。

Use `::` to detect the function, and assign to it if it does not exist.

{% highlight coffeescript %}
unless Array::filter
  Array::filter = (callback) ->
    element for element in this when callback element

array = [1..10]

array.filter (x) -> x > 5
# => [6,7,8,9,10]
{% endhighlight %}

## 讨论 Discussion

在JavaScript中（在CoffeeScript也一样），对象有一个原型成员，定义了那些以此为原型的对象所有可用的成员方法。在CoffeeScript中，你可以使用`::`操作符来直接访问原型。

Objects in JavaScript (and thus, in CoffeeScript) have a prototype member that defines what member functions should be available on all objects based on that prototype. In CoffeeScript, you can access the prototype directly via the `::` operator.
