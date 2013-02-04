---
layout: recipe
title: 去掉字符串首尾的空白
chapter: Strings
---
## 问题

你想去掉字符串首尾的空白

## 方法

使用JavaScript的正则表达式来替换空白。

使用下面的方式去掉首尾的空白字符：

{% highlight coffeescript %}
"  padded string  ".replace /^\s+|\s+$/g, ""
# => 'padded string'
{% endhighlight %}

如下方式，仅仅去掉首部的空白符：

{% highlight coffeescript %}
"  padded string  ".replace /^\s+/g, ""
# => 'padded string  '
{% endhighlight %}

去掉末尾的空白字符：

{% highlight coffeescript %}
"  padded string  ".replace /\s+$/g, ""
# => '  padded string'
{% endhighlight %}

## 详解

Opera，Firefox和Chrome都有一个原生的字符串原型方法`trim`，而且也可以为其他浏览器其添加同样的方法。对于这类方法，如果存在的话我就使用内置的，否则就自己补充一个：

{% highlight coffeescript %}
unless String::trim then String::trim = -> @replace /^\s+|\s+$/g, ""

"  padded string  ".trim()
# => 'padded string'
{% endhighlight %}


### 语法糖

你可以使用下面这种快捷方法添加一些类似于Ruby的语法糖：

{% highlight coffeescript %}
String::strip = -> if String::trim? then @trim() else @replace /^\s+|\s+$/g, ""
String::lstrip = -> @replace /^\s+/g, ""
String::rstrip = -> @replace /\s+$/g, ""

"  padded string  ".strip()
# => 'padded string'
"  padded string  ".lstrip()
# => 'padded string  '
"  padded string  ".rstrip()
# => '  padded string'
{% endhighlight %}

关于`trim`性能的讨论和数据，参看Steve Levithan的[这篇博客](http://blog.stevenlevithan.com/archives/faster-trim-javascript)。
