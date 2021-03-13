---
title: '浅谈 HashMap'
date: 2021-03-12
published: true
slug: 24-hashmap
tags: ['Java']
cover_image: "./images/p25.jpg"
canonical_url: false
description: '又是一个实验报告。'
---

# lab Hash

在 JDK1.8 中 HashMap 采用的是数组加链表加红黑树。其中该类集成了很多其他类，有些复杂。根据思想我实现了一个阉割版的，参照 JDK1.8 进行了部分优化。主要阉割了红黑树和部分边界条件的判断。除此之外采用了 Junit 做单元测试，提供单元测试代码。

# 结果

节点用 Node 表示。第一列是存于数组之中的 Node 类。`->` 表示链表。

```
0 : weijiew34,a134> ->  <weijiew78,a178> ->  <weijiew34,a134> ->  <weijiew89,a189>
1 : weijiew88,a188>
2 : weijiew47,a147> ->  <weijiew69,a169>
3 : weijiew13,a113>
4 : weijiew85,a185> ->  <weijiew63,a163> ->  <weijiew96,a196>
5 : weijiew73,a173> ->  <weijiew62,a162> ->  <weijiew62,a162>
6 : weijiew54,a154>
7 : NULL
8 : weijiew92,a192>
9 : NULL
10 : NULL
11 : NULL
12 : NULL
13 : weijiew4,a14> ->  <weijiew48,a148>
14 : NULL
15 : weijiew6,a16> ->  <weijiew17,a117>
```

# code

```java
package com.weijiew.myHashMap;

/**
 * @program: myHashMap
 * @description:
 * @author: jie wei
 * @create: 2021-03-12 15:20
 */
public class myHashMap {

    public Node[] table = new Node[16];;

    // 扰动函数，该函数的目的是为了减少哈希冲突
    public int myHash(String var0) {
        // 拿到 HashCode
        int var1 = myHashCode(var0);

        // 移位运算，分布均匀。
        int var2 = var1 ^ (var1 >>> 16);
        return var0 == null ? 0 : var2;
    }

    public int myHashCode(String value) {
        int var1 = 0;
        if (value.length() > 0) {
            for (int i = 0; i < value.length(); i++){
                var1 = 31 * var1 + value.charAt(i);
            }
        }
        return var1;
    }

    // 根据哈希之计算下标并插入
    public void put(String key,String value) {
        int h = myHash(key);
        int index = h & (16 - 1);
        Node first, e;
        Node node = new Node(h, key,value);
        if (table[index] == null) {
            table[index] = node;
        }else {
            e = table[index];
            while (e.next != null) {
                e = e.next;
            }
            e.next = node;
        }
    }

    public void PrintInfo() {
        for (int i = 0; i < 16; i++) {
            if (table[i] == null) {
                System.out.println(i + " : NULL");
            }else {
                Node t = table[i];
                System.out.printf( i + " : " + t.key + "," + t.value + ">");
                while (t.next != null) {
                    t = t.next;
                    System.out.printf(  " ->  <" + t.key + "," + t.value + ">");
                }
                System.out.println("");
            }
        }
    }
}
```

辅助类 Node 节点。

```java
package com.weijiew.myHashMap;

/**
 * @program: myHashMap
 * @description:
 * @author: jie wei
 * @create: 2021-03-12 20:00
 */
public class Node {
    final int hash;
    final String key;
    final String value;
    Node next;

    public Node(int hash, String key, String value) {
        this.hash = hash;
        this.key = key;
        this.value = value;
    }

}
```

测试类：

```java
package com.weijiew.test;

import com.weijiew.myHashMap.myHashMap;
import org.junit.Assert;
import org.junit.Test;

import java.util.HashMap;
import java.util.Random;

/**
 * @program: MainTest
 * @description: 测试类
 * @author: jie wei
 * @create: 2021-03-12 15:22
 */
public class MainTest {

    @Test
    public void myHashMapTest() {
        HashMap a = new HashMap();
        myHashMap b = new myHashMap();
        Assert.assertEquals("weijiew".hashCode(), b.myHashCode("weijiew"));
    }

    @Test
    public void myHashPutTest() {
        myHashMap b = new myHashMap();
        Random rand = new Random(1);
        for (int i = 0; i < 20; i++) {
            int r = rand.nextInt(100);
            b.put("weijiew" + r,"a1" + r);
        }
        b.PrintInfo();
    }
}
```

# 总结

以下是关于本实验的一些思考与总结。

## HashCode

在 JDK 1.8 中 HashCode 方法如下：

```java
    public int hashCode() {
        int var1 = this.hash;
        if (var1 == 0 && this.value.length > 0) {
            char[] var2 = this.value;

            for(int var3 = 0; var3 < this.value.length; ++var3) {
                var1 = 31 * var1 + var2[var3];
            }

            this.hash = var1;
        }

        return var1;
    }
```

我们只需要关注 for 循环内部的代码即可。

以字符串 “weijiew“ 为例。根据 ASCII 编码，强制转换为整形后结果为：119 101 105 106 105 101 119 .

那么该方法中循环的实际计算流程如下：

$$
var1 = ((((((((31\times 0 + 119)119 + 101)101 + 105)105 + 106)106 + 105)105 + 101)101 + 119)119) = 1230531596
$$

为什么是 31 ？ 这个问题在 《Effective Java》 第 42 页有回答。

简而言之，31 是一个奇素数。如果是偶数的话，以 2 为例。相乘的结果等价于该数字整体左移，低位补零，而承载信息的数字随着乘法的叠加都移向了高位，而乘法溢出会导致高位数字的信息丢失。低位数字全都是 0 。偶数都有类似的效应。

其次是 31 便于优化， `31 * i == (i << 5） - i`。也就是 31 通过位运算和减法实现乘法操作，大大提高的效率。JVM 可以自动实现改优化。

> “编写这种散列函数是个研究课题，最好留给数学家和理论方面的计算机科学家来完成。我们此次最重要的是知道了为什么使用31。” --《Effective Java》

我的实现，少了边界判断。

```java
    public int myHashCode(String value) {
        int var1 = 0;
        if (value.length() > 0) {
            for (int i = 0; i < value.length(); i++){
                var1 = 31 * var1 + value.charAt(i);
            }
        }
        return var1;
    }
```

## Hash

计算好 HashCode 后，

下面是扰动函数。因为后续要根据 hashcode 来计算数组的 index 。而下面的函数目的是为了 index 分散的更为均匀。

```java
    static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
```

没有查到为什么这样写有效。我的理解是通过位运算使得数据特征分散开来，降低“密度”。

## index

`int index = myHash(key) & (16 - 1);` 

这是计算 index 的代码，根据扰动函数计算出来的记过同 15 进行 `&` 运算。通过位运算实现了取余操作，将范围限定到了 0 - 16 之间。`a % b == (b-1) & a` 当 b 为 2 的指数之时该等式成立。