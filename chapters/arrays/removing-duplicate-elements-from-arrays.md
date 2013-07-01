---
layout: recipe
title: 数组去重 Removing Duplicate Elements from Arrays
chapter: Arrays
---
## 问题 Problem

你想给一个数组去重。

You want to remove duplicate elements from an array.

## 方法 Solution

{% highlight coffeescript %}
Array::unique = ->
  output = {}
  output[@[key]] = @[key] for key in [0...@length]
  value for key, value of output

[1,1,2,2,2,3,4,5,6,6,6,"a","a","b","d","b","c"].unique()
# => [ 1, 2, 3, 4, 5, 6, 'a', 'b', 'd', 'c' ]
{% endhighlight %}

## 讨论 Discussion

JavaScript中有很多中`unique`方法的实现。这里的这个实现基于"The fastest method to find unique items in array"，在[这里](http://www.shamasis.net/2009/09/fast-algorithm-to-find-unique-items-in-javascript-array/)可以找到。
There are many implementations of the `unique` method in JavaScript. This one is based on "The fastest method to find unique items in array" found [here](http://www.shamasis.net/2009/09/fast-algorithm-to-find-unique-items-in-javascript-array/).

**注意：** 尽管这在常见的语言，比如说Ruby中很常见，但是在JavaScript中扩展原生对象往往被成是不好的实践。(参看： [Maintainable JavaScript: Don’t modify objects you don’t own](http://www.nczonline.net/blog/2010/03/02/maintainable-javascript-dont-modify-objects-you-down-own/); [Extending built-in native objects. Evil or not?](http://perfectionkills.com/extending-built-in-native-objects-evil-or-not/))。

**Note:** Although it's quite common in languages like Ruby, extending native objects is often considered bad practice in JavaScript (see: [Maintainable JavaScript: Don’t modify objects you don’t own](http://www.nczonline.net/blog/2010/03/02/maintainable-javascript-dont-modify-objects-you-down-own/); [Extending built-in native objects. Evil or not?](http://perfectionkills.com/extending-built-in-native-objects-evil-or-not/)).


