---
title: Linux-环境变量
date: 2018-07-13 16:14:26
tags:
categories: linux
---

<!--more-->

## 配置文件

可以修改/etc/profile下面，这个是针对全局用户的，如果想针对某个用户修改，比如root用户，可以使用/root/.bashrc文件

## 配置格式

文件里面的配置格式

```
export JAVA_HOME=/path/to/java
export PATH=/path/to/add:${PATH}
```
