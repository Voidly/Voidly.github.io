title: Centos6.5创建Yum源
date: 2015-01-14 21:54:04
tags: [Linux,Yum]
categories: Linux
---
##吐槽起因

昨天给布置个新的需求，做一个Yum仓库，要求是HTTP式的，在某个服务器上搭建个Yum仓库，能让其它的机器有了这个机器的.repo仓库文件后就可以从本地下载安装软件，以前都是下载后直接yum install直接安装，也没怎么接触过，好吧，既然需求来了那就做呗。那么问题来了，什么是Yum呢，虽然一直在用，却知其然不知其所以然，果断谷歌之。

##简介

Yum（全称为 Yellow dog Updater, Modified）是一个在Fedora和RedHat以及CentOS中的Shell前端软件包管理器。基于RPM包管理，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包，无须繁琐地一次次下载、安装。是杜克大学为了提高RPM 软件包安装性而开发的一种软件包管理器。起初是由yellow dog 这一发行版的开发者Terra Soft 研发，用python 写成，那时还叫做yup(yellow dog updater)，后经杜克大学的Linux@Duke 开发团队进行改进，遂有此名。yum 的宗旨是自动化地升级，安装/移除rpm 包，收集rpm 包的相关信息，检查依赖性并自动提示用户解决。yum 的关键之处是要有可靠的repository，顾名思义，这是软件的仓库，它可以是http 或ftp 站点，也可以是本地软件池，但必须包含rpm 的header，header 包括了rpm 包的各种信息，包括描述，功能，提供的文件，依赖性等。正是收集了这些header 并加以分析，才能自动化地完成余下的任务。

yum 的理念是使用一个中心仓库(repository)管理一部分甚至一个distribution 的应用程序相互关系，根据计算出来的软件依赖关系进行相关的升级、安装、删除等等操作，减少了Linux 用户一直头痛的dependencies 的问题。这一点上，yum 和apt 相同。apt 原为debian 的deb 类型软件管理所使用，但是现在也能用到RedHat 门下的rpm 了。

yum 主要功能是更方便的添加/删除/更新RPM 包，自动解决包的倚赖性问题，便于管理大量系统的更新问题。

yum 可以同时配置多个资源库(Repository)，简洁的配置文件（/etc/yum.conf），自动解决增加或删除rpm 包时遇到的依赖性问题，保持与RPM 数据库的一致性。

##准备工作

1.首先需要检查一下你的系统的yum：　

	1 $rpm -qa | grep yum
	2 yum-plugin-priorities-1.1.30-17.el6_5.noarch                                                                                                                                              
	3 yum-3.2.29-43.el6.centos.noarch                                                                                                                                                          
	4 PackageKit-yum-plugin-0.5.8-21.el6.x86_64                                                                                                                                                
	5 yum-plugin-security-1.1.30-17.el6_5.noarch                                                                                                                                               
	6 yum-utils-1.1.30-17.el6_5.noarch                                                                                                                                                         
	7 yum-plugin-fastestmirror-1.1.30-17.el6_5.noarch                                                                                                                                          
	8 yum-metadata-parser-1.1.2-16.el6.x86_64                                                                                                                                                  
	9 PackageKit-yum-0.5.8-21.el6.x86_64

 其中第三个、第七个以及第八个比较重要。

2.然后下载安装createrepo

	1 yum install createrepo

##制作Yum源

　1.随便创建一个地方作为yum仓库，用于存放rpm包：

	1 $mkdir /usr/local/yumrepo

 2.把rpm包都拷贝进文件夹。

 3.把秘钥拷贝进来

	1 $cp /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6 .

 因为已经在yumrepo的目录里，所以用“.”表示当前目录。

　4.执行命令生成repodata：

	1 $createrepo -v /usr/local/yumrepo

因为我的rpm包是在此目录下，所以这么写，-v参数后面跟的是你的rpm包的文件夹！

　5.接下来就是制作一个*.repo的文件了。

　仿照着其它的文件写即可：

	1 [Mysql]
	2 name=Mysql
	3 baseurl=http://download.fedoraproject.org/pub/epel/6/$basearch
	4 enabled=1
	5 gpgcheck=1
	6 gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-6

名字嘛，随意起，开心就好~baseurl这个需要看你怎么玩了，一会细说，

第4行：enabled=1;此行的意思是是否启用该配置，1为启用，0不启用，至于其它的数字？没事的可以试着玩玩，或者818官网文档。

第5行：gpgcheck=1这个是是否启用检查验证，1为检查，0为关闭，如果检查的话那么第6行就有用武之地了，还记得大明湖畔拷贝的RPM-GPG-KEY-CentOS-6么，不记得？回到第三步自己瞅去。

接下来重点说一下第3行的baseurl，你若是本地使用的话按照如下方式来写：

	1 baseurl=file:///usr/local/yumrepo

当然我的是基于http的一会再说。当然还有一个就是不要忘了把创建的*.repo文件拷贝到/etc/yum.repos.d/文件夹下面！！！

##基于HTTP的Yum源配置

恩，接下来重点来了：

1.修改配置文件/etc/httpd/conf/httpd.conf：

在末尾有这么一些东西，将注释解掉改为如下的格式：


	1 <VirtualHost *:80>
	2 ServerAdmin root
	3 DocumentRoot /usr/
	4 ServerName IP地址
	5 #ErrorLog log
	6 #CustomLog logs/dummy-host.example.com-access_log common
	7 </VirtualHost>

注意了，第三行中需要写你的文件夹的第一层目录，即你存放位置的根目录，然后第4行写你的IP地址。

2.重启服务：

	1 $service httpd restart

　3.修改你的*.repo文件的baseurl。改为如下格式：

	1 baseurl=http://IP地址/local/yumrepo

然后就大功告成！什么太简单了，重点就这么几句话？别得了便宜还卖乖好吗？！要记住：简单的未必是最好的，但最好的一定是简单的！

##测试

你可以在本地测试，也可以在其他机器测试，首先输入以下命令：

	1 $yum list available
	2 .Loaded plugins: fastestmirror, priorities, refresh-packagekit, security                       
	3 .Loading mirror speeds from cached hostfile                                                    
	4 .base                                                                   | 2.9 kB     00:00     
	5 .Available Packages                                                                            
	6 .MySQL-client.i386                                    5.5.16-1.rhel4                       base
	7 .MySQL-devel.i386                                     5.5.16-1.rhel4                       base
	8 .MySQL-server.i386                                    5.5.16-1.rhel4                       base
	9 .mysql-community-release.noarch                       el6-5                                bas


　然后安装最后一个试试：

	1 $yum install   mysql-community-release.noarch 0:el6-5 
	2 Loaded plugins: fastestmirror, refresh-packagekit, security
	3 Loading mirror speeds from cached hostfile
	4 Setting up Install Process
	5 Resolving Dependencies
	6 --> Running transaction check
	7 ---> Package mysql-community-release.noarch 0:el6-5 will be installed
	8 --> Finished Dependency Resolution
	9 
	0 Dependencies Resolved
	1 
	2 ==============================================================================================	=============================================================================
	3  Package                                               Arch                                 	ersion                                Repository                          Size
	4 ==============================================================================================	=============================================================================
	5 Installing:
	6  mysql-community-release                               noarch                               	l6-5                                  base                               5.7 k
	7 
	8 Transaction Summary
	9 ==============================================================================================	=============================================================================
	0 Install       1 Package(s)
	1 
	2 Total download size: 5.7 k
	3 Installed size: 4.3 k
	4 Is this ok [y/N]: y
	5 Downloading Packages:
	6 mysql-community-release-el6-5.noarch.rpm                                                      	                                                      | 5.7 kB     00:00     
	7 Running rpm_check_debug
	8 Running Transaction Test
	9 Transaction Test Succeeded
	0 Running Transaction
	1 Warning: RPMDB altered outside of yum.
	2 ** Found 4 pre-existing rpmdb problem(s), 'yum check' output follows:
	3 cvs-1.11.23-16.el6.x86_64 has missing requires of vim-minimal
	4 1:libguestfs-1.20.11-2.el6.x86_64 has missing requires of vim-minimal
	5 1:libguestfs-tools-c-1.20.11-2.el6.x86_64 has missing requires of /bin/vi
	6 sudo-1.8.6p3-12.el6.x86_64 has missing requires of vim-minimal
	7   Installing : mysql-community-release-el6-5.noarch                                           	                                                                         1/1 
	8   Verifying  : mysql-community-release-el6-5.noarch                                           	                                                                         1/1 
	9 
	0 Installed:
	1   mysql-community-release.noarch 0:el6-5                                                      	                                                                             
	2 
	3 Complete!


　成功了，以上就是创建yum源的全部过程，忘了说了，我是在另一台机器上测试的，先用scp将yum.repos.d中的文件拷贝过去然后才进行测试~

　以上就是本次的全部内容了~



遇到的问题及解决方法：

先开始懒得进行验证然后报错：

	1 warning: rpmts_HdrFromFdno: V3 DSA/SHA1 Signature, key ID 5072e1f5: NOKEY 
	2 Public key for mysql-community-release-el6-5.noarch.rpm is not installed

明显是秘钥问题，然后懒得弄了就在安装的后面加了个参数：

　方法一：

	1 $yum install 包名 --nogpkcheck

对，没错，就是--nogpgcheck。

　方法二：

	1 rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

　方法三：

　yum.conf 文件,把里面的gpgcheck=1改为gpgcheck=0。　




---
> **版权声明**
> **本博文由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **博文链接：<http://voidy.net/>**