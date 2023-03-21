# 在你开始第一个项目之前，要掌握 15 个 Git 命令

> 原文：<https://towardsdatascience.com/15-git-commands-you-should-learn-before-your-very-first-project-f8eebb8dc6e9?source=collection_archive---------10----------------------->

## 您需要掌握的唯一 Git 教程是在命令行控制版本，并加速您的开发职业生涯

![](img/404ac5e39efeffadbab986a441cc2c77.png)

由[罗曼·辛克维奇](https://unsplash.com/@synkevych?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

作为一名 **web 开发人员**，你刚刚开始你的第一个项目，并且希望跟踪你的工作目录中的大量变化？你最终被选为**一名初级数据工程师**参加面试，并对版本控制话题感到有些生疏？或者您只是从事**数据科学**的工作，想要学习更多关于 Git 和命令行的知识，以便向您的团队展示您根本不使用任何 GUI 也可以获得同样的结果？

如果你回答“是的，我在这里！”至少添加到上面的一个配置文件中，那么本教程肯定可以帮助你通过命令行学习更多关于版本控制的知识，而且会很快完成！

和许多其他数据分析师和 BI 开发人员一样，我通过 **GitHub GUI** 了解了版本控制。当时，我的公司正在使用 [**Looker**](https://looker.com/platform/overview) 作为可视化工具，它可以配置一个方便的 [Git 集成，以便在开发模式下管理文件版本](https://docs.looker.com/data-modeling/getting-started/version-control-and-deploying-changes)。

在提交和部署变更时，每一步在视觉上都非常吸引人和用户友好，直到有一天一位数据工程师告诉我:“版本控制？我总是在我的终端上使用相同的 4–5 个命令，工作完成了！我很少访问用户界面”。

然后他又打了一个字，而我则羡慕地盯着他的笔记本电脑。那时我意识到下一步应该是学习命令行以及如何用 Git 进行版本控制。

在本教程的剩余部分，你会发现我在项目中使用最多的 15 个命令。

[](/3-nanodegrees-you-should-consider-to-advance-your-data-engineering-career-in-2021-baf597debc72) [## 3 纳米学位你应该考虑在 2021 年推进你的数据工程职业

towardsdatascience.com](/3-nanodegrees-you-should-consider-to-advance-your-data-engineering-career-in-2021-baf597debc72) 

## 首先，什么是 Git？

[Git 是一个用于版本控制的命令行工具](https://git-scm.com/docs/git)，只需在 shell 中输入 Git 即可访问。第一步是创建一个新目录并初始化一个存储库:

```
$ **mkdir medium_git**
$ **cd medium_git/**
```

我已经创建了一个 *medium_git* 文件夹，我将在本教程的剩余部分使用它，它将包括下面讨论的所有命令的列表。可以在我的 [GitHub](https://github.com/anbento0490/code_tutorials) 的 code_tutorials 资源库中找到。

## Git 命令解释

**1。git init →** 该命令初始化 *medium_git* 文件夹中的存储库，这意味着从现在开始，它将跟踪文件夹中文件的多个版本。初始化 Git 存储库还会创建一个名为**的目录。存储库文件夹中的 git** 。带有句点前缀的文件和文件夹。)通常是私有的，当您列出文件夹中的文件时，默认情况下不会显示，除非使用 ls -a 命令:

```
$ **git init**Initialized empty Git repository in /Users/antonellobenedetto/Documents/medium_git/.git/$ **ls -a**.  ..  .git 
```

然后，让我们创建一个名为 *git_commands、*的 markdown 文件(`.md`，它只包含对`git init` 功能的简要描述，并将它保存在 *medium_git* 存储库中。

2. **git 添加** →在 git 中，文件可以处于以下三种状态之一:**已修改**、**已登台**、**已提交**。如果您准备提交您修改过的文件，您可以使用`git add [file name]` 命令将它们添加到暂存区。然后，暂存文件被标记为包含在下一次提交中，但尚未提交:

```
$ cat git_commands.md1\. git init <-- initializes a repository inside the folder$ git add git_commands.md
```

使用`git add .`(或者使用通配符`git add *`)可以获得相同的结果。唯一的区别是，通过这种方式，存储库中的所有文件将同时转移。

**3。git 配置 user.name/user.email*→***在你第一次提交之前，告诉 git 你是谁是个好习惯。当您在团队中工作时，这一点尤为重要，这样每个成员都可以识别出是谁做出了某项承诺:

```
$ **git config --global user.name ‘anbento’**
$ **git config --global user.email ‘anbento@myemail.com’**
```

如果您希望检查您的配置设置，请键入`git config -l`。

**4。git commit-m ' your message '**→commit 在某个时间点将文件的快照存储在存储库中。通过构建这些快照的历史，您可以回滚到更早的时间点。 **-m** 标志表示您正在添加一条消息，并且它后面的引号中的文本就是提交消息本身。习惯上，提交消息应该提供一些信息，所以如果我们必须回卷或合并代码，那么很明显已经做了哪些更改以及何时做的:

```
$ **git commit -m ‘Adding git_commands.md file’**[master (root-commit) 815d087] Adding git_commands.md file1 file changed, 3 insertions(+)create mode 100644 git_commands.md
```

**5。git log** →您可以使用这个命令调出存储库的提交历史。每个提交都分配有一个 40 字符长的唯一 ID 或散列。**提交散列是永久的**，这意味着Git 保存它们，并将它们包含在本地和远程 repos 之间的传输中。另请注意，提交作者的姓名和电子邮件如何显示在每个提交历史记录下:

```
$ **git log**commit 815d087f132288112b7e427617be0408e6db4974 (**HEAD -> master**)Author: anbento <anbento@myemail.com>Date: Sat Apr 4 08:19:16 2020 +0100Adding git_commands.md file
```

**6。git remote add[remote name][remote URL]**→有时，您可能希望将您的更改推送到(例如)GitHub 上的一个存储库，以便您可以使用其他设备访问您的完整项目历史，或者只是与您的团队协作。为了做到这一点，您首先必须创建存储库本身，然后在终端中运行命令:

```
$ **git remote add origin** [https://github.com/anbento0490/code_tutorials.git](https://github.com/anbento0490/code_tutorials.git)$ **git remote -v**origin https://github.com/anbento0490/code_tutorials.git (fetch)
origin https://github.com/anbento0490/code_tutorials.git (push)
```

在这种情况下，首先我在我的 GitHub 帐户上创建了***code _ tutorials***资源库，然后我将它作为 remote 添加到 ***origin*** 别名下。请记住"*只是一个约定俗成的名称，但是如果您愿意，您可以随意给遥控器起不同的名字。通过指定 **-v** 选项，您可以检索分配给每个别名的完整远程 url。如果您犯了一个错误，希望移除遥控器，只需运行`git remote remove origin` 命令。*

*7。git push[remote name][branch name]→一旦您对 repo 的本地版本进行了更改，您就可以将它们推送到远程 repo，这样您的项目及其整个提交历史就可以安全地存储在云中。这意味着，即使你的笔记本电脑崩溃或被盗，你的作品也不会丢失，并且可以从你的 GitHub 帐户中找回。为此，您需要**将主分支推至远程*【原点】*回购**:*

```
*$ **git push origin master**Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 282 bytes | 282.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)To [https://github.com/anbento0490/code_tutorials.git](https://github.com/anbento0490/code_tutorials.git)
* [new branch] master -> master*
```

*一旦您这样做了，原来只在本地工作目录中可用的文件将被上传到您的远程 repo 中，以及关于您的提交的信息:*

*![](img/f7076c755c8bc38c6728aee7600f806f.png)*

***8。git clone[repository URL][folder name]→**如果您或您的同事希望在另一个设备上下载您的项目文件夹，您可以通过在本地克隆远程存储库来实现。在我的例子中，我回到了主目录，并克隆了存储库，指定了原来的名称 *medium_git* :*

```
*$ **git clone** [**https://github.com/anbento0490/code_tutorials.git**](https://github.com/anbento0490/code_tutorials.git) **medium_git**Cloning into ‘medium_git’…
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0Unpacking objects: 100% (3/3), done.*
```

*上面的命令不仅创建了一个名为" *medium_git* "的目录，而且还初始化了其中的一个`.git`目录，下载了该存储库的所有数据，并签出了一个最新版本的工作副本。*

*[第一次尝试从远程存储库克隆私有 repo 时，会要求您提供用户名和密码](https://help.github.com/en/github/using-git/caching-your-github-password-in-git) ( [除非已经使用 SSH 密钥连接到 GitHub](https://help.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh) )。凭据可以通过以下方式直接嵌入 url:*

```
*git clone https://your_username:your_pw@github.com/anbento0490/repo_name.git*
```

*提供一次身份验证后，Git 会在内存中缓存您的凭证，并在需要时分发它们。如果`git config credential.helper` 命令返回`manager`，密码被存储在 windows 凭证管理器中，如果它返回`store`，密码被存储在用户文件夹的`.git-credentials`文件中，如果它返回`oskeychain`，密码被存储在钥匙串中。*

***9** 。 **git pull** 对于本教程，为了模拟协作环境中通常会发生的情况，我直接在 GitHub 中编辑了 *git_commands.md* 文件，向列表中添加了更多命令，并提交了更改:*

*![](img/ebe44d87f475bce88bf33ee8dc86e02c.png)*

*此操作创建了一个具有唯一标识符的新提交。在运行以下命令之前，提交和更改仅在远程存储库中可见:*

```
*$ **git pull origin master**remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From [https://github.com/anbento0490/code_tutorials](https://github.com/anbento0490/code_tutorials)
* branch master -> FETCH_HEAD
815d087..b3088e3 master -> origin/master
Updating 815d087..b3088e3
Fast-forward
git_commands.md | 10 +++++++++-
1 file changed, 9 insertions(+), 1 deletion(-)*
```

***10** 。 **git diff [HEAD~1] [HEAD]→** 如果您现在从远程 repo 中提取后运行`git log` ,**您将看到两次提交，最上面的一次是最近的一次。要查看两次提交之间的差异，只需键入:***

```
***$ **git diff HEAD~1 HEAD****diff — git a/git_commands.md b/git_commands.md
index ddd61c8..3cb7e12 100644
— — a/git_commands.md
+++ b/git_commands.md** @@ -1,3 +1,11 @@+Git 15 Most Used Commands
1\. git init ← initializes a repository inside the folder
+2\. git add
+3\. git config user.name/user.email
+4\. git commit -m ‘ your message’
+5\. git log
+6\. git remote add [remote name][remote url]
+7\. git push [remote name] [branch name]
+8\. git clone [repository url] [folder name]
+9\. git pull [remote name] [branch name]***
```

***您可以使用名为 **HEAD** 的**特殊变量**，而不是键入每个散列的前 3-4 个字符，该变量总是指向当前分支中最近的提交。您还可以使用快捷方式来获取旧的提交散列，其中 **HEAD~1** 是本地回购中第二个最新的提交， **HEAD~2** 是第三个最新的提交，依此类推。***

*****11。git fetch[remote name][branch name]→**与`git pull` 完全一样，该命令用于从远程存储库下载文件并提交到本地存储库中。您可以考虑这两个命令的更保守的版本，因为它将下载远程内容，但不更新您的本地 repo 的工作状态，保持您当前的工作不变。如果您在本地有正在进行的未决变更，`git fetch` 将避免冲突的发生，并且通常用于协作环境中，以查看其他人都在做什么，而不会强迫您将变更合并到您的存储库中。***

***为了展示`git fetch`有多有用，我在 GitHub 的列表中添加了另一个命令并提交了更改，这样远程存储库再次比本地存储库有更多的提交。第三个提交 ID 以 *40ae0be* 开始，您可以通过运行以下 3 个命令来验证:***

```
***$ **git fetch origin master**remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From [https://github.com/anbento0490/code_tutorials](https://github.com/anbento0490/code_tutorials)
b3088e3..40ae0be master -> origin/master$ **git checkout origin/master**Note: checking out 'origin/master'.
You are in 'detached HEAD' state. You can look around, make experimental changes and commit them, and you can discard any commits you make in this state without impacting any branches by performing another checkout. If you want to create a new branch to retain commits you create, you may do so (now or later) by using -b with the checkout command again. Example:git checkout -b <new-branch-name>HEAD is now at 40ae0be... Update git_commands.md$ **git log**commit 40ae0bebb77b8df716439b94bb5005a65c0e6301 (**HEAD**, **origin/master**)Author: AnBento <35920666+anbento0490@users.noreply.github.com>
Date:   Sun Apr 5 06:56:07 2020 +0100Update git_commands.mdcommit b3088e373b881a526844826a2dade7b1443eefbb (**master**)
Author: AnBento <35920666+anbento0490@users.noreply.github.com>
Date:   Sat Apr 4 18:39:48 2020 +0100Update git_commands.mdcommit 815d087f132288112b7e427617be0408e6db4974
Author: anbento <anbento@myemail.com>
Date:   Sat Apr 4 08:19:16 2020 +0100
Adding git_commands.md file***
```

***因为提取不会向本地回购添加任何新的提交，所以您应该使用`git checkout origin/master` 从本地分支(**主**)切换到远程分支(**源/主**)并进入所谓的“分离头状态”。这允许您运行熟悉的命令，如`git log` 和`git diff` ，探索您的同事提交给远程回购的内容。***

*****12。git reset [ — flag][#hash]** →让我们假设在获取新的提交之后，您对团队所做的更改感到满意，因此您运行`git pull` 来更新您的本地目录，但是在稍后阶段，您注意到了一个问题，并且想要恢复到倒数第二个提交，或者您只是希望看到项目在更早的时间点是什么样子。在这两种情况下，您都可以运行:***

```
***$ **git reset — hard HEAD~1**HEAD is now at b3088e3 Update git_commands.md***
```

***使用这个命令，Git 切换回具有该特定散列的提交。如上所述，为了使你的工作流程更快，你可以利用**特殊可变头**。 **— hard** 标志将工作目录和 Git 历史重置为特定状态。如果您省略了这个标志，或者使用了 **—软**标志，它将跳过对工作目录的修改，只重置 Git 历史。***

*****13。Git branch[branch name]/Git branch-d[branch name]→**Git branch 允许用户在同一个 repo 中创建几个不同的工作区域。在专业环境中，**通常会创建一个新的分支，每当你希望对一个项目做一些改变或者修复一个 bug** ，然后当我们完成后，将那个分支合并回主分支。当你初始化一个新的本地 repo 时，Git 自动创建 ***主*** 分支:***

```
***$ **git branch**
* master***
```

***要创建新分支，请运行以下命令:***

```
***$ **git branch project1/add_commands**$ **git branch project1/branch_to_delete**$ **git branch*** master
project1/add_commands
project1/branch_to_delete***
```

***这样，您将创建两个分支( **project1/add_commands** 和**project 1/branch _ to _ delete**)，这两个分支现在将与主分支一起列出。分支的名字应该是有意义的，并且清楚地表明它最初被创建的原因(*添加一个新特性，修复一个 bug，执行一个杂务*)。使用`git branch delete [branch name]` 命令删除一个分支:***

```
***$ **git branch -d branch project1/branch_to_delete**Deleted branch project1/branch_to_delete (was b3088e3).***
```

*****14。git checkout [branch name] →** 要切换到新的分支，请运行以下命令:***

```
***$ **git checkout project1/add_commands**Switched to branch ‘project1/add_commands’***
```

***一个常见的快捷方式是使用`git checkout -b [branch name]` 命令创建一个新的分支并立即切换到它。一旦创建了分支并添加了 commit，就可以将分支推送到远程 repo。这允许其他人看到您的更改，并在单独的工作区域与您协作:***

```
***$ **git push origin project1/add_commands**Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 444 bytes | 444.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
remote: Create a pull request for ‘project1/add_commands’ on GitHub by visiting:
remote:[https://github.com/anbento0490/code_tutorials/pull/new/project1/add_commands](https://github.com/anbento0490/code_tutorials/pull/new/project1/add_commands)
remote: To [https://github.com/anbento0490/code_tutorials.git](https://github.com/anbento0490/code_tutorials.git)
* [new branch] project1/add_commands -> project1/add_commands***
```

***在我的例子中，我已经将最后 5 个命令添加到列表中，并通过**项目 1/add_commands** 分支**提交了更改。**最后，我将提交推送到远程回购，这样现在 GitHub 帐户上就可以看到两个分支，其中 **project1/add_commands** 比主回购多显示一个提交:***

***![](img/2881c35eed15e2ecca63625864b7ba49.png)***

***你也可以使用`git branch -r` 来显示遥控器上的所有分支，并确认你的分支在那里。相反，`git branch -a` 将显示本地可用的所有分支:***

```
***$ **git branch -r**origin/master
origin/project1/add_commands$ **git branch -a**master
* project1/add_commands
remotes/origin/master
remotes/origin/project1/add_commands***
```

***15。git merge [new_branch name] → 合并允许开发人员将提交从一个分支复制到另一个分支。这使得协作工作成为可能，因为每个团队成员都可以在他们自己的分支上高效地开发项目功能，而不会发生冲突，然后将它们合并到**主**中，以便最终用户可以访问它们。要将 **project1/add_commands** 合并到 ***master，*** 检出到 master 分支，然后运行`git merge` 命令:***

```
***$ **git checkout master**Switched to branch ‘master’$ **git merge project1/add_commands**Updating b3088e3..cdb97d4
Fast-forward
git_commands.md | 8 +++++++-
1 file changed, 7 insertions(+), 1 deletion(-)$ **git branch -d project1/add_commands**Deleted branch project1/add_commands (was cdb97d4).***
```

***为了避免混淆，一旦丢失的提交被合并到主*分支中，较新的本地分支通常被删除。****

## ******结论******

****对于那些设法完成本教程的人，我要说:“做得非常好！”我希望你在这里学到的，对你的耐心有所回报。在这篇文章中，我带你了解了在你开始你的第一个 web 开发或数据科学项目之前需要掌握的 15 个 Git 命令，因为版本控制很酷，可以帮你省去很多麻烦！如果你想阅读更多，请确保查看 Bitbucket 提供的这个优秀的 [Git 教程](https://www.atlassian.com/git/tutorials/what-is-git)。****

## ****您可能还喜欢:****

****[](/8-popular-sql-window-functions-replicated-in-python-e17e6b34d5d7) [## Python 中复制的 8 个流行的 SQL 窗口函数

### 关于如何利用业务分析中的 Pandas 高效复制最常用的 SQL 窗口的教程…

towardsdatascience.com](/8-popular-sql-window-functions-replicated-in-python-e17e6b34d5d7) [](/airflow-how-to-refresh-stocks-data-while-you-sleep-part-1-a464514e45b7) [## 气流:如何在睡觉时刷新股票数据——第 1 部分

### 在这个关于 Apache Airflow 的第一个教程中，学习如何构建一个数据管道来自动提取、转换和…

towardsdatascience.com](/airflow-how-to-refresh-stocks-data-while-you-sleep-part-1-a464514e45b7)****