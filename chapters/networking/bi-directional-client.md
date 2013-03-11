---
layout: recipe
title: 双向客户端
chapter: Networking
---
## 问题

你希望网络中能有一个持久的服务，能够与它的客户端保持一个持续的链接。

## 方法

创建一个双向的TCP客户端。

### 使用Node.js来实现

{% highlight coffeescript %}
net = require 'net'

domain = 'localhost'
port = 9001

ping = (socket, delay) ->
	console.log "Pinging server"
	socket.write "Ping"
	nextPing = -> ping(socket, delay)
	setTimeout nextPing, delay

connection = net.createConnection port, domain

connection.on 'connect', () ->
	console.log "Opened connection to #{domain}:#{port}"
	ping connection, 2000

connection.on 'data', (data) ->
	console.log "Received: #{data}"

connection.on 'end', (data) ->
	console.log "Connection closed"
	process.exit()
{% endhighlight %}

### 示例

访问[双向服务器](/chapters/networking/bi-directional-server)：

{% highlight console %}
$ coffee bi-directional-client.coffee
Opened connection to localhost:9001
Pinging server
Received: You have 0 peers on this server
Pinging server
Received: You have 0 peers on this server
Pinging server
Received: You have 1 peer on this server
[...]
Connection closed
{% endhighlight %}

## 讨论

这个特例开始与服务器交互，并在@connection.on 'connect'@处理器中与服务器交流。然而，在一个真实的客户端中，绝大部分的工作都依赖与@connection.on 'data'@处理器，它能够处理服务端的输出。重复的@ping@函数，仅仅只是为了表示与服务端的交互是持续性的，实际上在真实的客户端中可以把它移除。

参看[双向的服务器](/chapters/networking/bi-directional-server), [最基本的客户端](/chapters/networking/basic-client)这两个菜谱。

### 练习

* 添加自定义domain和端口的支持，可基于命令行参数，也可以使用配置文件。
