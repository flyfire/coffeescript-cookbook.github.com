---
layout: recipe
title: 搅乱数组中的元素 Shuffling Array Elements
chapter: Arrays
---
## 问题 Problem

你想搅乱数组中的元素。

You want to shuffle the elements in an array.

## 方法 Solution

JavaScript中的Array有一个`sort()`方法，接受一个自定义的排序函数。我们只需写一个`shuffle()`函数就行。

The JavaScript Array `sort()` method accepts a custom sort function. We can write a `shuffle()` method to add some convenience.

{% highlight coffeescript %}
Array::shuffle = -> @sort -> 0.5 - Math.random()

[1..9].shuffle()
# => [ 3, 1, 5, 6, 4, 8, 2, 9, 7 ]
{% endhighlight %}

## 详解 Discussion

更多关于这个混淆函数的工作原理，参看[StackOverflow上的讨论](http://stackoverflow.com/questions/962802/is-it-correct-to-use-javascript-array-sort-method-for-shuffling)。

For more background on how this shuffle logic works, see this [discussion at StackOverflow](http://stackoverflow.com/questions/962802/is-it-correct-to-use-javascript-array-sort-method-for-shuffling).

**注意：** 尽管这在常见的语言，比如说Ruby中很常见，但是在JavaScript中扩展原生对象往往被成是不好的实践。(参看： [Maintainable JavaScript: Don’t modify objects you don’t own](http://www.nczonline.net/blog/2010/03/02/maintainable-javascript-dont-modify-objects-you-down-own/); [Extending built-in native objects. Evil or not?](http://perfectionkills.com/extending-built-in-native-objects-evil-or-not/))。

**Note:** Although it's quite common in languages like Ruby, extending native objects is often considered bad practice in JavaScript (see: [Maintainable JavaScript: Don’t modify objects you don’t own](http://www.nczonline.net/blog/2010/03/02/maintainable-javascript-dont-modify-objects-you-down-own/); [Extending built-in native objects. Evil or not?](http://perfectionkills.com/extending-built-in-native-objects-evil-or-not/)).
