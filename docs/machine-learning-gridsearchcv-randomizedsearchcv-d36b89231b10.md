# 机器学习:GridSearchCV & RandomizedSearchCV

> 原文：<https://towardsdatascience.com/machine-learning-gridsearchcv-randomizedsearchcv-d36b89231b10?source=collection_archive---------12----------------------->

## 使用 GridSearchCV 和 RandomizedSearchCV 进行超参数调整

![](img/441fbf9e4110a98cef927b0b7b92c1c3.png)

[JESHOOTS.COM](https://unsplash.com/@jeshoots?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/study?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

**简介**

在这篇文章中，我想谈谈我们如何通过调整参数来提高机器学习模型的性能。

显然，为了改进我们的模型，了解我们想要调整的参数的含义是很重要的。出于这个原因，在谈论 GridSearchCV 和 RandomizedSearchCV 之前，我将首先解释一些参数，如 C 和 gamma。

**第一部分:SVC 中一些参数的概述**

在**逻辑回归**和**支持向量分类器**中，决定正则化强度的参数称为 **C** 。

对于一个**高 C** ，我们将有一个更少的正则化，这意味着我们试图尽可能地适应训练集。相反，当参数 **C** 的**值较低**时，算法会尝试调整到“大多数”数据点，并提高模型的泛化能力。

还有一个重要的参数叫做**伽马**。但在谈论它之前，我认为了解一点线性模型的局限性是很重要的。

线性模型在低维空间中非常有限，因为线和超平面具有有限的灵活性。使线性模型更加灵活的一种方法是添加更多要素，例如，添加输入要素的交互或多项式。

用于分类的线性模型只能使用线来分隔点，这并不总是更好的选择。因此，解决方案可以是在三维空间中表示点，而不是在二维空间中。事实上，在三维空间中，我们可以创建一个平面，以更精确的方式对我们数据集的点进行划分和分类。

有两种方法可以将你的数据映射到一个更高维的空间中:**多项式核**，计算所有可能的多项式，直到原始特征的某个程度；以及**径向基函数(RBF)** 核，也称为**高斯核**，其测量数据点之间的距离。这里 **gamma** 的任务是控制**高斯核的宽度。**

**第二部分:GridSearchCV**

正如我在我的[上一篇文章](/machine-learning-some-notes-about-cross-validation-4a0315599f2)中所展示的，交叉验证允许我们评估和改进我们的模型。但是还有另一个有趣的技术来改进和评估我们的模型，这个技术叫做**网格搜索**。

**网格搜索**是一种调整监督学习中参数，提高模型泛化性能的有效方法。通过网格搜索，我们尝试了感兴趣的参数的所有可能组合，并找到了最佳组合。

**Scikit-learn** 提供了 **GridSeaechCV** 类。显然，我们首先需要指定我们想要搜索的参数，然后 GridSearchCV 将执行所有必要的模型拟合。例如，我们可以创建下面的字典，它提供了我们想要为我们的模型搜索的所有参数。

```
parameters = {‘C’: [0.001, 0.01, 0.1, 1, 10, 100], 
‘gamma’: [0.001, 0.01, 0.1, 1, 10, 100]}
```

然后，我们可以用 SVC 模型实例化 GridSearchCV 类，并应用 6 个交叉验证实验。当然，我们还需要将数据分成训练集和测试集，以避免过度拟合参数。

```
from **sklearn.model_selection** import **GridSearchCV** from **sklearn.model_selection** import **train_test_split** from s**klearn.svm** import **SVC** search = GridSearchCV(SVC(), parameters, cv=5)X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=0)
```

现在，我们可以用我们的训练数据来拟合我们已经创建的搜索对象。

```
search.fit(X_train, y_train)
```

因此，GridSearchCV 对象搜索最佳参数，并在整个训练数据集上自动拟合新模型。

**第三部分:随机搜索**

当我们有很多参数要试，训练时间很长的时候，RandomizedSearchCV 就非常有用了。对于这个例子，我使用随机森林分类器，所以我想你已经知道这种算法是如何工作的。

第一步是写出我们想要考虑的参数，并从这些参数中选择最好的。

```
param = {‘max_depth: [6,9, None], 
         ‘n_estimators’:[50, 70, 100, 150], 
          'max_features': randint(1,6),
          'criterion' : ['gini', 'entropy'],
          'bootstrap':[True, False],
          'mln_samples_leaf': randint(1,4)}
```

现在我们可以创建我们的 **RandomizedSearchCV** 对象并拟合数据。最后，我们可以找到最佳参数和最佳分数。

```
from **sklearn.model_selection** import **RandomSearchCV** from **sklearn.ensemble** import **RandomForestClassifier**rnd_search = RandomizedSearchCV(RandomForestClassifier(), param, 
n_iter =10, cv=9)rnd_search.fit(X,y)
rnd_search.best_params_
rnd_search.best_score_
```

**结论**

因此，当我们处理少量的超参数时，网格搜索是很好的。但是，如果要考虑的参数数量特别多，并且影响的大小不平衡，那么更好的选择是使用随机搜索。

感谢你阅读这篇文章。您还可以通过其他方式与我保持联系并关注我的工作:

*   订阅我的时事通讯。
*   也可以通过我的电报群 [*初学数据科学*](https://t.me/DataScienceForBeginners) 取得联系。