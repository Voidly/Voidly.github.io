title: Markdown入门总结
date: 2015-01-03 23:37:21
tags: [markdown,hexo]
categories: hexo
---
	Markdown让我们专注于创作，而非排版。

而且用Hexo写博客都是用Markdown来写，因此也让我对此兴趣浓厚，也就学习了一下。首先我读了最基础的一些Markdown知识，对其有了简单的认识，也就有了第一篇的博文。

## 通过以下文章可以学习Markdown：
*	[简书：献给作者的Markdown新手指南](http://www.jianshu.com/p/q81RER)
*	[Markdown简明教程](http://www.jianshu.com/p/7bd23251da0a)
*	[Markdown写作规范参考](http://www.jianshu.com/p/3bd994e702a7)	
*	[Markdown语法说明（简体中文版）](http://wowubuntu.com/markdown/)
*	[Markdown入门指南](http://www.jianshu.com/p/1e402922ee32)
*	[Markdown入门学习小结](http://www.jianshu.com/p/21d355525bdf)

在此感谢以上作者~~~

## 什么是Markdown

*	Markdown 是一种适用于网络书写的轻量级「标记语言」
*	Markdown 理念是能让文档更容易读、写和随意改。
*	Markdown 以纯文本发布，用简洁的语法代替排版。

下面是一段 Markdown 示例：

    	# Why *you* should use Markdown to write your next blog post
		[Markdown][1] is just so dang legible, it will make your *whole life* easier. **I promise.**
		[1]: http://daringfireball.net/projects/markdown/basics

相比较，对应的 HTML Markup 语言是这样的：

		<h1>Why <em>you</em> should use Markdown to write your next blog post</h1>
		<p><a href="http://daringfireball.net/projects/markdown/">Markdown</a>
 		is just so dang legible, it will make your <em>whole life</em> easier. <strong>I promise.</strong>
 		</p>

怎么样，是不是感觉 Markdown 语言读、写起来都更清爽呢

## 为什么需要 Markdown

###	Markdown 优点

*	纯文本，所以兼容性极强，可以用所有文本编辑器打开。
*	Markdown 的标记语法有极好的可读性。
*	让你专注于文字而不是排版。
*	格式转换方便，Markdown 的文本你可以轻松转换为 html、电子书等。

##	谁在使用 Markdown

Markdown 诞生于互联网时代，更是由深谙互联网文本之道的 John Gruber 等人设计。因为 Ruby 与 Github 圈的极客们的热捧，以及来自 Github、Stackoverflow 等网站的大力支持（Github 和 Stackoverflow 的 Issues, comments, pull request descriptions and READMEs 都支持 Markdown 语法）。从一开始，就建立一个完整的生态链。
一切就这么简单。Markdown之所以在被鼓吹之后，越来越流行，不是因为它复杂，而是因为它足够简单。

## 如何掌握 Markdown

Markdown 的语法十分简单。常用的标记符号不超过十个，5分钟即可掌握。应该是为数不多，你真的可以彻底学会的语言。

## 基本Markdown语法总结

## *1.标题*
=
语法：

	# 一级标题
	## 二级标题
	### 三级标题
	#### 四级标题
	##### 五级标题
	###### 六级标题	

预览效果：

# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题


## *2.无序列表与有序列表*
=

语法：
	
	- 无序列表1
	- 无序列表2
	- 无序列表3

	* 无序列表1
	* 无序列表2
	* 无序列表3

	- 外层列表项目
 	 * 内层列表项目
	 * 内层列表项目
 	 * 内层列表项目
	- 外层列表项目

	1. 有序列表1
	2. 有序列表2
	3. 有序列表3

预览效果：


- 无序列表1
- 无序列表2
- 无序列表3

* 无序列表1
* 无序列表2
* 无序列表3

- 外层列表项目
 * 内层列表项目
 * 内层列表项目
 * 内层列表项目
- 外层列表项目

1. 有序列表1
2. 有序列表2
3. 有序列表3


##*3.粗体与斜体*

语法：


	**粗体**
	__粗体__
	*斜体*
	_斜体_

预览效果:

**粗体**

__粗体__

*斜体*

_斜体_

## *4.分割线与删除线*

语法：

	---
	***
	~~文字删除线~~


预览效果：

---

***

~~文字删除线~~

## *5.首行缩进*

语法：

	&ensp;(相当于一个空格)
	&emsp;(相当于两个空格)

预览效果：

&ensp;相当于一个空格

&emsp;相当于两个空格

## *6.添加超链接与图片*

语法：

	[点击跳转图片](链接地址)
	![图片](链接地址)

效果预览：

[名字](http://ww2.sinaimg.cn/large/5e8cb366jw1e62o63tkv3j20dh078q5a.jpg)

![名字](http://ww2.sinaimg.cn/large/5e8cb366jw1e62o63tkv3j20dh078q5a.jpg)

或者可以这样写：

语法：

	[点击跳转图片][1]
	![图片][2]

	[1]:链接地址
	[2]:链接地址

预览效果:


[名字][1]

![名字][2]

[1]:http://ww2.sinaimg.cn/large/5e8cb366jw1e62o63tkv3j20dh078q5a.jpg

[2]:http://ww2.sinaimg.cn/large/5e8cb366jw1e62o63tkv3j20dh078q5a.jpg


## *7.表格*

语法：

	| ABCD | EFGH | IJKL |
	| -----|:----:| ----:|
	| a    | b    | c    |
	| d    | e    |  f   |
	| g    | h    |   i  |

预览效果：

| ABCD | EFGH | IJKL |
| -----|:----:| ----:|
| a    | b    | c    |
| d    | e    |  f   |
| g    | h    |   i  |


或者这样：

语法：

	ABCD | EFGH | IGKL
	-----|------|----
	a    | b    | c
	d    | e    | f
	g    | h    | i


效果预览:

ABCD | EFGH | IGKL
-----|------|----
a    | b    | c
d    | e    | f
g    | h    | i



## *8.代码*

语法:

	`代码比较少`

效果预览：

`代码比较少`

如果比较多的话就这样：

语法：


	Tab或四个空格（在每行前添加）


效果如上。



## *9.引用*


语法：

	> 引用的文字
	> 引用的文字
	> 引用的文字

效果：

> 引用的文字
> 
> 引用的文字

> 引用的文字

还可以这样：

	> 引用的文字引用的文字引用的文字引用的文字引用的文字

	 >> 引言内的引言引言内的引言引言内的引言

	> 引用的文字引用的文字引用的文字引用的文字引用的文字


效果：

> 引用的文字引用的文字引用的文字引用的文字引用的文字

 >> 引言内的引言引言内的引言引言内的引言

> 引用的文字引用的文字引用的文字引用的文字引用的文字

## *10.创建链接*

语法：

	<test@domain.com>

效果预览：

<test@domain.com>

## *11.特殊符号*

语法:


	\\
	\`
	\*
	\_
	\{\}
	\[\]
	\(\)
	\#
	\+
	\-
	\.
	\!
	\|

预览效果:

\\

\`

\*

\_

\{\}

\[\]

\(\)

\#

\+

\-

\.

\!

\|


---
> **版权声明**
> **本博文由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **博文链接：<http://voidy.net/>**	
	