---
layout: recipe
title: 参数数组化 Splat Arguments
chapter: Functions
---
## 问题 Problem

你的函数被调用时会接收到个数不一的参数。

Your function will be called with a varying number of arguments.

## 方法 Solution

使用_参数列_。

Use _splats_.

{% highlight coffeescript %}
loadTruck = (firstDibs, secondDibs, tooSlow...) ->
	truck:
		driversSeat: firstDibs
		passengerSeat: secondDibs
		trunkBed: tooSlow

loadTruck("Amanda", "Joel")
# => { truck: { driversSeat: "Amanda", passengerSeat: "Joel", trunkBed: [] } }

loadTruck("Amanda", "Joel", "Bob", "Mary", "Phillip")
# => { truck: { driversSeat: "Amanda", passengerSeat: "Joel", trunkBed: ["Bob", "Mary", "Phillip"] } }
{% endhighlight %}

末尾有一个单独的参数：

With a trailing argument:

{% highlight coffeescript %}
loadTruck = (firstDibs, secondDibs, tooSlow..., leftAtHome) ->
	truck:
		driversSeat: firstDibs
		passengerSeat: secondDibs
		trunkBed: tooSlow
	taxi:
		passengerSeat: leftAtHome

loadTruck("Amanda", "Joel", "Bob", "Mary", "Phillip", "Austin")
# => { truck: { driversSeat: 'Amanda', passengerSeat: 'Joel', trunkBed: [ 'Bob', 'Mary', 'Phillip' ] }, taxi: { passengerSeat: 'Austin' } }

loadTruck("Amanda")
# => { truck: { driversSeat: "Amanda", passengerSeat: undefined, trunkBed: [] }, taxi: undefined }
{% endhighlight %}

## 讨论 Discussion

通过给函数的参数（最多一个）后面添加一个省略号（`...`），CoffeeScript会把所有没有被其他具名参数捕获的参数值合并到一个列表中。就算有的具名参数都没有对应的值，CoffeeScript也会提供一个空的列表。

By adding an ellipsis (`...`) next to no more than one of a function's arguments, CoffeeScript will combine all of the argument values not captured by other named arguments into a list.  It will serve up an empty list even if some of the named arguments were not supplied.
