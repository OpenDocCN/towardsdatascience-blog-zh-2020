# 机器学习工具箱的必备算法:XGBoost

> 原文：<https://towardsdatascience.com/a-must-have-algorithm-for-your-machine-learning-toolbox-xgboost-3e295cf8d69b?source=collection_archive---------39----------------------->

![](img/ec7d1d001a016711801b5e0d8e5d260a.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=67643) 的[维基图片](https://pixabay.com/users/WikiImages-1897/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=67643)

# 最高效的机器学习算法之一

XGBoost 是一种监督学习算法，可用于回归和分类。像所有的算法一样，它有它的优点和吸引人之处，我们将会详细介绍。

在这篇文章中，我们将从分类问题的上下文中学习 XGBoost。对于回归部分，请务必关注我在 datasciencelessons.com 的博客。

# 监督学习赶上来

我不会在这里深入探讨，但是对于那些需要快速复习监督学习的人来说；监督学习是指当你头脑中有一个你想要预测的特定事物时。例如，你想预测未来的房价；所以你有了你想要预测的东西，下一步就是标记历史数据作为预测未来的手段。为了更深入地研究这个例子；比方说，你想卖掉你的房子，但想知道你应该付多少钱，你可以潜在地积累关于房子的数据点，以及在同一时期它们的销售价格。从这一点出发，您将训练一个模型，您将把关于您自己住宅的数据点传递给该模型，以生成关于您住宅价值的预测。对于分类的例子，你的预测类；比方说你的 Gmail 和想要预测垃圾邮件…这需要一个模型来训练许多被“标记”为垃圾邮件的电子邮件以及相应数量的未被“标记”为垃圾邮件的电子邮件。

# 到底是什么？

XGBoost 使用的是所谓的系综方法。在不深入研究集成方法的情况下，XGBoost 的独特之处在于它如何利用许多模型的输出来生成预测！XGBoost 利用所谓的“弱学习者”来培养“强学习者”。

XGBoost 过程看起来像这样:

*   它迭代地训练许多弱模型
*   根据性能对每个预测进行加权
*   结合许多加权预测，得出最终输出。

# 是什么让 XGBoost 如此受欢迎？

*   准确(性)
*   速度
*   该算法很好地利用了现代计算，允许其并行化
*   持续优于其他算法

# 您的第一个 XGBoost 模型

我们来分解步骤吧！

*   您将从用 python 导入 XGBoost 包开始
*   分别用 y 和 X 分解你的因变量和自变量
*   分解列车测试分裂
*   实例化您的分类器
*   训练你的分类器
*   预测测试集的 y
*   评估准确性！

在这个例子中，我们将对泰坦尼克号上的幸存者进行分类。

```
import xgboost as xgb from sklearn.model_selection 
import train_test_splitX, y = titanic.iloc[:,:-1], titanic.iloc[:,-1]X_train, X_test, y_train, y_test= train_test_split(X, y, test_size=.2, random_state=44)xgb_model = xgb.XGBClassifier(objective='binary:logistic', n_estimators= 7, seed=44)xgb_model.fit(X_train, y_train) pred = xgb_model.predict(X_test)accuracy = float(np.sum(pred == y_test)) / y_test.shape[0]
```

干得好！这是一个伟大的第一次传球！让我们学习更多关于如何评估模型质量的知识。

# 评估绩效

我们已经在上面的代码片段中看到了准确性，但是还有混淆矩阵的其他方面，精度和召回。我不会在这里谈论这两个，但如果你想了解更多信息，请跳转到关于随机森林算法的这篇文章:[https://datasciencelessons . com/2019/08/13/random-forest-for-class ification-in-r/](https://datasciencelessons.com/2019/08/13/random-forest-for-classification-in-r/)

除了这些，我想说一下 AUC。

简单来说；如果您选择一个正数据点和一个负数据点，AUC 是正数据点比负数据点排名更高的概率。

XGBoost 允许您运行交叉验证测试&在核心算法调用本身中指定您关心的指标。这部分是通过创建一个叫做 dmatrix 的数据结构来完成的，这个数据结构由 X & y 值组成。

这次的核心区别是，我们将创建一个 dmatrix，指定模型的参数，然后生成一个输出，其中我们将指标指定为 AUC。

```
titanic_dm = xgb.DMatrix(data=X, label=y)params = {"objective":"reg:logistic", "max_depth":3}output = xgb.cv(dtrain=titanic_dm, params=params, nfold=3, num_boost_round=5, metrics="auc", as_pandas=True, seed=123)print((output["test-auc-mean"]).iloc[-1])
```

# 你应该多久使用一次？

XGBoost 并不是每次你需要预测什么的时候都要用。但是，在适当的情况下，它会非常有用:

*   你有很多训练数据
*   你不仅有分类数据，还有数值和分类变量的混合，或者只有数值变量。

你肯定希望 XGBoost 远离计算机视觉和 nlp 相关的任务…或者如果你有一些非常有限的数据。

一如既往，我希望这证明对您的数据科学工作有用！一定要看看我在 datasciencelessons.com 的其他帖子！

祝数据科学快乐！