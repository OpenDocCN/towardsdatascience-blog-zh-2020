# 数据科学家:从零到英雄

> 原文：<https://towardsdatascience.com/data-scientist-from-zero-to-hero-6060b639ee98?source=collection_archive---------12----------------------->

![](img/b5c91eed2fabfdba7a4b5e5d068447b9.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## 如何成为一名伟大的数据科学家:从在线资源中学习，了解新概念、最先进的算法和最新的技术。

如何成为一名数据科学家？在本文中，我提供了一个假设的成为数据科学家的学习途径。该指南主要由在线免费资源组成，是为想要学习数据科学的人制作的。如果您已经是数据专家，那么您可以忽略第一部分，直接跳到包含最新资源的部分。和所有的清单一样，这份清单并不完整。如果你有好的建议，请留下评论！

等等！什么是数据科学？放心吧！[维基百科](https://en.wikipedia.org/wiki/Data_science)是你的朋友！简而言之，我们可以将数据科学定义为通过使用算法和工具从数据中提取洞察力的跨学科领域。您可以将数据科学想象为数学、统计学和计算机科学的融合。此外，数据科学还应用于几个领域:哪里有数据，哪里就有数据！

先说学数据科学的步骤吧！

## 1.发展环境

要执行分析，您需要用特定的编程语言编写代码。

> 不要相信不懂编码的数据科学家！

使用最多的语言是 [R](https://www.r-project.org/) 和 [Python](https://www.python.org/) 。需要指出的是，它们并不相互排斥。r 更适合统计分析，而 Python 更适合机器学习。这并不意味着你不能用 R 进行机器学习或者用 Python 进行统计分析。就我个人而言，我主要使用 Python，但有时我们会部署用 r 编写的包，这取决于行业和你的(未来)公司生产环境的标准。我的推荐是两个都学！

两种语言都配备了广泛的软件包:查看 [CRAN 任务视图](https://cran.r-project.org/web/views/)和 [PyPI](https://pypi.org/) 。对于 R 来说，最好的 IDE 是 [RStudio](https://rstudio.com/) ，而对于 Python 来说，你有一些替代方案，比如 [Visual Studio Code](https://code.visualstudio.com/) 、 [PyCharm](https://www.jetbrains.com/pycharm/) 、 [Spyder](https://www.spyder-ide.org/) 或 [Jupyter](https://jupyter.org/) (尽管它并不是一个真正的 IDE)。学习这些语言，谷歌一下“R 入门”或者“Python 入门”就够了。对这些语言的任何合理介绍对我们的目的来说已经很好了！你不需要成为一个专业的程序员，在这个阶段知道一些基础知识就足够了:例如，如何定义一个函数，如何编写一个循环，如何绘制基本图，如何调试，等等。

作为你的操作系统，我的建议是不惜一切代价避开 Windows！如果您还没有安装，安装一个 Linux 发行版(例如， [Ubuntu](https://ubuntu.com/) 、 [Manjaro](https://manjaro.org/) 或 [Fedora](https://getfedora.org/) )并开始使用一个类似 Unix 的操作系统！你会发现有多少事情只能在一个终端内完成。

[](/linux-console-commands-data-scientist-566744ef1fb0) [## 每个数据科学家都应该知道的基本 Linux 控制台命令

### 对数据科学家最有用的终端命令列表:文件管理、系统管理和文本文件…

towardsdatascience.com](/linux-console-commands-data-scientist-566744ef1fb0) 

最后，使用 [git](https://git-scm.com/) 。它是一个广泛应用于数据科学项目的版本控制系统。在 [GitHub](https://github.com/) 或 [Bitbucket](https://bitbucket.org/product) 上打开一个个人资料页面，开始版本化您的代码！

## 2.数据操作和清理

在开始拟合模型之前，你需要对数据操作非常有信心！可以和[熊猫](https://pandas.pydata.org/)(Python 中的)和[dplyr](https://dplyr.tidyverse.org/)(R 中的)一起练习。需要性能？检查 [Dask](https://docs.dask.org/en/latest/dataframe.html) 和[数据表](https://cran.r-project.org/web/packages/data.table/)。

![](img/058d6d3599b05b0d0435397d31fb2fdb.png)

照片由 [Sid Balachandran](https://unsplash.com/@itookthose?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在许多情况下，您将需要编写纯粹的 [SQL](https://en.wikipedia.org/wiki/SQL) 查询来检查数据。因此，了解[主 SQL 语句](https://www.sqltutorial.org/sql-cheat-sheet/)会有所帮助！

请记住，在现实环境中，大量的时间将投入到数据清理中。因此，即使这不是最奇特的任务，它也是数据科学项目取得好成果的必要条件！

## 3.主要机器学习算法综述

准备好开发环境，让我们学习最常用的机器学习算法，以及如何在 R 或 Python 中应用它们。[机器学习 A-Z](https://www.udemy.com/course/machinelearning/) 是一个很好的起点。该课程非常实用，尽管没有深入解释模型，但对于每个模型，您都有 Python 和 r 语言的示例代码。另一个通用在线课程是[机器学习](https://www.coursera.org/browse/data-science/machine-learning)。它比前一个更加理论化，并且提供了 Octave/MatLab 中的实践练习。我的建议是跟着理论视频走，然后尝试用 Python 或者 r 解决习题，当然还有很多其他的在线课程，你可以自由选择适合自己需求的。

另一方面，如果你更喜欢看书，我建议:

*   [百页机器学习书](http://themlbook.com/)
*   [机器学习:理解数据的算法艺术和科学](http://people.cs.bris.ac.uk/~flach/mlbook/)
*   [统计学习的要素:数据挖掘、推理和预测](https://web.stanford.edu/~hastie/Papers/ESLII.pdf)

在这一点上，你应该有信心使用主要的数据科学软件包(例如， [caret](http://topepo.github.io/caret/index.html) 或 [scikit-learn](https://scikit-learn.org/stable/) )。你可以在一些公共数据集上测试你学过的算法: [UCI 数据集](https://archive.ics.uci.edu/ml/datasets.php)、 [Kaggle 数据集](https://www.kaggle.com/datasets)或者直接在[谷歌数据集搜索](https://datasetsearch.research.google.com/)上找一个。

## 4.填补空白！

到现在为止，你已经学会了基础。根据你所受的教育，你可能会错过一些数学、统计学或计算机科学的基本概念。例如，计算机科学学生可能不知道如何进行最大似然估计，或者数学学生可能不知道如何计算算法的复杂性。绝对正常！数据科学是一个非常广泛的领域，不可能什么都知道！

> 无所不知的数据科学家是不存在的！

记住:数据科学不仅仅是关于模型和性能！你可能有世界上最好的想法，但你不知道如何形式化它，你可能能够编写解决每个问题的算法，但该算法可能效率低下，需要太多的资源来运行。了解不同领域的基础知识将有助于您提供更有效的解决方案和可操作的见解。

![](img/6b7a9ab08488ba5ec8c5a313e0856fe6.png)

由[🇸🇮·扬科·菲利](https://unsplash.com/@itfeelslikefilm?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

基础数学课程包括线性代数、实分析和数值分析。基础统计学课程包括描述统计学、推断统计学、贝叶斯统计学。计算机科学的基本概念包括:时间和空间的复杂性，数据结构，排序和搜索算法，图上的算法和算法设计。你可以在任何本科教材中找到所有这些论点。当然，谷歌是你的朋友！

## 5.深度学习

因此，如果你来到这里，你已经知道了一堆建立模型的算法，你有信心用众所周知的软件包开发一个数据科学项目，并且你已经对神经网络如何工作有了一个概念。是时候完全沉浸到[深度学习](https://en.wikipedia.org/wiki/Deep_learning)中了！关于深度学习的常见误解是，它只是一系列可以预测任何事情的神经元层，就像魔法一样。

![](img/90e6b9db75e90ac564094fd96ae6808e.png)

由[朱利叶斯·德罗斯特](https://unsplash.com/@juliusdrost?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

完全错了！关于这个主题有密集的文献:如何训练神经网络，哪种类型的网络在特定的竞赛中起作用，训练一定规模的神经网络所需的硬件架构是什么。首先，你可以从[深度学习专业](https://www.deeplearning.ai/deep-learning-specialization/)开始学习课程，同时作为教科书检查[深度学习](https://www.deeplearningbook.org/)。构建人工神经网络最常用的两个框架是 [TensorFlow](https://www.tensorflow.org/) 和 [PyTorch](https://pytorch.org/) 。网上有很多资料都是从他们的官网开始的。

[](/custom-neural-networks-in-keras-a-street-fighters-guide-to-build-a-graphcnn-e91f6b05f12e) [## Keras 中的自定义神经网络:街头战士构建图表指南

### 如何建立具有自定义结构和层的神经网络:Keras 中的图形卷积神经网络(GCNN)。

towardsdatascience.com](/custom-neural-networks-in-keras-a-street-fighters-guide-to-build-a-graphcnn-e91f6b05f12e) 

如果没有 GPU，可以用 [Google Colab](https://colab.research.google.com/) 做原型！

[](/colab-free-gpu-ssh-visual-studio-code-server-36fe1d3c5243) [## 类固醇上的 Colab:带 SSH 访问和 Visual Studio 代码服务器的免费 GPU 实例

### 一步一步地引导 SSH 进入 Google Colab 提供的免费 GPU 实例，并安装 Visual Studio Code Server。

towardsdatascience.com](/colab-free-gpu-ssh-visual-studio-code-server-36fe1d3c5243) 

## 6.数据可视化

大多数时候，数据科学项目的关键部分是数据可视化。数据科学家应该能够通过有效的图表传达其见解，或者通过仪表板使结果可供企业使用。如果你用的是 Python， [matplotlib](https://matplotlib.org/) ， [plotly](https://plotly.com/dash/) ， [seaborn](https://seaborn.pydata.org/) 和 [streamlit](https://www.streamlit.io/) 是常用的选择。R 中有 [ggplot2](https://ggplot2.tidyverse.org/reference/ggplot.html) 和[闪亮仪表盘](https://rstudio.github.io/shinydashboard/)。如果你喜欢来硬的，可以试试 [D3.js](https://d3js.org/) 。您会发现有几种数据可视化解决方案，并且不可能学会使用所有的解决方案！我的哲学是学习其中的几个，准备好创建一个原型来展示结果。在许多项目中，已经选择了最终的数据可视化解决方案，因为它是现有工作流的一部分，您只需要创建[端点](https://palletsprojects.com/p/flask/)来交付您的模型的输出。

![](img/42aa69799c74a8db88c1a205f0f7bad5.png)

[布拉登·科拉姆](https://unsplash.com/@bradencollum?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 7.卡格尔

如果你到了这里，你就差不多准备好了！您应该已经用可重用的代码、有趣的用例以及应用程序示例填满了您的 GitHub 存储库！在 [Kaggle](https://www.kaggle.com/) 上参加在线竞赛！你可以赢钱！:)最重要的是，在 Kaggle 上你可以向其他人学习各种各样的问题设置！您将意识到，您所知道的只是数据科学社区开发的工具、方法和算法的一小部分。

> 永远不要停止学习！否则，你将被淘汰！

## 8.大数据

我想专门用一节来介绍大数据项目。通常，要在大数据背景下训练机器学习模型，传统数据科学项目的基本包是不够的！

![](img/ff25cb38d835b932200ccf30cb64a49e.png)

[ev](https://unsplash.com/@ev?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

你将面临不同的技术来摄取和操纵大数据。我们正处于数据科学家和数据工程师之间的边界。通常，大数据开发环境是由工程师建立的，但数据科学家应该知道如何使用这些新工具！于是，学会使用[卡夫卡](https://kafka.apache.org/)、 [Spark](https://spark.apache.org/) 、 [Scala](https://www.coursera.org/specializations/scala) 、 [Neo4j](https://neo4j.com/) ！

[](/kafka-docker-python-408baf0e1088) [## Apache Kafka: Docker 容器和 Python 中的例子

### 如何使用 Docker 安装 Kafka 并在 Python 中产生/消费消息

towardsdatascience.com](/kafka-docker-python-408baf0e1088) [](/neo4j-cypher-python-7a919a372be7) [## 如何从 Python 查询 Neo4j

### 在 Python 中使用 Neo4j:Neo4j Python 驱动程序和 Cypher 查询语言的快速入门。

towardsdatascience.com](/neo4j-cypher-python-7a919a372be7) 

## 9.云服务

近年来，一个日益增长的趋势是由[谷歌](https://cloud.google.com/products/ai)、[微软](https://azure.microsoft.com/en-us/services/machine-learning/)和其他参与者提供的数据科学项目开发的在线平台。这些平台主要提供开发环境和已经训练好的模型作为服务。同样，这些环境的设置主要由数据工程师完成，但了解某些类型的服务已经在线存在，以及哪些类型的预培训模型可以作为服务使用，可以提高您的工作效率并缩短交付时间！因此，请关注这些云平台，因为许多公司已经在使用它们了！

## 10.真实世界的体验

希望当你读到这一部分时，你已经是一名数据科学家了。通过在真实环境中工作，你会发现技术知识并不是数据科学项目的唯一部分。软技能和经验是完成一个成功项目的主要因素！作为一名数据科学家，您将与业务部门互动，将业务需求转化为技术需求，您将与其他专业人员(例如，数据工程师、仪表板设计师等)一起在团队中工作。)并能够以技术和非技术的方式分享您的发现。此外，正如你已经意识到的，对于每个问题设置，测试方法的数量总是比你完成项目的时间要多。

> 培养一种第六感的最佳方式是“尝试、失败、重复”，这种第六感能告诉我们什么可行，什么不可行。

只有在经历了相当多的成功和失败之后，你才能避免第一眼看到的非最优解！

## 11.专门化

到目前为止，你一直在学习成为一名普通的数据科学家。你什么都懂一点，但你不是任何事情的专家！当你在这里学习所有的东西时，其他成千上万的人也在做同样的事情！所以如果你想和其他人不一样，你需要专攻某样东西！你可能是一名 [NLP](https://en.wikipedia.org/wiki/Natural_language_processing) 专家，一名 [CV](https://en.wikipedia.org/wiki/Computer_vision#:~:text=Computer%20vision%20is%20an%20interdisciplinary,human%20visual%20system%20can%20do.) 专家，一名网络科学专家，或者一名数据可视化专家……这个名单可能会很长，但是请记住:专业不应该仅仅是技术方面的，也应该是商业领域的！很容易找到一个时间序列预测方面的数据科学家专家，但如果数据科学家也是股票市场方面的专家，就不那么容易了。

## 12.概念和工具

作为成为伟大的数据科学家指南的最后一点，我想指出概念和工具之间的区别。这些概念在你学会之后仍然存在，但是工具在将来可能会改变。神经网络如何工作是一个概念，但它的实现可以在 Tensorflow、PyTorch 或任何其他框架中完成。数据科学家应该对创新持开放态度。如果一门新的语言更适合你的项目，那就学习它，并使用它！不要依赖于你的工具！尝试新的东西！

![](img/2057fb765e7ecb0ab65003b2d3c0f39c.png)

照片由[尼克·费因斯](https://unsplash.com/@jannerboy62?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

到目前为止，我们已经看到了成为伟大的数据科学家的学习途径。这个清单远非完整，它是基于我的个人经验。如果你有任何建议，请给我留言！当然，这个列表的顺序只是推荐，你可以根据自己的需要随意调整！

在文章的这一部分，我将分享一些保持与新概念、最先进的算法和最新技术保持同步的方法。这些建议也可以在你学习文章的第一部分时使用！

## A.加入当地社区，参加研讨会

了解当地是否有数据科学家社区，并加入他们！参加 meetup，认识不同行业的人。不要低估网络的力量！在其他情况下成功应用的解决方案也可以适用于你的问题！

![](img/a26431e7aa9f7adf7131d75f2c3174ad.png)

照片由[海伦娜·洛佩斯](https://unsplash.com/@wildlittlethingsphoto?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

如果住在大学附近，说不定也有专门研究数据科学的研究部门！通常，研究中心会组织研讨会和会议。要求参加这些活动！这是一个很好的方式来保持最新的趋势研究课题和最先进的解决方案！

## B.看报纸

[谷歌学术](https://scholar.google.com/)提供了在某个特定研究者的新论文发表时被通知的可能性。dblp 和 arXiv 是搜索新论文的其他来源。它们都是继续你的专业或找到特定问题的解决方案的好方法！如果你需要一个程序来组织你阅读的论文，使用门德利的推荐系统，它会根据你的兴趣推荐相关的论文。

## C.关注在线网站

[走向数据科学](https://towardsdatascience.com/)、 [Analytics Vidhya](https://medium.com/analytics-vidhya) 、和[走向 AI](https://medium.com/towards-artificial-intelligence) 都是有效的[媒介](https://medium.com/)出版物。大部分文章都是带动手例子的教程！所以你已经有了代码的草稿！

除了媒体出版物，来自[谷歌](https://ai.googleblog.com/)、[脸书](https://ai.facebook.com/blog/)、微软和 [OpenAI](https://openai.com/blog/) 的研究博客是机器学习/人工智能领域的另一个新闻来源。

其他有趣的网站有:

*   [机器学习掌握度](https://machinelearningmastery.com/blog/)
*   [KDnuggets](https://www.kdnuggets.com/news/index.html)
*   [证件代码](https://paperswithcode.com/)

## D.订阅时事通讯

不想经常查看网页？你更喜欢电子邮件通知吗？订阅以下一些时事通讯！

*   [批量](https://www.deeplearning.ai/thebatch/)
*   [真正周刊](https://aiweekly.substack.com/)
*   [深度学习周刊](https://www.deeplearningweekly.com/)
*   [序列](https://thesequence.substack.com/subscribe)
*   [迪派](https://deepai.org/)
*   [同步](https://syncedreview.com/author/syncedreview/)

## E.在 Telegram 上关注数据科学小组

有专门研究数据科学的[电报](https://telegram.org/)频道/群组。我在这里报告其中一些:

*   [ODS . ai 的数据科学](https://t.me/opendatascience)
*   [人工智能、Python、认知神经科学](https://t.me/ai_python_en)
*   [团队数据科学聊天](https://t.me/teamdatascience)

## F.使用社交网络和 YouTube

你可以在 Twitter 或 LinkedIn 上关注并与许多专家数据科学家或机器学习巨星互动(只需寻找他们！).在 LinkedIn 上，即使你不想换工作，你也可以关注机器学习公司的招聘信息:从需求中，你可以了解生产环境中使用的技术的趋势！此外，YouTube 频道也可以有趣地跟上潮流！我在这里报告一些渠道:

*   [脸书艾](https://www.youtube.com/channel/UC5qxlwEKM7-5YZudb24l0bg)
*   [微软研究院](https://www.youtube.com/user/MicrosoftResearch)
*   [数据科学教程](https://www.youtube.com/channel/UCk5tiFqPvdjsl7yT4mmokmg)
*   [ai 工程](https://www.youtube.com/c/AIEngineeringLife)
*   [张量流](https://www.youtube.com/c/TensorFlow)
*   [计算机械协会](https://www.youtube.com/user/TheOfficialACM)
*   [365 数据科学](https://www.youtube.com/c/365DataScience)
*   莱克斯·弗里德曼
*   [安德烈亚斯·克雷茨](https://www.youtube.com/c/andreaskayy)
*   [数据学校](https://www.youtube.com/c/dataschool)
*   [send ex](https://www.youtube.com/c/sentdex/)

## G.个人项目

在许多数据科学项目中，可用的时间很短，但要做的事情很多。在这种情况下，数据科学家倾向于抑制某些方法或忽略可能的改进。创造次优产品的感觉可能会令人沮丧！

![](img/13a7574db1b847190d95d47cba3e6957.png)

照片由[屋大维丹](https://unsplash.com/@octadan?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

作为这个列表的最后一点，我建议开始一个你喜欢的个人项目。在这个项目中慢慢来，不要设置任何期限。通过测试每一种新方法来享受这个项目的每一部分！

联系人:[LinkedIn](https://www.linkedin.com/in/shuyiyang/)|[Twitter](https://twitter.com/deltarule)