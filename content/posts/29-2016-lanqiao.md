---
title: '2016 蓝桥杯国赛题目'
date: 2021-05-21
published: false
slug: 29-lcof
tags: ['算法']
cover_image: "./images/p29.jpg"
canonical_url: false
description: '剑指 offer 刷题记录'
---

这个链接里面有真题：https://www.lanqiao.cn/contests/?category_id=3 

# 1. 一步之遥

[一步之遥](https://www.lanqiao.cn/problems/652/learning/)

想到公式 `97x - 127y = 1` 后这道题就简单了，暴力枚举即可。

```cpp
#include <iostream>
using namespace std;

int main() {
    int k = 600;
    for (int i = 0; i < 600; i++) {
        for (int j = 0; j < 600; j++) {
            if ((97*i - 127*j) == 1) {
                k = min(k , i + j);
            }
        }
    }
    cout << k << endl;
    return 0;
}
```

如果数据大的话就不能用这个解法了，除此之外还有一个解法，根据欧几里得及其扩展原理。

# 凑平方数

https://www.lanqiao.cn/problems/653/learning/


# 算法很美代码记录

## 位运算

https://www.bilibili.com/video/BV17U4y1s777?p=3

### 1. 寻找重复数字

利用 `a^a = 0` 和 `a^0 = a` 的性质构造重复数字。因为数据范围是 1000 所以可以构造。

也可以利用哈希的思想，因为数据范围是 1000 。

```java
public class Main {

    public static void main(String[] args) {
        int N = 11;
        int[] arr = new int[N];
        for (int i = 0; i < N - 1; i++) {
            arr[i] = i + 1;
        }
        arr[N - 1] = 2;
        System.out.println();
        int x = 0;

        for (int i = 1; i <= N - 1; i++) {
            x ^= i;
        }

        for (int i = 0; i < N; i++) {
            x ^= arr[i];
        }
        System.out.println(x);
    }
}
```

### 2. 统计二进制 1 的个数。

https://www.bilibili.com/video/BV17U4y1s777?p=5

移位运算，逻辑移位和算术移位是区别：前者不论有无符号高位都补零，后者在有符号的情况下高位补 1 .在 Java 中前者是 >>> .

```java
public class Main {

    public static void main(String[] args) {
        int a = 35;
        System.out.println(Integer.toString(a , 2));
        int cnt = 0;
        // 左移 1
        for (int i = 0; i < 32; i++) {
            if ((a & (1<<i)) == (1 << i)) {
                cnt++;
            }
        }
        System.out.println(cnt);
        cnt = 0;
        // 右移 a
        for (int i = 0; i < 32; i++) {
            if (((a>>>i)&1) == 1) {
                cnt++;
            }
        }
        System.out.println(cnt);
    }
}
```

### 3. 判断 2 的整数次方。

2 的整数次方的二进制有如下规律：

    2: 10
    4: 100
    8: 1000
    16: 10000
    32: 100000
    64: 1000000

也就高位是 1 ，后续全是 0 所以据此， 该数减一后最高位是 0 其余位全是 1 .& 运算后结果为 0 .

```java
public class Main {

    public static void main(String[] args) {
        for (int i = 2; i < 100; i++) {
            if (((i - 1)&i) == 0) {
                System.out.println(i + ": " + Integer.toString(i , 2));
            }
        }
    }
}
```

### 4. 整数奇偶位互换

https://www.bilibili.com/video/BV17U4y1s777?p=7

& 运算有一个特性两真才真，一假即假。也就是保留的特性，如果 0&a 那么 a 的信息将被抹除，而 1&a 那么 a 的信息将被保留。

```java
public class Main {

    public static void main(String[] args) {
        int a = 6;
        // 构造偶数位
        int a1 = a&0xaaaaaaaa;
        // 构造奇数位
        int a2 = a&0x55555555;
        System.out.println((a1>>1)^(a2<<1));
    }
}
```

### 6. 
https://www.bilibili.com/video/BV17U4y1s777?p=8