---
title: 玩转Linux之内存管理-free
layout: post
date: 2015-02-09 21:48:08
tags: [Free,Linux]
categories: Linux
description: 本文介绍了Linux中free命令，以及缓冲区与缓存的区别，并举栗说明了free命令常用的参数
---
##简介

free命令可以显示Linux系统中空闲的、已用的物理内存及swap内存,及被内核使用的buffer。在Linux系统监控的工具中，free命令是最经常使用的命令之一。下面给出一个free命令的栗子：

	[root@compute ~]# free
	             total       used       free     shared    buffers     cached
	Mem:       8062392    2092832    5969560          0     187132    1498832
	-/+ buffers/cache:     406868    7655524
	Swap:      2097148          0    2097148

下面介绍一下这个命令的输出结果信息：

　　第一行：显示了内存的详细信息，比如说总内存、已用的内存、空闲的内存、多个进程共享的内存、用于缓冲区的内存以及用于缓存的内存。

　　第二行：显示了总的缓冲区内存/缓存的内存使用以及空闲的情况。使用的是第二行used总内存(2092832)-used缓冲区内存（187132）-used缓存区内存（1498832）=406868.空闲的是total的（8062392）-used的缓存/缓冲区内存（406868）=7655524.

　　第三行：显示了总的交换区总内存、已用的以及空闲的内存。交换区的就是在HDD上创建的用来增加虚拟的增加内存大小的虚拟内存。那么问题来了：

##缓冲区和缓存有什么区别呢？

缓冲区是针对特定的应用临时存储数据的地方，而且这些数据不能被其它应用使用。这和带宽的概念比较相似。当你尝试通过网络来传输突发性的数据时，如果你的网卡只能发送少量的数据时，它能把这些大量的数据存在缓冲区中，以便它能以较低的网卡能接受的速度来发送这些数据。在另一方面，缓存是为了更快的访问而存储一些被频繁使用的数据的东西。其它的不同就是缓存能被多次使用而缓冲区只能被用一次。但是它们都为你的数据处理提供一个临时存储。下面举些栗子来说下使用方法。

##free命令使用的栗子

###1.以兆字节为单位显示内存（常用）

这个是很好记的，就是-m：

	[root@compute ~]# free -m
	             total       used       free     shared    buffers     cached
	Mem:          7873       2043       5829          0        182       1463
	-/+ buffers/cache:        397       7476
	Swap:         2047          0       2047

###2.还有以字节、千字节、千兆为单位显示内存（不常用）

使用-b、-k、-g参数，即可以字节、千字节、千兆字节为单位显示内存的大小：

	[root@compute ~]# free -b
	             total       used       free     shared    buffers     cached
	Mem:    8255889408 2142736384 6113153024          0  191623168 1534803968
	-/+ buffers/cache:  416309248 7839580160
	Swap:   2147479552          0 2147479552

###3.显示总计使用情况

使用-t参数，将会多一行total用于显示总的使用量：

	[root@compute ~]# free -t
	             total       used       free     shared    buffers     cached
	Mem:       8062392    2092516    5969876          0     187132    1498832
	-/+ buffers/cache:     406552    7655840
	Swap:      2097148          0    2097148
	Total:    10159540    2092516    8067024

###4.关闭显示缓冲区那一行

使用-o参数，即可关闭第二行的显示：

	[root@compute ~]# free -o
	             total       used       free     shared    buffers     cached
	Mem:       8062392    2092764    5969628          0     187132    1498832
	Swap:      2097148          0    2097148

###5.以一个固定的时间间隔更新当前内存使用情况

加上-s参数，然后在-s参数后加上一个整数便会在定期的时间间隔中更新内存的使用情况，下面我将举个栗子，凑个整数吧，在1024s内更新一次：

	[root@compute ~]# free -o
	             total       used       free     shared    buffers     cached
	Mem:       8062392    2092764    5969628          0     187132    1498832
	Swap:      2097148          0    2097148

###6.额外显示低以及高的内存的统计数据

使用-l参数额外显示低以及高的内存大小统计数据：

	[root@compute ~]# free -l
	             total       used       free     shared    buffers     cached
	Mem:       8062392    2092516    5969876          0     187132    1498832
	Low:       8062392    2092516    5969876
	High:            0          0          0
	-/+ buffers/cache:     406552    7655840
	Swap:      2097148          0    2097148


###7.查看free命令的版本

使用-V参数显示版本信息：

	[root@compute ~]# free -V
	procps version 3.2.8

以上。

> 参考资料：
> <http://www.linuxnix.com/2013/05/find-ram-size-in-linuxunix.html>
> <http://www.tecmint.com/check-memory-usage-in-linux/>


---
> **版权声明**
> **本人博文若无特别说明，均由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **由于本人水平有限，所以难免有错，若发现错误，请在评论区任意吐槽~**
> **博文链接：<http://voidy.net/>**