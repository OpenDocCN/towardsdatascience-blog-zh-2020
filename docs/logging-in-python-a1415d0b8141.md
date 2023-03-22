# Python 日志记录简介

> 原文：<https://towardsdatascience.com/logging-in-python-a1415d0b8141?source=collection_archive---------25----------------------->

## 编程；编排

## 如何用 Python 记录消息，避免打印语句？

![](img/4045969b7a6f00b2088f6befd45b10eb.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3088958) 的 [xresch](https://pixabay.com/users/xresch-7410129/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3088958)

日志可能是任何软件开发中最被低估的方面。日志记录通过记录程序的事件来帮助开发健壮的程序。日志记录有多种用途，从调试到监控应用程序。

Python 的标准库提供了一个日志模块。在本文中，我将向您展示如何将日志添加到您的程序中，并开发更好的应用程序。

要在应用程序中使用日志记录，请导入日志记录模块

```
import logging
```

## 日志记录级别

日志模块预定义了 5 个严重级别，如下所示:

```
┌──────────┬───────┐
│  **Level** │ **Value** │
├──────────┼───────┤
│ CRITICAL │    50 │
│ ERROR    │    40 │
│ WARNING  │    30 │
│ INFO     │    20 │
│ DEBUG    │    10 │
└──────────┴───────┘
```

默认级别是警告，这意味着只记录该级别及以上的事件。

日志模块提供了一组简单易用的函数。如下例所示:

```
import logging

logging.debug('Debug message')
logging.info('Info message')
logging.warning('Warning message')
logging.error('Error message')
logging.critical('Critical message')
```

上述示例的输出将是:

```
WARNING:root:Warning message
ERROR:root:Error message
CRITICAL:root:Critical message
```

这是因为默认日志记录级别设置为警告。

## 更改日志记录级别

记录模块提供了设置记录器基本配置的功能。

要更改记录器的级别，请为`basicConfig()`传递`level`参数。

***举例:***

```
import logging

logging.basicConfig(level=logging.INFO)

logging.debug('Debug message')
logging.info('Info message')
logging.error('Error message')
```

上述示例的输出将是:

```
INFO:root:Info message
ERROR:root:Error message
```

在本例中，日志记录级别设置为 INFO。因此，将记录“信息消息”,但不会记录“调试消息”,因为信息级别高于调试级别。

## 记录到文件中

要将消息记录到文件中，只需在`basicConfig()`的`filename`参数中传递文件名

***例如:***

```
import logging

logging.basicConfig(filename='sample.log', level=logging.INFO)

logging.debug('Debug message')
logging.info('Info message')
logging.error('Error message')
```

该文件的内容将是:

```
INFO:root:Info message
ERROR:root:Error message
```

## 更改日志记录格式

默认的记录格式是`%(levelname)s:%(name):%(message)s`。

要改变默认格式，我们需要在`basicConfig()`中指定`format`参数。

***例如:***

```
FORMAT = '%(asctime)s:%(name)s:%(levelname)s - %(message)s'

logging.basicConfig(format=FORMAT, level=logging.INFO)
logging.info('Info message')
```

相应的输出将是:

```
2020-07-03 00:48:00,106:root:INFO - Info message
```

格式化属性的完整列表可以在[官方文档](https://docs.python.org/3/library/logging.html#logrecord-attributes)中找到。

## 更改日期格式

要更改日志中显示的日期格式，我们需要更改`datefmt`参数。

***例如:***

```
FORMAT = '%(asctime)s:%(name)s:%(levelname)s - %(message)s'

logging.basicConfig(format=FORMAT, 
                    level=logging.INFO, 
                    datefmt='%Y-%b-%d %X%z')

logging.info('Info message')
```

***输出:***

```
2020-Jul-03 00:56:31+0530:root:INFO - Info message
```

可用日期格式列表可在[文档](https://docs.python.org/3/library/time.html#time.strftime)中找到。

## 异常记录

要记录异常的跟踪，您可以使用`logging.exception`或使用`logging.error`和`exc_info=True`。

***例-1:有记录错误***

```
try:
    5/0

except:
    logging.error('Exception occured')
```

***输出-1:***

```
ERROR:root:Exception occured
```

***例-2:with logging . error and exc _ info = True***

```
try:
    5/0

except:
    logging.error('Exception occured', exc_info=True)
```

***输出 2:***

```
ERROR:root:Exception occured
Traceback (most recent call last):
  File "<ipython-input-2-933e0f6b1879>", line 11, in <module>
    5/0
ZeroDivisionError: division by zero
```

***例-3:有日志记录.异常***

```
try:
    5/0

except:
    logging.exception('Exception occured')
```

***输出-3:***

```
ERROR:root:Exception occured
Traceback (most recent call last):
  File "<ipython-input-3-e7d1d57e6056>", line 11, in <module>
    5/0
ZeroDivisionError: division by zero
```

## 结论

我希望您能够理解 Python 中日志记录的基础知识。通过这篇文章，您可以开始将日志记录用于任何应用程序的各种目的，例如调试、使用监控和性能监控。您可以参考[官方文档](https://docs.python.org/3/library/logging.html)获取可用方法的完整列表。

## **资源**

本文中使用的代码片段可以在我的 [GitHub 页面](https://jimit105.github.io/medium-articles/Introduction%20to%20Logging%20in%20Python.html)上找到。

## 参考

*   [https://docs.python.org/3/library/logging.html](https://docs.python.org/3/library/logging.html)
*   [https://docs.python.org/3/howto/logging.html](https://docs.python.org/3/howto/logging.html)

## 让我们连接

领英:[https://www.linkedin.com/in/jimit105/](https://www.linkedin.com/in/jimit105/)
GitHub:[https://github.com/jimit105](https://github.com/jimit105)
推特:[https://twitter.com/jimit105](https://twitter.com/jimit105)