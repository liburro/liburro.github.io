---
title: 大数据-pyspark
date: 2018-07-18 14:38:08
tags:
categories: bigdata
---

主要记录了pyspark的相关编程使用。

<!--more-->

## 基本操作

### SQL

SparkSession对象有一个sql函数，可以直接执行sql语句，比如和hive一起工作。如果直接通过pyspark命令行，那么会自动生成一个SparkSession对象叫spark，可以直接使用，比如获取SparkContext对象，可以使用spark.sparkContext，如果使用脚本的方式，就需要手动创建了。

```
spark = SparkSession.builder.appName('test').enableHiveSupport().getOrCreate() #enableHiveSupport让其支持hive

df = spark.sql("SELECT ....") #返回的是DataFrame对象
rdd = df.rdd #获取RDD对象

df1 = df.select(column) #类似sql的选取某些列，重新生成一个DataFrame
df2 = df.select(column).distinct() #选取，但是去重
```

### 计算

下面的两个标题都是英文，没有进行翻译，其中transformations大致翻译叫转换吧，actions翻译成动作，区别就是transformations不会立即执行计算，等到使用了actions以后才会。

#### transformations

```
map(f) #类似python的map函数
```

```
flatMap(f) #类似map函数，但是可以返回多个值，类似map函数返回了一个元组
```

#### actions

```
collect() #返回全部结果集，一个python列表
take(n) #返回指定数目的结果集，一个python列表
first() #返回结果的第一个值
```

### context session

context是全局的入口点，session是sparksql里面的概念，创建session的时候，默认页会创建context的，假如现在有一个session叫se。

```
ct = se.sparkContext #获取到了context
ct.setLogLevel('DEBUG') #设置日志级别为DEBUG
```

### 查看配置属性

```
attr = sc.getConf().getAll() #其中sc是SparkContext对象，如果使用pyspark进入的shell，sc已经默认创建好，类似spark一样
```
