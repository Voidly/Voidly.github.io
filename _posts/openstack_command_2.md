title: Openstack常用命令总结(续)
date: 2015-01-09 18:08:14
tags: [Openstack, Openstack command-line]
categories: [cloud computing]
---

上次总结了一些Openstack的常用命令以及Glance相关的常用命令，这次接着上次的总结一下Nova以及Neutron的命令行指令。

##Nova常用指令

**nova usage**

	usage: nova [--version] [--debug] [--os-cache] [--timings]
                [--timeout <seconds>] [--os-auth-token OS_AUTH_TOKEN]
                [--os-username <auth-user-name>] [--os-user-id <auth-user-id>]
                [--os-password <auth-password>]
                [--os-tenant-name <auth-tenant-name>]
                [--os-tenant-id <auth-tenant-id>] [--os-auth-url <auth-url>]
                [--os-region-name <region-name>] [--os-auth-system <auth-system>]
                [--service-type <service-type>] [--service-name <service-name>]
                [--volume-service-name <volume-service-name>]
                [--endpoint-type <endpoint-type>]
                [--os-compute-api-version <compute-api-ver>]
                [--os-cacert <ca-certificate>] [--insecure]
                [--bypass-url <bypass-url>]
                <subcommand> ...


**boot**

启动一个虚机。

**delete**

立刻关机并删掉指定机器。

**live-migration**

热迁移。

**lock**

锁定虚机。

**migrate**

冷迁移，目的机器由系统调度。

**pause**

暂停虚机。

**reboot**

重启虚机。

**rebuild**

关机，重新根据镜像启动虚机。

**show**

显示指定虚机详细信息。

**ssh**

SSH到虚机。

**start**

打开虚机。

**stop**

停止虚机。

**suspend**

挂起虚机。

**unlock**

解除虚机锁定。

**unpause**

解除虚机暂停。



##Keystone常用指令

        usage: keystone [--version] [--debug] [--os-username <auth-user-name>]
                        [--os-password <auth-password>]
                        [--os-tenant-name <auth-tenant-name>]
                        [--os-tenant-id <tenant-id>] [--os-auth-url <auth-url>]
                        [--os-region-name <region-name>]
                        [--os-identity-api-version <identity-api-version>]
                        [--os-token <service-token>]
                        [--os-endpoint <service-endpoint>] [--os-cache]
                        [--force-new-token] [--stale-duration <seconds>] [--insecure]
                        [--os-cacert <ca-certificate>] [--os-cert <certificate>]
                        [--os-key <key>] [--timeout <seconds>]
                        <subcommand> ...


**Subcommands**

**endpoint-create**

创建新的关联服务的endpoint。

**endpoint-delete**

删除服务的endpoint。

**endpoint-get**

通过指定的属性或服务类型找到endpoint。


**endpoint-list**

列出配置的服务endpoint。

**tenant-create**

创建新的tenant。

**tenant-delete**

删除tenant。

**tenant-get**

显示tenant详细信息。

**tenant-list**

列出所有tenant。

**tenant-update**

编辑tenant的名字，描述，可能的状态。

**token-get**

显示当前用户的token。

**user-create**

创建新用户。

**user-delete**

删除用户。

**user-get**

显示用户详细信息。

**user-list**

显示用户。

