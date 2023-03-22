# 我的 TensorFlow 认证开发者之旅

> 原文：<https://towardsdatascience.com/my-journey-to-tensorflow-certified-developer-a8bac8091567?source=collection_archive---------56----------------------->

## 这篇帖子是关于我如何通过 TensorFlow 开发者证书考试的。此外，它是关于我的机器学习之旅和我对机器学习驱动的软件开发的看法。

![](img/d487f5258f20cd879669011ddef4dd2a.png)

作者:安德烈·巴拉诺夫斯基

# 关于我

我是一个企业软件开发人员，正在跳上机器学习的列车。在过去的 15 年里，我一直在做独立的甲骨文咨询，主要与甲骨文开发者工具和 Java 相关。我喜欢与社区分享我的知识，从 2006 年到 2020 年，我发布了 1030 个带有示例代码的博客条目，其中大部分都与 Oracle 技术相关。我的博客在 Oracle 社区中很有名，我是 Oracle 开创性的大使和大会发言人(从 2007 年到 2018 年，我每年都在旧金山的 Oracle Open World 和 Oracle Code One 大会上发言)。当我在 2004 年攻读硕士学位时，我正在研究支持向量机和神经网络。然而，我转到了企业领域，开始使用 Oracle 工具。那是我在甲骨文工作的一段美好时光，作为一名独立顾问，我周游世界，结识了许多优秀的人，并在加拿大、美国、南非、香港、沙特阿拉伯、俄罗斯和欧洲国家工作过。2017 年末，我决定改变我的工作方式，开始开发自己的产品 Katana ML(带机器学习的业务自动化)。从那以后，我决定特别关注开源和 TensorFlow。获得 [TensorFlow 开发者证书](https://www.tensorflow.org/certificate)有助于更接近我从传统编程转向成为机器学习开发者的目标。

# 为什么是机器学习？

我为什么决定投身机器学习？我相信机器学习是未来的编程模式。这是我的主要原因。当然，它不会取代所有传统的编程用例，但它开启了一种全新的构建软件的方式。请看 Laurence Moroney 的精彩解释图，摘自他的 Youtube 视频— [机器学习基础:Ep #1 —什么是 ML？](https://www.youtube.com/watch?v=_Z9TRANg4c0&list=PLOU2XLYxmsII9mzQ-Xxug4l2o04JBrkLV):

![](img/6961f522fb1877b6ba30a506fdbf3bd6.png)

来源:[机器学习基础:Ep # 1——什么是 ML？](https://www.youtube.com/watch?v=_Z9TRANg4c0&list=PLOU2XLYxmsII9mzQ-Xxug4l2o04JBrkLV)

这张图表总结了传统编程和机器学习之间的主要区别。完美的例子——用传统的编程方法，你可以计算一个人何时在走路、跑步或骑自行车(使用速度)。但是怎么算一个人是不是在打高尔夫呢？这就是机器学习优势发挥作用的地方——它可以处理这些复杂的情况，否则根本不可能编写代码。这个例子来自 Laurence Moroney 视频— [机器学习基础:Ep #1 —什么是 ML？](https://www.youtube.com/watch?v=_Z9TRANg4c0&list=PLOU2XLYxmsII9mzQ-Xxug4l2o04JBrkLV):

![](img/77f4a96af25aefd200712b0cf4de17f4.png)

来源:[机器学习基础:Ep # 1——什么是 ML？](https://www.youtube.com/watch?v=_Z9TRANg4c0&list=PLOU2XLYxmsII9mzQ-Xxug4l2o04JBrkLV)

# 我是如何准备考试的

你的第一站应该是 TensorFlow 认证项目[网站](https://www.tensorflow.org/certificate)。在那里你会找到[考生手册](https://www.tensorflow.org/site-assets/downloads/marketing/cert/TF_Certificate_Candidate_Handbook.pdf)。这份文件列出了通过考试所需的所有技能。这个列表很长，但是不要害怕——这是可行的。

我不是在为备考做微观规划，我不太喜欢使用各种规划应用程序。我更喜欢遵循我脑海中的高层次计划，相当方便:)

我的高层次计划是，我严格遵循该计划(我应该提到，我已经在 TensorFlow 工作了 2 年)来成功通过考试:

1.  阅读弗朗索瓦·乔莱的一本书——《用 Python 进行深度学习》(也有第二版，将于今年晚些时候完成，现在以 MEAP 的名字出版)
2.  阅读 Aurélien Géron 的书—
    [使用 Scikit-Learn、Keras 和 TensorFlow 进行机器学习，第二版](https://learning.oreilly.com/library/view/hands-on-machine-learning/9781492032632/)
3.  在 Coursera 上学习劳伦斯·莫罗尼教授的实践专业化课程中的[张量流](https://www.coursera.org/specializations/tensorflow-in-practice)
4.  在 Coursera 上学习劳伦斯·莫罗尼教授的 [TensorFlow:数据和部署专业化](https://www.coursera.org/specializations/tensorflow-data-and-deployment)课程
5.  浏览谷歌的[机器学习速成班](https://developers.google.com/machine-learning/crash-course)

[证书](https://www.tensorflow.org/certificate)网站强烈推荐第 3 项。如果你没有时间阅读第一和第二本书，我建议你也仔细阅读第四和第五项。你应该仔细阅读 Coursera 上的资料。不要跳，跳过任何笔记本。不要复制粘贴代码，从头开始学习自己实现用例。这在考试的时候会有帮助。

仅仅通过考试并不是唯一的目标。我正在仔细学习，并花了相当多的时间来提高我的张量流技能，这是我参加考试的主要目的(非常好的学习动机)。

在准备考试的时候用 PyCharm 编写代码。如果你对 PyCharm 很有信心，这在考试中肯定会有帮助。PyCharm 提供了 API 自动完成功能，这是一件大事——它将加速您的编码并减少错误。

在我有信心参加考试之前，我花了大约 6 周的时间集中学习。我没有全职学习，我也有工作。我帮忙照看我 9 个月大的儿子，你可能知道——有时候会很忙:)

# 考试

完成准备后，我没有马上去考试。我计划在接下来的三四天里参加一场考试。我是故意这样做的，把知识沉淀在脑子里，带着一个新鲜的头脑去考试。

在考试期间，我在使用 Python 或 PyCharm IDE 时没有遇到任何问题。我在我的 MacBook Pro CPU(2.4 GHz 8 核英特尔酷睿 i9)和 64 GB RAM 上运行训练，没有使用 Colab GPU。

你有五个任务，在考试中建立五个模型。考试时间为 5 小时。当您通过用于 TensorFlow 认证的 PyCharm 插件启动考试时，时间计数器会立即启动。任务从最简单到最复杂排序。你的作业会被相应地评分，最简单的任务得分较少。

**任务 1** :我很快实现了，提交了——但是评分结果并不完美。我调整了一些调整(我不会深入细节，因为这是不公平的)，重新提交保存的模型，并获得了最高分

**任务二**:我被这个任务卡住了。它应该不太复杂，但我在模型训练中不断出错。在某个时刻，我想——好吧，也许我考试会不及格。但后来我设法找到了一个变通办法，训练了模型，并从评分员那里得到了最高分。我看了看时间计数器，还剩 3.5 小时。这意味着我在前两项任务上花了 1.5 小时

任务 3 :这个任务对我来说比较顺利，我重拾了信心——我仍然有机会通过考试。然而，模型训练的准确率并没有像我希望的那样快速提高。在这个任务上花了 30 分钟后，我决定切换到任务 4 和 5。这时，还剩 3 个小时。我在任务 4 和任务 5 上花了 1.5 小时，然后回到任务 3。在这个任务上工作了 30 多分钟后，我设法找到了解决方案，此时模型训练的准确性开始迅速提高。然而，我仍然没有从评分员那里得到最高分。我意识到训练需要运行更长时间，以获得更好的模型准确性。还剩 55 分钟，我将模型配置为训练 40 分钟(在这一点上我是在冒险，如果训练因任何原因失败，我将没有机会再次运行它)。幸运的是，培训按时完成，我还有 10 分钟提交模型。这次我取得了好成绩，提交了我的作品

**任务 4** :在 45 分钟内完成该任务，实现模型，并获得评分员的最高分

**任务 5** :与任务 4 类似，45 分钟完成模型，提交给评分员，获得最高分

下一刻，我收到了一封邮件，正文是——“恭喜你，你通过了 TensorFlow 开发者证书考试！”

# 摘要

我是一名软件开发人员，决定进入机器学习领域。如果你像我一样——一个想投身机器学习的软件开发人员，希望这篇文章能给你动力。花时间学习，努力工作——会有回报的。

![](img/0b908616319a9989d0ac4b68db9cd37e.png)

我的 [TensorFlow 开发者证书](https://www.credential.net/8d8715e1-f0eb-457f-ab61-1068aadb8441)