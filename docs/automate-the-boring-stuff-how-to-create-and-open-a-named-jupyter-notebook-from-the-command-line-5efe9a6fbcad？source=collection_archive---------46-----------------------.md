# 自动化枯燥的工作:如何从命令行创建并打开一个名为 Jupyter 的笔记本

> 原文：<https://towardsdatascience.com/automate-the-boring-stuff-how-to-create-and-open-a-named-jupyter-notebook-from-the-command-line-5efe9a6fbcad?source=collection_archive---------46----------------------->

今天我醒来，我意识到即使我喜欢自动化事情，我还是不断重复下面的行为:

1.  我决定开始一项新实验。
2.  我在终端上运行 **jupyter-notebook** 。
3.  我在仪表板中创建了一个新笔记本。
4.  我给笔记本重新命名。
5.  我开始黑。

我一直想输入**jupyter-notebook experiment . ipynb**，然后出现一个命名的笔记本。这将为我节省大约 10 秒钟的宝贵时间，我可能会用这些时间来研究更多关于冠状病毒的数字！

因此，这里有一个在你的生活中永远不要给笔记本重新命名的快速方法。它在 MacOs 和 Linux 上运行得非常好，你也可以在 Windows 上做类似的事情。

1.  打开您的 bash 个人资料。我的位于~/。*bash*RC(“~”自动展开到你的主目录)。
2.  将以下代码添加到文件的底部:

```
new-notebook() {
    echo "{
 \"cells\": [],
 \"metadata\": {},
 \"nbformat\": 4,
 \"nbformat_minor\": 2
}" > $(pwd)/$1

    jupyter-notebook $(pwd)/$1
}
```

3.关闭并重新打开终端，或者运行 *source ~/.bashrc.* 这将让您开始使用刚刚创建的函数。

![](img/82fe55f5dcff215ed0a892ab303d172c.png)

全部完成！资料来源:亚历山大·奈特(CC0)

如果您键入**new-notebook example . ipynb**，它将在当前工作目录中创建一个同名的新笔记本，并立即打开它。

请注意，该命令假设您将包含**。文件名中的 ipynb** 扩展名。如果您想避免这样做，只需修改函数代码，如下所示:

```
new-notebook() {
    echo "{
 \"cells\": [],
 \"metadata\": {},
 \"nbformat\": 4,
 \"nbformat_minor\": 2
}" > $(pwd)/$1.ipynb
 #or maybe 'jupyter-notebook', if that's what you use
    jupyter-notebook $(pwd)/$1.ipynb
}
```

重新激活 bash 概要文件后，您现在可以使用该命令，例如，使用 **new-notebook example2** 。

一个建议:我建议不要尝试使用 **jupyter-notebook** 作为命令名。一方面，我发现的唯一重载 bash 函数的技巧相当令人讨厌，可以说**最少*。另一方面，如果我们不乐观，重载 jupyter-notebook 命令的意外行为可能会导致数据丢失。为了安全起见，最好用一个新名字！*

**学到了什么？单击👏说“谢谢！”并帮助他人找到这篇文章。**