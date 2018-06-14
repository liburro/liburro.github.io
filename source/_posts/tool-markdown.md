---
title: '[tool]-markdown'
date: 2018-06-14 16:00:00
tags:
categories: 工具
---

主要记录了markdown的基本用法。

<!--more-->

## 概述

Markdown 的目标就是 易读易写，她的语法有一些text-to-HTML格式，Markdown的语法由一些精挑细选的符号组成，Markdown不是想要取代HTML，因为HTML已经很简单了，她们的区别就是，HTML是一种发布格式，Markdown是一种书写格式，Markdown兼容HTML，如果需要的格式不在Markdown范围内，你可以使用HTML来代替，直接写在你的Markdown文档里面即可。

## 工具安装

如果要编写markdown，最好是有一款能马上看见成果的工具，推荐配合python和jupyter使用。

首先你需要安装python。

然后通过pip安装jupyter

``` python
pip install jupyter
```

然后通过命令行

``` bash
jupyter notebook
```

就可以打开浏览器看见我们的jupyter启动啦，里面不仅仅可以调试python，还可以编写markdown，还可以两者一起使用，配合插件，还可以把编写的文档转换为html，rts，pdf等各种文件格式

## 基础语法

### 标题

使用`#`来表示标题，根据`#`的数量的多少来决定标题的分阶。`#`后面空一格，后面的内容就是标题内容

比如：

``` markdown
# 这是H1
## 这是H2
```

当然你也可以在最后加上`#`，这不是强制要求，而且最后的数量也不限制，只有开始的`#`会限制标题的阶数

### 区块引用

效果如下：

> 这是区块1

>> 这是子区块

他们源码是：

``` markdown
> 这是区块1

>> 这是区块2
```

注意其中的空行，空行在markdown里面是很重要的一个区分的概念。

### 列表

#### 无序列表

效果如下：

* Red
* Green
* Blue

源码是:

``` markdown
* Red
* Green
* Blue
```

当然上面的`*`也可替换成`+` 或 `-` 都是一样的效果

#### 有序列表

效果如下：

1. Bird
1. Mchale
2. Parish

源码：

``` markdown
1. Bird
1. Mchale
2. Parish
```

很奇怪？ 明明是1 1 2，对的，markdown会自动给你的有序列表安排序号，不管你怎么写。

### 代码块
如果想在一个段落里单独加入一些代码块，比如`printf()`，可以使用 \`printf()\`，这里的符号是键盘上TAB键上面那个。

效果2，下面是一整块的代码块

``` python
def testpython(para):
    if para:
        print para

    return None
```

只需要在代码段的前后分别加入三个\`即可

### 分割线

在一行中使用三个以上的星号，减号，底线来建立一个分割线，行内不能有其它东西，比如：

---

上面有一个分割线

### 强调

效果：

This is *em* and this is **strong**.

源码是：

```
This is *em* and this is **strong**.
```

### 超链接

比如，本文的参考链接是: [markdown语法说明](https://www.appinn.com/markdown/)

源码：

```
[markdown语法说明](https://www.appinn.com/markdown/)
```

另外一种格式: [百度][someid]

[someid]: http://www.baidu.com

源码：

```
[百度][someid]
```

在文章的任意地方，当然最好是文章的底部加入：

```
[someid]: http://www.baidu.com
```

备注： 如果链接的是图片的话，直接在左边的方括号前面加上一个感叹号即可，然后圆括号里面放入图片的地址，可以是本地的，也可以是远程的。
