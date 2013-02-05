---
layout: recipe
title: 数组最大值
chapter: Arrays
---
## 问题

你需要找到包含在数组中的最大值。

## 方法

你可以使用JavaScript的Math.max()方法与图示。

{% highlight coffeescript %}
Math.max [12, 32, 11, 67, 1, 3]...
# => 67
{% endhighlight %}

或者，也可以使用ES5的`reduce`方法。为了向后兼容旧的JavaScript实现，使用Math.max.apply：

{% highlight coffeescript %}
# ECMAScript 5
[12,32,11,67,1,3].reduce (a,b) -> Math.max a, b
# => 67

# Pre-ES5
Math.max.apply(null, [12,32,11,67,1,3])
# => 67
{% endhighlight %}

## 讨论

`Math.max`比较每一个参数并返回参数中的最大值。省略号(`...`)转变每一个数组值为传入function的参数。您还可以使用它与其他接收可变参数数量的方法，例如`console.log`