---
layout: post
title: "Tips:screen中kill掉多个会话"
description: 
modified: 2015-11-05
category: 
tags: []
comments: true
mathjax: 
---
{% highlight linenos %}
{% endhighlight %}

前些天用screen的时候在连接的时候发现平时忘了关screen的会话，导致出现了很长的会话列表，看着很不舒服，于是想要将其关掉，用常规和非常规方法关了两个之后，突然想到现在的方法好low，于是开始找一些比较trick的方法。现将关掉会话的方法总结一下。

###方法一：

{% highlight bash linenos %}
screen -ls
screen -r SessionID
Ctrl+A
:quit
{% endhighlight %}

###方法二：

{% highlight bash linenos %}
Ctrl+D
{% endhighlight %}

###方法三


{% highlight bash linenos %}
screen -ls
screen -S SessionID -X quit
{% endhighlight %}

###方法四


{% highlight bash linenos %}
screen -ls
kill SessionID
{% endhighlight %}

###方法五

{% highlight bash linenos %}
screen -ls | grep Detached | cut -d. -f1 | awk '{print $1}'| xargs kill
{% endhighlight %}

###方法六

{% highlight bash linenos %}
screen -ls | grep Detached | cut -d. -f1 | tr -d [:blank:] | xargs kill
{% endhighlight %}