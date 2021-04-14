---
title: 'CS:APP Lab 记录'
date: 2021-04-14
published: false
slug: 30-csapp-summary.md
tags: ['系统编程','CS:APP']
cover_image: "./images/p30.jpg"
canonical_url: false
description: '顶级索引，所有内容的总结。'
---

# 1. 引言

最近在学习计组，下面是我学习 CS:APP 这本书以及做实验过程的第一视角。如果觉得有帮助的话就给这个[仓库](https://github.com/weijiew/CSAPP-Labs)点个 star 吧。🤣

# 2. 阅读

如果不知道 CS:APP 是什么可以阅读知乎这个[问题](https://www.zhihu.com/question/20402534)，其中涵盖了一些学习方法和推荐的资源。

共 11 个 lab ，原始内容均来源于官方页面：http://csapp.cs.cmu.edu/3e/labs.html 。

当然，如果你觉得一个一个下载太麻烦的话，可以直接 clone 这个[仓库](https://github.com/weijiew/CSAPP-Labs)。我已经将所有的信息都做了备份，在 backup 目录中。

除此之外，这个[系列](https://www.zhihu.com/column/c_1325107476128473088)的文章对我的帮助也很大。

# 3. 环境搭建

我是在 win10 系统上做实验的，而实验环境要求 linux 平台，建议安装 docker 。除此之外 win10 可以选择使用 WSL ，这也完全可行。

为什么使用 docker ？

首先虚拟机笨重，docker 是单进程，轻量级。其次如果你不想在终端中编程的话，虚拟机需要重新安装 IDE ，例如 clion 或 vscode 之类的。而使用 docker 可以直接在 windows 下编程，使用 docker 编译。总之 docker 具备轻量和便捷的特点。

所以不论你是什么系统，安装好 docker 后，后续步骤都相同。

具体的环境搭建：https://cs.weijiew.com/book/csapp/lab0.html

# 4. 编程

