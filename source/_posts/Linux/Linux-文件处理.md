---
title: Linux-文件处理
date: 2018-07-12 15:16:55
tags:
categories: linux
---

本文主要记录了Linux的文件处理，比如压缩解压等。

<!--more-->

## 压缩

### zip

```
zip -r test.zip test #压缩test，压缩文件名为test.zip，另外压缩文件解压后就和test完全一致，没有里面在嵌套一层的说法
zip -t test.zip #在不解压的情况下查看压缩文件的内容
```
