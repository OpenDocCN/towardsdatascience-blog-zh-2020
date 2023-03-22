# 个性化您的 Python 通知声音与谐音

> 原文：<https://towardsdatascience.com/personalized-your-python-notification-sounds-with-chime-d04e803d7615?source=collection_archive---------37----------------------->

## 创建一个听觉通知系统来通知你正在运行的代码

![](img/b267c25a2601bcf4adbd83b38035ab5b.png)

瑞安·昆塔尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

我很确定当你在写代码的时候；有些时候你平行做。有一段时间，许多项目正在进行，你需要运行一个很长的脚本，你需要等待。

以前，我写过如何在你完成运行机器学习模型或任何脚本时通过电子邮件获得通知。

[](/automatic-notification-to-email-with-python-810fd357d89c) [## 使用 Python 自动通知电子邮件

### 使用 Python 自动化您的通知系统

towardsdatascience.com](/automatic-notification-to-email-with-python-810fd357d89c) 

虽然，有时你根本没有检查电子邮件，或者只是因为太忙于你的笔记本电脑，但当建模或编码脚本完成时，你真的需要通知。

在这种情况下，我之前发现了一个有趣的开源软件，由 [Max Halford](https://medium.com/u/aff4365c3dba?source=post_page-----d04e803d7615--------------------------------) 开发，名为 [Chime](https://github.com/MaxHalford/chime) ，当你运行完所有代码时(或者当你遇到错误时)，它会用声音通知你**。**

让我们试试这个！

# 和谐

首先，让我们安装所需的软件包。chime 包不依赖于任何依赖项，所以它会破坏您现有的任何包。

```
pip install chime
```

当你完成了软件包的安装，我们就可以开始使用提示音软件包得到通知了。

让我们从测试 chime 的基本功能开始。

```
#importing chime package
import chime#Successfully running the code sounds
chime.success()
```

当您运行上面的代码时，应该会有一个通知声音响起。这是一种特定的声音，当您成功运行代码或任何进程时会发出这种声音。

我们还可以通过运行以下代码来测试错误声音。

```
chime.error()
```

声音应该与成功的声音不同。

请注意，声音是在异步进程中播放的，因此是非阻塞的。无论声音长度如何，每个声音在编码处理后都需要大约 2 毫秒才能发出。

你可以尝试测试的另一个功能是警告功能和信息功能，它们也会发出不同的声音。

```
#Warning notification sound
chime.warning()#Information notification sound
chime.info()
```

# **与 Jupyter 整合的谐音**

好吧，你可以在你的脚本中包含谐音函数来通知你觉得需要通知的每个进程。尽管如此，如果你需要在每个 Jupyter 笔记本单元格中通知，我们需要做一些准备。

首先，我们需要通过运行下面的代码来加载扩展。

```
%load_ext chime
```

之后，我们准备在运行的每个 Jupyter 单元中获得声音通知。让我们尝试一个简单的方法，在 Jupyter 单元格中包装一行代码。

```
#Wrapping a single line in jupyter cell
%chime print("Sounds out")
```

如果您想要包装整个 Jupyter 单元，您需要以不同的方式运行它，就像下面的代码一样。

```
%%chime

print("Whole cell")
```

当您成功运行单元时，它将产生来自`chime.success()`的声音，否则它将使用`chime.error()`声音。

# 改变声音主题

您可以通过设置可用的钟声主题来更改钟声通知声音主题。要检查哪个主题可用，您可以运行以下代码。

```
chime.themes()
```

目前只有三个主题可用；《钟声》、《马里奥》和《塞尔达》。要更改主题，只需运行以下代码。

```
#Change the notification sound with Zelda theme
chime.theme('zelda')
```

这将把主题声音改为塞尔达通知主题声音。如果你想每次都有不同的声音，你可以把主题改为“随机”。

如果你想有自己的声音通知，你需要提出一个新的主题，通过[打开一个 pull 请求](https://github.com/creme-ml/creme/issues/new)来添加必要的。wav 文件到`[themes](https://github.com/MaxHalford/chime/tree/main/themes)` [目录下](https://github.com/MaxHalford/chime/tree/main/themes)。对于一个主题，它们由四个文件组成:`success.wav`、`warning.wav`、`error.wav`和`info.wav`。

# **结论**

Chime 是一个开源包，当您成功运行代码时(或者出现错误时)，它会发出声音通知您。

当你想添加自己声音的新主题时，需要先拉请求，提出自己的声音。

希望有帮助！

# 如果您喜欢我的内容，并希望获得更多关于数据或数据科学家日常生活的深入知识，请考虑在此订阅我的[简讯。](https://cornellius.substack.com/welcome)

> 如果您没有订阅为中等会员，请考虑通过[我的推荐](https://cornelliusyudhawijaya.medium.com/membership)订阅。