---
layout:   recipe
title:    字符串匹配
chapter:  Strings
---
## 问题

你想对两个或多个字符串进行匹配。

## 方法

计算编辑距离，即计算把某个字符串转为另外一个字符串需要操作的次数。

{% highlight coffeescript %}

Levenshtein =
  (str1, str2) ->
         
    l1 = str1.length
    l2 = str2.length

    Math.max l1, l2 if Math.min l1, l2 == 0      

    i = 0; j = 0; distance = []

    for i in [0...l1 + 1]
     distance[i] = []
     distance[i][0] = i

    distance[0][j] = j for j in [0...l2 + 1]

    for i in [1...l1 + 1]
     for j in [1...l2 + 1]
       distance[i][j] = Math.min distance[i - 1][j] + 1,
         distance[i][j - 1] + 1,                         
         distance[i - 1][j - 1] + 
           if (str1.charAt i - 1) == (str2.charAt j - 1) then 0 else 1

    distance[l1][l2]
    
{% endhighlight %}

## 详解

你可以使用Hirschberg或者Wagner-Fischer的算法来计算编辑距离。上例使用的是Wagner-Fischer算法。
