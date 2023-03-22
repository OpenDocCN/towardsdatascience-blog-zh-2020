# Python StdOuts:不要打印，记录！

> 原文：<https://towardsdatascience.com/dont-print-log-85df4c153abb?source=collection_archive---------25----------------------->

## [*小窍门*](https://towardsdatascience.com/tagged/tips-and-tricks)

## 显示状态消息的 pythonic 方式

![](img/532c15e57b2ac8a13cc08dbfc0daf133.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

通常在 Python 上，尤其是作为初学者，您可能会打印( )一个变量，以便查看程序中发生了什么。如果你在整个程序中依赖太多的打印语句，你可能会面临不得不在最后注释掉它们的噩梦。查看程序正在做什么的另一种方式是日志记录。然后，您可以将打印内容限制为最终用户希望看到的命令行输出。

# 为什么要日志？

日志是一种在开发阶段查看程序状态的舒适工具。理想情况是:

1.  您希望区分调试输出和程序输出
2.  您不希望在最终产品中出现不相关的标准输出
3.  您希望在开发和测试后禁用所有标准输出
4.  您希望将程序执行历史保存到一个文件中，该文件包含元信息，如时间和更详细的调试信息。

# 怎么日志？

您可以使用日志模块登录 python。

```
import logging
```

日志模块提供了在程序中记录各种状态消息的一些默认状态。默认级别为`DEBUG`、`INFO`、`WARNING`、`ERROR`和`CRITICAL`。

如果您执行以下程序:

```
import logging
import os

savepath = ‘path/to/save/at/’if not os.path.exists(savepath):
  logging.warning(‘Warning! The savepath provided doesn\’t exist!'     
                  'Saving at current directory ‘)
  savepath = os.getcwd() 
  logging.info(‘savepath reset at %s’,str(savepath))else:
    logging.info('Savepath provided is correct,'
                 'saving at %s’,str(savepath))
```

示例中的路径不存在。您将看到以下输出:

```
WARNING:root:warning! The savepath provided doesn’t exist!Saving at current directory
```

**不显示新的当前保存路径信息，即 logging.info。**

这是因为输出的默认严重级别是“警告”。严重程度的顺序如下:

`DEBUG`

`INFO`

`WARNING`(这是默认设置)

`ERROR`

`CRITICAL`

如果您想要查看较低严重性级别的输出，您必须在日志记录配置中显式设置它们。在这个设置之后启动一个新的解释器，否则它将无法工作。

```
logging.basicConfig(level = logging.DEBUG)
```

现在，一切都将被打印出来，从调试级开始。

输出:

```
WARNING:root:warning! The savepath provided doesn’t exist! Saving at current directory INFO:root:savepath reset at path/to/current/directory
```

太棒了。但是对于一个像样的日志，我们可能希望看到更多的信息，并将其放在一个单独的文件中。这可以使用配置中的格式和文件名来设置。在文件开头添加所需的配置:

```
logging.basicConfig(filename=’logfilename.log’,level = 
                    logging.DEBUG,format=’%(asctime)s %
                    (message)s’,
                    datefmt=’%d/%m/%Y %I:%M:%S %p’)
```

这会将您的信息连同时间戳一起记录到 logfilename.log 中。

```
13/06/2020 05:14:47 PM warning! The savepath provided doesn’t exist!  
                       Saving at current directory 
13/06/2020 05:14:47 PM savepath reset at current/working/directory
```

下一次运行该文件时，日志会被附加到文本文件中，这样您就可以在一个地方拥有日志的历史记录。您也可以更改它，每次都创建一个新的文本文件，方法是将它添加到您的 logging.basicConfig 中

```
filemode='w'
```

您可以通过在脚本中添加以下内容来集中禁用所有日志记录，而不必注释掉每条消息:

```
logger = logging.getLogger()
logger.disabled = True
```

或者，您可以选择更改需要写入日志文件的输出级别，以便只向最终用户显示警告和严重错误消息。

这是登录 python 的入门知识。日志记录提供了比上面描述的更大的灵活性(处理程序、过滤器、更改默认日志级别等)，但这应该足以开始使用基本的日志记录，并消除打印语句。