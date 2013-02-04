---
layout: recipe
title: 让整个字符串小写
chapter: Strings
---
##问题

你想让整个字符串变成小写的。

## 方法

使用JavaScript中String的toLowerCase()方法：

{% highlight coffeescript %}
"ONE TWO THREE".toLowerCase()
# => 'one two three'
{% endhighlight %}

## 详解

`toLowerCase()`是一个标准的JavaScript方法。但是别漏掉了括号。

### 语法糖

你可以使用下面方式添加一些像Ruby一样的语法糖：

{% highlight coffeescript %}
String::downcase = -> @toLowerCase()
"ONE TWO THREE".downcase()
# => 'one two three'
{% endhighlight %}

上面的这一小段代码，展示出了几个CoffeeScript的特性：

* 双冒号`::`是`.prototype`的缩写；
* `@`是`this.`的缩写。

编译成的JavaScript如下：

{% highlight javascript %}
String.prototype.downcase = function() {
  return this.toLowerCase();
};
"ONE TWO THREE".downcase();
{% endhighlight %}

**注意：**尽管在像Ruby这样的语言中扩展原生的对象很常见，但是在JavaScript中这被认为是不好的实践（参看：[Maintainable JavaScript: Don’t modify objects you don’t own](http://www.nczonline.net/blog/2010/03/02/maintainable-javascript-dont-modify-objects-you-down-own/)；[Extending built-in native objects. Evil or not?](http://perfectionkills.com/extending-built-in-native-objects-evil-or-not/)）。
