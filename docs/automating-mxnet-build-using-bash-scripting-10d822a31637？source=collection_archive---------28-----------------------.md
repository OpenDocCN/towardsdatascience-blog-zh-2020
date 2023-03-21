# 使用 Bash 脚本自动构建 MXNet

> 原文：<https://towardsdatascience.com/automating-mxnet-build-using-bash-scripting-10d822a31637?source=collection_archive---------28----------------------->

![](img/94fb1eff574473fc555e4c15eefa7a94.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [Franck V.](https://unsplash.com/@franckinjapan?utm_source=medium&utm_medium=referral) 拍摄的照片

## 为发布软件而构建二进制文件是非常辛苦的。但是使用 Bash 脚本的自动化证明是非常值得的！

作为测试 Apache MXNet 的一部分，我被委托为 Apache MXNet 内部分支构建二进制文件。

在构建和测试 MXNet 二进制文件时，我必须运行无数步骤。此外，该管道必须复制 8 次，以符合 Apache MXNet 支持的各种风格，如 CPU/GPU、+/- MKL 和+/-大张量(2 =8)

这项工作时间紧迫，同时也是重复的。需要按顺序执行某一组步骤。此外，它必须在多台机器上通过微小的调整进行复制。所有这些都需要自动化。

> 任何写了两遍以上的东西都可以而且应该自动化。

Bash (shell 脚本)是完成这项任务的理想选择。由于之前没有什么经验，我热衷于实时应用它。这里尝试捕捉我在这个过程中遇到的与 shell 脚本相关的概念。

> #(哈希)+！(砰)= #！(舍邦)

**从现有 shell 脚本中调用另一个 shell 脚本** 拥有多个 shell 脚本来执行特定任务是一个非常常见的用例(考虑模块化，但在文件级而不是函数级)。由于需要调用另一个脚本，我很快意识到为什么我们需要`shebang`。

有了 bash 的许多替代品(zsh，ksh，sh)，在顶部添加一个 shebang 告诉操作系统(是的！OS ftw！)来使用提到的库。程序连同绝对路径(`/usr/bin/env`)。如果未指定，操作系统将采用默认设置。

**打印/记录**

学习任何语言、测试任何代码、调试任何东西的第一步都是从打印语句开始的:控制台日志记录。打印到标准输出(标准输出)。

`echo`

**向 shell 脚本传递参数**

`./pip_build_test.sh mkl`

给定这个命令，`mkl`是我们需要处理的参数。
在 bash 脚本中使用变量$1、$2 等访问参数
$1 是第一个参数(当然是在命令之后): **mkl**

另一种传递参数的方法叫做 **Flags 方法**，在这篇 [livewire](https://www.lifewire.com/pass-arguments-to-bash-script-2200571) 文章中有很好的解释。

**担心暴露凭据？**

git 命令之一(针对托管在 [AWS 代码提交](https://aws.amazon.com/codecommit/)上的存储库)需要用户名和密码。

`git clone [https://username:password@server](https://username:)`

虽然上面提到的方法在谷歌上的顶级搜索是存在的，但它肯定不是最好的方法。

> 公开 git(或任何)凭证从来都不是一个好主意。

又快又脏的变通方法？省略用户名和密码，让用户自己在机器上配置。具有讽刺意味的是，这是自动化过程中唯一的手工操作。

更好的解决方案？

1.  [git-secrets](https://github.com/awslabs/git-secrets)
2.  [AWS 秘密管理器](https://aws.amazon.com/secrets-manager/) / [SSH 密钥](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_ssh-keys.html)
3.  环境变量！

**将命令输出存储到变量中**

有时，我们需要将命令的结果存储在一个变量中以备将来使用。可以使用`var=$(command)`来实现

**条件句**

这让我们回到了阿尔戈尔！`if`、`else`得到其次是`fi`、
镜像词的使用，得益于[斯蒂芬伯恩、](https://stackoverflow.com/questions/6310008/what-language-uses-fi)被从 Algol 移植到 Unix Shell 脚本语言。

```
if [[ condition ]]
then
    echo "yes"
else
    echo "no"
```

条件可以利用各种标志。例如，少数条件表达式-
`-d`:搜索目录
`-f`:搜索文件
`-z`:如果字符串长度为零，则返回 True

关于进一步的细节`man bash`命令。

**旁注** : *测试 vs 扩展测试*辩论。
`[]` vs `[[]]`:跟双`[[`走。它有增强功能。为什么？[改为](https://stackoverflow.com/questions/3427872/whats-the-difference-between-and-in-bash)。

**将用户输入读入变量**

此外，通过允许用户输入一个可以存储在变量中的值(也是为了将来使用)，可以实现灵活性。


将可用性提高一个档次，我们可以允许用户使用
`read -p “Continue? (Y/N): “ confirm && [[ $confirm == [yY] || $confirm == [yY][eE][sS] ]] || exit 1`来确认

安全第一。终于。

感谢[佩德罗·拉罗伊](https://medium.com/u/7e46baf4a1e0?source=post_page-----10d822a31637--------------------------------)指出 2 个方便的细节。
首先要做的事情。为了使 bash 脚本安全，使用`set -euxo pipefail`很重要。否则，bash 脚本中的错误将不会被发现，因为脚本将总是返回 true。其次，尽可能使用 Python 而不是 Bash。

为什么你会问？去阅读这篇关于[编写安全 Shell 脚本](https://sipb.mit.edu/doc/safe-shell/)的文章。

## 将脚本转换成可执行文件

说了这么多，做了这么多，还有最后一件事:将一个`.sh`文件转换成一个可执行的`.sh`脚本

`chmod u+x <name of the file>.sh`

现在，你可以选择其他权限来代替`u`

*   *u* 代表用户。
*   *g* 代表组。
*   *o* 代表别人。
*   *a* 代表全部。

## **最终外壳脚本**

pip _ build _ 测试. sh

注意:aws-cli 一般可以使用
`sudo apt install awscli`安装

上述使用 awscli-bundle 的过程是旧版本 Ubuntu 14.04 所必需的。(你能相信人们还在用 14.04 吗？老是金！)

这只是我典型的一天工作的一个例子。为 Apache MXNet 做贡献教会了我比在学校里学到的更多的东西。我们欢迎更多这样的贡献在我们的 Github 回购上:【https://github.com/apache/incubator-mxnet/

## 资源

[](https://stackoverflow.com/questions/8967902/why-do-you-need-to-put-bin-bash-at-the-beginning-of-a-script-file) [## 为什么需要放#！脚本文件开头的/bin/bash？

### 感谢贡献一个堆栈溢出的答案！请务必回答问题。提供详细信息并分享…

stackoverflow.com](https://stackoverflow.com/questions/8967902/why-do-you-need-to-put-bin-bash-at-the-beginning-of-a-script-file) [](https://stackoverflow.com/questions/19951369/how-to-store-grep-command-result-in-some-variable-in-shell) [## 如何将 grep 命令结果存储在 shell 中的某个变量中

### 感谢贡献一个堆栈溢出的答案！请务必回答问题。提供详细信息并分享…

stackoverflow.com](https://stackoverflow.com/questions/19951369/how-to-store-grep-command-result-in-some-variable-in-shell) [](https://stackoverflow.com/questions/18544359/how-to-read-user-input-into-a-variable-in-bash) [## 如何在 Bash 中将用户输入读入一个变量？

### 感谢贡献一个堆栈溢出的答案！请务必回答问题。提供详细信息并分享…

stackoverflow.com](https://stackoverflow.com/questions/18544359/how-to-read-user-input-into-a-variable-in-bash) [](https://stackoverflow.com/questions/46823193/git-pull-clone-with-username-and-password-in-aws-code-commit) [## 在 AWS 代码提交中使用用户名和密码进行 Git 拉/克隆

### 我需要使用 https url 作为一个命令行命令来执行 git pull。我需要将这个命令集成到 bash 脚本中。但是…

stackoverflow.com](https://stackoverflow.com/questions/46823193/git-pull-clone-with-username-and-password-in-aws-code-commit)  [## 在线 Bash 编译器-在线 Bash 编辑器-在线 Bash IDE -在线 Bash 编码-练习 Bash…

### 在线 Bash 编译器，在线 Bash 编辑器，在线 Bash IDE，在线 Bash 编码，在线练习 Bash，执行 Bash…

www.tutorialspoint.com](https://www.tutorialspoint.com/execute_bash_online.php) 

*出于隐私和安全的考虑，剧本的内容有所改动。*