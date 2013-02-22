---
layout: recipe
title: 命令模式
chapter: Design Patterns
---
## 问题

当你自己的代码运行时，你需要使用其他另外的对象来处理。

## 方法

使用[命令模式](http://en.wikipedia.org/wiki/Command_pattern)把引用传递给你的函数。

{% highlight coffeescript %}
# Using a private variable to simulate external scripts or modules
incrementers = (() ->
	privateVar = 0

	singleIncrementer = () ->
		privateVar += 1

	doubleIncrementer = () ->
		privateVar += 2
	
	commands = 
		single: singleIncrementer
		double: doubleIncrementer
		value: -> privateVar
)()

class RunsAll
	constructor: (@commands...) ->
	run: -> command() for command in @commands

runner = new RunsAll(incrementers.single, incrementers.double, incrementers.single, incrementers.double)
runner.run()
incrementers.value() # => 6
{% endhighlight %}

## 详解

承袭自JavaScript，由于函数是第一类对象，且函数与变量作用域绑定的关系，CoffeeScript让这个模式几乎不可见。事实上，所有把函数作为一个回调传递时都可以看作是一个*命令*。

jQuery AJAX方法返回的 `jqXHR`对象使用的就是这种模式。

{% highlight coffeescript %}
jqxhr = $.ajax
	url: "/"

logMessages = ""

jqxhr.success -> logMessages += "Success!\n"
jqxhr.error -> logMessages += "Error!\n"
jqxhr.complete -> logMessages += "Completed!\n"

# On a valid AJAX request:
# logMessages == "Success!\nCompleted!\n"
{% endhighlight %}
