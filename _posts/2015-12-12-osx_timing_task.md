---
layout: post
title: "OSX的定时任务管理"
description: 
modified: 2015-12-13
category: Mac
tags: [Mac,Linux]
comments: true
mathjax: 
---
{% highlight linenos %}
{% endhighlight %}


不同于Linux系统，Mac的定时任务管理不仅仅有常见的crontab，还有launchctl，下面简单介绍:innocent:。

##crontab命令

crontab是Linux下的定时任务命令，Mac自然也是支持的。使用man可以看到：

{% highlight bash linenos %}
crontab [-u user] { -l | -r | -e }
{% endhighlight %}

####相关参数说明

* `-u`：用来设定某个用户的crontab服务
* `file`：file是命令文件的名字,表示将file做为crontab的任务列表文件并载入crontab。如果在命令行中没有指定这个文件，crontab命令将接受标准输入（键盘）上键入的命令，并将它们载入crontab。
* `-l`：显示当前的crontab任务
* `-e`：编辑crontab任务
* `-r`：删除当前的crontab任务


####基本格式

{% highlight bash linenos %}
*		*		*		*		*	command
分		时		日		月		周	定时执行的指令/任务
0~59	0~23	1~31	1~12	0~7
{% endhighlight %}

> PS:周的0和7都代表星期天。


####特殊字符及其代表意义

| 特殊字符        | 代表意义           |
| ------------- |:-------------:|
| *(星号)      | 代表任意时刻，eg:在分钟那个位置代表每分钟执行 |
| ,(逗号)      | 代表时间点。eg:3,6 * * * * command 代表在每个小时的3分以及6分执行      |
| -(减号) | 代表时间范围。eg:06 8-12 * * * command代表在8点到12点每个小时的第6分钟执行      |
| /n(除n) | n代表数字，每隔n分钟执行一次。eg:/30 * * * * command代表每半个小时执行一次      |


> * 通过上面的命令介绍可见crontab的最小时间间隔是一分钟


##launchctl定时任务

这个是通过plist配置的方式来实现定时任务的，其优点就是最小时间间隔是一秒

plist脚本存放路径为/Library/LaunchDaemons或/Library/LaunchAgents，其区别是后一个路径的脚本当用户登陆系统后才会被执行，前一个只要系统启动了，哪怕用户不登陆系统也会被执行。

可以通过两种方式来设置脚本的执行时间。一个是使用StartInterval，它指定脚本每间隔多长时间（单位：秒）执行一次；另外一个使用StartCalendarInterval，它可以指定脚本在多少分钟、小时、天、星期几、月时间上执行，类似如crontab的中的设置。


####步骤

* 首先在`~/Library/LaunchAgents`下建一个plist文件,文件名随意写一个:com.test.launchctl.plist。

内容如下：

{% highlight xml linenos %}

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>Label</key>
  <string>com.test.launchctl.plist</string>
  <key>ProgramArguments</key>
  <array>
    <string>/Users/y/AutoMakeLog.sh</string>
  </array>
  <key>StartCalendarInterval</key>
  <dict>
        <key>Minute</key>
        <integer>14</integer>
        <key>Hour</key>
        <integer>13</integer>
  </dict>
  <key>StandardOutPath</key>
<string>/var/log/AutoMakeLog.log</string>
<key>StandardErrorPath</key>
<string>/var/log/AutoMakeLog.err</string>
</dict>
</plist>

{% endhighlight %}

简单说明下plist的内容：

* `plist`的主要内容是以键值对给出的。
* `label`是plist的文件名，唯一。
* `ProgramArguments`是执行的命令参数，也就是你的脚本文件。
* `StartCalendarInterval`是执行的时间，这里设置为每天13时14分执行。
* 剩下的两个是log日志以及错误日志，当运行有问题时用来查错。

####常用命令

加载命令，让任务每天定时执行：

* aunchctl load com.test.launchctl.plist

取消定时任务：

* launchctl unload com.test.launchctl.plist

直接开始命令：

* launchctl start com.test.launchctl.plist

直接结束命令：

* launchctl stop com.test.launchctl.plist

> PS:修改了脚本之后需要重新进行unload以及load。


##参考

> *  http://linuxtools-rst.readthedocs.org/zh_CN/latest/tool/crontab.html
> * https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man5/launchd.plist.5.html





