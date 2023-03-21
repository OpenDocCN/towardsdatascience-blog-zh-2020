# 安装 SSH 连接，这样您就可以像处理本地文件一样处理远程文件。

> 原文：<https://towardsdatascience.com/mounting-an-ssh-connection-so-you-can-treat-its-files-as-if-they-were-local-8c3ed0acbb49?source=collection_archive---------42----------------------->

## 您想在本地编辑远程文件吗？尝试这种简单、安全的方法，用 SSHFS 挂载您的远程驱动器。

![](img/bbaf2e9902421367bf7dfabb252418b6.png)

这可能是你。只要打破你所有的灯泡，拿出连帽衫。凯文·霍尔瓦特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

> **场景**:你在 GPU 服务器上有 Pytorch 或 Tensorflow 机器学习指令，或者你的 Raspberry Pi 上有连接到 GPIO 引脚的硬件项目，或者你的安全[气隙网络](https://en.wikipedia.org/wiki/Air_gap_%28networking%29)上有远程 docker 容器，你真的想深入了解一个高级 IDE。
> 
> 在某些情况下，您可以运行一个[远程开发环境](https://code.visualstudio.com/docs/remote/remote-overview)，但是如果您连接到一个轻量级系统，或者您没有完全访问权限，您可能会发现自己面临着使用远程系统命令行上可用的东西——通常是 [nano](https://www.nano-editor.org/) 或 [vim](https://www.vim.org/) 。很好，如果你想快速进出的话，但是他们在用户体验方面并不是很好。

# SSHFS—安全外壳文件系统

随之而来的是 [**SSHFS**](https://en.wikipedia.org/wiki/SSHFS) 。发音就像忍者明星飞过你的头时发出的声音(或者如果发音让你觉得奇怪，你可以大声说出这些字母)。SSHFS 是一种安全地挂载远程文件夹的方式，就像它是本地机器上的一个文件夹一样，而且非常简单。

那是什么意思？

> 这意味着你可以像在本地机器上一样浏览和操作你挂载的远程文件！

这意味着您可以避免来回传输文件，推和拉提交，在小而笨重的命令行编辑器中编辑，这是安全的。还有，我说过容易吗？

# **macOS:**

在您的本地工作站上，从 [FUSE 站点](https://osxfuse.github.io/)或用自制软件安装 FUSE 和 SSHFS:

```
brew cask install osxfuse
brew install sshfs
```

安装完成后，确保在您的远程系统上启用了 ssh，并使用您的终端连接到它。

**这里有一个例子:**

1.  假设您有一台本地地址为 192.168.68.55 的远程机器，您连接到端口`9876`。你的用户名是`ubuntu`，你的文件存储在`~/Documents/MyDir/`。
2.  在本地工作站上创建一个 sshfs 将挂载到的文件夹，或者一个*挂载点*。例如，您可以在您的 local: `~/Documents/projects/remote`上创建这个。

3.键入以下内容:

```
ssh ubuntu@192.168.68.55:/home/ubuntu/Documents/MyDir ~/Documents/projects/remote -p 9876
```

以下是基本规则:

`ssh <user>@<address>:</remote/dir/> </local/mountdir> <options>`

如果工作正常，系统会提示您输入密码(或者您可以使用键设置[无密码 SSH 登录)。当您现在导航到您的项目文件夹时，您将看到一个挂载的驱动器，可以访问您的本地文件系统。很简单，柠檬榨汁机。](https://linuxize.com/post/how-to-setup-passwordless-ssh-login/)

## **卸载**

要卸载本地驱动器上的文件夹，请使用:

```
umount -f /local/mountpoint
```

# Linux 操作系统

运行和在 macOS 上是一样的，只是安装有点不同。另请参见下面的卸载。

## Ubuntu/Debian

```
sudo apt update
sudo apt install sshfs
```

## 红帽，CentOS

```
sudo yum install sshfs
```

## 卸载

```
fusermount -u /local/mountpoint
```

# Windows 操作系统

有几种适用于 windows 的解决方案:

*   [WinFsp](https://github.com/billziss-gh/winfsp/releases/tag/v1.4.19049)
*   [SSHFS-Win](https://github.com/billziss-gh/sshfs-win/releases)
*   [多卡尼](https://dokan-dev.github.io/)

它们都有稍微不同的方法，所以我建议遵循它们文档中给出的说明，但是基本上，您将使用 Windows Explorer 来映射远程 SSH 机器上的网络驱动器。

# 利润！

你有它！这种方法不需要在你连接的机器上做任何繁重的工作，而且它真的让你选择的机器上的事情变得更容易。如果你想多走一英里(我知道你想)，那么看看如何自动安装你的驱动器，让它一直在那里！

快乐嘘！