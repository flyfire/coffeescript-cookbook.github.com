---
layout: recipe
title: 创建jQuery插件
chapter: jQuery
---
## 问题

你想使用CoffeeScript来创建jQuery插件。

## 方法

{% highlight coffeescript %}
# Reference jQuery
$ = jQuery

# Adds plugin object to jQuery
$.fn.extend
  # Change pluginName to your plugin's name.
  pluginName: (options) ->
    # Default settings
    settings =
      option1: true
      option2: false
      debug: false

    # Merge default settings with options.
    settings = $.extend settings, options

    # Simple logger.
    log = (msg) ->
      console?.log msg if settings.debug

    # _Insert magic here._
    return @each ()->
      log "Preparing magic show."
      # You can use your settings in here now.
      log "Option 1 value: #{settings.option1}"
{% endhighlight %}

## 详解

下面是两个使用这个新插件的例子。

### JavaScript：

{% highlight javascript %}
$("body").pluginName({
  debug: true
});

{% endhighlight %}

### CoffeeScript：

{% highlight coffeescript %}
$("body").pluginName
  debug: true

{% endhighlight %}
