# 导入错误:没有名为“XYZ”的模块

> 原文：<https://towardsdatascience.com/importerror-no-module-named-xyz-45e4a5339e1b?source=collection_archive---------8----------------------->

## Jupyter 笔记本找不到你已经安装的软件包？让我们解决问题。

![](img/cb343a328c266c24f39fec4d7cc18285.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=medium&utm_medium=referral) 拍摄的照片

如果你像我一样使用 Python 处理数据，那么你应该对标题中的错误信息再熟悉不过了。对你来说，这可能是最容易解决的问题之一。

我一直是这么想的，直到我在我的 Jupyter 笔记本上看到这条错误信息，它几乎让我发疯。

在这篇文章中，我将与你分享我遇到的问题和解决方法。希望能帮你在遇到同样的问题时节省一些时间。

## 模块导入问题

错误信息是找不到模块，所以我试着通过 ***pip install*** 、 ***easy_install*** 和 ***conda install*** 安装软件包，并重启 Jupyter 笔记本几次。

该包仍然无法导入笔记本。

为了确保我已经安装了这个包，我还通过在终端中键入“ ***python*** ”并导入这个包来检查我的 python 环境。这样做没有错误，这表明软件包安装正确。

因此，我很确定只有我的 Jupyter 笔记本有问题。

我想更好地框架如下问题。

> 为什么我不能导入已经安装在我的 Jupyter 笔记本上的 Python 包？

## 检查

要检查的是 Jupyter 笔记本使用的是哪种 python。因此，在 **Jupyter 笔记本**中键入下面的命令来提取可执行路径。

```
import sys
sys.path
```

这是我得到的，

```
'/Users/yufeng/anaconda3/envs/py33/lib/python36.zip',
 '/Users/yufeng/anaconda3/envs/py33/lib/python3.6',
 '/Users/yufeng/anaconda3/envs/py33/lib/python3.6/lib-dynload',
 '/Users/yufeng/anaconda3/envs/py33/lib/python3.6/site-packages',
 '/Users/yufeng/anaconda3/envs/py33/lib/python3.6/site-packages/aeosa',
 '/Users/yufeng/anaconda3/envs/py33/lib/python3.6/site-packages/IPython/extensions',
 '/Users/yufeng/.ipython'
```

然而，如果我在系统的 Python 中键入相同的命令，我会得到以下结果，

```
'/Users/yufeng/anaconda3/lib/python37.zip', '/Users/yufeng/anaconda3/lib/python3.7', '/Users/yufeng/anaconda3/lib/python3.7/lib-dynload', '/Users/yufeng/anaconda3/lib/python3.7/site-packages'
```

到现在为止，我们可以看到 ***Jupyter 笔记本*** 和 ***系统默认 Python*** 在 Python 环境上的区别。所有的包安装都通过 **pip 安装**和 **conda 安装**定向到系统默认的**Python/3.7 和**而不是**py33 和**笔记本所使用的环境。

## 解决方案

从头开始重新构建整个环境肯定是可行的，但是最简单的解决方案是将目标包安装到 Jupyter 笔记本的正确环境中。

在我的例子中，如上所示，Jupyter Notebook 中的可执行 python 是

```
/Users/yufeng/anaconda3/envs/py33/lib/python3.6
```

所以，我用它自己的 **pip** 重新安装包。通常自己的 **pip** 安装在同一个根目录下名为 ***/bin/*** 的文件夹中。

```
/Users/yufeng/anaconda3/envs/py33/bin/python -m pip install plotly
```

在那之后，我能够成功地在我的 Jupyter 笔记本中导入新包(“plotyly”，可以更改为您的目标包名称)。

**就是这样，简单又有用的一招。**

*通过分享这个经验，祝你不要像我一样在问题上浪费很多时间。*

![](img/9b885ef62c17f01cbbf34c191d89640d.png)

斯科特·沃曼在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片