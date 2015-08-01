---
layout: post
title: "Linux分屏利器之screen小记"
description: 
modified: 2015-08-01 
category: Linux
tags: [Linux]
comments: true
mathjax: 
---

{% highlight linenos %}
{% endhighlight %}

话说你是不是经常需要`SSH`或者`telent`远程登录到Linux服务器？你是不是经常为一些长时间运行的任务而头疼，比如系统备份、ftp 传输等等。通常情况下我们都是为每一个这样的任务开一个远程终端窗口，因为他们执行的时间太长了。必须等待它执行完毕，在此期间可不能关掉窗口或者断开连接，否则这个任务就会被杀掉，一切半途而废了。

##为嘛关掉窗口正在运行的程序会挂掉？

在Linux/Unix中，有这样几个概念：

* 进程组（process group）：一个或多个进程的集合，每一个进程组有唯一一个进程组ID，即进程组长进程的ID。
* 会话期（session）：一个或多个进程组的集合，有唯一一个会话期首进程（session leader）。会话期ID为首进程的ID。
* 会话期可以有一个单独的控制终端（controlling terminal）。与控制终端连接的会话期首进程叫做控制进程（controlling process）。当前与终端交互的进程称为前台进程组。其余进程组称为后台进程组。

根据POSIX.1定义：

* 挂断信号（SIGHUP）默认的动作是终止程序。
* 当终端接口检测到网络连接断开，将挂断信号发送给控制进程（会话期首进程）。
* 如果会话期首进程终止，则该信号发送到该会话期前台进程组。
* 一个进程退出导致一个孤儿进程组中产生时，如果任意一个孤儿进程组进程处于STOP状态，发送SIGHUP和SIGCONT信号到该进程组中所有进程。
因此当网络断开或终端窗口关闭后，控制进程收到SIGHUP信号退出，会导致该会话期内其他进程退出。

举个简单的栗子，你打开一个终端运行一个指令会，用`ps`可以看到这个指令的进程ID以及父进程，但是你关掉这个窗口，另开一个，在这个上面找刚刚指令的进程ID就会发现已然找不到，因为它已经被kill掉了，当然使用`nohup`可以使其在后台运行，可是这个方法也太挫了，不是么？

曾经我有介绍过一个好用的软件，名为tmux，不了解的朋友可以看下[Linux分屏利器tmux小记][1],虽然同为姊妹，但是我更喜欢用tmux，为什么呢？原因请戳[这里][2]和[这里][3],那么既然我这么推崇tmux，为毛还要介绍screen!这位同学，你的问题问的很好！因为如果你在公司的服务器上进行一些操作，例如测试、看log、一些运维操作的时候，你会发现你没有权限进行安装软件，没错，tmux没有内置在Linux系统上，而这时候，screen就派上用场了，因为它是内置在Linux系统内部的一个分屏利器，虽然没有tmux强大和易用，但是处理一些简单的问题还是蛮方便的，而且学习成本也不高，俗话说：技多不压身。啰嗦了那么一堆，下面开始使用撒。

##screen简单使用

直接在终端输入`screen`命令即可使用。现在尝试下screen保持会话的功能。

	screen vi hello.py

好了，现在假设你需要给孩子换尿布，然后你按下`C-a d`（PS：看过我tmux博文或者玩过的人知道C-a就是Ctrl+a的意思），Screen会给出detached提示：
	
	~ » screen vi hello.py
	[detached]

20分钟后你换完尿布回来了，查看screen的会话：
	
	~ » screen -ls                                                                                       
	There is a screen on:
	32634.ttys000.YdeMacBook-Pro	(Detached)
	1 Socket in /var/folders/tt/cspymn6s4yb91yx5c0b4s02c0000gn/T/.screen.

然后你重新连接该会话：

	screen -r 32634

你会发现还是原来的状态！Just go on!

##screen快捷键

你可能注意到给screen发送命令使用了特殊的键组合`C-a`。这是因为我们在键盘上键入的信息是直接发送给当前screen窗口，必须用其他方式向screen窗口管理器发出命令，默认情况下，screen接收以`C-a`开始的命令。这种命令形式在screen中叫做键绑定（key binding），`C-a`叫做命令字符（command character）。

可以通过C-a ?来查看所有的键绑定，常用的键绑定有：

| 快捷键   |描述 |
|---|:---:|
| C-a ?	  |显示所有键绑定信息|
| C-a w	  |显示所有窗口列表|
| C-a C-a |	切换到之前显示的窗口|
| C-a c	  |创建一个新的运行shell的窗口并切换到该窗口|
| C-a n	  |切换到下一个窗口|
| C-a p	  |切换到前一个窗口(与C-a n相对)|
| C-a 0..9|	切换到窗口0..9|
| C-a a	  |发送 C-a到当前窗口|
| C-a d	  |暂时断开screen会话|
| C-a k	  |杀掉当前窗口|
| C-a [	  |进入拷贝/回滚模式|

##Last but not least

如果由于某种原因其中一个会话死掉了（例如人为杀掉该会话），这时screen -list会显示该会话为dead状态。使用screen -wipe命令清除该会话。

##参考资料

如果还有什么不了解的，可以使用`man page`以及下面的链接。

* [官网][5]
* [man page][4]




[1]: http://voidy.net/linux_tmux/
[2]: http://superuser.com/questions/236158/tmux-vs-screen
[3]: http://dominik.honnef.co/posts/2010/10/why_you_should_try_tmux_instead_of_screen/
[4]: http://www.slac.stanford.edu/comp/unix/package/epics/extensions/iocConsole/screen.1.html
[5]: http://www.gnu.org/software/screen/