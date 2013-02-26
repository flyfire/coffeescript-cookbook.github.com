---
layout: recipe
title: 数学常数
chapter: Math
---
## 问题

你想使用常见的数学常数，例如π,e。

## 方法

使用JavaScript的Math对象提供的常用数学常数。

{% highlight coffeescript %}
Math.PI
# => 3.141592653589793

# 提示：大小写注意！这个过程没有输出结果，因为Pi未定义。
Math.Pi
# =>

Math.E
# => 2.718281828459045

Math.SQRT2
# => 1.4142135623730951

Math.SQRT1_2
# => 0.7071067811865476

# 2的自然对数. ln(2)
Math.LN2
# => 0.6931471805599453

Math.LN10
# => 2.302585092994046

Math.LOG2E
# => 1.4426950408889634

Math.LOG10E
# => 0.4342944819032518

{% endhighlight %}

## 讨论

还有一个例子是一个数学常数如何在现实世界中使用，请参看Math章节的 [度与弧度转换](/coffeescript-cookbook.github.com/chapters/math/radians-degrees)部分。
