---
title: '[专项]-python'
date: 2018-06-25 16:46:29
tags:
categories: 历史
---

本文主要记录了python的相关编程笔记。本文主要以python2.7为主，后面会增加python2与python3的差异补充说明。

<!--more-->

## 字典

### 概念

字典是一种键值对的方式表示的数据类型，是key-value的方式存在的，key是唯一的，如果后期重新给这个key赋值，前面的值是被覆盖掉的， python的字典是__无序__的。

> 注意， 如果使用`{}`的方式表示，需要与set类型进行区分，如果`{}`里面没有内容，那么产生的是一个字典，如果`{1, 2, 3}`这种方式，产生的是一个集合(set)。

### 判断字典是否包含某个key

使用`in`来代替函数`has_key`。

``` python
testdict = {'a': 1, 'b':2}

if 'b' in testdict:
    print '字典中包含b这个key'
```

### 遍历字典的key

``` python
testdict = {'a': 1, 'b':2}

for i in testdict:
    print i
```

### 遍历字典的value

``` python
testdict = {'a': 1, 'b':2}

for i in testdict.itervalues():
    print i
```

> 注意， python3的字典对象没有`iterxxx`类似的的函数了，比如`itervalues()`被替换为`values()`了，`values()`在python2里面也存在，但是python3里面的`values()`行为和python2里面的`itervalues()`行为类似了。

### 获取key对应的value

``` python
testdict = {'a': 1, 'b':2}

print testdict['b']

print testdict['c'] #其中这里会报错，因为字典testdict中没有c这个key。
```

``` python
testdict = {'a': 1, 'b':2}

print testdict.get('a', None)

print testdict.get('c', 3) #这里会输出3
```

上面的两种方式区别： 方式一是直接通过`[]`获取，那么这个key就必须存在于字典中，如果不存在，会报错，方式二是通过函数`get(key, value=None)`来获取，其中第一个参数是需要获取的key，如果这个key存在于字典中，那么返回这个key对应的实际的vlaue，否则返回第二个参数的值，默认是`None`。

### setdefault

`setdefault(key, value=None)`是一个函数，它的作用是，如果key存在于字典中，则返回字典的实际值，如果不存在，会给字典增加一个key，并且设置对应的值为value，默认是`None`，并且返回这个值。

``` python
testdict = {'a': 1, 'b':2}

print testdict.setdefault('a') #这里输出1

print testdict.setdefault('c') #这里输出None,因为'c'不在testdict中，并且没有传递第二个参数，使用None，并且现在testdict变成了 {'a': 1, 'b':2, 'c': None}

print testdict.setdefault('d', 4) #这里输出4，并且testdict变成了 {'a': 1, 'b':2, 'c': None, 'd': 4}
```

### 同时遍历key和vlaue

``` python
testdict = {'a': 1, 'b':2}

for key, value in testdict.iteritems():
    print key, value
```

> 注意， python3里面的`items`函数类似python2里面的`iteritems`函数。

## 上下文管理器

### 介绍

我们知道很多操作步骤完成后，需要有一个清理的动作，比如使用`open()`打开了一个文件后，不管是读还是写的操作完成后，我们都需要调用`close()`关闭这个资源。
有下面几种操作方式：

```
fp = open('file', 'r')
fp.read()
fp.close()
```

上面一种情况，如果在`read()`部分抛出了异常，那么`close`是不会执行的，也就是不能释放资源，这是很危险的动作。那么引申出了下面的用try来捕获异常的操作:

```
fp = open('file', 'r')
try:
    fp.read()
except Exception as e:
    print e.message
finally:
    if fp:
        fp.close()
```

其中finally里面的内容是一定会被执行的，也就是一定会释放资源。

其实python提供了上下文管理器对这个操作进行更为方便的管理，对应的就是`with`语句。

```
with open('file', 'r') as fp:
    fp.read()
```

通过`with`语句，会自动的关闭掉对应的资源。

### 实现自己的上下文管理器

python里面的通过类的方式来实现上下文管理器的。设计到两个method: `__exit__ __enter__`，通过`with`调用我们的类后，会执行到`__enter__`中。

```
class TestWith(object):
    def __enter__(self):
        return self
        
    def __exit__(self, exc_type, exc_value, tb):
        #参数exc_type, exc_value, tb分别表示异常类型，异常的值，跟踪对象，如果没有异常发生，默认都是None
        pass
        
    def fun(self):
        pass
```

假如有上面一个类，那么我们可以通过如下的方式进行调用：

```
with TestWith() as test:
    test.func()
```

> __注意1__， 如果类有`__init__`，一定会先执行这个，在执行`__enter__`， 也就是执行顺序是 `__init__` --> `__enter__` --> `with`里面的语句 --> `__exit__`。
> __注意2__， 如果在`__init__`中抛出了异常，那么是不会继续往后执行的
> __注意3__， 默认情况下，如果在`with`里面有异常发生， `__exit__`会捕捉到一场，并且三个参数对应了异常的各个值， 并且这个异常会继续抛出， 但是如果`__exit__`里面返回了真值，
那么这个异常会被抑制，不会向上抛出。
> __注意4__， 就算在`with`里面使用了`return`语句，`__exit__`仍然会被执行。

## 装饰器

python里面可以使用`@`语法糖来包装一个函数或者类。

先来看一个优先级规则: 假如fun是一个函数，没有参数，返回的也是一个函数，那么`fun()()`这种形式就是`fun`和第一个`()`结合，也就是`fun()`这是一个函数调用，返回的是一个函数，比如fun1，
这个时候就变成了`fun1()`，这里的`()`是`fun()()`里面的第二个括号，也是一个函数调用的关系。

形式大致如下：

```
def fun1(f):
    return f
    
@fun1
def test():
    print 'hehe'
    
test()
```

其中`fun1`只接收1个参数，这个参数是一个函数，在`test`函数上面使用了`@fun1`，那么我们在调用`test()`的时候，实际是调用了`fun1(test)()`。

那么上个例子中，实际调用`fun1(test)`返回的是一个函数，就是`test`本身，所以没有什么区别，如果返回的是另外的函数，那么当你调用`test()`的时候，函数的行为就变为新的的。

上一个例子打印的是hehe， 下面的例子打印的就是haha了。因为在`fun1`中，返回的是`inf`这个函数。

```
def fun1(f):
    def inf():
        print 'haha'
    return inf
    
@fun1
def test():
    print 'hehe'
    
test()
```

如果在`@`中，类似`@fun1`这种形式，那么证明`fun1`就是真正的装饰器，如果使用了`@fun1()`这种形式(可以带参数)，那么证明`fun1`并不是真正的装饰器，记住一点，装饰器函数只接收一个参数，就是被修饰的函数对象。

比如下面的例子会报错:

```
def fun1(f):
    def inf():
        print 'haha'
    return inf
    
@fun1()
def test():
    print 'hehe'
    
test()
```

因为使用了`@fun1()`的方式，实际的调用情况是`fun1()(test)()`，首先，`fun1`需要1个参数，这里没有传递，那么这里会报错。

```
def fun1(f):
    def inf():
        print 'haha'
    return inf
    
@fun1(1)
def test():
    print 'hehe'
    
test()
```

这个同理，调用方式为`fun1(1)(test)()`，首先`fun1(1)`返回的是`inf`，这个不接受任何参数，所以报错。

```
def fun1(f):
    def inf(x):
        print 'haha'
    return inf
    
@fun1(1)
def test():
    print 'hehe'
    
test()
```

这个同理，调用方式为`fun1(1)(test)()`，首先`fun1(1)`返回的是`inf`，接收一个参数，返回空(因为inf里面只有一句print，没有返回，python默认返回None)，报错。

```
def fun1(f):
    def inf(x):
        print 'haha'
        return x
    return inf
    
@fun1(1)
def test():
    print 'hehe'
    
test()
```

这个就对了，调用方式为`fun1(1)(test)()`，首先`fun1(1)`返回的是`inf`，接收一个参数，返回x，这个x就是我们的原函数test，这里会打印haha然后打印hehe。

### property

> python中有一个非常有用的装饰器函数`property`，看下面的例子

```
class Student(object):
    def __init__(self, age):
        self._age = age
    
    @property
    def age(self):
        return self._age
    
    @age.setter
    def age(self, age): #如果这里不配置setter，那么age就变成了只读的属性
        if -1 < age < 200:
            self._age = age
    
s1 = Student(5)
print s1.age
s1.age = 1000
print s1.age
```

看官方给的如下例子:

```
class C(object):
    def __init__(self):
        self._x = None

    def getx(self):
        return self._x

    def setx(self, value):
        self._x = value

    def delx(self):
        del self._x

    x = property(getx, setx, delx, "I'm the 'x' property.")
```

```
class C(object):
    def __init__(self):
        self._x = None

    @property
    def x(self):
        """I'm the 'x' property."""
        return self._x

    @x.setter
    def x(self, value):
        self._x = value

    @x.deleter
    def x(self):
        del self._x
```

## 常用库

### os.path

#### os.path.dirname

这个函数接收一个路径作为参数，并且返回这个参数所在的目录。

比如`os.path.dirname("C:\test\file.txt")` 会返回 C:\test，取得了file.txt文件的目录名。

> 注意，这个函数返回的是相对于执行python的时候的路径，比如说/root/1/2/3/test.py里面的代码是`os.path.dirname(__file__)`， 那么根据执行这个test.py的路径情况，会返回不同的结果，
如果是通过`python /root/1/2/3/test.py`方式执行， 那么返回/root/1/2/3，如果是在3目录下执行`python test.py`返回的是空，如果是在root/1目录下执行，返回2/3，如果需要获取全路径，使用`os.path.abspath(__file__)`

### subprocess

里面有一个类Popen 其中第一个参数是需要执行的命令，还有stdout=subprocess.PIPE, stderr=subprocess.PIPE，他们的默认值是None，如果不配置为PIPE，是无法通过popen对象
的communicate获取输出的，如果是None，默认输出到屏幕，如果设置为PIPE，可以通过communicate函数获取输出，communicate也接收一个参数作为输入，此时需要制定stdin也等于PIPE
，communicate返回一个二元组，第一个是stdout，第二个是stderr，有可能为空，poll是检查子进程是否结束，结束返回指定的命令返回代码，否则返回None，wait同理，但是会一直等，poll
不会，kill可以杀死进程。

### sys

```
reload(sys)
sys.setdefaultencoding('utf8')
```

### telnet

如果要下发ctrl+c，代码是`\x03`，引申ctrl+a，是`\x01`

### random

random是产生随机数的库。

产生一个int的随机数
```
random.randint(-100， 100) #会产生一个-100到100之间的随机数，包括-100和100
```

从一个序列中随机选择一个
```
random.choice(range(100)) #从0-99中随机选择一个
random.choice('abcde') #从abcde中随机选择一个
```

从一个序列中随机选择出一个新的序列
```
random.sample(range(100), 5) #从0-99中选择5个数组成一个新的序列
```

注意`sample`的第二个参数不能大于第一个序列的长度，`sample`不会重复选择同一个元素。

```
random.random() #会产生一个0.0 <= a < 1.0之间的浮点数
random.uniform(1, 5) #会产生一个1到5之间的浮点数
```

## 环境配置

如果在CentOS下没有pip安装工具，可以通过`yum -y install epel-release`，然后`yum install python-pip`安装pip。