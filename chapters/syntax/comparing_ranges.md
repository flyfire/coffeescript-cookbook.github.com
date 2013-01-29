---
layout: recipe
title: 值域
chapter: Syntax
---
## 问题

怎么判断某个变量的值在一个区间内。

## 方法

使用CoffeeScript链式比较的语法。

{% highlight coffeescript %}
maxDwarfism = 147
minAcromegaly = 213

height = 180

normalHeight = maxDwarfism < height < minAcromegaly
# => true
{% endhighlight %}

## 详解

这是从Python移植过来的特性，无需像下面这样写出完整的比较：

{% highlight coffeescript %}
normalHeight = height > maxDwarfism && height < minAcromegaly
{% endhighlight %}

CoffeeScript允许我们把两个比较连在一起，这种形式更加符合数学上的书写方式。
