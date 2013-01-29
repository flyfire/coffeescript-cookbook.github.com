---
layout: recipe
title: 在服务端和客户端重用代码
chapter: Syntax
---
## 问题

你用CoffeeScript写了一些功能模块，你既希望它们能在客户端跑在浏览器上，也希望能够通过Node.js跑在服务端。

## 方法

通过下面这样的方式暴露这些功能模块：

{% highlight coffeescript %}

# simpleMath.coffee

# these methods are private
add = (a, b) ->
	a + b

subtract = (a, b) ->
	a - b

square = (x) ->
	x * x

# create a namespace to export our public methods
SimpleMath = exports? and exports or @SimpleMath = {}

# items attached to our namespace are available in Node.js as well as client browsers
class SimpleMath.Calculator
	add: add
	subtract: subtract
	square: square

{% endhighlight %}

## 详解

如上例，我们创建了一个新的命名空间——SimpleMath。如果`exports`存在，我们的类就做为一个Node.js模块暴露出来，如果`exports`*不*存在，SimpleMath被加到全局中，然后就可以在页面上使用它了。

在Node.js中，我们可以使用`require`命令包含这个模块。

{% highlight console %}

$ node

> var SimpleMath = require('./simpleMath');
undefined
> var Calc = new SimpleMath.Calculator();
undefined
> console.log("5 + 6 = ", Calc.add(5, 6));
5 + 6 =  11
undefined
> 

{% endhighlight %}

在页面中，把它作为一个script引入进来我们就可以使用这个模块了。

{% highlight html %}

<!DOCTYPE HTML>
<html lang="en-US">
<head>
	<meta charset="UTF-8">
	<title>SimpleMath Module Example</title>
	<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js"></script>
	<script src="simpleMath.js"></script>
	<script>
		jQuery(document).ready(function	(){
			var Calculator = new SimpleMath.Calculator();
			var result = $('<li>').html("5 + 6 = " + Calculator.add(5, 6));
			$('#SampleResults').append(result);	
		});
	</script>
</head>
<body>
	<h1>A SimpleMath Example</h1>
	<ul id="SampleResults"></ul>
</body>
</html>

{% endhighlight %}

结果：

#一个简单的例子
* 5 + 6 = 11
