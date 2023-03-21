# 数据科学家需要知道的 15 个 Python 库

> 原文：<https://towardsdatascience.com/15-python-libraries-that-a-data-scientist-need-to-know-df0b3029ecd9?source=collection_archive---------1----------------------->

![](img/50c314644529239dbfecec7c844c3c58.png)

由 [Pexels](https://pixabay.com/users/Pexels-2286921/) 在 [Pixabay](https://pixabay.com/photos/books-students-library-university-1281581/) 拍摄的照片

## 数据收集、清理、转换、可视化、建模、音频/图像识别和网络相关

如果你是一个数据科学家、数据分析师或者仅仅是一个爱好者，你不应该错过一些非常流行和有用的 Python 库。

在本文中，将列出总共 15 个 Python 库，并对其进行简要介绍。我相信他们中的大多数你可能已经熟悉了，但如果没有，强烈建议你自己去看看。

这些库将分为几个类别，它们是

*   数据采集
*   数据清理和转换
*   数据可视化
*   数据建模
*   音频和图像识别
*   网

# 数据采集

![](img/ede48a6e4d3ead259cff9d9bb9a1cbe7.png)

照片由 [geralt](https://pixabay.com/users/geralt-9301/) 在 [Pixabay](https://pixabay.com/illustrations/arrows-centering-direction-central-1738067/) 上拍摄

大多数数据分析项目从数据收集和提取开始。有时，当你为某个公司工作以解决一个现存的问题时，数据集可能会被给出。但是，数据可能不是现成的，您可能需要自己收集。最常见的情况是，您需要从互联网上抓取数据。

## 1.Scrapy

 [## Scrapy |一个快速而强大的抓取和网络爬行框架

### 编辑描述

scrapy.org](https://scrapy.org/) 

当你想写一个 Python 爬虫从网站提取信息时，Scrapy 可能是最流行的 Python 库。例如，您可以使用它来提取一个城市中所有餐馆的所有评论，或者收集电子商务网站上某一类产品的所有评论。

典型的用法是根据 URL 模式和 XPath 模式来识别出现在 web 页面上的有趣信息的模式。一旦发现了这些模式，Scrapy 就可以帮助你自动提取所有需要的信息，并把它们组织成表格和 JSON 这样的数据结构。

您可以使用`pip`轻松安装 Scrapy

```
pip install scrapy
```

## 2.美味的汤

[](https://www.crummy.com/software/BeautifulSoup/#Download) [## 美味的汤

### 下载|文档|名人堂|企业版|来源|变更日志|讨论组|杂志]您没有…

www.crummy.com](https://www.crummy.com/software/BeautifulSoup/#Download) 

Beautiful Soup 是另一个用于抓取 Web 内容的 Python 库。人们普遍认为，与 Scrapy 相比，它的学习曲线相对较短。

此外，对于相对较小规模的问题和/或只是一次性的工作，美丽的汤将是更好的选择。不像 Scrapy，你必须开发自己的“蜘蛛”,然后回到命令行运行它，Beautiful Soup 允许你导入它的功能并在线使用它们。因此，你甚至可以在你的笔记本上使用它。

## 3.硒

 [## Selenium 客户端驱动程序— Selenium 3.14 文档

### Selenium WebDriver 的 Python 语言绑定。selenium 包用于从…自动化 web 浏览器交互

www .硒. dev](https://www.selenium.dev/selenium/docs/api/py/index.html) 

最初，Selenium 被开发为一个自动化的 Web 测试框架。但是开发者发现，把它当 Web scraper 用还是挺方便的。

Selenium 通常在与网页交互后需要获取有趣的数据时使用。比如你可能需要注册一个账号，然后登录，点击一些按钮和链接后获取内容，而这些链接被定义为 JavaScript 函数。在这些情况下，通常情况下，不容易使用刺痒或美丽的汤来实现，但硒可以。

然而，重要的是要注意到，硒将比正常的刮库慢得多。这是因为它实际上初始化了一个 web 浏览器，如 Chrome，然后模拟代码中定义的所有操作。

因此，在处理 URL 模式和 XPaths 时，一定要使用 Scrapy 或 Beautiful Soup。万不得已才选择硒。

# 数据清理和转换

![](img/e6adf8f22e2a30008da48d5f3050cd59.png)

照片由 [Sztrapacska74](https://pixabay.com/users/Sztrapacska74-8968314/) 在 [Pixabay](https://pixabay.com/photos/kitten-cat-4274170/) 上拍摄

我想没有必要去宣称数据清理和转换在数据分析和数据科学中有多么重要。还有，有太多优秀的 Python 库很好地做到了这些。我将挑选其中一些，作为数据科学家或分析师，您一定知道它们。

## 4.熊猫

[](https://pandas.pydata.org/) [## 熊猫

### pandas 是一个快速、强大、灵活且易于使用的开源数据分析和操作工具，构建于…

pandas.pydata.org](https://pandas.pydata.org/) 

我几乎可以肯定，把熊猫列入这个名单是没有必要的。只要是和数据打交道的，肯定都用过熊猫。

使用 Pandas，您可以操作 Pandas 数据框中的数据。有大量的内置函数可以帮助您转换数据。

不需要太多的文字。如果你想学习 Python，这是一个必须学习的库。

## 5.Numpy

[](https://numpy.org/) [## 数字——数字

### NumPy 是使用 Python 进行科学计算的基础包。它包含了其他的东西:一个强大的…

numpy.org](https://numpy.org/) 

类似地，Numpy 是 Python 语言用户的另一个必须学习的库，甚至不仅仅是数据科学家和分析师。

它将 Python 列表对象扩展为全面的多维数组。还有大量的内置数学函数来支持您在计算方面的几乎所有需求。通常，您可以使用 Numpy 数组作为矩阵，Numpy 将允许您执行矩阵计算。

我相信许多数据科学家将如下开始他们的 Python 脚本

```
import numpy as np
import pandas as pd
```

因此，可以肯定的是，这两个库可能是 Python 社区中最受欢迎的库。

## 6.空间

[](https://spacy.io/) [## Python 中的 spaCy 工业级自然语言处理

### spaCy 旨在帮助您做真正的工作——构建真正的产品，或收集真正的见解。图书馆尊重你的…

空间. io](https://spacy.io/) 

Spacy 大概没有前几部出名。Numpy 和 Pandas 是处理数字和结构化数据的库，而 Spacy 帮助我们将自由文本转换成结构化数据。

Spacy 是 Python 最流行的 NLP(自然语言处理)库之一。想象一下，当你从一个电子商务网站上搜集了大量产品评论时，你必须从这些免费文本中提取有用的信息，然后才能对它们进行分析。Spacy 有许多内置特性可以提供帮助，比如工作标记器、命名实体识别和词性检测。

此外，Spacy 支持许多不同的人类语言。在其官方网站上，声称支持 55 个以上。

# 数据可视化

![](img/47a94c41a11fdb6dfe04912a11c272e0.png)

在 [Pixabay](https://pixabay.com/photos/financial-analytics-blur-business-2860753/) 上由 [6689062](https://pixabay.com/users/6689062-6689062/) 拍摄的照片

数据可视化绝对是数据分析的基本需求。我们需要将结果和结果可视化，并讲述我们发现的数据故事。

## 7.Matplotlib

[](https://matplotlib.org/) [## Matplotlib: Python 绘图— Matplotlib 3.2.1 文档

### Matplotlib 是一个全面的库，用于在 Python 中创建静态、动画和交互式可视化…

matplotlib.org](https://matplotlib.org/) 

Matplotlib 是最全面的 Python 数据可视化库。有人说 Matplotlib 丑。然而，在我看来，作为 Python 中最基本的可视化库，Matplotlib 为实现你的可视化想法提供了最大的可能性。这就像 JavaScript 开发人员可能更喜欢不同种类的可视化库，但当有很多定制的功能不被那些高级库支持时，D3.js 就必须参与进来。

我写过另一篇文章介绍 Matplotlib。如果你想了解更多，可以看看这个。

[](https://levelup.gitconnected.com/an-introduction-of-python-matplotlib-with-40-basic-examples-5174383a6889) [## Python Matplotlib 介绍，包含 40 个基本示例

### Matplotlib 是 Python 中最流行的库之一。在本文中，提供了 40 个基本示例供您…

levelup.gitconnected.com](https://levelup.gitconnected.com/an-introduction-of-python-matplotlib-with-40-basic-examples-5174383a6889) 

## 8.Plotly

[](https://plotly.com/python/) [## Plotly Python 图形库

### Plotly 的 Python 图形库制作出交互式的、出版物质量的图形。如何制作线图的示例…

plotly.com](https://plotly.com/python/) 

老实说，虽然我相信 Matplotlib 是可视化的必学库，但大多数时候我更喜欢使用 Plotly，因为它使我们能够用最少的代码行创建最精美的图形。

无论您想要构建 3D 表面图、基于地图的散点图还是交互式动画图，Plotly 都能在短时间内满足要求。

它还提供了一个图表工作室，您可以将您的可视化上传到一个支持进一步编辑和持久化的在线存储库。

# 数据建模

![](img/bb2f612c246af0cba8a802d44d77f0b0.png)

由[菲洛娜](https://pixabay.com/users/FeeLoona-694250/)在 [Pixabay](https://pixabay.com/photos/child-tower-building-blocks-blocks-1864718/) 上拍摄的照片

当数据分析涉及建模时，我们通常称之为高级分析。如今，机器学习已经不是一个新奇的概念。Python 也被认为是最受欢迎的机器学习语言。当然，有很多优秀的库支持这一点。

## 9.Scikit 学习

[](https://scikit-learn.org/) [## sci kit-学习

### “我们使用 scikit-learn 来支持前沿基础研究[…]”“我认为这是我见过的设计最完美的 ML 软件包……

scikit-learn.org](https://scikit-learn.org/) 

在您深入“深度学习”之前，Scikit Learn 应该是您开始机器学习之路的 Python 库。

Scikit Learn 有 6 个主要模块

*   数据预处理
*   维度缩减
*   回归
*   分类
*   使聚集
*   型号选择

我敢肯定，一个钉钉了 Scikit Learn 的数据科学家，应该已经算是一个好的数据科学家了。

## 10.PyTorch

[](https://pytorch.org/) [## PyTorch

### 开源深度学习平台，提供从研究原型到生产部署的无缝路径。

pytorch.org](https://pytorch.org/) 

PyTorch 由脸书创作，作为 Python 的交互机器学习框架被开源。

与 Tensorflow 相比，PyTorch 在语法方面更“pythonic 化”。这也使得 PyTorch 更容易学习和使用。

最后，作为深度学习焦点库，PyTorch 拥有非常丰富的 API 和内置函数，可以辅助数据科学家快速训练他们的深度学习模型。

## 11.张量流

[](https://www.tensorflow.org/) [## 张量流

### 一个面向所有人的端到端开源机器学习平台。探索 TensorFlow 灵活的工具生态系统…

www.tensorflow.org](https://www.tensorflow.org/) 

Tensorflow 是另一个由 Google 开源的 Python 机器学习库。

Tensorflow 最受欢迎的功能之一是 Tensorboard 上的数据流图表。后者是一个自动生成的基于 Web 的仪表板，可视化机器学习流程和结果，这对调试和演示非常有帮助。

# 音频和图像识别

![](img/a7c868f14ce15f040f1e5a8dae053922.png)

由 [Pixabay](https://pixabay.com/photos/audio-technology-mixer-volume-knob-1839162/) 上的[像素](https://pixabay.com/users/Pexels-2286921/)拍摄的照片

机器学习不仅针对数字，还可以帮助处理音频和图像(视频被视为一系列图像帧)。因此，当我们处理这些多媒体数据时，那些机器学习库将是不够的。下面是一些流行的 Python 音频和图像识别库。

## 12.利布罗萨

 [## LibROSA — librosa 0.7.2 文档

### LibROSA 是一个用于音乐和音频分析的 python 包。它提供了创作音乐所必需的构件…

librosa.github.io](https://librosa.github.io/librosa/) 

Librosa 是一个非常强大的音频和语音处理 Python 库。它可以用来从音频片段中提取各种类型的特征，如节奏、节拍和速度。

使用 Librosa，那些极其复杂的算法，如拉普拉斯分段，可以很容易地在几行代码中实现。

## 13.OpenCV

[](https://opencv.org/) [## OpenCV

### 开放计算机视觉库

opencv.org](https://opencv.org/) 

OpenCV 是应用最广泛的图像和视频识别库。毫不夸张地说，OpenCV 使 Python 在图像和视频识别方面取代了 Matlab。

它提供了各种 API，不仅支持 Python，还支持 Java 和 Matlab，以及出色的性能，在业界和学术研究中都赢得了广泛的赞誉。

# 网

![](img/135f93e80b633a1ec5d58a3510538e35.png)

照片由[200 度](https://pixabay.com/users/200degrees-2051452/)在 [Pixabay](https://pixabay.com/vectors/website-page-template-internet-web-1624028/) 拍摄

不要忘记，Python 在数据科学领域流行起来之前，通常用于 Web 开发。所以，也有很多优秀的 web 开发库。

## 14.姜戈

[](https://www.djangoproject.com/) [## 姜戈

### Django 是一个高级 Python Web 框架，它鼓励快速开发和干净、实用的设计。建造者…

www.djangoproject.com](https://www.djangoproject.com/) 

如果你想用 Python 开发一个 Web 服务后端，Django 永远是最好的选择。它被设计成一个高级框架，可以用很少的几行代码构建一个网站。

它直接支持大多数流行的数据库，以节省您设置连接和数据模型开发的时间。您只需关注业务逻辑，而不必担心 Django 的凝乳操作，因为它是一个数据库驱动的框架。

## 15.瓶

[](https://flask.palletsprojects.com/) [## 欢迎使用 Flask — Flask 文档(1.1.x)

### 欢迎阅读 Flask 的文档。开始安装，然后了解快速入门概述。有…

flask.palletsprojects.com](https://flask.palletsprojects.com/) 

Flask 是 Python 中的一个轻量级 Web 开发框架。最有价值的特点是，它可以很容易地定制任何特定的要求非常容易和灵活。

由于 Flask 的轻量级特性，许多其他著名的 Python 库和提供 Web UI 的工具都是使用 Flask 构建的，如 Plotly Dash 和 Airflow。

# 结论

![](img/6c31027906dcfeadbed266186a6704f7.png)

凯利·西克玛在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

事实上，有更多著名的 Python 库有资格在此列出。看到 Python 社区欣欣向荣总是令人兴奋的。如果有更多的库成为数据科学家和分析师的必知库之一，可能有必要在另一篇文章中组织它们。

[](https://medium.com/@qiuyujx/membership) [## 通过我的推荐链接加入 Medium 克里斯托弗·陶

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

medium.com](https://medium.com/@qiuyujx/membership) 

**如果你觉得我的文章有帮助，请考虑加入灵媒会员来支持我和成千上万的其他作家！(点击上面的链接)**