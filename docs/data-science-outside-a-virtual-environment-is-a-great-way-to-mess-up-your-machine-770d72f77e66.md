# 虚拟环境之外的数据科学是搞乱你的机器的好方法

> 原文：<https://towardsdatascience.com/data-science-outside-a-virtual-environment-is-a-great-way-to-mess-up-your-machine-770d72f77e66?source=collection_archive---------40----------------------->

## 在自己的虚拟环境中运行 Jupyter 笔记本电脑

![](img/6d23b6efba7c4ff1659d4ed3131e66f7.png)

照片由[克里斯蒂娜·莫里洛](https://www.pexels.com/@divinetechygirl?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从[派克斯](https://www.pexels.com/photo/man-standing-infront-of-white-board-1181345/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

在做创业数据科学的时候，我看到过机器由于全局安装软件包而陷入无用的状态。

## 在一种情况下，

*   Spacy 的最新版本用于本地开发。
*   旧版本是在“无代码”供应商服务器上构建模型(在幕后)。
*   还有一个版本运行在 AWS 服务器上。

不仅两者之间存在兼容性问题，而且在没有虚拟环境的情况下，需要在本地不同时间卸载和重新安装不同版本才能使其工作。

此外，全局安装使得在进行部署时很难知道哪些版本应该实际安装在生产服务器上。

## 很容易避免这种废话。

给每个项目一个自己的环境。

你有几个选择。但是如果你刚开始，使用`pip`和`virtualenv`。

现在，我将带您在虚拟环境中设置 jupyter 笔记本电脑。

# 密码

**确保您安装了 python。**

```
$ python3 --version
```

*应该返回类似* `*Python 3.8.5*` *的东西。*

**如果没有，那么安装 python3。**

```
$ brew install python
```

**获取 python 的位置**

```
$ which python3
```

*应该返回类似* `*/usr/local/bin/python3*` *的东西。*

**创建虚拟环境**

```
$ virtualenv --python=/usr/bin/python3 venv
```

*将它指向上一步中 python 的位置。*

**激活您刚刚创建的环境**

```
$ source venv/bin/activate
```

**安装 jupyter**

```
$ pip3 install notebook
```

*这允许创建笔记本。*

很好的进步，但是你还没有完成！如果您现在使用 pip 安装软件包，您的笔记本仍将静默使用全局安装的软件包。而不是您在这个新环境中安装的内容。

**创建 jupyter 内核**

```
$ pip3 install ipykernel
```

**指向虚拟环境**

```
$ python3 -m ipykernel install --user --name=venv
```

现在你可以打开 Jupyter 了

```
$ jupyter notebook
```

单击右上角的“New ”,然后选择您的虚拟环境。

![](img/bb4ba4c15043c10bd86655a2330bb24b.png)

您的笔记本现在将使用安装在这个新环境中的包。

漂亮！

# 结论

我希望你觉得这很有用。并且它将其他人从与上述噩梦相似的命运中拯救出来。

毕竟，它是如此的简单，并且允许你与团队中的其他人共享精确的版本。

如果你有任何关于软件包管理的问题，请告诉我，我会尽力帮助你的！