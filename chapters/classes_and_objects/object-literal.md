---
layout: recipe
title: 不存在就赋值为对象字面量
chapter: Classes and Objects
---
## 问题

初始化一个对象字面量，但不能覆盖了已有的对象。

## 方法

使用存在操作符

{% highlight coffeescript %}
window.MY_NAMESPACE ?= {}
{% endhighlight %}

## 详解

它下面的这段代码等价：

{% highlight javascript %}
window.MY_NAMESPACE = window.MY_NAMESPACE || {};
{% endhighlight %}

这种使用对象字面量来定义一个命名空间是一种常用的JavaScript技巧，这能够避免破坏了命名空间，如果已经存在的话。
