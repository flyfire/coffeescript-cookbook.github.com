---
layout: recipe
title: 类方法和实例方法
chapter: Classes and Objects
---
## 问题

我想创建一个类方法和实例方法

## 方法

### 类方法

{% highlight coffeescript %}

class Songs
  @_titles: 0    # Although it's directly accessible, the leading _ defines it by convention as private property.

  @get_count: ->
    @_titles

  constructor: (@artist, @title) ->
    Songs._titles++

Songs.get_count()
# => 0

song = new Songs("Rick Astley", "Never Gonna Give You Up")
Songs.get_count()
# => 1

song.get_count()
# => TypeError: Object <Songs> has no method 'get_count'

{% endhighlight %}

### 实例方法

{% highlight coffeescript %}

class Songs
  _titles: 0    # Although it's directly accessible, the leading _ defines it by convention as private property.

  get_count: ->
    @_titles

  constructor: (@artist, @title) ->
    @_titles++

song = new Songs("Rick Astley", "Never Gonna Give You Up")
song.get_count()
# => 1

Songs.get_count()
# => TypeError: Object function Songs(artist, title) ... has no method 'get_count'

{% endhighlight %}


## 详细

CoffeeScript把类方法放到类对象本身，而不是放到类对象的原型上，这样能够节省内存，且能够集中式地存储类属性。
