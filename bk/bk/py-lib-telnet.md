---
title: '[python]-lib-telnet'
date: 2018-05-03 15:49:11
tags:
categories: 历史
---

本文记录了python通过telnet链接设备的是使用。

<!--more-->

## 基本操作

``` python
from telnetlib import Telnet

tel = Telnet('your_ipaddr')
tel.read_until('expected str', timeout) #这里返回读取到的回显
tel.write('something' + self.cr)
tel.expect(['es1', 'es2', '\d+'], timeout) #这里第一个参数必须是列表，里面内容可以是正则表达式，返回3个元素的元组，最后一个是读取到的内容
```

> 注意： expect返回一个三元组，第一个是匹配到expect第一个参数里面的索引，比如上例中如果匹配到了es1那么返回0，返回的第二个内容是匹配到的对象，第三个是读取的回显，如果没有匹配到任何东西，返回的是-1，第二个返回的对象是None,第三个返回的内容，有可能有，有可能为空

## 下发特殊命令

### 下发ctrl+c

``` python
tn.write('\x03')
```

对直接下发`\x03`就代表了ctrl+c
