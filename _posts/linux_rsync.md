title: 如何在Linux下使用Rsync
date: 2015-01-11 11:23:31
tags: [Rsync,Linux]
categories: Linux
---

#吐槽

昨天对scp进行总结之后看到最后有说到Rsync，俗语有云：好奇心害死猫。抱着学习的态度将Rsync给找了出来，然后进行了一些简单的学习。下面介绍一些个常用的命令。上篇的scp：[12个scp传输文件的命令栗子](http://voidy.gitcafe.com/2015/01/10/12%E4%B8%AAscp%E4%BC%A0%E8%BE%93%E6%96%87%E4%BB%B6%E7%9A%84%E5%91%BD%E4%BB%A4%E6%A0%97%E5%AD%90/)

#简介
rsync是类unix系统下的数据镜像备份工具——remote sync。一款快速增量备份工具 Remote Sync，远程同步 支持本地复制，或者与其他SSH、rsync主机同步。

对于各种组织和公司，数据对他们是最重要的，即使对于电子商务，数据也是同样重要的。Rsync是一款通过网络备份重要数据的工具/软件。它同样是一个在类Unix和Window系统上通过网络在系统间同步文件夹和文件的网络协议。Rsync可以复制或者显示目录并复制文件。Rsync默认监听TCP 873端口，通过远程shell如rsh和ssh复制文件。Rsync必须在远程和本地系统上都安装。

rsync的主要好处是：
> **速度**：最初会在本地和远程之间拷贝所有内容。下次，只会传输发生改变的块或者字节。
> 
> **安全**：传输可以通过ssh协议加密数据。
> 
> **低带宽**：rsync可以在两端压缩和解压数据块。

**语法:**

	1.#rsysnc [options] source path destination path

下面来介绍一下具体的使用技巧。

##一、启用压缩、详细信息以及递归

	[root@localhost /]# rsync -zvr /home/aloft/ /backuphomedir
	building file list ... done
	.bash_logout
	.bash_profile
	.bashrc
	sent 472 bytes received 86 bytes 1116.00 bytes/sec
	total size is 324 speedup is 0.58

在上述命令中：
> **-z：**此选项是启用压缩，这样可以加快传输速度，因为在传输过程中它进行压缩，但是在传输完成后在另一端又解压缩，所以会节省时间，一般情况下可以节省几倍左右的时间，当然了，对于那些已经压缩的文件就没有效果了。

> **-v:**此选项启用后可以查看传输的详细的信息，以便于及时看到反馈信息。

> **-r:**此选项是递归下载，可用于下载整个文件夹时使用。

##二、保留文件和文件夹属性

	
	[root@localhost /]# rsync -azvr /home/aloft/ /backuphomedir
	building file list ... done
	./
	.bash_logout
	.bash_profile
	.bashrc
	 
	sent 514 bytes received 92 bytes 1212.00 bytes/sec
	total size is 324 speedup is 0.53

> **-a:**此选项可以保留文件或者文件夹的属性，如所属用和所属组、时间戳、软链接以及权限。

##三、同步本地到远程主机

	root@localhost /]# rsync -avz /home/aloft/ azmath@192.168.1.4:192.168.1.4:/share/rsysnctest/
	Password:
	 
	building file list ... done
	./
	.bash_logout
	.bash_profile
	.bashrc
	sent 514 bytes received 92 bytes 1212.00 bytes/sec
	total size is 324 speedup is 0.53

这个比较简单了，只要指定远程主机IP或者主机，以及用户名，并且知道密码，那么你就可以很轻松的在本地以及远程机器之间进行文件或文件夹的同步。

##四、远程同步到本地主机

	[root@localhost /]# rsync -avz azmath@192.168.1.4:192.168.1.4:/share/rsysnctest/ /home/aloft/
	Password:
	building file list ... done
	./
	.bash_logout
	.bash_profile
	.bashrc
	sent 514 bytes received 92 bytes 1212.00 bytes/sec
	total size is 324 speedup is 0.53 - See more at: http://linoxide.com/how-tos/rsync-copy/#sthash.	2HsquzPh.dpuf
	
 当然了，有同步到远程自然也会有同步到本地撒，这个和上面的三类似，仅仅是地址相反而已了~

##五、找出文件之间的不同

	[root@localhost backuphomedir]# rsync -avzi /backuphomedir /home/aloft/
	building file list ... done
	cd+++++++ backuphomedir/
	>f+++++++ backuphomedir/.bash_logout
	>f+++++++ backuphomedir/.bash_profile
	>f+++++++ backuphomedir/.bashrc
	>f+++++++ backuphomedir/abc
	>f+++++++ backuphomedir/xyz
	
	sent 650 bytes received 136 bytes 1572.00 bytes/sec
	total size is 324 speedup is 0.41

上面的命令可以帮你找出源地址和目的地址的文件或者目录之间的不同。
> **-i:**此选项可以将文件或者目录间的不同列出来方便迅速定位修改过的文件。

##六、备份

rsync命令可以用来备份linux。你可以在cron中使用rsync安排备份。

	0 0 * * * /usr/local/sbin/bkpscript &> /dev/null

	vi /usr/local/sbin/bkpscript

	rsync -avz -e ‘ssh -p2093′ /home/test/ root@192.168.1.150:/oracle/data/

##七、其它相关参数:

	-v, --verbose 详细模式输出
	-q, --quiet 精简输出模式
	-c, --checksum 打开校验开关，强制对文件传输进行校验
	-a, --archive 归档模式，表示以递归方式传输文件，并保持所有文件属性，等于-rlptgoD
	-r, --recursive 对子目录以递归模式处理
	-R, --relative 使用相对路径信息
	-b, --backup 	创建备份，也就是对于目的已经存在有同样的文件名时，将老的文件重新命名为~filename。可以使用--	suffix选项来指定不同的备份文件前缀。
	--backup-dir 将备份文件(如~filename)存放在在目录下。
	-suffix=SUFFIX 定义备份文件前缀
	-u, --update 仅仅进行更新，也就是跳过所有已经存在于DST，并且文件时间晚于要备份的文件。(	不覆盖更新的文件)
	-l, --links 保留软链结
	-L, --copy-links 想对待常规文件一样处理软链结
	--copy-unsafe-links 仅仅拷贝指向SRC路径目录树以外的链结
	--safe-links 忽略指向SRC路径目录树以外的链结
	-H, --hard-links 保留硬链结
	-p, --perms 保持文件权限
	-o, --owner 保持文件属主信息
	-g, --group 保持文件属组信息
	-D, --devices 保持设备文件信息
	-t, --times 保持文件时间信息
	-S, --sparse 对稀疏文件进行特殊处理以节省DST的空间
	-n, --dry-run现实哪些文件将被传输
	-W, --whole-file 拷贝文件，不进行增量检测
	-x, --one-file-system 不要跨越文件系统边界
	-B, --block-size=SIZE 检验算法使用的块尺寸，默认是700字节
	-e, --rsh=COMMAND 指定使用rsh、ssh方式进行数据同步
	--rsync-path=PATH 指定远程服务器上的rsync命令所在路径信息
	-C, --cvs-exclude 使用和CVS一样的方法自动忽略文件，用来排除那些不希望传输的文件
	--existing 仅仅更新那些已经存在于DST的文件，而不备份那些新创建的文件
	--delete 删除那些DST中SRC没有的文件
	--delete-excluded 同样删除接收端那些被该选项指定排除的文件
	--delete-after 传输结束以后再删除
	--ignore-errors 及时出现IO错误也进行删除
	--max-delete=NUM 最多删除NUM个文件
	--partial 保留那些因故没有完全传输的文件，以是加快随后的再次传输
	--force 强制删除目录，即使不为空
	--numeric-ids 不将数字的用户和组ID匹配为用户名和组名
	--timeout=TIME IP超时时间，单位为秒
	-I, --ignore-times 不跳过那些有同样的时间和长度的文件
	--size-only 当决定是否要备份文件时，仅仅察看文件大小而不考虑文件时间
	--modify-window=NUM 决定文件是否时间相同时使用的时间戳窗口，默认为0
	-T --temp-dir=DIR 在DIR中创建临时文件
	--compare-dest=DIR 同样比较DIR中的文件来决定是否需要备份
	-P 等同于 --partial
	--progress 显示备份过程
	-z, --compress 对备份的文件在传输时进行压缩处理
	--exclude=PATTERN 指定排除不需要传输的文件模式
	--include=PATTERN 指定不排除而需要传输的文件模式
	--exclude-from=FILE 排除FILE中指定模式的文件
	--include-from=FILE 不排除FILE指定模式匹配的文件
	--version 打印版本信息
	--address 绑定到特定的地址
	--config=FILE 指定其他的配置文件，不使用默认的rsyncd.conf文件
	--port=PORT 指定其他的rsync服务端口
	--blocking-io 对远程shell使用阻塞IO
	-stats 给出某些文件的传输状态
	--progress 在传输时现实传输过程
	--log-format=formAT 指定日志文件格式
	--password-file=FILE 从FILE中得到密码
	--bwlimit=KBPS 限制I/O带宽，KBytes per second
	-h, --help 显示帮助信息

以上就是有关Rsync的用法了，不对之处欢迎指出~看完之后觉得有收获请留下足迹让我知道~


>参考文章：<http://linoxide.com/how-tos/rsync-copy/>

>了解更多:<http://linux.die.net/man/1/rsync>


---
> **版权声明**
> **本博文由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **博文链接：<http://voidy.net/>**