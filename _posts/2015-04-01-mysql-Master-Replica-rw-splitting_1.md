---
title: 基于Mysql-Proxy实现Mysql的主从复制以及读写分离（上）
layout: post
date: 2015-04-01 21:13:48
tags: Mysql
categories: Mysql
comments: true
description: 本篇主要基于CentOS6.5系统实现了Mysql数据库的主从复制
---

上周BOSS给分配任务让实现一下Mysql数据库的主从复制以及读写分离，然后花了一盏茶的功夫进行了调研，发现主从复制数据库进行一番配置直接可以实现，而读写分离则需要一些软件的支持，基本上读写分离的实现有两种：

 * Amoeba（变形虫）：是由前阿里员工实现的一个以MySQL为底层数据存储，并对应用提供MySQL协议接口的proxy。但是由于没人维护了，而且据说作者也不再回答开发者的问题，所以不予考虑。
 * Mysql-Proxy：是一个处于你的client端和MySQL server端之间的简单程序，它可以监测、分析或改变它们的通信。它使用灵活，没有限制，常见的用途包括：负载平衡，故障、查询分析，查询过滤和修改等等。简单的说，MySQL Proxy就是一个连接池，负责将前台应用的连接请求转发给后台的数据库，并且通过使用lua脚本，可以实现复杂的连接控制和过滤，从而实现读写分离和负载平衡。并且还有各种资料以及维护，虽然需要了解一些lua语言的东西，但是语言么，上手还是很简单的。

在基于稳定性、后期遇到问题解决难易性、与现有平台整合的难易性、实现的难易性以及种种情况考虑之后，最终决定使用mysql-proxy来实现数据库的读写分离。啰嗦那么多，接下来开始说说具体的实现方法。

##环境与配置

系统：CentOS6.5

Master：172.16.19.2

Slave：172.16.19.24

mysql-proxy：172.16.19.14

安装就不说了，基本都是yum install进行安装的。下图是基本的架构图（忘了从哪里扒拉出来的了，感谢作者）。

![](http://images.cnitblog.com/blog2015/666211/201504/012242548427461.jpg)

##主从复制的实现

主从复制的实现极其简单，只需要改一下vim /etc/my.cnf。

**Master的配置：**

	vim /etc/my.cnf
	log-bin=mysql-bin   #新增
	server-id=1              #新增

配置完之后，重启mysql，然后执行如下指令：

	master：mysql> grant replication slave on *.* to 'root'@'%' identified by 	'123456';然后执行show master status
	Mysql> show master status;
	+------------------+----------+--------------+------------------+
	| File | Position | Binlog_Do_DB | Binlog_Ignore_DB |
	+------------------+----------+--------------+------------------+
	| mysql-bin.000005 | 261 | | |
	+------------------+----------+--------------+------------------+

用脑袋或者纸笔记录一下File以及Positin，在这里是mysql-bin.000005以及261。

**Slave的配置：**

	vim /etc/my.cnf
	log-bin=mysql-bin   #新增
	server-id=2             #新增  server-id不能一样

同样的，配置完成之后需要重启mysql服务，然后执行如下指令：

	mysql> change master to master_host='172.16.19.2',master_user='root',master_password='123456',master_log_file='mysql-bin.000005',master_log_pos=261;
	mysql> start slave;

接下来，查看slave的状态，查看是否配置成功：


	show slave status\G
	==============================================
	**************** 1. row *******************
	Slave_IO_State:
	Master_Host: 172.16.19.2
	Master_User: rep1
	Master_Port: 3306
	Connect_Retry: 60
	Master_Log_File: mysql-bin.000005
	Read_Master_Log_Pos: 261
	Relay_Log_File: localhost-relay-bin.000008
	Relay_Log_Pos: 561
	Relay_Master_Log_File: mysql-bin.000005
	Slave_IO_Running: YES
	Slave_SQL_Running: YES
	Replicate_Do_DB:
	……………省略若干……………
	Master_Server_Id: 1
	1 row in set (0.01 sec)
	==============================================


其中Slave_IO_Running 与 Slave_SQL_Running 的值都必须为YES，才表明状态正常。

##主从复制的测试

先查看主服务器Master：

![](http://images.cnitblog.com/blog2015/666211/201504/012315121238879.png)

然后创建一个数据库，y：

![](http://images.cnitblog.com/blog2015/666211/201504/012317358269643.png)

接着去从服务器slave查看，会发现从服务器也有了y数据库。至此数据库的主从复制就实现了。有点晚了，至于说读写分离就下次再说吧。



---
> **版权声明**
> **本人博文若无特别说明，均由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **由于本人水平有限，所以难免有错，若发现错误，请在评论区任意吐槽~**
> **博文链接：<http://voidy.net/>**