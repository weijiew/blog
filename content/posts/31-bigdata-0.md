---
title: '大数据项目开发记录'
date: 2021-05-26
published: false
slug: 31-bigdata-0.md
tags: ['系统编程','CS:APP']
cover_image: "./images/p30.jpg"
canonical_url: false
description: '顶级索引，所有内容的总结。'
---

2021-5-26 环境搭建，Hadoop 平台，三个虚拟机之间互相 ping 同。


bin 目录存储 hdfs yarn 相关的一些命令。

etc 配置信息。

本地运行一个 wordcount 案例：

`[weijiew@hadoop102 hadoop-3.1.3]$ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.1.3.jar wordcount wcinput ./wcoutput`

