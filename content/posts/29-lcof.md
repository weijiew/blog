---
title: '剑指 offer 记录贴'
date: 2021-05-19
published: false
slug: 29-lcof
tags: ['算法']
cover_image: "./images/p29.jpg"
canonical_url: false
description: '剑指 offer 刷题记录'
---

# 1. 闰年判断

判断闰年方法：是4的倍数但不是100的倍数，或者是400的倍数。

闰年分为世纪闰年和普通闰年，前者是 400 的整数倍，后者是 4 的整数倍但不是 100 的整数倍。

`if((y % 400 == 0) || (y % 4 == 0 && y % 100 != 0))`


# 2. Java 高精度

```java
        Scanner sc = new Scanner(System.in);
        System.out.print(sc.nextBigInteger().add(sc.nextBigInteger()));
```


http://lx.lanqiao.cn/problem.page?gpid=T7

* 这个网站可以刷蓝桥杯的 VIP 题： https://acmore.cc 
* 代码参考： https://github.com/liuchuo/Lanqiao


```java

public class Main {

    public static void main(String[] args) {
        int sum = 0;
        for(int i = 1; i <= 2020; i++){
            sum += is2Num(i);
        }
        System.out.println(sum);
    }

    private static int is2Num(int i) {
        int k = 0;
        while(i != 0){
            if(i%10 == 2){
                k++;
            }
            i /= 10;
        }
        return k;
    }
}
```

