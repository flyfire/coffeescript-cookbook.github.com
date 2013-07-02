---
layout: recipe
title: 化简数组 Reducing Arrays
chapter: Arrays
---
## 问题 Problem

你有一个对象数组，你想将其简化为一个值，与Ruby的`reduce()`和`reduceRight()`类似。

You have an array of objects and want to reduce them to a value, similar to Ruby's `reduce()` and `reduceRight()`.

## 问题 Solution

很简单，你可以直接使用Array的`reduce()`和`reduceRight()`方法，传递一个匿名函数，保持代码的整洁性和可读性。化简本身可能相当简单，例如是用`+`操作符来连接数字或者字符串。

You can simply use Array's `reduce()` and `reduceRight()` methods along with an anonoymous function, keeping the code clean and readable. The reduction may be something simple such as using the `+` operator with numbers or strings.

{% highlight coffeescript %}
[1,2,3,4].reduce (x,y) -> x + y
# => 10
{% endhighlight %}

{% highlight coffeescript %}
["words", "of", "bunch", "A"].reduceRight (x, y) -> x + " " + y
# => 'A bunch of words'
{% endhighlight %}

或者，更复杂点，将一个列表中的对象聚集到一个联合的对象中。

Or it may be something more complex such as aggregating elements from a list into a combined object.

{% highlight coffeescript %}
people =
    { name: 'alec', age: 10 }
    { name: 'bert', age: 16 }
    { name: 'chad', age: 17 }

people.reduce (x, y) ->
    x[y.name]= y.age
    x
, {}
# => { alec: 10, bert: 16, chad: 17 }
{% endhighlight %}

## 讨论 Discussion

JavaScript在1.8版中加入了`reduce`和`reduceRight`方法。CoffeeScript提供了一种简单但很自然的方法表达匿名函数。在这个把一系列的项合并到一个结果的问题中非常漂亮地结合到了一起。

Javascript introduced `reduce` and `reduceRight` in version 1.8. Coffeescript provides a natural and simple way to express anonymous functions. Both go together cleanly in the problem of merging a collection's items into a combined result.
