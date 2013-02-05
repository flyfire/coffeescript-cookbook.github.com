---
layout: recipe
title: 检查值的类型是否是数组
chapter: Arrays
---
## 问题

你想检查某个值的类型是否为`Array`。

{% highlight coffeescript %}
myArray = []
console.log typeof myArray // outputs 'object'
{% endhighlight %}

`typeof`用在数组上时，输出的结果是错误的。

## 方法

使用如下的代码：

{% highlight coffeescript %}
typeIsArray = Array.isArray || ( value ) -> return {}.toString.call( value ) is '[object Array]'
{% endhighlight %}

如下调用`typeIsArray`就行：

{% highlight coffeescript %}
myArray = []
typeIsArray myArray // outputs true
{% endhighlight %}

## 详解

上面的方法摘自“the Miller Device”。还可以使用Douglas Crockford的这段代码：

{% highlight coffeescript %}
typeIsArray = ( value ) ->
    value and
        typeof value is 'object' and
        value instanceof Array and
        typeof value.length is 'number' and
        typeof value.splice is 'function' and
        not ( value.propertyIsEnumerable 'length' )
{% endhighlight %}
