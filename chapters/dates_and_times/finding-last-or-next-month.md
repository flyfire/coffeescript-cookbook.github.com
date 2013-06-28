---
layout: recipe
title: 计算上一个（下一个）月份 Finding Last (or Next) Month
chapter: Dates and Times
---
## 问题 Problem

你需要计算出一个相对的日期范围，比如说“上个月”或者“下个月”。

You need to calculate a relative date range like "last month" or "next month".

## 方法 Solution

在当前月份上做加法或者减法，这没什么问题，要知道JavaScript的Date构造器会处理好这些计算。

Add or subtract from the current month, secure in the knowledge that JavaScript's Date constructor will fix up the math.

{% highlight coffeescript %}
# these examples were written in GMT-6
# Note that these examples WILL work in January!
now = new Date
# => "Sun, 08 May 2011 05:50:52 GMT"

lastMonthStart = new Date 1900+now.getYear(), now.getMonth()-1, 1
# => "Fri, 01 Apr 2011 06:00:00 GMT"

lastMonthEnd = new Date 1900+now.getYear(), now.getMonth(), 0
# => "Sat, 30 Apr 2011 06:00:00 GMT"
{% endhighlight %}

## 讨论 Discussion

JavaScript的Date对象乐于处理星期或者月份上的上溢或者下溢的情况，并会对日期进行相应的调整。例如，你可以获取三月的第42天，你会得到四月的第11天。

JavaScript Date objects will cheerfully handle underflows and overflows in the month and day fields, and will adjust the date object accordingly. You can ask for the 42nd of March, for example, and will get the 11th of April.

JavaScript的日期对象的年份存储的是从1900年来的第几年，月份从0到11，而日期是从1到31。在上面的方案中，last_month_start获取的是当前年份这个月的第一天，但是月份是从-1到10，如果月份是-1，则实际上Date会返回上一年的十二月份。

JavaScript Date objects store the year as the number of years since 1900, the month as an integer from 0 to 11, and the date (day of month) as an integer from 1 to 31. In the solution above, last_month_start is obtained by asking for the first day of a month in the current year, but the month is -1 to 10. If month is -1 the Date object will actually return December of the previous year:

{% highlight coffeescript %}
lastNewYearsEve = new Date 1900+now.getYear(), -1, 31
# => "Fri, 31 Dec 2010 07:00:00 GMT"
{% endhighlight %}

对于上溢也是一样的：

The same is true for overflows:

{% highlight coffeescript %}
thirtyNinthOfFourteember = new Date 1900+now.getYear(), 13, 39
# => "Sat, 10 Mar 2012 07:00:00 GMT"
{% endhighlight %}
