---
layout: recipe
title: 检查每一个元素 Testing Every Element
chapter: Arrays
---
## 问题 Problem

你想针对某一个特别的条件对数组中的每一个元素进行检查。

You want to be able to check that every element in an array meets a particular condition.

## 方法 Solution

使用Array.every （ECMAScript 5）：

Use Array.every (ECMAScript 5):

{% highlight coffeescript %}
evens = (x for x in [1..10] by 2)

evens.every (x)-> x % 2 == 0
# => true
{% endhighlight %}

Mozilla的JavaScript 1.6中加入了Array.every，并在ECMAScript 5 中标准化。如果想支持没有实现EC5哦浏览器，就看看[underscore.js中的`_.all`][underscore]。

Array.every was addded to Mozilla's Javascript 1.6 and made standard with EcmaScript 5. If you to support browsers that do not implement EC5 then check out [`_.all` from underscore.js][underscore].

举个真实世界中的例子，假设一个多选的下拉列表，像下面这样：

For a real world example, prentend you have a multiple select list that looks like:

{% highlight html %}
<select multiple id="my-select-list">
  <option>1</option>
  <option>2</option>
  <option>Red Car</option>
  <option>Blue Car</option>
</select>
{% endhighlight %}

现在你想确定用户只选中的数组，让我们用Array.every看看：

Now you want to verify that the user selected only numbers. Let's use Array.every:

{% highlight coffeescript %}
validateNumeric = (item)->
  parseFloat(item) == parseInt(item) && !isNaN(item)

values = $("#my-select-list").val()

values.every validateNumeric
{% endhighlight %}

## 讨论 Discussion

这个方法与Ruby中数组的all?方法类似。

This is similar to using ruby's Array#all? method.

[underscore]: http://documentcloud.github.com/underscore/
