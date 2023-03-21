# 机器学习算法。这里是端到端。

> 原文：<https://towardsdatascience.com/machine-learning-algorithms-heres-the-end-to-end-a5f2f479d1ef?source=collection_archive---------42----------------------->

## 通用分类算法的端到端运行；包括随机森林、多项式朴素贝叶斯、逻辑回归、kNN 和来自 sklearn 的支持向量机

![](img/5798d0a7b2ce7a4167f19627afac862f.png)

约翰·汤纳在[Unsplash](https://unsplash.com/s/photos/forest?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【1】上的照片。

# 目录

1.  介绍
2.  随机森林
3.  多项式朴素贝叶斯
4.  逻辑回归
5.  支持向量机
6.  k-最近邻
7.  摘要
8.  参考

# 介绍

本文的目标是:

> 1.学习常见的数据科学、分类算法。
> 
> 2.执行这些算法的示例代码。

虽然有几篇关于机器学习算法的文档和文章，但我想总结一下作为一名专业数据科学家最常用的算法。此外，我将包含一些带有虚拟数据的示例代码，以便您可以开始执行各种模型！下面，我将从最受青睐的库 [sklearn](https://scikit-learn.org/stable/) ( *也称为 scikit-learn*)【2】中概述这些主要的分类算法:

*   随机森林(*RandomForestClassifier*)—可用于回归
*   多项式朴素贝叶斯(*多项式*
*   逻辑回归(*逻辑回归*
*   支持向量机( *svm* ) —可用于回归
*   k-最近邻(*KNeighborsClassifier*)-可用于回归

**什么是分类？**

```
Assigning new data into a category or bucket. 
```

而无监督学习，如常用的 K-means 算法，旨在将相似的数据组分组在一起，而无需标签、监督学习或分类——嗯，将数据分类为各种类别。下面描述了一个简单的分类示例。

*标签:水果——香蕉、橘子和苹果*

*特征:形状、大小、颜色、种子数等。*

分类模型从关于水果的特征中学习，以建议输入食物和水果标签。现在，想象一下，对于我将在下面描述的这些分类算法，来自数据的每个观察值，输入，都有关于它的各种特征，然后将基于预定的标签进行分类。类别的其他名称包括标签、类、组(*通常在无监督聚类中引用，但仍然可以描述分类*)和桶。

分类的重要性不仅仅在于教育，还在于它在商业世界中极其实用和有益。比方说你要给成千上万的文档分类，给衣服分类，或者检测图像；这些都使用了我将在下面描述的这些流行且强大的分类模型。

# 随机森林

![](img/1fc399ba2e7ad504e0b3a706400c5efb.png)

Lukasz Szmigiel 在[Unsplash](https://unsplash.com/s/photos/trees?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【3】上拍摄的照片。

这种集成分类器通过在所用数据集的子样本上拟合几个决策树来工作。然后，它对这些决策树的准确性进行平均，并创建一个比仅使用决策树更好的预测准确性，以控制过度拟合。与大多数分类器一样，有大量的参数可以帮助您调整您的模型，使其更差或更好(*希望更好*)。下面描述了常见的参数，我将包括默认的参数值，以便您对从哪里开始调优有一个大致的了解:

```
**n_estimators**: number of trees in your forest (100)**max_depth**: maximum depth of your tree (None) - recommendation, change this parameter to be an actual number because this parameter could cause overfitting from learning your traning data too well**min_samples_split**: minimum samples required to split your node (2)**min_samples_leaf**: mimimum number of samples to be at your leaf node (1)**max_features**: number of features used for the best split ("auto")**boostrap**: if you want to use boostrapped samples (True)**n_jobs**: number of jobs in parallel run (None) - for using all processors, put -1**random_state**: for reproducibility in controlling randomness of samples (None)**verbose**: text output of model in process (None)**class_weight**: balancing weights of features, n_samples / (n_classes * np.bincount(y)) (None) - recommendation, use 'balanced' for labels that are unbalanced
```

我将在这里提供一个随机森林算法的例子。它将使用虚拟数据(*所以准确性对我来说并不重要*)。重点是代码、概念，而不是准确性，除非您的输入数据已经很好地建立。在代码示例中有五个单独的行，您可以注释掉所有的分类器， *clf* **，**除了一个——您正在测试的那个。下面是主代码示例[4]:

```
# import libraries
from sklearn.feature_extraction.text import TfidfVectorizer, CountVectorizer
from sklearn.base import BaseEstimator, TransformerMixin
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.pipeline import Pipeline, FeatureUnion
from sklearn.naive_bayes import MultinomialNB
from sklearn.linear_model import LogisticRegression
from sklearn.neighbors import KNeighborsClassifier
from sklearn import svm
from sklearn import metrics
import pandas as pd# text and numeric classes that use sklearn base libaries
class TextTransformer(BaseEstimator, TransformerMixin):
    """
    Transform text features
    """
    def __init__(self, key):
        self.key = keydef fit(self, X, y=None, *parg, **kwarg):
        return selfdef transform(self, X):
        return X[self.key]

class NumberTransformer(BaseEstimator, TransformerMixin):
    """
    Transform numeric features
    """
    def __init__(self, key):
        self.key = keydef fit(self, X, y=None):
        return selfdef transform(self, X):
        return X[[self.key]]# read in your dataframe
df = pd.read_csv('/Users/data.csv')# take a look at the first 5 observations
df.head()# use the term-frequency inverse document frequency vectorizer to transfrom count of text
# into a weighed matrix of term importance
vec_tdidf = TfidfVectorizer(ngram_range=(1,1), analyzer='word', norm='l2')# compile both the TextTransformer and TfidfVectorizer 
# to the text 'Text_Feature' 
color_text = Pipeline([
                ('transformer', TextTransformer(key='Text_Feature')),
                ('vectorizer', vec_tdidf)
                ])# compile the NumberTransformer to 'Confirmed_Test', 'Confirmed_Recovery', 
# and 'Confirmed_New' numeric features
test_numeric = Pipeline([
                ('transformer', NumberTransformer(key='Confirmed_Test')),
                ])
recovery_numeric = Pipeline([
                ('transformer', NumberTransformer(key='Confirmed_Recovery')),
                ])
new_numeric = Pipeline([
                ('transformer', NumberTransformer(key='Confirmed_New')),
                ])# combine all of the features, text and numeric together
features = FeatureUnion([('Text_Feature', color_text),
                      ('Confirmed_Test', test_numeric),
                      ('Confirmed_Recovery', recovery_numeric),
                      ('Confirmed_New', new_numeric)
                      ])# create the classfier from list of algs - choose one only
clf = RandomForestClassifier()
clf = MultinomialNB()
clf = LogisticRegression()
clf = svm.SVC()
clf = KNeighborsClassifier()# unite the features and classfier together
pipe = Pipeline([('features', features),
                 ('clf',clf)
                 ])# transform the categorical predictor into numeric
predicted_dummies = pd.get_dummies(df['Text_Predictor'])# split the data into train and test
# isolate the features from the predicted field
text_numeric_features = ['Text_Feature', 'Confirmed_Test', 'Confirmed_Recovery', 'Confirmed_New']
predictor = 'Text_Predictor'X_train, X_test, y_train, y_test = train_test_split(df[text_numeric_features], df[predictor], 
                                                    test_size=0.25, random_state=42)# fit the model
pipe.fit(X_train, y_train)# predict from the test set
preds = pipe.predict(X_test)# print out your accuracy!
print("Accuracy:",metrics.accuracy_score(y_test, preds))
```

*如果你想看看. py 格式的 GitHub gist 在 python 中是什么样子的，这里有我的 GitHub gist 的代码【4】:*

GitHub gist [4]上作者的整篇文章示例代码。

# 多项式朴素贝叶斯

![](img/7f93be275a5426ce4c012b8e9e190cd6.png)

帕特里克·托马索在[Unsplash](https://unsplash.com/s/photos/words?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【5】上拍摄的照片。

这个分类器是一个朴素贝叶斯分类器，用于多项式模型，正如算法的名字所暗示的那样。它广泛应用于文本分类问题中，其中文本是来自字数统计的特征。使用该算法时，建议使用*术语-频率逆文档频率* ( *td-idf* )管道，因为您想要一个分布良好的特性。因为这个分类器是基于*贝叶斯定理*的，所以假设特征之间存在独立性。下面是分类器的主要代码:

```
clf = MultinomialNB()
```

所用模型的参数和属性如下:

```
parameters - **alpha**: a paramter for smoothing (1.0)**class_prior**: looking at the previous class probability (None) attributes -**feature_count**: number of samples for each class or feature (number of classes, number of features)**n_features**: number of features for sample
```

# 逻辑回归

逻辑回归，或者更像逻辑分类，是一种使用 *logit* 函数的算法。估计值通常是二进制值，如真-假、是-否和 0-1。逻辑函数也使用 sigmoid 函数。关于这个模型的细节有很多文章(*见*下面有用的链接)，但是我将强调如何执行 sklearn 库中的代码。下面是分类器的主要代码:

```
clf = LogisticRegression()
```

逻辑回归有许多重要的参数。以下是其中的几个例子:

```
**penalty**: l1 or l2 (lasso or ridge), the normality of penalization (l2)**multi_class**: binary versus multiclass label data ('auto')
```

# 支持向量机

一种监督技术，不仅包括分类，还包括回归。支持向量机(SVMs)的好处是，它比其他构成高维度的算法效果更好。支持向量机的目标是找到在两个分类类别之间划分数据的线(*它们在那条线*之间的距离)。支持向量机的代码与上面的算法略有不同:

```
clf = svm.SVC()
```

这种代码上的差异引用了增加的 *SVC* ，也就是 C-支持向量分类。对于多类情况，您可以使用参数:

```
parameter **-** **decision_function_shape**: 'ovr' or 'one-versus-rest' approach
```

# k-最近邻

![](img/99ba5beef84c2f9639e97dd190bd815d.png)

照片由[克里斯蒂安·斯塔尔](https://unsplash.com/@woodpecker65?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/neighbors?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【6】上拍摄。

k 近邻法，或称 KNN 法，是通过多数投票来实现的。它根据周围的邻居对新类别进行分类；常用的距离测量函数是欧几里德函数。用于该算法的代码是:

```
clf = KNeighborsClassifier()
```

最重要的参数可能是:

```
parameter - **n_neighbors**: number of neighbors (5)
```

# 摘要

虽然有大量关于广泛使用的机器学习算法的文章；

> 我希望这篇文章对你有用，因为我已经提供了端到端的代码。

请查看下面有用的链接以及本文中描述的每种算法的官方文档。感谢您阅读我的文章，如果您有任何问题，请告诉我！

# 参考

[1]约翰·汤纳在 [Unsplash](https://unsplash.com/s/photos/forest?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2016)

[2] sklearn， [sklearn](https://scikit-learn.org/stable/index.html) ，(2020)

[3]Lukasz Szmigiel 在 [Unsplash](https://unsplash.com/s/photos/trees?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2015)

[4] M.Przybyla，[分类-模型](https://gist.github.com/mprzybyla123/9af0d3f9ff09e8a04ddca8d1e14aa01e)，(2020)

[5]帕特里克·托马索在 [Unsplash](https://unsplash.com/s/photos/words?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2016)

[6]照片由 [Christian Stahl](https://unsplash.com/@woodpecker65?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/neighbors?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄，(2017)

**有用的 sklearn 链接:**

[随机森林](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)

[多项式朴素贝叶斯](https://scikit-learn.org/stable/modules/generated/sklearn.naive_bayes.MultinomialNB.html)

[逻辑回归](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html)

[支持向量机](https://scikit-learn.org/stable/modules/svm.html)

[K-最近邻](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsClassifier.html)