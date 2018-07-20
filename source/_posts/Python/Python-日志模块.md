---
title: Python-日志模块
date: 2018-07-16 10:16:51
tags:
categories: python
---

本文主要记录了标准版模块里面的logging模块的一些使用方法。

<!--more-->

## 个人的log库

```
import os
import logging
import logging.handlers


__all__ = [
    'set_log_level',
    'set_log_path',
    'debug_log',
    'info_log',
    'warn_log',
    'error_log',
    'except_log',
]


class SksLog(object):
    """
    使用方法：

    from log import *
    set_log_path(path/to/log)
    set_log_level(level)
    debug_log(msg)
    info_log(msg)

    说明：
    1. set_log_path和set_log_level用于设定log输出路径和日志级别(日志级别只针对标准输出有效，文件日志里面会包含全部日志信息)
    这两者默认值是：路径为本文件(log.py)存在路径，日志级别为WARN，如果需要手动设定，这两个函数必须在调用任何*_log函数之前，
    一旦有任何*_log函数被执行，后期在设置路径或者级别均无效；
    2. 日志存放大小默认为2M，默认保存5个日志。
    """

    def __init__(self, level='WARN'):
        self.LEVEL_CONF = {
            'DEBUG': logging.DEBUG,
            'INFO': logging.INFO,
            'WARN': logging.WARN,
            'ERROR': logging.ERROR
        }
        self.level = self.LEVEL_CONF[level]
        self.path = os.path.join(os.path.dirname(os.path.abspath(__file__)), 'sks_log.log')
        self._logger = None
        self.level_change = False
        self.path_change = False
        self.new_level = False
        self.new_path = False

    def set_log_level(self, level):
        # 必须在任何调用log输出之前设定，一旦使用默认启动(也即使没有调用本函数)，后期的任何修改无效。
        level = level.upper()
        self.new_level = self.LEVEL_CONF[level]
        if self.level != self.new_level:
            self.level = self.new_level
            self.level_change = True

    def set_log_path(self, path=None):
        # 必须在任何调用log输出之前设定，一旦使用默认启动(也即使没有调用本函数)，后期的任何修改无效。
        self.new_path = path if path else self.path
        if self.path != self.new_path:
            self.path = self.new_path
            self.path_change = True

    @property
    def logger(self):
        if self._logger and self.level_change:
            for i in self._logger.handlers:
                print(type(i))
                if type(i) == logging.StreamHandler:
                    i.setLevel(self.new_level)
                    break
            self.level_change = False

        if self._logger and self.path_change:
            for i in self._logger.handlers:
                if type(i) == logging.handlers.RotatingFileHandler:
                    i.baseFilename = self.new_path
                    break
            self.path_change = False

        if self._logger:
            return self._logger
        self._logger = logging.getLogger('sks_logger')
        handler = logging.StreamHandler()
        handler.setLevel(self.level)
        formatter = logging.Formatter(
            '%(asctime)s [SKS-%(levelname)s]  %(message)s', datefmt="%Y-%m-%d %H:%M:%S")
        handler.setFormatter(formatter)
        self._logger.addHandler(handler)
        handler = logging.handlers.RotatingFileHandler(self.path, maxBytes=1024, backupCount=5)
        handler.setLevel(logging.DEBUG)
        formatter = logging.Formatter(
            '%(process)d %(module)s %(funcName)s %(lineno)d %(asctime)s [SKS-%(levelname)s]  %(message)s')
        handler.setFormatter(formatter)
        self._logger.addHandler(handler)
        self._logger.setLevel(logging.DEBUG)
        return self._logger

    def debug(self, msg):
        self.logger.debug(msg)

    def info(self, msg):
        self.logger.info(msg)

    def warn(self, msg):
        self.logger.warning(msg)

    def error(self, msg):
        self.logger.error(msg)

    def exception(self, msg):
        self.logger.exception(msg)


log = SksLog()
set_log_level = log.set_log_level
set_log_path = log.set_log_path
debug_log = log.debug
info_log = log.info
warn_log = log.warn
error_log = log.error
except_log = log.exception


def _test():
    set_log_level('debug')
    set_log_path('D:\\pycharm_wk\\testlog\\x.log')
    debug_log('test debug')


if __name__ == '__main__':
    _test()


```

## 个人lib第二版

```
import os
import logging
import logging.handlers
import tempfile


__all__ = [
    'set_log_level',
    'set_log_path',
    'debug_log',
    'info_log',
    'warn_log',
    'error_log',
    'except_log',
]


class SksLog(object):
    """
    使用方法：

    from log import *
    set_log_path(path/to/log)
    set_log_level(level)
    debug_log(msg)
    info_log(msg)

    说明：
    1. set_log_path和set_log_level用于设定log输出路径和日志级别(日志级别只针对标准输出有效，文件日志里面会包含全部日志信息)
    这两者默认值是：路径为本文件(log.py)存在路径，日志级别为WARN，如果需要手动设定，这两个函数必须在调用任何*_log函数之前，
    一旦有任何*_log函数被执行，后期在设置路径或者级别均无效；
    2. 日志存放大小默认为2M，默认保存5个日志。
    """

    def __init__(self, level='WARN'):
        self.LEVEL_CONF = {
            'DEBUG': logging.DEBUG,
            'INFO': logging.INFO,
            'WARN': logging.WARN,
            'ERROR': logging.ERROR
        }
        self.level = self.LEVEL_CONF[level]
        self.path = os.path.join(tempfile.gettempdir(), 'sks_log.log')
        self._logger = None
        self.level_can_be_changed = True
        self.path_can_be_changed = True

    def set_log_level(self, level):
        # 必须在任何调用log输出之前设定，一旦使用默认启动(也即使没有调用本函数)，后期的任何修改无效。
        if not self.level_can_be_changed:
            return
        level = level.upper()
        new_level = self.LEVEL_CONF[level]
        if self.level != new_level:
            for i in self._logger.handlers:
                if type(i) == logging.StreamHandler:
                    i.setLevel(new_level)
                    self.level = new_level
                    self.level_can_be_changed = False
                    break

    def set_log_path(self, path=None, remove_old_file=True):
        # 必须在任何调用log输出之前设定，一旦使用默认启动(也即使没有调用本函数)，后期的任何修改无效。
        if not self.path_can_be_changed:
            return
        new_path = path if path else self.path
        if self.path != new_path:
            if remove_old_file:
                for i in self._logger.handlers:
                    if type(i) == logging.handlers.RotatingFileHandler:
                        self._logger.removeHandler(i)
                        break
            handler = logging.handlers.RotatingFileHandler(new_path, maxBytes=1024 * 1024 * 2, backupCount=5)
            handler.setLevel(logging.DEBUG)
            formatter = logging.Formatter('%(asctime)s [SKS-%(levelname)s] %(filename)s %(process)d %(thread)d %(threadName)s %(module)s %(funcName)s %(lineno)d:  %(message)s')
            handler.setFormatter(formatter)
            self._logger.addHandler(handler)
            self.path = new_path
            self.path_can_be_changed = False

    @property
    def logger(self):
        if self._logger:
            return self._logger
        self._logger = logging.getLogger('sks_logger')
        handler = logging.StreamHandler()
        handler.setLevel(self.level)
        formatter = logging.Formatter(
            '%(asctime)s [SKS-%(levelname)s]:  %(message)s', datefmt="%Y-%m-%d %H:%M:%S")
        handler.setFormatter(formatter)
        self._logger.addHandler(handler)
        handler = logging.handlers.RotatingFileHandler(self.path, maxBytes=1024 * 1024 * 2, backupCount=5)
        handler.setLevel(logging.DEBUG)
        formatter = logging.Formatter('%(asctime)s [SKS-%(levelname)s] %(filename)s %(process)d %(thread)d %(threadName)s %(module)s %(funcName)s %(lineno)d:  %(message)s')
        handler.setFormatter(formatter)
        self._logger.addHandler(handler)
        self._logger.setLevel(logging.DEBUG)
        return self._logger


log = SksLog()
set_log_level = log.set_log_level
set_log_path = log.set_log_path
debug_log = log.logger.debug
info_log = log.logger.info
warn_log = log.logger.warning
error_log = log.logger.error
except_log = log.logger.exception


def _test():
    set_log_level('debug')
    set_log_path('D:\\pycharm_wk\\testlog\\x.log')
    debug_log('test debug')


if __name__ == '__main__':
    _test()


```

## level

logger和handler都有setLevel，其中logger是设置哪些级别的日志会传递给handler，handler是设置我接收哪些级别的日志，比如logger设置了debug，证明所有debug以上的信息都会传递到handler那边，handler如果设置了warn级别，那么只有warn级别的日志才会处理，比如写进文件，但是如果有多个handler,并且大家级别不一样，比如handler1是debug的，handler2是warn的，那么来的debug日志，只有handler1会处理，handler2不会处理，但是来的是warn日志，那么两者都会处理

## 基本

`logger = logging.getLoger(name)` 可以获取一个logger，如果没有，则会创建它，是根据这个name进行判断是否有这个logger，并且这个name可以有继承关系，比如a.b，那么创建的这个logger，如果以前有个logger叫a，那么这里创建的a.b会是a的子类logger，这个和类的关系差不多，比如有创建了一个a.b.c，c就是b的子类一样。

`logger.addHandler(handler)` 这里可以为logger增加一个handler，也就是处理器，处理logger传递过来的消息，默认是会创建一个标准输出的handler的。可以创建多个handler。

`logger.setLevel(logging.WARN)` 设置logger处理日志的级别

`handler.setLevel(logging.ERROR)` 设置handler处理日志的级别

区别查看level章节的说明。

`handler.setFormatter(formatter)` 增加格式器，也就是输出的日志的格式， 每个handler可以设置不同的格式

`formatter = logging.Formatter(fmt)` 设置格式，有很多格式可以指定，下面会列举出来

`handler = logging.StreamHanlder()` 创建一个标准输出的handler

当然还有很多的handler，比如日志回滚的handler，输出到文件，输出到socket等等，下面也会列举很多。

`logger.hasHandler()` 是否包含handler

`logger.handlers` 一个列表，包含了logger的所有handler

`logger.removeHanler(handler)` 移除handler

### formatter

```
%(name)s            Name of the logger (logging channel)
%(levelno)s         Numeric logging level for the message (DEBUG, INFO,
                    WARNING, ERROR, CRITICAL)
%(levelname)s       Text logging level for the message ("DEBUG", "INFO",
                    "WARNING", "ERROR", "CRITICAL")
%(pathname)s        Full pathname of the source file where the logging
                    call was issued (if available)
%(filename)s        Filename portion of pathname
%(module)s          Module (name portion of filename)
%(lineno)d          Source line number where the logging call was issued
                    (if available)
%(funcName)s        Function name
%(created)f         Time when the LogRecord was created (time.time()
                    return value)
%(asctime)s         Textual time when the LogRecord was created
%(msecs)d           Millisecond portion of the creation time
%(relativeCreated)d Time in milliseconds when the LogRecord was created,
                    relative to the time the logging module was loaded
                    (typically at application startup time)
%(thread)d          Thread ID (if available)
%(threadName)s      Thread name (if available)
%(process)d         Process ID (if available)
%(message)s         The result of record.getMessage(), computed just as the record is emitted
```

使用方式类似：

```
formatter = logging.Formatter('%(asctime)s [SKS-%(levelname)s] %(message)s',datefmt="%Y-%m-%d %H:%M:%S")
```

> datefmt 参数和time.strftime一致，对asctime的输出会产生影响

### handler

```
StreamHandler：logging.StreamHandler；日志输出到流，可以是sys.stderr，sys.stdout或者文件
FileHandler：logging.FileHandler；日志输出到文件
BaseRotatingHandler：logging.handlers.BaseRotatingHandler；基本的日志回滚方式
RotatingHandler：logging.handlers.RotatingHandler；日志回滚方式，支持日志文件最大数量和日志文件回滚
TimeRotatingHandler：logging.handlers.TimeRotatingHandler；日志回滚方式，在一定时间区域内回滚日志文件
SocketHandler：logging.handlers.SocketHandler；远程输出日志到TCP/IP sockets
DatagramHandler：logging.handlers.DatagramHandler；远程输出日志到UDP sockets
SMTPHandler：logging.handlers.SMTPHandler；远程输出日志到邮件地址
SysLogHandler：logging.handlers.SysLogHandler；日志输出到syslog
NTEventLogHandler：logging.handlers.NTEventLogHandler；远程输出日志到Windows NT/2000/XP的事件日志
MemoryHandler：logging.handlers.MemoryHandler；日志输出到内存中的指定buffer
HTTPHandler：logging.handlers.HTTPHandler；通过"GET"或者"POST"远程输出到HTTP服务器
```