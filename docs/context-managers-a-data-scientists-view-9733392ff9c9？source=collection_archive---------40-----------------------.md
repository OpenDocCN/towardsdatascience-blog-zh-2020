# 上下文管理器:数据科学家的观点

> 原文：<https://towardsdatascience.com/context-managers-a-data-scientists-view-9733392ff9c9?source=collection_archive---------40----------------------->

![](img/f73b357f9f0ef0e834893b692fbaa8fd.png)

[https://unsplash.com/photos/KVihRByJR5g?utm_source=unsplash&UTM _ medium = referral&UTM _ content = creditShareLink](https://unsplash.com/photos/KVihRByJR5g?utm_source=unsplash&utm_medium=referral&utm_content=creditShareLink)

## python 上下文管理器如何清理您的代码

这篇文章是一个系列的一部分，在这个系列中，我将分享我在干净的 python 代码这个主题上所学到的东西。我是一名数据科学家，希望通过编写更多的 python 代码来提升自己的 python 技能，并找到更好的方法来构建更大的代码库。我正在通读 Python 中的[干净代码，并用其他资源丰富材料。我的目标是通过在这里总结来巩固我正在学习的主题，并希望帮助其他人理解这些主题！](https://www.amazon.com/Clean-Code-Python-Refactor-legacy/dp/1788835832#:~:text=Book%20Description&text=The%20book%20begins%20by%20describing,best%20practices%20for%20software%20design.)

这篇文章是关于 Python 中的上下文管理器的。首先，我将描述什么是上下文管理器，以及它们为什么有用。然后，我将带您看一个 web 抓取的实际例子。

# 什么是上下文管理器？

描述上下文管理器的最好方式是展示一个几乎每个 Python 程序员都在某个时候遇到过的例子，而不知道他们正在使用它！下面的代码片段打开一个. txt 文件，并将这些行存储在 python 列表中。让我们看看这个是什么样子的:

```
with open('example.txt') as f:
    lines = f.readlines()print(lines)
```

我们来分析一下。第一行打开一个文件，并将文件对象分配给`f`。下一行在 file 对象上执行`readlines`方法，返回一个列表，列表中的每一项代表`example.txt`文件中的一行文本。最后一行打印列表。使它成为上下文管理器的原因是，一旦 with 语句下的缩进被退出，文件就会被隐式关闭。如果没有上下文管理器，代码将如下所示:

```
f = open('example.txt')
lines = f.readlines()
f.close()print(lines)
```

使用上下文管理器使其可读性更好。想象一个打开和关闭多个文件的脚本，它们之间有操作行。它可能会变得难以阅读，并且您很容易忘记关闭文件。即使遇到异常，也要保证成交。

# 编写自己的上下文管理器

为了说明如何编写自己的代码，我们将使用一个实际的例子。我最近在使用 selenium 进行网络抓取时遇到了一个问题，我无法在现有的浏览器中打开一个新的 url。解决方案是打开一个新的浏览器来访问 url，并在我收集完数据后关闭它。有两种方法可以做到。我们将从定义一个类开始，并使用`__enter__`和`__exit__` dunder 方法。

```
# imports
from selenium import webdriver
from selenium.webdriver.common.keys import Keys# define context manager class
class OpenBrowser():

    def __enter__(self):
        self.driver = webdriver.Chrome(chrome_driver_path)
        return self.driver

    def __exit__(self, exception_type, exception_value, 
                 exception_traceback):
        self.driver.close()# use context manager
with OpenBrowser() as driver:
    driver.get("[http://www.python.org](http://www.python.org)")
    elem = driver.find_element_by_name("q")
    elem.clear()
    elem.send_keys("pycon")
    elem.send_keys(Keys.RETURN)
    html = driver.page_sourceprint(html)
```

这看起来很复杂，其实不然。你用`__enter()__`和`__exit()__`定义了一个类，它们分别定义了当你通过`with`调用它时会发生什么，以及当你退出缩进时会发生什么。`__exit__`的参数是固定的，而`__enter__`的返回值是可选的。现在读者很清楚，这个驱动程序的存在只是为了在 python.org 搜索 pycon 并获取结果 html。无论发生什么，浏览器都会关闭。

这个语法不算太差，但是有更简单的方法。上下文库提供了一个 decorator 来处理 boiler 板，并用函数替换类。让我们看看上面的例子是如何使用装饰器的:

```
# imports
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
import contextlib[@contextlib](http://twitter.com/contextlib).contextmanager
def open_browser():
    driver = webdriver.Chrome(chrome_driver_path)
    yield driver
    driver.close()with open_browser() as driver:
    driver.get("[http://www.python.org](http://www.python.org)")
    elem = driver.find_element_by_name("q")
    elem.clear()
    elem.send_keys("pycon")
    elem.send_keys(Keys.RETURN)
    html = driver.page_source
```

这里，我们只是导入`contextlib`并用 contextlib.contextmanager 修饰一个函数。该函数需要有一个 yield 语句，yield 值是可选的，但它告诉上下文管理器`__enter__`和`__exit__`出现的位置，分别在 yield 之前和之后。

# 结论

我希望我不仅足够好地解释了上下文管理器，而且让你相信它们在你的工具箱中非常有价值。即使作为一名数据科学家，我也发现了许多这些派上用场的案例。欢迎留下任何问题或评论，编码愉快！