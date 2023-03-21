# 5 分钟内解释日志记录 Python 演示

> 原文：<https://towardsdatascience.com/logging-explained-in-5-minutes-walkthrough-with-python-8bd7d8c2cf3a?source=collection_archive---------20----------------------->

## 掌握编程和数据科学的必备工具之一。

让您的代码可以投入生产并不是一件容易的事情。需要考虑的事情太多了，其中之一就是能够监控应用程序的流程。这就是日志的用武之地——一个简单的工具，可以节省一些精力和很多很多时间。

![](img/a0455a3f42a4cc237e020cd45ab2272d.png)

照片由 [Giammarco Boscaro](https://unsplash.com/@giamboscaro?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

Python 对日志记录有很好的内置支持。它是通过`logging`库实现的，与其他主要编程语言中的选项非常相似。

如果你更喜欢视频，或者只是想巩固你的知识，请随意观看我们关于这个主题的视频。

在我们进入代码之前，让我们简单地讨论一下为什么您应该关心日志记录，以及它背后的一些理论。

# 伐木——为什么？

我之前提到过，日志记录用于监控应用程序流，以及其他功能。你现在的问题可能是*为什么我们不能只使用打印语句？我们可以，但这并不理想。没有办法通过简单的打印语句来跟踪消息的严重性。这就是伐木业的闪光点。*

以下是我认为应该在应用程序中使用日志记录的三大理由:

1.  **了解你的代码是如何工作的**——你不想在生产中盲目
2.  **捕获并修复意外错误**——检测代码中潜在的更大问题
3.  **分析并可视化应用程序如何执行** —更高级的主题

如前所述，`logging`库内置于 Python 编程语言中，提供了 5 个严重级别:

*   调试
*   信息
*   警告
*   错误
*   批评的

您可以仅从名称来推断何时应该使用一个而不是另一个，但是需要注意的是，Python 默认显示严重级别为 WARNING 及以上的消息。这种行为是可以改变的。

现在让我们用一些简单的代码来探索日志记录。

# 记录——如何记录？

首先，让我们执行几个导入:

```
import random
import time
from datetime import datetime
import logging
```

如你所见，这里包括了`logging`图书馆。我之前提到过 Python 将只显示严重级别为警告及以上的消息，所以我们可以这样改变:

```
logging.basicConfig(*level***=**logging.DEBUG)
```

就是这样！让我们声明一个简单的函数，该函数生成一个从 0 到 4 的随机数，并基于该随机数记录不同严重性级别的消息。显示消息后，程序会休眠一秒钟。该功能仅用于测试:

```
def log_tester():
    x = random.randint(0, 4)
    if x == 0:
        logging.debug(‘Debug message’)
    elif x == 1:
        logging.info(‘Info message’)
    elif x == 2:
        logging.warning(‘Warning message’)
    elif x == 3:
        logging.error(‘Error message’)
    elif x == 4:
        logging.critical(‘Critical message’) time.sleep(1)
    return
```

最后，我们需要在某个地方调用这个函数，为什么不在循环中调用呢？只是为了打印出多条消息:

```
for i in range(5):
    log_tester()
```

如果您现在运行这段代码，您将得到类似于我的输出:

```
Output:WARNING:root:Warning message
ERROR:root:Error message
DEBUG:root:Debug message
INFO:root:Info message
INFO:root:Info message
```

请记住，由于随机化过程，您的输出可能会有所不同。

这种格式在某些情况下很好，但在其他情况下，我们可能需要更多的控制，并且能够进一步定制输出的外观。

让我们来探索如何。

## 输出格式

让我们稍微改变一下`logging.basicConfig`:

```
logging.basicConfig(*level***=**logging.DEBUG, *format***=**’%(levelname)s → %(name)s:%(message)s’)
```

如果您现在运行此代码，消息将按照指定的方式格式化:

```
Output:CRITICAL → root:Critical message
WARNING → root:Warning message
DEBUG → root:Debug message
DEBUG → root:Debug message
CRITICAL → root:Critical message
```

那很好，但是我喜欢做的是给消息添加当前的日期和时间信息。使用格式字符串很容易做到:

```
logging.basicConfig(*level***=**logging.DEBUG, *format***=**f’%(levelname)s → {datetime.now()} → %(name)s:%(message)s’)
```

这是我们新格式的样子:

```
DEBUG → 2020–08–09 10:32:11.519365 → root:Debug message
DEBUG → 2020–08–09 10:32:11.519365 → root:Debug message
DEBUG → 2020–08–09 10:32:11.519365 → root:Debug message
ERROR → 2020–08–09 10:32:11.519365 → root:Error message
WARNING → 2020–08–09 10:32:11.519365 → root:Warning message
```

现在我们有进展了！要获得消息被记录的实际时间，您需要在消息中嵌入对`datetime.now()`的调用。

唯一的问题是，一旦终端窗口关闭，日志将永远丢失。因此，与其将消息输出到控制台，不如让我们探索如何将它们保存到文件中。

## 保存到文件

要将日志消息保存到文件中，我们需要为另外两个参数指定值:

*   `filename` —保存日志的文件的名称
*   `filemode` —写入或追加模式，我们稍后将探讨这些模式

让我们先看看如何使用`write`模式。此模式将在每次运行应用程序时用指定的名称覆盖任何现有文件。配置如下:

```
logging.basicConfig(
    *filename***=**’test.log’,
    *filemode***=**’w’,
    *level***=**logging.DEBUG,
    *format***=**’%(levelname)s → {datetime.now()} → %(name)s:%(message)s’
)
```

如果您现在运行该程序，控制台中将不会显示任何输出。相反，会创建一个名为`test.log`的新文件，它包含您的日志消息:

```
test.log:WARNING → *2020–08–09* *10:35:54.115026* → root:Warning message
INFO → *2020–08–09* *10:35:54.115026* → root:Info message
WARNING → *2020–08–09* *10:35:54.115026* → root:Warning message
DEBUG → *2020–08–09* *10:35:54.115026* → root:Debug message
CRITICAL → *2020–08–09* *10:35:54.115026* → root:Critical message
```

如果您再次运行该程序，这 5 行将会丢失并被新的 5 行所替换。在某些情况下，这不是你想要的，所以我们可以使用`append`模式来保留以前的数据，并在末尾写入新的行。配置如下:

```
logging.basicConfig(
    *filename***=**’test.log’,
    *filemode***=**’a’,
    *level***=**logging.DEBUG,
    *format***=**’%(levelname)s → {datetime.now()} → %(name)s:%(message)s’
)
```

如果您现在运行该程序，并查看我们的文件，您会看到有 10 行:

```
test.log:WARNING → *2020–08–09* *10:35:54.115026* → root:Warning message
INFO → *2020–08–09* *10:35:54.115026* → root:Info message
WARNING → *2020–08–09* *10:35:54.115026* → root:Warning message
DEBUG → *2020–08–09* *10:35:54.115026* → root:Debug message
CRITICAL → *2020–08–09* *10:35:54.115026* → root:Critical message
DEBUG → *2020-08-09* *10:36:24.699579* → root:Debug message
INFO → *2020-08-09* *10:36:24.699579* → root:Info message
CRITICAL → *2020-08-09* *10:36:24.699579* → root:Critical message
CRITICAL → *2020-08-09* *10:36:24.699579* → root:Critical message
CRITICAL → *2020-08-09* *10:36:24.699579* → root:Critical message
```

仅此而已。您现在已经了解了日志记录的基本知识。让我们在下一部分总结一下。

# 在你走之前

当然，记录并不是最有趣的事情。但是没有它，你基本就是瞎子。花点时间想想，如果不记录日志，您将如何监控已部署应用程序的行为和流程？我知道不容易。

在 5 分钟内，我们已经掌握了基本知识，现在您可以在下一个应用程序中实现日志了。严格来说，它不一定是一个应用程序，您也可以将它用于数据科学项目。

感谢阅读。保重。

喜欢这篇文章吗？成为 [*中等会员*](https://medium.com/@radecicdario/membership) *继续无限制学习。如果你使用下面的链接，我会收到你的一部分会员费，不需要你额外付费。*

[](https://medium.com/@radecicdario/membership) [## 通过我的推荐链接加入 Medium-Dario rade ci

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

medium.com](https://medium.com/@radecicdario/membership) 

加入我的私人邮件列表，获取更多有用的见解。