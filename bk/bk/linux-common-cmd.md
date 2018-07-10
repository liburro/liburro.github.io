---
title: '[linux]-常用命令'
date: 2018-05-07 09:30:32
tags:
categories: 历史
---

本文收录了个人常用的linux命令，很基础的一些命令就没有列举了。

<!--more-->

## ls

![ls](linux-common-cmd/ls.png)

## find

## grep

用于匹配查找

``` bash
grep ISM_Base_0490 output.xml #在output.xml文件中查找ISM_Base_0490
```

-n: 打印所在行
-v: 反向匹配
-i: 忽略大小写
-A n: 显示匹配的前n行
-B n: 显示匹配的后n行

``` bash
grep -A 5 -B 5 teststr
```

## dh/df

## top

## ps

## ntpdata

如果需要和某个ntpserver进行时间同步，使用

``` bash
ntpdate ntpserver_ip
```
