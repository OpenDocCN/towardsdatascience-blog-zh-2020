# 天啊！

> 原文：<https://towardsdatascience.com/venvs-pyenvs-pipenvs-oh-my-2411149e2f43?source=collection_archive---------16----------------------->

## 深入了解不同 python 环境的初学者指南，每种环境的优点，以及如何开始使用它们

![](img/b7b24eeffefac3cf0913bc4fb48d9d0d.png)

由 [Oleksii Hlembotskyi](https://unsplash.com/@lshphoto?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

当开发人员开始研究 python 环境以及如何清理他们的工作流时，他们会受到各种不同选项的轰炸。如此庞大的菜单自然会导致开发人员不必要地筛选文章和文档，以找到“最好”的一个来使用。在本文中，我们将讨论每个主要虚拟环境选项之间的差异和优势，以便将所有这些参考整合到一篇文章中。最后，希望你能找到最适合你需求的环境！

在我们开始之前，如果您不知道什么是虚拟环境或者为什么您应该使用虚拟环境，请随意停下来看看我的[上一篇文章](/why-you-should-use-a-virtual-environment-for-every-python-project-c17dab3b0fd0)强调了虚拟环境给您的整体工作流程带来的好处。

首先，我强烈建议用户在 Python 3.3+之后避免使用。正如您将看到的， **venv** 现在是一个标准的附带库，总体来说更不容易出错。

# VENV

***Venv*** 创建*沙箱化的、全新的、用户可安装库的、多 python 安全的虚拟环境。*

![](img/01e8707f144349cf81b82120d2de12d9.png)

布莱恩·多在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

F***resh***要求环境只从 python 附带的标准库开始；这意味着当环境处于活动状态时，您将不得不用`pip install`重新安装您需要的其他库。

![](img/5548747e35abf75394789f7fe69ccd17.png)

由[马库斯·斯皮斯克](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

S ***加框*** 表示环境中的任何安装或活动都不会影响基本系统。换句话说，你可以假设炸毁你的整个虚拟环境，放火烧了它，并最终把它扔进垃圾桶，删除整个东西，而不必担心弄乱你的基本 python 安装。

![](img/33ffe93ebb169d54a9ea7956486c3c65.png)

Thomas de LUZE 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

U 如果这没有意义，这仅仅意味着您不需要`sudo`许可就可以将库安装到虚拟 env 上。

![](img/73ce0303373b1d072f1a9fafb903e9cf.png)

照片由[戴恩·托普金](https://unsplash.com/@dtopkin1?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

M***ulti-python safe***最后，是 venv 带到桌面上的另一大特色*。*当您激活虚拟环境时，外壳只能看到用于构建该环境的 python 版本。例如，对于使用 mac 和 python 2.7 并希望使用 python 3.5 运行程序的个人而言，不需要指定您希望运行特定文件/程序的 python 版本(即`python3 *file.py*` 与`python *file.py*`)，因为环境中只有一个安装。简单地做`python *filename*`将运行虚拟 env python 版本的软件。

## VENV 用法

为了使用 venv，您只需要在项目目录中输入这个简单的命令(假设您已经安装了 python):

`$ python3 -m venv env`

在本例中，我们刚刚在我们的项目文件夹中创建了一个名为“ **env** ”的 python 3 环境——您可以随意更改名称。这个新的 env 文件夹包含三个子文件夹；虽然这不重要，但出于好奇，这里列出了每一个包含的内容:

*   **bin** :与虚拟环境交互的文件
*   **包含编译 python 包的** : C 头文件
*   **lib**:python 版本的副本，以及安装每个依赖项的 site-packages 文件夹

为了使用该环境的资源，您需要使用以下命令来“激活”它:

`$ source env/bin/activate`

您现在应该在命令行的开头看到一个 **(env)** 。还可以通过使用函数来确保您使用的 python 版本是环境的版本:

`$ which python`

并分析它提供的路径和 python 版本。在环境中完成后，您只需输入:

`$ deactivate`

回到您的“系统”并退出虚拟环境。就这样，简单！

每当您想要使用您的环境时，一直键入`source …/activate`可能会很烦人，但是不要担心——阅读本文末尾的*奖励部分*以查看解决方案！

# PYENV

![](img/eca9d35215237bbee437ef42c042a5b2.png)

兰迪·法特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

P***yenv***主要用于**隔离你机器内的** python 版本。例如，如果您想在 Python 2.7、3.6、3.7 等版本上测试您的代码。pyenv 将提供一种跨所有版本测试你的软件的方法。

该环境通过在 PATH 环境变量前面加上`~/.pyenv/shims`来工作。在高层次上，这几乎只是允许不同的“**隧道**”供您的代码通过，以便在特定的 python 版本上运行。

Pyenv 还使用命令`pyenv install *version*` *使多个 Python 版本的下载和安装变得更加容易。*

Pyenv 与 venv 非常相似，它允许您管理机器中的多个 python 版本。然而，它不包括*回滚库安装*的能力，并且可能*需要管理员权限*来更新库。由于这些争论，我确实认为 venv 更好一点。同样，windows 用户也无法使用该选项。

下一个！

# PIPENV

![](img/60ab3e4017101600053f7c98e8b389aa.png)

照片由 [Cyril Saulnier](https://unsplash.com/@c_reel?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

P ***ipenv*** 是一个类固醇上的`venv`:它力求将`pipfile`、`pip`和`venv`组合成一个单一的命令。

您可以简单地使用`pip install pipenv`安装 pipenv，然后使用`pipenv env`开始创建环境。但是，您必须使用命令`pipenv`(而不是`pip`)来安装您的所有软件包。`pipenv`命令允许您:

*   指定将软件包安装到哪个环境中
*   直接与 PyPi 或本地存储库集成
*   为每个环境创建一个具有单独部分的 pipfile(这是每个环境需要的`requirements.txt`文件的替代文件，带有`virtualenv`和`venv`
*   允许您`pipenv lock`您的虚拟环境；这创建了一个`pipfile.lock`文件，它解决了构建所需的所有依赖关系

![](img/df6b61bcc817d3231d1a7aa3d96f17b2.png)

照片由[莎伦·麦卡琴](https://unsplash.com/@sharonmccutcheon?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

pipenv 带来的最大优势，在我看来，是与`requirements.txt`和`pip freeze`相比，它如何处理**依赖管理**。

Pipenv 由于上面列出的优点正被越来越多的开发人员所采用，并且正迅速获得关注，所以我确实建议看一看它。

然而，如果你不知道这意味着什么，并且上面的列表项目你都没有想到，我建议你坚持使用 venv 来获得一个**更简单和更直接的**环境。

# DIRENV(奖金！！)

如果玩虚拟环境，我强烈建议你使用`direnv`。当你`cd`进入一个包含`.env`的目录时，direnv 会自动激活环境。再也不需要处理那些烦人的`source .../activate`事务了。

# 结论

![](img/2e1978d58321ad5d4bf9667d21b9bc73.png)

由[斯坦尼斯拉夫·康德拉蒂耶夫](https://unsplash.com/@technobulka?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

总的来说，我们研究了开发人员在工作流程中使用的三种最流行的虚拟环境选项。根据你的项目范围，我最终建议两种选择: **venv** 和 **pipenv** 。如果你不需要 pipenv 带来的所有花哨功能，我建议你去看看 venv。另一方面，如果 pipenv 列表对你不利，那就继续使用它吧！

和往常一样，如果你想对一个话题有更多的澄清或者对任何事情感到困惑，请随时留下你的想法，我可以编辑甚至写一篇新文章！