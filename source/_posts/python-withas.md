---
title: '[python]-with as 语句'
date: 2018-05-02 11:40:04
tags:
categories: python
---

本文主要讲解了 with as 语句的使用，就是python里面上下文管理器的一种概念(context manager)。

<!--more-->

官方链接： 
https://docs.python.org/2/reference/datamodel.html#context-managers
https://docs.python.org/2/reference/compound_stmts.html#the-with-statement

我们知道平时打开文件进行操作使用如下语句，可以在任何时候都关闭掉打开的文件：

``` python
with open('test.txt', 'rb') as f:
    f.do_something()
```

其实我们也可以创建自己的上下文管理器来完成相应的动作，在做完或者中间有异常的情况下，我们设定的代码都一定会执行。
我们只需要定义一个类，然后创建两个函数，格式如下：

``` python
class testwith(object):
    def __init__(self): pass
    
    def __enter__(self):
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        print 'exit'
    
    def fun(self):
        print 'fun'

with testwith() as p:
    p.fun()
```

只要定义了`__enter__`和`__exit__`两个函数，那么就可以使用`with as`语句了，执行顺序如下：
1. 先执行`__init__`
2. 执行`__enter__`，返回的对象赋值给`with as`语句as后面的变量
3. 执行`with as`的代码块(也就是执行p.fun())
4. 执行`__enter__`，这个函数必须是4个参数，`self`是固定的，`exc_type`是异常类型，`exc_value`是异常实例对象，`traceback`是异常跟踪对象，如果没有异常发生，后面三个参数都是`None`。

__注意__: 如果在未执行代码块(也就是p.fun())以前发生了任何异常，是不会执行`__exit__`的，也就是说`__init__`或者`__enter__`里面发生了异常，是不会执行到`__exit__`里面的

__注意__: 默认情况下`__exit__`返回的是`None`，这个时候，异常是会继续抛出的，如果想出现异常不抛出，这个函数需要显示的返回真值，比如`True`。
