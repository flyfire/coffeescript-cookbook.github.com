---
layout: recipe
title: 备忘录模式 Memento Pattern
chapter: Design Patterns
---
## 问题 Problem

你想记录一个对象的变化。

You want to anticipate the reversion of changes to an object.

## 方法 Solution

使用[备忘录模式](http://en.wikipedia.org/wiki/Memento_pattern)来跟踪一个对象的变化。使用这个模式的类会返回一个`memento`对象，可以保存到其他地方。

Use the [Memento pattern](http://en.wikipedia.org/wiki/Memento_pattern) to track changes to an object.  The class using the pattern will export a `memento` object stored elsewhere.

例如，你有个程序，用户可以编辑文本文件，他们应该有需要取消他们最后一次编辑。你可以在用户改变文件之前保存当前的状态，之后可以进行恢复。

If you have application where the user can edit a text file, for example, they may want to undo their last action.  You can save the current state of the file before the user changes it and then roll back to that at a later point.

{% highlight coffeescript %}
class PreserveableText
	class Memento
		constructor: (@text) ->

	constructor: (@text) ->
	save: (newText) ->
		memento = new Memento @text
		@text = newText
		memento
	restore: (memento) ->
		@text = memento.text

pt = new PreserveableText "The original string"
pt.text # => "The original string"

memento = pt.save "A new string"
pt.text # => "A new string"

pt.save "Yet another string"
pt.text # => "Yet another string"

pt.restore memento
pt.text # => "The original string"
{% endhighlight %}

## 讨论 Discussion

由`PreserveableText#save`返回的备忘录对象单独保管着重要的状态信息。 你甚至可以把这个备忘录对象序列化，便于在硬盘上维护一个“撤销”缓冲区，或者remotely for such data-intensive objects as edited images。

The Memento object returned by `PreserveableText#save` stores the important state information separately for safe-keeping.  You could even serialize this Memento in order to maintain an "undo" buffer on the hard disk or remotely for such data-intensive objects as edited images.
