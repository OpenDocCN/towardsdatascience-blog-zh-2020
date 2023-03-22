# 如何修复 ModuleNotFoundError 和 ImportError

> 原文：<https://towardsdatascience.com/how-to-fix-modulenotfounderror-and-importerror-248ce5b69b1c?source=collection_archive---------0----------------------->

## 做适当的模块导入，让你的生活更轻松

![](img/506263c103acd492518e1b6a4a07b4ec.png)

照片由[莱昂·温特](https://unsplash.com/@fempreneurstyledstock)在[unsplash.com](https://unsplash.com/photos/mTkXSSScrzw)拍摄

# TL；博士；医生

*   使用**绝对**进口
*   **将您的项目根目录附加到** `PYTHONPATH` —在您希望运行 Python 应用程序的任何环境中，如 Docker、vagger 或您的虚拟环境，即在 bin/activate 中，运行(或例如，如果您使用 virtualenv，则添加到`bin/activate`)以下命令:

```
export PYTHONPATH="${PYTHONPATH}:/path/to/your/project/"
```

*   *避免使用`sys.path.append("/path/to/your/project/")`

模块导入肯定会让人们感到沮丧，尤其是那些对 Python 相当陌生的人。由于我每天都在 StackOverflow 上看到相关的问题，所以我决定在 Medium 上写一篇文章，尝试描述 import 在幕后是如何工作的，以及为了使您的生活更轻松，您需要遵循什么方法。

# 术语

首先，让我们从定义一些有用的术语开始，这些术语将帮助您理解本文中描述的概念。

*   一个 **python** **模块**是一个带有*的单个文件。py* 分机。
*   一个 **python** **包**是一个包含至少一个 python 模块的文件夹。对于 python2，一个包需要一个 *__init__。py* 文件
*   一个 python 包可以包含任意数量的嵌套**子包**，即包含项目结构中其他包的包。
*   **导入**在一个模块需要使用另一个模块(相同或不同的包或子包)中编写的一些功能(如函数或类)时很有用

例如，考虑以下项目结构:

```
└── myproject
    ├── mypackage
    │   ├── a.py
    └── anotherpackage
        ├── b.py
        ├── c.py
        └── mysubpackage
            └── d.py
```

项目`myproject`包含两个包，`mypackage`和`anotherpackage`，每个包包含许多 python 模块，而后者还包含一个名为`mysubpackage`的子包，该子包又包含一个额外的 python 模块。

# 模块导入是如何在幕后工作的？

现在让我们假设在您当前的模块中，您希望导入另一个模块，如下所示:

```
import a
```

Python 将分两步执行上述语句:

*   定位、加载和初始化(如果需要)所请求的模块
*   在本地名称空间和相应的作用域中定义必要的名称

现在 Python 解释器将按照下面的步骤尝试解决`a`。

**步骤 1:系统模块查找**

最初，Python 会尝试在`[sys.modules](https://docs.python.org/3/library/sys.html#sys.modules)`中搜索模块名，这是一个将模块名映射到已经加载的模块的字典。如果名称解析成功(这意味着另一个模块已经加载了它),那么它将可用于本地名称空间；否则，跳到步骤 2。

**步骤 2: Python 标准库查找**

> [Python 标准库](https://docs.python.org/3/library/)包含内置模块(用 C 编写),提供对系统功能的访问，如 Python 程序员无法访问的文件 I/O，以及用 Python 编写的模块，为日常编程中出现的许多问题提供标准化解决方案。其中一些模块被明确设计成通过将特定于平台的抽象成平台中立的 API 来鼓励和增强 Python 程序的可移植性。

如果在`sys.modules`中找不到这个名字，那么 Python 将在 Python 标准库中搜索它。同样，如果名称被解析，那么它将在本地名称空间中定义，否则需要遵循步骤 3。

**步骤 3:系统路径查找**

现在，如果在`sys.modules`和标准库中都找不到模块名，Python 将最终尝试在`sys.path`下解析它。这是事情肯定会出错的地方。我相信大部分 Python 程序都很熟悉`**ModuleNotFoundError**`

```
import aModuleNotFoundError: No module named 'a'
```

或者`**ImportError**`:

```
from . import aImportError: cannot import name 'a'
```

# 绝对进口与相对进口

在**绝对导入**中，我们指定了从项目根目录开始的显式路径。在我们的例子中

```
└── myproject
    ├── mypackage
    │   ├── a.py
    └── anotherpackage
        ├── b.py
        ├── c.py
        └── mysubpackage
            └── d.py
```

这意味着如果我们想要在模块`b`中导入模块`a`，我们必须指定

```
import mypackage.a
```

其他有效的示例包括以下导入:

```
# in module a.py
import anotherpackage.mysubpackage.d# in module b
import anotherpackage.c
```

另一方面，在**相对导入**中，我们指定了相对于当前模块位置的模块路径。我们示例中的几个例子可能是:

```
# in module a.py
from ..anotherpackage import b
from ..anotherpackage.b import another_function# in module b
from . import c
from .c import my_function
```

我个人不赞成使用相对进口，因为它们不如绝对进口可读性强，PEP-8 也是这样建议的。在一些罕见的情况下，您可能必须使用相对导入来避免不必要的长路径。例如，

```
from package_a.sub_b.sub_c.sub_d.module_d import my_function
```

# 如何修复 ModuleNotFoundError 和 ImportError？

现在，我们已经了解了基本导入语句的执行方式以及绝对导入和相对导入之间的区别，我们现在可以继续讨论当您的 Python 应用程序在使用`ModuleNotFoundError` 或`ImportError`失败时该怎么办。

在大多数情况下，这两种错误都是由于 Python 无法解析`sys.path`中的模块名而导致的。回想一下，当您调用`import a`时，如果模块的名称既没有在`sys.modules`中找到，也没有在标准库中找到，Python 将尝试在`sys.path`中解析它。同样，当您使用`from`语法(例如`from mypackage import a`)时，Python 将首先尝试查找并加载模块。当它失败时，Python 会在第一种情况下抛出`ModuleNotFoundError`，在第二种情况下抛出`ImportError`。

如果是这样的话，回想一下我们下面的例子，

```
└── myproject
    ├── mypackage
    │   ├── a.py
    └── anotherpackage
        ├── b.py
        ├── c.py
        └── mysubpackage
            └── d.py
```

*   **首先确保你使用的是绝对导入**
*   **将项目的根目录导出到**

大多数现代 Python IDEs 会自动完成这一任务，但如果不是这样，我很肯定会有这样的选项，您可以为您的 Python 应用程序(至少是 PyCharm)定义`PYTHONPATH`。

如果您在任何其他环境中运行 Python 应用程序，比如 Docker、vagger 或者在您的虚拟环境中，您可以在 bash 中运行以下命令:

```
export PYTHONPATH="${PYTHONPATH}:/path/to/your/project/"# * For Windows
set PYTHONPATH=%PYTHONPATH%;C:\path\to\your\project\
```

现在既然你的项目的根目录已经被附加到了`PYTHONPATH`中，你的绝对导入应该会非常有效。

或许我也能做到，但这绝对不是一次好的练习。

# 结论

如果您是 Python 的新手，导入模块可能会成为一场噩梦，尤其是当您需要处理复杂的项目结构时。如果您遵循两步规则——即使用绝对导入并将项目的根目录附加到 PYTHONPATH 中——那么您就不应该担心将来的模块导入。

**如果你是 Python 新手，我强烈推荐你在亚马逊上买一本** [**学习 Python**](https://www.amazon.co.uk/gp/product/1449355730/ref=as_li_tl?ie=UTF8&camp=1634&creative=6738&creativeASIN=1449355730&linkCode=as2&tag=gmyrianthous-21&linkId=a409673edf6734d7fbb75a41d6a362ff) **的书。**

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读介质上的每一个故事。你的会员费直接支持我和你看的其他作家。你也可以在媒体上看到所有的故事。**

[](https://gmyrianthous.medium.com/membership) [## 通过我的推荐链接加入 Medium-Giorgos Myrianthous

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

gmyrianthous.medium.com](https://gmyrianthous.medium.com/membership) 

免责声明:本文包括附属链接