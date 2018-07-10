---
title: '[专项]-工具合计'
date: 2018-06-25 13:46:27
tags:
categories: 历史
---

本文主要记录了一些工具的使用，主要包括：Windows、 Linux、 Jenkins、Gitblit等。

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

#### 网络配置

##### IP地址配置

###### CentOS

```
vim etc/sysconfig/network-scripts/ifcfg-eth0
device=eth0
bootproto=dhcp
onboot=yes
hwaddr=09:0a:xxxxx
IPADDR=10.0.1.27
NETMASK=255.255.255.0
GATEWAY=10.0.1.1

service network restart
```

##### DNS配置

如果直接修改`/etc/resolv.conf`里面配置`namesever 61.139.2.69`，可以临时进行使用。

##### 防火墙配置

参考文章：[CentOS 6和CentOS 7防火墙的关闭](https://www.linuxidc.com/Linux/2016-12/138979.htm)

###### CentOS7

```
CentOS7

[root@localhost ~]# firewall-cmd --state
not running
[root@localhost ~]# systemctl list-unit-files | grep firewalld
firewalld.service                             disabled
[root@localhost ~]# systemctl status firewalld.service
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
     Docs: man:firewalld(1)
[root@localhost ~]#

systemctl stop firewalld.service #停止firewall
systemctl disable firewalld.service #禁止firewall开机启动

启动一个服务：systemctl start firewalld.service
关闭一个服务：systemctl stop firewalld.service
重启一个服务：systemctl restart firewalld.service
显示一个服务的状态：systemctl status firewalld.service
在开机时启用一个服务：systemctl enable firewalld.service
在开机时禁用一个服务：systemctl disable firewalld.service
查看服务是否开机启动：systemctl is-enabled firewalld.service;echo $?
查看已启动的服务列表：systemctl list-unit-files|grep enabled

firewall-cmd --list-ports

firewall-cmd --zone=public --add-port=80/tcp --permanent
命令含义：

–zone #作用域

–add-port=80/tcp #添加端口，格式为：端口/通讯协议

–permanent #永久生效，没有此参数重启后失效

firewall-cmd --reload #重启firewall
```

#### 远程登陆

##### SSH

centos默认root用户是不能通过ssh登陆的，需要修改如下配置即可。

```
vi /etc/ssh/sshd_config
PermitRootLogin yes 这一行不应该被注释掉

/etc/rc.d/init.d/sshd restart 
```

#### 系统信息

1. 查看内存信息: `cat /proc/meminfo`
2. 查看系统在线时长： uptime

#### 权限

``` bash
chmod 777 somefile
chown jenkins testdir #修改所属用户
chown -R jenkins testdir #递归
chgrp jenkins testdir #修改所属用户组
chgrp -R jenkins testdir #递归
```

#### 软件安装

##### CentOS

centos下可以使用的是yum进行安装。直接使用这种方式在线安装，可以避免各种依赖库的问题。

```
yum search java | grep jdk #搜索可用的jdk包
yum install java-1.8.0-openjdk.x86_64 #安装jdk
```

当然如果你下载了rpm安装包，可以使用：

```
rpm -ivh xx.rpm
```

## Jenkins

可以参考[W3C](https://www.w3cschool.cn/jenkins/)的相关内容。

### 安装

#### Windows下安装

直接下载jenkins的jar包，放到任意目录下，在cmd命令行执行 `java -jar jenkins.war` 即可，然后通过 https://ip:8080/ 就可访问了，最开始会提示进行安装，安装提示即可简单的完成。

#### CentOS下安装

直接通过yum安装好java环境，到[jenkins官方](https://pkg.jenkins.io/redhat)下载好对应的版本，通过rmp的方式安装好。

安装完成后，使用`/etc/init.d/jenkins start`即可启动jenkins。如果不能通过ip地址加上8080端口访问，检查防火墙的配置和jenkins的log文件`/var/log/jenkins/jenkins.log`。

### 常用url

1. 内建变量： http://10.10.11.240:8080/env-vars.html/
2. 重启： http://localhost:8080/restart
1. 重新加载： http://localhost:8080/reload

### 执行

#### 定时执行

`H 21 * * *` 这个意思是每天晚上9点执行

### 插件

#### Job顺序

有这么一个需求： 有个名为X的job，需要等待A, B, C, D等几个job都完成后再执行， 默认的jenkins的job flow都是只要A, B, C, D中任何一个完成了， 就会触发X这个job， 不满足条件，但是有个join plugin这个插件可以完成这个功能， 但是需要附加一个job，比如名为Z，
则需要在Z这个job里面配置构建后步骤，需要增加两个步骤， 一是 Build other projects， 里面填入 A, B, C, D几个job， 二是增加 Join Trigger(这个需要安装了join plugin这个插件后才会有的选项)， 在Projects to build once, after all downstream projects have finished这个里面
填入X这个job。那么运行Zjob完成后，会触发A, B, C, D的运行，并且X这个job会等待A, B, C, D都完成后再执行。

#### 控制台时间戳

控制台显示时间戳的插件是： Timestamper

#### job迁移

当Jenkins服务器A需要同步Jenkins服务器B的某个job，可以在服务器A上安装job-import插件，然后再服务器A上的主界面上就有Job Import Plugin这个插件，根据提示直接使用即可。(当然需要到 系统管理-系统设置-Job Import Plugin这里配置远程的jenkins服务器)

### 常见问题

#### Opening Robot Framework log failed

Opening Robot Framework log failed
Verify that you have JavaScript enabled in your browser.
Make sure you are using a modern enough browser. Firefox 3.5, IE 8, or equivalent is required, newer browsers are recommended.
Check are there messages in your browser's JavaScript error log. Please report the problem if you suspect you have encountered a bug.

如果遇见上面的错误，可以在 系统管理-脚本命令行 里面输入 `System.setProperty("hudson.model.DirectoryBrowserSupport.CSP","")`执行后，重新打开浏览器即可。

## Gitblit

Gitblit是一个git服务器，它可以通过web界面进行管理。

### 安装运行

#### Windows下安装

直接下载好windows下的安装包后，解压，进入解压目录，使用gitblit.cmd直接运行即可。

可以通过 https://ip:8443/ 这个地址访问，默认的管理员用户名和密码都是admin。

> 注意，由于gitblit是基于java开发的，所以需要java的运行环境，需要配置好java的环境变量， JAVA_HOME是jdk安装路径，Path环境变量里面需要增加%JAVA_HOME%\bin和%JAVA_HOME%\jre\bin

### 工单配置

默认情况下，gitblit是没有打开工单功能的，需要在gitblit的data目录下的defaults.properties这个文件中，配置tickets.serveice，这个默认是空的，根据需要配置，注释里面提供了很多的选项，我只使用过com.gitblit.tickets.FileTicketService这个。

## wireshark

### 过滤http

普通过滤

```
http
```

## tcpdump

### 根据端口号过滤

```
tcpdump -i eth0 port 12348
```

## 其它工具

### excel

#### 筛选功能

有时候筛选结果没有内容，是因为所筛选的列下面有空的单元格，可以使用ctrl+f替换为空格即可。

### notpad++

#### 快捷键

注释: ctrl + q

## 好用工具

git-web界面： gitlab, gitblit



