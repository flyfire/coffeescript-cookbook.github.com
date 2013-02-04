---
layout: recipe
title: 分割字符串
chapter: Strings
---
## 问题

你想分割一个字符串。

## 方法

使用JavaScript的split()方法：

{% highlight coffeescript %}
"foo bar baz".split " "
# => [ 'foo', 'bar', 'baz' ]
{% endhighlight %}

## Discussion
## 详解

String的split()是一个标准的JavaScript方法。它可以使用任意的分割符来切割字符串，甚至可以使用正则表达式。它还能接受第二个参数，指明需要返回的切片个数。

{% highlight coffeescript %}
"foo-bar-baz".split "-"
# => [ 'foo', 'bar', 'baz' ]
{% endhighlight %}

{% highlight coffeescript %}
"foo   bar  \t baz".split /\s+/
# => [ 'foo', 'bar', 'baz' ]
{% endhighlight %}

{% highlight coffeescript %}
"the sun goes down and I sit on the old broken-down river pier".split " ", 2
# => [ 'the', 'sun' ]
{% endhighlight %}
