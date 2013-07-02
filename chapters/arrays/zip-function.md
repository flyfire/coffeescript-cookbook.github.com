---
layout: recipe
title: Python式的Zip函数 Python-like Zip Function
chapter: Arrays
---
## 问题 Problem

你想把多个数组压缩到一个以数组为元素的数组中，类似于Python的zip函数。Python的zip函数返回的是一个元组数组，每个元组包含的每一个参数数组中第i个元素。

You want to zip together multiple arrays into an array of arrays, similar to Python's zip function.  Python's zip function returns an array of tuples, where each tuple contains the i-th element from each of the argument arrays.

## 方法 Solution

是用下面这段CoffeeScript：

Use the following CoffeeScript code:

{% highlight coffeescript %}
# Usage: zip(arr1, arr2, arr3, ...)
zip = () ->
  lengthArray = (arr.length for arr in arguments)
  length = Math.min(lengthArray...)
  for i in [0...length]
    arr[i] for arr in arguments

zip([0, 1, 2, 3], [0, -1, -2, -3])
# => [[0, 0], [1, -1], [2, -2], [3, -3]]
{% endhighlight %}
