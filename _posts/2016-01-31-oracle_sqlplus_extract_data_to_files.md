---
layout: post
title: "oracle将数据导出至文件"
description: 
modified: 2016-01-31
category: 数据库
tags: [Oracle]
comments: true
mathjax: 
---
{% highlight linenos %}
{% endhighlight %}

近期有个需求需要将oracle的数据导入进一个文件中，查了一些资料，发现可以使用sqlplus命令行按照以下几步来实现这个功能：

* 编写sqlplus的命令行脚本
* 将执行脚本重定向到你想导出的文件中

然而，除此之外还有一种更好用的导出方式:sqluldr。


##sqlplus导出数据至文件


脚本如下：

{% highlight bash linenos %}
#!/bin/bash

sqlplus64 -s user/passwd@//host:port/SID << EOF
set heading off
set feedback off
set linesize 999
set newpage none
set pagesize 0
SELECT *  FROM 表名
EOF
{% endhighlight %}

然后执行脚本文件，并将内容重定向即可获取数据文件。

可以看到在脚本文件中有一些设置，这里简单介绍一下各个含义：

| 参数        | 含义          |
| ------------- |:-------------:|
| set echo on/off | 是否显示脚本中的需要执行的命令  |
| set feedback on/off        | 是否显示select结果之后返回多少行的提示      |
| set heading on/off | 是否显示查询结果的列名，如果设置为off，将用空白行代替  |
| set pagesize 0 | 去除该空白行 |
| set linesize n     | 设置一行最多显示多少字符 |
| set newpage none | 设置查询出来的数据分多少页显示，如果需要连续的数据，中间不要出现空行就把newp设置为none |


更多可参考[官网][0]

##使用sqluldr导出数据至文件

首先，需要下载sqluldr工具，下载链接[在此][1]。将适合版本的工具放到你自己的`bin`目录下，然后即可命令行使用了。

命令示例：

{% highlight bash linenos %}
sqluldr2_linux64_10204.bin 
user=username/password@tnsname 
query="select * from 表名" 
field='0x09'  
file=test.txt
{% endhighlight %}

下面对于各个字段含义做一下说明：

| 参数        | 含义          |
| ------------- |:-------------:|
| user |  username/password@tnsname |
| query | 查询语句 |
| fields | 列间的分隔符 |
| record | 行间的分隔符 |
| file | 输出文件 |

操作相当的简单，而且导出效率非常之高。两千万行的数据，秒级完成。

另，此间分隔符的表示方法与一般的不同，采用16进制表示，具体对应如下：

| 符号       | 含义          |
| ------------- |:-------------:|
| 0x0d | \r |
| 0x0a | \n |
| 0x7c | | |
| 0x2c | , |
|  0x09 | \t |
| 0x3a | : |
| 0x23 | # |
| 0x22 | " |
| 0x27 | ' |





[0]:http://ss64.com/ora/syntax-sqlplus-set.html
[1]:http://www.anysql.net/software/sqluldr.zip