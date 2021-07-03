# Spark 学习

https://www.bilibili.com/video/BV11A411L7CK

一次性数据计算：从存储设备读取数据然后进行逻辑运算最后将结果存入介质中。但是这种方式在处理复杂逻辑时性能很低，因为是基于底层 map 和 reduce 两个简单操作，那么为了解决复杂的逻辑在此之上需要进行很多的复杂操作。而每次计算的结果都要和磁盘进行交互所以效率很低。不适合迭代计算。

Spark 解决这个问题的方式是，不是通过和磁盘交互，而是将结果存入内存中，从而加快速度。

Spark 分为 Spark Core，Spark SQL，Spark Streaming，Spark MLlib，Spark GraphX 后四个都是基于 Spark Core。

## Word Count

大致逻辑为按行拆分，然后行内拆分，之后把相同的单词放到一起，然后统计相同单词的个数。