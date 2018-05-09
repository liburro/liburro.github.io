---
title: '[Linux]-文件查看操作'
date: 2018-04-26 13:47:37
tags:
categories: linux
---

本文主要对Linux下查看文件的几个命令的常用方式进行了总结，没有进行详细解释，适合作为查阅，备忘。

<!--more-->

## cat

一次显示整个文件，参数:

-n: 输出行号
-b: 输出行号，但是忽略空白行

## more

每次显示一屏内容

按键：
enter 下一行
space 下一屏
= 显示当前行号
:f 显示当前文件和行号
ctrl+b 上一屏(对应的ctrl+f下一屏)

参数：
+num 从num行开始显示
-num 每屏显示num行
+/pattern 从第一个匹配pattern的前两行开始显示

举例：
``` bash
$ more +10 file
$ more -10 file
$ more +10 -10 file
$ more +/test -10 file
```

## less

类似于[more](#more)，但是less支持使用page down 和 page up进行翻页。
less 和 more 都支持v命令直接调用vim编辑器，都支持-s参数把多个连续的空行显示为一行。

按键：
enter 下一行
y 上一行
g 调到最开始
G 调到最末尾
n 显示下一个搜索的匹配
N 显示上一个搜索的匹配
m 输入以后会提示输入标记，比如输入a，那么当浏览到其它内容以后，想回到a这个标记点，输入'a就可以了


参数：
-i 文章内进行搜索的时候忽略大小写
-N 显示行号
-M 显示读取进度，显示内容类似：`iperft lines 19-48/1836 3%` 分别表示文件名iperft，当前屏幕显示的是19-48行，总共1836行

## head

默认显示文件的前10行，-n num参数可以指定显示多少行，这个参数对[tail](#tail)命令也适用。

## tail

默认显示文件的最后10行

## grep

可以进行字符匹配，-i 忽略大小写， -n 显示行号， -v 反向匹配

比如：

``` bash
$ grep -i test file
$ grep -ni test file
$ ps -ef | grep firefox -v grep
```

## wc

计算byte，字数或者列数

比如：

``` bash
ll | wc -l #显示ll命令回显有多少行
```
