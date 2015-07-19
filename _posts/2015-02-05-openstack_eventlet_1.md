---
title: 玩转Openstack之Nova中的协同并发（一）
layout: post
date: 2015-02-05 18:11:44
tags: [Openstack,cloud computing]
categories: [cloud computing]
comments: true
description: 本文为了便于理解，所以先介绍了一些有关Python的并发知识，详细介绍了Python的协程的知识。
---

前不久参加了个Opnstack的Meetup，其中有一个来自EasyStack的大大就Nova中的协同并发做了一番讲解，有所感触，本想当天就总结一下，但是由于前段时间工作上比较忙，加上为了履行诺言每天几更的来写设计模式系列性文章，故而拖到今天才写此次的总结。好吧，其实归根结底还是自己太懒了，趁着闲时在补新番小笼包之类的。废话就此打住，开始正文。

##Python中协程的介绍

在此之前，先介绍下Python中的并发，在Python中，并发有三种，分别是：

* 进程：Python中一般使用multiprocessing/subprocess来实现
* 线程：threading/thread是Python中用来实现多线程的模块
* 协程（Coroutines）：Python中用于处理协程的模块倒是比较多，有eventlet、Twisted、Tulip、asyncio

有关进程、协程、线程中的关系图如下所示（图来自EasyStack的大大）：

![](http://images.cnitblog.com/blog/666211/201502/051646566241527.png)

想必大家对进程以及线程那是相当的熟悉了，所以就重点介绍下协程：协程源自 Simula 和 Modula-2 语言，但也有其他语言支持。协程更适合于用来实现彼此熟悉的程序组件，如合作式多任务，迭代器，无限列表和管道。 协程最初在1963年被提出。那么协程又有什么特点呢？

* 每个协程都有自己的私有stack以及局部变量。
* 线程我们都知道可以多个同时运行，也就是所谓的多线程，但是同一时间只有一个协程在运行，所以就无须对某些共享变量加锁。
* 由于协程比较轻量级，所以一个线程中可以有多个协程。
* 协程之间的执行顺序，完全由程序来控制。
* 其实协程也就仅仅是一种概念罢了，非操作系统可见，在多种语言中都有实现，一会详细介绍的eventlet就是在Python中实现的一种。

##Eventlet的介绍

eventlet其实就是对greenlet的一个封装，对其进行简单的封装之后，就成了所谓的greenthread，greenlet是一个称为协程(coroutine)的东西。下面上一个greenlet的栗子来介绍一下greenlet：

	from greenlet import greenlet
	
	def test1():
	    print 12
	    gr2.switch()
	    print 34
	
	def test2():
	    print 56
	    gr1.switch()
	    print 78
	
	gr1 = greenlet(test1)
	gr2 = greenlet(test2)
	gr1.switch()

这个栗子的执行结果是：

	12
	56
	34

也就是说在这里先定义了两个函数test1,test2以及两个协程gr1,gr2，最后一行g1.switch()跳转到 test1() ，它打印12，然后执行gr2.switch()，跳转到 test2() ，打印56，然后又执行了gr1.switch()跳转回 test1() ，打印34，然后 test1() 就结束，gr1死掉，回到父greenlet，不会再切换到test2，所以不会打印78。在上面的例子中main greenlet就是它们的父 greenlet。

eventlet是一个用来处理和网络相关的python库函数，而且可以通过协程来实现并发，在eventlet里，把“协程”叫做greenthread(绿色线程)。所谓并发，就是开启了多个greenthread，并且对这些greenthread进行管理，以实现非阻塞式的I/O。比如说用eventlet可以很方便的写一个性能很好的web服务器，或者是一个效率很高的网页爬虫，这都归功于eventlet的“绿色线程”，以及对“绿色线程”的管理机制。更让人不可思议的是，eventlet为了实现“绿色线程”，竟然对python的和网络相关的几个标准库函数进行了改写，并且可以以补丁（patch）的方式导入到程序中，因为python的库函数只支持普通的线程，而不支持协程，eventlet称之为“绿化”。Eventlet库在OpenStack服务中上镜率很高，尤其是在服务的多线程和WSGI Server并发处理请求的情况下。

**主要API如下：**

　　**Greenthread 产生：**

　　**spawn(func, *args, **kwargs)：** 创建一个绿色线程去运行func这个函数，后面的参数是传递给这个函数的参数。返回值是一个eventlet.GreenThread对象，这个对象可以用来接受func函数运行的返回值。在绿色线程池还没有满的情况下，这个绿色线程一被创建就立刻被执行。其实，用这种方法去创建线程也是可以理解的，线程被创建出来，肯定是有一定的任务要去执行，这里直接把函数当作参数传递进去，去执行一定的任务，就好像标准库中的线程用run()方法去执行任务一样。

　　**spawn_n(func, *args, **kwargs)： **

这个函数和spawn()类似，不同的就是它没有返回值，因而更加高效，这种特性，使它也有存在的价值。


　　**spawn_after(seconds, func, *args, **kwargs)：**  

这个函数和spawn()基本上一样，都有一样的返回值，不同的是它可以限定在什么时候执行这个绿色线程，即在seconds秒之后，启动这个绿色线程。

　　**Greenthread 控制：**

　　　　**sleep(seconds=0)：**中止当前绿色线程，以允许其它绿色线程执行。
　　　　**eventlet.GreenPool：**  这是一个类，在这个类中用set集合来容纳所创建的绿色线程，并且可以指定容纳线程的最大数量（默认是1000个），它的内部是用Semaphore和Event这两个类来对池进行控制的，这样就构成了线程池，下面有一些重要的方法：

　　　　* running()：返回当前池中的绿色线程数
　　　　* free()：返回当前池中可容纳的绿色线程数
　　　　* spawn()：创建新的绿色线程
　　　　* spawn_n()：同上


　　今天就先介绍到这里，下次再继续介绍协同并发在Nova中的应用。

参考资料：

　　　  1. greenlet官网

　　　　2. greenlet官方文档翻译

　　　　3. greenlet原理介绍

　　　　4. openstack nova基础知识之eventlet

　　　　5. easystack演讲


---
> **版权声明**
> **本人博文若无特别说明，均由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **由于本人水平有限，所以难免有错，若发现错误，请在评论区任意吐槽~**
> **博文链接：<http://voidy.net/>**