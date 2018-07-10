---
title: '[mysql]-实例'
date: 2018-05-09 10:38:49
tags:
categories: 历史
---

本文主要使用MySQL来进行SQL语句练习。首先会创建一个数据库`sqltest`，会涉及两张表，`tb_websites`和`tb_accesslog`，第一张表里面有`id name url country` 几个字段，分别表示ID 名称 URL地址 国家，第二张表里面有`id site_id accesscount date`几个字典，分别表示ID `tb_websites`的ID 日访问量 日期。

<!--more-->

## 准备

创建数据库

``` sql
CREATE DATABASE sqltest;
```

创建表

``` sql
CREATE TABLE tb_websites (
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(10) NOT NULL,
  url VARCHAR(50) NOT NULL,
  country VARCHAR(5) NOT NULL
) ENGINE=innoDB DEFAULT CHARSET=utf8;
```

``` sql
CREATE TABLE tb_accesslog (
  id INT PRIMARY KEY AUTO_INCREMENT,
  site_id INT,
  accesscount INT DEFAULT 0,
  date DATE
) ENGINE=innoDB DEFAULT CHARSET=utf8;
```

## INSERT

表创建完成后，里面没有数据，我们通过INSERT插入数据

``` sql
INSERT INTO tb_websites VALUES (1, '百度', 'http://www.baidu.com', 'CN');
```

## 命令行

### 连接数据库

``` bash
mysql -hip -uuser -ppassword
```

### 显示数据库

``` bash
show databases;
```

### 使用数据库

``` bash
use database_name;
```

### 显示表

``` bash
show tables;
```

### 快速复制数据库

首先需要创建一个新的数据库，用于存储。

``` bash
CREATE DATABASE `bk_sks_test` DEFAULT CHARACTER SET UTF8 COLLATE UTF8_GENERAL_CI;
```

然后使用mysqldump命令配置mysql进行复制，下面使用远程复制，如果是本机复制，就没有-h参数

``` bash
mysqldump sks_test -h10.10.11.117 -uroot  --add-drop-table | mysql bk_sks_test -h10.10.11.117 -uroot
```
