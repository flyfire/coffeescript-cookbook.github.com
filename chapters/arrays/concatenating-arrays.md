---
layout: recipe
title: 连接数组
chapter: Arrays
---
## 问题

你想要将两个数据连接到一起。

## 方法

在JavaScript中，有两个连接数组的标准选项。

第一个是使用JavaScript中Array的`concat()`方法：

{% highlight coffeescript %}
array1 = [1, 2, 3]
array2 = [4, 5, 6]
array3 = array1.concat array2
# => [1, 2, 3, 4, 5, 6]
{% endhighlight %}

需要注意的是`array1`在操作中没有被修改。串接的数组返回一个新的对象。

如果你希望合并两个数组而不创建新对象，你可以使用以下技巧：

{% highlight coffeescript %}
array1 = [1, 2, 3]
array2 = [4, 5, 6]
Array::push.apply array1, array2
array1
# => [1, 2, 3, 4, 5, 6]
{% endhighlight %}

在上面的例子中，`Array.prototype.push.apply(a, b)`方法修改`array1`对象，而无需创建一个新数组对象。

我们可以简化上面的模式，使用CoffeeScript通过创建一个新的`merge()`对数组的方法。

{% highlight coffeescript %}
Array::merge = (other) -> Array::push.apply @, other

array1 = [1, 2, 3]
array2 = [4, 5, 6]
array1.merge array2
array1
# => [1, 2, 3, 4, 5, 6]
{% endhighlight %}

或者，我们可以通过CoffeeScript图示(`array2...`)直接使用`push()`，避免Array原型。

{% highlight coffeescript %}
array1 = [1, 2, 3]
array2 = [4, 5, 6]
array1.push array2...
array1
# => [1, 2, 3, 4, 5, 6]97
{% endhighlight %}

## 讨论

CoffeeScript缺乏一种特定的语法来加入数组，但`concat()`和`push()`是标准的JavaScript方法。
