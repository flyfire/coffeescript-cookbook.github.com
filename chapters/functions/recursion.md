---
layout: recipe
title: 递归函数 Recursive Functions
chapter: Functions
---
## 问题 Problem

你想在函数体中调用函数本身。

You want to call a function from within that same function.

## 方法 Solution

使用一个具名函数：

With a named function:

{% highlight coffeescript %}
ping = ->
	console.log "Pinged"
	setTimeout ping, 1000
{% endhighlight %}

在匿名函数中，使用@arguments.callee@：

With an unnamed function, using @arguments.callee@:

{% highlight coffeescript %}
delay = 1000

setTimeout((->
	console.log "Pinged"
	setTimeout arguments.callee, delay
	), delay)
{% endhighlight %}

## 讨论 Discussion

尽管`arguments.callee`允许在一个匿名函数中使用递归，并且在内存密集型的程序中可能会有优势，但是具名函数依然有其用途，可以让代码更加清晰，容易维护。

While `arguments.callee` allows for the recursion of anonymous functions and might have the advantage in a very memory-intensive application, named functions keep their purpose more explicit and make for more maintainable code.
