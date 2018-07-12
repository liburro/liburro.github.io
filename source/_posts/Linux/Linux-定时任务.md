---
title: Linux-定时任务
date: 2018-07-12 15:17:06
tags:
categories: Linux
---

本文主要记录了Linux的定时任务，比如crontab。

<!--more-->

## Crontab

我们可以通过crontab命令来配置我们的定时任务，比如每小时，每天，每隔多少分钟(小时，天，周)，每周的第几天等等，有点类似Jenkins上配置定时任务的功能，语法也类似。

### 创建定时任务

首先通过命令`crontab -e`会进入crontab的编辑界面，里面的操作就类似于使用vim编辑文件一样，只是需要遵守格式即可，格式如下：

```
10 * * * * task #没小时的10分钟的时候执行task
30 10 * * * task #每天的10:30分执行task
```

```
crontal -l #查看crontab里面配置的定时任务
```

### 任务跟踪

可以通过下面的命令查看crontab的日志

```
tail -n 100 /var/spool/mail/root #这个会把crontab的运行日志记录在里面，当然这里使用tail命令只查看了最后的100行

tail -f /var/log/cron #这个是查看crontab执行了哪些任务的跟踪
```