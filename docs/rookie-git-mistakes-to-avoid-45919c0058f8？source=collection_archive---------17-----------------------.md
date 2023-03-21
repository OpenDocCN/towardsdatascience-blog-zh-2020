# 要避免的菜鸟饭桶错误

> 原文：<https://towardsdatascience.com/rookie-git-mistakes-to-avoid-45919c0058f8?source=collection_archive---------17----------------------->

![](img/d77dca6c871eb2512a6e355fad5c1b06.png)

[https://towards data science . com/how-to-think-about-data-28 EC 05 a 75 CD 2](/how-to-think-about-data-28ec05a75cd2)

## 使用 Git 时，做一些简单的事情来避免沮丧和浪费时间

数据工程师通常比数据分析师、数据科学家和 ML 工程师更熟悉 Git 这样的开发工具。在过去的几年里，随着越来越多的非工程工作涉及到编写代码，像 Git 这样的源代码控制系统已经被广泛采用。尽管收养率有所上升，但没有足够多的新收养者对此感到满意。

当雇主没有足够重视培训他们的团队，或者当他们没有从他们的工程同行那里得到足够的支持时，非工程团队经常面临使用 Git 的问题。*人人自立*的模式是不可持续的。培训是团队发展和成长的一个非常重要的部分。

非工程团队在编写代码时有不同的思维方式。我试图在[的另一篇文章](/how-to-think-about-data-28ec05a75cd2)中总结这些观点。在这里，我们将讨论的不是运行错误的 Git 命令可能会犯的错误，而是犯更基本的错误，比如直接在主分支上工作、进行非常大的提交等等。

# 当师父是你唯一的分支

如果你是团队的一员，并且你唯一的分支是 master，那么你基本上错过了 Git(或者一般的版本控制系统)背后的整个概念。在这种情况下，每个人都有一个本地主分支，并提交到该分支，然后将更改推送到远程主分支，发现有无数的冲突。避免这种情况，使用分支——这就是乐趣所在。

在 [StackOverflow](https://stackoverflow.com/questions/5713563/reasons-for-not-working-on-the-master-branch-in-git) 上有一篇 [knittl](https://stackoverflow.com/users/112968/knittl) 写的关于为什么不使用 master branch 进行开发的很好的总结。

> *主分支应该代表代码的“稳定”历史。使用分支来试验新特性，实现它们，当它们足够成熟时，您可以将它们合并回 master。*
> 
> 这样，master 中的代码几乎总是可以顺利构建，并且可以直接用于发布。

# 不遵循分支方法

一旦您理解了分支的基本概念，并且能够创建分支并将它们合并到 master 中，您将遇到的下一个问题更多的是从分支管理的角度来看。所有分支方法中最流行和最有价值的是 gitflow。我从一个数据工程师的角度写了这篇文章。同样的原则也适用于任何使用 pandas、numpy、scipy 等工具用 SQL 或类似的 Python 脚本编写查询的人。

[](/git-best-practices-for-sql-5366ab4abb50) [## SQL 的 Git 最佳实践

### 使用 GitHub、GitLab、BitBucket 等来存储和组织 SQL 查询，以实现发现和重用

towardsdatascience.com](/git-best-practices-for-sql-5366ab4abb50) 

Gitflow 包括三个层次的分支，其中`master`、`develop`和`feature`是三个不同的层次。这三种情况也有例外，但你不必一开始就陷入其中。点击此处了解更多信息—

 [## GitFlow 简介

### GitFlow 是 Git 的一个分支模型，由 Vincent Driessen 创建。它吸引了很多关注，因为它是…

datasift.github.io](https://datasift.github.io/gitflow/IntroducingGitFlow.html) 

# 在一次提交中提交一千个文件

从技术上讲，这不是一个错误，只是一个非常糟糕的做法。除了 repo 中的初始提交之外，您的提交应该只包含可以在单行提交消息中明确定义的更改。*提交所有未提交的更改*对于一千个新文件来说不是一个好的提交消息。

> 小投入，多投入。

按照这条规则去做，你会没事的。这个想法是推动最小的完整工作单元。有一篇很棒的文章深入探讨了这个问题。

[](https://medium.com/better-programming/why-you-should-write-small-git-commits-c9a042737aa6) [## 为什么应该编写小的 Git 提交

### 易于理解的小提交是伟大软件开发的支柱

medium.com](https://medium.com/better-programming/why-you-should-write-small-git-commits-c9a042737aa6) 

这里有几篇我喜欢并找到的其他好文章

*   [Git 提交最佳实践— **Perforce**](https://www.perforce.com/blog/vcs/git-best-practices-git-commit)
*   [Git 最佳实践— **Seth Robertson**](https://sethrobertson.github.io/GitBestPractices/)

# [结论](https://linktr.ee/kovid)

就像 SQL、Python、Javascript 等其他广泛采用的技术一样，如果你想写代码的话，Git 可能是你不可或缺的东西——不管是以什么身份。你肯定要处理版本控制系统。因此，如果你花一些时间去理解它是什么以及它是如何工作的，那是最好的。

[](https://medium.com/@gohberg/the-biggest-misconception-about-git-b2f87d97ed52) [## 对 Git 最大的误解

### 你可能误解了 git。

medium.com](https://medium.com/@gohberg/the-biggest-misconception-about-git-b2f87d97ed52) 

最初，您可能不需要复杂的 Git 命令，因为只需要基本的命令就可以了。有几篇类似于 [this](https://www.infoworld.com/article/3512975/6-git-mistakes-you-will-make-and-how-to-fix-them.html) 的文章讨论了运行错误的 Git 命令所犯的错误。只要把这几条( [1](https://about.gitlab.com/blog/2018/08/08/git-happens/) 、 [2](https://www.educative.io/edpresso/5-git-mistakes-i-made-as-a-junior-developer) 、 [3](https://remi.space/blog/clean-git-history/) 、 [4](https://www.datree.io/resources/git-commands) 、 [5](https://ohshitgit.com) )过一遍，就应该排序了。