# 像专家一样管理 Python 中的文件和数据库连接

> 原文：<https://towardsdatascience.com/manage-files-and-database-connections-in-python-like-a-pro-73e8fc0b7967?source=collection_archive---------9----------------------->

## 如何使用自定义上下文管理器管理 Python 中的外部资源

![](img/ad74c159077ad0c53612c3fcecb5bd87.png)

照片由[余伟](https://www.pexels.com/@psad?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从[像素](https://www.pexels.com/photo/greyscale-photo-of-people-walking-inside-tunnel-2081168/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

在数据工程和数据科学中，我们经常需要从数据库或平面文件中检索数据。但是，使用来自外部资源的数据而不关闭与底层数据库或文件的连接可能会导致不必要的后果。在本文中，我们将讨论如何构建定制的上下文管理器，帮助我们以安全有效的方式使用这些资源。

# 上下文管理器

多亏了[上下文管理器](https://docs.python.org/3/library/contextlib.html)，我们可以从外部资源读取数据，并确保与底层数据库或文件的连接已关闭，即使我们在代码中遇到一些未处理的异常。

> 上下文管理器的典型用途包括保存和恢复各种全局状态、锁定和解锁资源、关闭打开的文件等[1]

关闭资源至关重要，因为同时打开的文件或数据库连接的数量是有限制的。可以同时打开的文件数量取决于相应操作系统允许的文件描述符数量。在 Linux 和 macOS 上，我们可以通过使用`ulimit -n` [2]来检查这个数字:

```
➜  ~ ulimit -n
256
```

同样，并发数据库连接的数量也有限制。例如，默认情况下，开放 PostgreSQL 连接的最大数量限制为 100 [3]。

## 上下文管理器的典型用法

下面的代码显示了上下文管理器的典型用法:

*   将数据写入文件

*   从同一文件中读取数据:

## 上下文管理器背后的原因

您可能会问:为什么不直接向文件写入和读取数据，而不添加这个`with`？我们可以编写如下相同的代码:

但是想象一下，如果我们在写入这个文件时遇到错误，会发生什么？连接永远不会关闭。我们可以通过将代码包含在`try-finally`块中来改进这一点，如下所示:

但是这可能写起来很乏味，我们的代码变得冗长。因此，上下文管理器是很棒的——我们可以在仅仅两行代码中完成更多的工作，并且我们的代码变得更具可读性。

总结起来，上下文管理器的理由是:

*   确保资源被释放，即使我们遇到一些未处理的异常
*   可读性
*   便利——我们让自己变得更容易，因为我们不再*忘记*关闭与外部资源的连接。

# 自定义上下文管理器

有两种定义上下文管理器的方法:使用定制类，或者使用生成器[4]。让我们看看生成器选项，创建一个上下文管理器，让我们管理一个 MySQL 数据库连接。

## 用上下文管理器处理 MySQL 数据库连接

该逻辑与使用`try-finally`块的逻辑相似，除了我们**产生**连接对象，而不是返回它——这是由于生成器在需要时会延迟返回对象的性质(*即，当我们迭代它们时*)。在我们的例子中，上下文管理器将产生一个值——连接对象。

注意，我们所要做的就是添加一个装饰器`@contextlib.contextmanager`，并在`try`块中使用`yield`。

要使用这个上下文管理器将 MySQL 中的数据检索到 pandas 数据框中，我们只需通过上下文管理器导入并应用这个函数:

注意我们的 *bus_logic.py* 脚本变得多么简单！

`try`块可以包含任何定制的设置代码，比如从 secrets manager、环境变量或配置文件中检索配置细节和凭证。同样，在`finally`-块中，您可以定义任何拆卸逻辑，比如删除临时文件、关闭连接或改回工作目录。这是我们接下来要做的。

## 使用上下文管理器临时更改工作目录

假设您在`src`目录中，但是当您从 S3 下载文件时，您想将它们存储在`data`目录中。在这种情况下，我们可能希望暂时将工作目录更改为`data`文件夹，并在功能完成后将其更改回之前的目录。以下上下文管理器可以实现这一点:

通过使用上面的上下文管理器，我们可以确保我们在下载文件时位于正确的目录( *data* )。然后，一旦下载完成，我们就变回原来的工作目录。

# 结论

在本文中，我们研究了 Python 中的上下文管理器，它让我们能够以更优雅的方式处理外部资源。当未处理的异常导致我们的脚本在拆卸代码有机会执行之前结束时，它们还可以防止可能发生的错误。这样，上下文管理器可以防止离开锁定的资源、打开的文件或数据库连接。最后，通过抽象出安装和拆卸代码，它们使我们的代码更具可读性。

感谢您的阅读！如果这篇文章有帮助，请随时关注我的下一篇文章。

**资源:**

[1][https://docs . python . org/3/reference/data model . html # context-managers](https://docs.python.org/3/reference/datamodel.html#context-managers)

[2][https://stack overflow . com/questions/2425084/我能一次打开多少个文件](https://stackoverflow.com/questions/2425084/how-many-files-can-i-have-opened-at-once)

[3][https://www . PostgreSQL . org/docs/current/runtime-config-connection . html](https://www.postgresql.org/docs/current/runtime-config-connection.html)

[https://book.pythontips.com/en/latest/context_managers.html](https://book.pythontips.com/en/latest/context_managers.html)