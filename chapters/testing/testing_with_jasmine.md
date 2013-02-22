---
layout: recipe
title: 使用Jasmine测试
chapter: Testing
---
## 问题

你使用CoffeeeScript写了一个简单的计算器，然后你想确认一下它的函数是否如你所想。你决定使用[Jasmine](http://pivotal.github.com/jasmine/)测试框架。

## 详解

使用Jasmine测试框架，把测试写在一个特殊的文件（spec）中，测试中描述了被测代码所期望的功能点。

例如，我们希望我们的计算机可以做加减法，并且能够正确地处理正负数。我们的测试用例如下。

{% highlight coffeescript %}

# calculatorSpec.coffee

describe 'Calculator', ->

	it 'can add two positive numbers', ->
		calculator = new Calculator()
		result = calculator.add 2, 3
		expect(result).toBe 5

	it 'can handle negative number addition', ->
		calculator = new Calculator()
		result = calculator.add -10, 5
		expect(result).toBe -5

	it 'can subtract two positive numbers', ->
		calculator = new Calculator()
		result = calculator.subtract 10, 6
		expect(result).toBe 4

	it 'can handle negative number subtraction', ->
		calculator = new Calculator()
		result = calculator.subtract 4, -6
		expect(result).toBe 10

{% endhighlight %}


### 配置Jasmine

想要运行你的测试用例，你必须下载Jasmine，并对其进行配置，具体步骤如下：

1. 下载最新的[Jasmine](http://pivotal.github.com/jasmine/download.html) zip文件；
2. 在你的项目中创建两个文件夹，spec和spec/jasmine；
3. 把下载好的Jasmine文件解压到spec/jasmine文件夹中；
4. 创建一个测试的runner。

### 创建Test Runner

Jasmine能够通过一个spec runner HTML文件在浏览器中运行你的测试用例。spec runner是一个简单的HTML页面，引用了一些必要的JavaScript和CSS文件，包括Jasmine和你的代码。示例如下。

{% highlight html linenos %}

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
  "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
  <title>Jasmine Spec Runner</title>
  <link rel="shortcut icon" type="image/png" href="spec/jasmine/jasmine_favicon.png">
  <link rel="stylesheet" type="text/css" href="spec/jasmine/jasmine.css">
  <script src="http://code.jquery.com/jquery.min.js"></script>
  <script src="spec/jasmine/jasmine.js"></script>
  <script src="spec/jasmine/jasmine-html.js"></script>
  <script src="spec/jasmine/jasmine-jquery-1.3.1.js"></script>

  <!-- include source files here... -->
  <script src="js/calculator.js"></script>

  <!-- include spec files here... -->
  <script src="spec/calculatorSpec.js"></script>

</head>

<body>
  <script type="text/javascript">
    (function() {
      var jasmineEnv = jasmine.getEnv();
      jasmineEnv.updateInterval = 1000;

      var trivialReporter = new jasmine.TrivialReporter();

      jasmineEnv.addReporter(trivialReporter);

      jasmineEnv.specFilter = function(spec) {
        return trivialReporter.specFilter(spec);
      };

      var currentWindowOnload = window.onload;

      window.onload = function() {
        if (currentWindowOnload) {
          currentWindowOnload();
        }
        execJasmine();
      };

      function execJasmine() {
        jasmineEnv.execute();
      }

    })();
  </script>
</body>
</html>

{% endhighlight %}

可以从[gist](https://gist.github.com/2623232)上下载这个spec runner。

SpecRunner.html使用起来也很简单，引入jasmine.js和相关的依赖，然后接着引入编译好的JavaScript文件和测试用例即可。

在上例中，我们引入了有待开发的calculator.js文件（14行）以及编译好的calculatorSpec.js文件（17行）。

## 运行测试用例

只需在浏览器中打开SpecRunner.js就能运行我们的测试用例。在我们的例子中，我们会看到4个失败的spec，总共包含8个没有通过的测试用例（如下）。

<img src="images/jasmine_failing_all.jpg" alt="All failing tests" />

看起来我们的测试用例没有通过是因为Jasmine无法找到Calculator变量。因为它就没被创建出来过。我们现在来创建，我们创建一个名为js/calculator.coffee的文件。

{% highlight coffeescript %}

# calculator.coffee

window.Calculator = class Calculator

{% endhighlight %}

编译calculator.coffee，然后刷新浏览器，重新运行测试用例。

<img src="images/jasmine_failing_better.jpg" alt="Still failing, but better" />

现在，未通过的测试用例从原来的8个变成了现在的4个。只增加了一行代码，就有50%的提升。

## 让测试都通过

让我们把方法都实现出来，看看这些测试用例能够通过么？

{% highlight coffeescript %}

# calculator.coffee

window.Calculator = class Calculator
	add: (a, b) ->
		a + b

	subtract: (a, b) ->
		a - b 

{% endhighlight %}

刷新后，我们看到所有的测试用例都通过了。

<img src="images/jasmine_passing.jpg" alt="All passing" />


## 重构测试用例

现在我们的测试通过了，我们应该检查一下我们的代码或者测试用例是否可以重构一下。

在我们的spec文件中，每个测试用例都会创建它自己的calculator实例。这会让我们的测试用例过于重复，对于较大的测试用例尤其如此。理想情况下，我们应当考虑把那些初始化的代码移进每个测试运行之前都需例行运行的代码中。

恰好Jasmine有一个名为beforeEach的函数可以实现这种需求。

{% highlight coffeescript %}

describe 'Calculator', ->
	calculator = null

	beforeEach ->
		calculator = new Calculator()

	it 'can add two positive numbers', ->
		result = calculator.add 2, 3
		expect(result).toBe 5

	it 'can handle negative number addition', ->
		result = calculator.add -10, 5
		expect(result).toBe -5

	it 'can subtract two positive numbers', ->
		result = calculator.subtract 10, 6
		expect(result).toBe 4

	it 'can handle negative number subtraction', ->
		result = calculator.subtract 4, -6
		expect(result).toBe 10

{% endhighlight %}

当我们重新编译我们的spec刷新浏览器之后，测试还是都通过了。

<img src="images/jasmine_passing.jpg" alt="All passing" />

