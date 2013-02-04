---
layout: recipe
title: 大写整个字符
chapter: Strings
---
## 问题

你想大写整个字符串。

## 方法

使用JavaScript String的toUpperCase()方法：

{% highlight coffeescript %}
"one two three".toUpperCase()
# => 'ONE TWO THREE'
{% endhighlight %}

##　详解

`toUpperCase()`是一个标准的JavaScript方法。 别忘了括号。

### 语法糖

你可以使用下面这种便捷方法添加一些与Ruby相似的语法糖。

{% highlight coffeescript %}
String::upcase = -> @toUpperCase()
"one two three".upcase()
# => 'ONE TWO THREE'
{% endhighlight %}

上面的代码片段展示了CoffeeScript的数个特性：

* 双冒号::是.prototype的缩写；
* @是this.的缩写。


上面的代码会被编译为如下的JavaScript代码：

{% highlight javascript %}
String.prototype.upcase = function() {
  return this.toUpperCase();
};
"one two three".upcase();
{% endhighlight %}

**注意：**尽管在像Ruby这样的语言中扩展原生的对象很常见，但是在JavaScript中这被认为是不好的实践（参看：[Maintainable JavaScript: Don’t modify objects you don’t own](http://www.nczonline.net/blog/2010/03/02/maintainable-javascript-dont-modify-objects-you-down-own/)；[Extending built-in native objects. Evil or not?](http://perfectionkills.com/extending-built-in-native-objects-evil-or-not/)）。
