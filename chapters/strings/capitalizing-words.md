---
layout: recipe
title: 首字母大写
chapter: Strings
---
## 问题

让一段字符串中每一个单词首字母大写。

## 方法

使用`split`、`map`、`join`模式：把字符串分割成一个个单词，然后把每个单词映射为首字母大且其他字母小写的新单词，最后使用`join`方法恢复成一个字符串。

{% highlight coffeescript %}
("foo bar baz".split(' ').map (word) -> word[0].toUpperCase() + word[1..-1].toLowerCase()).join ' '
# => 'Foo Bar Baz'
{% endhighlight %}

或者，通过列表解析来实现：

{% highlight coffeescript %}
(word[0].toUpperCase() + word[1..-1].toLowerCase() for word in "foo   bar   baz".split /\s+/).join ' '
# => 'Foo Bar Baz'
{% endhighlight %}

## 详解

Split，map和join是一种常用的脚本模式，可追溯到perl语言。使用[扩展类](/chapters/objects/extending-classes)把这些函数直接放到String类中会更好。

Split，map和join模式也有两点不足之处需要注意。一是，如果被分割的字符串比较固定的话split没什么问题，不过不过源字符串中包含多个空格，使用split方法就需要考虑进去，以免混入多余的空单词。使用正则表达式代替单个空格来对连续空格进行切分是一种方法：

{% highlight coffeescript %}
("foo    bar    baz".split(/\s+/).map (word) -> word[0].toUpperCase() + word[1..-1].toLowerCase()).join ' '
# => 'Foo Bar Baz'
{% endhighlight %}

……但是这会把我们导向另一个瑕疵：注意，join后，连续的空格现在被精简成了单个空格。

然而，通常这两个瑕疵或多或少是可以接受的，因此，slipt，map和join模式是非常有用的工具。
