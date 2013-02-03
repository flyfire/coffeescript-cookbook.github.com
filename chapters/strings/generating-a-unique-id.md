---
layout: recipe
title: 生成唯一的ID
chapter: Strings
---
## 问题

你想生成唯一的ID。

## 方法

你可以从一个随机数中创建一个基于36进制编码的字符串。

{% highlight coffeescript %}
uniqueId = (length=8) ->
  id = ""
  id += Math.random().toString(36).substr(2) while id.length < length
  id.substr 0, length

uniqueId()    # => n5yjla3b
uniqueId(2)   # => 0d
uniqueId(20)  # => ox9eo7rt3ej0pb9kqlke
uniqueId(40)  # => xu2vo4xjn4g0t3xr74zmndshrqlivn291d584alj
{% endhighlight %}

## 详解

可能还有其他方法，不过在这是相对高效和灵活的方法。

