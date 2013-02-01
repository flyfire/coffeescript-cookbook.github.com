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

## 详细

Split, map, join is a common scripting pattern dating back to perl. This function may benefit from being placed directly onto the String class by [Extending Classes](/chapters/objects/extending-classes).

Be aware that two wrinkles can appear in the split, map, join pattern. The first is that the split text works best when it is constant. If the source string has multiple spaces in it, the split will need to take this into account to prevent getting extra, empty words. One way to do this is with a regular expression to split on runs of whitespace instead of a single space:

{% highlight coffeescript %}
("foo    bar    baz".split(/\s+/).map (word) -> word[0].toUpperCase() + word[1..-1].toLowerCase()).join ' '
# => 'Foo Bar Baz'
{% endhighlight %}

...but this leads us to the second wrinkle: notice that the runs of whitespace are now compressed down to a single character by the join.

Quite often one or both of these wrinkles is acceptable, however, so the split, map, join pattern can be a powerful tool.
