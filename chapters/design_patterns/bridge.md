---
layout: recipe
title: 桥接模式
chapter: Design Patterns
---
## 问题

你需要维护一个可靠的接口，这个接口能够应对频繁的代码变化，或者能够在不同的实现间切换。

## 方法

在不同的实现或者是其他的代码中间，使用桥接模式作为媒介。

假设你开发了一个文本编辑器，把内容保存到云端。但是，现在你需要把它移植到一个单独的客户端，把内容保存在本地。

{% highlight coffeescript %}
class TextSaver
	constructor: (@filename, @options) ->
	save: (data) ->

class CloudSaver extends TextSaver
	constructor: (@filename, @options) ->
		super @filename, @options
	save: (data) ->
		# Assuming jQuery
		# Note the fat arrows
		$( =>
			$.post "#{@options.url}/#{@filename}", data, =>
				alert "Saved '#{data}' to #{@filename} at #{@options.url}."
		)

class FileSaver extends TextSaver
	constructor: (@filename, @options) ->
		super @filename, @options
		@fs = require 'fs'
	save: (data) ->
		@fs.writeFile @filename, data, (err) => # Note the fat arrow
			if err? then console.log err
			else console.log "Saved '#{data}' to #{@filename} in #{@options.directory}."

filename = "temp.txt"
data = "Example data"

saver = if window?
	new CloudSaver filename, url: 'http://localhost' # => Saved "Example data" to temp.txt at http://localhost
else if root?
	new FileSaver filename, directory: './' # => Saved "Example data" to temp.txt in ./

saver.save data
{% endhighlight %}

## 详解

桥接模式帮助你把特殊实现的代码从眼前移走，可以让你把注意力放在你程序中更重要的代码中。在上面的例子中，你程序的其他地方可以调用`saver.save data`，无需考虑文件最终会放到哪里。
