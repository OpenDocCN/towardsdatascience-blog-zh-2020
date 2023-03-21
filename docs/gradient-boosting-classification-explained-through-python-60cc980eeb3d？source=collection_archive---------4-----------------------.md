# 通过 Python 解释梯度增强分类

> 原文：<https://towardsdatascience.com/gradient-boosting-classification-explained-through-python-60cc980eeb3d?source=collection_archive---------4----------------------->

![](img/0b1bb7e97c7463ce7d62c4cef2fb3311.png)

马切伊·鲁明凯维奇在 [Unsplash](https://unsplash.com/s/photos/rocket-boost?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

在我的[上一篇文章](https://medium.com/@vagifaliyev/a-hands-on-explanation-of-gradient-boosting-regression-4cfe7cfdf9e)中，我讨论并经历了一个用于回归的梯度推进的 python 实例。在这篇文章中，我想讨论梯度推进是如何用于分类的。如果你没有读过那篇文章，没关系，因为我会重申我在上一篇文章中讨论的内容。所以，让我们开始吧！

# 集成方法

通常，你可能有几个好的预测器，你想把它们都用上，而不是痛苦地选择一个，因为它有 0.0001 的准确性增加。来了*合奏学习。*在集成学习中，不是使用单个预测器，而是聚合数据中的多个预测器和训练及其结果，通常比使用单个模型给出更好的分数。例如，一个随机森林就是一组打包(或粘贴)的决策树。

![](img/d372964cd39a9f56bb10cf77655c992b.png)

Samuel Sianipar 在 [Unsplash](https://unsplash.com/s/photos/orchestra?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

你可以把合奏方法想象成一个管弦乐队；不是只有一个人演奏一种乐器，而是多人演奏不同的乐器，通过组合所有的音乐组合，音乐听起来通常比单人演奏要好。

虽然梯度增强是一种集成学习方法，但它更具体地说是一种*增强*技术。那么，是什么在推动？

# 助推

*Boosting* 是一种特殊类型的集成学习技术，它通过将几个*弱学习器(*准确性差的预测器 *)* 组合成一个强学习器(准确性强的模型)来工作。这是通过每个模型关注其前任的错误来实现的。

两种最流行的升压方法是:

*   适应性增强(你可以在这里阅读我的文章)
*   梯度推进

我们将讨论梯度推进。

# 梯度推进

在梯度推进中，每个预测器都试图通过减少误差来改进其前任。但是梯度推进背后的有趣想法是，它不是在每次迭代中对数据拟合预测器，而是实际上对前一个预测器产生的残差拟合一个新的预测器。我们来看一下梯度推进分类工作原理的逐步示例:

![](img/f0e72bdef0106eb02e33b29eeb16b4c6.png)

照片由[林赛·亨伍德](https://unsplash.com/@lindsayhenwood?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/steps?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

1.  为了对数据进行初始预测，算法将获得目标特征的几率的*对数。这通常是真值(值等于 1)的数量除以假值(值等于 0)的数量。*

因此，如果我们有一个包含 6 个实例的乳腺癌数据集，其中 4 个实例是患有乳腺癌的人(4 个目标值= 1)，2 个实例是没有患乳腺癌的人(2 个目标值= 0)，那么 log(odds) = log(4/2) ~ 0.7。这是我们的基本估计。

1.  一旦有了对数(赔率)，我们通过使用逻辑函数将该值转换成概率，以便进行预测。如果我们继续我们之前的 0.7 的对数(比值)值的例子，那么逻辑函数也将等于 0.7 左右。

由于该值*大于 0.5，*该算法将为每个实例预测 0.7 作为其基本估计值。将对数(赔率)转换成概率的公式如下:

```
e * log(odds) / (1 + e * log(odds))
```

1.  对于训练集中的每个实例，它计算该实例的*残差*，或者换句话说，观察值减去预测值。
2.  一旦它完成了这些，它就建立一个新的决策树，实际上试图预测先前计算的残差。然而，与梯度推进回归相比，这是它变得稍微棘手的地方。

构建决策树时，允许有固定数量的叶子。这可以由用户设置为一个参数，通常在 8 到 32 之间。这导致了两种可能的结果:

*   多个实例落在同一片叶子中
*   单个实例有自己的叶

与回归的梯度推进不同，我们可以简单地平均实例值以获得输出值，并将单个实例作为其自己的一片叶子，我们必须使用一个公式来转换这些值:

![](img/1e0ff39aae352588253e5c6a6ee55261.png)

鸣谢:博客空间

σ符号表示“的和”，而 *PreviousProb* 指的是我们之前计算的概率(在我们的例子中，为 0.7)。我们对树中的每一片叶子都应用这种变换。我们为什么要这样做？因为请记住，我们的基本估计量是一个对数(赔率)，而我们的树实际上是建立在概率上的，所以我们不能简单地将它们相加，因为它们来自两个不同的来源。

## 做预测

现在，为了进行新的预测，我们做两件事:

1.  获取训练集中每个实例的对数(赔率)预测
2.  将预测转换成概率

对于训练集中的每个实例，进行预测的公式如下:

```
base_log_odds + (**learning_rate *** predicted residual value)
```

*learning_rate* 是一个超参数，用于缩放每棵树的贡献，牺牲偏差以获得更好的方差。换句话说，我们将这个数字乘以预测值，这样我们就不会过度拟合数据。

一旦我们计算出对数(赔率)预测，我们现在必须使用前面将对数(赔率)值转换为概率的公式将其转换为概率。

## 重复&对看不见的数据进行预测

![](img/fdeccb4cdbfd484fc88369fc9f77eeb1.png)

照片由[布雷特·乔丹](https://unsplash.com/@brett_jordan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/repeat?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

完成这个过程后，我们计算树的新残差，并创建一个新的树来拟合新的残差。再次重复该过程，直到达到某个预定义的阈值，或者残差可以忽略。

如果我们训练了 6 棵树，并且我们想要对一个看不见的实例进行新的预测，那么它的伪代码将是:

```
X_test_prediction = base_log_odds + (**learning_rate *** tree1_scaled_output_value) + 
(**learning_rate *** tree2_scaled_output_value) +
(**learning_rate *** tree3_scaled_output_value) +
(**learning_rate *** tree4_scaled_output_value) +
(**learning_rate *** tree5_scaled_output_value) +
(**learning_rate *** tree6_scaled_output_value) +prediction_probability = 
e*X_test_prediction / (1 + e*X_test_prediction)
```

好了，现在你应该对用于分类的梯度推进的底层机制有所了解了，让我们开始编码来巩固这些知识吧！

![](img/843ea9feaab2c5f156ca199e97a54b1d.png)

罗布·萨米恩托在 [Unsplash](https://unsplash.com/s/photos/brick-and-cement?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# 使用 Scikit-Learn 进行梯度增强分类

我们将使用预先构建到 scikit-learn 中的乳腺癌数据集作为示例数据。首先，让我们弄清楚一些重要的东西:

```
import pandas as pd
import numpy as npfrom sklearn.metrics import classification_report
from sklearn.model_selection import KFold
from sklearn.datasets import load_breast_cancerfrom sklearn.ensemble import GradientBoostingClassifier
```

在这里，我们只是导入 pandas、numpy、我们的模型和一个评估我们模型性能的指标。

```
df = pd.DataFrame(load_breast_cancer()['data'],
columns=load_breast_cancer()['feature_names'])df['y'] = load_breast_cancer()['target']
df.head(5)
```

为了方便起见，我们将数据转换成 DataFrame，因为这样更容易操作。请随意跳过这一步。

```
X,y = df.drop('y',axis=1),df.ykf = KFold(n_splits=5,random_state=42,shuffle=True)for train_index,val_index in kf.split(X):
    X_train,X_val = X.iloc[train_index],X.iloc[val_index],
    y_train,y_val = y.iloc[train_index],y.iloc[val_index],
```

在这里，我们定义了我们的特征和标签，并使用 5 折交叉验证将 ur 数据分成一个训练和验证。

```
gradient_booster = GradientBoostingClassifier(learning_rate=0.1)
gradient_booster.get_params()OUT:{'ccp_alpha': 0.0,
 'criterion': 'friedman_mse',
 'init': None,
 'learning_rate': 0.1,
 'loss': 'deviance',
 'max_depth': 3,
 'max_features': None,
 'max_leaf_nodes': None,
 'min_impurity_decrease': 0.0,
 'min_impurity_split': None,
 'min_samples_leaf': 1,
 'min_samples_split': 2,
 'min_weight_fraction_leaf': 0.0,
 'n_estimators': 100,
 'n_iter_no_change': None,
 'presort': 'deprecated',
 'random_state': None,
 'subsample': 1.0,
 'tol': 0.0001,
 'validation_fraction': 0.1,
 'verbose': 0,
 'warm_start': False}
```

这里有很多参数，所以我只讨论最重要的:

*   **标准**:用于寻找分割数据的最佳特征和阈值的损失函数
*   **learning_rate** :该参数衡量每棵树的贡献
*   **max_depth** :每棵树的最大深度
*   **n_estimators** :要构建的树的数量
*   **init:初始估计量。**默认情况下，它是将对数(赔率)转换成概率(就像我们之前讨论的那样)

```
gradient_booster.fit(X_train,y_train)print(classification_report(y_val,gradient_booster.predict(X_val)))OUT:precision    recall  f1-score   support0       0.98      0.93      0.96        46
1       0.96      0.99      0.97        67 accuracy                           0.96       113
   macro avg       0.97      0.96      0.96       113
weighted avg       0.96      0.96      0.96       113
```

好吧，96%的准确率！

我希望这篇文章已经帮助你理解了梯度推进分类(以某种形式)。我祝你在 ML 的努力中一切顺利，并记住；知之甚少却知之甚详的人，胜过知之甚多却知之甚少的人！

![](img/79f050733d12bcc83ce13aaceff626cf.png)

由 [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/thank-you?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片