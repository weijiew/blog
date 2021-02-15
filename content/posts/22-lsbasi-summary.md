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

首先定义字符串变量，方便后续操作，含义就是字面意思。

```python
EOF, INTEGER, PLUS = 'EOF', 'INTEGER', 'PLUS'
```

## 1. Token 类

Token 类有两个属性，类型和对应的值。

> 例如 1 是整型，所以类型 type 为 INTEGER ，而值 VALUE 就是 1 。

那么构造函数如下：

```python
class Token(object):
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

完整代码：[part1](https://github.com/weijiew/lsbasi/blob/main/part1/calc1.py) 。

> 至此 PART 1 结束，建议打开 IDE 自己顺着逻辑写一遍，应该会存在一些问题，重点关注这些问题！

> 建议能够独立写出后再开启下一章，否则你依旧需要回头看，因为很多东西仅仅看一遍理解不到位！这些篇文章正是我在回头看时写下的 :) 。

# Part 2

之前的代码仅支持两个一位整数加法操作，和实际情况差别很大。接下来的功能一点点添加。

首先完善减法操作。这个逻辑其实非常简单！照着加法改就好了，可以自己想着实现一下再看答案！

## 1. 减法实现

首先定义减法的变量。

```python
EOF, PLUS, MINUS,INTEGER = 'EOF', 'PLUS', 'MINUS','INTEGER'
···

接下来修改 `expr` 方法的逻辑：

```python
    def expr(self):

        self.current_token = self.get_next_token()

        left = self.current_token
        self.eat(INTEGER)

        op = self.current_token
        self.eat(PLUS)

        right = self.current_token
        self.eat(INTEGER)

        return left.value + right.value
```

然后是 `get_next_token` 的逻辑：

```python
    def get_next_token(self):

        if self.pos > len(self.text) - 1:
            return Token(EOF,None)

        current_char = self.text[self.pos]

        if current_char.isdigit():
            self.pos += 1
            return Token(INTEGER,int(current_char))

        if current_char == '+':
            self.pos += 1
            return Token(PLUS, '+')

        self.error()
```

现在可以支持减法操作了，减法测试：

```python
calc2 > 5-5
0
calc2 > 5-10
4
calc2 > 9-4
5
calc2 > 9-3
6
calc2 > 9-10
8
```

## 2. 跳过空格

上面的代码目前无法识别空格，接下来需要考虑跳过空格。

在看答案之前我也实现过处理空格的逻辑，修改 get_next_token 方法即可，具体方式如下：

```python
    def get_next_token(self):
        text = self.text

        if self.pos > len(text) - 1:
            return self.error()

        current_char = text[self.pos]

        while current_char == ' ':
            self.pos += 1
            current_char = self.text[self.pos]

        if current_char.isdigit():
            self.pos += 1
            return Token(INTEGER, int(current_char))

        if current_char == '+':
            self.pos += 1
            return Token(PLUS,'+')

        if current_char == '-':
            self.pos += 1
            return Token(MINUS, '-')

        self.error()
```

测试如下：

```python
calc2 > 5 + 5
10
calc2 > 5 - 5
0
calc2 >  5 + 5
10
```

但是答案的实现方式就很优雅！其实就是我写的循环抽象出来写成函数。

`skip_whitespace` 方法实现了越过空格，其中调用了 advace 方法，而该方法本质上就是递增 pos ，只不过做了越界处理。

```python
    def skip_whitespace(self):
        while self.current_char is not None and self.current_char.isspace():
            self.advance()

    def advance(self):
        self.pos += 1
        if self.pos > len(self.text) - 1:
            self.current_char = None
        else:
            self.current_char = self.text[self.pos]
```

看了上面的代码会发现 `self.current_char` 变成了成员变量，之前是卸载 get_net_token 方法中的局部变量。

因为如果还是局部变量需要传参，多个函数都要用，索性改成成员变量。将 current_char 放入成员变量中。

```python
class Interpreter(object):
    def __init__(self, text):
        self.text = text
        self.pos = 0
        self.current_token = None
        self.current_char = self.text[self.pos]
```

接下来就是修改 get_next_token 了：

```python
    def get_next_token(self):

        while self.current_char is not None:

            # 先处理空格逻辑
            if self.current_char.isspace():
                self.skip_whitespace()
                continue

            if self.current_char.isdigit():
                return Token(INTEGER, self.integer())

            if self.current_char == '+':
                self.advance()
                return Token(PLUS,'+')

            if self.current_char == '-':
                self.advance()
                return Token(MINUS, '-')

            self.error()

        return Token(EOF, None)
```

完整代码可参考：[calc2.py](https://github.com/weijiew/lsbasi/blob/main/part2/calc2.py)

> 以上就能够处理多位整数加减法，跳过空格的功能。
> 但是目前只能处理两个整数加减法，无法处理多个整数加减法！例如 1 + 2 + 3 + 4 。
> 其实接下来就是在 expr 函数上做文章，这个逻辑是写死的，很不友好。


# Part 3

支持多个多位整数加减法。

看下边的例子，其实是有规律的，每次增加的都是一个符号和一个整数。

> 1 + 2
> 1 + 2 + 3
> 1 + 2 + 3 + 4

所以将 expr 函数改成循环即可！

```python
    def expr(self):

        self.current_token = self.get_next_token()
        result = self.current_token.value
        self.eat(INTEGER)

        while self.current_token.type in (PLUS, MINUS):
            op = self.current_token
            if op.type == PLUS:
                self.eat(PLUS)
                result = result + self.current_token.value
                self.eat(INTEGER)
            elif op.type == MINUS:
                self.eat(MINUS)
                result = result - self.current_token.value
                self.eat(INTEGER)

        return result
```

答案的将一些冗余操作封装成函数了，比较简洁。

```python
    def term(self):
        token = self.current_token
        self.eat(INTEGER)
        return token.value

    def expr(self):

        self.current_token = self.get_next_token()
        result = self.term()

        while self.current_token.type in (PLUS, MINUS):
            op = self.current_token
            if op.type == PLUS:
                self.eat(PLUS)
                result = result + self.term()
            elif op.type == MINUS:
                self.eat(MINUS)
                result = result - self.term()

        return result
```

# Part 4

单纯的乘除是很简单的，将加减改为乘除即可！这个逻辑很简单。

到目前为止，Interpreter 类已经非常臃肿了。为了解决这个问题，创建了一个解析词法分析器类 lexer 。
该类要解决的问题就是讲字符串转换为 token 。

直接看代码吧，挺简单的，就是讲 lexer 剥离出来。

此时重新梳理一下！ lexer 将处理 token 的逻辑封装了起来。例如跳过空格，多位整数处理。从抽象的角度将我们只需要明白输出是 token 即可。而 Interpreter 则专注于处理乘除的逻辑。

这一节代码很简单，但是加减乘除混合就不简单了，因为存在优先级。下一节就是讲这个的！

# Part 5  

众所周知乘除的优先级大于加减。

语法规则如下：

$$
expr: term((PLUS|MINUS)term)*
$$

$$
term: facter((MUL|DIV)facter)*
$$

$$
facter: INTEGER
$$

这不是词法分析，而是后续要关注的事情，所以要写在 Interpreter 类中。

知晓这个语法规则后，增加一个 expr 函数即可。该函数的逻辑和 term 函数几乎一样，只不过是处理加减法。可以尝试着自己写，然后回头比对，很简单！

实现：[calc5.py](https://github.com/weijiew/lsbasi/blob/main/part5/calc5.py) 。


# Part 6

如何处理左右括号？例如 `(1 + 2) * (3 + 4)` ，之前因为没有处理括号的逻辑，所以执行起来真正的顺序是 `1 + 2 * 3 + 4` 。

将处理括号的语法加入规则中，更新后的语法规则如下：

$$
expr: term((PLUS|MINUS)term)*
$$

$$
term: facter((MUL|DIV)facter)*
$$

$$
facter: INTEGER | LPAREN expr RPAREN
$$

这个实现起来也很简单，照着语法规则改就行了。

实现：[calc6.py](https://github.com/weijiew/lsbasi/blob/main/part6/calc6.py) 。

# Part 7 

将语法树改为了 AST 。

getattr 三个参数 a,b,c  可以将 a 理解为对象，a.b 。 如果没有 b 就返回 c 。

输入一个简单的例子以 debug 一遍就明白了，例如 5 + 6 。

直接看代码吧：[spi.py](https://github.com/weijiew/lsbasi/blob/main/part7/spi.py)

# Part 8

增加了  `- -1` `1 - - 2` 的处理。

[part8](https://github.com/weijiew/lsbasi/blob/main/part8/spi.py) 。

重点是观察者模式，最好跑一遍代码，看明白后再看下一章。

我是在 win10 下用 pycharm 写代码的，但是 win10 安装 free pascal 后出现了不兼容的问题。
于是采用 wsl 来编译代码。也就是 Ubuntu 20.04 LTS ，装一个 Pascal 解释器就好了，一行命令搞定。
这点不重要，只是验证代码结果而已。

# Part 9 

我在写代码的时候感觉自己写的代码很乱，有必要看一下 Python 代码规范。

下面是我参考的资源：[python 代码规程](https://google.github.io/styleguide/pyguide.html) / [中文版](https://zh-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/python_style_rules/)

这一节看起来复杂，其实也很简单。增加 AST node 类型，修改 lexer 代码，将语法规则转换成代码即可。语法规则对应的图比较重要，建议多看几遍，理解清楚后再开始动手写。

# Part 10 

* 支持解析 Pascal header 。
* 支持解析变量声明。
* 采用 div 来做整数除法，采用 / 来做浮点数除法。
* 支持 Pascal 注释。

