---
layout: recipe
title: 双向服务端 Bi-Directional Server
chapter: Networking
---
## 问题 Problem

你希望网络中能有一个持久的服务，能够与它的客户端保持一个持续的链接。

You want to provide a persistent service over a network, one which maintains an on-going connection with a client.

## 方法 Solution

创建一个双向通信的TCP服务器。
Create a bi-directional TCP server.

### 用Node.js In Node.js

{% highlight coffeescript %}
net = require 'net'

domain = 'localhost'
port = 9001

server = net.createServer (socket) ->
	console.log "New connection from #{socket.remoteAddress}"

	socket.on 'data', (data) ->
		console.log "#{socket.remoteAddress} sent: #{data}"
		others = server.connections - 1
		socket.write "You have #{others} #{others == 1 and "peer" or "peers"} on this server"

console.log "Listening to #{domain}:#{port}"
server.listen port, domain
{% endhighlight %}

### 示例 Example Usage

使用[双向通信的客户端](/chapters/networking/bi-directional-client)。

Accessed by the [Bi-Directional Client](/chapters/networking/bi-directional-client):

{% highlight console %}
$ coffee bi-directional-server.coffee
Listening to localhost:9001
New connection from 127.0.0.1
127.0.0.1 sent: Ping
127.0.0.1 sent: Ping
127.0.0.1 sent: Ping
[...]
{% endhighlight %}

## 讨论 Discussion

大部分工作都在@socket.on 'data'@这个处理器中，所有从客户端传过来的数据都在这里处理。一个真实的服务器会把数据传递给另外一个函数，对其进行处理，并产生响应供原始的处理器使用。

The bulk of the work lies in the @socket.on 'data'@ handler, which processes all of the input from the client.  A real server would likely pass the data onto another function to process it and generate any responses so that the original handler.

参看[双向的客户端](/chapters/networking/bi-directional-client), [最简单的客户端](/chapters/networking/basic-client), 以及[最简单的服务器](/chapters/networking/basic-server)这几个菜谱。

See also the [Bi-Directional Client](/chapters/networking/bi-directional-client), [Basic Client](/chapters/networking/basic-client), and [Basic Server](/chapters/networking/basic-server) recipes.

### 练习 Exercises

* 添加自定义domain和端口的支持，可基于命令行参数，也可以使用配置文件。

* Add support for choosing the target domain and port based on command-line arguments or on a configuration file.
