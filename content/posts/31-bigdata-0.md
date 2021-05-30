---
title: '大数据项目开发记录'
date: 2021-05-26
published: true
slug: 31-bigdata-0.md
tags: ['系统编程','CS:APP']
cover_image: "./images/p30.jpg"
canonical_url: false
description: '顶级索引，所有内容的总结。'
---

# 记录贴

先行而后知，这篇文章不侧重于细节，主要记录搭建过程中一些比较重要的知识。

## 5-26

环境搭建，Hadoop 平台，三个虚拟机之间互相 ping 同。

bin 目录存储 hdfs yarn 相关的一些命令。

etc 配置信息。

本地运行一个 wordcount 案例：

`[weijiew@hadoop102 hadoop-3.1.3]$ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.1.3.jar wordcount wcinput ./wcoutput`

跨机器拷贝文件：

`scp -r <源文件路径> <目标文件路径>`

-r 表示递归

rsync 远程同步，当文件发生变动时进行更新。

`rsync -av <源文件路径> <目标文件路径>`

## 5-30

etc/hadoop/ 路径下是 Hadoop 的配置文件。

集群崩溃后首先停止所有服务，`sbin/stop-dfs.sh` 。

其次删除每个集群上的 data 和 logs 文件夹。

然后格式化集群 `hdfs namenode -format`。

然后启动集群 `sbin/start-dfs.sh`。

根据服务器能够连接外网来判断是否需要时间同步，能连接的话不需要，不能连接的话需要。

硬件时钟要比软件模拟出来的时间更准确。

HDFS 适合一次写入多次读出的场景。通过增加副本来保证高容错性，适合大数据场景，适合多文件，可以构建在廉价的机器上。