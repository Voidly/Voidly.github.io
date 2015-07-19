---
title: 恢复git reset --hard删除的文件
layout: post
date: 2015-02-03 21:39:46
tags: [Git]
categories: [Git]
comments: true
description: 恢复git使用中，无意使用了git reset --hard后不小心误删的文件
---

##吐槽

在说恢复方法之前，先说下缘由。今天在转载完缓存算法之后，就打算把文件给同步到github上，以做备份。然后像往常一样打开git bash，在git add时遇到了错误，无法进行git add，因为我使用了hexo new进行生成文件，而hexo new生成的文件中，文件名的小括号和中括号都会变成'--'，也就是说`[转载]`变成了`-转载-`，而git bash无法识别`-`，所以造成无法提交，这时，我灵机一动，想到将其他的所有文件都移出去，然后`git add .`来添加然后再提交（因为其他文件有改动，所有想把此文件单独提交后再来进行全部的提交），移出之后我发现原来已经执行过`git add .`了。没办法，那就重置吧，但是我从网上找来一篇文章，`git reset --hard`，再然后，悲剧上演~

所以，骚年们，以后一定不要那么随便的相信一篇博文，要对知识抱有一定的怀疑态度，然后多看几篇博文甚至官方文档。不过最终从stackOverflow上找出了解决方法~

##恢复已提交文件的方法

* 使用`git reflog`来找到最近提交的信息,这里贴出部分信息。:
	
	F:\voidy>git reflog
	WARNING: terminal is not fully functional
	5ccb94c HEAD@{2}: commit: 设计模式汇总
	ba944a7 HEAD@{3}: commit: 组合模式
	49bf0f5 HEAD@{4}: commit: 状态模式


* 使用`git reset ID`来恢复文件（第一列为ID）,举个栗子：
比如说`设计模式汇总`,那么执行`git reset 5ccb94c`好了，如果是一般的情况到此已经解决了。但是，我没有提交啊。那么该怎么办？

##恢复未提交文件的方法

下面是stackoverFlow上一些人提供的方法

	If you didn't already commit your local changes (or at least stage them via `git add`, they're gone. `git reset --hard` is a destructive operation for uncommitted changes.

	If you did happen to stage them, but didn't commit them, try `git fsck --lost-found` and then search through the contents of .git/lost-found - it will contain all of the objects that aren't referenced by a known commit, and may include versions of files that were staged.

	You can recover anything you git added, with git fsck --lost-found and poke around in .git/lost-found.  find .git/objects -type f | xargs ls -lt | sed 60q will give you the last 60 things to get added to the repo, that'll help.

	Anything you didn't git add is gone as surely as if you'd deleted it yourself.

我总结一下吧，其实呢，就两步：

* 执行`find .git/objects -type f | xargs ls -lt | sed 60q`,这里`60q`的意思是最近60次的add，然后会出来这么个东东,此处仅仅截取一部分信息，然后就是恢复了:

	$ find .git/objects -type f | xargs ls -lt | sed 60q
	-r--r--r--    1 Y        Administ      222 Feb  3 21:00 .git/objects/02/18fb7591
	36a0ee550d2e4d179f01bd75af48a0
	-r--r--r--    1 Y        Administ      176 Feb  3 21:00 .git/objects/5c/cb94ce63
	fd5196db10dffa6bab149c8b30546e
	-r--r--r--    1 Y        Administ       77 Feb  3 21:00 .git/objects/d5/177a8da3
	96b5d6450d2c8e6ecf2f3ad8e41cd2
	-r--r--r--    1 Y        Administ     1387 Feb  3 21:00 .git/objects/e0/7e9099c0
	b27a4dd1a432db0bbf2112ca544ebd
	-r--r--r--    1 Y        Administ     5480 Feb  3 20:38 .git/objects/74/ea02bfa7
	353be6095959503abdd7dc0a178f53
	-r--r--r--    1 Y        Administ     2701 Feb  3 20:38 .git/objects/7a/d366bac4
	91f6793d9b7da0cc3e7fb5ba26d403
	-r--r--r--    1 Y        Administ     2597 Feb  3 20:38 .git/objects/a7/9c139160
	fa387b365629ead63f5818177d2e48
	-r--r--r--    1 Y        Administ     2458 Feb  3 20:38 .git/objects/c3/cc71cfa6
	40e80731bb8080a6baad1c0f2efa44
	-r--r--r--    1 Y        Administ     2463 Feb  3 20:38 .git/objects/c5/71825cf2
	3df3147e8854cd4903d00a08723db2
	-r--r--r--    1 Y        Administ     2666 Feb  3 20:38 .git/objects/e5/c001e1b2
	548ebc786f266b27941cbe7d93592a
	-r--r--r--    1 Y        Administ     1941 Feb  3 20:38 .git/objects/ec/08fe6869
	a6d1eb8cfb8b4e740da56f104a5646



* 使用`git cat-file -p ID > a.md`，解释一下这个命令，就是将ID所示的文件读取出来重定向保存到`a.md`文件内，ID是objects后面的一串东西，比如第一个就是`0218fb759136a0ee550d2e4d179f01bd75af48a0`。（PS：需要将之间的`/`去掉）。

好了，以上。对了，忘了说了，恢复未提交的文件仅仅是`git add`过的，如果你连`git add`也没有执行，那么我就不晓得怎么做了，如果你知道的话，请告诉我，另外，一定要记着，没事不要执行`git reset --hard`~然后网上的教程什么的也不要轻信，要时刻对知识抱有一种怀疑的态度，包括本文~



---
> **版权声明**
> **本人博文若无特别说明，均由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **由于本人水平有限，所以难免有错，若发现错误，请在评论区任意吐槽~**
> **博文链接：<http://voidy.net/>**