---
layout: post
title: "Python之logging模块"
description: 
modified: 2016-01-24
category: Python
tags: [Python]
comments: true
mathjax: 
---
{% highlight linenos %}
{% endhighlight %}

俗话说：没有bug的代码是不存在的，那么当代码出了问题，想要找出问题的时候，最有用的东西无过于一份完整的log，只要有程序运行的完整的log，一般都可以很快的定位到问题所在，然后将bug fix掉。Python的logging模块定义的函数和类为应用程序实现了一个灵活的事件日志系统。

下面主要介绍了三种常见的Python的日志模块的使用方法。

##一、直接引用logging模块

直接`import logging`，然后使用basicConfig定义一些参数。

**常用参数列表**

|格式|描述|
|:---:|:---:|
|filename|指定日志文件名|
|filemode|和file函数意义相同，指定日志文件的打开模式，'w'或'a'|
|format|指定输出的格式和内容，format可以输出很多有用信息|
|%(asctime)s|人类可读的LogRecord创建的时间。默认形式为‘2003-07-08 16:49:45,896’（逗号后面的数字表示时间的毫秒部分）。|
|%(created)s|LogRecord创建的时间|
|%(levelno)s| 打印日志级别的数值|
|%(levelname)s| 打印日志级别名称|
|%(pathname)s| 打印当前执行程序的路径，其实就是sys.argv[0]|
|%(filename)s| 打印当前执行程序名|
|%(funcName)s| 打印日志的当前函数|
|%(lineno)d| 打印日志的当前行号|
|%(thread)d| 打印线程ID|
|%(threadName)s| 打印线程名称|
|%(process)d| 打印进程ID|
|%(message)s| 打印日志信息|
|datefmt| 指定时间格式，同time.strftime()|
|level| 设置日志级别，默认为logging.WARNING|
|stream| 指定将日志的输出流，可以指定输出到sys.stderr,sys.stdout或者文件，默认输出到sys.stderr，当stream和filename同时指定时，stream被忽略|

示例：

{% highlight python linenos %}
import logging

logging.basicConfig(filename='app.log', format='%(asctime)s  %(levelname)s:%(message)s', level=logging.DEBUG, datefmt='%m/%d/%Y %I:%M:%S %p')
 logging.debug('Started')
  logging.debug('Ended')
{% endhighlight %}

##使用logger模块

使用logger模块可以自定义自己的日志系统，然后使用不同的hadler来设置不同的日志类型，在使用的时候视场景来使用不同的hadler以及格式，产生不同的输出。

**常用hadler类型**

|类型|描述|
|:--:|:--:|
|logging.StreamHandler|日志输出到流，可以是sys.stderr、sys.stdout或者文件|
|logging.FileHandler|日志输出到文件|
|logging.handlers.BaseRotatingHandler|日志回滚|
|logging.handlers.RotatingFileHandler|日志回滚|
|logging.handlers.TimedRotatingFileHandler|日志回滚|
|logging.handlers.SocketHandler |远程输出日志到TCP/IP sockets|
|logging.handlers.DatagramHandler |远程输出日志到UDP sockets|
|logging.handlers.SMTPHandler| 远程输出日志到邮件地址|
|logging.handlers.SysLogHandler| 日志输出到syslog |
|logging.handlers.NTEventLogHandler| 远程输出日志到Windows NT/2000/XP的事件日志 |
|logging.handlers.MemoryHandler| 日志输出到内存中的制定buffer|
|logging.handlers.HTTPHandler| 通过"GET"或"POST"远程输出到HTTP服务器|


示例：

{% highlight python linenos %}
import logging

#create logger
logger = logging.getLogger()
logger.setLevel(logging.DEBUG)
#create console handler and set level
ch = logging.StreamHandler()
ch.setLevel(logging.DEBUG)
#create formatter
formatter = logging.Formatter('%(asctime)s -  %(levelname)s - %(message)s')
ch.setFormatter(formatter)
logger.addHandler(ch)
logger.debug('debug message')
logger.info('info message')
logger.warn('warn message')
logger.error('error message')
logger.critical('critical message')
{% endhighlight %}

##使用logger+config文件

写logger，当配置很多的情况下，代码逻辑就会比较多，可读性就会降低，而且还不易于维护，可扩展性也会比较差，这时候就可以将配置单独拿出来，写在`logging.conf`文件中，然后在调用时直接使用其中某个配置、某个格式的logger的handler就好了。具体示例如下：


**logging.conf文件**

{% highlight bash linenos %}
[loggers]
keys=root,simpleExample,Example

[handlers]
keys=consoleHandler,fileHandler

[formatters]
keys=simpleFormatter
[logger_root]
level=DEBUG
handlers=consoleHandler

[logger_simpleExample]
level=DEBUG
handlers=consoleHandler
qualname=simpleExample
propagate=0

[logger_Example]
level=DEBUG
handlers=fileHandler
qualname=Example
propagate=0

[handler_consoleHandler]
class=StreamHandler
level=DEBUG
formatter=simpleFormatter
args=(sys.stdout,)

[handler_fileHandler]
class=FileHandler
level=DEBUG
formatter=simpleFormatter
args=('smplog.log', 'w')

[formatter_simpleFormatter]
format=%(asctime)s - %(name)s - %(levelname)s - %(message)s
datefmt=%m/%d/%Y %I:%M:%S %p
{% endhighlight %}

**示例代码**

{% highlight python linenos %}
import logging
import logging.config

logging.config.fileConfig('logging.conf')
logger = logging.getLogger('simpleExample')

logger.debug('debug message')
logger.info('info message')
logger.warn('warn message')
logger.error('error message')
logger.critical('critical message')
{% endhighlight %}

