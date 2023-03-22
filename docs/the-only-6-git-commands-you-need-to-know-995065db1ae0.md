# 科学家应该知道的 6 个 Git 命令数据

> 原文：<https://towardsdatascience.com/the-only-6-git-commands-you-need-to-know-995065db1ae0?source=collection_archive---------14----------------------->

![](img/2631fad56be535f9c45e00f82be52f13.png)

## 将 Git 用于机器学习项目的介绍

我已经使用 [Git](https://git-scm.com/) 很长时间了。有时我会忘记，对于刚刚开始学习如何使用版本控制的数据科学家来说，这可能有点陌生。所以我决定写这个简短的指南，如果你对使用命令行来版本化你的机器学习项目的想法是新的，并且需要一个你应该知道的最少命令的简单备忘单。

> 版本控制如此强大的原因是能够回顾一个项目的历史，并看到两个版本之间的确切变化。这在训练新模型和调整超参数时非常重要。

考虑到这一点，我不打算讨论如何安装 Git。外面有很多很棒的资源[告诉你如何做这个](https://help.github.com/en/github/getting-started-with-github/set-up-git)。不要忘记[设置一个 SSH 密钥](https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)。相反，我将假设您已经安装了 Git 并准备好了。如果您在 Mac/Linux 上在命令行中键入`git`,或者在 Windows 上键入 GitBash，您应该会看到下面的内容。

![](img/6034eb237bb09d27c4da9f4a0c0fbe1b.png)

## 1.初始化

我们将从最基本的命令`init`开始。这将**初始化**一个 Git 项目，这是把 Git 需要的文件添加到你所在文件夹的根目录的一种奇特方式。简单地把`cd`放到你的项目目录中，输入如下内容:

```
git init
```

作为回报，您将收到一条消息，表明您的项目已经准备就绪。

![](img/d4e38b7154a8b8ad0dd209592703e8ba.png)

每个项目只需这样做一次。

## 2.远程添加

现在，多亏了`init`命令，您的项目有了一个本地 repo。虽然您可以做任何您需要的事情来在本地对您的项目进行适当的版本控制，但是您会希望在线创建一个项目来充分利用 Git。大多数开发者都有一个 [GitHub](https://github.com/) 账户，尽管也有很好的替代品，比如 [BitBucket](https://bitbucket.org/) 和 [GitLab](https://about.gitlab.com/) 。我用 GitHub 做我所有的项目。微软收购它之后，他们免费开放了私有存储库。但老实说，所有这些服务对任何刚开始的人来说都是一样的。

在您选择了想要使用的服务之后，继续创建一个新项目。

![](img/4479f7386367fd98ac75b488c18eaf6b.png)

一旦你创建了你的项目，你会得到一个自定义网址。在这种情况下，为此项目生成的是:

```
[https://github.com/jessefreeman/GitExample.git](https://github.com/jessefreeman/GitExample.git)
```

我们现在需要做的就是将本地项目连接到我们创建的在线项目。为此，您将使用以下命令:

```
git remote add origin <PROJECT_URL>
```

您也可以通过 SSH 路径连接，这取决于您的 Git 服务为您提供了什么。

## 3.增加

此时，您的项目就可以在本地跟踪版本了。继续向文件夹添加一个`ReadMe.txt`文件或其他东西。请记住，Git 将忽略空文件夹。一旦您准备好了，您会想要将变更添加到存储库中。 **Staging** 是 Git 跟踪特定文件直到你准备好保存它们的一个版本的一种奇特说法。

要添加文件，只需键入以下命令:

```
git add .
```

虽然您可以在`add`之后按名称指定文件，但是`.`是一个**通配符**，它将自动包含自上次提交以来发生更改的任何文件。

## 4.状态

在任何时候，您都可以获得 Git 项目中文件的列表，并查看它们是否是暂存的。为此，请键入以下命令:

```
git status
```

这将返回 Git 在目录中看到的所有文件的列表。

![](img/9b041443c8c67f9ee8ad697819f028d9.png)

您可以看到当前未添加的文件显示为红色。准备保存的文件将以绿色显示。当您希望在保存更改之前直观地查看当前暂存的所有内容时，文件状态列表很有帮助。

## 5.犯罪

一旦你暂存了你的文件，你就会想要保存它们的一个版本。我们称之为**提交**。Git 将获取您之前添加的文件，并将它们与上一个版本进行比较。然后，它将增量或差异保存在项目根目录下一个特殊的隐藏的`.git`文件夹中。

要保存更改，请在命令行中键入以下内容:

```
git commit -m "Add a message so you know what you did."
```

这可能是迄今为止我们使用过的最复杂的命令。如果我们查看每个部分，我们调用的是`commit`而`-m`代表添加消息。最后，您将在引号中提供提交消息。这样，您可以在以后回顾提交日志，以记住您做了哪些更改。

如果你正在使用 GitHub，在提交代码时，你可以利用一些有趣的特性。我使用[问题渠道](https://guides.github.com/features/issues/)来跟踪我需要在项目中完成的任务。每期都有一个唯一的 ID 号，您可以通过在评论中引用它来链接到它，如下所示:

```
git commit -m "Finished making changes to #1."
```

GitHub 会自动从您的提交中添加一个链接到该问题。这是一个让你工作时保持有条理的好方法。

## 6.推

这个过程的最后一步是**将所有提交推送到 GitHub 上的远程存储库。Git 的独特之处在于，它允许您在本地工作，直到您准备好将它们添加回主存储库。这就是为什么团队可以在他们的电脑上如此高效地工作，然后将所有东西合并成一个项目。**

还记得之前，我们将本地项目链接到在线项目的时候吗？现在，当你告诉 Git`push`所做的更改时，它们将被复制并合并到主项目中。您只需在命令行中键入以下内容:

```
git push --all
```

`--all`类似于我们之前在暂存文件时使用的`.`通配符。这个**标志**告诉 Git 将所有最近的提交推送到默认原点，这是我们用`remote add`命令定义的。

## 冲洗并重复

总而言之，这是使用 Git 的六个基本命令:

1.  `init`创建新项目
2.  将本地项目链接到在线项目
3.  `add`添加要跟踪的文件
4.  `status`返回本地项目中的文件列表
5.  `commit`保存对这些文件的更改
6.  `push`将这些变更复制到在线项目中

现在你可以做**步骤 3–6**来保存和推动你所做的改变。当我知道事情稳定时，我倾向于做出承诺。如果这是一个代码项目，我只在代码工作时提交。对于写作，我倾向于在一节课结束时提交。你可以提交的次数没有限制，尽管如果你做了很多改变，你可能需要不时地[清理 Github repo](https://git-scm.com/book/en/v2/Git-Internals-Maintenance-and-Data-Recovery) 。

> 请务必查看我的一个 [TextGenRNN](https://github.com/minimaxir/textgenrnn) 项目，看看我如何在 GitHub 上设置它。随着您对使用 Git 越来越熟悉，一定要了解如何使用`.gitignore`文件来[排除您不想被跟踪的特定项目](https://git-scm.com/docs/gitignore)。我的项目[有一个您可以用作参考的项目](https://github.com/jessefreeman/MarathonTextGenRNN/blob/master/.gitignore)，它忽略了我生成的模型和一些 python 项目配置文件，这些文件对于希望在本地运行项目的其他人来说并不重要。

最后，值得注意的是 Git 很难存储大的二进制文件，这是使用`.gitignore`文件的另一个原因。你的许多训练数据，如图像、视频或任何超过 50 兆字节的数据都会引起问题。 [GitHub 对大文件的大小有一个硬性限制](https://help.github.com/en/github/managing-large-files/working-with-large-files#conditions-for-large-files)，如果你不小心提交了一个文件，[需要几个额外的步骤来撤销更改](https://help.github.com/en/github/managing-large-files/removing-files-from-a-repositorys-history)，尤其是如果你不熟悉 Git 或者命令行的话。最好将这些文件备份到不同的版本控制系统中。

如果您是 Git 新手或者正在寻找快速复习工具，我希望这有所帮助。如果你在下面留下评论，我很乐意回答你提出的任何问题。