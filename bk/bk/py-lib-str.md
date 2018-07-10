---
title: '[python]-字符串操作'
date: 2018-05-02 14:04:16
tags:
categories: 历史
---

本文主要讲解一下python字符串的相关操作，包括string库的一些实用。

<!--more-->

## 常量字符串的操作

常量字符串，就是我们定义的一个字符串，它们是一个常量，也就是不可改变的。

比如：

``` python
var = "test_str"
```

这里把常量字符串`test_str`赋值给了变量`var`，其中`test_str`这是一个不可改变的，`var`只是一个变量，可以被重新赋值。

### strip

https://docs.python.org/2/library/stdtypes.html#str.strip
https://docs.python.org/2/library/string.html#string.strip

涉及到3个函数，`strip`, `lstrip`, `rstrip`，同理string库里面也有这3个函数，`strip`是复制当前字符串，把当前字符串的前后的特定的字符给去掉后返回一个新的字符串，默认去掉的字符是空格。`lstrip`是去掉左边特定的字符，`rstrip`是去掉右边的特定字符。
需要去掉的字符可以是多个，当遇到第一个不是去掉的字符的时候停止，比如'testxxxtest'要去掉里面的"test"几个字符，`strip`函数会匹配第一个t发现在去掉的字符中，e s t几个也在，所以都去掉，遇到了x不在test中则停止。

``` python
" test ".strip() #这里返回test
"test".strip('t') #这里返回es
"testxxxtest".strip("test") #这里返回xxx
"mississippi".rstrip("ipz") #返回mississ
"www.example.com".lstrip("cmowz.") #返回example.com

import string
string.strip(" test ") #返回test，其它几个使用类似
```

### split

https://docs.python.org/2/library/stdtypes.html#str.split
https://docs.python.org/2/library/string.html#string.split

这个函数是用于分割字符串，按照指定的字符把字符串分割，返回一个分割后的列表，同理也有`lsplit`和`rsplit`函数，同理string库里面也有相同的函数可以操作。

默认的分隔符是空格。

``` python
"1 2 3 4".split() #返回['1', '2', '3', '4']

import string
string.split("1 2 3 4") #返回['1', '2', '3', '4']
```

## 字符串的格式化

### %s

``` python
string="hello" 
   
#%s打印时结果是hello  
print "string=%s" % string   # output: string=hello  
   
#%2s意思是字符串长度为2，当原字符串的长度超过2时，按原长度打印，所以%2s的打印结果还是hello  
print "string=%2s" % string   # output: string=hello  
   
#%7s意思是字符串长度为7，当原字符串的长度小于7时，在原字符串左侧补空格，  
#所以%7s的打印结果是 hello  
print "string=%7s" % string   # output: string= hello  
   
#%-7s意思是字符串长度为7，当原字符串的长度小于7时，在原字符串右侧补空格，  
#所以%-7s的打印结果是 hello  
print "string=%-7s!" % string   # output: string=hello !  
   
#%.2s意思是截取字符串的前2个字符，所以%.2s的打印结果是he  
print "string=%.2s" % string  # output: string=he  
   
#%.7s意思是截取字符串的前7个字符，当原字符串长度小于7时，即是字符串本身，  
#所以%.7s的打印结果是hello  
print "string=%.7s" % string  # output: string=hello  
   
#%a.bs这种格式是上面两种格式的综合，首先根据小数点后面的数b截取字符串，  
#当截取的字符串长度小于a时，还需要在其左侧补空格  
print "string=%7.2s" % string  # output: string=   he  
print "string=%2.7s" % string  # output: string=hello  
print "string=%10.7s" % string # output: string=   hello  
   
#还可以用%*.*s来表示精度，两个*的值分别在后面小括号的前两位数值指定  
print "string=%*.*s" % (7,2,string)   # output: string=   he
```

### %d

``` python
num=14 
   
#%d打印时结果是14  
print "num=%d" % num      # output: num=14  
   
#%1d意思是打印结果为1位整数，当整数的位数超过1位时，按整数原值打印，所以%1d的打印结果还是14  
print "num=%1d" % num      # output: num=14  
   
#%3d意思是打印结果为3位整数，当整数的位数不够3位时，在整数左侧补空格，所以%3d的打印结果是 14  
print "num=%3d" % num      # output: num= 14  
   
#%-3d意思是打印结果为3位整数，当整数的位数不够3位时，在整数右侧补空格，所以%3d的打印结果是14_  
print "num=%-3d" % num     # output: num=14_  
   
#%05d意思是打印结果为5位整数，当整数的位数不够5位时，在整数左侧补0，所以%05d的打印结果是00014  
print "num=%05d" % num     # output: num=00014  
   
#%.3d小数点后面的3意思是打印结果为3位整数，  
#当整数的位数不够3位时，在整数左侧补0，所以%.3d的打印结果是014  
print "num=%.3d" % num     # output: num=014  
   
#%.0003d小数点后面的0003和3一样，都表示3，意思是打印结果为3位整数，  
#当整数的位数不够3位时，在整数左侧补0，所以%.3d的打印结果还是014  
print "num=%.0003d" % num    # output: num=014  
   
#%5.3d是两种补齐方式的综合，当整数的位数不够3时，先在左侧补0，还是不够5位时，再在左侧补空格，  
#规则就是补0优先，最终的长度选数值较大的那个，所以%5.3d的打印结果还是 014  
print "num=%5.3d" % num     # output: num= 014  
   
#%05.3d是两种补齐方式的综合，当整数的位数不够3时，先在左侧补0，还是不够5位时，  
#由于是05，再在左侧补0，最终的长度选数值较大的那个，所以%05.3d的打印结果还是00014  
print "num=%05.3d" % num    # output: num=00014  
   
#还可以用%*.*d来表示精度，两个*的值分别在后面小括号的前两位数值指定  
#如下，不过这种方式04就失去补0的功能，只能补空格，只有小数点后面的3才能补0  
print "num=%*.*d" % (04,3,num) # output: num= 014
```

### %f

``` python
import math  
   
#%a.bf，a表示浮点数的打印长度，b表示浮点数小数点后面的精度  
   
#只是%f时表示原值，默认是小数点后5位数  
print "PI=%f" % math.pi       # output: PI=3.141593  
   
#只是%9f时，表示打印长度9位数，小数点也占一位，不够左侧补空格  
print "PI=%9f" % math.pi      # output: PI=_3.141593  
   
#只有.没有后面的数字时，表示去掉小数输出整数，03表示不够3位数左侧补0  
print "PI=%03.f" % math.pi     # output: PI=003  
   
#%6.3f表示小数点后面精确到3位，总长度6位数，包括小数点，不够左侧补空格  
print "PI=%6.3f" % math.pi     # output: PI=_3.142  
   
#%-6.3f表示小数点后面精确到3位，总长度6位数，包括小数点，不够右侧补空格  
print "PI=%-6.3f" % math.pi     # output: PI=3.142_  
   
#还可以用%*.*f来表示精度，两个*的值分别在后面小括号的前两位数值指定  
#如下，不过这种方式06就失去补0的功能，只能补空格  
print "PI=%*.*f" % (06,3,math.pi)  # output: PI=_3.142
```

## 参考链接
http://www.jb51.net/article/134290.htm
