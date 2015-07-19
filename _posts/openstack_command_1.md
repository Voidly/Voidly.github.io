title: Openstack常用命令总结
date: 2015-01-08 18:11:13
tags: [Openstack,Openstack command-line]
categories: [cloud computing]
---

##	常用命令

### 1.查看版本号

	$ PROJECT --version

举个栗子：

	$ nova --version
	2.17.0

### 2.openstack服务相关

	$ openstack-service start

同理可知：重启是

	$ openstack-service restart

关闭：

	$ openstack-service stop

查看状态:

	$ openstack-status

那么问题来了，这是对于所有的服务进行的操作，如果想对单个的服务进行操作又该怎么办？
so easy!

举个栗子：

对nova里面的compute服务进行操作:

	$ service openstack-nova-compute stop
	Stopping openstack-nova-compute:                           [  OK  ]

只需如此这般即可，那么不知道名字怎么办？查看状态，然后找到对应服务的名字。

## glance相关命令

**glance usage**

	usage: glance [--version] [-d] [-v] [--get-schema] [--timeout TIMEOUT]
                  [--no-ssl-compression] [-f] [--os-image-url OS_IMAGE_URL]
                  [--os-image-api-version OS_IMAGE_API_VERSION] [-k]
              	  [--os-cert OS_CERT] [--cert-file OS_CERT] [--os-key OS_KEY]
                  [--key-file OS_KEY] [--os-cacert <ca-certificate-file>]
                  [--ca-file OS_CACERT] [--os-username OS_USERNAME]
                  [--os-user-id OS_USER_ID]
                  [--os-user-domain-id OS_USER_DOMAIN_ID]
                  [--os-user-domain-name OS_USER_DOMAIN_NAME]
                  [--os-project-id OS_PROJECT_ID]
                  [--os-project-name OS_PROJECT_NAME]
                  [--os-project-domain-id OS_PROJECT_DOMAIN_ID]
                  [--os-project-domain-name OS_PROJECT_DOMAIN_NAME]
                  [--os-password OS_PASSWORD] [--os-tenant-id OS_TENANT_ID]
                  [--os-tenant-name OS_TENANT_NAME] [--os-auth-url OS_AUTH_URL]
                  [--os-region-name OS_REGION_NAME]
                  [--os-auth-token OS_AUTH_TOKEN]
                  [--os-service-type OS_SERVICE_TYPE]
                  [--os-endpoint-type OS_ENDPOINT_TYPE]
                  <subcommand> ...

**Subcommands**

__image-create__

创建镜像。

**image-delete**

删除指定镜像。

**image-download**

下载指定镜像。

**image-list**

列出你能获取的所有镜像。

**image-show**

描述指定镜像。

**image-update**

编辑指定镜像。

下面介绍几个glance组件中常用的几个命令：

### 1. glance image-create

**参数**

	usage: glance image-create [--id <IMAGE_ID>] [--name <NAME>] [--store <STORE>]
                               [--disk-format <DISK_FORMAT>]
                           	   [--container-format <CONTAINER_FORMAT>]
                               [--owner <TENANT_ID>] [--size <SIZE>]
                               [--min-disk <DISK_GB>] [--min-ram <DISK_RAM>]
                               [--location <IMAGE_URL>] [--file <FILE>]
                               [--checksum <CHECKSUM>] [--copy-from <IMAGE_URL>]
                               [--is-public {True,False}]
                               [--is-protected {True,False}]
                               [--property <key=value>] [--human-readable]
                               [--progress]


举个栗子：

	glance image-create --name "cirros-0.3.3-x86_64" --file cirros-0.3.3-x86_64-disk.img \
  	--disk-format qcow2 --container-format bare --is-public True --progress

### 2.glance image-delete

	usage: glance image-delete <IMAGE> [<IMAGE> ...]

**<IMAGE\>**

	Name or ID of image(s) to delete.

###3.glance image-list

	usage: glance image-list [--name <NAME>] [--status <STATUS>]
                             [--container-format <CONTAINER_FORMAT>]
                             [--disk-format <DISK_FORMAT>] [--size-min <SIZE>]
                             [--size-max <SIZE>] [--property-filter <KEY=VALUE>]
                             [--page-size <SIZE>] [--human-readable]
                             [--sort-key {name,status,container_format,disk_format,size,id,created_at,updated_at}]
                             [--sort-dir {asc,desc}] [--is-public {True,False}]
                             [--owner <TENANT_ID>] [--all-tenants]



### 4.glance image-show
	usage: glance image-show [--human-readable] [--max-column-width <integer>]
                         <IMAGE>


### 5. glance image-update

	usage: glance image-update [--name <NAME>] [--disk-format <DISK_FORMAT>]
                               [--container-format <CONTAINER_FORMAT>]
                               [--owner <TENANT_ID>] [--size <SIZE>]
                               [--min-disk <DISK_GB>] [--min-ram <DISK_RAM>]
                               [--location <IMAGE_URL>] [--file <FILE>]
                               [--checksum <CHECKSUM>] [--copy-from <IMAGE_URL>]
                               [--is-public {True,False}]
                               [--is-protected {True,False}]
                               [--property <key=value>] [--purge-props]
                               [--human-readable] [--progress]
                               <IMAGE>

更多相关信息请移步至官网：[openstack command-line](http://docs.openstack.org/cli-reference/content/ch_preface.html)