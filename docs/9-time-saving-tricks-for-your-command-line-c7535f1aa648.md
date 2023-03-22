# 命令行的 9 个省时技巧

> 原文：<https://towardsdatascience.com/9-time-saving-tricks-for-your-command-line-c7535f1aa648?source=collection_archive---------16----------------------->

## 3.充分利用你的历史

![](img/3c9ea1c8b32671d2f2a6a374c6f7bd3b.png)

变得更快，更伟大。Alex Kotliarskyi 在 Unsplash 上拍摄的照片

![H](img/48e23a03d8242a6d12cda96ebf3b6c88.png)  H 你如何写出伟大的代码？通过提高效率。如果你想创造一些很棒的东西，你必须消除那些让你慢下来的时间。只需几个小技巧，你就可以加快工作速度，专注于重要的事情。

# 1.创建别名

每个 shell 都有一个名为`~/.bashrc`或`~/.bash_profile`的文件。您将使用哪一个取决于您的系统。[本帖](https://medium.com/@abhinavkorpal/bash-profile-vs-bashrc-c52534a787d3)详细讲解。

现在想想您经常使用的命令。每次打字都很费力，如果你容易打错字，你会把事情弄糟。这就是别名存在的原因:它们用快捷方式代替了原来的命令。

```
alias $preferredAlias='$commandToAlias'
```

例如，我有一个需要经常访问的 Dropbox 文件夹。由于我不喜欢输入太多的字符，我在我的`~/.bash_profile`中使用了这个别名:

```
alias topbox="cd ~/Dropbox/top-eft/"
```

其他一些有用的别名有:

```
alias mv="mv -i"
alias cp="cp -i"
```

选项`-i`表示在覆盖文件之前会有提示。如果你曾经不小心覆盖了一些重要的东西，你知道我在说什么。

如果您使用了很多别名，那么创建一个单独的文件可能是有意义的。只需将你所有的别名打包到一个名为`~/.bash_aliases`的新文件中。现在，您需要告诉您的原始配置文件别名所在的位置。为此，在您的`~/.bash_profile`或`~/.bashrc`顶部添加以下行:

```
if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi
```

你完了！

无论何时编辑 bash 文件，都要确保运行以下两个相关命令:

```
source ~/.bash_profile
source ~/.bashrc
```

这样，你就告诉你的系统从现在开始坚持你的改变。如果你打开一个新窗口，它将自动获取文件。

![](img/83afd7a61b8b188e2f8ca13165fce04d.png)

创建快捷方式，缩短你的工作日。[维多利亚·希斯](https://unsplash.com/@vheath?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/hacker?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# 2.给你的提示拉皮条

您的命令行提示符是您开始工作时看到的第一样东西，也可能是您离开之前看到的最后一样东西。所以根据你的喜好来定制是有意义的。

我是一个极简主义者，所以我只在提示中包含当前目录。因此，在我的`~/.bash_profile`中，我指定了:

```
PS1='[\W]$ '
```

这里的`\W`是指当前目录，`PS1`是指提示的变量。其他受欢迎的选项有:

*   一个**时间戳**帮助你追溯你的工作。在你的文件中添加`\@`。
*   如果你在远程服务器上工作，添加你的**用户名**和**主机名**是有意义的。添加`\u`和/或`\h`。
*   如果与您的工作相关，您可以添加**外壳**和**外壳版本**。将`\s`和/或`\v`添加到您的文件中。
*   一个**美元符号** `$`通常标志着提示的结束。

您也可以[用彩色打印您的提示](https://www.cyberciti.biz/faq/bash-shell-change-the-color-of-my-shell-prompt-under-linux-or-unix/)。当您正在生成大量输出(或错误消息…)并且希望看到程序从哪里开始时，这可能会有所帮助。

# 3.充分利用你的历史

当然，您不希望一遍又一遍地输入相同的命令。当然还有制表符补全——开始键入命令，然后点击`tab`自动补全。但是如果想要访问您过去的命令呢？有几个选项:

*   上下箭头可让您浏览最近的历史记录。
*   如果你想重新执行你最后的命令，输入`!!`。
*   如果您想从`foo`开始重新执行上一个命令，请键入`!foo`。例如，假设我最后一次使用我最喜欢的文本编辑器时，打开了我的配置文件:`vim ~/.bash_profile`。下一次，我就输入`!vim`。
*   如果想访问前面命令的参数，可以使用`!$`。假设我刚刚用 vim 打开了我的配置文件，但是现在我想使用一个不同的文本编辑器。然后`nano !$`就够了。
*   如果您记住了一个长命令的中间部分，但却记不住第一个字母，该怎么办？您可以使用`ctrl+R`搜索命令，然后键入您知道的字母。找到命令后，像往常一样点击`enter`。
*   如果你想看到你最近发出的大约 500 条命令，只需输入`history`。通过在 shell 配置中添加`HISTSIZE=1000000`和`HISTFILESIZE=1000000`，您可以将存储在历史记录中的命令数量更改为一百万个条目。如果你想删除你的全部历史，输入`rm ~/.bash_history`。

![](img/04f761bfed5d8a9b90ec45d1154b41d9.png)

你的壳历史是你的朋友。由 [David Rangel](https://unsplash.com/@rangel?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/hacker?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# 4.高效利用你的环境

您已经遇到了一些环境变量— `PS1`、`HISTSIZE`和`HISTFILESIZE`。一般来说，这些是用大写字母书写的变量，它们定义了系统的重要属性。

您可以使用`set`命令访问它们的完整列表。另一个例子是`SHELLOPTS`。它列出了在终端会话启动时设置为“on”的所有程序。预设变量的完整文档可在 [GNU 文档](https://www.gnu.org/software/bash/manual/html_node/Bash-Variables.html)中找到。

[](https://medium.com/chingu/an-introduction-to-environment-variables-and-how-to-use-them-f602f66d15fa) [## 环境变量及其使用方法介绍

### 对环境变量内部工作的更深入的指导。

medium.com](https://medium.com/chingu/an-introduction-to-environment-variables-and-how-to-use-them-f602f66d15fa) 

# 5.空壳期权利润

你可以用 shell 选项以多种方式[定制你的 shell](https://bash.cyberciti.biz/guide/Setting_shell_options) 。要显示所有选项，请在终端中运行以下命令:

```
bash -O
bash -o
```

选项`-O`指特定于 bash shell 的选项，而`-o`指所有其他选项。

在显示的列表中，您还可以看到选项是打开还是关闭。您可以通过在配置文件中添加一行来更改默认设置。一些方便的例子是:

```
*# Correct spelling*
shopt -q -s cdspell*# Cd into directory with only its name* shopt -s autocd*# Get immediate notification of background job termination*
set -o notify
```

这里列出的第一个选项使 shell 对于输入错误更加健壮。第二种方法可以让你在每次想改变目录的时候都不用输入`cd`。如果您有许多后台作业正在运行，您可能希望通过将第三个选项添加到您的环境文件中来得到通知。

# 6.按名称查找文件

假设您正在搜索文件`foo.txt`，但是不知道放在哪里了。然后在您的主目录中，键入:

```
find . -name foo.txt
```

这里，`.`代表当前工作目录，您可以用选项`-name`指定文件名。您也可以使用通配符。例如，该命令将以`txt`格式返回所有文件:

```
find . -name *.txt
```

![](img/a0fe27178b503a1303d8b65e80bd738b.png)

更快地找到重要的东西。在 [Unsplash](https://unsplash.com/s/photos/programmer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由 [NESA 拍摄的制造商](https://unsplash.com/@nesabymakers?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 7.按内容搜索文件

假设您想在`foo.txt`中搜索`bar`的所有事件。然后`grep`是你的朋友:

```
grep bar foo.txt
```

如果您想要搜索多个文件，您可以像这样添加它们:

```
grep bar foo.txt foo2.txt foo3.txt
```

如果你想在目录`dirfoo`的所有文件中搜索`bar`，你可以使用递归方式:

```
grep -r bar dirfoo
```

要了解更多选项，您可以查看手册页。只需在你的终端点击`man grep`。

# 8.处理大量输出

有时你会想用`grep`来得到某样东西，但是输出太长了，不能打印在屏幕上。这就是带有符号`|`的管道派上用场的地方。

例如，目录`dirfoo`中有一千个包含`bar`的文件。您可能希望筛选输出，挑选出您感兴趣的文件。然后，您可以键入:

```
grep -r bar dirfoo | less
```

这将以更容易理解的方式显示输出。然后，您可以记下您想要的文件，并通过键入`q`关闭显示。这将使你的命令行界面整洁。

也可以反过来用`grep`。假设你想运行一个产生大量输出的程序`fooprog`。但是你只对包含`bar`的部分感兴趣。

为了使这个输出更容易理解，您可能希望在每次出现`bar`之前添加三行，在其后添加五行。然后，您可以键入:

```
./fooprog | grep -B3 -A5 -i bar
```

这里，`-B3`指的是`bar`之前的三行，`-A5`指的是其后的五行。这样，您可以确保只在终端上打印有意义的信息。

![](img/c9a9123a3f7a877a7b4d0177d917d5ca.png)

使用 grep-command 使代码更容易理解。由 [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/programmer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# 9.在文本中移动

你以为你的终端全是按键？嗯，这里有一个:你可以`alt`-在终端的一行中间点击[。有点笨重，但足以打动你的朋友。](https://stackoverflow.blog/2020/02/12/when-laziness-is-efficient-make-the-most-of-your-command-line/)

然而，能节省你大量时间的是键盘快捷键。首先，我建议你开始采用以下方法:

*   用`ctrl+a`跳到一行的开头。
*   用`ctrl+e`跳到一行的末尾。
*   用`ctrl+u`删除从行首直到光标的所有内容。
*   用`ctrl+k`删除从光标到行尾的所有内容。

你可以使用苹果键盘快捷键的完整列表来采用其他快捷键。它在 Linux 命令行上也能很好地工作。

当然你不可能一下子学会所有的键盘快捷键。我建议从其中的几个开始，一旦你掌握了第一个，就开始下一个。

# 底线:变得更快，变得更神奇。

您已经学习了如何使用别名、提示符、环境变量和 shell 选项定制您的终端。你现在可以访问它的历史，筛选大量的文件，并导航到有意义的部分。

编码是一个持续的学习过程。变得高效永远不晚！

[](https://itnext.io/increase-developer-productivity-with-unix-bash-command-one-liners-2973bccd7600) [## 使用 Unix Bash 命令一行程序提高开发人员的工作效率

### 用一些魔法击中你的终端。有用的 OSX/Linux/Unix bash 终端命令…适合一行！

itnext.io](https://itnext.io/increase-developer-productivity-with-unix-bash-command-one-liners-2973bccd7600)