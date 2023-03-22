# 使用 Python 更新 Github 存储库

> 原文：<https://towardsdatascience.com/updating-your-github-repository-using-python-a5c283a011d7?source=collection_archive---------17----------------------->

## 使用 GitPython 自动从远程目录中提取

![](img/16bc75084638bd166df2c5637f5aa31c.png)

照片由 [Richy Great](https://unsplash.com/@richygreat?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/github?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

我每天至少运行两次以下命令:

```
>>>git fetch upstream master
>>>git merge upstream/master -m 'Here we go again...again'
>>>git push
```

每次 10 个单词，在我为期 15 周的训练营中每天跑两次，总共 1500 个单词。客观地说，这相当于我为你写的三篇高质量的博客。真的，必须做点什么。

# **GitPython**

Git python 创建了一个 repo 对象，它允许你与你自己的库或者你的代码所依赖的公共库进行交互。

## [安装](https://github.com/gitpython-developers/GitPython)

```
>>>pip3 install GitPython
```

安装过程会仔细检查库的依赖关系。但是，您应该将您的用户名和密码添加到您的 [git 配置](https://stackoverflow.com/questions/35942754/how-to-save-username-and-password-in-git-gitextension)中。

## 设置

您可以简单地将下面两个代码片段中的任何一个放在脚本的顶部，以编程方式更新您的本地存储库。

如果您想从**远程源**更新您的代码本地存储库，代码很简单:

但是，如果您想从**远程上游存储库**进行更新，代码会稍有不同:

## **申请**

我以前的文章概述了如何使用 python 制作一个[命令行工具。让我们使用该教程来构建一个“自动更新程序”。](/how-to-turn-your-python-scripts-into-an-executable-data-scientists-utility-belt-e2a68889909e)

```
>>>mv autoupdate.py autoupdate
>>>chmod +x autoupdate
>>>cp autoupdate ~/.local/bin/autoupdate
>>>source ~/.zshrc
>>>autoupdate
```

就这样，简单多了！新讲座？没问题。《纽约时报》更新了他们的知识库？我的地图会在下次打印时更新。

## 编码快乐！

![](img/137136b54df016cfe0ba7bdf2836df59.png)

照片由[卡蒂亚·奥斯丁](https://unsplash.com/@katya?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/thumbs-up?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄