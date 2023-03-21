# 我应该在什么时候使用哪个 Python 数据科学包？

> 原文：<https://towardsdatascience.com/which-python-data-science-package-should-i-use-when-e98c701364c?source=collection_archive---------15----------------------->

## Python 数据科学图书馆的地图

Python 是数据科学最流行的语言。不幸的是，很难知道何时使用众多数据科学库中的哪一个。☹️

了解何时使用哪个库是快速上手的关键。在本文中，我将向您介绍重要的 Python 数据科学库。😀

![](img/ad9509229712128a88779a6bc1dc7b21.png)

资料来源:pixabay.com

你将看到的每个软件包都是免费的开源软件。👍感谢所有创建、支持和维护这些项目的人们！🎉如果你有兴趣了解如何为开源项目贡献补丁，[这里有一个很好的指南](https://www.dataschool.io/how-to-contribute-on-github/)。如果你对支持这些项目的基金会感兴趣，我在这里写了一个概述[。](/the-unsung-heroes-of-modern-software-development-561fc4cb6850)

![](img/5a535ecc882d319dc5a4f156a5b281c1.png)

我们开始吧！🚀

# [熊猫](https://pandas.pydata.org/)

Pandas 是一个帮助你理解和操作数据的工具。使用 pandas 来操作表格数据(比如电子表格)。Pandas 非常适合数据清理、描述性统计和基本的可视化。

![](img/f129914e4fde4d49fbb5dbac843f3297.png)

熊猫相对来说是对大脑友好的，尽管 API 是巨大的。如果你想从 API 最重要的部分开始，可以看看我关于熊猫的书。

与 SQL 数据库不同，pandas 将所有数据存储在内存中。这有点像微软 Excel 和 SQL 数据库的混合体。Pandas 使大量数据的操作快速且可重复。

您机器上的内存容量限制了 pandas 可以处理的行数和列数。作为一个粗略的指南，如果你的数据少于几千列和几亿行，pandas 应该在大多数计算机上运行良好。

像一只真正的熊猫，熊猫是温暖和毛茸茸的。🐼

![](img/b5473c58122b77d8af189d8b1fabd040.png)

资料来源:pixabay.com

当你的数据超过熊猫的处理能力时，你可能会想使用 NumPy。

# [NumPy](https://numpy.org/)

NumPy ndarrays 就像更强大的 Python 列表。它们是构建机器学习大厦的数据结构。它们拥有你需要的多维数据。

![](img/b2ec5411a94728c4c87f324577d81600.png)

你有每个像素三个颜色通道的视频数据和很多帧吗？没问题。😀

NumPy 没有像 pandas 那样方便的方法来处理时间序列数据和字符串。事实上，每个 NumPy ndarray 只能有一种数据类型(感谢[凯文·马卡姆](https://medium.com/u/a9e4103439ec?source=post_page-----e98c701364c--------------------------------)建议我包含这个区分符)。

对于表格数据，NumPy 也比熊猫更难处理。您不能像在 pandas 中那样在表格中轻松显示列名。

NumPy 的速度/内存效率比 pandas 高一点，因为它没有额外的开销。然而，如果您拥有真正的大数据，还有其他方法可能会更好地扩展。我有一个关于这个话题的大纲，所以让我知道你是否有兴趣在[推特](https://twitter.com/discdiver)上听到它。👍

NumPy 还有什么用？

*   n 数组的数学函数。
*   ndarrays 的基本统计函数。
*   从普通分布产生随机变量。NumPy 有 27 个分布可以随机抽样。

NumPy 就像没有便利函数和列名的熊猫，但是有一些速度增益。🚀

# [Scikit-learn](https://scikit-learn.org/stable/)

scikit-learn 库是机器学习的瑞士军刀。如果你在做一个不涉及深度学习的预测任务，scikit-learn 就是你要用的。它可以毫无问题地处理 NumPy 数组和 pandas 数据结构。

![](img/d8874cabec464f085cc454f29c664455.png)

Scikit-learn 管道和模型选择函数非常适合准备和操作数据，可以避免无意中偷看您的保留(测试集)数据。

scikit-learn API 对于预处理转换器和估算器非常一致。这使得搜索超过许多机器学习算法的最佳结果相对容易。这让你更容易理解图书馆。🧠

Scikit-learn 支持多线程，因此您可以加快搜索速度。然而，它不是为 GPU 构建的，所以它不能利用那里的加速。

Scikit-learn 还包含方便的基本 NLP 函数。

![](img/dbe9a429ebaf545d9839662f66a6693a.png)

资料来源:pixabay.com

如果你想做机器学习，熟悉 scikit-learn 是必不可少的。

接下来的两个库主要用于深度神经网络。它们与 GPU、TPU 和 CPU 配合得很好。

# [张量流](https://www.tensorflow.org/)

TensorFlow 是最受欢迎的深度学习库。在工业上尤其常见。它是由谷歌开发的。

![](img/472c3cc8542ddce121732d4a1951dcc7.png)

从版本 2.0 开始，Keras 高级 API 现在与 TensorFlow 紧密集成。

除了在 CPU 芯片上工作，TensorFlow 还可以使用 GPU 和 TPU。这些矩阵代数优化的芯片为深度学习提供了巨大的加速。

# [PyTorch](https://pytorch.org/)

PyTorch 是第二受欢迎的深度学习库，现在是学术研究中最常见的。它是由脸书开发的，并且越来越受欢迎。你可以在这里看到我关于[的文章。](/is-pytorch-catching-tensorflow-ca88f9128304)

![](img/6b723baec433e8de7797362291cfc29f.png)

PyTorch 和 TensorFlow 现在提供了非常相似的功能。它们都有数据结构，叫做*张量*，类似于 NumPy ndarrays。张量和可以很容易地转换成 n 数组。这两个软件包都包含一些基本的统计函数。

PyTorch 的 API 通常被认为比 TensorFlow 的 API 更加 pythonic 化。

[Skorch](https://github.com/skorch-dev/skorch) 、 [FastAI](https://github.com/fastai) 和[PyTorch lighting](https://github.com/PyTorchLightning/pytorch-lightning)是减少使用 py torch 模型所需代码量的包。 [PyTorch/XLA](https://github.com/pytorch/xla) 允许您将 PyTorch 与 TPU 一起使用。

PyTorch 和 TensorFlow 都可以让你做出顶尖的深度学习模型。👍

# [统计模型](https://github.com/statsmodels/statsmodels)

Statsmodels 是统计建模库。这是做推理频率统计的地方。

![](img/0f801b9df2eb85b87afff41ad5aaa0c3.png)

想要运行统计测试并获得一些 p 值吗？Statsmodels 是您的工具。🙂

来自 [R](https://www.r-project.org/about.html) 的统计学家和科学家可以使用 [statsmodels formula API](https://www.statsmodels.org/devel/example_formulas.html) 平稳过渡到 Python 领域。

除了 t 检验、ANOVA 和线性回归等常见的统计测试之外，statsmodels 还有什么好处？

*   测试您的数据与已知分布的匹配程度。
*   用 ARIMA、霍尔特-温特斯和其他算法做时间序列建模。

谈到线性回归等常见公式时，Scikit-learn 与 statsmodels 有一些重叠。但是，API 是不同的。此外，scikit-learn 更侧重于预测，而 statsmodels 更侧重于推理。⚠️

Statsmodels 建立在 NumPy 和 SciPy 的基础上，与熊猫玩得很好。🙂

说到 SciPy，我们来看看什么时候用。

# [SciPy](https://docs.scipy.org/doc/scipy/reference/) 🔬

" SciPy 是建立在 Python 的 NumPy 扩展上的数学算法和便利函数的集合."— [文件](https://docs.scipy.org/doc/scipy/reference/tutorial/general.html)。

![](img/387a4c4b6ecf2855430469b23cb7cac9.png)

SciPy 就像 NumPy 的双胞胎。许多 NumPy 数组函数也可以通过 SciPy 调用。这两个软件包甚至共享同一个【T2 文档】网站。

SciPy 稀疏矩阵用于 scikit-learn。一个[稀疏矩阵](https://en.wikipedia.org/wiki/Sparse_matrix)是一个被优化的矩阵，当大部分元素为零时，它比一个常规的密集矩阵使用更少的内存。

SciPy 包含通用常量和线性代数功能。[*scipy . stats*](https://docs.scipy.org/doc/scipy/reference/stats.html)*子模块用于概率分布、描述性统计和统计测试。它有 125 个分布可以随机抽样，比 NumPy 多了将近 100 个。😲然而，除非您正在进行大量的统计，否则作为一名实践数据科学家，您可能会对 NumPy 中的发行版感到满意。*

*如果 statsmodels 或 NumPy 没有您需要的功能，那么就去 SciPy 中查找。👀*

# *[Dask](https://dask.org/)*

*Dask 有一个模仿熊猫和 NumPy 的 API。当您想使用 pandas 或 NumPy，但数据量超过内存容量时，请使用 Dask。*

*![](img/f4521da783b44943b238aa25d8b48a27.png)*

*Dask 还可以加快大数据集的计算速度。它在多个设备上执行多线程。您可以将 Dask 与[**Rapids**](https://rapids.ai/dask.html)**结合使用，以获得 GPU 上分布式计算的性能优势。👍***

# ***PyMC3***

***PyMC3 是贝叶斯统计软件包。比如马尔可夫链蒙特卡罗(MCMC)模拟？PyMC3 是你的果酱。🎉***

***![](img/ded2e7efdeb321720f9024a58c77ca1d.png)***

***我发现 PyMC3 API 有点令人困惑，但它是一个强大的库。但是那可能是因为我不经常使用它。***

# ***其他流行的数据科学包***

***我不会深入研究可视化、NLP、梯度增强、时间序列或模型服务库，但我会重点介绍每个领域中一些流行的包。***

## ***可视化库📊***

***Python 中有大量可视化库。 [**Matplotlib**](https://matplotlib.org/) 、 [**seaborn**](https://seaborn.pydata.org/) 和 [**Plotly**](https://plotly.com/python/) 是最受欢迎的三款。在这篇文章的最后，我浏览了一些选项。***

## ***NLP 库🔠***

***自然语言处理(NLP)是机器学习的一个巨大而重要的领域。无论是 [**空间**](https://spacy.io/) 还是 [**NLTK**](https://www.nltk.org/) 都将拥有你需要的大部分功能。两者都很受欢迎。***

## ***梯度推进回归树库🌳***

***[**LightGBM**](https://lightgbm.readthedocs.io/en/latest/) 是最流行的渐变升压包。Scikit-learn 已经克隆了它的算法。 [**XGBoost**](https://github.com/dmlc/xgboost) 和 [**CatBoost**](https://catboost.ai/) 是其他类似 LightGBM 的 boosting 算法包。如果你看看 Kaggle 机器学习竞赛的排行榜，深度学习算法不是解决问题的好方法，你可能会看到获胜者使用的这些梯度提升库之一。***

## ***时间序列库📅***

***[**Pmdarima**](https://alkaline-ml.com/pmdarima/index.html#) 让拟合 arima 时间序列模型不那么痛苦。它包装了 statsmodels 算法。然而，选择超参数的过程不是完全自动化的。***

***[**Sktime**](https://github.com/alan-turing-institute/sktime) 目标是做一个“时间序列机器学习的统一 Python 库”。它包装了 pmdarima、statsmodels Holt-Winters 和其他库。这是最前沿的，但很有希望。此时，它仍然需要知道它所包装的底层库的 API。***增加了 2020 年 12 月 14 日的 sktime 部分* * ****

***[**Prophet**](https://facebook.github.io/prophet/) 是另一个用时间序列数据做预测的包。“Prophet 是一种基于加法模型预测时间序列数据的程序，其中非线性趋势符合每年、每周和每天的季节性，加上假日影响。它最适用于具有强烈季节效应的时间序列和几个季节的历史数据。——[文件](http://Prophet)。它是由脸书创造的。API 和文档都是用户友好的。如果你的数据符合上面的描述，那就值得一试。***

## ***模型服务图书馆🚀***

***说到模型服务， [**Flask**](https://flask.palletsprojects.com/en/1.1.x/) 、 [**FastAPI**](https://fastapi.tiangolo.com/) 和 [**Streamlit**](https://www.streamlit.io/) 是三个流行的库，它们实际上对您的模型预测做了一些事情。😉Flask 是一个基本的框架，用于制作一个 API 或者服务一个已经过实战检验的网站。FastAPI 使得设置 REST 端点变得更快更容易。Streamlit 可以在单页应用程序中快速提供模型。如果你有兴趣了解更多关于 streamlit 的知识，我在这里写了一个入门指南。***

***![](img/12bb350467dc4ba2777a254f435a8152.png)***

***资料来源:pixabay.com***

# ***包装***

***以下是何时使用哪个主要 Python 数据科学库的快速回顾:***

*   *****熊猫**进行表格数据的探索和操作。***
*   *****NumPy** 用于普通分布的随机样本，以节省内存或加快运算速度。***
*   *****scikit-learn** 用于机器学习。***
*   *****深度学习的 TensorFlow** 或 **PyTorch** 。***
*   *****statsmodels** 用于统计建模。***
*   *****SciPy** 用于在 NumPy 或 statsmodels 中找不到的统计测试或分布。***
*   *****Dask** 当你想要熊猫或者 NumPy 但是拥有真正的大数据的时候。***
*   *****PyMC3** 为贝叶斯统计。***

***我希望您喜欢这个关键的 Python 数据科学包之旅。如果你有，请在你最喜欢的社交媒体上分享，这样其他人也可以找到它。😀***

***现在，您有望对不同的 Python 数据科学库之间的相互关系以及它们之间的联系有一个更清晰的心理模型。***

***我写关于 [Python](https://memorablepython.com) 、 [SQL](https://memorablesql.com) 、 [Docker](https://memorabledocker.com) 和其他技术主题的文章。如果你对这些感兴趣，请注册我的[邮件列表，那里有很棒的数据科学资源](https://dataawesome.com)，点击阅读更多内容，帮助你提高技能[。👍](https://medium.com/@jeffhale)***

***[![](img/ba32af1aa267917812a85c401d1f7d29.png)](https://dataawesome.com)******![](img/fc1135106be85da22ab501d47d2ecd51.png)***

***资料来源:pixabay.com***

***探索愉快！😀***