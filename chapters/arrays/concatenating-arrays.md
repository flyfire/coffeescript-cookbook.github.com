---
layout: recipe
title: 链接数组
chapter: Arrays
---
## 问题

你想把两个数组合并到一起。

## 方法

在JavaScript中有两种标准的方法来合并数组。

第一种是使用JavaScript Array的`concat()`方法：

{% highlight coffeescript %}
array1 = [1, 2, 3]
array2 = [4, 5, 6]
array3 = array1.concat array2
# => [1, 2, 3, 4, 5, 6]
{% endhighlight %}

注意，操作并不会修改`array1`。合并后的数组会作为一个新的对象返回。

如果你想合并两个数组，而又不创建新的对象，你可以使用下面这样的技巧：

{% highlight coffeescript %}
array1 = [1, 2, 3]
array2 = [4, 5, 6]
Array::push.apply array1, array2
array1
# => [1, 2, 3, 4, 5, 6]
{% endhighlight %}

在上面的例子中，`Array.prototype.push.apply(a, b)`这样的方式，会直接更改`array1`，无需创建一个新的数组对象。

我们可以为数组创建一个新的`merge()`方法，使用CofeeScript简化上面的模式：

{% highlight coffeescript %}
Array::merge = (other) -> Array::push.apply @, other

array1 = [1, 2, 3]
array2 = [4, 5, 6]
array1.merge array2
array1
# => [1, 2, 3, 4, 5, 6]
{% endhighlight %}

作为Array原型的替代，我们可以直接给`push()`传递一个参数列（array2...）。

{% highlight coffeescript %}
array1 = [1, 2, 3]
array2 = [4, 5, 6]
array1.push array2...
array1
# => [1, 2, 3, 4, 5, 6]
{% endhighlight %}

## 详解

CoffeeScript虽然缺少链接数组的特殊语法，但`concat()`和`push()`是JavaScript标准的方法。
