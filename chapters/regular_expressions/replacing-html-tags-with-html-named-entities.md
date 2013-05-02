---
layout: recipe
title: 使用HTML字符实体替换HTML标签 Replacing HTML Tags with HTML Named Entities
chapter: Regular Expressions
---
## 问题 Problem

你需要使用字符实体来替换HTML标签：

You need to replace HTML tags with named entities:

`<br/> => &lt;br/&gt;`

## 办法 Solution

{% highlight coffeescript %}
htmlEncode = (str) ->
  str.replace /[&<>"']/g, ($0) ->
    "&" + {"&":"amp", "<":"lt", ">":"gt", '"':"quot", "'":"#39"}[$0] + ";"

htmlEncode('<a href="http://bn.com">Barnes & Noble</a>')
# => '&lt;a href=&quot;http://bn.com&quot;&gt;Barnes &amp; Noble&lt;/a&gt;'
{% endhighlight %}

## 讨论 Discussion

可能还有比上面方法更好的实现。

There are probably better ways to implement the above method.
