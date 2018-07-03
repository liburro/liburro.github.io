---
title: '[专项]-robotframework'
date: 2128-06-28 14:07:29
tags:
categories: 专项
---

本文主要记录了robotframework框架的一些使用笔记。

本文实际发布时间是2018-06-25，为了置顶，所以修改了发布时间为将来的某个时间。

<!--more-->

## 常用命令参数

默认的使用方式是：

```
pybot casefile.txt
```

其中最后一个参数就是需要运行用例或者用例目录，其它参数都必须位于这个参数前面

```
# 指定输出路径
pybot -d test case.txt
```

```
# 指定运行lib
pybot -P test case.txt
```

```
# 指定运行tag
-i tagname
# 排除包含tag的case
-e tagname
```

```
# 指定运行级别
-L trace
```

```
# 指定变量值
-v var:value
```

> -v的优先级最高，里面的变量都会变成这个-v指定的值，记住是任何情况下最高，执行的环境中有-v来指定top文件，里面有引入的变量，如果-v了top里面的变量，都是以-v里面的生效，不是-v top里面的生效。

## 日志修复

有时候使用robot跑了脚本，如果想中间停止，或者中间抛出了错误，只产生了一个output.xml文件，这个文件不便于阅读，想转换为好看的log.html，可以使用下面的方法进行日志修复。

```
pip install robotfixml
python -m robotfixml output.xml fixed.xml
rebot fixed.xml
```
