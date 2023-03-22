# 不归路:过去 6 个月使用 nbdev 改变了我在 Jupyter 笔记本上编码的方式

> 原文：<https://towardsdatascience.com/the-point-of-no-return-using-nbdev-for-the-past-6-months-changed-the-way-i-code-in-jupyter-2c5b0e6d2c4a?source=collection_archive---------27----------------------->

## 有了 nbdev，Jupyter 笔记本现在是一个有文化的编程环境。有了 nbdev，我的生产力提高了。我的代码现在更有条理了。我处理新项目的方式永远改变了。

![](img/b31773326cc9535252a87e0b43db2792.png)

由[凯特琳·贝克](https://unsplash.com/@kaitlynbaker?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](/s/photos/working?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片。

nbdev 是什么？—如果你在问这个问题，请做好准备，因为你即将发现的东西会让你的大脑爆炸。不是字面意思，不用担心。你会没事的。如果你已经使用了 nbdev，你可能会对我的故事的某些方面感兴趣。请告诉我，我很想听听你的经历。

> `"nbdev`是一个允许你在 [Jupyter Notebooks](https://jupyter.org/) 中完全开发一个库的库，把你所有的代码、测试和文档放在一个地方。那就是:你现在拥有了一个真正的[有文化的编程](https://en.wikipedia.org/wiki/Literate_programming)环境，正如唐纳德·克努特在 1983 年所设想的那样！”—引用 nbdev 存储库。

作为一名研究人员和数据科学爱好者，我每天都与数据打交道。没有空气、水和食物，我们人类就无法生存。对于数据科学家来说，数据应该是列表的一部分。

我在日常生活中使用 Jupyter 笔记本，这是我编码、解决问题和试验新想法的环境。然而**单单 Jupyter 笔记本就有一个重要的局限性**。当需要将代码转移到生产环境并创建 python 包时，有必要将所有代码复制到 Python 模块中。无数次我花了很多时间来重组和清理代码。一个以大量复制、粘贴和调试为特征的过程。

我可以忍受这种工作程序，它并不理想，但我不知道最好的。这在我去年生日的时候改变了。同一天，杰瑞米·霍华德宣布了新的 nbdev 图书馆。这一天当然是一个巧合，但它感觉像一个生日礼物。在读到它的承诺后，我立刻意识到这是一件大事。

在接下来的三个部分中，我将介绍**nbdev 的主要特性**、**如何开始使用 nbdev** 以及**它如何在过去的 6 个月中改变了我的编码方式**。然后，我将最后说几句话。让我们开始吧。

# nbdev 的主要功能

在我看来，nbdev 最令人兴奋的特性如下:

*   从笔记本中自动生成 Python 包；
*   该包的文档也是从笔记本中构建的，包括您可能希望包含在笔记本中的带有图形和图像的代码示例；
*   默认情况下，自动生成的文档可以在 GitHub 页面中以好看的格式查看。
*   当你把你的代码推送到 GitHub 时，会自动进行一系列的检查，以确保笔记本的格式是干净的，你包含在包中的测试也会运行；
*   您的包可以通过一个命令上传到 PyPi。
*   所有这些开发过程都可以扩展到大型项目。如果你想要一个例子，那就去看看从 70 台 Jupyter 笔记本中生成的 fastai2 库。

# 如何开始使用 nbdev

nbdev 入门非常简单。在我阅读了 GitHub 页面上的描述后，我马上开始使用它。任何问题通常会在包装的[文档](https://nbdev.fast.ai/)中回答，或者以检查 [fastai2 笔记本](https://github.com/fastai/fastai2/tree/master/nbs)为例。基本步骤是:

*   从他们提供的[模板中创建一个 GitHub 库](https://github.com/fastai/nbdev_template)；
*   克隆存储库并运行`nbdev_install_git_hooks`命令为您的项目进行配置；
*   用基本信息编辑并完成`settings.ini`文件，比如包的名称、你的 GitHub 用户名等等；
*   开始在 Jupyter 笔记本上开发您的代码；

笔记本的第一个单元格应该包含`#default_exp module_name`,以告诉如何调用从笔记本自动生成的 Python 模块。每个笔记本将是一个不同的 python 模块。为了告诉 nbdev 如何处理每个笔记本单元格，您在单元格的顶部写了一些注释标签。您通常会使用:

*   `#export` —用于包含应导出到 Python 模块的代码的单元格；
*   `#hide` —隐藏不想在文档中显示的单元格。

有几个命令可以在命令行上运行。它们从`nbdev_`开始，你可以使用 tab 补全来找到[所有现有的命令](https://nbdev.fast.ai/cli/)，但是大多数时候你需要`nbdev_build_lib`来构建库和`nbdev_build_docs`来构建文档。

值得一提的是，nbdev 也使得[为你的包创建控制台脚本](https://nbdev.fast.ai/tutorial/#Set-up-console-scripts)变得非常容易！

一旦一切准备就绪，就可以用`pip install -e .`在本地安装这个包，但是你可能想把它上传到 PyPi。在这种情况下，只要你遵循了[配置步骤](https://github.com/fastai/nbdev#adding-your-project-to-pypi)，你只需运行`make pypi`，nbdev 就会处理好一切。

# nbdev 如何在过去的 6 个月里改变我的编码方式

在过去的 6 个月里，在我的大部分项目中使用 nbdev 对我的编码方式产生了显著的影响。不仅仅是因为 nbdev 功能本身，还因为**它让我更多地思考我应该如何构建代码**。为了充分利用 nbdev，代码需要有一些组织。

## 回顾过去

*   我的代码非常杂乱，通常由无数个 Jupyter 笔记本组成，它们有相同进程的不同版本和非提示性的文件名；
*   大多数时候我会为自己写代码；
*   如果几个月后我不得不回头看我自己的代码，我会努力去理解它；
*   我从来不敢创建 PyPi 包，对于一个简单的项目来说，这看起来太费力了。

## 现在看看现在

*   自从我开始使用 nbdev 以来，我的代码逐渐变得更加结构化，更易于共享和重用；
*   现在，我创建我的笔记本，认为它们是要被人类理解的；
*   当我不得不回到一个我暂时没有做的项目时，我可以很容易地通读笔记本来刷新我的记忆；
*   使用 nbdev，创建包只是笔记本中代码的结构和组织的自然结果。从那以后，我创建了一些包。我在另一个名为*的故事中讲述了一个最近的例子，用 Python 分割重叠的边界框。*

[](/split-overlapping-bounding-boxes-in-python-e67dc822a285) [## 在 Python 中分割重叠的边界框

### 从问题公式化到创建 PyPI 包，用 5 个步骤完成教程，使用来自全球…

towardsdatascience.com](/split-overlapping-bounding-boxes-in-python-e67dc822a285) 

# **最后备注**

为人类编写代码是当今世界需要接受的一个概念。Jupyter 笔记本是分享想法和知识的绝佳方式。想法和知识是建设美好未来的基石。如果我们能够更有效地共享构建模块，我们将会加速进步。**把想法打磨成简单优雅的形式需要时间**。但这很重要。因为否则所有试图建立这些想法的人将不得不为了他们自己的理解而单独润色它们。在某种程度上， **nbdev 以一种简单而优雅的形式**让分享想法变得更容易、更省时。nbdev 负责自动化的工作，让开发人员有更多的时间专注于笔记本的展示。

我希望这个故事对你有用并有所启发！

# 关于我

[](/my-3-year-journey-from-zero-python-to-deep-learning-competition-master-6605c188eec7) [## 我的 3 年历程:从零 Python 到深度学习竞赛高手

### 自从 2017 年开始学习 Python 以来，我一直遵循的道路是成为一名独自参加 Kaggle 比赛的大师…

towardsdatascience.com](/my-3-year-journey-from-zero-python-to-deep-learning-competition-master-6605c188eec7) 

*感谢阅读！祝您愉快！*