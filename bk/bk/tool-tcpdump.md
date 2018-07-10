---
title: '[tool]-tcpdump使用'
date: 2018-06-05 19:21:18
tags:
categories: 历史
---

tcpdump抓包工具使用手册

<!--more-->

## 根据指定IP地址

``` bash
tcpdump -i eth0 host 192.168.150.1
```

## 根据指定的协议

``` bash
tcpdump -i eth0 udp
```

``` bash
tcpdump -i eth0 icmp
```

