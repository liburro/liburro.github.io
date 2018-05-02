---
title: '[python]-lib-ConfigParser'
date: 2018-05-02 10:48:47
tags:
categories: python
---

本文以python2.7版本为例进行ConfigParser基础进行解析，ConfigParser是python用于解析ini文件的库。

<!--more-->

官方文档：https://docs.python.org/2/library/configparser.html

下面先看一下ini文件的格式，大致如下：
``` ini
[SECTION1]
OPTION1 = VALUE1
OPTION2 = VALUE2

[SECTION2]
OPTION3 = VALUE3
OPTION4 = VALUE4

[DEFAULT]
OP = DEFAULT1 #如果其它section没有提供这个option，默认使用这里的值
```

## ConfigParser

ConfigParser有几个类，下面先讲解它的ConfigParser这个类，首先引入：

``` python
from ConfigParser import ConfigParser as cp
```

### read
然后实例化这个类并且读取init文件

``` python
mycp = cp()
cp.read('your_ini_file.ini')
```

### `has_section`
然后就可以使用如下的函数进行各种操作了：

``` python
mycp.has_section('section') #判断某个section是否在ini文件中
```

### `has_option`

``` python
mycp.has_option('section', 'option') #判断在section中是否有某个option
```

### get

``` python
mycp.get('section', 'option') #返回section下的option的值
```

上面的`get`函数返回的是字符串，还有`getinit`, `getfloat`等可以返回整形和浮点型数据。

### sections

``` python
mycp.sections() #返回一个列表，包含ini文件的所有section，不包含default内容。
```

### items

``` python
mycp.items('section') #返回一个列表，每个元素是一个包含两个元素的元组
#元组第一个元素是option内容，第二个是值，会获取default的内容。
```

### 重复的情况

__注意__：如果有section的内容相同，取最后一个section里面的内容，如果section里面有option相同，取最后一个option的值。
