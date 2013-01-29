---
layout: recipe
title: AJAX
chapter: jQuery
---
## 问题

你想使用jQuery的AJAX调用

## 方法

{% highlight coffeescript %}
$ ?= require 'jquery' # For Node.js compatibility

$(document).ready ->
	# Basic Examples
	$.get '/', (data) ->
		$('body').append "Successfully got the page."

	$.post '/',
		userName: 'John Doe'
		favoriteFlavor: 'Mint'
		(data) -> $('body').append "Successfully posted to the page."

	# Advanced Settings
	$.ajax '/',
		type: 'GET'
		dataType: 'html' error: (jqXHR, textStatus, errorThrown) ->
			$('body').append "AJAX Error: #{textStatus}"
		success: (data, textStatus, jqXHR) ->
			$('body').append "Successful AJAX call: #{data}"

{% endhighlight %}

jQuery 1.5 及更高的版本新补充了一个API，用于处理不同的事件回调。

{% highlight coffeescript %}
	request = $.get '/'
	request.success (data) -> $('body').append "Successfully got the page again."
	request.error (jqXHR, textStatus, errorThrown) -> $('body').append "AJAX Error: ${textStatus}."
{% endhighlight %}

## 详解

jQuery和 $ 这两个变量可以交换使用。参看 [回调绑定](/chapters/jquery/callback-bindings-jquery).
