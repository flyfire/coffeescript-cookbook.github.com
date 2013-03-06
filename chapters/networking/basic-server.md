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

### 使用示例

使用[简单的客户端](/chapters/networking/basic-client)访问：

{% highlight console %}
$ coffee basic-server.coffee
Listening to localhost:9001
Received connection from 127.0.0.1
Received connection from 127.0.0.1
[...]
{% endhighlight %}

## 详解

传递给@net.createServer@的函数接受一个新的socket对象，这个socket对象会提供一个指向客户端的链接。这种简单的服务直接就和访问这个沟通，但如果是比较繁忙的服务器。会把这个socket单独传递给一个专属的处理器。然后会过来继续会过来完成任务——等待下一个客户端请求。

参看[简单的客户端](/chapters/networking/basic-client), [双向服务器](/chapters/networking/bi-directional-server), 以及[双向客户端](/chapters/networking/bi-directional-client)这几分“菜肴”.


### 练习

* 添加自定义domain和端口的支持，可基于命令行参数，也可以使用配置文件。

