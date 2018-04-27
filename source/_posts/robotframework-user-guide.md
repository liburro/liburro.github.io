---
title: '[robotframework]-指导说明'
date: 2018-04-26 17:08:35
tags:
categories: ROBOTFRAMEWORK
---

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
