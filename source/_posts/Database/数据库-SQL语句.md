---
title: 数据库-SQL语句
date: 2018-07-13 11:29:44
tags:
categories: database
---

本文主要记录了SQL语句的使用。

<!--more-->

## 分组

分组的原则是先按照条件选取数据，然后按照分组统计数据。

比如现在有下面的一张表：

```
areaid client tx
1   a   100
1   a   200
1   b   150
2   c   200
```

现在通过下面的语句：

```
SELECT areaid, count(distinct client) as count, sum(tx) as tx GROUP BY areaid
```

得到的结果是

```
areaid count tx
1   2   450
2   1   200
```

注意上面对client使用的distinct去重，如果不用得到的结果是，整体过程是先选择areaid，并且按照areaid分组，然后统计每个组下面的去重后的client的数量，同时统计每个组下面的tx的和，注意没有根据任何去重。

```
areaid count tx
1   3   450
2   1   200
```

如果使用下面的语句

```
select areaid, count(client), sum(tx) from test group by areaid, client;
```

得到的结果是

```
areaid count tx
1   2   300
1   1   150
2   1   200
```

也就是先按照了areaid分组，在按照了client进行分组


