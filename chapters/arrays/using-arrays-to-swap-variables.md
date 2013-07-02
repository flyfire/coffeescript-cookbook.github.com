---
layout: recipe
title: 使用数组来做值交换 Using Arrays to Swap Variables
chapter: Arrays
---
## 问题 Problem

你想使用数组来做值交换。

You want to use an array to swap variables.

## 方法 Solution

使用CoffeeScript的[解构赋值](http://jashkenas.github.com/coffee-script/#destructuring)语法：

Use CoffeeScript's [destructuring assignment](http://jashkenas.github.com/coffee-script/#destructuring) syntax:

{% highlight coffeescript %}
a = 1
b = 3

[a, b] = [b, a]

a
# => 3

b
# => 1
{% endhighlight %}

## 讨论 Discussion

解构赋值允许交换两个值，而无需使用中间变量。

Destructuring assignment allows swapping two values without the use of a temporary variable.

当迭代数组需要保证迭代最短的数组时，这个特性相当有用：

This can be useful when traversing arrays and ensuring iteration only happens over the shortest one:

{% highlight coffeescript %}

ray1 = [ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 ]
ray2 = [ 5, 9, 14, 20 ]

intersection = (a, b) ->
  [a, b] = [b, a] if a.length > b.length
  value for value in a when value in b

intersection ray1, ray2
# => [ 5, 9 ]

intersection ray2, ray1
# => [ 5, 9 ]

{% endhighlight %}
