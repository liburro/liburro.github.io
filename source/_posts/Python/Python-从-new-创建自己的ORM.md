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
