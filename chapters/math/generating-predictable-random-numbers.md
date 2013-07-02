---
layout: recipe
title: 生成可预测的随机数
chapter: Math
---
## 问题

你需要在一定范围内生成一个随机数，但是你也需要设定生成程序以提供可预测的数据。

## 方法

写你自己的随机数生成程序。有__很多__种方法去实现它。这里有个简单的示例。_这个生成程序绝对+不适用+于加密目的！_

{% highlight coffeescript %}
class Rand
  # if created without a seed, uses current time as seed
  constructor: (@seed) ->
    # Knuth and Lewis' improvements to Park and Miller's LCPRNG
    @multiplier = 1664525
    @modulo = 4294967296 # 2**32-1;
    @offset = 1013904223
    unless @seed? && 0 <= seed < @modulo
      @seed = (new Date().valueOf() * new Date().getMilliseconds()) % @modulo

  # sets new seed value
  seed: (seed) ->
    @seed = seed

  # return a random integer 0 <= n < @modulo
  randn: ->
    # new_seed = (a * seed + c) % m
    @seed = (@multiplier*@seed + @offset) % @modulo

 # return a random float 0 <= f < 1.0
  randf: ->
    this.randn() / @modulo

  # return a random int 0 <= f < n
  rand: (n) ->
    Math.floor(this.randf() * n)

  # return a random int min <= f < max
  rand2: (min, max) ->
    min + this.rand(max-min)
{% endhighlight %}

## 讨论 

JavaScript 和 CoffeeScript 并没有提供可以预先设置的随机数生成程序。写你自己的随机函数将主要练习用一种用简单随机生成程序去生成大量随机数。关于随机过程的讨论超出了这本Cookbook的范畴。有兴趣的同学可以阅读Donald Knuth的_The Art of Computer Programming_的第二篇第三章节"Random Numbles"，以及_Numerical Recipes in C_第二版第七章"Random Numbers"。

然而，一个关于随机数生成程序的简单解释是有序。它就是Linear Congruential Pseudorandom Number Generator(线性全等伪随机数产生器)。LCPRNG在数学上的表现形式是`I<sub>j+1</sub> = (aI<sub>j</sub>+c) % m`，在公式中，a是乘数，c是偏移常数，m是模数。

每请求一次随机数，一个很大规模的乘法器和加法器在运行——"很大"是相对于主要的区间而言——并且结果数被模除后，分布在主要的区间中。

这种随机数生成器的位数范围是2<sup>32</sup>，如果以加密为目的，这几乎不可以接受，要不是大多数简单没有规则的需求，它足够适用。在重复自己之前，`randn()`会覆盖整个区间，而下一个随机数由前一个决定。

如果你想粗略的修补一下随机程序，_强烈_建议你阅读 Knuth 的 _The Art of Computer Programming_ 的第三章。随机数生成程序很容易被搞砸，而且 Knuth 从一个糟糕的案例中解释怎样去判断一个好的随机数生成程序(RNG, Random Number Generator)。

避免把这个生成的结果进行模除。如果你需要一个整数的范围，用除法。线性全等生成器在数据位数很小时并不随机。这种算法特别在当偶数种子的时候，会产生奇数。因此，如果你需要一个0或1的随机数，__不要__使用下面的方法：

{% highlight coffeescript %}
# NOT random! Do not do this!
r.randn() % 2
{% endhighlight %}

因为你几乎可以肯定不会得到随机位数。用`r.randi(2)`代替上面的方法。
