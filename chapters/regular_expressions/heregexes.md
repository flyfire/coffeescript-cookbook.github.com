---
layout: recipe
title: 使用定点 Using Heregexes
chapter: Regular Expressions
---
## 问题 Problem

你需要编写一个复杂的正则表达式。

You need to write a complex regular expression.

## 方法 Solution

使用CoffeeScript的“顶点”——对正则表达式进行了扩展，可以忽略正则中的空格，还可以包含注释。

Use Coffeescript's "heregexes" -- extended regular expressions that ignore internal whitespace and can contain comments.

{% highlight coffeescript %}
pattern = ///
  ^\(?(\d{3})\)? # Capture area code, ignore optional parens
  [-\s]?(\d{3})  # Capture prefix, ignore optional dash or space
  -?(\d{4})      # Capture line-number, ignore optional dash
///
[area_code, prefix, line] = "(555)123-4567".match(pattern)[1..3]
# => ['555', '123', '4567']
{% endhighlight %}

## 讨论 Discussion

把你的正则表达式分割开，并给关键的部分添加注释，可以让正则表达式更容易理解和维护。例如，改变这个正则，使得前缀和line-number之间可以有空格，就很明显了。

Breaking up your complex regular expressions and commenting key sections makes them a lot more decipherable and maintainable. For example, changing this regex to allow an optional space between the prefix and line number would now be fairly obvious.

### 在顶点总的空字符 Whitespace characters in heregexes

既然空格符在heregexes中被省略掉了，那如果你需要匹配一个字面的ASCII空格该怎么办呢？

Whitespace is ignored in heregexes -- so what do you do if you need to match a literal ASCII space?

一种方法就是使用@\s@这个字符类，它可以匹配空格、tab和换行。如果你只想匹配一个空格，那就需要使用`\x20`来表示一个字面的ASCII空格符。

One solution is to use the @\s@ character class, which will match spaces, tabs
and line breaks. If you only want to match a space, though, you'll need to use
`\x20` to denote a literal ASCII space.
