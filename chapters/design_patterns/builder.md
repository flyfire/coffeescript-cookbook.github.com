---
layout: recipe
title: 建造者模式 Builder Pattern
chapter: Design Patterns
---
## 问题 Problem

你需要准备一个复杂的、由多个部分组成的对象，但是你希望生成多次，或者使用不同的配置生成。

You need to prepare a complicated, multi-part object, but you expect to do it more than once or with varying configurations.

## 方法 Solution

创建一个“建造者”来分装对象的创建过程。

Create a Builder to encapsulate the object production process.

[Todo.txt](http://todotxt.com)格式提供了一种高级的（仍然是纯文本）方法来管理todo列表。手动打出每一项很累也很容易出错，因而一个TodoTxtBuilder类可以避免这些麻烦：

The [Todo.txt](http://todotxt.com) format provides an advanced but still plain-text method for maintaining lists of to-do items.  Typing out each item by hand would provide exhausting and error-prone, however, so a TodoTxtBuilder class could save us the trouble:

{% highlight coffeescript %}
class TodoTxtBuilder
    constructor: (defaultParameters={ }) ->
        @date = new Date(defaultParameters.date) or new Date
        @contexts = defaultParameters.contexts or [ ]
        @projects = defaultParameters.projects or [ ]
        @priority =  defaultParameters.priority or undefined
    newTodo: (description, parameters={ }) ->
        date = (parameters.date and new Date(parameters.date)) or @date
        contexts = @contexts.concat(parameters.contexts or [ ])
        projects = @projects.concat(parameters.projects or [ ])
        priorityLevel = parameters.priority or @priority
        createdAt = [date.getFullYear(), date.getMonth()+1, date.getDate()].join("-")
        contextNames = ("@#{context}" for context in contexts when context).join(" ")
        projectNames = ("+#{project}" for project in projects when project).join(" ")
        priority = if priorityLevel then "(#{priorityLevel})" else ""
        todoParts = [priority, createdAt, description, contextNames, projectNames]
        (part for part in todoParts when part.length > 0).join " "

builder = new TodoTxtBuilder(date: "10/13/2011")

builder.newTodo "Wash laundry"

# => '2011-10-13 Wash laundry'

workBuilder = new TodoTxtBuilder(date: "10/13/2011", contexts: ["work"])

workBuilder.newTodo "Show the new design pattern to Lucy", contexts: ["desk", "xpSession"]

# => '2011-10-13 Show the new design pattern to Lucy @work @desk @xpSession'

workBuilder.newTodo "Remind Sean about the failing unit tests", contexts: ["meeting"], projects: ["compilerRefactor"], priority: 'A'

# => '(A) 2011-10-13 Remind Sean about the failing unit tests @work @meeting +compilerRefactor'

{% endhighlight %}

## 讨论 Discussion

TodoTxtBuilder包揽了所有繁重的生成文本的工作，使得程序员把注意力放在每个todo项不一样的部分。而且，可以把命令行工具或者GUI插入到这段代码中，and still retain support for later, more advanced versions of the format with ease.

The TodoTxtBuilder class takes care of all the heavy lifting of text generation and lets the programmer focus on the unique elements of each to-do item.  Additionally, a command line tool or GUI could plug into this code and still retain support for later, more advanced versions of the format with ease.

### 开工前 Pre-Construction

无需每次都重新创建一个需要对象的实例，我们可以组建转移到别的对象上，这样我们可以在随后的创建过程中擦改。
Instead of creating a new instance of the needed object from scratch every time, we shift the burden to a separate object that we can then tweak during the object creation process.

{% highlight coffeescript %}
builder = new TodoTxtBuilder(date: "10/13/2011")

builder.newTodo "Order new netbook"

# => '2011-10-13 Order new netbook'

builder.projects.push "summerVacation"

builder.newTodo "Buy suntan lotion"

# => '2011-10-13 Buy suntan lotion +summerVacation'

builder.contexts.push "phone"

builder.newTodo "Order tickets"

# => '2011-10-13 Order tickets @phone +summerVacation'

delete builder.contexts[0]

builder.newTodo "Fill gas tank"

# => '2011-10-13 Fill gas tank +summerVacation'
{% endhighlight %}

### 练习 Exercises

* 补充project-和context-tag产生器的代码，过滤掉重复的项；
* 有些Todo.txt的用户常常会把project和context标记插入到todo项的描述中，添加一些代码吧这些标识出这些记号，并把它们从后面的标记剔除掉。

* Expand the project- and context-tag generation code to filter out duplicate entries.
* Some Todo.txt users like to insert project and context tags inside the description of their to-do items.  Add code to identify these tags and filter them out of the end tags.
