---
title: Linux-网络配置
date: 2018-07-04 10:03:09
tags:
categories: linux
---

主要记录了Linux网络相关的配置，比如IP地址，防火墙等。

<!--more-->

## IP配置

### CentOS

```
首先编辑文件：vim etc/sysconfig/network-scripts/ifcfg-eth0
device=eth0
bootproto=dhcp #如果需要手动配置IP地址，这里配置为none
onboot=yes #是否随系统启动
hwaddr=09:0a #mac地址
IPADDR=10.0.1.27  #IP地址
NETMASK=255.255.255.0  
GATEWAY=10.0.1.1
最后重启服务：service network restart
```

## 路由配置

### CentOS

```
编辑文件：vim etc/sysconfig/network-scripts/route-eth0
格式为：10.0.0.0/8 via 192.168.53.1，其中192.168.53.1这个是网关
```

## SSH配置

### CentOS

如果遇到root用户不能登陆的情况，修改`/etc/ssh/sshd_config`文件，取消`PermitRootLogin`这个参数的注释。
