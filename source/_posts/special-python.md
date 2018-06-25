---
title: '[专项]-python'
date: 2018-06-25 16:46:29
tags:
categories: 专项
---

本文主要记录了python的相关编程笔记。本文主要以python2.7为主，后面会增加python2与python3的差异补充说明。

<!--more-->

## 字典

### 概念

字典是一种键值对的方式表示的数据类型，是key-value的方式存在的，key是唯一的，如果后期重新给这个key赋值，前面的值是被覆盖掉的， python的字典是__无序__的。

> 注意， 如果使用`{}`的方式表示，需要与set类型进行区分，如果`{}`里面没有内容，那么产生的是一个字典，如果`{1, 2, 3}`这种方式，产生的是一个集合(set)。

### 判断字典是否包含某个key

使用`in`来代替函数`has_key`。

``` python
testdict = {'a': 1, 'b':2}

if 'b' in testdict:
    print '字典中包含b这个key'
```

### 遍历字典的key

``` python
testdict = {'a': 1, 'b':2}

for i in testdict:
    print i
```

### 遍历字典的value

``` python
testdict = {'a': 1, 'b':2}

for i in testdict.itervalues():
    print i
```

> 注意， python3的字典对象没有`iterxxx`类似的的函数了，比如`itervalues()`被替换为`values()`了，`values()`在python2里面也存在，但是python3里面的`values()`行为和python2里面的`itervalues()`行为类似了。

### 获取key对应的value

``` python
testdict = {'a': 1, 'b':2}

print testdict['b']

print testdict['c'] #其中这里会报错，因为字典testdict中没有c这个key。
```

``` python
testdict = {'a': 1, 'b':2}

print testdict.get('a', None)

print testdict.get('c', 3) #这里会输出3
```

上面的两种方式区别： 方式一是直接通过`[]`获取，那么这个key就必须存在于字典中，如果不存在，会报错，方式二是通过函数`get(key, value=None)`来获取，其中第一个参数是需要获取的key，如果这个key存在于字典中，那么返回这个key对应的实际的vlaue，否则返回第二个参数的值，默认是`None`。

### setdefault

`setdefault(key, value=None)`是一个函数，它的作用是，如果key存在于字典中，则返回字典的实际值，如果不存在，会给字典增加一个key，并且设置对应的值为value，默认是`None`，并且返回这个值。

``` python
testdict = {'a': 1, 'b':2}

print testdict.setdefault('a') #这里输出1

print testdict.setdefault('c') #这里输出None,因为'c'不在testdict中，并且没有传递第二个参数，使用None，并且现在testdict变成了 {'a': 1, 'b':2, 'c': None}

print testdict.setdefault('d', 4) #这里输出4，并且testdict变成了 {'a': 1, 'b':2, 'c': None, 'd': 4}
```

### 同时遍历key和vlaue

``` python
testdict = {'a': 1, 'b':2}

for key, value in testdict.iteritems():
    print key, value
```

> 注意， python3里面的`items`函数类似python2里面的`iteritems`函数。



