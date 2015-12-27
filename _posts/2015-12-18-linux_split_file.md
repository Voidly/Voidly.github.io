---
layout: post
title: "linux split command"
description: 
modified: 2015-12-18
category: Linux
tags: [Linux, Split]
comments: true
mathjax: 
---
{% highlight linenos %}
{% endhighlight %}

前一阶段遇到了一个需求，是将一个大文件切割问多个小文件，本想用python的readlines()来做，但是听说Linux可能有简单的方法，于是便调研了下，最后找到了split的这个命令，留此备忘。

##有关split

是一个将文件进行分割的命令，可以将一个文件切分为多个更小的文件。

##语法

{% highlight bash linenos %}
split [OPTION]... [INPUT [PREFIX]]
{% endhighlight %}

##描述

split将文件切分为指定大小的块，文件名默认以`前缀aa`、`前缀ab`、、、命名

##选项

| 选项        | 含义           |
| ------------- |:-------------:|
| -a N, --suffix-length=N      | 后缀的长度，默认为2 |
| -b SIZE, --bytes=SIZE      | 每个输出文件的大小     |
| -C SIZE, --line-bytes=SIZE | 按行切割的文件大小      |
| -d, --numeric-suffixes | 使用数字作为后缀，而不是字母      |
| -e, --elide-empty-files | 省略空文件      |
| -l NUMBER, --lines=NUMBER | 按行分割      |
| --verbose | 输出详细信息      |
| --help | 帮助项     |
| --version | 版本      |

大小可以使用以下选项

| 后缀       | 乘数           |
| ------------- |:-------------:|
|   KB   | 1000 |
|   K   | 1024 |
|   MB   | 1000x1000 |
|   M   | 1024x1024 |

除此以外还有G、T、P...


##栗子

{% highlight bash linenos %}
split -l 300 file.txt new
{% endhighlight %}
