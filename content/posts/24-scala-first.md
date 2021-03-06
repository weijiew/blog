---
title: 'Arch 安装后的思考'
date: 2021-03-05
published: true
slug: 24-scala-first
tags: ['Scala','大数据']
cover_image: "./images/p24.jpg"
canonical_url: false
description: 'Scala 学习'
---

```note
最近要学 Scala ，这门语言好像很厉害。正好发现了一门课。有 Java 基于

[【大数据教程】scala入门到精通](https://www.bilibili.com/video/BV1DK4y1s7bm?p=48)
```

# 1. 和 Java 的区别

Scala 程序的执行流程和 Java 类似，区别在于不仅可以调用 Java 库，自身还有一套库，最终都是跑在 JVM 上。

# 2. var val

var 可以理解为变量，初始化后可以随便修改。

val 可以理解为常量，初始化后不能被修改。

如果不清楚后续到底是否被修改，建议优先使用 val 。

# 3. 惰性赋值

如果字符串比较大，直接加载到内存中会比较耗费资源。

这个问题可以采用 lazy 关键字优化。也就是不直接加载，直到需要用到的时候才加载。

```scala
scala> lazy val a = """sql ....."""
lazy val a: String // unevaluated

scala> val a = """sql ....."""
val a: String = sql .....
```

# 4. 数据类型

Scala 数据类型首字母要大写，是强类型语言。

数据类型分为数值类型和引用类型。前者例如 Double，Boolean，Unit 等。后者例如 String，数组，对象类型。

注意 Unit 类型与 void ，当不知道函数的返回类型时使用。

null 类型是所有引用类型的子类。而 nothing 是数值类型和引用类型的子类，一般用于异常。

当类型混合之时，最终结果会变为数据范围大的类型，也就是类型提升。看下面的例子，3 + 3.2 的最终结果是 6.2 是 Double 类型，而写成 Int 就报错了。

```scala
scala> val a:Int = 3 + 3.2
                   ^
       error: type mismatch;
        found   : Int(3)
        required: ?{def +(x$1: ? >: Double(3.2)): ?}
       Note that implicit conversions are not applicable because they are ambiguous:
        both method int2float in object Int of type (x: Int): Float
        and method int2long in object Int of type (x: Int): Long
        are possible conversion functions from Int(3) to ?{def +(x$1: ? >: Double(3.2)): ?}
                     ^
       error: overloaded method + with alternatives:
         (x: Int)Int <and>
         (x: Char)Int <and>
         (x: Short)Int <and>
         (x: Byte)Int
        cannot be applied to (Double)

scala> val a:Double = 3 + 3.2
val a: Double = 6.2
```


# 5. 比较

`a == b` 比较的是内容， `a.eq(b)` 比较的是地址。这点和 Java 相反！

# 6. 守卫

for 循环和 if 表达式结合。

```scala
scala> for (i <- 1 to 10 if (i % 3 == 0)) {println("Hell" + i)}
Hell3
Hell6
Hell9
```

```scala
scala> val num = for (i <- 1 to 10) yield i * 10
val num: IndexedSeq[Int] = Vector(10, 20, 30, 40, 50, 60, 70, 80, 90, 100)
```

# 7. 惰性方法

Java 可以通过懒汉式来实现。而 lazy 则是 scala 的原生关键字。

lazy 不能修饰 var 变量，因为 var 变量可被修改，没有意义。


https://www.bilibili.com/video/BV1DK4y1s7bm?p=52 

截止到此处看了俩小时。很多东西其实可以直接跳过，主要看和 Java 的区别即可！
