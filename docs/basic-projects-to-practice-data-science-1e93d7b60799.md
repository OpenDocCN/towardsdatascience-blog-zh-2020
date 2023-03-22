# 实践数据科学的基础项目

> 原文：<https://towardsdatascience.com/basic-projects-to-practice-data-science-1e93d7b60799?source=collection_archive---------41----------------------->

## 从这 6 个基础数据科学项目开始您的数据科学之旅

![](img/58714463efdf77b86bbe77b88a86db6e.png)

[斯科特·格雷厄姆](https://unsplash.com/@sctgrhm?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

数据科学是目前最令人兴奋的领域之一，对专家的需求正在增长。网上有很多数据科学课程。学习数据科学的困难在于，它需要大量的实践才能适应现实生活中的数据科学项目。

在过去的几个月里，我一直在学习数据科学并探索这个领域。值得一提的是，我不是数据科学家(我的主要领域是 Web 开发)，但我喜欢所有编程的东西，我想尝试一下，并找到更多关于各种数据科学技术和算法的信息。

我想知道数据科学是否真的像人们所说的那样令人兴奋和强大。我将在另一篇文章中回答我对它的看法，但现在，我想与您分享我参与的六个项目，如果您是数据科学新手或想尝试一下，它们将帮助您扩展您的数据科学知识。如果你是数据科学的新手，或者只是想多探索一下这个领域，那么下面的项目将非常适合你。

在这 6 个项目中，您将发现在从事数据科学项目时可能面临的最常见问题。从数据清理，通过标准化和规范化、降维、特征工程到回归、计算机视觉、自然语言处理(NLP)到神经网络，使用流行的 Python 库，如 Pandas、Numpy、scikit-learn、Tensorflow、Keras、TextBlob 等。完成此列表中的所有项目后，您将拥有流行的数据科学技术和算法的实践经验。

如果您从未从事过数据科学项目，这也是一些介绍性的文章，它们将帮助您设置您的计算机，为您提供从事这些项目所必需的一切，并向您展示如何使用 Git 和 Github，以便您可以在那里存储您的项目。

**你需要知道的事情**

对 Python 的基本理解和知识会很有用。在这些文章中，我没有涉及 Python 编程语言。如果您不了解 Python，我建议您在开始使用这些项目之前，至少要熟悉基本的 Python。例如，你可以在 [Coursera](https://www.coursera.org) 上完成[人人编程(Python 入门)](https://www.coursera.org/learn/python)课程。

一旦你掌握了 Python 的基础知识，你就可以开始从事这些项目了。先前的数据科学知识是有帮助的，但不是必需的。所有项目都包含对项目中使用的所有算法、概念和 Python 数据科学库的解释。我会解释代码和项目的每一步，所以你可以理解什么和为什么你必须为每个项目做。

虽然用于所有项目的 Jupyter 笔记本也可以在 Github 上获得，并且欢迎您使用它们，但是我建议您自己编写代码，不要复制/粘贴或使用 Jupyter 笔记本。这样你会学到更多，记住更多的信息。

# 简介和设置

## **(1)** [**如何设置你的计算机进行数据科学**](https://medium.com/@pjarz/how-to-set-up-your-computer-for-data-science-9b880118ead)

在开始从事数据科学项目之前，您需要在计算机上设置一些东西。幸运的是，有一些免费的开源工具可以让这个过程变得非常简单。

## **(2)**[**Git 和 GitHub 简介**](https://medium.com/@pjarz/introduction-to-git-and-github-57ac9059d429)

在本教程中，我将解释一些基本步骤，这些步骤将使您能够创建您的 [GitHub](https://github.com) 资源库，将您的本地文件添加并提交到 Git，并将它们推送到 GitHub 上的在线资源库。

# 基础数据科学项目

## **(1)** [**分析医药销售数据**](/analysing-pharmaceutical-sales-data-in-python-6ce74da818ab)

在本项目的第一部分，您将学习如何将数据集从文件加载到 Pandas (Python 数据操作和分析库),以及如何执行统计分析并在 Pandas 数据框中查找特定信息。在本项目的第二部分，您将学习使用回归(一种能够发现自变量和因变量之间关系的技术)根据历史销售数据预测未来销售。您将使用三种不同的回归算法:线性回归、多项式回归和支持向量回归(SVR)。在训练您的回归模型之前，您将缩放数据并将其分为训练数据和测试数据，这两者都是非常常见和重要的数据科学技术。缩放将实现更好的模型性能，由于分割数据，我们可以在不同的数据集上训练我们的模型，然后计算模型的准确性分数，以查看它在另一组数据上的表现。因为您使用不同的回归模型，所以您也可以使用 VotingRegressor 来获得更好的结果。VotingRegressor 是一种集合方法，可拟合多个回归变量，并对各个预测进行平均，以形成最终预测。您将使用一个流行的 Matplotlib 库来可视化数据和回归预测。熟悉和练习 Pandas 数据操作和 Matplotlib 可视化非常重要，因为它们在许多数据科学项目中非常常见，用于操作数据和可视化结果。回归也是许多数据科学项目中非常常见和有用的技术。

以下是项目资源:

**中篇:**[https://towards data science . com/analyzing-pharmaceutical-sales-data-in-python-6 ce 74 da 818 ab](/analysing-pharmaceutical-sales-data-in-python-6ce74da818ab)

**GitHub 上的项目:**[https://GitHub . com/pj online/Basic-Data-Science-Projects/tree/master/1-analyzing-Pharmaceutical-Sales-Data](https://github.com/pjonline/Basic-Data-Science-Projects/tree/master/1-Analysing-Pharmaceutical-Sales-Data)

## **(2)** [**利用数据科学和机器学习预测泰坦尼克号幸存者**](/data-science-titanic-challenge-solution-dd9437683dcf)

在本项目中，您将使用泰坦尼克号幸存者的数据集来构建一个模型，根据乘客的性别、年龄、乘客等级等特征来预测泰坦尼克号灾难中的幸存者和死者。将数据从文件加载到 Pandas 数据框后，您将执行探索性数据分析。探索性数据分析使我们能够了解我们的数据集中有哪些特征，它们是如何分布的，以及我们的数据集中是否有任何缺失值。更好地理解数据将有助于我们进行数据预处理和特征工程。在预处理阶段，您将清理数据并填充任何缺失的值。您还将从现有要素中提取一些新要素(通过使用数据宁滨等技术)，并移除不需要且对模型性能没有影响的要素。为了训练模型，您将使用两个新的分类器模型:KNeighborsClassifier 和 DecisionTreeClassifier。然后，您将比较这些模型的性能。在这个项目中，你还将学习 K-Fold 交叉验证技术。这种技术有助于更好地使用数据，减少偏差，并让我们更好地了解模型的性能。

以下是项目资源:

**中篇:**[https://towards data science . com/data-science-titanic-challenge-solution-DD 9437683 DCF](/data-science-titanic-challenge-solution-dd9437683dcf)

**GitHub 上的项目:**[https://GitHub . com/pj online/Basic-Data-Science-Projects/tree/master/5-Titanic-Challenge](https://github.com/pjonline/Basic-Data-Science-Projects/tree/master/5-Titanic-Challenge)

## **(3)** [**计算机视觉导论与 MNIST**](https://medium.com/swlh/introduction-to-computer-vision-with-mnist-2d31c6f4d9a6)

在这个项目中，你将开始学习两个非常重要的数据科学概念；计算机视觉和神经网络。MNIST 是一个手写数字的数字数据库。您将构建并训练一个神经网络来识别手写的数字图像。您将使用 Keras，它是专门针对神经网络的 Python 库。您将看到不同类型的神经网络层激活功能以及神经网络的其他功能和配置。您还将学习如何在文件中保存和加载您的训练模型。您还将使用 Keras 函数*to _ categorial*，该函数将整数转换为二进制类矩阵，从而提高神经网络的性能。在本练习中，您将学习如何使用 Keras 创建、训练和使用简单有效的神经网络，并评估其性能。

以下是项目资源:

**Medium 文章:**[https://Medium . com/swlh/introduction-to-computer-vision-with-mnist-2d 31 c 6 f 4d 9 a 6](https://medium.com/swlh/introduction-to-computer-vision-with-mnist-2d31c6f4d9a6)

**GitHub 上的项目:**[https://GitHub . com/pj online/Basic-Data-Science-Projects/tree/master/3-Introduction-to-Computer-Vision-with-MNIST](https://github.com/pjonline/Basic-Data-Science-Projects/tree/master/3-Introduction-to-Computer-Vision-with-MNIST)

## **(4)** [**利用 Tensorflow 神经网络识别猫和狗**](https://medium.com/swlh/recognising-cats-and-dogs-using-neural-networks-with-tensorflow-6f366ad30dbf)

在这个项目中，你将继续使用计算机视觉和神经网络，并使用 Keras 和 Tensorflow 构建一个稍微复杂一点的网络。你在这个项目中的任务是建立、训练和测试一个神经网络，该网络将对猫和狗的图片进行识别和分类。你会有三组猫狗的图像:训练、测试、有效；训练对神经网络模型进行训练，在训练期间对模型进行有效性验证，然后测试对训练好的模型进行测试。每组包含不同的猫和狗的图像。

以下是项目资源:

**Medium 文章:**[https://Medium . com/swlh/recogning-cats-and-dogs-using-neural-networks-with-tensor flow-6f 366 ad 30 DBF](https://medium.com/swlh/recognising-cats-and-dogs-using-neural-networks-with-tensorflow-6f366ad30dbf)

**GitHub 上的项目:**[https://GitHub . com/pj online/Basic-Data-Science-Projects/tree/master/9-Cats-and-Dogs](https://github.com/pjonline/Basic-Data-Science-Projects/tree/master/9-Cats-and-Dogs)

## **(5)**[**Python 中的图像人脸识别**](https://medium.com/an-idea/image-face-recognition-in-python-30b6b815f105)

在这个项目中，你将处理一个不同的但也很常见和有趣的计算机视觉问题，即人脸识别。您将使用[*face _ recognition*Python 库](https://github.com/ageitgey/face_recognition)进行人脸识别，使用 [Python 图像库(PIL)](https://pypi.org/project/Pillow/) 进行图像处理。你不仅可以在测试图像上识别出已知的人脸，还可以用 PIL 在图像上标记人脸。

以下是项目资源:

**Medium 文章:**[https://Medium . com/an-idea/image-face-recognition-in-python-30 b6b 815 f 105](https://medium.com/an-idea/image-face-recognition-in-python-30b6b815f105)

**GitHub 上的项目:**[https://GitHub . com/pj online/Basic-Data-Science-Projects/tree/master/4-Face-Recognition](https://github.com/pjonline/Basic-Data-Science-Projects/tree/master/4-Face-Recognition)

## **(6)**[**Python 中的 Twitter 情绪分析**](/twitter-sentiment-analysis-in-python-1bafebe0b566)

在这个项目中，您将了解数据科学的另一个重要概念，即*自然语言处理(NLP)* 。使用 Python NLP 库 [TextBlob](https://textblob.readthedocs.io/en/dev/) ，您将对一个选定的 Twitter 帐户最近的一些推文进行情感分析。要访问 tweets，您首先要设置 [Twitter 开发者 API](https://developer.twitter.com/en) 账户。然后，您将创建一个虚拟环境并安装项目所需的库。您将使用 [Tweepy](https://www.tweepy.org) Python 库向 Twitter 开发者 API 认证并下载 tweets。然后，您将清理 tweets 并执行一些基本的 NLP。你将计算每条推文的主观性和极性，并将每条记录标记为正面或负面。然后，您将计算该帐户的积极推文的百分比，并在图表上显示推文的类别。最后，你将生成一个单词云，以查看你正在分析的推文中使用的主题和最常用的单词。

以下是项目资源:

**中篇:**[https://towards data science . com/Twitter-perspection-analysis-in-python-1 bafebe 0 b 566](/twitter-sentiment-analysis-in-python-1bafebe0b566)

**GitHub 上的项目:**[https://GitHub . com/pj online/Basic-Data-Science-Projects/tree/master/8-Twitter-情操-分析](https://github.com/pjonline/Basic-Data-Science-Projects/tree/master/8-Twitter-Sentiment-Analysis)

**总而言之，您将学习和实践以下数据科学技术、算法和概念:**

*   熊猫
*   Matplotlib
*   Python 图像库(PIL)
*   数据预处理
*   特征工程
*   特征缩放
*   训练/测试数据分割
*   宁滨数据
*   统计数字
*   投票回归变量
*   线性回归
*   逻辑回归
*   多项式回归
*   支持向量回归
*   k 近邻分类器(KNN)
*   决策树分类器
*   带 Keras 的神经网络
*   人脸识别(计算机视觉)
*   主成分分析
*   k 倍交叉验证
*   使用 accuracy_score 指标进行性能验证
*   十二岁
*   WordCloud
*   文本二进制大对象
*   词干分析
*   标记化(计数矢量器)

我希望这个基本数据科学项目列表是有用的，它将帮助您了解更多信息并练习您的数据科学技能。

编码快乐！

还没有订阅媒体？考虑[报名](https://pjwebdev.medium.com/membership)成为中等会员。每月只需 5 美元，你就可以无限制地阅读媒体上的所有报道。[订阅媒介](https://pjwebdev.medium.com/membership)支持我和媒介上的其他作者。