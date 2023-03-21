# 下面是我如何制作一个 CLI 工具来与 Google Colab 配合使用。

> 原文：<https://towardsdatascience.com/heres-how-i-made-a-cli-tool-to-work-with-google-colab-notebooks-7678a88ca662?source=collection_archive---------40----------------------->

![](img/942d430686292e8052939d6f69fd7ba9.png)

colab + cli =💖[杰克纳克科斯](https://unsplash.com/@jakenackos)在 [unsplash](https://unsplash.com/photos/4SHxst61_Sg) 上的照片

## 使用这个 CLI 工具，您可以从您的终端管理 google colab 笔记本

# 我建造的东西

[colab-cli](https://github.com/Akshay090/colab-cli) :一个 cli 工具，用于自动化 google colab 的工作流程，从 CLI 创建、下载或添加笔记本到 google colab，并在 google drive 中组织它们。

# 存在的问题及解决方案

对于每个深度学习实践者来说，google colab 是一个在 Jupyter 笔记本上工作的必去之地。但是从 GitHub 库打开项目，保存到驱动器，然后在驱动器中创建文件夹来组织所有这些笔记本的整个 UX 是非常乏味的。

要将代码保存在 git 资源库中，GitHub 只有一个选项，GitLab 或其他服务都不支持。

因此，我想出了解决所有这些问题的解决方案是 [colab-cli](https://github.com/Akshay090/colab-cli) ，使用这个工具，人们可以从终端使用以下命令轻松打开 google colab 中的任何笔记本

> ***cola b-CLI open-nb filename . ipynb***

该工具的工作原理是将笔记本上传到 google drive，文件结构与本地 git repo 中的相同。

当你完成了谷歌 colab 的工作后

> ***cola b-CLI pull-nb filename . ipynb***

以在本地获取所有这些更改。现在你可以自由地提交它，并把它推到任何地方。

如果在本地对笔记本进行了一些更改，您可以在 google colab 中使用

> ***cola b-CLI push-nb filename . ipynb***

当从头开始一个项目时，首先要在其中初始化 git repo

> ***git init***

因为需要找到项目的根目录。要创建新的 Jupyter 笔记本，请使用

> ***cola b-CLI new-nb my _ nb . ipynb***

这个命令在本地和 google drive 中创建一个新的 Jupyter 笔记本，在 colab 中打开它让你工作。

# 堆栈是什么？在这个过程中，我遇到了问题或发现了新的东西吗？

是用 python 写的。我用 [Typer](https://typer.tiangolo.com) 做了这个 CLI 工具。Typer 有非常好的入门教程和文档。处理 google drive API 的部分由 [PyDrive](https://pythonhosted.org/PyDrive/quickstart.html) 处理。包装部分因[诗](https://python-poetry.org/)而易于管理。

主要耗时的部分是编写这些[实用程序](https://github.com/Akshay090/colab-cli/blob/master/colab_cli/utilities)来处理文件夹和文件的创建和删除。

我还使用了 [gitpython](https://gitpython.readthedocs.io/en/stable) 来获取当前目录的 git 根目录，但是后来我在移动了我需要的文件之后移除了它，因为它有各种其他的依赖项，这增加了包的安装时间。

我从这个项目中了解到一些有趣的事情
*文件夹实际上是带有一些元数据的文件。
*为了从 API 创建一个新的 colab 笔记本，我必须知道 google colab 笔记本的 mime 类型，这显然在任何地方都没有像[官方 google drive API 页面](https://developers.google.com/drive/api/v3/mime-types)那样的文档，为了获得这个 mime 类型，我搜索了 google drive 发出的所有 API 请求，发现

> google colab 笔记本的 mime 类型是:` application/vnd . Google . co laboratory`。

这是一个非常有趣的周末项目。如果您能尝试一下并给出您的反馈或帮助进一步改进，我将不胜感激。

# 演示

你可以在这里观看

## 链接到代码

完整的项目是开源的，你可以在 https://github.com/Akshay090/colab-cli 找到它

# 其他资源/信息

* Typer 教程:[https://typer.tiangolo.com/tutorial/](https://typer.tiangolo.com/tutorial/)
*楼包配诗:[https://typer.tiangolo.com/tutorial/package/](https://typer.tiangolo.com/tutorial/package/)
*帖子图片来源:[https://unsplash.com/photos/4SHxst61_Sg](https://unsplash.com/photos/4SHxst61_Sg)