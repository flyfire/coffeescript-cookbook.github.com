---
layout: recipe
title: 链式调用
chapter: Classes and Objects
---
## 问题

多次调用同一个对象的方法时，你希望可以可以不用每次都引用这个对象。

## 方法

在每一个需要链式调用的方法最后返回`this`（即`@`）。

{% highlight coffeescript %}
class CoffeeCup
	properties:
		strength: 'medium'
		cream: false
		sugar: false
	strength: (newStrength) ->
		@properties.strength = newStrength
		@
	cream: (newCream) ->
		@properties.cream = newCream
		@
	sugar: (newSugar) ->
		@properties.sugar = newSugar
		@

morningCup = new CoffeeCup()

morningCup.properties # => { strength: 'medium', cream: false, sugar: false }

eveningCup = new CoffeeCup().strength('dark').cream(true).sugar(true)

eveningCup.properties # => { strength: 'dark', cream: true, sugar: true }

{% endhighlight %}

## 详解

jQuery类库使用的方式也类似，每一个相关的方法都会返回一个选择器对象（即jQuery对象），通过调整选择来修改后续的方法。

{% highlight coffeescript %}
$('p').filter('.topic').first()
{% endhighlight %}

对于你自己的对象，使用点元编程的手法，就能够自动化地实现创建过程，明确地表明返回`this`的意图。

{% highlight coffeescript %}
addChainedAttributeAccessor = (obj, propertyAttr, attr) ->
	obj[attr] = (newValues...) ->
		if newValues.length == 0
			obj[propertyAttr][attr]
		else
			obj[propertyAttr][attr] = newValues[0]
			obj

class TeaCup
	properties:
		size: 'medium'
		type: 'black'
		sugar: false
		cream: false

addChainedAttributeAccessor(TeaCup.prototype, 'properties', attr) for attr of TeaCup.prototype.properties

earlgrey = new TeaCup().size('small').type('Earl Grey').sugar('false')

earlgrey.properties # => { size: 'small', type: 'Earl Grey', sugar: false }

earlgrey.sugar true

earlgrey.sugar() # => true
{% endhighlight %}
