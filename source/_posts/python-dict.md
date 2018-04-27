---
title: '[python]-字典的使用'
date: 2018-04-26 17:03:18
tags:
categories: PYTHON
---

本文以python3.6.5本本为例。

<!--more-->

## 字典基础操作

### 判断key是否在字典中

``` python
In [2]: test_dict
Out[2]: {'a': 1, 'b': 2, 'c': 3}

In [3]: if 'a' in test_dict:
   ...:     print(True)
True
```

### 遍历字典的key

方式一：
``` python
In [12]: for i in test_dict:
    ...:     print(i, end=' ')
a b c
```

方式二：
``` python
In [13]: for i in test_dict.keys():
```

> 注意python3里面没有iterkeys()这个函数了。

### 获取字典的值

#### get(key, value=None)

有一种情况，当字典存在某个key时，把key对应的值赋值给某个变量，当不存在这个key时，这个变量获得None值。使用dict.get()方法可以完成这个逻辑。
get方法接收两个参数，第一个是需要判定的key，第二个是如果key不存在时此方法返回的值，默认值是None，函数返回key对应的值或者第二个参数的值。

``` python
In [27]: test_dict
Out[27]: {'a': 1, 'b': 2, 'c': 3}

In [28]: test_dict.get('a') #'a'这个key在字典中，所以返回实际的值
Out[28]: 1

In [29]: test_dict.get('d', 4) #'d'这个key不在字典中，所以返回第二个参数的值4
Out[29]: 4

In [30]: print(test_dict.get('e')) #'e'这个key不在字典中，没有提供第二个参数，返回None
None
```

#### setdefault(key, value=None)

当key存在于字典中，那么返回这个key对应的值，如果不存在，则设置字典的key为对应的value，并且返回vlaue。

``` python
In [31]: test_dict
Out[31]: {'a': 1, 'b': 2, 'c': 3}

In [32]: test_dict.setdefault('a', 3)
Out[32]: 1

In [33]: test_dict
Out[33]: {'a': 1, 'b': 2, 'c': 3}

In [34]: test_dict.setdefault('d', 4)
Out[34]: 4

In [35]: test_dict
Out[35]: {'a': 1, 'b': 2, 'c': 3, 'd': 4}

In [36]: test_dict.setdefault('d')
Out[36]: 4

In [37]: test_dict.setdefault('e')

In [38]: test_dict
Out[38]: {'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': None}
```

### 同时遍历key和value

``` python
In [42]: test_dict
Out[42]: {'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': None}

In [43]: for k, v in test_dict.items():
    ...:     print(k, v)
    ...:
a 1
b 2
c 3
d 4
e None
```

> 注意： python3已经没有iteritems()函数。

## 字典视图对象

字典视图可以动态反馈字典的各种修改。

使用dict.keys(), dict.values(), dict.items()返回的就是字典的视图对象。

> python2里面是通过dict.viewkeys(), dict.viewvalues(), dict.viewitems()返回的字典视图对象。

``` python
In [44]: test_dict
Out[44]: {'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': None}

In [45]: keys = test_dict.keys()

In [46]: keys
Out[46]: dict_keys(['a', 'b', 'c', 'd', 'e'])

In [47]: values = test_dict.values()

In [48]: values
Out[48]: dict_values([1, 2, 3, 4, None])

In [49]: len(keys)
Out[49]: 5

In [50]: test_dict.setdefault('f', 6)
Out[50]: 6

In [51]: len(keys)
Out[51]: 6

In [52]: keys
Out[52]: dict_keys(['a', 'b', 'c', 'd', 'e', 'f'])
```

## lib-OrderedDict

我们知道，dict是无序的，也就是说我们插入数据以后，我们的数据在dict里面是无序的，不是按照插入的顺序排列的，python提供了OrdereDict这个类，保证了dict是按照我们插入的顺序排列的。

使用之前需要引入：
``` python
from collections import OrderedDcit
```
使用方法和一般字典使用方法一致。注意函数popitem()的使用。
在一般字典中：
``` python
dict.popitem()
```
返回(key, value)并且从字典中移除这一队值，注意，这里移除是任意的，不确定移除谁，因为dict是无序的。如果字典是空，这里将抛出KeyError异常

在OrderedDict中：
``` python
ord.popitem(last=True)
```
这里有一个参数last，默认是True，表示移除最后一个元素，如果为False，则移除第一个元素，这里移除的内容就一定是确定的。同理，字典为空，抛出KeyError异常。

OrderdDict多了一个`move_to_end`函数，有两个参数，第一个参数是待移动的key，第二个参数表示移动到最后(True默认值)还是移动到开头(False)。

``` python
ord.move_to_end(key, last=True)
```

同理，如果key不存在，抛出KeyError错误。

## lib-defaultdict

是dict的子类，但是提供了一个参数用于初始化字典的key对应的值

同样需要先引入
``` python
from collections import defaultdict
```

结构如下：
``` python
defaultdict(default_factory=None)
```

使用实例：
``` python
In [92]: a = defaultdict(list)
In [95]: a['a']
Out[95]: []
```
上面定义了一个defaultdict实例a，并且参数是list，我们并没有定义a的key 'a'，但是后面直接使用a['a']并没有报错，而是直接输出了一个空的列表。也就是说如果默认情况下，使用一个不存在的key，defaultdict会创建这个key，并且赋初始值为`default_factory`。
