# 改善陈旧终端的 5 个技巧

> 原文：<https://towardsdatascience.com/5-tips-to-improve-your-stale-terminal-6426389d9ccd?source=collection_archive---------39----------------------->

## 升级的时间到了

![](img/29412b78b4fc2c3f7e18c5d73d6225bc.png)

由[约书亚·罗森-哈里斯](https://unsplash.com/@joshrh19?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

你可能不知道，但你现在的终端更像一辆卡丁车，而你本可以拥有一辆**法拉利**。

我将向您展示一些很棒的工具，让您的命令行更上一层楼。

# iTerm2

如果您在 Mac 上并使用默认终端。停下来。去安装 [iTerm2](https://iterm2.com/) 。

iTerm2 有许多出色的特性，包括:

*   易于分割窗格，以便在一个屏幕上显示多个终端
*   在您的终端内搜索
*   自动完成
*   轮廓
*   256 色所以看起来很漂亮
*   智能选择—只需一次点击，就可以轻松拷贝您最想要的文本
*   丰富的定制选项

一旦你下载了它，花些时间四处看看，了解所有的选项。开箱即用，虽然，这是一个巨大的升级，并有很大的默认值。

# Zsh

Zsh 是 bash 的一个增强版本，它基本上为您提供了一系列额外的特性，使您在命令行导航时更加轻松。以下是一些例子:

*   自动光盘-你只需输入目录的名称，它改变目录，而不需要光盘。
*   带修正的路径扩展——如果你不记得路径名，但认为“re”在某个地方，只需键入“re ”,点击 tab，zsh 就会扩展所有的可能性。即使“re”位于路径名的中间。它还会自动更正拼写错误。
*   插件和主题——这使得添加更多的功能和美观变得容易。

首先，我会查看一下 [zprezto](https://github.com/sorin-ionescu/prezto) 。它会自动为你设置许多不错的默认设置和功能，并提供易于使用的主题。这里有一个:

![](img/b153ef6daf5d9a7008708a52476c37ee.png)

已经比你默认的终端好看多了。

要查看其他主题，请按照下列步骤操作:

1.  要查看主题列表，请键入`prompt -l`。
2.  要预览主题，请键入`prompt -p name`。
3.  在 *~/中加载你喜欢的主题。然后打开一个新的 Zsh 终端窗口或标签。*

此外，您可以在这里看到您可以打开[的所有模块的列表。我会确保检查出 git 和编辑器模块。这将允许您在终端中集成 git，并打开各种编辑器，如 vim 或 emacs。我喜欢能够在终端中使用 vim 命令。](https://github.com/sorin-ionescu/prezto/tree/master/modules)

# 使过度曝光

经过日晒的将会添加经过广泛研究的漂亮的配色方案，给你很大的对比度，这使得你的命令行容易阅读并且看起来很棒。

[这里的](https://github.com/altercation/solarized/tree/master/iterm2-colors-solarized)是安装 iTerm2 的链接。

# 源代码专业版

事实证明，在编程时，并非所有的字体都是一样的。事实上，许多字体是专为程序员设计的。我目前最喜欢的是带电源线的[源码 Pro。](https://github.com/powerline/fonts/tree/master/SourceCodePro)

源代码 pro 是字体，而 powerline 提供了额外的状态线图形，iTerm 可以将其用于各种应用程序，如 vim 和 zsh。

这种字体是 Adobe 开源库的一部分，因此可以免费使用。从上面的链接下载并安装字体后，您可以在首选项中更改 iTerm2 字体。

# **语法高亮显示**

谁不爱语法高亮？它使得所有的东西都更容易阅读，并且为 zsh 安装起来也很快。如果你没有自制软件，就去[安装](https://brew.sh/)吧。

然后运行以下命令:

```
brew install zsh-syntax-highlighting
```

就是这样！现在你只需重启你的终端，享受语法高亮！

此时，每次打开航站楼，你都应该感觉自己是在驾驶一辆法拉利。您应该会看到一个漂亮的命令行，该命令行已经过定制，可以为您提供最佳的编码体验。你可能会发现很难不向你的朋友炫耀它。😊

这个故事也可以在[这里](https://learningwithdata.com/posts/tylerfolkman/5-tips-to-improve-your-stale-terminal-6426389d9ccd/)找到。

**加入我的** [**邮箱列表**](https://mailchi.mp/86690f2e72bb/tyler_folkman) **保持联系。**