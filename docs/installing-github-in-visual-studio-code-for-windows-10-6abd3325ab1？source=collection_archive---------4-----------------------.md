# 在 Windows 10 的 Visual Studio 代码中安装 GitHub

> 原文：<https://towardsdatascience.com/installing-github-in-visual-studio-code-for-windows-10-6abd3325ab1?source=collection_archive---------4----------------------->

## VS 代码/ GitHub

## 将 GitHub 与 VS 代码集成的分步安装指南。

![](img/bb5593a789e48154721cf4bf727cc436.png)

照片由 iambipin 拍摄

isual Studio Code 已经成为全球大多数开发人员的首选代码编辑器。它的受欢迎程度每时每刻都在飙升。这要归功于它吸引人的一系列特性，如智能感知，这些特性使它成为开发人员不可或缺的工具。

和 VS 代码一样，GitHub 对于开发者社区也是必不可少的。因此，知道如何在 Visual Studio 代码中安装 GitHub 是非常重要的。对于外行来说，GitHub 是一个基于 Git 版本控制系统的基于 web 的托管服务。

在安装之前，检查 Git 是否安装在 Windows 上是必不可少的。要进行验证，请通过按 windows 键和 r 键打开 Windows 的命令提示符。

![](img/df736744effb6af600e49f14d40285a7.png)

在命令提示符中，键入 git-version 以了解安装的 Git 版本。如果没有安装 Git，命令提示符将返回一条消息，如下图所示。

![](img/450b034f50f4c662216802a961f84745.png)

现在打开 VS 代码，进入它的活动栏的源代码控制。在没有 Git 的系统中，将显示“没有注册源代码管理提供程序”消息。

![](img/df604bd41945c50f6317f41c23387cf4.png)

# Windows 安装

让我们从 https://git-scm.com/downloads 的[下载 git。](https://git-scm.com/downloads)

![](img/5c29d1cba3e742241e15d80d35b73dea.png)

运行。从 Git 网站下载的. exe 文件。按照下面给出的镜像顺序在 Windows 10 中安装 Git。

![](img/e71aec7b25e05b89741a916ae788b741.png)![](img/22c55c3c9d9515bff5cde12431357802.png)![](img/4d1e813a8130c4d7b0625f74ef2b3c17.png)![](img/24d4dd93271cf7c3390e8558bd08d591.png)![](img/9d3045222894f4cb5f022baf6388333b.png)![](img/3ed6cb1fa3d471e3de270b162a471235.png)![](img/289ea8ebd5801548693845b11894c1a9.png)![](img/88e9195249bac8ebca9a6f1b69ca9e93.png)![](img/1adf394af1a6fcdf63b5d48a68c67351.png)![](img/14f7ef6c7b24ad1bb99bdc7da6edca44.png)![](img/38cf39b03eb5b3dc609d93dcd1576c99.png)![](img/ecda2100eb5950bf2f72d8726af26a14.png)

现在已经安装了 Git，让我们通过打开 Git Bash 来验证这一点。

![](img/10d41be7a5a8298260040cc9351ab0f2.png)![](img/4750ef9095e29c4d6e22b6f104d98a34.png)

# 使用 VS 代码从 GitHub 克隆一个 repo

**第一步:**在 GitHub 中选择一个库，点击右上角的绿色按钮**克隆或下载**。复制出现在下拉栏上的链接。

![](img/2e4596e8e4c3b17d1831ff9eb4ad086e.png)

**步骤 2:** 打开 VS 代码，转到文件- >将文件夹添加到工作区…

![](img/3ab0e0daaacd7cfae10f08a508761844.png)

添加新创建的文件夹。

![](img/67bd0f604fbaf3aa7e12e484dbad96c1.png)

VS 代码界面将类似于下图:

![](img/4ab8832788ed46a10f5cd9a958086426.png)

**第三步:**打开终端。

![](img/69754e392439412322ecee8cf34307ca.png)

**步骤 4:** 要链接您的 GitHub 帐户，请键入***git config-global user . name<GitHub 用户 ID >***

![](img/97b3eb06aca2d757916980b221beb8fb.png)

**步骤 5:** 键入 ***git 克隆< url 在步骤 1 >*** 中从 GitHub 复制一个 repo 使用 VS 代码。

![](img/d7da775b61783c4b3c7fdfeb8d686935.png)

安装完成后，VS 代码用户界面将如下图所示。

![](img/9cc28368fe5a95df68ca189c3ac5b4a5.png)

转到活动栏的源代码控制，验证 Git 是否已经正确安装。如果安装正确，UI 将类似于下图(**源代码控制:GIT** 可以在底层图像的左上角看到)。

![](img/8534ef4a11e4e78b290bc320cbb32227.png)

## 附加注释

要从当前工作目录(CWD)打开 VS 代码，有下面提到的两种方法:

**方法一**:打开命令提示符(Windows 键+ R >键入 cmd)。然后通过 ***cd <路径转到所需目录>***

![](img/70ee78f01e5cc9e5b1f2cec7c432434b.png)

然后键入 ***代码。*** 在命令提示符下打开 VS 代码。

![](img/bdb0dd30ad80658f663421b6ded6a571.png)

您可以通过打开终端并检查 Windows Powershell 来验证这一点。

![](img/61a57c7f715608d1fdaccebb2439d4c0.png)![](img/7374d480a509ead1b26c970f53bf3749.png)

**方法二** : ***文件>打开文件夹……***

![](img/00a1c3027ecd9da57ff9607c896707bd.png)

选择所需的文件夹。

![](img/3c244cf56977d48821f134244a9dae88.png)

那么当前工作目录将是所选的文件夹。

![](img/2c8e61facb98f012da6b59a243d90744.png)

我希望这些信息对你们有所帮助。编码快乐！！！