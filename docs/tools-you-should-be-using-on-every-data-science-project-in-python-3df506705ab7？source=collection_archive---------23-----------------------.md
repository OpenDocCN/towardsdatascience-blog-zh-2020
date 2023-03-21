# Python 中每个数据科学项目都应该使用的工具

> 原文：<https://towardsdatascience.com/tools-you-should-be-using-on-every-data-science-project-in-python-3df506705ab7?source=collection_archive---------23----------------------->

## 保持组织有序和高质量的软件开发工具

数据科学中使用的软件和软件包有许多在线列表。熊猫、Numpy 和 Matplotlib 始终是特色，还有机器学习库 Scikit-learn 和 Tensorflow。

然而，同样重要的是一些不太特定于 DS 的软件开发工具，它们应该成为每个项目工作流程的一部分。

![](img/dfda03de70225ed6034eb3d182cea40c.png)

托德·夸肯布什在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## 饭桶

版本控制对于任何编码项目都是必要的，数据科学也不例外。在任何规模的项目中，跟踪谁在什么时候做了什么，并拥有一个工作的、经过测试的代码的全面历史是无价的，尤其是在与他人合作时。

Git 跟踪对您的代码库所做的更改，维护代码版本的注释审计跟踪。可以从主存储库中克隆代码用于开发，然后提交带有注释的更改，并在充分测试后推回。您可以在任何时候在版本之间切换，并创建分支来分离主要特性的开发，然后将它们合并回主分支。

这允许多人同时处理同一个代码库的不同特性，而没有覆盖其他人的工作的风险。根据您团队的结构和项目的规模，您可能希望在合并之前围绕测试和代码审查制定一些策略。

![](img/df392676bfa7889f4055768beed00cab.png)

照片由[扎克·赖纳](https://unsplash.com/@_zachreiner_?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

GitHub(归微软所有)可能是最受欢迎的 Git 存储库托管服务，但也存在其他替代服务，如 GitLab、BitBucket 和 Launchpad。这些工具都提供了各种各样的管理工具来帮助进一步组织您的项目，例如路线图和问题跟踪。

一个很棒的教程可以在这里找到，但是没有什么比实践更好，所以确保你的下一个项目充分利用了版本控制和它的所有特性。

## 康达

管理您的环境对于代码的可复制性至关重要。没有人希望在克隆一个 git 存储库后，花费数小时修复兼容性错误，安装和重新安装模块。Conda 通过虚拟环境来处理这个问题，你可以在虚拟环境中安装你需要的软件，然后在它们之间轻松切换。这允许您保持您的环境简约和干净，只包括您当前项目需要的包。

Anaconda Navigator 是一个可移植的 GUI，它使得管理 VEs 变得非常容易。只需创建一个新的环境，搜索您想要使用的包并点击“应用”来添加它们。从那里，您可以在终端、直接 Python 或 Jupyter 笔记本中访问该环境。

我经常不得不在多台机器和操作系统上工作，因此快速轻松地安装我需要的工具的能力非常有用，使我可以避免移动数据。

![](img/093c98d6688184cb49a18562e3ff738b.png)

由[大卫·克劳德](https://unsplash.com/@davidclode?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

如果你使用的是终端，Conda 以学习一些命令为代价提供了更多的灵活性。这包括将环境导出为。yml 文件，可以导入到其他地方。将这些环境文件保存在您的 Git 存储库中意味着其他人在尝试运行您的代码之前很容易复制您的环境。

Tim Hopper 在这里详细介绍了使用 Git 和 Conda 的工作流程。

## 统一测试

布莱恩·克尼根[曾经写道](https://www.linusakesson.net/programming/kernighans-lever/index.php#:~:text=Debugging%20is%20twice%20as%20hard%20as%20writing%20the%20code%20in,smart%20enough%20to%20debug%20it.&text=The%20%22cleverness%22%20required%20to%20write,is%20an%20acquired%20mental%20skill.):

> 每个人都知道调试比一开始写程序要难两倍。所以，如果你在编写它的时候尽可能的聪明，你将如何调试它呢？

每个人都知道，当一个项目进行到一半时，事情比你预想的要复杂得多，这种沮丧的感觉。80/20 法则经常适用，你花 20%的时间完成 80%的工作，然后是最后的 20%导致所有的头痛。数据科学项目的复杂性可能会迅速上升，让你的脑袋适应所有的活动部分可能会很棘手。

因此，将您的项目分割成更小、更简单的组件对于协作和创建健壮的代码而不迷失在复杂性中是至关重要的。当处理一小段独立的代码时，单元测试会让您确信它正在按预期工作。这让你可以忘记它是如何工作的，而只关心它做了什么——一个名为“[黑盒抽象](https://en.wikipedia.org/wiki/Black_box)的概念。

![](img/409df854a48dcabb0aa45c32c72206b2.png)

[国立癌症研究所](https://unsplash.com/@nci?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Python 附带安装了 Unittest 框架。使用这个框架，您可以使用各种 assert 语句指定任意多的测试来检查函数的输出。这些可以作为脚本运行，使测试变得容易，并在编辑时继续测试代码的正确性。

例如，假设您正在著名的[泰坦尼克号](https://www.kaggle.com/c/titanic/data)数据集的“Cabin”字段上进行一些特征工程。您可能希望编写一个函数，从船舱值“C38”中提取甲板字母“C”。在编写完函数之后，您可以指定一些测试用例以及预期的输出。如果这些单元测试都像预期的那样运行，那么您可以确信您已经正确地编写了函数。

进一步设想，以后，您希望添加提取客舱号码“38”的功能。在编辑功能时，你不想破坏原有的功能。将这些单元测试放在手边，以确保没有任何东西被破坏(如果有，使用 Git 来恢复您的更改),这使您能够编辑代码，而没有损坏您以前构建的东西的风险。

数据科学中的测试有其自身的一系列挑战。您的代码所依赖的底层数据可能会在不同的测试之间发生变化，这使得难以重现意外的行为，并且数据的大小可能会使常规测试非常耗时。解决这个问题的一个好办法是使用静态的数据子集进行测试。这允许您在几秒钟内明确调试任何奇怪的行为并运行您的代码。您还可以添加虚拟的数据点来测试您的代码如何响应边缘情况。

关于如何使用 Unittest 的更多细节可以在这里找到。

## 还有其他的吗？

你能在上面的列表中添加更多的例子吗？