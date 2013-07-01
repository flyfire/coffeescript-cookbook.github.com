---
layout: recipe
title: 获取两个日期相差的天数 Get Days Between Two Dates
chapter: Dates and Times
---
## 问题 Problem

你需要获取两个日期之间差了多少分钟，或者是差了多少小时、天、月和年？

You need to find how much seconds minutes, hours, days, months or years has passed between two dates.

## 方法 Solution

使用JavaScript Date的`getTime()`方法，该方法会返回从01/01/1970到现在过去的毫秒数。

Use JavaScript's Date function `getTime()`. Which provides how much time in miliseconds has passed since 01/01/1970:

{% highlight coffeescript %}
DAY = 1000 * 60 * 60  * 24

d1 = new Date('02/01/2011')
d2 = new Date('02/06/2011')

days_passed = Math.round((d2.getTime() - d1.getTime()) / DAY)
{% endhighlight %}

## 讨论 Discussion

使用毫秒避免了使用Date溢出的错误。因此我们首先计算出一天包含多少毫秒。然后，给定两个不一样的日期，只需获取这两个日期相差的毫秒数，再除以一天包含的毫秒数，就可以得到这两个日期之间包含了多少天。

Using miliseconds makes the life easier to avoid overflow mistakes with Dates. So we first calculate how much miliseconds has a day. Then, given two distincit dates, just get the diference in miliseconds betwen two dates and then divide by how much miliseconds has a day. It will get you the days between two distinct dates.

如果你想要计算两个日期隔了多少小时，你只需要除以一小时的毫秒数就行。分钟和秒数如法炮制。

If you'd like to calculate the hours between two dates objects you can do that just by dividing the diference in miliseconds by the convertion of miliseconds to hours. The same goes to minutes and seconds.

{% highlight coffeescript %}
HOUR = 1000 * 60 * 60

d1 = new Date('02/01/2011 02:20')
d2 = new Date('02/06/2011 05:20')

hour_passed = Math.round((d2.getTime() - d1.getTime()) / HOUR)
{% endhighlight %}
