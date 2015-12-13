---
layout: post
title: "python环境管理pythonbrew和virtualenv"
description: 
modified: 2015-12-03
category: Python
tags: [Python]
comments: true
mathjax: 
---
{% highlight linenos %}
{% endhighlight %}

Python作为一门编程语言是很赞的，但是它的两个流行的版本，2.x和3.x却在有些时候让我颇为伤脑筋，毕竟2.x和3.x的差别还是蛮大的，具体参见[What’s New In Python 3.0][0]，而在平时玩的项目中，有时候会使用python2而有时候会使用python3，那么切换的问题就比较伤脑筋了。而pythonbrew作为一个很赞的python环境管理工具很好的解决了这个问题。

那基本的python环境的问题算是解决了，但是在你开发项目的时候，你会发现，有时你不得不安装一些包或是其他的什么来打造你的项目环境，但是项目玩结后这些环境就不需要了，久而久之，你的电脑将会有无数的不需要的东西，将你的生产环境搞的乱七八糟，就在此时，`virtualenv`出现了，它可以打造一个干净的虚拟环境，可以在上面安装多各种项目需要的东西，而在项目完成后，就可以将其清除。相当好用，不是么?

##[pythonbrew][1]


####安装


{% highlight bash linenos %}
curl -kL http://xrl.us/pythonbrewinstall | bash
{% endhighlight %}

然后将下面的加到`~/.bashrc`或者`~/.zshrc`中：


{% highlight bash linenos %}
[[ -s $HOME/.pythonbrew/etc/bashrc ]] && source $HOME/.pythonbrew/etc/bashrc
{% endhighlight %}

####常用指令

{% highlight bash linenos %}

pythonbrew list
pythonbrew list -k
pythonbrew install 2.7.2
pythonbrew uninstall 2.7.2
pythonbrew switch 2.7.2  #长久使用
pythonbrew use 2.7.2  #仅在当前bash使用
pythonbrew cleanup

{% endhighlight %}

##[virtualenv][2]

####安装

{% highlight bash linenos %}
$ pip install virtualenv
 pip install virtualenvwrapper
{% endhighlight %}

第一个包提供了创建虚拟环境的基础.如需更多细节可以参看Jamie Matthews的a non-magical introduction to Pip and Virtualenv for Python Beginners.如果仅仅使用virtualenv将会变得很复杂.安装的第二个包就是使这个过程简化

启动脚本，建议放到`~/.bashrc`或者`~/.zshrc`中：

{% highlight bash linenos %}
 source virtualenvwrapper.sh
{% endhighlight %}

####常用指令

创建虚拟环境

` mkvirtualenv myenv`

列出虚拟环境

`lsvirtualenv`

激活并使用虚拟环境

`workon myenv`

接下来你就可以在你的虚拟环境安装各种东西了(仅限当前虚拟env有效)，而这不会对你的电脑环境造成任何影响。

[0]:https://docs.python.org/3/whatsnew/3.0.html
[1]:https://github.com/utahta/pythonbrew
[2]:https://github.com/pypa/virtualenv