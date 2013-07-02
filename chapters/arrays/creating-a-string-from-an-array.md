---
layout: recipe
title: 数组转成字符串 Creating a String from an Array
chapter: Arrays
---
## 问题 Problem

你想把一个数组转成字符串。

You want to create a string from an array.

## 方法 Solution

使用JavaScript Array的toString()方法。

Use JavaScript's Array toString() method:

{% highlight coffeescript %}
["one", "two", "three"].toString()
# => 'one,two,three'
{% endhighlight %}

## 讨论 Discussion

`toString()`是一个标准的JavaScript方法。别忘了括号。

`toString()` is a standard JavaScript method. Don't forget the parentheses.
