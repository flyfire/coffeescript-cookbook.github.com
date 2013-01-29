---
layout: recipe
title: 嵌入JavaScript代码
chapter: Syntax
---
## 问题

你需要在CoffeeScript中插入已有的或预编写的JavaScript代码。

## 方法

把JavaScript代码包裹在重音符中：

{% highlight coffeescript %}
`function greet(name) {
return "Hello "+name;
}`

# Back to CoffeeScript
greet "Coffee"
# => "Hello Coffee"
{% endhighlight %}

## 详解

这种把JavaScript代码块插入到CoffeeScript中的方式很简单，无需根据CoffeeScript的语法做转化。[CoffeeScript语言参考](http://jashkenas.github.com/coffee-script/#embedded)中有说，你可以在某种程度上混合这两种语言。

{% highlight coffeescript %}
hello = `function (name) {
return "Hello "+name
}`
hello "Coffee"
# => "Hello Coffee"

{% endhighlight %}

上例中，`hello`变量是CoffeeScript中的，但是给它赋值了一个函数，而这个函数是使用JavaScript编写的。
