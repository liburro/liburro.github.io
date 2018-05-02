---
title: '[robotframework]-指导说明'
date: 2018-04-26 17:08:35
tags:
categories: robotframework
---

robotframework是一个自动化测试框架。

<!--more-->

## 基础实例
## case的组织
## 运行命令

## 所有可用的设置
## 内建库的常用函数
## 变量

### 变量的优先级和作用域

根据定义变量的方式有不同的作用域和优先级。

#### 变量的优先级(由高到底排序)

内建变量 > 执行测试设置的变量 > 命令行 > table表 > resource或者variable文件引入

1. 内建变量，比如${TEMPDIR}拥有最高的优先级。
2. 测试过程中产生的变量，比如使用set test/suite/global variable keywords产生的变量，__会覆盖已经存在的在当前作用域内的变量__(也就是说，如果一个case的variable里面有一个变量，先于这个case运行的任何文件里面使用了set global variable这个keyword的话，这个变量会被替换为global的值)，虽然优先级较高，但是不会影响作用域外的变量。
3. 命令行变量，会覆盖variable table和资源文件里面的所有变量，命令行变量可以被设置多次，只有最后一个生效。
4. table表变量，可用于定义这个变量的文件的任意位置，会覆盖资源文件引入的变量。
5. 资源文件变量，资源文件引入多个相同的变量，第一个引入的变量生效。

#### 变量的作用域

1. 全局作用域: 可以在全局任意一个地方可用，可以通过命令行和set global variable设置，当然也包括内建变量。
2. 测试套作用域: 可以在当前测试套种使用的，比如当前测试文件引入的，当前文件的variable table里面定义的，或者 set suite variable定义的。
3. 测试用例作用域: 可以在当前测试用例中使用，一般通过set test variable设置的。
4. 本地作用域: 比如keyword的参数，keyword获得的返回值等。
## keyword
## 资源文件
## 用户自定义测试库

### 自定义测试lib接受列表

假如用python定义了一个自己的keyword，并且接受列表，比如：

``` python
def test_lib(*this_is_list):
```

那么在robotframework中需要使用如下格式：

``` robotframework
test_lib  a  b  c d
```

注意参数空格，a和b都是前后两个空格，c和d之间只有一个空格，那么参数类似于：
``` python
['a', 'b', 'c d']
```

## 内部API

robotframework提供了很多内部api，这个是可以使用python获取java直接调用的函数。下面介绍一些常用的。
文档：http://robot-framework.readthedocs.io/en/v3.0.4/

### 日志

在`robot.api`这个包下的`logger`模块，如果要使用，需要引入：

``` python
from robot.api import logger
```

`logger`模块有几个函数，`write`, `trace`, `debug`, `info`, `warn`, `error`, `console`，所有函数的第一个参数都是你想打印的信息，注意`console`只会打印到命令行里面，日志里面没有，`info`默认不会显示到命令行，但是有一个`also_console`参数，设置为`true`，即可以打印到控制台，也可以输出到日志。

具体参见：http://robot-framework.readthedocs.io/en/v3.0.4/autodoc/robot.api.html#module-robot.api.logger
