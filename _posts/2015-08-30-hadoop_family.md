---
layout: post
title: "hadoop家族介绍"
description: 
modified: 2015-08-30
category: Hadoop
tags: [Hadoop,大数据]
comments: true
mathjax: 
---
{% highlight linenos %}
{% endhighlight %}
##简介
Apache Hadoop是一款支持数据密集型分布式应用并以Apache 2.0许可协议发布的开源软件框架。它支持在商品硬件构建的大型集群上运行的应用程序。Hadoop是根据Google公司发表的MapReduce和Google档案系统的论文自行实作而成。

Hadoop框架透明地为应用提供可靠性和数据移动。它实现了名为MapReduce的编程范式：应用程序被分割成许多小部分，而每个部分都能在集群中的任意节点上执行或重新执行。此外，Hadoop还提供了分布式文件系统，用以存储所有计算节点的数据，这为整个集群带来了非常高的带宽。MapReduce和分布式文件系统的设计，使得整个框架能够自动处理节点故障。它使应用程序与成千上万的独立计算的电脑和PB级的数据。

常用的项目包括Hadoop, Hive, Pig, HBase, Sqoop, Mahout, Zookeeper, Avro, Ambari, Chukwa，YARN, Hcatalog, Oozie, Cassandra, Hama, Whirr, Flume, Bigtop, Crunch, Hue等。

关系图如下：

![][21]

##各项目简介

* [Apache Hadoop][1]: 是Apache开源组织的一个分布式计算开源框架，提供了一个分布式文件系统子项目(HDFS)和支持MapReduce分布式计算的软件架构。
* [Apache Hive][2]: 是基于Hadoop的一个数据仓库工具，可以将结构化的数据文件映射为一张数据库表，通过类SQL语句快速实现简单的MapReduce统计，不必开发专门的MapReduce应用，十分适合数据仓库的统计分析。
* [Apache Pig][3]: 是一个基于Hadoop的大规模数据分析工具，它提供的SQL-LIKE语言叫Pig Latin，该语言的编译器会把类SQL的数据分析请求转换为一系列经过优化处理的MapReduce运算。
* [Apache HBase][4]: 是一个高可靠性、高性能、面向列、可伸缩的分布式存储系统，利用HBase技术可在廉价PC Server上搭建起大规模结构化存储集群。
* [Apache Sqoop][5]: 是一个用来将Hadoop和关系型数据库中的数据相互转移的工具，可以将一个关系型数据库（MySQL ,Oracle ,Postgres等）中的数据导进到Hadoop的HDFS中，也可以将HDFS的数据导进到关系型数据库中。
* [Apache Zookeeper][6]: 是一个为分布式应用所设计的分布的、开源的协调服务，它主要是用来解决分布式应用中经常遇到的一些数据管理问题，简化分布式应用协调及其管理的难度，提供高性能的分布式服务
* [Apache Mahout][7]:是基于Hadoop的机器学习和数据挖掘的一个分布式框架。Mahout用MapReduce实现了部分数据挖掘算法，解决了并行挖掘的问题。
* [Apache Cassandra][8]:是一套开源分布式NoSQL数据库系统。它最初由Facebook开发，用于储存简单格式数据，集Google BigTable的数据模型与Amazon Dynamo的完全分布式的架构于一身
* [Apache Avro][9]: 是一个数据序列化系统，设计用于支持数据密集型，大批量数据交换的应用。Avro是新的数据序列化格式与传输工具，将逐步取代Hadoop原有的IPC机制
* [Apache Ambari][10]: 是一种基于Web的工具，支持Hadoop集群的供应、管理和监控。
* [Apache Chukwa][11]: 是一个开源的用于监控大型分布式系统的数据收集系统，它可以将各种各样类型的数据收集成适合 Hadoop 处理的文件保存在 HDFS 中供 Hadoop 进行各种 MapReduce 操作。
* [Apache Hama][12]: 是一个基于HDFS的BSP（Bulk Synchronous Parallel)并行计算框架, Hama可用于包括图、矩阵和网络算法在内的大规模、大数据计算。
* [Apache Flume][13]: 是一个分布的、可靠的、高可用的海量日志聚合的系统，可用于日志数据收集，日志数据处理，日志数据传输。
* [Apache Giraph][14]: 是一个可伸缩的分布式迭代图处理系统， 基于Hadoop平台，灵感来自 BSP (bulk synchronous parallel) 和 Google 的 Pregel。
* [Apache Oozie][15]: 是一个工作流引擎服务器, 用于管理和协调运行在Hadoop平台上（HDFS、Pig和MapReduce）的任务。
* [Apache Crunch][16]: 是基于Google的FlumeJava库编写的Java库，用于创建MapReduce程序。与Hive，Pig类似，Crunch提供了用于实现如连接数据、执行聚合和排序记录等常见任务的模式库
* [Apache Whirr][17]: 是一套运行于云服务的类库（包括Hadoop），可提供高度的互补性。Whirr学支持Amazon EC2和Rackspace的服务。
* [Apache Bigtop][18]: 是一个对Hadoop及其周边生态进行打包，分发和测试的工具。
* [Apache HCatalog][19]: 是基于Hadoop的数据表和存储管理，实现中央的元数据和模式管理，跨越Hadoop和RDBMS，利用Pig和Hive提供关系视图。
* [Cloudera Hue][20]: 是一个基于WEB的监控和管理系统，实现对HDFS，MapReduce/YARN, HBase, Hive, Pig的web化操作和管理。



##参考资料

> * <http://blog.fens.me/hadoop-family-roadmap/>
> * <http://wiki.apache.org/hadoop/>
> * <https://zh.wikipedia.org/wiki/Apache_Hadoop>


[1]:http://hadoop.apache.org/
[2]:http://hive.apache.org/
[3]:http://pig.apache.org/
[4]:http://hbase.apache.org/
[5]:http://sqoop.apache.org/
[6]:http://zookeeper.apache.org/
[7]:http://mahout.apache.org/
[8]:http://cassandra.apache.org/
[9]:http://avro.apache.org/
[10]:http://incubator.apache.org/ambari/
[11]:http://incubator.apache.org/chukwa/
[12]:http://hama.apache.org/
[13]:http://flume.apache.org/
[14]:http://giraph.apache.org/
[15]:http://oozie.apache.org/
[16]:http://incubator.apache.org/crunch/
[17]:http://whirr.apache.org/
[18]:http://bigtop.apache.org/
[19]:http://incubator.apache.org/hcatalog/
[20]:http://cloudera.github.io/hue/
[21]:https://github.com/Voidly/Img/blob/master/blog/hadoopFamilyRoadmap.png?raw=true


