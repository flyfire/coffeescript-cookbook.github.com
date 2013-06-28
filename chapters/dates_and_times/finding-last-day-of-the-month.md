---
layout: recipe
title: 找出某月的最后一天是几号 Finding the Last Day of the Month
chapter: Dates and Times
---
## 问题 Problem

你需要找出某月的最后一天，但是又不想去当年每个月的日期表。

You need to find the last day of the month, but don't want to keep a lookup table of the number of days in each month of the year.

## 方案 Solution

使用JavaScript Date下溢特性来找到该月份的前一（第-1天）天。

Use JavaScript's Date underflow to find the -1th day of the following month:

{% highlight coffeescript %}
now = new Date
lastDayOfTheMonth = new Date(1900+now.getYear(), now.getMonth()+1, 0)
{% endhighlight %}

## 讨论 Discussion

JavaScript的Date构造函数乐于处理上溢或者下溢情况，这使得日期计算很容易。让操作变得很容易，无需担心给定月份有多少天；主需要在算术上做微调即可。在12月份时，上面这个方法实际上会请求当年第13个月的第0天，结果算出来就是下一年1月第一天的前一天，即当年的12月份的第31天。

JavaScript's Date constructor cheerfully handles overflow and underflow conditions, which makes date math very easy. Given this ease of manipulation, it doesn't make sense to worry about how many days are in a given month; just nudge the math around. In December, the solution above will actually ask for the 0th day of the 13th month of the current year, which works out to the day before the 1st day of January of NEXT year, which works out to the 31st day of December of the current year.
