---
title: 'Java 集合 API 总结'
date: 2021-04-07
published: false
slug: 27-java-collective-api.md
tags: ['分布式']
cover_image: "./images/p27.jpg"
canonical_url: false
description: '刷题的时候频繁用到，做个总结吧。'
---

# 1. Queue

特性：先进先出。

```java
        Queue<String> queue = new LinkedList<String>();
```

## 1.1 添加和删除

* q.add() / q.offer()  二者均是添加元素，区别在于添加失败之时前者抛出异常，后者返回 null 。推荐后者。
* q.remove() / q.poll()  二者均是删除元素，区别同上，推荐后者。

## 1.2 查询队头元素

* q.element() / q.peek() 二者均是查询队头元素，区别同上。

# 2. Stack
