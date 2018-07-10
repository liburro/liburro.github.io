---
title: '[tool]-git的使用'
date: 2018-06-05 18:56:22
tags:
categories: 历史
---

git使用手册

<!--more-->

## 基本使用

### 多重库

如果一个库里面包含另外一个库，比如A库里面有一个子库B，可以在A库的.ignore里面增加子库B，那么A库和B库就可以单独分离

### 新建服务器分支

``` bash
git push origin new_branch
```

直接上传新的分支即可

### 分支合并

在需要合并的分支上执行下面的命令即可，比如master分支，当在分支dev中开发完成后，需要合并到master分支上，先切换到master分支，然后进行合并即可。

``` bash
git checkout master
git merge new_branch
```

### 恢复

返回add之前

``` bash
git reset --soft HEAD^
```

还有hard mixed几种方式

### 修改提交的log

``` bash
git commit --amend
```

### 分支

### 提交

``` bash
git add --all #add所有修改
git add .    #类似--all
git add spc_file #add特定的文件spc_file
```

``` bash
git commit -m "msg" #提交add之后的修改到repo，并且增加消息msg
```

``` bash
git push #直接上传
git push origin master #上传到master分支
git push origin dev #上传到dev分支
```
