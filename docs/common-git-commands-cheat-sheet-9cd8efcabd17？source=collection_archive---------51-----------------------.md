# 我的通用 Git 命令备忘单

> 原文：<https://towardsdatascience.com/common-git-commands-cheat-sheet-9cd8efcabd17?source=collection_archive---------51----------------------->

![](img/5840a1dd7a684f79469a8ea6abb86195.png)

照片由[汉娜·约书亚](https://unsplash.com/@hannahjoshua?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在这篇博客中，我记录了一些我经常使用或者为了特定目的使用过几次的常用 git 命令。这肯定不是一个详尽的列表，但更全面，是为了快速参考。

# 初始化/克隆 GIT 存储库

![](img/2db0b31ec2c75ade985ebed4dc8ec009.png)

照片由[丹妮尔·麦金尼斯](https://unsplash.com/@dsmacinnes?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

要创建新的本地 git 存储库:

`git init`

要从本地 git 存储库复制:

`git clone /path/to/repository`

要从本地 git 存储库复制:

`git clone username@host:/path/to/repository`

# 从远程存储库更新本地存储库

要实现(合并)从远程存储库到本地存储库的更改，请执行以下操作:

`git pull`

或者

```
git fetch
git merge <branch_name>
```

NB: *git pull* 只不过是应用了自动分支策略的 *git 获取和合并分支*。

# Git 配置

要配置本地存储库的用户名和电子邮件:

`git config user.name "your_name"`

`git config user.email "your_email_address"`

要为所有提交配置用户名和电子邮件:

`git config --global user.name "your_name"`

`git config --global user.email "your_email_address"`

# 分支

![](img/2f16aa1bb559c8e4fbe1393c5a89568b.png)

照片由[蒂姆·约翰逊](https://unsplash.com/@mangofantasy?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

要创建新的分支和交换机:

`git checkout -b <branch_name>`

要切换到现有分支:

`git checkout <branch_name>`

要查看工作分支:

`git branch`

要删除分支:

`git branch -d <branch_name>`

# 将变更添加到本地存储库中

1.  对文件进行更改。
2.  验证已更改文件的文件列表以及尚未转移或提交的文件:

`git status`

3.将文件添加到暂存/索引:

`git add <file_name>`

4 .将阶段性更改提交给 head:

`git commit -m "Your_message_about_the_commit"`

要提交已添加到暂存中的任何更改以及此后更改但尚未暂存的文件，请执行以下操作:

`git commit -a "Your_message_about_the_commit"`

5.要将更改推送到分支:

`git push origin <branch_name>`

6.要将更改推送到母版:

`git push origin master`

理想情况下，在协作软件开发环境中*拉请求*用于批准对*主*所做的更改。因此，我们应该将我们的变更推送到我们的工作分支，并打开一个拉请求来合并变更。

# 使用 Git Stash

要记录工作目录和索引的当前状态并返回到干净的工作目录:

`git stash save "your_message"`

*“您的消息”使您更容易在存储列表中找到存储状态。*

要获得隐藏的工作目录的有序列表:

`git stash list`

*输出是一个编号列表，如 stash@{0}、stash@{1}等。*

要清除所有存储状态:

`git stash clear`

要删除选定的存储树，在这种情况下，是倒数第二个存储状态:

`git stash drop stash@{1}`

*默认—最后一次隐藏状态*

要从隐藏列表中删除所选的隐藏状态并应用于当前工作树状态，请执行以下操作:

`git stash apply stash@{1}`

*默认—最后一次隐藏状态*

从隐藏列表中删除选定的隐藏状态，并应用于当前工作树状态之上。此外，它删除弹出的存储状态。

`git stash pop stash@{1}`

*默认-上次隐藏状态*

# GIT 命令的日志和历史记录

![](img/f519dc4197885c61155d3cba7cc493a4.png)

丹尼尔·利维斯·佩鲁西在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

要检查 git 命令历史记录(仅 HEAD):

`git reflog [show]`

要检查 git 命令历史记录:

`git reflog show --all`

要检查所有 git 命令的历史记录:

`git reflog --date=iso`

要检查所有“特定命令”(如拉/提交/存储)命令历史记录，包括时间:

`git reflog --date=iso|grep specific_command`

要查看分支中的提交日志:

`git log origin/master`

![](img/a0820a5425bc01663edc406b5dc9f9f4.png)

刘汉宁·奈巴霍在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

**我的链接:** [中](https://medium.com/@nroy0110)|[LinkedIn](https://www.linkedin.com/in/nabanita-roy/)|[GitHub](https://github.com/royn5618)

***感谢光临。希望这有帮助！***