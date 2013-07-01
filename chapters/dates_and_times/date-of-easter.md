---
layout: recipe
title: 计算复活节岛日期 Calculate the Date of Easter Sunday
chapter: Dates and Times
---
## 问题 Problem

你需要计算出某年复活节所在地月份和日期。

You need to find the month and day of the Easter Sunday for given year.

## 方案 Solution

下面这个函数返回一个数组，包含两个元素：复活节的月份（1-12）和日期。如果省略参数，给出的就是当年的结果。这是[Anonymous Gregorian algorithm](http://en.wikipedia.org/wiki/Computus#Anonymous_Gregorian_algorithm)CoffeeScript实现。

The following function returns array with two elements: month (1-12) and day of the Easter Sunday. If no arguments are given result is for the current year. This is an implementation of [Anonymous Gregorian algorithm](http://en.wikipedia.org/wiki/Computus#Anonymous_Gregorian_algorithm) in CoffeeScript.

{% highlight coffeescript %}

gregorianEaster = (year = (new Date).getFullYear()) ->
  a = year % 19
  b = ~~(year / 100)
  c = year % 100
  d = ~~(b / 4)
  e = b % 4
  f = ~~((b + 8) / 25)
  g = ~~((b - f + 1) / 3)
  h = (19 * a + b - d - g + 15) % 30
  i = ~~(c / 4)
  k = c % 4
  l = (32 + 2 * e + 2 * i - h - k) % 7
  m = ~~((a + 11 * h + 22 * l) / 451)
  n = h + l - 7 * m + 114
  month = ~~(n / 31)
  day = (n % 31) + 1
  [month, day]

{% endhighlight %}

## 讨论 Discussion

注意！JavaScript把月份记作从0到11，因此如果调用`.getMonth()`，日期在三月份，则返回值为2，但是上面这个函数返回的是3。如果你想保持一致，你可以修改这个函数。

NB! Javascript numbers months from 0 to 11 so `.getMonth()` for date in March will return 2, this function will return 3. You can modify the function if you want this to be consistent.

这个函数还是用了`~~`技巧来代替`Math.floor()`。

The function uses ~~ trick instead of Math.floor().

{% highlight coffeescript %}

gregorianEaster()    # => [4, 24] (April 24th in 2011)
gregorianEaster 1972 # => [4, 2]

{% endhighlight %}
