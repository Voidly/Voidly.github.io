---
title: 玩转Openstack之Nova中的协同并发（二）
layout: post
date: 2015-02-06 18:31:29
tags: [Openstack,cloud computing]
categories: [cloud computing]
comments: true
description: 本文从Nova代码入手，分析了协同并发，介绍了eventlet、greenthread在Nova中的应用
---
昨天介绍了Python中的并发处理，主要介绍了Eventlet，今天就接着谈谈Openstack中Nova对其的应用。

##eventlet　　

在nova/cmd/__init__.py中，就直接调用了eventlet的方法，代码如下：　

	from nova import debugger
	
	if debugger.enabled():
	    eventlet.monkey_patch(os=False, thread=False)
	else:
	    eventlet.monkey_patch(os=False)

这里在调试器被启动后，关闭线程，然后启用远程调试器。这个就是eventlet.monkey_patch()的方法。这里仅仅是因为dnspython无法支持IPV6，所以使用eventlet的monkeypatch检测一下环境变量的设置是否符合。

##greenthread

在虚机迁移过程中如果看过我写的源码分析，相信对于下面的代码不会陌生：

	greenthread.spawn(self._live_migration, context, instance, dest,
	                         post_method, recover_method, block_migration,
	                         migrate_data)

这个是热迁移中所使用的所调用的由eventlet所封装而成的绿色线程，调用了spawn(func,*args, kwargs)的函数，创建了一个绿色线程去运行live_migration也就是热迁移的函数，返回值是一个eventlet.greenthread的对象，这个对象可以用来接受live_migration运行的返回值。在绿色线程池未满的情况下，就可以直接执行热迁移的函数。

##greenthread.sleep	

然后Nova中用到的最多的绿色线程的栗子可能就是time.sleep了吧，下面随便找了几个用到的栗子：

	for cnt in range(max_retry):
	            try:
	                self.plug_vifs(instance, network_info)
	                break
	            except processutils.ProcessExecutionError:
	                if cnt == max_retry - 1:
	                    raise
	                else:
	                    LOG.warn(_('plug_vifs() failed %(cnt)d. Retry up to '
	                               '%(max_retry)d.'),
	                             {'cnt': cnt,
	                              'max_retry': max_retry},
	                             instance=instance)
	                    greenthread.sleep(1)     

这个是调用plug_vifs的函数中的greenthread.sleep()的函数调用，这个函数多次的发送请求。

	except exception.InstanceNotFound:
	               
	                pass
	            greenthread.sleep(0)
	        return disk_over_committed_size

像这样的栗子还有好多，一般情况下，greenthread.sleep()绿色线程的函数是为了中止当前的线程，用来给其它的线程一个执行的机会。其实说的通俗点就是传说中的孔融让梨了，不过此处的梨就是CPU、内存等等一些资源了，绿色池中的空间了之类的，突然发现程序也是那么的有人情味啊~

##loopingcall

接下来谈谈用loopingcall实现固定时间间隔运行的函数：

	def _wait_for_reboot():
	            state = self.get_info(instance)['state']
	
	            if state == power_state.RUNNING:
	                LOG.info(_("Instance rebooted successfully."),
	                         instance=instance)
	                raise loopingcall.LoopingCallDone()
	
	        timer = loopingcall.FixedIntervalLoopingCall(_wait_for_reboot)
	        timer.start(interval=0.5).wait()


这个函数是等待虚机重启的函数，每隔0.5s调用一次函数，检查虚机状态，直到虚机重新启动。此函数通过抛出LoopingCallDone来异常退出。

好了，对于虚机中的协同并发就到此结束了。

以上。

---
> **版权声明**
> **本人博文若无特别说明，均由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **由于本人水平有限，所以难免有错，若发现错误，请在评论区任意吐槽~**
> **博文链接：<http://voidy.net/>**