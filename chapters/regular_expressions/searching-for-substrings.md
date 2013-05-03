---
layout: recipe
title: 搜索子字符串 Searching for Substrings
chapter: Regular Expressions
---
## 问题 Problem

你想搜索一个字符串，获取该子字符串匹配的起始位置或匹配值。

You need to search for a substring, and return either the starting position of the match or the matching value itself.

## 方法 Solution

使用正则表达式有好几种方法来实现，有些是调用`RegExp`字面量或者对象的方法，有的是调用`String`对象上的。

There are several ways to accomplish this using regular expressions. Some methods are called on a `RegExp` pattern or object and some are called on `String` objects.

### `RegExp`对象 `RegExp` objects

第一种方法是调用`RegExp`字面量或者对象上的`test`方法，这个`test`方法会返回一个布尔值：
The first way is to call the `test` method on a `RegExp` pattern or object. The `test` method returns a boolean value:

{% highlight coffeescript %}
match = /sample/.test("Sample text")
# => false

match = /sample/i.test("Sample text")
# => true
{% endhighlight %}

下一种方法是调用`RegExp`字面量或者对象的`exec`方法，这个方法会返回一个数组（包含了匹配信息）或者`null`；

The next way to is to call the `exec` method on a `RegExp` pattern or object. The `exec` method returns an array an array with the match information or `null`:

{% highlight coffeescript %}
match = /s(amp)le/i.exec "Sample text"
# => [ 'Sample', 'amp', index: 0, input: 'Sample text' ]

match = /s(amp)le/.exec "Sample text"
# => null
{% endhighlight %}

### `String`对象 `String` objects

`match`方法对一个给定的字符串与`RegExp`匹配。如果带有`g`标志，则返回一个包含了所有匹配的数组，否则只返回第一个匹配值，或者返回`null`，如果没有匹配的话。

The `match` method matches a given string with the `RegExp`. With 'g' flag returns an array containing the matches, without 'g' flag returns just the first match or if no match is found returns `null`.

{% highlight coffeescript %}
"Watch out for the rock!".match(/r?or?/g)
# => [ 'o', 'or', 'ro' ]

"Watch out for the rock!".match(/r?or?/)
# => [ 'o', index: 6, input: 'Watch out for the rock!' ]

"Watch out for the rock!".match(/ror/)
# => null
{% endhighlight %}

`search`用来做`RegExp`和字符串的匹配，如果匹配到就返回匹配的起始位置，否则返回-1。

The `search` method matches `RegExp` with string and returns the index of the beginning of the match if found, -1 if not.

{% highlight coffeescript %}
"Watch out for the rock!".search /for/
# => 10

"Watch out for the rock!".search /rof/
# => -1
{% endhighlight %}

## 讨论 Discussion

正则表达式是一种测试或者匹配字串的强大的方式。

Regular Expressions are a powerful way to test and match substrings.
