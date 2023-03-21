# SimpleGAN —用 3 行代码训练 GAN

> 原文：<https://towardsdatascience.com/simplegan-train-gans-with-3-lines-of-code-c221bbf244?source=collection_archive---------28----------------------->

## 基于 TensorFlow 简化生成模型训练的框架

![](img/aebf1875daab9cc3f458f134c073479a.png)

来源:[https://pix abay . com/photos/fall-autumn-red-season-Woods-1072821/](https://pixabay.com/photos/fall-autumn-red-season-woods-1072821/)

# 介绍

近年来，深度学习背景下的生成模型领域一直在快速增长，特别是自从敌对网络出现以来。然而，训练这些模型并不总是容易的，即使你是一个只是试图在自定义数据集上复制结果的专家。解决方案: **SimpleGAN** 。SimpleGAN 是一个使用 TensorFlow 2.0 编写的框架，旨在通过提供高级 API 来促进生成模型的训练，同时提供强大的可定制性来调整模型和运行实验。

# 装置

安装 SimpleGAN 是一个非常简单的过程。有两种方法可以执行安装。

*   使用 pip 软件包管理器。

```
$ pip install simplegan
```

*   从源构建

```
$ git clone https://github.com/grohith327/simplegan.git
$ cd simplegan
$ python setup.py install
```

# 例子

现在您已经安装了软件包(如果没有，您应该😁)，让我们来看两个例子，帮助你入门。

让我们看看如何使用 SimpleGAN 框架训练卷积自动编码器

## Pix2Pix

现在让我们看一个例子，其中我们将利用对抗性训练来将图像从一个域翻译到另一个域，例如将分割图转换成具有细节的图像。看看这个[链接](https://phillipi.github.io/pix2pix/)。

来源:[https://tenor . com/view/Cheetos-零食-饿了么-yum-gif-16187308](https://tenor.com/view/cheetos-snacks-hungry-yum-gif-16187308)

## 注意:

对于那些可能想知道*“这不是 3 行代码”*的人，上面的例子只是为了展示框架的可用功能，从技术上讲，你仍然只需要下面显示的 3 行代码来训练你的模型。

```
>>> gan = Pix2Pix()
>>> train_ds, test_ds = gan.load_data(use_maps = True)
>>> gan.fit(train_ds, test_ds, epochs = 100)
```

所以，是的，这不是一个诱饵。

# 重要链接

*   [文档](https://simplegan.readthedocs.io/en/latest/index.html) —查看文档，更好地理解框架提供的方法
*   [示例笔记本](https://github.com/grohith327/simplegan/tree/master/examples)—colab 笔记本列表可以帮助您入门并进一步理解框架
*   [问题](https://github.com/grohith327/simplegan/issues) —如果您发现框架有任何错误，请在 Github 页面提出问题

# 结论

开发该框架是为了简化具有高级抽象的生成模型的训练，同时提供一些定制模型的选项。我相信“边做边学”是理解新概念的最好方法，这个框架可以帮助人们开始学习。

来源:[https://tenor . com/view/simple-easy-easy-game-easy-life-deal-it-gif-9276124](https://tenor.com/view/simple-easy-easy-game-easy-life-deal-with-it-gif-9276124)

[](https://github.com/grohith327/simplegan) [## grohith327/simplegan

### 简化生成模型训练的框架 SimpleGAN 是一个基于 TensorFlow 的框架，用于对生成模型进行训练

github.com](https://github.com/grohith327/simplegan)  [## SimpleGAN - SimpleGAN v0.2.8 文档

### 是一个使用 TensorFlow 构建的 python 库，旨在通过高级 API 简化生成模型的训练…

simplegan.readthedocs.io](https://simplegan.readthedocs.io/en/latest/index.html) [](https://pypi.org/project/simplegan/) [## 简单根

### 简化生成模型训练的框架 SimpleGAN 是一个基于 TensorFlow 的框架，用于对生成模型进行训练

pypi.org](https://pypi.org/project/simplegan/)