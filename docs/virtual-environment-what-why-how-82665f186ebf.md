# 虚拟环境:什么，为什么，如何？

> 原文：<https://towardsdatascience.com/virtual-environment-what-why-how-82665f186ebf?source=collection_archive---------55----------------------->

## 使用虚拟环境管理 python 和 python 包

![](img/76660b9cd61a13a9456ca36134de2fc6.png)

图片由 [용한 배](https://pixabay.com/ko/users/yhbae-4048436/) 发自 [Pixabay](https://pixabay.com/)

Mac 预装 python 2。通过安装不同版本的 python 和不同的包，可以在 Mac 上的不同目录中安装更多的 python 环境。了解您正在使用哪种 python 非常重要。

您可以通过在终端中键入以下代码来检查当前正在使用的 python:

```
ericli@ERICYNLI-MB0 ~ % which python 
/usr/bin/python
```

在这个例子中，我目前使用的 python 是预装的 python 2。如果 python 不是您想要使用的，那么您可能会遇到一个小问题。您可以通过将所需 python 的目录放在 Mac 上 path 变量的开头来解决这个问题。但是不推荐这种方法，因为如果您需要为不同的项目使用不同版本的 python 或 python 包，您将不得不一致地升级或降级您的 python 或 python 包。在这种情况下，强烈建议使用虚拟环境。

# 虚拟环境

虚拟环境是包含特定版本的 python 及其相关包的目录。虚拟环境是完全隔离的，因此您可以在 Mac 上拥有任意多个虚拟环境，并且始终非常清楚您正在使用哪个虚拟环境。

我强烈建议每个人在进入下一步之前安装 Anaconda，因为它很简单。在您安装了 Anaconda(【https://docs.anaconda.com/anaconda/install/】)之后，您需要将 Anaconda bin 的目录添加到路径中。要添加目录，只需输入以下代码并键入您的 Mac 密码:

```
ericli@ERICYNLI-MB0 ~ % sudo vim /etc/paths 
Password:
```

对于那些感兴趣的人来说，“sudo”的意思是“超级用户 do”，它授予当前用户管理员权限。输入密码并按 enter 键后，您会看到如下内容:

```
/usr/local/bin 
/usr/bin 
/bin 
/usr/sbin 
/sbin
```

在该文件中，创建一个新行，并将 Anaconda bin 目录添加到该文件中。

```
/usr/local/bin 
/usr/bin 
/bin 
/usr/sbin 
/sbin 
/opt/anaconda3/bin
```

文件保存后，您可以在终端中使用“conda”命令。

*Vim 命令提示:
"o ":在光标下打开新一行
"O ":在光标上方打开新一行
":wq ":保存并退出
":q ":退出
":q！":强制退出(当有不需要的更改时)*

使用 conda 命令，您可以使用所需的 python 版本和 python 包创建虚拟环境。通过激活虚拟环境，您在 Mac 上的路径被修改——虚拟环境的目录被放在路径文件的开头，因此一旦在虚拟环境中找到，您输入的任何命令都将被搜索和执行。

使用 python 版创建虚拟环境并激活它:

```
ericli@ERICYNLI-MB0 ~ % conda create --name py38 python=3.8 ericli@ERICYNLI-MB0 ~ % conda activate py38
```

要检查虚拟环境列表和当前环境(标有星号):

```
(py38) ericli@ERICYNLI-MB0 ~ % conda env list 
# conda environments:
#
base                     /opt/anaconda3
py38                  *  /opt/anaconda3/envs/py38
```

要检查当前环境中安装了哪些软件包，并安装其他软件包:

```
(py38) ericli@ERICYNLI-MB0 ~ % conda list 
(py38) ericli@ERICYNLI-MB0 ~ % conda install PACKAGENAME
```

激活新环境后，您会发现您正在使用的 python 就是您的虚拟环境中的 python。

```
(py38) ericli@ERICYNLI-MB0 ~ % which python /opt/anaconda3/envs/py38/bin/python
```

现在，您的新虚拟环境已经设置好了。您可以通过在终端中输入“python”或“jupyterlab ”(如果您安装了 jupyter lab)来开始使用这个环境。

*最初发表于*[*【http://github.com】*](https://gist.github.com/ynericli/928ce2da9eadfa4031ca5657620eac31)*。*