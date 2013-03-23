---
layout: recipe
title: 不依赖jQuery的Ajax请求 Ajax Request Without jQuery
chapter: Ajax
---
## 问题 Problem

你想不借助jQuery，使用AJAX从服务器上下载数据。

You want to load data from your server via AJAX without using the jQuery library.

## 方法 Solution

你可以使用原生的<a href="http://en.wikipedia.org/wiki/XMLHttpRequest" target="_blank">XMLHttpRequest</a>对象。

You will use the native <a href="http://en.wikipedia.org/wiki/XMLHttpRequest" target="_blank">XMLHttpRequest</a> object.

我们搭建一个简单的页面，该页面上有一个按钮。

Let's set up a simple test HTML page with a button.

{% highlight html %}
<!DOCTYPE HTML>
<html lang="en-US">
<head>
	<meta charset="UTF-8">
	<title>XMLHttpRequest Tester</title>
</head>
<body>
	<h1>XMLHttpRequest Tester</h1>
	<button id="loadDataButton">Load Data</button>
	
	<script type="text/javascript" src="XMLHttpRequest.js"></script>
</body>
</html>
{% endhighlight %}

当按钮被点击时，我们希望可以给服务器发送一个Ajax请求，获取一些数据。为此，我们使用了一个很小的JSON文件。

When the button is clicked, we want to send an Ajax request to the server to retrieve some data.  For this sample, we have a small JSON file.

{% highlight javascript %}
// data.json
{
  message: "Hello World"
}
{% endhighlight %}

下一步，创建一个CoffeeScript文件来处理页面逻辑。本文件中的代码创建了一个函数，当Load Data按钮被点击时调用它。

Next, create the CoffeeScript file to hold the page logic.  The code in this file creates a function to be called when the Load Data button is clicked.

{% highlight coffeescript linenos %}
# XMLHttpRequest.coffee
loadDataFromServer = ->
  req = new XMLHttpRequest()

  req.addEventListener 'readystatechange', ->
    if req.readyState is 4                        # ReadyState Compelte
      if req.status is 200 or req.status is 304   # Success result codes
        data = eval '(' + req.responseText + ')'
        console.log 'data message: ', data.message
      else
        console.log 'Error loading data...'
        
  req.open 'GET', 'data.json', false
  req.send()

loadDataButton = document.getElementById 'loadDataButton'
loadDataButton.addEventListener 'click', loadDataFromServer, false
{% endhighlight %}

## 讨论 Discussion

在上面的代码中，我们本质上是获取了一个HTML中（16行）的按钮的一个句柄，添加了一个*click*事件监听器（17行）。在我们的事件监听器中，吧我们的回调函数定义为loadDataFromServer。

In the above code we essentially grab a handle to the button in our HTML (line 16) and add a *click* event listener (line 17).  In our event listener, we define our callback function as loadDataFromServer.

我们在第2行那里定义了我们的loadDataFromServer回调函数。

We define our loadDataFromServer callback beginning on line 2.

我们创建了一个XMLHttpRequest请求对象（3行），并添加了一个*readystatechange*事件处理器。只要请求对象的readystate发生变化，该事件处理器就会被触发。

We create a XMLHttpRequest request object (line 3) and add a *readystatechange* event handler.  This fires whenever the request's readyState changes.

在事件处理器中，我们检查readyState是否等于4，即表明请求已经完成。然后，我们再查看请求的状态值，状态码为200或者304都代表这是一次成功的请求。其他状态码都表示请求出错。

In the event handler we check to see if the readyState = 4, indicating the request has completed.  Then, we check the request status value.  Both 200 or 304 represent a succsessful request.  Anything else represents an error condition.

如果请求确实成功了，我们会对从服务器端返回来的JSON进行求值，然后把其赋值给一个数据变量。这个时候，我们就可以随心所欲地使用返回的数据了。

If the request was indeed successful, we eval the JSON returned from the server and assign it to a data variable.  At this point, we can use the returned data in any way we need to.

最后一件要做的事情就是把我们的请求发出去。

The last thing we need to do is actually make our request.

13行，打开一个'GET'请求，来获取data.json文件。

Line 13 opens a 'GET' request to retreive the data.json file.

14行，把请求发给服务器。

Line 14 sends our request to the server. 

## 支持老掉牙的浏览器 Older Browser Support

如果你的程序需要支持老版本的Internet Explorer，你需要确保XMLHttpRequest对象是否存在。做法就是在创建XMLHttpRequest示例之前，先包含下面这段代码。

If your application needs to target older versions of Internet Explorer, you will need to ensure the XMLHttpRequest object exists.  You can do this by including this code before creating the XMLHttpRequest instance.

{% highlight coffeescript %}
if (typeof @XMLHttpRequest == "undefined")
  console.log 'XMLHttpRequest is undefined'
  @XMLHttpRequest = ->
    try
      return new ActiveXObject("Msxml2.XMLHTTP.6.0")
    catch error
    try
      return new ActiveXObject("Msxml2.XMLHTTP.3.0")
    catch error
    try
      return new ActiveXObject("Microsoft.XMLHTTP")
    catch error
    throw new Error("This browser does not support XMLHttpRequest.")
{% endhighlight %}

这段代码用来保证在全局作用域内存在XMLHttpRequest对象。

This code ensures the XMLHttpRequest object is available in the global namespace.

