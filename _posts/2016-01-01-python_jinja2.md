---
layout: post
title: "python_Jinja2"
description: 
modified: 2016-01-01
category: Python
tags: [Python,Jinja2]
comments: true
mathjax: 
---
{% highlight linenos %}
{% endhighlight %}

##初识
前些日子看Flask，发现它用的模板是`Jinja2`，一时兴起，便翻起文档看了起来，发现语法和众多模板差不多，当然我只是粗略的玩了一下，发现`Jinja`的名字是源于`Jinja`是日本寺庙的名称， `Jinja2`与`Mako`的性能相近，比`Django`的模板引擎或`Genshi`快10到20倍，因为项目中尚未用到，所以也没有深入研究，现在粗浅的介绍下我所了解的。

特性：

* 沙箱中执行
* 强大的 HTML 自动转义系统保护系统免受 XSS
* 模板继承
* 及时编译最优的 python 代码
* 可选提前编译模板的时间
* 易于调试。异常的行数直接指向模板中的对应行。
* 可配置的语法

##安装

首先自然是安装咯，一般都有两种安装方法，

####直接通过`easy_install`或者`pip`安装：

{% highlight bash linenos %}
easy_install Jinja2
pip install Jinja2
{% endhighlight %}

####源码安装：

* 安装`git`

* 从[github地址][0]将源码下载下来

{% highlight bash linenos %}
git clone https://github.com/mitsuhiko/jinja2.git
{% endhighlight %}

* 然后`cd jinja2`
* 最后使用下面命令进行安装

{% highlight bash linenos %}
ln -s jinja2 /usr/lib/python2.X/site-packages
{% endhighlight %}

####通过tar包安装

* 在[下载页面][1]下载最新的tar包
* 解压缩
* 使用命令行`sudo python setup.py install`安装

##“Hello World”

现在安装完毕，那么先通过简单的Hello,World来使用下Jinja2吧，创建一个模板，然后渲染它，使它出现“Hello,World”。

{% highlight python linenos %}
>>> from jinja2 import Template
>>> template = Template('Hello {{ name }}!')
>>> template.render(name='World')
u'Hello World!'
{% endhighlight %}

##基本原理

`Jinja2`使用一个名为`Environment`的中心对象。这个类的实例用于存储配置、全局对象，并用于从文件系统或其它位置加载模板。即使你通过`:class:Template`类的构造函数用字符串创建模板，也会为你自动创建一个环境，尽管是共享的。

大多数应用在应用初始化时创建一个`Environment`对象，并用它加载模板。在某些情况下，如果使用多份配置，使用并列的多个环境无论如何是有用的。

配置 Jinja2 为你的应用加载文档的最简单方式看起来大概是这样:

{% highlight python linenos %}
from jinja2 import Environment, PackageLoader
env = Environment(loader=PackageLoader('yourapplication', 'templates'))
{% endhighlight %}

这会创建一个默认设定下的模板环境和一个在*yourapplication python*包中的*templates*文件夹中寻找模板的加载器。多个加载器是可用的,如果你需要从数据库或其它资源加载模板，你也可以自己写一个。

你只需要调用`get_template()`方法从这个环境中加载模板，并会返回已加载的`Template`:

{% highlight python linenos %}
template = env.get_template('mytemplate.html')
{% endhighlight %}

用若干变量来渲染它，调用`render()`方法:

{% highlight python linenos %}
print template.render(the='variables', go='here')
{% endhighlight %}

使用一个模板加载器，而不是向`Template`或`Environment.from_string()`传递字符串，有许多好处。除了使用上便利，也使得模板继承成为可能。

##基本语法

####\{\% ... \%\} 和 \{\{ ... \}\} 

前者用于执行诸如 for 循环 或赋值的语句，后者把表达式的结果打印到模板上。

####变量

你可以使用点`.`来访问变量的属性，作为替代，也可以使用所谓的“下标”语 法`[]`。下面的几行效果是一样的:

{% highlight python linenos %}
{{ foo.bar }}
{{ foo['bar'] }}
{% endhighlight %}

花括号 不是 变量的一部分，而是打印语句的一部分是重要的。如果你访问标签 里的不带括号的变量。如果变量或属性不存在，会返回一个未定义值。你可以对这类值做什么取决于应用的配 置，默认的行为是它如果被打印，其求值为一个空字符串，并且你可以迭代它，但其它 操作会失败。

####过滤器

过滤器与变量用管道符号`|`分割，并且也 可以用圆括号传递可选参数。多个过滤器可以链式调用，前一个过滤器的输出会被作为 后一个过滤器的输入。

{% highlight python linenos %}
{{ name|striptags|title }}
{{ list|join(', ') }}
{% endhighlight %}

* 第一个例子是移除 name 中的所有 HTML 标签并且改写为标题样式的大小写格式
* 第二个例子是把一个列表用逗号连接起来

####判断语句

判断一个变量或表达式，你要在变量后加上一个`is`以及要判断的名称。例如，要得出一个值是否定义过，你可以用 `name is defined`，这会根据`name`是否定义返回 true 或 false 。

栗子：

{% highlight python linenos %}
\{\% if loop.index is divisibleby 3 \%\}
\{\% if loop.index is divisibleby(3) \%\}
{% endhighlight %}

####注释

要把模板中一行的部分注释掉，默认使用`{# ... #}`注释语法。这在调试或 添加给你自己或其它模板设计者的信息时是有用的。


##其它模板

* [Mako][2]
* [Jade][3]
* [cheetah][4]

> 未完待续...

##参考

> * http://docs.jinkan.org/docs/jinja2/index.html
> * http://www.oschina.net/question/5189_3943









[0]:https://github.com/mitsuhiko/jinja2
[1]:http://pypi.python.org/pypi/Jinja2
[2]:http://www.makotemplates.org/
[3]:https://github.com/SyrusAkbary/pyjade
[4]:http://www.cheetahtemplate.org/docs/users_guide_html/