---
layout: recipe
title: 列表解析 List Comprehensions
chapter: Arrays
---
## 问题 Problem

你手里有个对象数组，你想将它映射为另一个数组，就像Python的列表解析。

You have an array of objects and want to map them to another array, similar to Python's list comprehensions.

## 方法 Solution

使用列表解析，但是别忘了[数组映射](/chapters/arrays/mapping-arrays)。

Use a list comprehension, but don't forget about [mapping arrays](/chapters/arrays/mapping-arrays).

{% highlight coffeescript %}
electric_mayhem = [ { name: "Doctor Teeth", instrument: "piano" },
                    { name: "Janice", instrument: "lead guitar" },
                    { name: "Sgt. Floyd Pepper", instrument: "bass" },
                    { name: "Zoot", instrument: "sax" },
                    { name: "Lips", instrument: "trumpet" },
                    { name: "Animal", instrument: "drums" } ]

names = (muppet.name for muppet in electric_mayhem)
# => [ 'Doctor Teeth', 'Janice', 'Sgt. Floyd Pepper', 'Zoot', 'Lips', 'Animal' ]
{% endhighlight %}

## 讨论 Discussion

CoffeeScript直接支持列表解析，因此它们的工作方式与你在Python中所熟知的用法一直。对于简单的映射，列表解析更加具有可读性。针对更加复杂的转换或者链式映射，[数组映射](/chapters/arrays/mapping-arrays)将更加合适。

Because CoffeeScript directly support list comprehensions, they work pretty much as advertised wherever you would use one in Python. For simple mappings, list comprehensions are much more readable. For complicated transformations or for chained mappings, [mapping arrays](/chapters/arrays/mapping-arrays) might be more elegant.
