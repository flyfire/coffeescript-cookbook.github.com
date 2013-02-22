---
layout: recipe
title: 工厂方法模式
chapter: Design Patterns
---
## 问题

只到运行时，你才知道你需要什么样的对象。

## 方法

使用[工厂方法模式](http://en.wikipedia.org/wiki/Factory_method_pattern)，动态地选择要生成的对象。

假设你需要加载文件到编辑器中，在用户选择文件之前，你无法知道文件的格式。一个使用[工厂方法模式](http://en.wikipedia.org/wiki/Factory_method_pattern)的类可以根据文件的扩展名来给出不一样的解析器。

{% highlight coffeescript %}
class HTMLParser
	constructor: ->
		@type = "HTML parser"
class MarkdownParser
	constructor: ->
		@type = "Markdown parser"
class JSONParser
	constructor: ->
		@type = "JSON parser"

class ParserFactory
	makeParser: (filename) ->
		matches = filename.match /\.(\w*)$/
		extension = matches[1]
		switch extension
			when "html" then new HTMLParser
			when "htm" then new HTMLParser
			when "markdown" then new MarkdownParser
			when "md" then new MarkdownParser
			when "json" then new JSONParser

factory = new ParserFactory

factory.makeParser("example.html").type # => "HTML parser"

factory.makeParser("example.md").type # => "Markdown parser"

factory.makeParser("example.json").type # => "JSON parser"
{% endhighlight %}

## 详解

在上例中，你可以忽略不用文件格式的特点，把注意力放在解析的内容。甚至还有更高级的工厂方法，例如，这些方法可以在文件中查找文件版本数据，返回更加精确的解析器（例如使用HTML5解析器代替HTML4解析器）。
