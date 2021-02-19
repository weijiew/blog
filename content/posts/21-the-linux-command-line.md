---
title: '《The Linux Command Line》 阅读'
date: 2021-02-07
published: false
slug: 21-the-linux-command-line
tags: ['阅读']
cover_image: "./images/p21.jpg"
canonical_url: false
description: '读书笔记'
---

# 概述

偶然看到了这本书，一方面训练英语阅读，另一方面作为已有知识体系的补充。

这本书的应为名称为 《The Linux Command Line》 根据首字母缩写简称 TLCL 。

因为经常和 Linux 打交道，虽然学过这方面的知识，但在实践的时候总感觉缺点什么，认知不全面。也就是缺乏理论支撑，排查问题时不知所以然。综上我觉得有必要阅读这本书来补充认知。

# 资源

[《Linux命令行大全》](https://book.douban.com/subject/22226727/)

[中文翻译 TLCL](https://github.com/billie66/TLCL)

[在线版](http://billie66.github.io/TLCL/book/)

在线版阅读体验很好，有中英文对照。

# 总结

## 1. shell 和 terminal 的区别

shell 被用于和操作系统交互，将从键盘中输入的命令发送给操作系统。

terminal 也称终端，在终端中输入命令，shell 将命令发送给操作系统执行。

## 2. 程序安装目录

绝大多数的程序都安装在这个目录（`/usr/bin`）下 。

## 3. 软链接和硬链接

软链接可以理解为文件的别名，因为文件的版本一致在变化，如果文件名中包含了版本那么需要修改所有引用该版本的文件。

> 例如文件 foo-1 ，如果修改为 foo-2 ，那么所有引用 foo-1 的文件都要修改，显然很麻烦。
> 此时软链接的作用就体现出来了，创建软连接 foo 指向 foo-1 ，所有引用指向 foo 。当修改 foo-1 为 foo-2 时，只需要将 foo 指向 foo-2 即可。

http://billie66.github.io/TLCL/book/chap08.html

> 有时间再看，先暂停。