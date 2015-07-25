---
layout: post
title: "通过ip获取所属地shell脚本实现"
description: 
modified: 2015-07-25
category: shell
tags: []
comments: true
---
##缘由
今天群里一个人问了个问题，是将IP解析成所属地。
##思考
恩，立即能想到的解决方法有三种：

* 方案一：使用在线网站查询。
* 方案二：写个shell脚本通过curl来做。
* 方案三：下载个数据库，然后通过映射关系来做。

方案一和二是简单易得的，但是劣势也很明显，精确度不会很高，依赖于制作网站的人数据库中数据多少，而且量大的话耗时也会长（百万级别）。而第三种方案，由于是自己做，工作量自然会相对较大，但是优点也很明显，自己可以搜罗IP库，然后进行整合，打造出较为庞大的IP地址库。

##实现
这里就说下简单的脚本实现。
假设我们有很多的IP,为了方便起见，我们将IP存在一个文件ip中，然后读取IP，并将IP查询的结果放入到result文件中。具体代码如下：

	#!/bin/bash
	#get_address_from_ip.sh
	#use: 
	#     (1)chmod +x get_address_from_ip.sh
	#     (2)./get_address_from_ip.sh ip.txt
	#PS: ip.txt is a file which store the ips.
	while read line
	do
    	result=$(curl http://www.ip.cn/index.php?ip=$line)
   	 	echo $result >> ip_result
	done < $1
	
也就是从第一个参数读取ip，然后查询将结果放入ip_result文件中。

#效果
![](https://github.com/Voidly/Img/blob/master/blog/get_address_form_ip.png?raw=true)
