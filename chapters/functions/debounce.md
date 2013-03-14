---
layout: recipe
title: 反抖动函数
chapter: Functions
---
## 问题 Problem

You want to execute a function only once, coalescing multiple sequential calls into a single execution at the beginning or end.

你只想执行函数一次，把一系列的调用合并成一个单一的调用，在这一些列调用的开头或者结尾执行。

## 方法 Solution

With a named function:

使用一个有名的函数：

{% highlight coffeescript %}
debounce: (func, threshold, execAsap) ->
  timeout = null
  (args...) ->
    obj = this
    delayed = ->
      func.apply(obj, args) unless execAsap
      timeout = null
    if timeout
      clearTimeout(timeout)
    else if (execAsap)
      func.apply(obj, args)
    timeout = setTimeout delayed, threshold || 100
{% endhighlight %}

{% highlight coffeescript %}
mouseMoveHandler: (e) ->
  @debounce((e) ->
    # Do something here, but only once 300 milliseconds after the mouse cursor stops.
  300)

someOtherHandler: (e) ->
  @debounce((e) ->
    # Do something here, but only once 250 milliseconds after initial execuction.
  250, true)
{% endhighlight %}

## 详解 Discussion

Learn about [debouncing JavaScript methods](http://unscriptable.com/2009/03/20/debouncing-javascript-methods/) at John Hann's excellent blog article.

参看John Hann的这篇很赞的文章[debouncing JavaScript methods](http://unscriptable.com/2009/03/20/debouncing-javascript-methods/)。

