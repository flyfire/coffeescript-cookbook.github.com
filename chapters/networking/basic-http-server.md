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

What if our webserver was able to hold some data? We'll try to come up with a simple key-value store in which elements are retrievable via GET requests. Provide a key on the request path and the server will return the corresponding value &mdash; or 404 if it doesn't exist.

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

We can try several URLs to see how it responds:

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

### Use your head(ers)

Let's face it, `text/plain` is kind of lame. How about if we use something hip like `application/json` or `text/xml`? Also, our store retrieval process could use a bit of refactoring &mdash; how about some exception throwing &amp; handling? Let's see what we can come up with:

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

This server will still return the value which matches a given key, or 404 if non-existent. But it will structure the response either in JSON or XML, according to the `Accept` header. See for yourself:

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

### You gotta give to get back

The obvious last step in our adventure is to provide the client the ability to store data. We'll keep our RESTiness by listening to POST requests for this purpose.

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

Notice how the data is received in a POST request. By attaching some handlers on the `'data'` and `'end'` events of the request object, we're able to buffer and finally save the data from the client in the `store`.

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

Give `http.createServer` a function in the shape of `(request, response) -> ...` and it will return a server object, which we can use to listen on a port. Interact with the `request` and `response` objects to give the server its behaviour. Listen on port 8000 using `server.listen 8000`.

For API and overall information on this subject, check node.js's [http](http://nodejs.org/docs/latest/api/http.html) and [https](http://nodejs.org/docs/latest/api/https.html) documentation pages. Also, the [HTTP spec](http://www.ietf.org/rfc/rfc2616.txt) might come in handy.

### Exercises

* Create a layer in between the server and the developer which would allow the developer to do something like:

{% highlight coffeescript %}
server = layer.createServer
    'GET /': (req, res) ->
        ...
    'GET /page': (req, res) ->
        ...
    'PUT /image': (req, res) ->
        ...
{% endhighlight %}

