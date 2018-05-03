---
title: '[python]-lib-telnet'
date: 2018-05-03 15:49:11
tags:
categories: python
---

本文记录了python通过telnet链接设备的是使用。

<!--more-->

``` python
from telnetlib import Telnet

tel = Telnet('your_ipaddr')
tel.read_until('expected str', timeout) #这里返回读取到的回显
tel.write('something' + self.cr)
tel.expect(['es1', 'es2', '\d+'], timeout) #这里第一个参数必须是列表，里面内容可以是正则表达式，返回3个元素的元组，最后一个是读取到的内容
```

