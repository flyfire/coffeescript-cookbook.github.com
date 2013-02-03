---
layout:   recipe
title:   字符串插值 
chapter:  Strings
---
## 问题

你想创建一个字符串，包含字符串，可以代表一个CoffeeScript变量。

## 方法

使用CoffeeScript类似Ruby的字符插值法，来替代JavaScript字符串拼接。

字符串插值：

{% highlight coffeescript %}
muppet = "Beeker"
favorite = "My favorite muppet is #{muppet}!"

# => "My favorite muppet is Beeker!"
{% endhighlight %}

{% highlight coffeescript %}
square = (x) -> x * x
message = "The square of 7 is #{square 7}."

# => "The square of 7 is 49."
{% endhighlight %}

## 讨论

CoffeeScript的字符串插值与Ruby的惯用法相似。绝大多数的表达式都可以放到插值语法`#{...}`中。

CoffeeScript允许在插值中使用多个表达式，这可能会有副作用，有时候是危险的信号。只有最后一个值会被返回。

{% highlight coffeescript %}
# You can do this, but don't. YOU WILL GO MAD.
square = (x) -> x * x
muppet = "Beeker"
message = "The square of 10 is #{muppet='Animal'; square 10}. Oh, and your favorite muppet is now #{muppet}."

# => "The square of 10 is 100. Oh, and your favorite muppet is now Animal."
{% endhighlight %}
