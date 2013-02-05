---
layout: recipe
title: 检查值的类型是否是一个数组
chapter: Arrays
---
## 问题

你想要检查一个对象是否为`Array`。

{% highlight coffeescript %}
myArray = []
console.log typeof myArray // 输出 'object'
{% endhighlight %}

“typeof”方法对数组对象提供了一个不友好的输出。

## 方法

使用如下代码：

{% highlight coffeescript %}
typeIsArray = Array.isArray || ( value ) -> return {}.toString.call( value ) is '[object Array]'
{% endhighlight %}

使用它只需要像这样调用`typeIsArray`：

{% highlight coffeescript %}
myArray = []
typeIsArray myArray // 输出 true
{% endhighlight %}

## 讨论

上面的方法已经被“the Miller Device”采用。另一种方法是使用Douglas Crockford的代码片段：

{% highlight coffeescript %}
typeIsArray = ( value ) ->
    value and
        typeof value is 'object' and
        value instanceof Array and
        typeof value.length is 'number' and
        typeof value.splice is 'function' and
        not ( value.propertyIsEnumerable 'length' )
{% endhighlight %}
