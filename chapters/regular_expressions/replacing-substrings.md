---
layout: recipe
title: 替换子字符串 Replacing Substrings
chapter: Regular Expressions
---
## 问题 Problem

你想把字符串的一部分替换为其他的值。

You need to replace a portion of a string with another value.

## 方法 Solution

使用JavaScript的`replace`方法。`replace`掉给定字符串中的匹配，返回修改后的字符串。

Use the JavaScript `replace` method. `replace` matches with the given string, and returns the edited string.

第一种形式，接受两个参数：_模式_和_替换为的字符串_

The first version takes 2 arguments: _pattern_ and _string replacement_

{% highlight coffeescript %}
"JavaScript is my favorite!".replace /Java/, "Coffee"
# => 'CoffeeScript is my favorite!'

"foo bar baz".replace /ba./, "foo"
# => 'foo foo baz'

"foo bar baz".replace /ba./g, "foo"
# => 'foo foo foo'
{% endhighlight %}

第二种形式，也是接受两个参数：_模式_和_回调函数_

The second version takes 2 arguments: _pattern_ and _callback function_

{% highlight coffeescript %}
"CoffeeScript is my favorite!".replace /(\w+)/g, (match) ->
  match.toUpperCase()
# => 'COFFEESCRIPT IS MY FAVORITE!'
{% endhighlight %}

每个匹配都会调用回调函数，并把匹配值作为参数传递给它。

The callback function is invoked for each match, and the match value is passed as the argument to the callback.

## 讨论 Discussion

正则表示式是一种强大的匹配和替换字符串的方式。

Regular Expressions are a powerful way to match and replace strings.
