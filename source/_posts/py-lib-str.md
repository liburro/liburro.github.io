---
title: '[python]-字符串操作'
date: 2018-05-02 14:04:16
tags:
categories: python
---

本文主要讲解一下python字符串的相关操作，包括string库的一些实用。

<!--more-->

## 常量字符串的操作

常量字符串，就是我们定义的一个字符串，它们是一个常量，也就是不可改变的。

比如：

``` python
var = "test_str"
```

这里把常量字符串`test_str`赋值给了变量`var`，其中`test_str`这是一个不可改变的，`var`只是一个变量，可以被重新赋值。

### strip

https://docs.python.org/2/library/stdtypes.html#str.strip
https://docs.python.org/2/library/string.html#string.strip

涉及到3个函数，`strip`, `lstrip`, `rstrip`，同理string库里面也有这3个函数，`strip`是复制当前字符串，把当前字符串的前后的特定的字符给去掉后返回一个新的字符串，默认去掉的字符是空格。`lstrip`是去掉左边特定的字符，`rstrip`是去掉右边的特定字符。
需要去掉的字符可以是多个，当遇到第一个不是去掉的字符的时候停止，比如'testxxxtest'要去掉里面的"test"几个字符，`strip`函数会匹配第一个t发现在去掉的字符中，e s t几个也在，所以都去掉，遇到了x不在test中则停止。

``` python
" test ".strip() #这里返回test
"test".strip('t') #这里返回es
"testxxxtest".strip("test") #这里返回xxx
"mississippi".rstrip("ipz") #返回mississ
"www.example.com".lstrip("cmowz.") #返回example.com

import string
string.strip(" test ") #返回test，其它几个使用类似
```

### split

https://docs.python.org/2/library/stdtypes.html#str.split
https://docs.python.org/2/library/string.html#string.split

这个函数是用于分割字符串，按照指定的字符把字符串分割，返回一个分割后的列表，同理也有`lsplit`和`rsplit`函数，同理string库里面也有相同的函数可以操作。

默认的分隔符是空格。

``` python
"1 2 3 4".split() #返回['1', '2', '3', '4']

import string
string.split("1 2 3 4") #返回['1', '2', '3', '4']
```
