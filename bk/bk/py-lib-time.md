---
title: '[python]-lib-时间处理'
date: 2018-05-03 12:32:59
tags:
categories: 历史
---

本文主要记录了python处理时间的几个库的使用。

<!--more-->

## 基本概念

### 时间戳

时间戳一般在python里面是表示从1970年1月1日开始按秒计算的时间偏移量，通过`time.gmtime(0)`可以查看。

### UTC

世界协调时间，标准时间，中国是UTC+8

### DST

夏令时

## time

``` bash
import time
```

### time

``` bash
time.time() #返回类似1525322407.005这种格式
```

### localtime

``` bash
time.localtime()
```

返回类似如下：

``` bash
time.struct_time(tm_year=2018, tm_mon=5, tm_mday=3, tm_hour=12, tm_min=34, tm_sec=17, tm_wday=3, tm_yday=123, tm_isdst=0)
```
返回的雷人分别表示 年 月 日 时 分 秒 一周的第几天(0表示周日) 一年中的第几天(从1开始) 是否是夏令时。

函数可以接收一个时间戳作为参数，返回对应的时间元组，默认是使用当前的时间戳作为参数。

### gmtime

``` bash
time.gmtime()
```

返回格式类似于[`localtime`](#localtime)，只不过`gmtime`返回的是UTC标准时间，`localtime`返回的是当前本地时间。
如果是中国的话，`gmtime`比`localtime`获取到的时间会小8个小时。

### strftime

格式化时间，第一个参数是格式化的格式，第二个参数是时间元组，比如`localtime`返回的时间，默认是`localtime`的返回值。

其中 %Y %m %d %H %M %S 分别表示年 月 日 时 分 秒

``` bash
time.strftime("%Y-%m-%d %H:%M:%S") #返回'2018-05-03 14:01:57'
```

### strptime

这个就是`strftime`的反向操作。

``` bash
time.strptime('2018-05-03 14:01:57', "%Y-%m-%d %H:%M:%S")
```

返回内容如下：

``` bash
time.struct_time(tm_year=2018, tm_mon=5, tm_mday=3, tm_hour=14, tm_min=1, tm_sec=57, tm_wday=3, tm_yday=123, tm_isdst=-1)
```

## 参考链接

https://docs.python.org/2/library/time.html
https://docs.python.org/2/library/datetime.html
https://www.cnblogs.com/renpingsheng/p/6965044.html
http://www.jb51.net/article/49326.htm
https://blog.csdn.net/tigerking1017/article/details/51332220
https://blog.csdn.net/u012175089/article/details/62044335
