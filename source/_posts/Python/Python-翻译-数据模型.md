---
title: Python-翻译-数据模型
date: 2018-07-23 10:00:26
tags:
categories: python
---

本文是翻译python官方的文档，原文档是：https://docs.python.org/3/reference/datamodel.html?highlight=metaclass

<!--more-->

## 对象，值和类型

Python里面的所有数据都被看成是对象，包括代码也是对象。

所有的对象都有一个identity，类型和值，其中identity在对象被创建后就不会在变化，你可以把它比作对象在内存中的地址。`is`操作符比较的是两个对象的identity，`id()`函数返回一个代表这个对象的整形的值。

__CPython实现细节：__ 对于CPython，`id(x)`是x这个对象在内存中的地址表示。

一个对象的类型决定了这个对象可以支持哪些操作(比如：是否有长度)。`type()`函数返回的是对象的类型，和identity一样，这个是不可变的。

在一些对象对应的值是可以改变的，这种情况我们称为_mutable_(可变)，如果被创建以后值就不能变化，我们叫做_immutable_(不可变)。(如果一个不可变的对象包含了某些可变的对象，这些可变对象是可以被修改的，我们仍然叫不可变对象，因为，相对来说，这个不可变对象仍然是没有被改变的)。

## 标准继承关系

### 可调用对象(Callable types)

#### 用户定义函数(User-defined functions)

就是我们常用的使用`def`定义的函数。这些函数有如下的一些属性：

`__doc__`: `None`或者函数的文档字符串
`__name__`: 函数名
`__module__`: 模块名
`__defaults__`: 一个元组包含参数的默认值或者`None`，注意只有值
`__kwdefaults__`: 一个字典，包含默认值的参数
`__code__`: code对象

#### 实例方法(Instance methods)

简单理解就是一个实例对象的方法，它和类关联，通常就是一个用户定义函数。

有如下几个特殊的属性：

`__self__`: 类实例对象
`__func__`: 函数对象
`__doc__`: 方法文档，类似`__func__.__doc__`
`__name__`： 方法名，类似`__func__.__name__`

当一个实例方法对象被创建，`__self__`被设置为实例对象，并且这个方法叫做被绑定的(bound method)，并且`__func__`被设置为初始函数对象。

举例1：

```
class A(object):
    pass
    
def f(first):
    pass
    
A.func = f

A().func.__func__ == f #True，其中f的第一个参数(first)会被(强制)传递为__self__
```


举例2：

```
class A(object):
    def f(self):
        pass
        
A().f #当我们调用的时候，实际是A.f(A())，在具体点就是A.f.__func__(A())
```

也就是说，在类里面定义的方法，实际上是类的一个属性(就和普通类变量一样)，通过`A.__dict__`可以看到类的属性，在默认情况下，所有的属性都是属于类的，也就是通过`instance.__dict__`其实是空的(除非有通过`self.xxx`设置过)，而`A.__dict__`是可以看见的。