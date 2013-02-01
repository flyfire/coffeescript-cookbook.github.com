---
layout: recipe
title: 克隆对象（深度克隆）
chapter: Classes and Objects
---
## 问题

克隆对象，包括其下的所有子对象。

## 方法

{% highlight coffeescript %}
clone = (obj) ->
  if not obj? or typeof obj isnt 'object'
    return obj

  if obj instanceof Date
    return new Date(obj.getTime()) 

  if obj instanceof RegExp
    flags = ''
    flags += 'g' if obj.global?
    flags += 'i' if obj.ignoreCase?
    flags += 'm' if obj.multiline?
    flags += 'y' if obj.sticky?
    return new RegExp(obj.source, flags) 

  newInstance = new obj.constructor()

  for key of obj
    newInstance[key] = clone obj[key]

  return newInstance

x =
  foo: 'bar'
  bar: 'foo'

y = clone(x)

y.foo = 'test'

console.log x.foo isnt y.foo, x.foo, y.foo
# => true, bar, test
{% endhighlight %}

## 详解

赋值与克隆函数这两种对象克隆方式不同点在于对引用的处理。赋值仅仅复制对象的引用，而克隆函数会创建一个全新的对象：

* 创建一个与源对象同样的新对象；  
* 把源对象的所有属性拷贝到新对象中；  
* 递归地调用克隆函数，对所有的子对象重复以上步骤。  

赋值拷贝到例子：

{% highlight coffeescript %}
x =
  foo: 'bar'
  bar: 'foo'

y = x

y.foo = 'test'

console.log x.foo isnt y.foo, x.foo, y.foo
# => false, test, test
{% endhighlight %}

如你所见，拷贝后，你改变`y`，同时也会改变`x`。
