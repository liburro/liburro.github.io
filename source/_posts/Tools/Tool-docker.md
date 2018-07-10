---
title: Tool-docker
date: 2018-07-04 10:31:39
tags:
categories: 工具
---

本文主要记录了docker的基本使用。

<!--more-->

## 安装

### CentOS下安装

#### 直接通过yum的方式安装

```
yum update #确保是最新的yum源
yum -y install docker-io #这里-y参数是在yum里面会有各种提示y/d/n等你输入，如果这里-y参数提供了，那么默认输入y
service docker start #启动docker服务

docker run hello-world #可以测试一个helloworld程序，这个默认是不存在的，会在docker仓库拉取一次
```

> 如果CentOS7里面使用`service`命令启动docker报错，可以使用`systemctl start docker`的方式查看是否可以启动。

> 如果CentOS7里面启动docker报错，可以试着重新安装`yum install docker-engine`这个即可，安装之前卸载掉前面安装的内容`yum remove docker;yum remove docker-selinux`。

##### 服务相关几个命令(以docker服务为例)： 

```
启动docker：systemctl start docker
停止docker：systemctl stop docker
重启docker：systemctl restart docker
查看docker状态：systemctl status docker
开机启动：systemctl enable docker

[root@localhost system]# systemctl list-unit-files | grep docker
docker-cleanup.service                        disabled
docker-storage-setup.service                  disabled
docker.service                                disabled
docker-cleanup.timer                          disabled
[root@localhost system]# 
[root@localhost system]# 
[root@localhost system]# systemctl cat docker.service #这里可以查看服务配置文件位置
```

#### 镜像源的配置

由于国外镜像不能访问或者很慢，可以编辑`/etc/docker/daemon.json`这个文件，配置内容：

```
{
  "registry-mirrors": ["http://hub-mirror.c.163.com"]
}

当然也可以配置"insecure-registries":["10.10.30.240:5000"]为自己的私人仓库地址
```

> 注意如果在CentOS7里面，配置这个不生效的情况，可以查看`/etc/sysconfig/docker`这个文件，里面让`Do not add registries in this file anymore. Use /etc/containers/registries.conf`这样做，我们查看`/etc/containers/registries.conf`这个文件，可以看到`registries.search`这个字段，我们增加我们自己的仓库即可。比如增加阿里云的仓库`r6w9c7qa.mirror.aliyuncs.com/library`。

## 本地仓库搭建

有时候内网无法访问外网，通过设置http代理的方式是不能解决docker pull镜像的，所以可以在一台可以访问外网的机器上搭建自己的私有仓库，然后内网去拉去这里面的镜像即可。

### CentOS7的搭建

```
docker pull registry
docker run -d -p 5000:5000 -v /opt/registry:/var/lib/registry --restart=always --name registry registry
docker ps #可以查看刚刚启动的registry，通过范文`http://IP:5000/v2/_catalog`查看仓库已有的镜像，如果是新建的，一般是空的
docker pull busybox #拉取一个busybox到本地，这里使用busybox进行测试，因为这个很小
docker images #查看拉取得镜像
docker tag busybox registry:5000/busybox #给自己的镜像取个名字，加入tag，这里只是多了一个镜像，busybox这个原始镜像仍然还在
```

> 注意上面说的tag步骤，说法不是很准确，如果给一个镜像修改自己的tag，完整的是`docker tag busybox new_name:new_tag`，其中`new_name:new_tag`里面的new_name是取得新的名字，比如上例中的`registry:5000/busybox`，这只是一个名字，加入的tag是默认的latest。

修改配置文件

```
root@Jenkins:~# cat /etc/docker/daemon.json 
{ "insecure-registries":["registry:5000"] }
```

查看仓库的镜像

```
curl -XGET http://registry:5000/v2/_catalog
```

内网机器拉取镜像

```
systemctl status docker #查看docker服务的配置文件，一般是/usr/lib/systemd/system/docker.service，在这个文件的ExecStart字段里面增加--add-registry=registry:5000 --insecure-registry=registry:5000

systemctl start docker #启动docker服务

docker pull busybox #拉取本地镜像，这个速度很快的
```

> 注意了， registry:5000上面的配置中registry都必须是仓库的IP地址，包括`docker tag busybox registry:5000/busybox`这句，里面也是固定的，否则通过`docker push`是push到了docker.io里面去的。

> docker run里面的参数说明：-d -p -v --restart --name

## 基本使用

### 帮助信息

```
docker --help #可以查看docker命令的帮助
docker stats --help #可以查看stats子命令的帮助

docker info #查看docker环境的配置信息
```

### 查找镜像

```
docker search ubuntu
```

### 删除镜像

```
docker rmi imagename #如果镜像容器已经运行，需要先删除容器
```


