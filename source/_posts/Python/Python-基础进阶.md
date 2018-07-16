---
title: Python-基础进阶
date: 2018-07-16 09:44:08
tags:
categories: python
---

本文记录了python的一些基础操作以及基础进阶的内容。包括大多数内建函数，列表，字典等的操作，也包含了sys等模块的一些基础特性。

<!--more-->

## len函数的替代

先看下面的代码，假设temp是一个列表，或者一个字符串，或者任意的可求长度的类型。

```
sum(1 for _ in temp)

len(temp)
```

上面的两组操作都可以获取temp的长度。

> 注意`_`在python中也是一个有效的变量名。另外在python交互式窗口里面，`_`代表上一次执行的结果。

```
In [25]: a = 1

In [26]: a
Out[26]: 1

In [27]: _
Out[27]: 1

In [28]: b = 2

In [29]: print b
2

In [30]: _
Out[30]: 1

In [31]:
```

## and等运算符

```
1 and 0 #返回0
2 or 1 #返回2
```

## ifelse

```
a = 1 if 1 == 1 else 0
```

## 空值的判断

```
a = None #python里面[],'',0,False等都是非真值
if a:
    do_something()
else:
    do_other_something()
```

## lambda表达式

## 字典的操作

## 解包(`*`和`**`的操作)

## 迭代器

## 生成器

### yield语句

#### 基本操作

```
def test_yield(x):
    for i in range(x):
        a = yield i
        print(a)

a = test_yield(5)
print(next(a))
print(next(a))

print(next(a)) #0
print(next(a)) #None 1
print(a.send(100)) #100 2
print(next(a)) #None 3

0 #首先执行到yield，把后面的表达式i返回了，也就是第一次进入，为0
None #第二次调用next的时候，接着上一次暂停的地方继续执行，打印a，默认为None
1 #然后又执行到了yield的地方，返回了第二次的结果1
100 #然后send了100，a获取到了100，并且执行到了下一次的yield，下一步马上打印2
2 #接上一步
None #最后一个next，a没有值，默认None
3 #执行到了yield地方，打印3
```

#### yield from子句

#### throw函数

## 上下文管理器

## 装饰器

### 多重装饰器

## 类相关

### property

### classmethod

### staticmethod

### 类变量

### 继承

#### 特殊成员的继承

包括类变量， class和static method的继承

## import的理解

## 特殊的函数和变量

### `__all__`

### `__file__`

### `__name__`

## sys模块基础功能

### 命令行参数

当使用下面的方式执行py脚本的时候：

```
python example.py 1 abc
```

sys模块提供了一个变量`sys.argv`可以获取到命令行参数的值，其中`sys.argv[0]`是这个脚本的名字， `sys.argv[1]`是1，`sys.argv[2]`是abc。

> 其中`sys.argv[0]`这个的值是根据`python example.py 1 abc` 中py文件获取的，也就是说，如果你跟的是路径，那么`sys.argv[0]`也获取到路径，简单理解为python是一个命令，后面的都是参数，传递什么，`sys.argv`就会原封不动的获取到什么。


