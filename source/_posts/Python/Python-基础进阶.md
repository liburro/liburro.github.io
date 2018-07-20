---
title: Python-基础进阶
date: 2018-07-16 09:44:08
tags:
categories: python
---

本文记录了python的一些基础操作以及基础进阶的内容。包括大多数内建函数，列表，字典等的操作，也包含了sys等模块的一些基础特性。

<!--more-->

## 列表的几个操作

```
a = list()
a.append(1) # 增加一个元素，[1]
a.append([2,3]) #append是添加一个元素，而不管元素内容， 其中[2,3]作为a的第二个元素 [1,[2,3]]
a.extend([2,3]) #extend直接作用于a，并且只能接受列表参数，[1,[2,3],2,3]
a + [1,2,3] #这个返回值是[1,[2,3],2,3,1,2,3]但是a本身仍然没有变化
```

## 集合

```
a = set()
a.add(1) #增加元素1
a.add(1) #再次增加，但是a里面只有有一个1
```

## len函数的替代

先看下面的代码，假设temp是一个列表，或者一个字符串，或者任意的可求长度的类型。

```
sum(1 for _ in temp)

len(temp)
```

上面的两组操作都可以获取temp的长度。

> 注意`_`在python中也是一个有效的变量名。另外在python交互式窗口里面，`_`代表上一次执行的结果。

```
In [25]: a = 1

In [26]: a
Out[26]: 1

In [27]: _
Out[27]: 1

In [28]: b = 2

In [29]: print b
2

In [30]: _
Out[30]: 1

In [31]:
```

## and等运算符

```
1 and 0 #返回0
2 or 1 #返回2
```

## ifelse

```
a = 1 if 1 == 1 else 0
```

## 空值的判断

```
a = None #python里面[],'',0,False等都是非真值
if a:
    do_something()
else:
    do_other_something()
```

## 函数

### 参数类型

```
def f(x: str):
    print(x)


f(1)
```

上面定义函数的时候，使用`x: str`的方式指定了参数x是str类型，对实际效果是没有什么影响的，也就是说，你传什么，就是什么，不会变成str类型，只不过是告诉使用的人，这里应该传递str类型的数据。

## lambda表达式

## 字典的操作

## 解包(`*`和`**`的操作)

## 迭代器

## 生成器

### yield语句

#### 基本操作

```
def test_yield(x):
    for i in range(x):
        a = yield i
        print(a)

a = test_yield(5)
print(next(a))
print(next(a))

print(next(a)) #0
print(next(a)) #None 1
print(a.send(100)) #100 2
print(next(a)) #None 3

0 #首先执行到yield，把后面的表达式i返回了，也就是第一次进入，为0
None #第二次调用next的时候，接着上一次暂停的地方继续执行，打印a，默认为None
1 #然后又执行到了yield的地方，返回了第二次的结果1
100 #然后send了100，a获取到了100，并且执行到了下一次的yield，下一步马上打印2
2 #接上一步
None #最后一个next，a没有值，默认None
3 #执行到了yield地方，打印3
```

#### yield from子句

#### throw函数

## 上下文管理器

### 多重with

```
with A() as a, with B() as b:
    do_some()
```

和下面相等：

```
with A() as a:
    with B() as b:
        do_some()
```

## 装饰器

### 基本

```
def wrapper(f):
    return f
    
@wrapper
def test():
    pass
```

在调用`test()`的时候，调用过程类似`wrapper(test)()`，我们分解一下，首先`test()`最后的`()`和`wrapper(test)()`最后的`()`一致，就是函数调用，那么`wrapper(test)()`中`wrapper()`也是一个函数调用，参数是test，也就是一个函数，返回了f本身，也就是test本身了，然后结合最后的`()`形成了函数调用。

也就是说，紧挨着被包装的函数(这里就是test这个函数)的那一层，必须接受一个参数，这个参数就是被包装的函数，并且需要返回一个可被调用的对象(比如一个函数)。

> 如果@wrapper这个装饰器，没有使用@wrapper(...)这种方式，默认会把所修饰的函数作为参数传递给wrapper，否则如果使用了@wrapper(...)这种方式，会使用@wrapper(...)(fun)方式，其中fun是被修饰的函数。

### 带有参数的装饰器

```
def wrapper(a):
    def inner_fun():
        print(2)
    return inner_fun

@wrapper
def test():
    print(1)

test() # wrapper(test)()
```

上面实际打印结果是2，我们来分解一下，`test()`的调用相当于是， `wrapper(test)()`，那么`wrapper(test)`其实返回的是`inner_fun`，那么就`wrapper(test)()`等于了`inner_fun()`


我们看看下面的情况：

```
def wrapper(a):
    def inner_fun():
        print(2)
    return inner_fun


@wrapper(a=1)
def test():
    print(1)

test() # wrapper(a=1)(test)()
```

分解一下，`test()`相当于`wrapper(a=1)(test)()`，同理最后一个`()`大家一致，`wrapper(a=1)`相当于一次函数调用，返回的是`inner_fun`，拼接起来`wrapper(a=1)(test)()`变成了`inner_fun(test)()`，在分解`inner_fun(test)`也是一次函数调用，但是实际的`inner_fun`不接受任何参数，所以上面的会引发异常。

在看看下面的例子：

```
def wrapper(a):
    print(a)
    def inner_fun(f):
        def deep_fun(*args):
            for i in args:
                print(i)
        return deep_fun
    return inner_fun


@wrapper(a=1)
def test(b):
    print(100)

test(20) # wrapper(1)(test)(b)
```

打印结果是1，20

分解： test(20) ---> wrapper(1)(test)(20) ---> inner_fun(test)(20) ----> deep_fun(20) ----> 最后使用了for循环打印了传递过来的参数

### 多重装饰器

```
def w1(f):
    print('w1')
    def inner():
        print(1)
    return inner

def w2(f):
    print('w2')

@w1
@w2
def test():
    print('test')

test() #w1(w2(test))()
```

打印结果w2, w1, 1

分解： test() --> w1(w2(test)() --> w1(None)() --> inner()

首先最后的括号先不变`()`，然后把test换成调用关系w1(w2(test))，其中w2(test)首先调用，没有返回值，那么默认值None，变成了w1(None)()，w1返回inner，获取到了最后结果。

看一个复杂的：

```
def w1(a):
    print('w1')
    print(a)
    def w1_in(f):
        def w1_deep():
            print('w1_deep')
        return w1_deep
    return w1_in

def w2(a):
    print('w2')
    print(a)
    def w2_in(f):
        print('w2_in')
    return w2_in

@w1(1)
@w2(2)
def test():
    print(3)

test() #w1(1)(w2(2)(test))()
```

最后的打印结果是： w1，1，w2，2，w2_in，w1_deep

首先改写test()，先确保最后的括号`()`，然后替换test，由于装饰器都使用了带有`()`的方式，那么默认参数就不是被修饰的函数了，test的改写过程是，从下往上看， `w2(2)(test)`，在往上`w1(1)(w2(2)(test))`，最后联合`w1(1)(w2(2)(test))()`

分解一下： test() --> w1(1)(w2(2)(test))() --> w1_in(w2(2)(test))() -->
w1_in(w2_in(test))() --> w1_in(None)() --> w1_deep()

### 类装饰器

### 总结

分解是最好理解装饰器的办法，其中函数名先确定好，最后的`()`一定是不变的，另外如果装饰器没有使用带`()`的方式，那么一定会把被修饰的对象作为参数，如果带了`()`，那么一定会返回一个可调用对象，并且接收一个参数，这个参数一定是被修饰的对象，多重修饰，就按照从上往下的方式进行修饰。

## 类相关

### property

### classmethod

### staticmethod

### 类变量

### 继承

#### 特殊成员的继承

包括类变量， class和static method的继承

## import的理解

## type和isinstance

type(obj) 返回的是一个对象的类型，比如type(1)返回int。

isinstance(1, int) 返回True或者False，判断对象是否属于某个类型。

但是isinstance判断不了子类的情况，比如A是一个类，B继承了这个类，b = B()， 那么使用isinstance(b, A)返回的是True，而type(b) == A返回的是False，具体的应用需要根据实际情况选择。

## 特殊的函数和变量

### `__all__`

### `__file__`

### `__name__`

## sys模块基础功能

### 命令行参数

当使用下面的方式执行py脚本的时候：

```
python example.py 1 abc
```

sys模块提供了一个变量`sys.argv`可以获取到命令行参数的值，其中`sys.argv[0]`是这个脚本的名字， `sys.argv[1]`是1，`sys.argv[2]`是abc。

> 其中`sys.argv[0]`这个的值是根据`python example.py 1 abc` 中py文件获取的，也就是说，如果你跟的是路径，那么`sys.argv[0]`也获取到路径，简单理解为python是一个命令，后面的都是参数，传递什么，`sys.argv`就会原封不动的获取到什么。

## python3的坑

#### map对象

在python2里面map返回的是一个列表类型的，但是在python3里面返回的是一个迭代器，那么这里就有影响了。比如python2里面可以这样使用：

```
a = [1,2,3]
b = map(str, a) #这里b是一个列表
for i in b:
    print i
for i in b:
    print i #上面不会影响这里的结果，和上面打印的内容一致
```

我们来看python3:

```
a = [1,2,3]
b = map(str, a) #这里b是一个迭代器
for i in b:
    print(i) #这里和上面一样，123都会打印
for i in b:
    print(i) #注意了，这里不会有任何输出
```

为什么呢，因为第一个for已经把迭代器的元素遍历完了，后面继续遍历就已经没有内容了，同样需要注意，所有可以对这种迭代对象进行操作的函数也会产生这样的效果。比如python3里面:

```
a = [1,2,3]
b = map(str, a) #这里b是一个迭代器
c = ' '.join(b) #把b里面的元素用空格连接，返回一个字符串
for i in b:
    print(i) #注意了，这里不会有任何输出，因为join函数已经把b遍历完了。
```

迭代器的相关详情上面已经记录过，可以回去仔细看看。


