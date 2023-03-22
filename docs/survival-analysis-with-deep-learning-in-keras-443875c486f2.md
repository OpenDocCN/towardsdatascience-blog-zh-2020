# Keras 中基于深度学习的生存分析

> 原文：<https://towardsdatascience.com/survival-analysis-with-deep-learning-in-keras-443875c486f2?source=collection_archive---------19----------------------->

## 应用生存方法估计概率密度函数

![](img/290d0395754910939fde43862ddf22ae.png)

叶夫根尼·切尔卡斯基在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

生存分析是*统计学的一个特殊分支，用于分析一个或多个事件发生前的预期持续时间。*它也被称为**‘事件时间’分析**，因为目标是估计一些感兴趣的**事件**发生的**时间**。它在机器学习中的应用没有限制:在寿命预期、客户保持、预测性维护等问题中应用这些技术是很常见的。

乍一看，生存函数估计似乎是一项艰苦的工作，但经过一些经验和基本的数学概念，它不是那么令人望而却步！最好的起点是头脑中有清晰的想法，我们需要知道什么是我们的**时间**和**事件**。只有这样，我们才能继续开发一个模型，为感兴趣的实体提供可靠的生存函数预测。通过一些技巧，我们可以利用生存函数的映射属性，并利用它的其他形式的派生(如危险函数)。

我已经在之前的[文章](/survival-analysis-with-lightgbm-plus-poisson-regression-6b3cc897af82)中澄清了这些直觉，在那里我也使用梯度推进方法(比如 LGBM)来估计一个不常见场景中的生存函数。在这篇文章中，我试图遵循同样的目标:估计密度函数，但采用神经网络。

# 数据

我们使用的[数据集](https://www.kaggle.com/marklvl/bike-sharing-dataset)包含华盛顿特区首都自行车共享系统从 2011 年到 2012 年的两年历史数据，以每日和每小时的格式提供。提供了各种预测因子来生成预报，如时间或天气回归因子。现在我们考虑小时格式，因为我们需要大量数据来拟合我们的神经网络模型。

![](img/f9b18c393c2497578903545e84c65646.png)

我们的范围是预测临时用户在某种时间和天气条件下注册了多少辆每小时的自行车租赁。

乍一看，这个任务可以像简单的回归问题一样轻松地执行。我们试图让事情变得更辣，采取一种生存的方法。我们必须估计每小时临时用户数量的累积密度函数(CDF)。在这个场景中，我们的**时间**变量由到目前为止注册的临时用户的数量表示，而**事件**日期被定义为小时结束时的用户总数。**我们的模型将学习注册少于一定数量的用户的概率。**

![](img/a74fef1ddef20f0557dd53d357928a00.png)

累积分布函数

为了处理这个问题，维持生存方法、线性或增强方法(如前[后](/survival-analysis-with-lightgbm-plus-poisson-regression-6b3cc897af82)中所做的)需要尊重一些基本假设，以便提供生存函数的正确估计。这也包括，例如，对数据进行一点处理，或者对我们的目标应用一个简单的映射。对于神经网络，我们不关心这个，我们只需要选择正确的结构，并给它提供大量的数据。

一般来说，我们在构建自己的生存深度学习模型时，可以做出两种不同的选择:用回归或者分类来处理问题。这些方法能够估计我们期望的生存函数，提供不同的损失最小化过程，包括以两种不同的形式处理我们的目标。

![](img/44ae872de5f105a0bd867dfc31165bb0.png)

与同一样本相关的目标示例，用于回归和分类

# 回归神经网络

用神经网络和回归估计生存函数意味着直接估计每小时的 CDF。当临时用户的数量小于真实计数时，CDF 为 0，而在该值之后为 1。为了使我们的目标密度函数具有相同的长度，我们将可达用户的最大数量固定为 400。在我们使用的网络结构之下。它将我们的外部预测值(天气和时间特征)作为输入，试图复制我们的密度函数作为输出。最后一层中的 sigmoid 激活有助于将最终值限制在 0 和 1 之间。计算训练以最小化均方误差损失。

```
def get_model_reg():

    opt = Adam(lr=0.01)

    inp = Input(shape=(len(columns)))
    x = Dense(512, activation='relu')(inp)
    x = Dropout(0.3)(x)
    x = Dense(256, activation='relu')(x)
    x = Dropout(0.2)(x)
    x = Dense(256, activation='relu')(x)
    out = Dense(max_count, activation='sigmoid')(x)

    model = Model(inp, out)
    model.compile(optimizer=opt, loss='mse')

    return model
```

我们直接根据我们的模型估计的整个概率函数来计算我们的模型的优良性。为此，我们使用连续排名概率得分(CRPS)作为性能指标。

![](img/60c1038b26eb2b0e14800f800c57a24f.png)

CRPS 对我们任务的提法

我们预测每个小时样本的完整生存曲线，最终，我们在我们的测试数据上达到 0.074 CRPS，这优于基于测试生存函数的虚拟估计的简单模型，其中 CDF 来自于训练(0.121 CRPS)。

![](img/5d4fe39e373248a194773e89a8d93077.png)

使用回归进行每小时生存函数预测的示例

# 分类神经网络

使用分类方法，我们将目标作为一次性实例来处理。我们的目标，长度等于可达用户的最大数量，对应于注册的精确计数等于 1，否则等于 0。我们的模型经过训练，可以预测到目前为止临时用户的准确数量。为此，我们的神经网络试图最小化分类交叉熵，并具有最终的 softmax 激活函数。

```
def get_model_clas():

    opt = Adam(lr=0.0001)

    inp = Input(shape=(len(columns)))
    x = Dense(512, activation='relu')(inp)
    x = Dropout(0.3)(x)
    x = Dense(256, activation='relu')(x)
    x = Dropout(0.2)(x)
    x = Dense(256, activation='relu')(x)
    out = Dense(max_count, activation='softmax')(x)

    model = Model(inp, out)
    model.compile(optimizer=opt, loss='categorical_crossentropy')

    return model
```

在预测期间，为了获得我们想要的生存函数，我们必须简单地对输出进行后处理，对每个预测应用累积和变换。利用我们估计的生存函数，我们可以像以前一样计算我们在测试集上的性能。利用分类方法，我们达到 0.091 CRPS，这比基于测试生存函数的虚拟估计的简单模型更好，其中 CDF 来自训练(0.121 CRPS)。

![](img/1ec04f3e88b0a4f101a071b9882e7b63.png)

分类的每小时生存函数预测示例

# 摘要

在这篇文章中，我们开发了生存神经网络结构，它能够在给定一些外部预测的情况下估计生存函数。我们工作的特殊性包括在处理标准回归问题时改变观点。我们解决了这个问题，提供了对问题的生存解释，并展示了使我们能够通过深度学习来估计生存密度函数的策略。

[**查看我的 GITHUB 回购**](https://github.com/cerlymarco/MEDIUM_NoteBook)

保持联系: [Linkedin](https://www.linkedin.com/in/marco-cerliani-b0bba714b/)