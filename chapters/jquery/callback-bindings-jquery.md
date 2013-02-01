---
layout: recipe
title: 回调绑定 # using => instead of ->
chapter: jQuery
---
## 问题

你想给一个对象绑定回调事件。

## 方法

{% highlight coffeescript %}
$ ->
  class Basket
    constructor: () ->
      @products = []

      $('.product').click (event) =>
        @add $(event.currentTarget).attr 'id'

    add: (product) ->
      @products.push product
      console.log @products

  new Basket()
{% endhighlight %}

## 详解

通过使用胖箭头 (`=>`) 取代单箭头 (`->`), 回调函数会自动获取上下文中的对象并进行绑定。
