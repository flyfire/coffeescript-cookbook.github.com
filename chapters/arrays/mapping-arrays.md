---
layout: recipe
title: 数组映射 Mapping Arrays
chapter: Arrays
---
## 问题 Problem

你手里有个对象数组，你想将它映射为另一个数组，就像Ruby的数组。

You have an array of objects and want to map them to another array, similar to Ruby's map.

## 方法 Solution

使用`map()`方法，传递一个匿名函数，但是别忘了[list comprehensions](/chapters/arrays/list-comprehensions)。

Use map() with an anonymous function, but don't forget about <a href="/chapters/arrays/list-comprehensions">list comprehensions</a>.

{% highlight coffeescript %}
electric_mayhem = [ { name: "Doctor Teeth", instrument: "piano" },
                    { name: "Janice", instrument: "lead guitar" },
                    { name: "Sgt. Floyd Pepper", instrument: "bass" },
                    { name: "Zoot", instrument: "sax" },
                    { name: "Lips", instrument: "trumpet" },
                    { name: "Animal", instrument: "drums" } ]

names = electric_mayhem.map (muppet) -> muppet.name
# => [ 'Doctor Teeth', 'Janice', 'Sgt. Floyd Pepper', 'Zoot', 'Lips', 'Animal' ]
{% endhighlight %}

## 讨论 Discussion

因为CoffeeScript对匿名函数的支持很干净，所以在CoffeeScript中映射一个数组就像在Ruby中一样容易。

Because CoffeeScript has clean support for anonymous functions, mapping an array in CoffeeScript is nearly as easy as it is in Ruby.

在CoffeeScript中做复杂的转换或者链式映射Map是一种很方便方式。但是如果你的转换和上面的例子一样简单，那作为一个[list comprehensions](/chapters/arrays/list-comprehensions)将会更加干净。

Maps are are good way to handle complicated transforms and chained mappings in CoffeeScript. If your transformation is as simple as the one above, however, it may read more cleanly as a <a href="/chapters/arrays/list-comprehensions">list comprehension</a>.
