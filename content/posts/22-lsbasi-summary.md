---
title: '编译器'
date: 2021-02-13
published: false
slug: 22-lsbasi-summary
tags: ['编译器','总结']
cover_image: "./images/p22.jpg"
canonical_url: false
description: '动手写一个编译器！'
---

# Part 1

## 1. Token 类

Token 类有两个属性，类型和对应的值。

> 例如 1 是整型，所以类型 type 为 INTEGER ，而值 VALUE 就是 1 。

那么构造函数如下：

```python
    def __init__(self, type, value):
        self.type = type
        self.value = value
```

在此之前需要介绍一下魔法方法。类似于 `__xxxx__()` 这种类型的方法都被称作魔法方法。

更多魔法方法相关内容可查看：[英文版](https://rszalski.github.io/magicmethods/) / [中文版](https://pycoders-weekly-chinese.readthedocs.io/en/latest/issue6/a-guide-to-pythons-magic-methods.html) 。

当使用 print 输出对象的时候，只要自己定义了__str__(self) 方法，那么就会打印从在这个方法中return的数据

除此之外 Token 类中还有两个函数。目的是为了方便调试，打印对象信息。

```python
    def __str__(self):
        print("调用了 str 方法！")
        return 'Token({type}, {value})'.format(
            type=self.type,
            value=repr(self.value)
        )

    def __repr__(self):
        print("调用了 repr 方法！")
        return self.__str__()
```

区别：

__repr__ 目的是为了表示清楚，是为开发者准备的。

__str__ 目的是可读性好，是为使用者准备的。

__str__ 实际上调用了 __repr__ 。

具体可参考[Python的两个魔法方法：__repr__和__str__](https://blog.csdn.net/sinat_41104353/article/details/79254149)。


## 2. 主函数

我觉得在描述解释器类之前有必要知晓流程，也就是主函数 `main` 。

```python
def main():
    while True:
        text = input('cin> ')
        interpreter = Interpreter(text)
        result = interpreter.expr()
        print(result)

if __name__ == "__main__":
    main()
```

写成死循环是为了方便测试，输入一个结果输出一个结果。

首先输入字符串（text），根据字符串构造一个解释器（Interpreter）对象，然后调用成员方法来解析字符串内容。

字符串本质上是一个算术表达式。测试如下：

```python
cin> 5+6
10
cin> 6+6
12
cin> 7+7
14
cin> 
```

## 3. Interpreter

该类是主要流程，解决了将字符串转换为 Token 并计算其结果。

里面共有四个函数（`__init__`、`error`，`get_next_token`，`expr`），第一个是构造函数。

先看构造函数 `__init__` ，然后看 `expr` 知道调用顺序。

```python
class Interpreter():
    def __init__(self, text):
        # 存储完整字符
        self.text = text
        # 指向当前所扫描的字符
        self.pos = 0
        # 指向当前所拿到了 token
        self.current_token = None

    def error(self):
        return Exception("Error parsing input")

    # 返回值 Token ，将 pos 指向的值变为 Token
    def get_next_token(self):

        text = self.text

        if self.pos > len(text) - 1:
            return Token(EOF, None)

        current_char = self.text[self.pos]

        if current_char.isdigit():
            self.pos += 1
            return Token(INTEGER, int(current_char))

        if current_char == '+':
            self.pos += 1
            return Token(PLUS, '+')

        self.error()

    def expr(self):
        # 拿到当前扫描到的 Token 序列

        left = self.get_next_token()

        op = self.get_next_token()

        right = self.get_next_token()

        return left.value + right.value
```

到目前为止，expr 函数只能处理一位整数的加法操作

接下来开始进化！这个函数看起来很单薄，在该类中增加一个 eat 函数。
目的是为了验证当前函数类型并拿到下一个函数的 Token 。

函数变动如下：

```python
    def eat(self,token_type):

        if self.current_token.type == token_type:
            self.current_token = self.get_next_token()
        else:
            self.error()

    def expr(self):
        # 拿到当前扫描到的 Token 序列

        self.current_token = self.get_next_token()
        left = self.current_token
        self.eat(INTEGER)

        op = self.current_token
        self.eat(PLUS)

        right = self.current_token
        self.eat(INTEGER)

        return left.value + right.value

```

> 至此 PART 1 结束，建议打开 IDE 自己顺着逻辑写一遍，应该会存在一些问题，重点关注这些问题！

> 建议能够独立写出后再开启下一章，否则你依旧需要回头看，因为看一遍很多东西理解不到位！:)


# Part 2

