# 具有 Github 操作的简单版本化数据集

> 原文：<https://towardsdatascience.com/simple-versioned-datasets-with-github-actions-bd7adb37f04b?source=collection_archive---------48----------------------->

## [实践教程](https://towardsdatascience.com/tagged/hands-on-tutorials)

## 不到 10 行 Python 代码，保存了著名 CEO 的历史

![](img/1e61ae1c710a26b0ad16fb818431a034.png)

凯文·Ku 拍摄于 [Pexels](https://www.pexels.com/photo/coding-computer-data-depth-of-field-577585/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

# 背景

几天前偶然发现了一篇关于作者创造的“git 抓取”概念的文章。这个过程最引人注目的方面是，作为开发人员，我不必维护服务器或大型代码库来获得最新的版本化数据集。我实际上可以利用 git 的版本控制功能和 Github 的动作。这个过程对我来说太有趣了，我不得不亲自尝试一下。这是我记录的旅程，所以你可以对你自己的数据集做同样的事情。

# 查找我的数据集

为了充分利用我的数据集的 git 版本控制，我想寻找一个数据集，在这个数据集上，随着时间的推移，查看它的变化会有附加值。我也不希望这些更改过于频繁，这样提交就会变得稀疏，这样单独的更新就更容易被识别出来。

在未能找到一个好的啤酒厂数据集后，我决定监控这张著名公司首席执行官(CEO)的表格。这个列表以表格的形式出现在维基百科页面上。

 [## 首席执行官名单

### 以下是著名公司首席执行官的名单。的…

en.wikipedia.org](https://en.wikipedia.org/wiki/List_of_chief_executive_officers) 

# 抓取维基百科

现在我的数据集已经确定了，我只需要编写一些代码来检索我们想要存储的信息。我还想确保以一种将来易于接收的方式公开它，以一种自动化的方式，因此考虑到这一点，我决定将这些数据存储为 CSV 文件。

## 要求

对于我提出的解决方案，需要以下 Python 3 包。在最终的存储库中，这些显示在`requirements.txt`文件中。

*   熊猫——这可能是另一个类似于 **tablib** 的库，但是为了以防万一，我选择了**熊猫**。
*   **lxml** —这个库在从 html 表格中获取表格的函数中得到利用。

## 摄入代码

我需要做的第一件事是从维基百科获取表格。幸运的是熊猫正好有这个功能。

在上面的代码中，我使用`read_html`检索该表，将标题标识为第一行，并选择第一个表添加到 DataFrame 中。此时，我已经将文件加载到 Pandas Dataframe 中，但是仍然需要将该文件导出为 CSV 文件格式。在此之前，我注意到摄取的数据有问题。

如您所见，我仍然需要删除显示在结果中的引用注释。为此，我添加了一个正则表达式来去掉这些注释。

首先将数据帧转换成 CSV 文件更容易，所以在应用正则表达式去除注释之前，我先转换了数据格式。在这一点上，我需要做的只是写入我希望数据驻留的文件，数据集已经成功地到达了预期的目的地。

如果您想将此代码用作参考实现，这里是运行来更新数据集的最后一个 main 函数。

由 Github 操作运行的 main.py

# Github 操作工作流程

此时，我能够从 Wikipedia 获取该表，并将清理后的结果写入 CSV 文件。下一步是现在安排这个过程，当且仅当数据改变时，将结果提交给 Github。

在我构建动作工作流的步骤之前，我需要安排它每小时运行刚刚开发的代码。为了做到这一点，我需要以预定义的 yaml 格式为每小时的频率定义 cron 调度。

在构建我的工作流之前，我想定义 Github 动作需要运行的图像。为了简单起见，我只是让它使用最新版本的 ubuntu 运行，但在未来，一旦我有时间更深入地进行操作，我会使用一些更轻松的图像，如 python slim 版本或 alpine。

这个 Github 动作在它的工作流程中有许多步骤，在此之前，数据集可以被监控是否有变化。我们 cron 工作流的第一步是签出我们主分支的工作副本。我最近在实验中注意到，这通常是我利用的大多数行动的第一步。

在上面的第 4 行和第 5 行中，我们实际上不需要指定提取深度，因为默认值是 1，但是这定义了要提取的提交数量。起初，我打算尝试用它来比较数据集的版本，但正如你稍后会注意到的，这是一个不需要探索的兔子洞。我能够使所有这些工作合理化的方法是在我的文章中保留对该功能的引用，以供将来的读者查看。

既然我已经有了在动作工作流中运行的代码的工作副本，下一步将是让 Python 3.8 设置开始运行代码。和前面的步骤一样，下面你可以看到 Github 也为此提供了一个动作。Python 版本很容易设置，但是对于我的例子来说，目标是 3.8。

现在我们已经安装了 Python，我们希望安装所需的库。我在这个 repo 中包含了一个 requirements.txt 文件，但是这个步骤也可能是一个`pip insall pandas lxml`。

在工作流中的这一点上，运行我们的代码以更新数据集所需的一切都已经提供给了环境，因此在这一步中，我们现在将这样做。

此时，我们拥有数据集的最新版本，该版本可能已更改，也可能未更改。因此，我们在这里要做的只是提交，如果数据已经更新，就推送。注意这里我们提前退出动作，如果是这样就不要推了。这里可以使用一些不同的模式，但是如果你好奇的话，Simon Wilson 在这里提到了一些。否则，一个干净的方式来实现我们想要的可以在下面找到。

这是其全部荣耀中的操作流程，您可以随意使用，但此时只需提交不到 10 行的 Python 代码和带有操作工作流的 yaml，一切都应设置为保存历史的版本化数据集。

我通常会发现，当我有一个参考实现时，工作起来会更容易，所以这里是包含完整代码库的 repo，其中包含我刚才单步执行的操作工作流。

[](https://github.com/dylanroy/ceo-dataset) [## Dylan Roy/CEO-数据集

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/dylanroy/ceo-dataset) 

# 关键要点

正如你所看到的，它实际上很容易接收，并保持数据集最新，而不需要你自己的基础设施和一点点代码。Github 动作为简单的自动化提供了更多的选择。在这篇文章的例子中，我的回购是公开的，所以为了支持开源项目，Github 完全免费。

正如你从我的旅程中看到的，用一点 Python 知识，利用 Github 动作开始保持数据集更新并不困难。

我有另一个教程，我将在本周出版，所以请随时关注我，这样你就不会错过它，如果你对我的其他教程感兴趣，我将它们列在下面。

# 阅读迪伦的其他教程

[](/create-beautiful-architecture-diagrams-with-python-7792a1485f97) [## 用 Python 创建漂亮的架构图

### 停止花费时间手动调整未对准的箭头

towardsdatascience.com](/create-beautiful-architecture-diagrams-with-python-7792a1485f97) 

# 资源

*   [https://simonwillison.net/2020/Oct/9/git-scraping/](https://simonwillison.net/2020/Oct/9/git-scraping/)
*   [https://til . simonwillison . net/til/til/github-actions _ commit-if-file-changed . MD](https://til.simonwillison.net/til/til/github-actions_commit-if-file-changed.md)
*   [https://pandas . pydata . org/pandas-docs/stable/reference/API/pandas . read _ html . html](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_html.html)