---
title: '[python]-python3的使用'
date: 2018-06-07 08:14:57
tags:
categories: python
---

由于以后python2不在维护，所以准备基于python3开发新的自动化测试框架，以后python2的自动化测试会转移到python3上。

本文记录了一些使用python3的一些知识。

<!--more-->

## python2 与 python3 共用

由于在同一台pc上都装了python2和python3，那么这里专门对python3配置了虚拟环境。

具体使用参见: https://liburro.github.io/2018/04/27/py-virtualenv/

## python3与python2的不同

### 打印输出

首先`print`在python3中变成了函数，使用方式

``` python3
print("something")
```

``` bash
In [3]: print?
Docstring:
print(value, ..., sep=' ', end='\n', file=sys.stdout, flush=False)

Prints the values to a stream, or to sys.stdout by default.
Optional keyword arguments:
file:  a file-like object (stream); defaults to the current sys.stdout.
sep:   string inserted between values, default a space.
end:   string appended after the last value, default a newline.
flush: whether to forcibly flush the stream.
Type:      builtin_function_or_method

```

### Views 和 Iterators 代替 列表

1. 字典对象的几个函数，dict.keys() dict.items() dict.values()现在都返回视图对象，比如不支持下面的操作了，k = d.keys(); k.sort(),但是你可以使用k = sorted(d)来完成
2. 那么，dict.iterkeys(), dict.iteritems()　dict.itervalues()已经不支持了。
3. map() 和 filter() 现在返回的是iterator对象，如果你确实想要返回的是一个list对象，那么使用list()函数进行一个包装，比如list(map(...))即可。
4. range()函数现在已经类似xrange()。
5. zip()函数现在返回的也是iterator对象。

### 比较

1. 如果比较的对象不是同类型的或者根本无法进行自然比较的，会抛出TypeError异常，比如 1 < '', 0 > None, None < None, len < len等(这里的len是内置函数len)
2. builtin.sorted 和 list.sort 都不支持cmp这个参数了，因为key这个参数完全可以完成相同的功能
3. cmp函数应该确保不在使用，同时`__cmp__`特殊函数也不在支持，你可以使用`(a > b) - (a < b)`来代替`cmp(a, b)`，当然`__lt__, __eq__`等函数仍然支持

下图中，左边是py2，右边是py3
![cmp](py-py3\cmp.png)

### 整数

1. 本质上讲，已经没有long这种类型，都是int类型，但是int类型很多特性和long类型一致
2. 1 / 2返回的是float， 1//2返回除数，看下图
![div](py-py3\div.png)
3. sys.maxint已经被移除，因为不在限制int的值，但是sys.maxsize可以用
4. 8进制不支持`0720`格式，需要使用`0o720`来代替。

