---
title: '[SQL]-sql基础'
date: 2018-04-25 14:44:47
tags:
categories: [DATABASE]
---

本文主要介绍了sql(Structured Query Language)语句的基本使用，主要使用mysql进行操作。

<!--more-->

参考链接:

* http://www.w3school.com.cn/sql/

## 基本语法

### 创建数据库

数据库是数据的容器，在做任何有关数据的操作时，都需要创建一个数据库。

```sql
CREATE DATABASE [IF NOT EXISTS] database_name;
```

注意 `IF NOT EXISTS`语句是可选的，这句话的意思是如果在服务器已经有了和当前需要创建的数据库同名的数据库，就不创建。

### 创建表

mysql等数据库，都是需要用表来存放数据的。

```sql
CREATE TABLE [IF NOT EXISTS] table_name(
    column_list
);
```

举例:

```sql
CREATE TABLE test_table(
    table_id INT AUTO_INCREMENT, #设置一个列，取名叫table_id，数据类型为INT，并且是自动增长的
    title VARCHAR(10) NOT NULL, #创建title列，数据类型为VARCHAR类型，最长10，并且不为空
    create_date DATE, #创建create_date列，数据类型为DATE，可以为空
    usercount INT DEFAULT 0, #创建usercount列，设置默认值为0
    PRIMARY KEY (id), #设置主键为id
    UNIQUE (title), #设置title必须唯一
    CHECK (usercount>-1), #设置usercount列的数据不能是负数
    FOREGIN KEY (another_id) REFERENCES anoter_table(another_id), #设置another_id列，并且与anoter_table表的another_id关联
);
```

解释:
> 主键的作用：确定了数据的唯一性，便于其它表的关联，配合索引提升数据查找性能
> SQL约束：`NOT NULL`, `UNIQUE`, `PRIMARY KEY`, `FOREIGN KEY`, `DEFAULT`, `CHECK`

### SELECT语句

基本语法:

``` sql
SELECT column_name FROM table_name;
```

或者:

```sql
SELECT * FROM table_name;
```
### WHERE语句

基本语法:

```sql
SELECT * FROM table_name WHERE cloumn_name='condition';
```

### INSERT语句

基本语法:

```sql
# 指定列名插入
INSERT INTO table_name(column1, column2) VALUES (value1, value2);

# 指定列名插入多条数据
INSERT INTO table_name(column1, column2) VALUES (value1, value2), (value11, value22);

# 如果插入全部数据，可以不指定列名，注意自动增加的字段可以不写
INSERT INTO table_name VALUES (value1, value2);

# 不指定列名插入多条数据
INSERT INTO table_name VALUES (value1, value2), (value11, value22);
```

### UPDATE语句

基本语法:

```sql
UPDATE table_name SET column1 = new_value1, column2 = new_value2 WHERE condition;
```

注意: SET后面是需要更新的列，指定多列，中间用英文的逗号分开，WHERE语句是可选的，如果没有这个语句，默认更新所有的行的数据。

### DELETE语句

基本语法:

```sql
DELETE FROM table_name WHERE condition;
```

注意: 如果没有WHERE子句进行限制，默认删除所有的数据。
