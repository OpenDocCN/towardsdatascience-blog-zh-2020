# 我如何最大化我的数据科学生产力:PyCharm + Anaconda + JupyterLab

> 原文：<https://towardsdatascience.com/how-i-maximize-my-data-science-productivity-pycharm-anaconda-jupyterlab-2bd045ecadb3?source=collection_archive---------15----------------------->

## 如何开始您的数据科学编码

![](img/a6a3e00a46830e2b34c4eafc8aeaf338.png)

[RawFilm](https://unsplash.com/@rawfilm?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 介绍

不要误解我的意思——我们总是想提高我们的生产力——用同样多的时间，我们可以完成更多的工作。对数据科学研究人员来说也是如此。设置好硬件后，是时候考虑如何选择启动数据科学项目所需的软件了。问题是市面上的选择太多了，出于学习的目的，你可能已经尝试了不同的工具。换句话说，你的购物清单太长了，你可能不知道该从哪里开始。

在本文中，我想分享我发现的适合我的数据科学项目需求的组合。**当然，这不会是一个适合所有人的解决方案。**但如果你对自己的配置没有想法，或许可以先尝试一下。

具体来说，我们将使用三个工具:PyCharm、Anaconda 和 JupyterLab。我将首先介绍安装，然后讨论每个工具的作用。我会尽量简洁，因为如果我倾注太多的信息，初学者会不知所措。

## 装置

**PyCharm** 要安装 PyCharm，可以去 PyCharm 网站:[https://www.jetbrains.com/pycharm/download/#section=windows](https://www.jetbrains.com/pycharm/download/#section=windows)。根据您的操作系统，您需要下载正确的版本。我在一家非盈利教育机构工作，所以我可以接触到专业版。如果你有类似的情况，你可以利用这个好处。然而，如果您主要从事 Python 开发，社区版本应该工作得很好。

下载完成后，按照提示操作即可。没什么特别的。

**Anaconda**
要安装 Anaconda，可以去 Anaconda 网站:[https://www.anaconda.com/products/individual](https://www.anaconda.com/products/individual)。对于我们大多数人来说，我们可以只使用个人版本。但是对于团队和企业还有其他版本。下面是 [**链接**](https://www.anaconda.com/pricing) 对不同方案的比较。

同样，您需要为自己的操作系统选择版本。下载完成后，按照提示操作即可。一切都应该自己说得通。

JupyterLab
你真的不需要为 JupyterLab 下载任何特别的东西，因为一旦你启动了 Anaconda，你就可以在 Anaconda 中非常方便地访问它，它会为你处理所有的安装和其他设置。

# 每个工具的角色

## **皮查姆**

*   **Python 脚本编码。它有以下我喜欢使用的功能。当然，其他的 ide 也有这些特性，但是它们有多好会有所不同。**

```
* **Python coding style check.** It can check if there are problems with the coding style, such as naming and indentation. You'll learn the best practices for Python coding along the way.
* **Auto-completion hints.** The auto-completion suggestions are prompted quickly after you start to type. There are also built-in short snippets that can automatically prompted, such as __init__ method for a class.
* **Code analysis.** It can check whether variables have been used or not, whether any imported modules are used or not, whether certain variables are used before their definitions, and various other analyses. An important feature of code analysis is to inform you about the duplicates, which will help you refactor your code.
* **Definition look-up.** It's very convenient to look up any variable or functions with a shortcut (hold down Cmd or Ctrl and click). It's two-way look-up. If it's a definition itself, it'll prompt the usages. If it's a reference to a variable, it'll direct you to the definition.
* **Refactor.** When you change variable names, change function signatures, or delete files, it will allow you to do it systematically, which will prevent bugging due to these refactoring actions.
```

*   **与版本控制集成。**无论你是数据科学家还是软件工程师，你总是想使用版本控制工具。对于我们中的许多人来说，选择将是 GitHub，它的使用不仅允许我们拥有代码的备份，还可以访问不同版本的代码以进行重构。PyCharm 有一套专门的版本控制管理工具。您不需要了解很多关于 git 命令的知识。大多数操作只需点击即可完成。在我的项目中，我将只使用常见操作的快捷方式，例如 commit (Cmd + K)和 puch commits (Cmd + Shift + K)。
*   **软件包安装提示。**对于许多常见的包，您可以开始用 PyCharm 编写代码。如果所需的软件包没有安装，系统会提示您安装该软件包。在大多数情况下，PyCharm 能够很好地完成工作。
*   **虚拟环境集成。**当您创建一个项目时，您可以指定应该如何设置虚拟环境(接下来将详细介绍)。您可以简单地指定 Conda 作为新的环境管理器。

## 蟒蛇

*   **环境管理。Python 程序员应该不会对虚拟环境这个术语感到陌生。由于 Python 的开源特性，我们有大量可用的包。问题是不同的包可能有不同的需求，因此不可能只有一个 Python 安装和相关的包来满足所有应用程序的需求。
    虚拟环境就是通过创建具有特定依赖关系的虚拟环境，为每个应用程序形成独立的自包含盒子来解决这个问题。**

```
* **Create/Clone New Environment.** You can create a new environment from scratch or clone one from an exiting virtual environment.
* **Import Environment.** If you have set up an environment somewhere else, you can import the setup file, which allows you to re-construct the environment easily with Anaconda.
```

*   **启动应用程序。**在每种环境下，您都可以启动想要使用的应用程序。例如，您可以在这里启动 PyCharm 或 JupyterLab。用于数据科学的许多其他常见应用程序也很容易访问，如 Visual Studio 代码和 RStudio。

## JupyterLab

*   **朱庇特笔记本。**虽然 PyCharm 支持 Jupyter 笔记本，但我发现体验并不好。您的屏幕有两部分——一部分是编码，另一部分是显示结果。因此，为琐碎的工作编辑笔记本是可以的。但是，如果你想有一个更具交互性和响应性的笔记本体验，你可能需要使用 JupyterLab 来编辑笔记本。
*   **笔记本扩展。**许多开发者已经开发出了有用的笔记本扩展。因此，通过在 JupyterLab 中运行笔记本，您可以使用这些扩展来提高工作效率，例如查看目录和变量检查器。

## 在你走之前

这是一个数据科学项目的典型工作流程，这个项目一直在为我工作。当然，它是我到目前为止讨论的这三种工具的组合。

1.  运行 PyCharm 并创建一个项目，使用 Conda 进行虚拟环境管理。
2.  用 PyCharm 编写脚本。如前所述，通过提供代码完成和分析特性，PyCharm 允许您比许多其他 ide 更快地编写代码。我没有提到的另一件事是对科学模式的支持，它为您创建单独的单元来运行更小的代码片段。
3.  创建笔记本。当您准备创建您的 ML 或其他需要更多交互或图形的模型时，您可能希望现在就创建笔记本。在 PyCharm 中创建笔记本很重要，它将为您设置正确的解释器版本。
4.  在 JupyterLab 中编辑记事本。去阿纳康达，在那里发射 JupyterLab。打开创建的笔记本，就可以开始编辑笔记本了。
5.  在这个过程中，不要忘记使用 PyCharm 中的集成工具为您的项目添加版本控制。