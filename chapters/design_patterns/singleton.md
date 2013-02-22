---
layout: recipe
title: 单例模式
chapter: Design Patterns
---
## 问题

很多时候你需要一个仅且一个类的实例。例如，你也许只需要一个类实例，这个类创建服务端资源，并且你想保证只有一个对象可以控制这些资源。然而，要注意单例模式很容易引入不必要的全局变量。

## 方法

公共可用的类只包含一个方法，使用该方法获取那个唯一的实例。这个实例被保存在这个公共对象的毕包中，并且每次返回的都是这个对象。

单例类的真实定义跟在后面。

注意，我使用了惯用的模块暴露的特性，来强调这个模块公共可访问的部分。别忘了CoffeeScript会把所有的文件内容包含在一个函数块中，以此保护全局作用域不被污染。

{% highlight coffeescript %}
root = exports ? this # http://stackoverflow.com/questions/4214731/coffeescript-global-variables

# The publicly accessible Singleton fetcher
class root.Singleton
  _instance = undefined # Must be declared here to force the closure on the class
  @get: (args) -> # Must be a static method
    _instance ?= new _Singleton args

# The actual Singleton class
class _Singleton
  constructor: (@args) ->

  echo: ->
    @args

a = root.Singleton.get 'Hello A'
a.echo()
# => 'Hello A'

b = root.Singleton.get 'Hello B'
a.echo()
# => 'Hello A'

b.echo()
# => 'Hello A'

root.Singleton._instance
# => undefined

root.Singleton._instance = 'foo'

root.Singleton._instance
# => 'foo'

c = root.Singleton.get 'Hello C'
c.foo()
# => 'Hello A'

a.foo()
# => 'Hello A'
{% endhighlight %}

## 详解

在上例中，可以看出，所有的实例如此从同一个单例类的实例中输出的。

看看CoffeeScript是如何让这个设计模式变得如此简单的，对应JavaScript实现的参考和讨论，请参看[Essential JavaScript Design Patterns For Beginners](http://addyosmani.com/resources/essentialjsdesignpatterns/book/).
