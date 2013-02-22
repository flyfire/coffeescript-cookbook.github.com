---
layout: recipe
title: 简单的服务器
chapter: Networking
---
## 问题

你想要提供一个网络服务。

## 方法

创建一个简单的TCP服务器。

### 使用Node.js

{% highlight coffeescript %}
net = require 'net'

domain = 'localhost'
port = 9001

server = net.createServer (socket) ->
	console.log "Received connection from #{socket.remoteAddress}"
	socket.write "Hello, World!\n"
	socket.end()

console.log "Listening to #{domain}:#{port}"
server.listen port, domain
{% endhighlight %}

### Example Usage
### 使用示例

Accessed by the [Basic Client](/chapters/networking/basic-client):

{% highlight console %}
$ coffee basic-server.coffee
Listening to localhost:9001
Received connection from 127.0.0.1
Received connection from 127.0.0.1
[...]
{% endhighlight %}

## 详解

The function passed to @net.createServer@ receives the new socket provided for each new connection to a client.  This basic server simply socializes with its visitors but a hard-working server would pass this socket along to a dedicated handler and then return to the task of waiting for the next client.

See also the [Basic Client](/chapters/networking/basic-client), [Bi-Directional Server](/chapters/networking/bi-directional-server), and [Bi-Directional Client](/chapters/networking/bi-directional-client) recipes.

### Exercises
### 练习

* Add support for choosing the target domain and port based on command-line arguments or from a configuration file.

