---
layout: recipe
title: 弧度与度转换
chapter: Math
---
## 问题

你需要在弧度与度之间转换。

## 方法

使用JavaScript的Math.PI和一个简单的公式在弧度与度之间进行转换。

{% highlight coffeescript %}
# 从弧度到度
radiansToDegrees = (radians) ->
    degrees = radians * 180 / Math.PI

radiansToDegrees(1)
# => 57.29577951308232

# 从度到弧度
degreesToRadians = (degrees) ->
    radians = degrees * Math.PI / 180

degreesToRadians(1)
# => 0.017453292519943295
{% endhighlight %}

## 讨论

有问题吗?
