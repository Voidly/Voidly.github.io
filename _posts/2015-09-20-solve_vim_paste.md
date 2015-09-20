---
layout: post
title: "解决vim复制排版混乱的问题"
description: 
modified: 2015-09-20
category: Bugs
tags: [Vim]
comments: true
mathjax: 
---
{% highlight linenos %}
{% endhighlight %}

##问题

最近有些时候需要从其它地方复制一些东西到vim，在i模式下，它会将你拷贝的文本中已经有表示缩进的空格或者制表符也当成字符串，从而被缩进，而这会造成排版混乱的问题。本来是对齐的结果复制到vim里面就各种偏移。

##解决方案

####方案一

在命令行模式下：
* 在拷贝前输入`:set paste` (这样的话，vim就不会启动自动缩进，而只是纯拷贝粘贴）
* 拷贝完成之后，输入`:set nopaste` (关闭paste)

####方案二

* 用映射，`pastetoggle` 选项可以用来指定切换 `paste` 选项的热键。
* 在`vimrc`里面设置：`set pastetoggle=<leader>p`

##有关paste模式

打开`paste`选项时 :

* 屏蔽插入模式和命令行模式的映射
* 屏蔽缩写
* `textwidth` 设为 0
* `wrapmargin` 设为 0
* `autoindent` 被复位
* `smartindent` 被复位
* `softtabstop` 设为 0
* `revins` 被复位
* `ruler` 被复位
* `showmatch` 被复位
* `formatoptions` 的使用方式就像它为空一样

下面这些选项保持原来的值，但没有实际的效果:

* lisp
* indentexpr
* cindent

> PS: 打开 `paste` 选项时如果你开始编辑别的文件，模式行或者自动命令可能会再次改变这些设置，从而在粘贴文本时造成麻烦。这时，你可能需要再次置位 `paste` 选项。`paste` 选项复位时，上面提到的选项被复原为上次 `paste` 从关到开时的设置。置位 `paste` 之前复位 `paste` 没有任何效果。