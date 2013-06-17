---
layout: recipe
title: 策略模式 Strategy Pattern
chapter: Design Patterns
---
## 问题 Problem

你很多方法用来解决某个问题，但是需要在运行时选择或者切换这些方法。

You have more than one way to solve a problem but you need to choose (or even switch) between these methods at run time.

## 方法 Solution

将你的算法分装到策略对象中。

Encapsulate your algorithms inside of Strategy objects.

例如，给定一个无序列表，我们可以在不同的情景中选择不同的算法。

Given an unsorted list, for example, we can change the sorting algorithm under different circumstances.

### 基类 The base class:

{% highlight coffeescript %}
class StringSorter
    constructor:(@algorithm)->    
    sort: (list) -> @algorithm list
{% endhighlight %}

### 策略 The strategies:

{% highlight coffeescript %}
bubbleSort = (list) ->
    anySwaps = false
    swapPass = ->
        for r in [0..list.length-2]
            if list[r] > list[r+1]
                anySwaps = true
                [list[r], list[r+1]] = [list[r+1], list[r]]

    swapPass()
    while anySwaps
        anySwaps = false
        swapPass()
    list

reverseBubbleSort = (list) ->
    anySwaps = false
    swapPass = ->
        for r in [list.length-1..1]
            if list[r] < list[r-1]
                anySwaps = true
                [list[r], list[r-1]] = [list[r-1], list[r]]

    swapPass()
    while anySwaps
        anySwaps = false
        swapPass()
    list
{% endhighlight %}

### 使用策略 Using the strategies:

{% highlight coffeescript %}
sorter = new StringSorter bubbleSort

unsortedList = ['e', 'b', 'd', 'c', 'x', 'a']

sorter.sort unsortedList

# => ['a', 'b', 'c', 'd', 'e', 'x']

unsortedList.push 'w'

# => ['a', 'b', 'c', 'd', 'e', 'x', 'w']

sorter.algorithm = reverseBubbleSort

sorter.sort unsortedList

# => ['a', 'b', 'c', 'd', 'e', 'w', 'x']
{% endhighlight %}

## 讨论 Discussion

“遇到敌人后一切战斗计划都失效了”，用户也同样，但是我们可以使用已掌握的知识来适应环境。例如，在上面的例子中，新加项打乱了兴趣。了解细节，针对确切的场景，我们可以通过切换算法来加快排序，仅仅只需一次简单的再赋值即可。

"No plan survives first contact with the enemy", nor users, but we can use the knowledge gained from changing circumstances to adapt.  Near the end of the example, for instance, the newest item in the array now lies out of order.  Knowing that detail, we can then speed the sort up by switching to an algorithm optimized for that exact scenario with nothing but a simple reassignment.

### 练习 Exercises

* 把`StringSorter`扩展成一个`AlwaysSortedArray`类，实现原生数组的所有功能，而且有通过插入方法新项时能够自动地排序（例如，`push`和`shift`）。
 
* Expand `StringSorter` into an `AlwaysSortedArray` class that implements all of the functionality of a regular array but which automatically sorts new items based on the method of insertion (e.g. `push` vs. `shift`).
