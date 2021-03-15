---
title: 'Java 集合框架'
date: 2021-03-15
published: true
slug: 26-java-data-structure
tags: ['Java']
cover_image: "./images/p26.jpg"
canonical_url: false
description: 'Java 集合框架总结'
---

# 1. 集合简介

集合用于存储对象，实现数组的功能。

数组长度固定而集合长度可变，数组既可以存基本类型也可以存引用类型，而集合只能存引用类型。

Collection 中含有 List 和 Set 两个接口。 List 的特点是有序，有下标，元素可重复。Set 则相反，无序，无下表，元素不重复。

List 中含有 ArrayList，Vector 和 LinkList 几个实现方式。

ArrayList 内部维护了一个数组，数组的查询快，增删慢。运行效率快，线程慢。 

注意无参构造出来的数组容量是 0 ，也就是数组中没有元素的时候容量为 0 。一旦添加元素，如果容量（size）小于 10 那么容量将变为 10 。反之如 size > 10 那么容量将变为 size 。数组扩容大小为原来的 1.5 倍。

Vector 和 ArrayList 性质类似，但是为了保证线程安全速度略低于 ArrayList 。

LinkList 内部维护了一个双向链表，增删快，查询慢，不带头结点。

Set 接口分为 HashSet 和 TreeSet 两种实现方式。

# 参考

1. [千锋教育-2020年最新版 Java集合框架详解 通俗易懂](https://www.bilibili.com/video/BV1zD4y1Q7Fw)

