---
title: Python-从__new__创建自己的ORM
date: 2018-07-20 17:20:32
tags:
categories: python
---

ORM是Object Relational Mapping的简写，就是在操作数据库的时候，使用一个类来代表了一个表。而`__new__`是python里面一个很特殊的函数，本文也引入了`type, metaclass`等概念。

<!--more-->

## __new__

```
class A(object):
    def __init__(self):
        print("A:__init__")

    def __new__(cls, *args, **kwargs):
        print("A:__new__")
        return object.__new__(cls)


a = A()
print(type(a))
```

上面输出：

```
A:__new__
A:__init__
<class '__main__.A'>
```

### 说明

`__new__`需要返回的是一个实例，这个实例可以是其它类通过`__new__`返回的实例，比如`object.__new__(cls[,...])`，也可以是自己定义的一个实例，比如直接返回一个字符串，这个字符串是str的一个实例对象，其中`__new__`是一个静态函数，而且不需要使用`@static`等关键字说明，它一定是一个静态函数，第一个参数一定是当前类。

`__new__`是创造一个实例对象，`__init__`是定制化这个实例对象。

## 参考

### 参考链接

https://www.jianshu.com/p/b0f4d9c9afbb

### 笔记


