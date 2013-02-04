---
layout: recipe
title: 重复字符串
chapter: Strings
---
## 问题

你想重复一个字符串。

## 方法

创建一个一个数组，包含n+1个null，然后把待重复的字符串作为胶水，将它们粘在一起：

{% highlight coffeescript %}
# create a string of 10 foos
Array(11).join 'foo'

# => "foofoofoofoofoofoofoofoofoofoo"
{% endhighlight %}

## 详解

JavaScript缺少一个字符串重复的函数，CoffeeScript也一样。凑活下，这里也可以使用列表解析和映射map。但是像这种简单的重复字符串的需求，创建一个n+1个null的数组，然后把它们粘在一起更简单。
