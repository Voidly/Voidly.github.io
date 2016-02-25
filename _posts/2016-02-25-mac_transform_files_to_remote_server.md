---
layout: post
title: "Mac iTerm 2使用rz、sz从远程上传下载文件"
description: 
modified: 2016-02-25
category: Mac
tags: [Mac, lrzsz]
comments: true
mathjax: 
---
{% highlight linenos %}
{% endhighlight %}

最近有将本地文件传向服务器的需求，于是从同事那里听来了rz、sz这个黑魔法，迫不及待就安装使用了一下，极其好用。

（PS：不要问为嘛不使用scp，因为种种原因无法使用，而且有了这个之后即使可以使用scp我也不会用了，为嘛？懒！输命令很麻烦！ip记不住更麻烦！）

##安装配置

####1.安装lrzsz

使用`brew install lrzsz`。如果安装遇到错误的话，使用以下方法：

- `curl -k https://ohse.de/uwe/releases/lrzsz-0.12.20.tar.gz -o lrzsz-0.12.20.tar.gz`

- 将下载的文件放到`~/Library/Caches/Homebrew`目录下
- `brew install lrzsz`

####2.将 `iterm2-send-zmodem.sh` 和 `iterm2-recv-zmodem.sh`脚本保存在 `/usr/local/bin/`

{% highlight bash linenos %}
#!/bin/bash
# iterm2-recv-zmodem.sh

FILE=`osascript -e 'tell application "iTerm" to activate' -e 'tell application "iTerm" to set thefile to choose folder with prompt "Choose a folder to place received files in"' -e "do shell script (\"echo \"&(quoted form of POSIX path of thefile as Unicode text)&\"\")"`
if [[ $FILE = "" ]]; then
	echo Cancelled.
	# Send ZModem cancel
	echo -e \\x18\\x18\\x18\\x18\\x18
	echo \# Cancelled transfer
	echo
else
	echo $FILE
	cd "$FILE"
	/usr/local/bin/rz 
	echo \# Received $FILE
	echo
fi
{% endhighlight %}



{% highlight bash linenos %}
#!/bin/bash
# iterm2-send-zmodem.sh

FILE=`osascript -e 'tell application "iTerm" to activate' -e 'tell application "iTerm" to set thefile to choose file with prompt "Choose a file to send"' -e "do shell script (\"echo \"&(quoted form of POSIX path of thefile as Unicode text)&\"\")"`
if [[ $FILE = "" ]]; then
	echo Cancelled.
	# Send ZModem cancel
	echo -e \\x18\\x18\\x18\\x18\\x18
	echo \# Cancelled transfer
	echo
else
	echo $FILE
	/usr/local/bin/sz "$FILE"
	echo \# Received $FILE
	echo
fi
{% endhighlight %}

####3.在iTerm 2添加Triggers

打开iTerm的Preferences，或者使用`command`+`,`，点进Profiles项，Advanced项,进入Tirggers，点击Edit，添加两条规则：

{% highlight linenos %}
Regular expression: rz waiting to receive.\*\*B0100
Action: Run Silent Coprocess
Parameters: /usr/local/bin/iterm2-send-zmodem.sh

Regular expression: \*\*B00000000000000
Action: Run Silent Coprocess
Parameters: /usr/local/bin/iterm2-recv-zmodem.sh
{% endhighlight %}

至此，所有配置完成。下面就可以使用黑科技来传输文件了。

##使用方法

####将文件传到远端服务器

1. 在远端服务器上输入`rz`，回车
2. 选择本地要上传的文件
3. 等待上传

####从远端服务器下载文件

1. 在远端服务器输入`sz filename filename1 ... filenameN`
2. 选择本地的存储目录
3. 等待下载

**PS:**远端服务器也需要安装lrzsz。centOS安装方法：
`yum -y install lrzsz`

##参考

> https://github.com/mmastrac/iterm2-zmodem