# 如何构建您的第一个 Python 包

> 原文：<https://towardsdatascience.com/how-to-build-your-first-python-package-6a00b02635c9?source=collection_archive---------5----------------------->

## 让全世界都能方便地获得你的优秀代码，因为你也很优秀

**在媒体*上阅读此文，而*不是使用此** [**好友链接**](/how-to-build-your-first-python-package-6a00b02635c9?sk=eb5a7a0edb3213cda8fcd7a8d5c1811e) **的媒体会员！**

![](img/af9a46addac68f31466cfde94ae8b208.png)

来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2071537) 的 [Vadim_P](https://pixabay.com/users/Vadim_P-4246915/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2071537) 的原始图像

*这篇博文现在也有波兰语版本，请在*[*bulldogjob . pl*](https://bulldogjob.pl/news/1108-jak-stworzyc-pakiet-pythona-i-wyslac-go-do-pypi)上阅读

大约两年前，我发表了第一篇与数据科学相关的博客。它是关于[分类相关性](https://medium.com/@shakedzy/the-search-for-categorical-correlation-a1cf7f1888c9)，我真的认为没有人会觉得它有用。这只是实验性的，对我自己来说。1.7K 鼓掌后来，我明白了我不能决定其他人会发现什么是有用的，我很高兴我能在网上帮助其他人，就像其他人在网上帮助我一样。

当时我对 Python 和 Github 还很陌生，所以我也尝试着为我写的这些分类相关性编写代码，并将其发布在 Github 上。我给那段代码起了个名字: [*Dython*](http://shakedzy.xyz/dython) ，就像 pYTHON 的*数据工具*里一样。

但这不是这篇博文的内容——它是关于在我刚刚提到的所有事情之后我做的最后一件实验性的事情:我创建了一个 Python 库并上传到 PyPI——你可能知道它是`pip install`。同样，这一切都是从学习练习开始的——但是随着每月大约 1.5K 的安装，我再次意识到我低估了自己对他人有用的代码能力。

但是为什么我要告诉*你*这一切？因为几乎每个人的笔记本电脑或 Github 账户上都有一些很酷的代码。这段很酷的代码可能对其他人很有帮助——尽管我们大多数人认为这可能不是真的，还有*“其他人能在我的一小段代码中找到什么价值”*。我也是这么想的，结果发现我错了。你也可能是错的，我在这里劝你——并帮助你——只需几个步骤就可以把你漂亮的代码变成一个完整的 Python 包——这样你就可以用一个`pip install`节省其他人几个小时的编程时间。成为协助其他程序员的人——让我向你展示如何通过六个步骤来做到这一点。我们开始吧。

# 在我们开始之前:代码清理和源代码控制

我相信这是不言而喻的，但我还是想确保我们在同一页上。程序员分为两类:一类编写可读代码，另一类使用名为 *x，x1，x2，yx，*等变量。如果你属于第一种，可以直接跳过去。如果你是第二种类型——是时候转换到另一边了。记住其他人会阅读你的代码，所以要确保它是可读的。给事物起一个有意义的名字，把长函数分解成短编码的单一用途的方法，等等。你知道，就像他们教的那些编码课程一样。

另一件事，我假设你正在使用一些源代码控制，可能是 GitHub。虽然这并不是真正必需的，但比建议的要多。如果你以前从未使用过 GitHub，这是一个花几分钟学习如何使用的好机会——查看[官方教程](https://guides.github.com/activities/hello-world/)，或者这篇[博文](/getting-started-with-git-and-github-6fcd0f2d4ac6)，然后开始使用。

# 步骤 1:选择您的 API

把你的代码变成一个包的第一步是决定用户应该如何使用它——并使它可导入。我们将把它分成两个动作:

## #1:公共和私有方法

您的代码可能由几个函数和方法组成，但并不是所有的函数和方法都是供用户使用的——有些是内部函数，应该只供您的代码使用。虽然 Python 不支持*私有方法*，但约定是用下划线前缀`def _private_function()`标记私有方法。像这样重命名你所有的内部函数，这样用户会很容易理解什么应该向他们公开，什么不应该。

***Pro 提示:*** 您可以使用特殊变量`__all__`来精确定义当用户使用`from your_package import *`时，哪些函数需要公开，哪些不需要公开。查看 StackOverflow 上的[线程](https://stackoverflow.com/questions/44834/can-someone-explain-all-in-python)了解更多信息。

## #2:添加`__init__.py`

如果您的代码由几个模块(文件)组成，如下所示:

```
your_package
  |-- module1.py
  |-- module2.py
```

您需要在您的包中添加另一个文件`__init__.py`，以便让 Python 将`your_package`目录解释为 Python 包。这意味着现在它应该是这样的:

```
your_package
  |-- __init__.py
  |-- module1.py
  |-- module2.py
```

您可以将文件留空作为起点，但它必须在那里，以便允许`from your_package import module1`。

***Pro 提示:***`__init__.py`中的代码在你的包被导入后被执行，这允许你做各种很酷的事情。例如，你可以使用`__init__.py`来缩短你的 API。假设您的代码中最重要的方法`imp_func`在`module1`中。您可以允许用户将其作为`from your_package import imp_func`而不是`from your_package.module1 import imp_func`导入，只需添加:

```
from .module1 import imp_func
```

到您的`__init__.py`文件。

# 第二步:记录

文档就像排队——我们希望每个人都能简单地做这件事，但是我们倾向于尽可能地放松自己。*步骤 2* 这里与我们开始部分之前的*相关，因为它意味着让其他人理解你的代码做什么。您需要添加两种类型的文档:*

## #1:文档字符串

库中的每个函数都应该有一个文档字符串，它总结了函数的内容。它应包含:

*   用简单的语言解释这个函数应该做什么
*   **参数:**解释什么是函数期望的参数和参数类型
*   **返回值:**解释函数返回的内容
*   **示例:**虽然不是必须的，但添加一个用法示例总是有用的

这里有一个例子:

你可以在 DataCamp 上的这篇[超酷文章中了解更多关于文档字符串格式的信息。](https://www.datacamp.com/community/tutorials/docstrings-python)

## #2:自述文件

自述文件是对软件包内容的总结。当放置在你的包的顶层目录时，GitHub 将它作为你的包的“登陆页”,作为访客看到的第一件东西显示给他们。

一个好的自述文件会有软件包的概述，安装信息(比如需求)和一些*快速启动*的例子，描述你的软件包的基本用法。

自述文件通常以一种被称为 *MarkDown* 的格式编写，你可以在这里了解更多关于[的内容。这就是它们通常被命名为`README.md`的原因。](https://www.markdownguide.org/)

# 第三步:许可

当您的代码即将公开时，您应该为它附加一个许可证，向其他人解释他们应该如何使用您的代码。你可能想要使用一个普通的许可证，比如麻省理工学院的 [*许可证*](https://opensource.org/licenses/MIT) 或者阿帕奇 2.0 的 [*许可证*](https://opensource.org/licenses/Apache-2.0) 。GitHub 会通过点击 *License* 选项来帮助你选择一个，它会给你的包添加一个名为`LICENSE`的新文件。如果坚持不用 GitHub，可以手动添加文件。

# 第四步:重新整理包装

此时，您的包应该是这样的:

```
your_package  
  |-- README.md 
  |-- LICENSE
  |-- __init__.py
  |-- module1.py
  |-- module2.py
```

现在，我们将添加一个文件，从您的代码中构建一个可安装的 Python 包。为此，您必须添加一个名为`setup.py`的新文件，并按照以下方式重新排列您的文件:

```
your_package 
  |-- setup.py 
  |-- README.md 
  |-- LICENSE
  |-- your_package
        |-- __init__.py
        |-- module1.py
        |-- module2.py
```

安装文件规定了 Python 安装程序在安装软件包时需要知道的一切。一个非常基本的，你可以简单地复制粘贴，看起来像这样:

除了编辑你的个人信息，还有两件事你必须记录:

*   **版本:**每次你发布 PyPI 的新版本(我们马上会讨论)，你都必须修改你的版本号
*   安装需要:这是你的包的所有外部依赖的列表。一定要在这里列出所有的东西，这样 Python 会把它们和你的包一起安装

# 第五步:注册 PyPI

我们快到了！接下来:去[pypi.org](https://pypi.org/)创建一个账户。你需要它来上传你的新图书馆。

# 步骤 6:构建和部署！

我们差不多完成了。剩下的工作就是运行一些命令，从您的代码中构建一个可安装的 Python 包，并将其部署到 PyPI。

在开始之前，我们需要安装`twine`，这将允许我们部署到 PyPI。这很简单，因为:

```
pip install twine
```

接下来，我们将创建一个可安装包。转到`setup.py`文件所在的目录，运行:

```
python setup.py sdist bdist_wheel
```

这将创建几个新目录，如`dist`、`build`和`your_package.egg-info`。我们现在关心的是`dist`，因为它包含了我们想要部署到 PyPI 的安装文件。你会发现里面有两个文件:一个压缩的`tar`文件和一个[和*轮子*文件](https://pythonwheels.com/)。很快，全世界都可以看到它们。

***Pro 提示:*** 将这些目录添加到您的`.gitignore`文件中，防止将安装文件推送到您的 repo 中。(*没听说过* `.gitignore` *？* [*读此*](https://git-scm.com/docs/gitignore) )

接下来，通过运行以下命令来验证您刚刚创建的分发文件是否正常:

```
twine check dist/*
```

是时候把你的包上传到 PyPI 了。我建议首先部署到*PyPI*测试域，这样您就可以验证一切是否如您所愿。为此，请使用:

```
twine upload --repository-url https://test.pypi.org/legacy/ dist/*
```

去[test.pypi.org](https://test.pypi.org/)看看你的新图书馆。满意吗？太好了，让我们来看看真正的 PyPI:

```
twine upload dist/*
```

就是这样。从现在开始，每个人都可以使用`pip install`安装你的软件包。干得好！

***Pro 提示:*** 您可以使用 Bash 脚本让您的生活变得更轻松，它将使用一个命令来构建、检查和部署您的包。在与`setup.py`相同的目录下创建一个名为`build_deploy.sh`的新文件。将以下代码复制粘贴到该文件中:

现在你需要做的就是跑:

```
./build_deploy.sh
```

从文件所在的目录，就是这样。您也可以运行:

```
./build_deploy.sh --test
```

上传到测试域。(*注意，在第一次运行之前，您必须使脚本可执行。只需运行文件所在目录下的* `chmod +x build_deploy.sh` *。*)

# 额外的一英里:写一篇博客

你的令人敬畏的代码现在可供每个人使用——但是人们仍然不知道你的令人敬畏的代码现在可供他们使用。你怎么能让他们知道？写一篇博文，告诉你为什么首先要编写这段代码，以及它的用例是什么。分享你的故事，这样其他人就会知道你刚刚用你出色的代码为他们节省了多少精力。

# 最后的话

让你的代码更上一层楼——向全世界发布——在第一次这么做的时候可能看起来很可怕，这既是出于个人原因(*“谁会想要使用它？”*)和技术原因(*“我怎么做得到？”*)。我希望在这篇文章中，我帮助你克服了这些挫折，现在你也可以帮助世界上成千上万的其他程序员。干得好，欢迎来到开源社区！