---
layout: recipe
title: 装饰者模式
chapter: Design Patterns
---
## 问题

你有一组数据，你需要对它们进行多次不同可处理。

## 方法

使用装饰者模式来构建你的处理方式。

{% highlight coffeescript %}
miniMarkdown = (line) ->
    if match = line.match /^(#+)\s*(.*)$/
        headerLevel = match[1].length
        headerText = match[2]
        "<h#{headerLevel}>#{headerText}</h#{headerLevel}>"
    else
        if line.length > 0
            "<p>#{line}</p>"
        else
            ''

stripComments = (line) ->
    line.replace /\s*\/\/.*$/, '' # Removes one-line, double-slash C-style comments

TextProcessor = (@processors) ->
    reducer: (existing, processor) ->
        if processor
            processor(existing or '')
        else
            existing
    processLine: (text) ->
        @processors.reduce @reducer, text
    processString: (text) ->
        (@processLine(line) for line in text.split("\n")).join("\n")

exampleText = '''
              # A level 1 header
              A regular line
              // a comment
              ## A level 2 header
              A line // with a comment
              '''

processor = new TextProcessor [stripComments, miniMarkdown]

processor.processString exampleText

# => "<h1>A level 1 header</h1>\n<p>A regular line</p>\n\n<h2>A level 2 header</h2>\n<p>A line</p>"
{% endhighlight %}

### 结果

{% highlight html %}
<h1>A level 1 header</h1>
<p>A regular line</p>

<h2>A level 1 header</h2>
<p>A line</p>
{% endhighlight %}

## 详解

TextProcessor正在这里就是一个装饰者的角色，把独立的不同的文本处理方式放到了一起。miniMarkdown和stripComments这两个组件被解放出来，专注于处理单行文本。其他开发者只需要编写这样的函数，返回一个字符串，并且把这个函数添加到处理器组中。

我们甚至可以随时修改现有的装饰者：

{% highlight coffeescript %}
smilies =
    ':)' : "smile"
    ':D' : "huge_grin"
    ':(' : "frown"
    ';)' : "wink"

smilieExpander = (line) ->
    if line
        (line = line.replace symbol, "<img src='#{text}.png' alt='#{text}' />") for symbol, text of smilies
    line

processor.processors.unshift smilieExpander

processor.processString "# A header that makes you :) // you may even laugh"

# => "<h1>A header that makes you <img src='smile.png' alt='smile' /></h1>"

processor.processors.shift()

# => "<h1>A header that makes you :)</h1>"
{% endhighlight %}

