---
title: Python-时间处理
date: 2018-07-16 09:43:53
tags:
categories: python
---

Python时间相关的有好几个模块，提供了很丰富的API，一般都能满足我们的要求。

<!--more-->

## 时间相关概念

### UTC

UTC是世界统一时间，中国是UTC+8。

### DST

DST是夏令时，大概意思是日照时间提前，会人为的把时间调前比如1个小时。

## time模块

https://docs.python.org/3/library/time.html

time模块直接使用的是C的库，所以根据平台的特性会有不同的效果。

### `struct_time`

有6个常用的函数会使用到这个namedtuple，其中gmtime,localtime,strptime会返回这个对象，而asctime,mktime,strftime会接收这个对象。

其中gmtime,localtime接收time()数据返回`struct_time`格式，mktime接受`strcut_time`数据，返回time()格式。

![struct_time](struct_time.png)

### time

time函数不接受参数，返回本地时间的相对于世纪元的秒数，是一个float类型。

### mktime

mktime接收一个`strcut_time`格式，返回一个time()格式。

### gmtime localtime

gmtime返回UTC时间，localtime返回本地时间。

他们两个都有一个可选参数，传递的是time()返回的格式数据，如果传递0，那么会获取到新世纪元的时间起始点，一般是从1970年开始的。

### asctime ctime

asctime返回类似`Sun Jun 20 23:21:05 1993`这样的字符串，如果没有参数，默认使用localtime的返回值。

ctime返回和asctime类似，只不过接收的参数是time函数返回的格式，默认使用time的返回值。

### strptime strftime

两个函数用于格式化和反格式化时间。strptime返回接收格式化时间，返回`struct_time`格式，strftime接受`struct_time`，返回一个字符串表示。

常用的格式化符号有： %Y %m %d %H %M %S(分别表示年月日时分秒)

```
a = strftime("%Y%m%d%H%M%S", localtime()) #其中第二个参数默认是localtime，a打印的结果是20180716181239
b = strptime(a, "%Y%m%d%H%M%S") #返回一个struct_time类型数据
```

## datetime模块

datetime模块包含了date, time, datetime, timedelta, tzinfo, timezone几个类。

其中timedelta是用于datetime其它模块经过计算后的结果，或者作为中间件进行计算。

timedelta虽然可以接受days,hours,minutes,seconds等作为参数，但是只有days,seconds,microseconds可以直接使用，其中hours,minutes都被转换为seconds了。

```
now = datetime.now() #获取当前时间，类似datetime.today()和datetime.fromtimestamp(time.time())，当然datetime也支持构造函数创建对象，可以指定特定的时间点。
per = datetime.now().replace(day=1) #replace会返回一个新的datetime对象

d = now - per #返回的是timedelta对象，(days, seconds, microseconds)
d = now.date() - per.date() #返回timedelta对象，(days)，其中now.date()返回date对象，now.time()返回time对象

now > per #返回True
now.time() > per.time() #返回True
```

> 注意，datetime.time对象不支持加减操作，但是支持大小比较操作。

```
datetime.now().strftime("time format string") #类似time的strftime
datetime.strptime(datetime_obj, "time format string") #这个是类函数，功能类似time的strptime
```

## calender 模块

## 第三方库dateutil

https://pypi.org/project/python-dateutil/

https://dateutil.readthedocs.io/en/stable/

## 个人模块

python的时间处理模块中，可以对hour,day进行加减，但是没有对月进行处理，比如计算当前月的前一个月，前五个月的情况，没有处理，个人单独写了一个函数可以处理。

```
def relative_months(obj, months=0):
    year, month, day = obj.year, obj.month, obj.day
    year_delta, month = divmod(obj.month + months, 12)
    year = obj.year + year_delta
    if month == 0:
        year, month = year - 1, 12

    if month in [2, 4, 6, 9, 11] and obj.day == 31:
        day = 30
    if month == 2 and obj.day > 29:
        try:
            datetime(year, 2, 29)
            day = 29
        except:
            day = 28

    return datetime(year, month, day, obj.hour, obj.minute)
```
