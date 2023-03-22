# 家酿和 Pyenv Python 愉快地合作

> 原文：<https://towardsdatascience.com/homebrew-and-pyenv-python-playing-pleasantly-in-partnership-3a342d86319b?source=collection_archive---------13----------------------->

## 如何使用 pyenv 的 Python 来满足自制程序的依赖性，因此只有 pyenv 管理您机器上的 Python 3 安装

![](img/8487759d6b8ae22a32ec7ac0729a1194.png)

接下来，我要清理我的桌子！图片作者。

像[很多](/how-to-setup-a-python-environment-for-machine-learning-354d6c29a264) [数据](/power-up-your-python-projects-with-visual-studio-code-401f78dd97eb) [科学家](/the-python-dreamteam-27f6f9f08c34)和 [Python](/guide-of-choosing-package-management-tool-for-data-science-project-809a093efd46) [开发者](/how-to-setup-an-awesome-python-environment-for-data-science-or-anything-else-35d358cc95d5) [之前](/managing-virtual-environment-with-pyenv-ae6f3fb835f8) [我](/power-up-your-python-projects-with-visual-studio-code-401f78dd97eb)，[我已经](/setting-up-your-data-science-work-bench-4a8d3a28205c) [给了](/python-environment-101-1d68bda3094d)[上](/the-top-4-virtual-environments-in-python-for-data-scientists-5db1c01fd779)管理我自己的 Python 构建并转向 [pyenv](https://github.com/pyenv/pyenv) (链接到《走向数据科学》中的编年帖子)。在不同的时间点，我自己从源代码中构建 Python 版本或者在 OSX 上使用[预建框架安装程序](https://www.python.org/downloads/mac-osx/)，同时手工管理`/usr/local/bin`中的链接。使用这两种方法中的任何一种，我从任何已安装的版本中旋转虚拟环境都没有问题，但是最近我和 PATH 混淆了我们是使用家酿的`python@3.8`还是我安装的框架版本。输入 pyenv。如果你在寻找如何设置 pyenv，[有](https://amaral.northwestern.edu/resources/guides/pyenv-tutorial) [有](https://opensource.com/article/20/4/pyenv)[无](https://wilsonmar.github.io/pyenv/)[文章](https://realpython.com/intro-to-pyenv/) [或](https://medium.com/python-every-day/python-development-on-macos-with-pyenv-2509c694a808) [博客](https://medium.com/@weights_biases/pyenv-tutorial-for-machine-learning-9638e43a790f) [帖子](https://sourabhbajaj.com/mac-setup/Python/) [上](https://anil.io/blog/python/pyenv/using-pyenv-to-install-multiple-python-versions-tox/) [如何](https://stackabuse.com/managing-python-environments-with-direnv-and-pyenv/) [到](https://duncanleung.com/set-up-python-pyenv-virtualenv-poetry/) [安装](https://binx.io/blog/2019/04/12/installing-pyenv-on-macos/) [它](https://www.digitalocean.com/community/tutorials/how-to-manage-python-with-pyenv-and-direnv)正如这些文章中提到的，有许多方法可以在您的 OSX 系统上安装 Python:

*   有内置的 Python 2.7.x(注:Python 2 自 2019 年 12 月起已弃用)。
*   Homebrew 将提供一个 Python 3 版本来满足它自己的依赖性。
*   你可以有框架版本。
*   您可以从源代码构建它。
*   或者像许多人建议的那样，您可以使用 pyenv 来管理版本。

您的系统需要内置的，而 Homebrew 实际上只会为您保留一个版本的 Python 3，所以如果您想管理 Python 2 或 3 的特定版本，您需要在后三个版本中进行选择。我省略了 Anaconda，因为我不需要它，也没有花时间去真正理解它是如何工作的，但是我听说过 Windows 用户的成功故事。在这里，我们将选择 pyenv 来管理 python 版本，但是这样做的话，我们的系统上最初会有多个版本的 Python 2 和 Python 3(除了 builtin 版本 2 和 homebrew 版本 3 之外，还有来自 pyenv 的版本)。再说一次，我们对 Python 2 的内置无能为力，OSX 需要它。**对于 Python 3，我们可以让自制软件*使用 pyenv 的 Python 版本之一*来消除冗余的来源。**在我们开始之前，值得注意的是，如果你像 pyenv 推荐的那样使用你的`PATH`，你不太可能经历 pyenv 和 homebrew 之间的[冲突，并且](https://stackoverflow.com/questions/32018969/coexistence-of-homebrew-and-pyenv-on-macosx-yosemite/64364243#64364243) [Homebrew 对此发出警告](https://docs.brew.sh/Homebrew-and-Python)；你正在涉水进入依靠你自己的理解来支持这个用例的领域(正如家酿在他们的帖子中所说的)。

# 使用 pyenv 的 Python 来满足自制依赖

这些[堆栈溢出答案](https://stackoverflow.com/questions/30499795/how-can-i-make-homebrews-python-and-pyenv-live-together)有使用符号链接(symlinks)的正确想法，但是据我所知，他们没有得到正确的链接。让我们把它们做好！

如果你已经有了任何家酿的 python3 的痕迹，首先把它们去掉(`brew unlink python3`和`brew uninstall python3`)。如果你已经有了来自 Homebrew 的 python@3.8，当卸载它的时候，记下依赖它的包(例如，`ffmpeg`，然后重新安装它们。

在这篇文章中，Homebrew 预计是 Python 3.8.6，因为它是 python@3.8，所以首先按照他们的文档安装 pyenv [的那个版本:](https://github.com/pyenv/pyenv)

```
pyenv install 3.8.6
```

这将(默认情况下)把实际的 Python 安装放在

```
~/.pyenv/versions/3.8.6
```

现在我们只需要添加一个链接，然后让 brew 完成剩下的工作。我将在这里使用完整路径，因此您可以从任何地方运行它(记住在头脑中把`ln -s ... ...`读成“link-symbolic[target][linkname]”):

```
ln -s ~/.pyenv/versions/3.8.6 $(brew --cellar python)/3.8.6
```

使用`-f`标志，您可以省略后面的`/3.8.6`，因为`ln`将使用目标的名称。为了在链接上尽可能清晰，您应该

```
ln -s ~/.pyenv/versions/3.8.6 /usr/local/Cellar/python@3.8/3.8.6
```

结果应该是这样的:

```
➜  ~ ll $(brew --cellar python)
total 0
lrwxr-xr-x  1 [my username]  admin    36B Oct 14 16:52 3.8.6 ->
/Users/[my username]/.pyenv/versions/3.8.6
```

最后，让家酿管理其余必要的链接:

```
brew link python3
```

这篇文章的灵感来自于我自己对如何让它工作的探索，而这个关于栈溢出的问题的答案是没有答案的([我回答了，我想！](https://stackoverflow.com/a/64364156/2577988))。