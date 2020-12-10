---
title: 'Missing Semester'
date: 2020-08-15
published: true
slug: missing-semester
tags: ['Course']
cover_image: "./images/p8.jpg"
canonical_url: false
description: '最近发现了一门课，这个课程的名称是缺失的课程，也就课堂上学不到的知识，是讲如何使用工具的。'
---

# 概述

花了三天时间将这门课学完了，受益匪浅。我的笔记：[Notes](https://weijiew.com/codestep/book/missing/ch0.html)。

学习这门课的目的在于有一个全局观，不是要求当前熟练应用，而是在开发过程中遇到了某些问题，知晓某个工具的存在可以解决当前的问题从而节省时间。

分为 11 个部分，部分之间没有很强的关联。每部分对应一节课，时长大约为一个小时。

我本来打算略过这门课，认为工具这类实践知识用到什么学什么就行了，
感觉没有必要专门去学。
但是现实却是知识点不成系统，
实际使用中出现问题后不知道如何排查。

后续又看到了这个评论以及之前的经历使得我重新有了动力去学这门课：

![动力](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/20200824145924.png)

视频有一些细节，文档上没有，关于这一点，文档也有说明。建议先阅读文档作为预习，然后再倍速浏览视频。

# 资源

* 原版网页和仓库地址：[missing-semester](https://missing.csail.mit.edu/) / [repo](https://github.com/missing-semester/missing-semester) 。
* 中文版网页和仓库地址：[missing-semester-cn](https://github.com/missing-semester-cn/missing-semester-cn.github.io) / [repo](https://missing-semester-cn.github.io/)
* B 站有一个机翻版的视频 ：[bilibili](https://www.bilibili.com/video/av86911412)

> 我这里无法访问网页，不知道是不是被墙了。我采取的措施是将仓库下载下来，直接查看文件内容。没有通过网页阅读。不过殊途同归。

# 环境

如果是 windows 平台的话建议使用 WSL 来体验 Linux 。
不建议使用虚拟机，仅安装配置就需要很久，我之前花了好几天才配好。
WSL 下载安装的话几分钟就好了。

我之前写过一篇 WSL 的配置指南，在我的博客里面，可以参考使用。
文章地址：https://weijiew.com/22/

# 总结

|       课程       |                     总结                     |
| :--------------: | :------------------------------------------: |
| 课程概览与shell  | 这一节主要是一些基础命令，和命令相关的概念。 |
| Shell 工具和脚本 |            脚本语法，函数的学习。            |
|   编辑器 (Vim)   |            实操性很强，日常使用。            |
|     数据整理     |            主要正则表达式的使用。            |
|    命令行环境    |            配置一个好看的命令行。            |
|  版本控制(Git)   |    版本控制，Github 建几个仓库尝试尝试。     |
|  调试及性能分析  |         使用技能，debug 常常会用到。         |
|      元编程      | 一些系统方面的知识，如何构建系统，模块依赖。 |
|   安全和密码学   | 对称加密，不对称加密，公钥私钥等，大致了解。 |
|      大杂烩      |           散乱的内容，md 语法等。            |
|    提问&回答     |                  一些问题。                  |
