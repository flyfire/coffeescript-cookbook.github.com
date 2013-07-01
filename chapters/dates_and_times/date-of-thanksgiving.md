---
layout: recipe
title: 计算感恩节的日期（美国和加拿大） Calculate the Date of Thanksgiving (USA and Canada)
chapter: Dates and Times
---
## 问题 Problem

你需要算出给你年份哪一天过感恩节。

You need to calculate when is Thanksgivinig in given year.

## 方案 Solution

下面这个函数将返回给定年份感恩节的日期。如果没有指定年份，就计算当年的。

The following functions return the day of Thanksgiving for a given year. If no year is given then current year is used.

美国人在每年的11月的第4个星期四庆祝他们自己的感恩节。

In the USA Thanksgiving is celebrated on the fourth Thursday in November:

{% highlight coffeescript %}

thanksgivingDayUSA = (year = (new Date).getFullYear()) ->
  first = new Date year, 10, 1
  day_of_week = first.getDay()
  22 + (11 - day_of_week) % 7

{% endhighlight %}

而在加拿大，是十月的第2个周一。

In Canada it is the second Monday in October:

{% highlight coffeescript %}

thanksgivingDayCA = (year = (new Date).getFullYear()) ->
    first = new Date year, 9, 1
    day_of_week = first.getDay()
    8 + (8 - day_of_week) % 7

{% endhighlight %}

## 讨论 Discussion

{% highlight coffeescript %}

thanksgivingDayUSA() #=> 24 (November 24th, 2011)

thanksgivingDayCA() # => 10 (October 10th, 2011)

thanksgivingDayUSA(2012) # => 22 (November 22nd)

thanksgivingDayCA(2012) # => 8 (October 8th)

{% endhighlight %}

思想非常简单：
1. 找出相应的月份第一天对应的是星期几（美国是11月，而加拿大是10月）；
2. 计算出到下一个要求的星期几有多少天（美国是周四，加拿大是周一）；
3. 再加上距离第一个可能的假期的天数（美国的感恩节是第22天，加拿大的是第8天）；

The idea is very simple:
1. Find out what day of the week is the first day of respective month (November for USA, October for Canada).
2. Calculate offset from that day to the next occurrence of weekday required (Thursday for USA, Monday for Canada).
3. Add that offset to the first possible date of the holiday (22nd for USA Thanksgiving, 8th for Canada).
