# Scikit-learn、TensorFlow、PyTorch、Keras……但是从哪里开始呢？

> 原文：<https://towardsdatascience.com/scikit-learn-tensorflow-pytorch-keras-but-where-to-begin-9b499e2547d0?source=collection_archive---------8----------------------->

## 关于机器学习任务可用内容的综合初学者指南。

![](img/4af7d310d67d6e5623cbdc0e82d6d4cb.png)

Javier Allegue Barros 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

如果你是机器学习的新手，这意味着你已经被这个不可思议的研究领域及其无限的视角所诱惑，*祝贺你，欢迎你！*

现在，你可能想预测下一任美国总统会是谁，用人工智能驱动的交易工具赚钱，或者只是在图片上检测猫和狗。幸运的是，有大量的在线资源可以帮助你实现这些目标。

存在许多机器学习(ML)和深度学习(DL)框架，但在本文中，我将只考虑使用 Python 的四个最常见的框架，即 [Scikit-learn](https://scikit-learn.org/stable/index.html) 、 [TensorFlow](https://www.tensorflow.org/) 、 [Keras](https://keras.io/) 和 [PyTorch](https://pytorch.org/) 。其中一些服务于不同的目的，一些比其他的更有用，这取决于你的目标和你的个人投资，一些对于结果的可解释性更好，等等。让我们一起回顾一下有助于回答这个重要问题的要点:

> 作为初学者，应该用哪个 ML 框架？

# 四个主要框架的简介

*   **TensorFlow** (TF)是谷歌的一个端到端的机器学习框架，允许你执行极其广泛的下游任务。有了 [TF2.0](https://blog.tensorflow.org/2019/09/tensorflow-20-is-now-available.html) 和更新的版本，给游戏带来了更多的效率和便利。
*   Keras 建立在 TensorFlow 之上，这使得它成为深度学习目的的包装器。这是难以置信的用户友好和容易拿起。一个坚实的资产是它的神经网络块模块化，以及它是用 Python 编写的这一事实，这使得它易于调试。
*   **PyTorch** 是 TensorFlow 的直接竞争对手，由脸书开发，广泛应用于研究项目。它允许几乎无限制的定制，非常适合在 GPU 上运行张量运算(实际上 TensorFlow 也是如此)。
*   Scikit-learn 是另一个用户友好的框架，它包含大量有用的工具:分类、回归和聚类模型，以及预处理、维度缩减和评估工具。

每个框架都是开源的，当然其他框架也是可用的，其中一些你可以在本文中了解[。](https://medium.com/edureka/top-10-machine-learning-frameworks-72459e902ebb)

# 你的目的是什么？

![](img/a24a152b006ece5454ebcb95d0905727.png)

由[马克·弗莱彻-布朗](https://unsplash.com/@boab?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

随着你对 ML 的了解越来越多，你会经常阅读本书*，但是**你最终用来执行任务的框架和模型应该取决于任务的性质**。如果你想预测人们对电影评论的看法，那么使用 Keras 或 PyTorch 的深度学习方法是有意义的，如果你想预测未来 NBA 比赛门票的价格，那么 scikit-learn 处理结构化数据的能力就是你所需要的。*

*如果你正在进行学术研究，并且想要深入复杂的建模，那么 TF 和 PyTorch 就是为你准备的，去做吧！相比之下，使用 Keras 和 scikit-learn 可以在 5 分钟内完成编码，2 分钟内完成训练，10 分钟内完成部署。这就引出了我的下一个观点:*

# *从简单开始！*

*![](img/e0bf541c522a918d046300e556a6523f.png)*

*mostafa meraji 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片*

*当我在大学课程中第一次开始学习 ML 和 AI 时，作业使用 PyTorch，我们被要求编写正则化函数，这在当时看起来非常复杂，让我自然而然地转向更简单的东西来构建神经网络:Keras。*

*关键是，并不是所有的框架都同样容易掌握。例如，您可以使用 scikit-learn 用大约三行代码构建和训练一个简单的模型:*

```
***from** **sklearn.svm** **import** SVC
model = SVC()
model.fit(X, y)*
```

*这不会给你最好的结果，但是从简单开始是熟练学习曲线的关键。然后，您将利用您的基础知识，探索其他模型，调整参数，并可能继续进行更复杂和更具挑战性的工作。*

*相比之下，从实现端到端 TensorFlow 或 PyTorch 模型开始可能会有点棘手，一开始会让人不知所措。*

# *不要都学！*

*![](img/9d2aee9ac9ecb03394e5651cee69eb4e.png)*

*兰迪·雷伯恩在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片*

*这一点似乎是显而易见的，但是相信我，对于初学者来说，认为他们需要彻底检查每个框架的源代码才能被聘为 ML 工程师或数据科学家是相当普遍的。**让过多的信息充斥你的大脑可能会阻碍你解决问题的能力**，这不是我们想要的，对吗？*

*一种更有趣的方式是学习基本概念，这将帮助你更好地理解为了解决你的问题，你应该熟悉框架的哪些特定部分。例如，您将在某个时候使用 **scikit-learn 的度量**函数来评估您的模型的性能，但是这并不意味着您应该记住所有可用的度量函数才能做得更好。*

# *基本要点*

*   *探索并**权衡你的选择**在承诺深入学习机器学习框架之前，你可能会对其他可用的东西感到惊讶。*
*   *不要担心不能马上取得惊人的艺术效果。**了解你的学习曲线**，享受发现的乐趣。*
*   ***绝不半吊子两件事。全屁股一个东西**。我保证，自信地使用任何一个提到的框架都是一笔宝贵的财富，没有必要了解所有的事情来解决问题。*

**感谢您的阅读，我希望这对您有所帮助，并且我成功地将您引向了正确的方向！**