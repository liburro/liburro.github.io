---
title: '[专项]-工具合计'
date: 2018-06-25 13:46:27
tags:
categories: 专项
---

本文主要记录了一些工具的使用，主要包括：Jenkins、Gitblit等。

<!--more-->

## Windows

### 命令行

#### 重启系统

`shutdown -r` 是重启系统， `shutdown -s` 是关机。

### 定时任务

这个配置在 控制面板-小图标-管理工具-任务计划程序 这里就可以进行配置。

### 服务

找到`InstallUtil.exe`这个文件，然后在命令行使用`InstallUtil.exe servicename`安装你的服务，可以通过`InstallUtil.exe /u servicename`卸载服务，然后就可以使用`net start/stop servicename`对服务进行启动/停止了。

## Linux

### 命令行

#### 系统信息

1. 查看内存信息: `cat /proc/meminfo`

## Jenkins

### 安装

#### Windows下安装

直接下载jenkins的jar包，放到任意目录下，在cmd命令行执行 `java -jar jenkins.war` 即可，然后通过 https://ip:8080/ 就可访问了，最开始会提示进行安装，安装提示即可简单的完成。

## Gitblit

Gitblit是一个git服务器，它可以通过web界面进行管理。

### 安装运行

#### Windows下安装

直接下载好windows下的安装包后，解压，进入解压目录，使用gitblit.cmd直接运行即可。

可以通过 https://ip:8443/ 这个地址访问，默认的管理员用户名和密码都是admin。

> 注意，由于gitblit是基于java开发的，所以需要java的运行环境，需要配置好java的环境变量， JAVA_HOME是jdk安装路径，Path环境变量里面需要增加%JAVA_HOME%\bin和%JAVA_HOME%\jre\bin

### 工单配置

默认情况下，gitblit是没有打开工单功能的，需要在gitblit的data目录下的defaults.properties这个文件中，配置tickets.serveice，这个默认是空的，根据需要配置，注释里面提供了很多的选项，我只使用过com.gitblit.tickets.FileTicketService这个。


