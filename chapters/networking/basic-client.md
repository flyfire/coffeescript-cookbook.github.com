---
layout: recipe
title: 简单的客户端
chapter: Networking
---
## 问题

你想访问一个网络客户端。

## 方法

创建一个简单的TCP客户端。

### 使用Node.js

{% highlight coffeescript %}
net = require 'net'

domain = 'localhost'
port = 9001

connection = net.createConnection port, domain

connection.on 'connect', () ->
	console.log "Opened connection to #{domain}:#{port}."

connection.on 'data', (data) ->
	console.log "Received: #{data}"
	connection.end()
{% endhighlight %}

### 用法示例

访问这个[简单的客户端](/chapters/networking/basic-server)：

{% highlight console %}
$ coffee basic-client.coffee
Opened connection to localhost:9001
Received: Hello, World!
{% endhighlight %}

## 详解

_connection.on 'data'_处理器中包含了最关键的地方，客户端从服务端接受数据，也许还需要安排返回的数据。

参看[简单的服务器]、[Bi-Directional Client](/chapters/networking/bi-directional-client)和[Bi-Directional Server](/chapters/networking/bi-directional-server)。

### 练习

* 支持domian和端口的自定义，从命令行接受参数，或者从一个配置文件。

