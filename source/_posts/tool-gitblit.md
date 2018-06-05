---
title: '[tool]-gitblit的使用'
date: 2018-06-05 18:14:15
tags:
categories: 工具
---

gitblit工具手册，gitblit是一个可以在windows端搭建git服务器的工具，提供web浏览界面。

本文是在win7 64环境下进行操作。

<!--more-->

## 安装配置

### 安装

由于gitblit是使用java开发的，所以需要安装java环境。

![java-env](tool-gitblit\java_env1.png)
![java-env](tool-gitblit\java_env2.png)

去gitblit上下载好后，直接解压出来即可。

然后找到解压目录下的`gitblit.cmd`文件，直接运行，服务就启动了，可以通过：

`https://192.168.53.133:8443/`就可以访问网页了，注意IP地址需要改成自己的。

默认的登陆用户名和密码分别是 admin admin

## 配置

### 工单配置

gitblit默认关闭了工单的，如果需要打开，需要配置data目录下的defaults.properties这个文件，配置属性tickets.service，其中这个默认是空的，根据注释可以看见提供了3中选择。

![tickets](tool-gitblit\tickets.png)
