---
layout: recipe
title: 最简单的HTTP客户端
chapter: Networking
---

## 问题

你想创建一个HTTP客户端

## 方法

在本菜谱中，我们将使用[node.js](http://nodejs.org/)的HTTP库。我们先从一个简单的GET请求示例开始，然后实现可以返回电脑真实IP的客户端。

## GET些啥

{% highlight coffeescript %}
http = require 'http'

http.get { host: 'www.google.com' }, (res) ->
    console.log res.statusCode
{% endhighlight %}

`get`函数是node.js的`http`模块提供，可以向HTTP服务器发送一个GET请求。响应会以回调的形式返回，我们可以在一个函数中处理它。本例只是简单地把响应的状态码打印出来。请看：

{% highlight console %}
$ coffee http-client.coffee 
200

{% endhighlight %}

### 我的IP地址是多杀？

如果你处在一个像LAN这样的网络中，依赖于[NAT](http://en.wikipedia.org/wiki/Network_address_translation)，你有时候可能会碰到这样的问题，我真实的IP地址是多少呢？让我编写一小段coffeescript来搞定它：

{% highlight coffeescript %}
http = require 'http'

http.get { host: 'checkip.dyndns.org' }, (res) ->
    data = ''
    res.on 'data', (chunk) ->
        data += chunk.toString()
    res.on 'end', () ->
        console.log data.match(/([0-9]+\.){3}[0-9]+/)[0]
{% endhighlight %}

我们可以监听`'data'`事件，从返回的对象中获取数据；并且当`'end'`事件触发时，我们可以知道数据传送完了。当传送结束时，我们可以使用一个简单的正则表达式来匹配出我们的IP地址，试试看：

{% highlight console %}
$ coffee http-client.coffee 
123.123.123.123
{% endhighlight %}

## 详解

要知道`http.get`是`http.request`的快捷方式。后者允许你使用不同的方法发送HTTP请求，比如说POST或者PUT。

关于这个主题的API或者更为详细的信息，请参考[http](http://nodejs.org/docs/latest/api/http.html)以及[https](http://nodejs.org/docs/latest/api/https.html)这两页文档。而且[HTTP spec](http://www.ietf.org/rfc/rfc2616.txt)迟早也会用到。

### 练习

* 基于[Basic HTTP Server](basic-http-server)，创建一个针对键值对存储的HTTP服务器的客户端。
