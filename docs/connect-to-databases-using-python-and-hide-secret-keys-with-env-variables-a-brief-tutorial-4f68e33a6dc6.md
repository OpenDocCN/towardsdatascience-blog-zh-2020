# 使用环境变量隐藏您的密钥

> 原文：<https://towardsdatascience.com/connect-to-databases-using-python-and-hide-secret-keys-with-env-variables-a-brief-tutorial-4f68e33a6dc6?source=collection_archive---------8----------------------->

## [Python](https://towardsdatascience.com/tagged/python) | [编程](https://towardsdatascience.com/tagged/programming) | [办公时间](https://towardsdatascience.com/tagged/office-hours)

## 关于如何在使用 Python 连接到数据库时使用环境变量保护密码的教程

![](img/468a298822ca2ad8d7a3a8676ef2a100.png)

照片由[丹·尼尔森](https://unsplash.com/@danny144?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

*我向我的读者强烈推荐的课程:*

*   [***Python 中的命令行自动化(data camp)***](https://datacamp.pxf.io/jWm4G0)***→****学习自动化许多常见的文件系统任务，并能够管理和与进程通信。*

# 介绍

假设您正在进行一个数据科学项目，您决定通过使用 Python 建立连接来从公司数据库中提取数据。如果有必要通过 GitHub 分享你的脚本，让更广泛的团队审阅，那该怎么办？那么，在这种情况下，为了避免其他人滥用，你最好隐藏你的秘密细节。

在本文中，我将展示如何通过命令行使用环境变量来保证用户名、*密码*和*数据库名*的安全，避免不必要的信息泄露。

[](/write-datasets-from-g-sheet-to-your-database-with-python-bd2c2643f958) [## 使用 Python 将 Google 电子表格写入数据库

### 在本教程中，学习如何用几行代码构建简单的 ETL 来跨系统移动数据。

towardsdatascience.com](/write-datasets-from-g-sheet-to-your-database-with-python-bd2c2643f958) 

# **环境变量解释**

环境变量是 **KEY=value** 对，类似于 Shell 变量，可以通过命令行*、*创建，但功能更强大，因为一旦声明，它们就可以被任何编程语言使用，因为它们与您的环境/操作系统相关联。

在命令行环境中:

*   变量**键**必须用 ***大写字母*** 书写，并且可以是*字符*、*数字*和*下划线*的混合。
*   另一方面，任何**值**数据类型都可以赋给它们，在 Bash 中使用字符串时引号是可选的，除非字符串包含空格。这是因为 Bash 对空格很敏感，所以包含空格的字符串除非用引号括起来，否则无法正常工作。
*   最后，在“*=”*符号前后不能留有空格，如下所示:

```
**MY_VARIABLE=”my_value”** --> works(capital, no space, quotes)**MY_VARIABLE=my_value** --> works(capital, no space, no quotes but underscore)**MY_VARIABLE=my value** --> fails(capital, no space, space in value)**MY_VARIABLE = ”my_value”** --> fails(capital, space between equal sign, quotes)
```

[](/3-ways-to-compute-a-weighted-average-in-python-4e066de7a719) [## Python 中计算加权平均值的 3 种方法

### 在这篇简短的教程中，我将展示如何在 Python 中计算加权平均值，要么定义自己的函数，要么使用…

towardsdatascience.com](/3-ways-to-compute-a-weighted-average-in-python-4e066de7a719) 

# **配置环境变量**

在使用 Mac OS 时永久声明环境变量的一个简单方法是将它添加到**中。bash_profile** 是在您的主目录中定义的一个隐藏文件，用于在您的系统中配置 Bash Shell。要访问该文件，首先运行您的终端，使用 **cd ~** 导航到主目录，然后在**打开的情况下打开文件编辑器。bash_profile** 命令:

```
$ cd ~$ open .bash_profile
```

在文件编辑器中，为您想要隐藏的秘密信息键入 **KEY=value** 对，前面是 **export** 命令，表明您希望这些变量在 shell 之外也可用:

```
export USERNAME=”Insert Your User Name”
export DBPW=”Insert Your PW"
export DBNAME=”Insert Your DB Name"
```

添加完所有变量后，保存文本文件并用**源刷新 bash 概要文件。bash_profile** 命令:

```
$ source .bash_profile
```

[如果您忽略运行上面的命令，您将无法立即看到刚刚创建的环境变量，您需要退出并重新登录命令行](https://stackoverflow.com/questions/4608187/how-to-reload-bash-profile-from-the-command-line)。

现在，在尝试数据库连接之前，让我们通过在命令行中打开*交互式 Python 控制台*并导入 [**os 模块**](https://docs.python.org/3/library/os.html) 来确保变量确实可以在 Python 中使用。其中，os 模块使 Python 能够使用您的操作系统环境变量。您可以使用 **os.environ** 列出它们，这是一个返回 Python 字典的方法，该字典包含系统中当前可用的所有变量，或者选择每个键，如下所示:

```
**$ ipython****In [1]: import os****In [2]: os.environ['USERNAME']****Out[2]: 'Insert Your User Name'****In [3]: os.environ['DBPW']****Out[3]: 'Insert Your PW'****In [4]: os.environ['DBNAME']****Out[4]: 'Insert Your DB Name'**
```

太棒了。似乎您现在可以在数据库连接设置中用环境变量键替换秘密信息字符串。如果你像我一样使用`psycopg2.connect()`和`pd.read_sql()`方法*和*来连接红移，你会得到这样的结果:

# **结论**

在本教程中，我尝试解决了一个我最近几次遇到的实际问题:在处理共享 Python 脚本时隐藏密钥，我希望你会发现环境变量对你的工作流和对我一样有用。请注意，这只是一个介绍性的指南，还有很多关于环境变量的内容需要学习。例如，我发现一门特别有启发性的课程是由 [DataCamp](https://datacamp.pxf.io/2rdeLQ) 教授的 Python 中的[命令行自动化。](https://datacamp.pxf.io/jWm4G0)

*免责声明:这个帖子包括附属链接，如果你购买的话，我可以免费给你一点佣金。*

# **你可能也喜欢:**

[](/15-git-commands-you-should-learn-before-your-very-first-project-f8eebb8dc6e9) [## 在你开始第一个项目之前，要掌握 15 个 Git 命令

### 您需要掌握的最后一个 Git 教程是命令行版本控制。

towardsdatascience.com](/15-git-commands-you-should-learn-before-your-very-first-project-f8eebb8dc6e9) [](/10-algorithms-to-solve-before-your-python-coding-interview-feb74fb9bc27) [## Python 编码面试前要解决的 10 个算法

### 在这篇文章中，我介绍并分享了 FAANG 中经常出现的一些基本算法的解决方案

towardsdatascience.com](/10-algorithms-to-solve-before-your-python-coding-interview-feb74fb9bc27) [](https://medium.com/analytics-vidhya/5-pythons-sets-problems-to-solve-before-your-coding-interview-41bb1d14ac25) [## 5 套算法解决之前，你的 Python 编码屏幕

### 你对 Python 中的集合了解多少？用这些“简单”和“中等”的 LeetCode 问题挑战自己。

medium.com](https://medium.com/analytics-vidhya/5-pythons-sets-problems-to-solve-before-your-coding-interview-41bb1d14ac25)