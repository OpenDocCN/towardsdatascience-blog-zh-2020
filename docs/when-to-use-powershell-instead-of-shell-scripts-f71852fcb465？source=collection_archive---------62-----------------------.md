# 什么时候用 PowerShell 代替 Shell 脚本？

> 原文：<https://towardsdatascience.com/when-to-use-powershell-instead-of-shell-scripts-f71852fcb465?source=collection_archive---------62----------------------->

![](img/44b6f57c1711419465af5a6ce6f768a1.png)

罗曼·辛克维奇在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 假设我们需要编写一段代码，负责替换现有文件中一条信息，或者只是定期清理一个目录。

要解决这些情况，您认为什么工具可以简单地完成这项工作？

在我看来，shell 脚本是这项工作的合适人选。为什么我会相信？因为它们编写简单、易于理解、易于维护。

对，那么为什么我需要使用其他工具来做同样的工作并得到同样的输出呢？其实你没有，我的想法只是换个方式来说明:)

# 重要的事情先来

[在这里](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell?view=powershell-7)你可以找到关于 Powershell 安装的完整文档。

好了，不多说了，我们来编码吧。

# 使用 Powershell 将数据替换到文件中

回到我们的场景，当我们想要替换现有文件中的一条信息时。假设我们有一个 *json* ，它有多个层次，如下所示:

# 替换字符串值

假设我们想要将“文件名”替换为类似“myAwesomeApp-${shortdate}”的内容。日志”，我们的代码可能是这样的:

文件更新时间:

现在，稍微复杂一点:

# 从数组中移除项目

现在，我们的目标是当“minLevel”是“Info”时删除“rule”。我们的 PowerShell 脚本可以是:

输出:

# 全部放在一起，然后进行一些重构

现在我们可以把所有这些放在一起，做一个小的重构:

最后一个文件是:

# 结论

就像上面的一切一样，没有绝对的真理，你可以做任何你想做的事情，你想做的事情，你可以毫无问题地工作。

但是在我看来，当你想把“面向对象”的行为带到你的任务中时，你应该使用 PowerShell 脚本。

你怎么想？你同意吗？你不同意吗？如果有你认为重要的补充，请告诉我:)

非常感谢您的宝贵时间！

最初发布于[https://devan dops . cloud/when-use-powershell-inst-of-shell-scripts/](https://devandops.cloud/when-use-powershell-instead-of-shell-scripts/)