---
layout: recipe
title: 生成随机数
chapter: Math
---
## 问题

你需要在一定范围内生成一个随机数。

## 方法

使用JavaScript的 Math.random()方法去得到一个0到1.0之间（0 <= x < 1.0）的浮点型随机数。
再使用乘法和Math.floor()方法得到一定范围内的这个数字。

{% highlight coffeescript %}
probability = Math.random()
0.0 <= probability < 1.0
# => true

# 需要注意的是百分位没有达到100。 一个完整的范围为0至100,实际上它是一个跨度为101的范围。

percentile = Math.floor(Math.random() * 100)
0 <= percentile < 100
# => true

dice = Math.floor(Math.random() * 6) + 1
1 <= dice <= 6
# => true
{% endhighlight %}

## 讨论

这是从JavaScript的一个直线提升。

需要注意的是JavaScript的Math.random() 不允许随机数生成器强制生成特定的值。请参看 [生成可预测随机数](/coffeescript-cookbook.github.com/chapters/math/generating-predictable-random-numbers)。

要产生一个从0到n但不包括n的随机数，用n乘。要生成一个编号从1到n且包括n的随机数，乘以n再加1。
