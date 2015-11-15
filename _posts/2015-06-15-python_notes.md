---
layout: post
title: "python拾遗"
description: 
modified: 2015-06-15
category: Python
tags: [Python]
comments: true
mathjax: 
---

python好久没玩过了，因为项目一直都是java，所以对python虽然喜欢，但是由于条件所致，用到的并不是很多。本次对于常见的小的Tips进行一些记录。

##获取本机的hostname

{% highlight python linenos %}
import socket
hostname = socket.gethostname()
print “%(hostname)s” %locals() 效果等于 “%s” %hostname
{% endhighlight %}

##str.zfill()

字符串填充，若括号内数字≤字符串长度，则返回原有字符串，否则在左侧补0.

##shutil:对文件或文件夹进行操作

{% highlight python linenos %}
shutil.copyfile（sour,dest）
shutil.rmtree(dir)
{% endhighlight %}

##日志处理：

{% highlight python linenos %}
import logging  
logging.basicConfig(filename = os.path.join(os.getcwd(), 'log.txt'), level = logging.DEBUG)  
logging.debug('this is a message')
{% endhighlight %}

##type()动态创建类：

{% highlight bash linenos %}
>>> def fn(self, name='world'): # 先定义函数
...     print('Hello, %s.' % name)
...
>>> Hello = type('Hello', (object,), dict(hello=fn)) # 创建Hello class
>>> h = Hello()
>>> h.hello()
Hello, world.
>>> print(type(Hello))
<type 'type'>
>>> print(type(h))
<class '__main__.Hello'>
{% endhighlight %}

要创建一个class对象，type()函数依次传入3个参数：

class的名称；
继承的父类集合，注意Python支持多重继承，如果只有一个父类，别忘了tuple的单元素写法；
class的方法名称与函数绑定，这里我们把函数fn绑定到方法名hello上。
通过type()函数创建的类和直接写class是完全一样的，因为Python解释器遇到class定义时，仅仅是扫描一下class定义的语法，然后调用type()函数创建出class。

正常情况下，我们都用class Xxx...来定义类，但是，type()函数也允许我们动态创建出类来，也就是说，动态语言本身支持运行期动态创建类，这和静态语言有非常大的不同，要在静态语言运行期创建类，必须构造源代码字符串再调用编译器，或者借助一些工具生成字节码实现，本质上都是动态编译，会非常复杂。

##执行命令行

* os.system(cmd):

该方法在调用完shell脚本后，返回一个16位的二进制数，低位为杀死所调用脚本的信号号码，高位为脚本的退出状态码，即脚本中“exit 1”的代码执行后，os.system函数返回值的高位数则是1，如果低位数是0的情况下，则函数的返回值是0×100,换算为10进制得到256。
如果我们需要获得os.system的正确返回值，那使用位移运算可以还原返回值：

{% highlight bash linenos %}
>>>  n = os.system(test.sh)
>>> n >> 8
>>> 3
{% endhighlight %}

* os.popen(cmd):

这种调用方式是通过管道的方式来实现，函数返回一个file-like的对象，里面的内容是脚本输出的内容（可简单理解为echo输出的内容）。使用os.popen调用test.sh的情况：
python调用Shell脚本，有两种方法：os.system(cmd)或os.popen(cmd),前者返回值是脚本的退出状态码，后者的返回值是脚本执行过程中的输出内容。实际使用时视需求情况而选择。
明显地，像调用”ls”这样的shell命令，应该使用popen的方法来获得内容。

* commands模块

可以很方便的取得命令的输出（包括标准和错误输出）和执行状态位

{% highlight bash linenos %}
import commands
a,b = commands.getstatusoutput('ls')
a是退出状态
b是输出的结果。
>>> import commands
>>> a,b = commands.getstatusoutput('ls')
>>> print a
0
>>> print b
anaconda-ks.cfg
install.log
install.log.syslog
{% endhighlight %}

* subprocess模块

使用subprocess模块可以创建新的进程，可以与新建进程的输入/输出/错误管道连通，并可以获得新建进程执行的返回状态。使用subprocess模块的目的是替代os.system()、os.popen*()、commands.*等旧的函数或模块。

{% highlight bash linenos %}
import subprocess
subprocess.call(command, shell=True)
#会直接打印出结果。
subprocess.Popen(command, shell=True) #也可以是subprocess.Popen(command, stdout=subprocess.PIPE, shell=True) 这样就可以输出结果了。
#如果command不是一个可执行文件，shell=True是不可省略的。
#shell=True意思是shell下执行command
{% endhighlight %}

