---
layout: post
title: "hive学习笔记"
description: 
modified: 2016-02-05
category: 
tags: []
comments: true
mathjax: 
---
{% highlight linenos %}
{% endhighlight %}

- list bucket table

    - 是一种表，在建表时候需要指定数据倾斜的字段的值，这样在写入数据的时候会单独针对这个倾斜字段建立一个子目录，查询的时候会快一些

- 将SQL查询转换为一系列在Hadoop集群上运行的MR作业

- 使用-S参数可以在启动程序时不显示运行时的信息打印输出到标准错误输出
- hive表存储地址

    - 由hive.metastore.warehouse.dir控制，默认是/usr/hive/warehouse/table_name

- 配置Hive

    - 在conf目录下
        - hive-site.xml：设置每次运行Hive时希望Hive使用的选项

            - 可以使用hive --config   /usr/dev/hive-conf目录重新定义Hive配置文件
        - hive-default.xml：记录Hive使用的选项和默认值
    - Hive属性优先级
        - Hive SET命令

        - 命令行-config选项
        - hive-site.xml
        - hive-default.xml
        - hadoop-site.xml或等价的core-site.xml、hdfs-site.xml、mapred-site.xml
        - hadoop-default.xml或core-default.xml、hafs-default.xml、mapred-default.xml
- Hive日志
    - /tmp/$USER/hive.log

    - 配置文件：
        - conf/hive-log4j.properties

    - 在会话中配置，输出到控制台
        - hive -hiveconf hvie.root.logger=DEBUG,console
- Hive服务
    - cli

        - 命令行接口
    - hiveserver

        - 让Hive以提供Thrift服务的服务器形式运行，允许用不同的语言编写的客户端进行访问。使用Thrift、JDBC和ODBC连接器的客户端需要运行Hive服务器来和Hive进行通信，
    - hwi
        - Hive的Web接口
    - jar

        - 与hadoop jar等价的hvie接口
    - metastore

        - 是Hive元数据的集中存放地，包含两部分

            - 服务

            - 后台数据的存储

        - 默认情况下，metastore和Hive服务运行在同一个进程里，使用这个服务，可以让metastore作为一个单独的进行运行

        - 此服务与Hive服务运行在同一个JVM中，包含一个内嵌的以本地磁盘作为存储的Derby服务运行

- 和传统数据库比较

    - 读时模式 vs. 写时模式

        - 传统数据库是在数据加载时强制确定表的模式
        - hive是在查询时对数据进行验证

        - 读时模式：数据加载迅速

        - 写时模式：查询性能高

    - 托管表 vs. 外部表

        - 托管表，创建即是文件移动，删除会将元数据、数据都删掉

        - 外部表创建时需要声明位置，删除后仅仅删除了元数据

    - 分区和桶
        - Hive将表组织成分区，表分区可以进一步分为“桶”

        - 可以使用show partitions 表名;来查看有哪些分区

        - 将分区/表组织成桶的好处

            - 获得更高的查询处理效率

            - 使“取样”更高效

        - 划分桶的方法

            - 使用CLUSTERED BY子句
                - CREATE TABLE xx (id INT, name STRING) CLUSTERED BY (id) INTO 4 BUCKETS;

                - 使用id来确定如何划分桶(hive 使用对值进行哈希并将结果除以桶的个数取余数)

            - 声明一个表使其使用桶排序
                - CREATE TABLE xx (id INT, name STRING) CLUSTERED BY (id) SORTED BY (id ASC) INTO 4 BUCKETS;

        - 对数据进行取样

            - 取四分之一的数据
            - SELECT * FROM xxxx TABLESAMPLE(BUCKET 1 OUT OF 4 ON id);

            - 取一半的数据
            - SELECT * FROM xxxx TABLESAMPLE(BUCKET 1 OUT OF 2 ON id);

    - 存储格式
        - Hive从两个维度对表存储进行管理，“行格式”和“文件格式”
        - “行格式”由SerDe定义，
            - SerDe是“序列化反序列化工具”(Serializer-Deserializer)

            - 查询时
                - SerDe将文件中字节形式的数据行反序列化为Hive内部操作数据行时所使用的对象形式

            - 执行INSERT或CTAS时
                - SerDe会把Hive数据行内部表示形式序列化成字节形式并写入到输出文件中

        - 默认存储格式

            - 按行存储，行内分隔符为Control-A（ASCII码为1）：因为出现在字段文本中可能性比较小

            - 集合元素默认分隔符是Control-B，分隔ARRAY或STRUCT的元素或MAP的键/值对

            - 映射键分隔符是Control-C
            - Hive支持六级分隔符，分别对应ASCII码1~8

            - 二进制SerDe不应该和默认的TEXTFILE格式(或显示的STORED AS TEXTFILE子句)一起使用。二进制数据行总会包含换行符，这会导致Hive把行截断

        - 二进制存储格式：顺序文件和RCFile

        - 删数据但是保留表定义

            - 删除数据文件
                - dfs -rmr xxx

            - 使用like创建新表
                - create table xxx like xxx；

    - 连接

        - 内连接
            - SELECT A.*, B.* FROM A JOIN B ON (A.id = B.id);

        - 查看Hive对某个查询起了多少MR
            - EXPLAIN ...

        - 外连接

            - 将JOIN改为LEFT OUTER JOIN或RIGHT OUTER JOIN或者FULL OUTER JOIN

- 用户自定义函数
    - UDF

        - 操作作用于单个数据行，且产生一个数据行作为输出

        - 满足条件

            - 必须是org.apache.hadoop.hive.ql.exec.UDF的子类

            - 必须至少实现evaluate()方法
    - UDAF(用户定义聚集函数)

        - 接受多个输入数据行，且产生一个输出数据行
    - UDTF(用户定义表生成函数)

        - 操作作用于单个数据行，且产生多个数据行-一个表作为输出