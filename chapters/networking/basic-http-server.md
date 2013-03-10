---
layout: recipe
title: 最简单的HTTP服务器
chapter: Networking
---

## 问题

你想到网络中搭建一个HTTP服务器。在本菜谱中，我们从最简单的服务器到一个功能完好的键值对存储服务器，一步步地学习创建HTTP服务器。

## 方法

处于自私的目的，我们将使用[node.js](http://nodejs.org/)的HTTP类库，使用CoffeeScript创建最简单的服务器。

### 问候 'hi\n'

我们可以从引入node.js的HTTP模块开始。该模块包含了`createServer`，这是一个简单的请求处理器，返回一个HTTP服务器。我们让这个服务器监听在一个TCP端口上。

{% highlight coffeescript %}
http = require 'http'
server = http.createServer (req, res) -> res.end 'hi\n'
server.listen 8000
{% endhighlight %}

把这些代码放在一个文件中运行，就可以执行这个示例。你可以使用`Ctrl-C`来关掉它。我们可以使用`curl`命令测试，在绝大多数的\*nix平台上都可以运行：


{% highlight console %}
$ curl -D - http://localhost:8000/
HTTP/1.1 200 OK
Connection: keep-alive
Transfer-Encoding: chunked

hi
{% endhighlight %}

### 接下来呢？

让我们弄点反馈，看看我们服务器上发生了什么。同时，我们还对我们用户更友好一点，并为他们提供一些HTTP头。

{% highlight coffeescript %}
http = require 'http'

server = http.createServer (req, res) ->
    console.log req.method, req.url
    data = 'hi\n'
    res.writeHead 200,
        'Content-Type':     'text/plain'
        'Content-Length':   data.length
    res.end data

server.listen 8000
{% endhighlight %}

试着再访问一次这个服务器，但是这次要使用其他的URL地址。比如 `http://localhost:8000/coffee`。你可以在服务器上看到如下的调试信息：

{% highlight console %}
$ coffee http-server.coffee 
GET /
GET /coffee
GET /user/1337
{% endhighlight %}

### GET点啥

服务器上放点数据？我们就放一个简单的键值存储吧，键值元素可以通过GET请求获取。把key放到请求路径中，服务器就会返回相应的value &mdash，如果不错在的话就返回404。

{% highlight coffeescript %}
http = require 'http'

store = # we'll use a simple object as our store
    foo:    'bar'
    coffee: 'script'

server = http.createServer (req, res) ->
    console.log req.method, req.url
    
    value = store[req.url[1..]]

    if not value
        res.writeHead 404
    else
        res.writeHead 200,
            'Content-Type': 'text/plain'
            'Content-Length': value.length + 1
        res.write value + '\n'
    
    res.end()

server.listen 8000
{% endhighlight %}

我们可以找几个URLs尝试一下，看看他是如何返回的：

{% highlight console %}
$ curl -D - http://localhost:8000/coffee
HTTP/1.1 200 OK
Content-Type: text/plain
Content-Length: 7
Connection: keep-alive

script

$ curl -D - http://localhost:8000/oops
HTTP/1.1 404 Not Found
Connection: keep-alive
Transfer-Encoding: chunked

{% endhighlight %}

### 加上headers

承认吧，`text/plain`挺无聊的。我们要不使用`application/json`或者`text/xml`等试试看？并且，我们的读取存储的过程是不是应该重构一下&mdash，添加点异常处理？让我们看看能产生什么效果：

{% highlight coffeescript %}
http = require 'http'

# known mime types
[any, json, xml] = ['*/*', 'application/json', 'text/xml']

# gets a value from the db in format [value, contentType]
get = (store, key, format) ->
    value = store[key]
    throw 'Unknown key' if not value
    switch format
        when any, json then [JSON.stringify({ key: key, value: value }), json]
        when xml then ["<key>#{ key }</key>\n<value>#{ value }</value>", xml]
        else throw 'Unknown format'

store =
    foo:    'bar'
    coffee: 'script'

server = http.createServer (req, res) ->
    console.log req.method, req.url
    
    try
        key = req.url[1..]
        [value, contentType] = get store, key, req.headers.accept
        code = 200
    catch error
        contentType = 'text/plain'
        value = error
        code = 404
 
    res.writeHead code,
        'Content-Type': contentType
        'Content-Length': value.length + 1
    res.write value + '\n'
    res.end()

server.listen 8000
{% endhighlight %}

服务器返回的还是key能够匹配到的值，无匹配的话就返回404。但是它根据`Accept`头，把返回值格式化成了JSON或者XML。自己试试看：

{% highlight console %}
$ curl http://localhost:8000/
Unknown key

$ curl http://localhost:8000/coffee
{"key":"coffee","value":"script"}

$ curl -H "Accept: text/xml" http://localhost:8000/coffee
<key>coffee</key>
<value>script</value>

$ curl -H "Accept: image/png" http://localhost:8000/coffee
Unknown format
{% endhighlight %}

### 你必须让他们有恢复的能力

在我们冒险旅行的上一步，给我们的客户端提供了存储数据的能力。我们会保证我们是RESTfull的，提供对POST请求的监听。

{% highlight coffeescript %}
http = require 'http'

# known mime types
[any, json, xml] = ['*/*', 'application/json', 'text/xml']

# gets a value from the db in format [value, contentType]
get = (store, key, format) ->
    value = store[key]
    throw 'Unknown key' if not value
    switch format
        when any, json then [JSON.stringify({ key: key, value: value }), json]
        when xml then ["<key>#{ key }</key>\n<value>#{ value }</value>", xml]
        else throw 'Unknown format'

# puts a value in the db
put = (store, key, value) ->
    throw 'Invalid key' if not key or key is ''
    store[key] = value

store =
    foo:    'bar'
    coffee: 'script'

# helper function that responds to the client
respond = (res, code, contentType, data) ->
    res.writeHead code,
        'Content-Type': contentType
        'Content-Length': data.length
    res.write data
    res.end()

server = http.createServer (req, res) ->
    console.log req.method, req.url
    key = req.url[1..]
    contentType = 'text/plain'
    code = 404
    
    switch req.method
        when 'GET'
            try
                [value, contentType] = get store, key, req.headers.accept
                code = 200
            catch error
                value = error
            respond res, code, contentType, value + '\n'

        when 'POST'
            value = ''
            req.on 'data', (chunk) -> value += chunk
            req.on 'end', () ->
                try
                    put store, key, value
                    value = ''
                    code = 200
                catch error
                    value = error + '\n'
                respond res, code, contentType, value

server.listen 8000
{% endhighlight %}

请注意是如何接受POST请求中的数据的。听过给请求对象的`'data'`和`'end'`事件绑定处理器来实现。我们可以把来来自客户端的数据暂存或者最终存储到`store`中。

{% highlight console %}
$ curl -D - http://localhost:8000/cookie
HTTP/1.1 404 Not Found # ...
Unknown key

$ curl -D - -d "monster" http://localhost:8000/cookie
HTTP/1.1 200 OK # ...

$ curl -D - http://localhost:8000/cookie
HTTP/1.1 200 OK # ...
{"key":"cookie","value":"monster"}
{% endhighlight %}

## Discussion
## 讨论

给`http.createServer`传递一个型如`(request, respone) ->...`这样的函数，它就会返回一个server对象，我们可以使用这个server对象监听某个端口。与`request`和`response`对象交互，实现server的行为。使用`server.listen 8000`来监听8000端口。

关于这个主题的API或者更为详细的信息，请参考[http](http://nodejs.org/docs/latest/api/http.html)以及[https](http://nodejs.org/docs/latest/api/https.html)这两页文档。而且[HTTP spec](http://www.ietf.org/rfc/rfc2616.txt)迟早也会用到。

### 练习

* 在服务器和开发者之间建立一个layer（layer），允许开发者可以像下面这样写：

{% highlight coffeescript %}
server = layer.createServer
    'GET /': (req, res) ->
        ...
    'GET /page': (req, res) ->
        ...
    'PUT /image': (req, res) ->
        ...
{% endhighlight %}
