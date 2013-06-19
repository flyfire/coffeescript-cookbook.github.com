---
layout: recipe
title: 扩展内置对象 Extending Built-in Objects
chapter: Metaprogramming
---
## 问题 Problem

你想给一个类扩展新的功能或者替换掉老的。

You want to extend a class to add new functionality or replace old.

## 方案 Solution

使用`::`给对象或者类的原型赋值上你的新函数。

Use `::` to assign your new function to the prototype of the object or class.

{% highlight coffeescript %}
String::capitalize = () ->
  (this.split(/\s+/).map (word) -> word[0].toUpperCase() + word[1..-1].toLowerCase()).join ' '

"foo bar     baz".capitalize()
# => 'Foo Bar Baz'
{% endhighlight %}

## 讨论 Discussion

在JavaScript中（在CoffeeScript也一样），对象有一个原型成员，定义了那些以此为原型的对象所有可用的成员方法。在CoffeeScript中，你可以使用`::`操作符来直接访问原型。

Objects in JavaScript (and thus, in CoffeeScript) have a prototype member that defines what member functions should be available on all objects based on that prototype. In CoffeeScript, you can access the prototype directly via the `::` operator.

**注意：** 尽管在像Ruby这样的语言中，扩展原生的对象很常见，但是在JavaScript中这种行为被看作是不好的实践（参看： [Maintainable JavaScript: Don’t modify objects you don’t own](http://www.nczonline.net/blog/2010/03/02/maintainable-javascript-dont-modify-objects-you-down-own/)和[Extending built-in native objects. Evil or not?](http://perfectionkills.com/extending-built-in-native-objects-evil-or-not/)）。

**Note:** Although it's quite common in languages like Ruby, extending native objects is often considered bad practice in JavaScript (see: [Maintainable JavaScript: Don’t modify objects you don’t own](http://www.nczonline.net/blog/2010/03/02/maintainable-javascript-dont-modify-objects-you-down-own/); [Extending built-in native objects. Evil or not?](http://perfectionkills.com/extending-built-in-native-objects-evil-or-not/)).
