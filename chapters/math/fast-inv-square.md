---
layout: recipe
title: 快速逆平方根
chapter: Math
---
## 问题

你想[快速][5]的计算一个数字的逆平方根。

## 方法
根据 Quake III Arena [源代码][1], 这种奇怪的算法使用整数运算，以及一个‘神奇的数字(幻数)’来计算逆平方根的近似值。
在此CoffeeScript的变种中，我不仅提供原始的经典之作，还有由[Chris Lomont][2]发现的新最佳的32位幻数,此外还有64位大小的幻数。

包含的另一个特征是能够改变的精度水平。 这是通过控制执行[牛顿法][3]的迭代次数实现。

这个算法在经典的解法上仍然可以提升性能，这取决于计算机和精确水平。

用coffee编译运行这段脚本：
    coffee -c script.coffee

然后复制粘贴编译后的js代码到你的浏览器JavaScript控制台中。

提示：你需要一个浏览器支持 [类型化数组][4]。

参考:
1. [ftp://ftp.idsoftware.com/idstuff/source/quake3-1.32b-source.zip](ftp://ftp.idsoftware.com/idstuff/source/quake3-1.32b-source.zip)
2. [http://www.lomont.org/Math/Papers/2003/InvSqrt.pdf](http://www.lomont.org/Math/Papers/2003/InvSqrt.pdf)
3. [http://en.wikipedia.org/wiki/Newton%27s_method](http://en.wikipedia.org/wiki/Newton%27s_method)
4. [https://developer.mozilla.org/en/JavaScript_typed_arrays](https://developer.mozilla.org/en/JavaScript_typed_arrays)
5. [http://en.wikipedia.org/wiki/Fast_inverse_square_root](http://en.wikipedia.org/wiki/Fast_inverse_square_root)

[1]: ftp://ftp.idsoftware.com/idstuff/source/quake3-1.32b-source.zip "ftp://ftp.idsoftware.com/idstuff/source/quake3-1.32b-source.zip"
[2]: http://www.lomont.org/Math/Papers/2003/InvSqrt.pdf "http://www.lomont.org/Math/Papers/2003/InvSqrt.pdf"
[3]: http://en.wikipedia.org/wiki/Newton%27s_method "http://en.wikipedia.org/wiki/Newton%27s_method"
[4]: https://developer.mozilla.org/en/JavaScript_typed_arrays "https://developer.mozilla.org/en/JavaScript_typed_arrays"
[5]: http://en.wikipedia.org/wiki/Fast_inverse_square_root "http://en.wikipedia.org/wiki/Fast_inverse_square_root"

源代码来源于gist:
[https://gist.github.com/1036533](https://gist.github.com/1036533)

{% highlight coffeescript %}
###

Author: Jason Giedymin <jasong _a_t_ apache -dot- org>
        http://www.jasongiedymin.com
        https://github.com/JasonGiedymin

Appearing in the Quake III Arena source code[1], this strange algorithm uses
integer operations along with a 'magic number' to calculate floating point
approximation values of inverse square roots[5].

In this CoffeeScript variant I supply the original classic, and newer optimal
32 bit magic numbers found by Chris Lomont[2]. Also supplied is the 64-bit
sized magic number.

Another feature included is the ability to alter the level of precision.
This is done by controling the number of iterations for performing Newton's
method[3].

Depending on the machine and level of percision this algorithm may still
provide performance increases over the classic.

To run this, compile the script with coffee:
    coffee -c <this script>.coffee

Then copy & paste the compiled js code in to the JavaSript console of your
browser.

Note: You will need a browser which supports typed-arrays[4].

References: 
[1] ftp://ftp.idsoftware.com/idstuff/source/quake3-1.32b-source.zip
[2] http://www.lomont.org/Math/Papers/2003/InvSqrt.pdf
[3] http://en.wikipedia.org/wiki/Newton%27s_method
[4] https://developer.mozilla.org/en/JavaScript_typed_arrays
[5] http://en.wikipedia.org/wiki/Fast_inverse_square_root

###

approx_const_quake_32 = 0x5f3759df # See [1]
approx_const_32 = 0x5f375a86 # See [2]
approx_const_64 = 0x5fe6eb50c7aa19f9 # See [2]

fastInvSqrt_typed = (n, precision=1) ->
	# Using typed arrays. Right now only works in browsers.
	# Node.JS version coming soon.

    y = new Float32Array(1)
    i = new Int32Array(y.buffer)

    y[0] = n
    i[0] = 0x5f375a86 - (i[0] >> 1)
    
    for iter in [1...precision]
        y[0] = y[0] * (1.5 - ((n * 0.5) * y[0] * y[0]))
    
    return y[0]

### Sample single runs ###
testSingle = () ->
    example_n = 10

    console.log("Fast InvSqrt of 10, precision 1: #{fastInvSqrt_typed(example_n)}")
    console.log("Fast InvSqrt of 10, precision 5: #{fastInvSqrt_typed(example_n, 5)}")
    console.log("Fast InvSqrt of 10, precision 10: #{fastInvSqrt_typed(example_n, 10)}")
    console.log("Fast InvSqrt of 10, precision 20: #{fastInvSqrt_typed(example_n, 20)}")
    console.log("Classic of 10: #{1.0 / Math.sqrt(example_n)}")

testSingle()

{% endhighlight %}

## 讨论

有问题吗？
