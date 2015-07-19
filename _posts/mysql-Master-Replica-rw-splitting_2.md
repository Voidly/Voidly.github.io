title: 基于Mysql-Proxy实现Mysql的主从复制以及读写分离（下）
date: 2015-04-02 17:10:57
tags: Mysql
categories: Mysql
description: 本篇是上篇的续篇，主要讲了使用mysql-proxy来实现数据库的读写分离
---

昨天谈到了Mysql实现主从复制，但由于时间原因并未讲有关读写分离的实现，之所以有读写分离，是为了使数据库拥有双机热备功能，至于双机热备，特指基于高可用系统中的两台服务器的热备（或高可用），因两机高可用在国内使用较多，故得名双机热备，双机高可用按工作中的切换方式分为：主-备方式（Active-Standby方式）和双主机方式（Active-Active方式），主-备方式即指的是一台服务器处于某种业务的激活状态（即Active状态），另一台服务器处于该业务的备用状态（即Standby状态)。而双主机方式即指两种不同业务分别在两台服务器上互为主备状态（即Active-Standby和Standby-Active状态）。接下来开始使用Mysql-Proxy实现读写分离。

##环境

系统：CentOS6.5

Master：172.16.19.2

Slave：172.16.19.24

mysql-proxy：172.16.19.14

安装就不说了，基本都是yum install进行安装的。和昨天的一样（其实就是接着昨天的来~）。

##读写分离的实现

首先，在第三台服务器安装mysql-proxy：yum install mysql-proxy。

然后修改配置文件：

	[mysql-proxy]
	daemon = true   #以后台守护进程方式启动
	pid-file = /var/run/mysql-proxy.pid
	log-file = /var/log/mysql-proxy.log
	log-level = debug  #设置日志级别为debug，可以在调试完成后改成info
	max-open-files = 1024
	plugins = admin,proxy
	user = mysql-proxy
	#
	#Proxy Configuration
	proxy-address = 0.0.0.0:3307    #指定mysql-proxy的监听地址
	#proxy-address = 172.16.19.24:4040
	#proxy-backend-addresses = localhost:3306  
	proxy-backend-addresses = 172.16.19.2:3306   #设置后台主服务器：主服务器，master可读写
	proxy-read-only-backend-addresses = 172.16.19.24:3306   	#设置后台从服务器：从服务器，slave，只读
	proxy-lua-script = /usr/lib64/mysql-proxy/lua/rw-splitting.lua    	#设置读写分离脚本路径，可在官网下载mysql-proxy包，解压后
	#proxy-skip-profiling = true
	#
	# Admin Configuration
	#admin-address = 0.0.0.0:4041
	admin-lua-script = /usr/lib64/mysql-proxy/lua/admin.lua  	#设置管理后台lua脚本路径，改脚本默认没有要自动定义
	admin-username = root     #设置登录管理地址用户
	admin-password = 123456    #设置管理用户密码


设置完成后，即可执行：service mysql-proxy start启动服务。　

使用mysql -uroot -p -h172.16.19.14 --port=3307进入数据库查看或更改数据库。

使用mysql -uroot -p -h172.16.19.14 --port=4041进入数据库管理数据库。

![](http://images.cnitblog.com/blog2015/666211/201504/021706264821633.png)

若没有用户通过mysql-proxy连接到后端，则状态为unknown，否则的话为up。至此，mysql 的主从复制以及读写分离就实现了。

---
> **版权声明**
> **本人博文若无特别说明，均由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **由于本人水平有限，所以难免有错，若发现错误，请在评论区任意吐槽~**
> **博文链接：<http://voidy.net/>**