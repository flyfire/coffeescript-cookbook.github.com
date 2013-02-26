---
layout: recipe
title: 更快的斐波那契算法 
chapter: Math
---
## 问题

你想计算一个数字N的斐波那契序列，但是希望计算更加效率快捷。

## 方法

以下的解决方案（仍有待提高）来源于罗宾休斯(Robin Houston)的博客中。

这里有一些链接讨论关于改进的算法和方法:
* [http://bosker.wordpress.com/2011/04/29/the-worst-algorithm-in-the-world/](http://bosker.wordpress.com/2011/04/29/the-worst-algorithm-in-the-world/)
* [http://www.math.rutgers.edu/~erowland/fibonacci](http://www.math.rutgers.edu/~erowland/fibonacci.html)
* [http://jsfromhell.com/classes/bignumber](http://jsfromhell.com/classes/bignumber)
* [http://www.math.rutgers.edu/~erowland/fibonacci](http://www.math.rutgers.edu/~erowland/fibonacci.html)
* [http://bigintegers.blogspot.com/2010/11/square-division-power-square-root](http://bigintegers.blogspot.com/2010/11/square-division-power-square-root.html)
* [http://bugs.python.org/issue3451](http://bugs.python.org/issue3451)

代码来源于gist:
[https://gist.github.com/1032685](https://gist.github.com/1032685)

{% highlight coffeescript %}
###
Author: Jason Giedymin <jasong _a_t_ apache -dot- org>
        http://www.jasongiedymin.com
        https://github.com/JasonGiedymin

CoffeeScript编译的JavaScript实现的快速菲波那契算法是基于Robin Houston的博客中的python代码。
参看上面的链接

A few things I want to introduce in time are implementions of
Newtonian, Burnikel / Ziegler, and Binet's algorithms on top
of a Big Number framework.
我想给大家及时讲解一些Newtonian, Burnikel / Ziegler和 Binet 在 Big Number框架上的具体实现。

Todo:
- https://github.com/substack/node-bigint
- BZ and Newton mods.
- Timing

###

MAXIMUM_JS_FIB_N = 1476

fib_bits = (n) ->
	#Represent an integer as an array of binary digits.

	bits = []
	while n > 0
    	[n, bit] = divmodBasic n, 2
    	bits.push bit

  	bits.reverse()
  	return bits

fibFast = (n) ->
	#Fast Fibonacci

	if n < 0
		console.log "Choose an number >= 0"
		return

	[a, b, c] = [1, 0, 1]

	for bit in fib_bits n
	    if bit
	    	[a, b] = [(a+c)*b, b*b + c*c]
	    else
	    	[a, b] = [a*a + b*b, (a+c)*b]

	    c = a + b
	  	return b

divmodNewton = (x, y) ->
	throw new Error "Method not yet implemented yet."

divmodBZ = () ->
	throw new Error "Method not yet implemented yet."

divmodBasic = (x, y) ->
	###
	Absolutely nothing special here. Maybe later versions will be Newtonian or
	Burnikel / Ziegler _if_ possible...
	###

	return [(q = Math.floor x/y), (r = if x < y then x else x % y)]

start = (new Date).getTime();
calc_value = fibFast(MAXIMUM_JS_FIB_N)
diff = (new Date).getTime() - start;
console.log "[#{calc_value}] took #{diff} ms."
{% endhighlight %}

## 讨论

还有问题吗？
