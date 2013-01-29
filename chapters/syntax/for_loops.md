---
layout: recipe
title: For循环
chapter: Syntax
---
## 问题

你需要使用一个for循环来迭代一个数组、对象或者值域。

## 方法

{% highlight coffeescript %}
# for(i = 1; i<= 10; i++)
x for x in [1..10]
# => [ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 ]

# To count by 2
# for(i=1; i<= 10; i=i+2)
x for x in [1..10] by 2
# => [ 1, 3, 5, 7, 9 ]

# Perform a simple operation like squaring each item.
x * x for x in [1..10]
# = > [1,4,9,16,25,36,49,64,81,100]
{% endhighlight %}

## 详解

在CoffeeScript中，可以使用列表解析（List Comprehensions）代替for循环，但最终编译为JavaScript时，还是传统的for循环。

