---
layout: recipe
title: MongoDB
chapter: Databases
---

## 问题

你想与MongoDB数据库进行交互。

## Solution
## 方法

### 使用Node.js

### 安装

* 如果你的电脑中还没有的话请先[安装MongoDB](http://www.mongodb.org/display/DOCS/Quickstart) 。

* [安装MongoDB原生的模块](https://github.com/christkv/node-mongodb-native)。

#### 保存记录

{% highlight coffeescript %}
mongo = require 'mongodb'

server = new mongo.Server "127.0.0.1", 27017, {}

client = new mongo.Db 'test', server

# save() updates existing records or inserts new ones as needed
exampleSave = (dbErr, collection) ->
	console.log "Unable to access database: #{dbErr}" if dbErr
	collection.save { _id: "my_favorite_latte", flavor: "honeysuckle" }, (err, docs) ->
		console.log "Unable to save record: #{err}" if err
		client.close()

client.open (err, database) ->
	client.collection 'coffeescript_example', exampleSave
{% endhighlight %}

#### 查找记录

{% highlight coffeescript %}
mongo = require 'mongodb'

server = new mongo.Server "127.0.0.1", 27017, {}

client = new mongo.Db 'test', server

exampleFind = (dbErr, collection) ->
	console.log "Unable to access database: #{dbErr}" if dbErr
	collection.find({ _id: "my_favorite_latte" }).nextObject (err, result) ->
		if err
			console.log "Unable to find record: #{err}"
		else
			console.log result # => {  id: "my_favorite_latte", flavor: "honeysuckle" }
		client.close()

client.open (err, database) ->
	client.collection 'coffeescript_example', exampleFind
{% endhighlight %}

### 浏览器端

一个[基于REST的接口](https://github.com/tdegrunt/mongodb-rest)为此而生。它提供了基于AJAX的访问方式。


## 讨论

本菜谱把*save*和*find*分成了两个示例，目的是为了把MongoDB独有的链接任务和回调管理方面的担忧分离。[async](https://github.com/caolan/async)模块可以帮上忙。
