---
layout: recipe
title: 查找子字符串
chapter: Strings
---
## 问题

在一个消息中搜索字，获取第一次出现或者最后一次出现的子字符串。

## 方法

可以分别使用JavaScript的indexOf()和lastIndexOf()方法来查找第一个或者最后一个出现的字符串。

语法：string.indexOf searchstring, start

{% highlight coffeescript %}
message = "This is a test string. This has a repeat or two. This might even have a third."
message.indexOf "This", 0
# => 0

# Modifying the start parameter
message.indexOf "This", 5
# => 23

message.lastIndexOf "This"
# => 49

{% endhighlight %}

## 详解

还需要梁歪的技巧来计算字符串在一个消息中出现的次数。
