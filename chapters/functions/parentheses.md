---
layout: recipe
title: 当函数调用的括号不可以省略时 When Function Parentheses Are Not Optional
chapter: Functions
---
## 问题 Problem

你想调用一个无参数的函数，但是你不想使用括号。

You want to call a function that takes no arguments, but don't want to use parentheses.

## 方法 Solution

不管怎样，都是用括号。

Use parentheses anyway.

另外一种方案：利用do标记法，例如：

Another alternative is to utilize the do-notation like so:

{% highlight coffeescript %}
notify = -> alert "Hello, user!"
do notify if condition
{% endhighlight %}

编译成JavaScript如下：

This compiles to the following JavaScript:

{% highlight javascript %}
var notify;
notify = function() {
	return alert("Hello, user!");
};
if (condition) {
	notify();
}
{% endhighlight %}

## 讨论 Discussion

和Ruby一样，CoffeeScript允许你在调用方法时不使用括号。然后，CoffeeScript会把纯粹的函数名当作指向这个函数的指针。这样实际会导致这样的结果——你没有给方法传递函数，CoffeeScript就无法分辨出你想调用这个函数还是把它当作一个引用使用。

Like Ruby, CoffeeScript allows you to drop parentheses to method calls. Unlike Ruby, however, CoffeeScript treats a bare function name as the pointer to the function. The practical upshot of this is that if you give no arguments to a method, CoffeeScript cannot tell if you want to call the function or use it as a reference.

谁优谁劣？我这并没有好坏之分，就只是不一样。这产生了一个与预期不一样的语法特例——括号不_总是_可选的——但是作为回报，它给予了使用名字平凡传递或者接受一个函数的能力，这样看Ruby就有点不给力了。

Is this good or bad? It's just different. It creates an unexpected syntax case -- parentheses aren't _always_ optional -- but in exchange it gives you the ability to pass and receive functions fluently by name, something that's a bit klunky in Ruby.

这种do记法对于那些使用CoffeeScript但有括号恐惧症的人来说是一种优雅的方式。尽管如此，有些人还是喜欢在调用函数时把括号写上。

This usage of the do-notation is a neat approach for CoffeeScript with parenphobia. Some people simply prefer to write out the parentheses in the function call, though.
