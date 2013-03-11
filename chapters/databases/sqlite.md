---
layout: recipe
title: SQLite
chapter: Databases
---

## 问题

你想在Node.js中访问[SQLite](http://www.sqlite.org/)数据库。

## 方法

使用[SQLite module](http://code.google.com/p/node-sqlite/)。

{% highlight coffeescript %}
sqlite = require 'sqlite'

db = new sqlite.Database

# The module uses asynchronous methods,
# so we chain the calls the db.execute
exampleCreate = ->
	db.execute "CREATE TABLE snacks (name TEXT(25), flavor TEXT(25))",
		(exeErr, rows) ->
			throw exeErr if exeErr
			exampleInsert()

exampleInsert = ->
	db.execute "INSERT INTO snacks (name, flavor) VALUES ($name, $flavor)",
		{ $name: "Potato Chips", $flavor: "BBQ" },
		(exeErr, rows) ->
			throw exeErr if exeErr
			exampleSelect()

exampleSelect = ->
	db.execute "SELECT name, flavor FROM snacks",
		(exeErr, rows) ->
			throw exeErr if exeErr
			console.log rows[0] # => { name: 'Potato Chips', flavor: 'BBQ' }

# :memory: creates a DB in RAM
# You can supply a filepath (like './example.sqlite') to create/open one on disk
db.open ":memory:", (openErr) ->
	throw openErr if openErr
	exampleCreate()
{% endhighlight %}

## 讨论

你也可以事先准备好SQL语句：


{% highlight coffeescript %}
sqlite = require 'sqlite'
async = require 'async' # Not required but added to make the example more concise

db = new sqlite.Database

createSQL = "CREATE TABLE drinks (name TEXT(25), price NUM)"

insertSQL = "INSERT INTO drinks (name, price) VALUES (?, ?)"

selectSQL = "SELECT name, price FROM drinks WHERE price < ?"

create = (onFinish) ->
	db.execute createSQL, (exeErr) ->
		throw exeErr if exeErr
		onFinish()
	
prepareInsert = (name, price, onFinish) ->
	db.prepare insertSQL, (prepErr, statement) ->
		statement.bindArray [name, price], (bindErr) ->
			statement.fetchAll (fetchErr, rows) -> # Called so that it executes the insert
				onFinish()

prepareSelect = (onFinish) ->
	db.prepare selectSQL, (prepErr, statement) ->
		statement.bindArray [1.00], (bindErr) ->
			statement.fetchAll (fetchErr, rows) ->
				console.log rows[0] # => { name: "Mia's Root Beer", price: 0.75 }
				onFinish()

db.open ":memory:", (openErr) ->
	async.series([
		(onFinish) -> create onFinish,
		(onFinish) -> prepareInsert "LunaSqueeze", 7.95, onFinish,
		(onFinish) -> prepareInsert "Viking Sparkling Grog", 4.00, onFinish,
		(onFinish) -> prepareInsert "Mia's Root Beer", 0.75, onFinish,
		(onFinish) -> prepareSelect onFinish
	])
{% endhighlight %}

[SQLite版的SQL](http://www.sqlite.org/lang.html)和[node-sqlite模块文档](https://github.com/orlandov/node-sqlite#readme)提供了更完整的信息。

