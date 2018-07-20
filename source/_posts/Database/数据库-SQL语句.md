---
title: 数据库-SQL语句
date: 2018-07-13 11:29:44
tags:
categories: database
---

本文主要记录了SQL语句的使用。

<!--more-->

## 插入数据

```pgsql
INSERT INTO test (areaid, datefmt, client, mac) VALUES (1, 20180911, 100, 'x'), (1, 20180911, 100, 'y') #插入多组数据之间用,分割
```

## 排序

```
order by
```

## 分组

分组的原则是先按照条件选取数据，然后分组，然后统计数据。

比如现在有下面的一张表：

```pgsql
areaid client tx
1   a   100
1   a   200
1   b   150
2   c   200
```

现在通过下面的语句：

```pgsql
SELECT areaid, count(distinct client) as count, sum(tx) as tx GROUP BY areaid
```

得到的结果是

```pgsql
areaid count tx
1   2   450
2   1   200
```

注意上面对client使用的distinct去重，如果不用去重得到的结果是：

```pgsql
areaid count tx
1   3   450
2   1   200
```

如果使用下面的语句

```pgsql
select areaid, count(client), sum(tx) from test group by areaid, client;
```

得到的结果是

```pgsql
areaid count tx
1   2   300
1   1   150
2   1   200
```

也就是先按照了areaid分组，在按照了client进行分组，然后在统计。

### 处理分组内容

假设有下面一张表(test)。

其中areaid是一个整形的类似ID的数据，datefmt是日期的整形表示，比如20180711表示的是2018年7月11日。

![group1](group1.png)

需要选取areaid, datefmt，datefmt统计到月，并且按照月areaid, datefmt进行分组，统计client在每个areaid的每月的总数。

```pgsql
select areaid, datefmt/100 as datefmt, sum(client) from test group by areaid, datefmt/100; #如果需要转换类型，使用下面的语句
```

```hive
select areaid, int(datefmt/100) as datefmt, sum(client) from test group by areaid, int(datefmt/100);
```

统计结果：

![group1_result](group1_result.png)

### 分组总结

首先选取列，然后进行分组，分组从左往右进行，最后根据分组进行统计。

## UNION

合并两个选择的结果，UNION默认去掉了重复的，UNION ALL不会去掉重复的

```
select a, b from table1 union select a, b from table2
select a, b from table1 union all select a, b from table2
```

