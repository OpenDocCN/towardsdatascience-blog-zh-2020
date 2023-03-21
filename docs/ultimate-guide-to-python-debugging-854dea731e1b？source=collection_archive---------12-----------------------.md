# Python 调试终极指南

> 原文：<https://towardsdatascience.com/ultimate-guide-to-python-debugging-854dea731e1b?source=collection_archive---------12----------------------->

## 让我们探索使用 Python 日志记录、回溯、装饰器等等进行调试的艺术…

即使您编写了清晰易读的代码，即使您用测试覆盖了您的代码，即使您是非常有经验的开发人员，奇怪的错误还是会不可避免地出现，您需要以某种方式调试它们。许多人求助于仅仅使用一堆`print`语句来查看他们的代码中发生了什么。这种方法远非理想，还有更好的方法来找出代码中的问题，其中一些我们将在本文中探讨。

![](img/5add754970e528ab1fcf48d0d6c2afbb.png)

照片由[丹尼·麦克](https://unsplash.com/@dannymc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

# 记录是必须的

如果您编写的应用程序没有某种日志设置，您最终会后悔的。没有来自应用程序的任何日志会使排除任何错误变得非常困难。幸运的是——在 Python 中——设置基本记录器非常简单:

这就是开始将日志写入文件所需要的全部内容，文件看起来会像这样(您可以使用`logging.getLoggerClass().root.handlers[0].baseFilename`找到文件的路径):

这种设置可能看起来足够好了(通常如此)，但是拥有配置良好、格式化、可读的日志可以让您的生活变得更加轻松。改进和扩展配置的一个方法是使用由记录器读取的`.ini`或`.yaml`文件。例如，您可以在配置中执行以下操作:

在 python 代码中包含这种大量的配置将很难导航、编辑和维护。将东西保存在 YAML 文件中使得用非常特殊的设置来设置和调整多个记录器变得更加容易。

如果你想知道所有这些配置字段是从哪里来的，这些在这里[有记录](https://docs.python.org/3.8/library/logging.config.html)，其中大部分只是第一个例子中所示的*关键字参数*。

所以，现在文件中有了配置，意味着我们需要加载 is。对于 YAML 文件，最简单的方法是:

Python logger 实际上并不直接支持 YAML 文件，但它支持*字典*配置，可以使用`yaml.safe_load`从 YAML 轻松创建。如果你倾向于使用旧的`.ini`文件，那么我只想指出，根据[文档](https://docs.python.org/3/howto/logging.html#configuring-logging)，使用*字典*配置是新应用的推荐方法。更多例子，请查看[日志食谱](https://docs.python.org/3/howto/logging-cookbook.html#an-example-dictionary-based-configuration)。

# 伐木装饰工

继续前面的日志记录技巧，您可能会遇到需要记录一些有问题的函数调用的情况。您可以使用 logging decorator 来代替修改所述函数的主体，它会用特定的日志级别和可选的消息来记录每个函数调用。让我们来看看装潢师:

不骗你，这可能需要一点时间来理解(你可能想直接*复制粘贴*并使用它)。这里的想法是，`log`函数获取参数，并使它们对内部的`wrapper`函数可用。然后，通过添加附属于装饰器的访问函数，使这些参数变得可调整。至于`functools.wraps`装饰器——如果我们不在这里使用它，函数名(`func.__name__`)将被装饰器名覆盖。但是这是一个问题，因为我们想打印名字。这由`functools.wraps`解决，因为它将函数名、文档字符串和参数列表复制到装饰函数上。

无论如何，这是上面代码的输出。很漂亮，对吧？

# __repr__ 获取更多可读的日志

使代码更易调试的简单改进是在类中添加`__repr__`方法。如果你不熟悉这个方法，它所做的只是返回一个类的实例的字符串表示。使用`__repr__`方法的最佳实践是输出可用于重新创建实例的文本。例如:

如果如上所示表示对象不可取或不可能，好的替代方法是使用`<...>`表示，例如`<_io.TextIOWrapper name='somefile.txt' mode='w' encoding='UTF-8'>`。

除了`__repr__`之外，实现`__str__`方法也是一个好主意，默认情况下，当`print(instance)`被调用时使用这个方法。有了这两种方法，你就可以通过打印变量获得大量信息。

# 用于字典的 __missing__ Dunder 方法

如果你出于某种原因需要实现自定义字典类，那么当你试图访问一些实际上并不存在的键时，你会发现一些来自`KeyError`的错误。为了避免在代码中摸索并查看哪个*键*丢失，您可以实现特殊的`__missing__`方法，该方法在每次`KeyError`被引发时被调用。

上面的实现非常简单，只返回并记录丢失了*键*的消息，但是您也可以记录其他有价值的信息，以提供更多关于代码中哪里出错的上下文。

# 调试崩溃应用程序

如果您的应用程序在您有机会看到发生了什么之前就崩溃了，您可能会发现这个技巧非常有用。

用`-i`参数(`python3 -i app.py`)运行应用程序会导致程序一退出就启动交互式 shell。此时，您可以检查变量和函数。

如果这样还不够好，可以带个更大的锤子—`pdb`—*Python 调试器*。有相当多的特性足以保证一篇独立的文章。但这里有一个例子和最重要的位的纲要。让我们先看看我们的小崩溃脚本:

现在，如果我们用`-i`参数运行它，我们就有机会调试它:

上面的调试会话非常简要地展示了您可以用`pdb`做什么。程序终止后，我们进入交互式调试会话。首先，我们导入`pdb`并启动调试器。此时，我们可以使用所有的`pdb`命令。作为上面的例子，我们使用`p`命令打印变量，使用`l`命令列出代码。大多数时候，你可能想设置断点，你可以用`b LINE_NO`来设置，然后运行程序直到断点被命中(`c`)，然后用`s`继续单步执行函数，也可以用`w`打印堆栈跟踪。关于命令的完整列表，您可以查看`[pdb](https://docs.python.org/3/library/pdb.html#debugger-commands)` [文档](https://docs.python.org/3/library/pdb.html#debugger-commands)。

# 检查堆栈跟踪

比方说，你的代码是运行在远程服务器上的 Flask 或 Django 应用程序，在那里你不能获得交互式调试会话。在这种情况下，您可以使用`traceback`和`sys`包来更深入地了解代码中的失败之处:

运行时，上面的代码将打印最后一次引发的异常。除了打印异常，您还可以使用`traceback`包来打印堆栈跟踪(`traceback.print_stack()`)或提取原始堆栈帧，将其格式化并进一步检查(`traceback.format_list(traceback.extract_stack())`)。

# 调试期间重新加载模块

有时，您可能正在调试或试验交互式 shell 中的某些功能，并对其进行频繁的更改。为了使运行/测试和修改的循环更容易，您可以运行`importlib.reload(module)`以避免每次更改后都必须重启交互会话:

这个技巧更多的是关于效率而不是调试。能够跳过一些不必要的步骤，让你的工作流程更快更有效率，这总是很好的。一般来说，不时地重新加载模块是一个好主意，因为它可以帮助您避免试图调试同时已经修改了很多次的代码。

> *调试是一门艺术*

# 结论

大多数时候，编程的真正含义只是大量的尝试和错误。另一方面，在我看来，调试是一门*艺术*,精通它需要时间和经验——你对你使用的库或框架了解得越多，就越容易。上面列出的提示和技巧可以让您的调试更高效、更快速，但是除了这些特定于 Python 的工具，您可能还想熟悉一些通用的调试方法——例如，Remy Sharp 的[调试艺术](https://remysharp.com/2015/10/14/the-art-of-debugging)。

如果你喜欢这篇文章，你应该看看我下面的其他 Python 文章！

*本文原帖*[*martinheinz . dev*](https://martinheinz.dev/blog/24?utm_source=devto&utm_medium=referral&utm_campaign=blog_post_24)

[](/ultimate-setup-for-your-next-python-project-179bda8a7c2c) [## 下一个 Python 项目的最终设置

### 从零开始任何项目都可能是一项艰巨的任务…但如果您有这个最终的 Python 项目蓝图就不会了！

towardsdatascience.com](/ultimate-setup-for-your-next-python-project-179bda8a7c2c) [](/automating-every-aspect-of-your-python-project-6517336af9da) [## 自动化 Python 项目的各个方面

### 每个 Python 项目都可以从使用 Makefile、优化的 Docker 映像、配置良好的 CI/CD、代码…

towardsdatascience.com](/automating-every-aspect-of-your-python-project-6517336af9da) [](/making-python-programs-blazingly-fast-c1cd79bd1b32) [## 让 Python 程序快得惊人

### 让我们看看我们的 Python 程序的性能，看看如何让它们快 30%！

towardsdatascience.com](/making-python-programs-blazingly-fast-c1cd79bd1b32)