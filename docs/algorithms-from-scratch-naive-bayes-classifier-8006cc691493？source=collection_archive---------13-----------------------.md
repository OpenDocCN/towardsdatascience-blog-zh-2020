# 从零开始的算法:朴素贝叶斯分类器

> 原文：<https://towardsdatascience.com/algorithms-from-scratch-naive-bayes-classifier-8006cc691493?source=collection_archive---------13----------------------->

## [从零开始的算法](https://towardsdatascience.com/tagged/algorithms-from-scratch)

## 从头开始详述和构建朴素贝叶斯分类器

![](img/093ed1dea8adc1bd3def72c5cb745598.png)

由[马库斯·温克勒](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## 介绍

T 朴素贝叶斯分类器是一种 [*急切学习*](https://en.wikipedia.org/wiki/Eager_learning#:~:text=In%20artificial%20intelligence%2C%20eager%20learning,is%20made%20to%20the%20system.) 算法，属于基于贝叶斯定理的简单概率分类器家族。

虽然贝叶斯定理(简而言之，是一种在没有联合概率的情况下计算条件概率的原则方法)假设每个输入都依赖于所有其他变量，但为了将其用作分类器，我们删除了这一假设，并认为每个变量都是相互独立的，并将这种用于预测建模的贝叶斯定理简化称为朴素贝叶斯分类器。换句话说，朴素贝叶斯假设类中某个预测值的存在与任何其他预测值的存在无关。这是一个非常强有力的假设，因为预测者在现实世界的数据中不发生相互作用的可能性非常小。

顺便说一下，如果你对渴望学习不熟悉，渴望学习是指在系统的训练过程中，系统旨在构造一个通用的、与输入无关的目标函数的学习方法。相反，像[*K-最近邻*](/algorithms-from-scratch-k-nearest-neighbors-fe19b431a57) 这样的算法，是一个懒惰的学习者，要等到进行查询之后，才能在训练数据之外进行任何泛化。

本质上，朴素贝叶斯(或白痴贝叶斯)赢得了它的名字，因为每个类的计算被简化以使它们的计算易于处理，然而，分类器证明了自己在许多现实世界的情况下是有效的，无论是二元分类还是多类分类，尽管它的设计简单且假设过于简化。

对于本笔记本中使用的完整代码…

[](https://github.com/kurtispykes/ml-from-scratch/blob/master/naive_bayes.ipynb) [## kurtispykes/ml-从零开始

### permalink dissolve GitHub 是超过 5000 万开发人员的家园，他们一起工作来托管和审查代码，管理…

github.com](https://github.com/kurtispykes/ml-from-scratch/blob/master/naive_bayes.ipynb) 

## 创建模型

如前所述，如果我们要对条件概率分类模型(朴素贝叶斯模型)应用贝叶斯定理，那么我们需要简化计算。

在讨论如何简化模型之前，我将简要介绍边际概率、联合概率和条件概率:

**边际概率** —不考虑其他随机变量的事件概率，例如 P(A ),这意味着事件发生的概率。

**联合概率** —两个或两个以上同时发生事件的概率，例如 P(A 和 B)或 P(A，B)。

**条件概率** —给定一个或多个事件发生的概率，例如 P(A|B)可以表述为给定 B 的概率

关于这些概率的一件很酷的事情是，我们可以用它们来计算彼此。联合概率可以通过使用条件概率来计算，这被称为乘积规则；参见图 1。

![](img/00708fb83bb4183de63ebc3e70e6a137.png)

图 1:使用条件概率计算联合概率

关于乘积法则的一个有趣的事实是，它是对称的，意味着 P(A，B) = P(B，A)。另一方面，条件概率是不对称的，这意味着 P(A|B)！= P(B|A ),但是我们可以利用联合概率来计算条件概率，见图 2。

![](img/19a383d230fcb1d04c034c7ff5b58d67.png)

图 2:使用联合概率计算条件概率

*图 2 有一个问题；计算联合概率通常很困难，所以当我们想要计算条件概率时，我们使用另一种方法。我们使用的另一种方法称为贝叶斯法则或贝叶斯定理，它是通过使用一个条件*概率来计算另一个条件来完成的——参见图 3**

![](img/c97ba69d000401e54431a073f8753ea8.png)

图 3:贝叶斯定理——这个等式的逆等式也成立，因此 P(B|A) = P(A|B) * P(B) / P(A)

> **注**:当反向条件概率可用或更容易计算时，我们也可以决定使用替代方法来计算条件概率。

为了给贝叶斯定理中的术语命名，我们必须考虑使用该方程的上下文—参见图 4。

![](img/47ffc7dfb19495d36ca656fff1df6529.png)

图 4:命名贝叶斯定理的术语

因此，我们可以把贝叶斯定理重述为…

![](img/3defe8231b7d468a41191b3ce3a863d8.png)

图 5:重述贝叶斯定理

为了将此建模为分类模型，我们这样做…

![](img/6776b30a86b8d2e50a73c9ade4249292.png)

图 6:用分类模型表示的贝叶斯定理

然而，这个表达式在我们的计算中是一个复杂的原因，因此为了简化它，我们去除了依赖性的假设，并且考虑到每个变量独立，我们简化了我们的分类器。

> **注意**:我们去掉了分母(本例中观察到数据的概率)，因为它对于所有计算都是常数。

![](img/ae43d14c4e3f8be0c0eac7738b439e15.png)

图 7:朴素贝叶斯分类器

现在你明白了…嗯，不完全是。这个计算是针对每个类标签执行的，但是我们只想知道对于给定的实例最有可能的类。因此，我们必须找到最有可能被选为给定实例的分类的标签；这个决策规则的名字叫做[最大后验概率](https://en.wikipedia.org/wiki/Maximum_a_posteriori_estimation)(MAP)——现在你有了朴素贝叶斯分类器。

**分块算法**

1.  将数据按类分段，然后计算每个类中 *x* 的均值和方差。
2.  使用高斯概率密度函数计算概率
3.  获取类别概率
4.  获得最终预测

**实施**

我们将使用 iris 数据集，由于该数据集中使用的变量是数值型的，因此我们将构建一个高斯朴素贝叶斯模型。

> **注**:不同的朴素贝叶斯分类器的区别主要在于它们对 P(Xi | y)分布的假设(**来源** : [Scikit-Learn 朴素贝叶斯](https://scikit-learn.org/stable/modules/naive_bayes.html))

```
import numpy as np 
import pandas as pdfrom sklearn.datasets import load_iris
from sklearn.naive_bayes import GaussianNB
from sklearn.metrics import accuracy_score
from sklearn.model_selection import train_test_split# loading the data
iris = load_iris()
X, y = iris.data, iris.target# spliting data to train and test
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size= 0.2, random_state=1810)
X_train.shape, y_train.shape, X_test.shape, y_test.shape((120, 4), (120,), (30, 4), (30,))# scikit learn implementation 
nb = GaussianNB()
nb.fit(X_train, y_train)
sklearn_preds = nb.predict(X_test)print(f"sklearn accuracy:{accuracy_score(y_test, sklearn_preds)}")
print(f"predictions: {sklearn_preds}")sklearn accuracy:1.0
predictions: [0 0 2 2 0 1 0 0 1 1 2 1 2 0 1 2 0 0 0 2 1 2 0 0 0 0 1 1 0 2]
```

Scikit-Learn 实现在推理上给了我们一个完美的准确度分数，让我们构建自己的模型，看看我们是否可以匹配 Scikit-learn 实现。

我构建了一个效用函数`get_params`，这样我们就可以为我们的训练数据获取一些参数。

```
def get_params(X_train, y_train): 
    """
    Function to get the unique classes, number of classes and number of features in training data
    """
    num_examples, num_features = X_train.shape
    num_classes = len(np.unique(y_train))
    return num_examples, num_features, num_classes# testing utility function
num_examples, num_features, num_classes = get_params(X_train, y_train)
**print**(num_examples, num_features, num_classes)120 4 3
```

我们的效用函数工作得很好，所以我们可以进行第一步，按类获取统计数据(特别是均值、方差和先验)。

```
def get_stats_by_class(X_train, y_train, num_examples=num_examples, num_classes=num_classes): 
    """
    Get stats of dataset by the class
    """
    # dictionaries to store stats
    class_mean = {}
    class_var = {} 
    class_prior = {} 

    # loop through each class and get mean, variance and prior by class
    for cls in range(num_classes): 
        X_cls = X_train[y_train == cls]
        class_mean[str(cls)] = np.mean(X_cls, axis=0)
        class_var[str(cls)] = np.var(X_cls, axis=0)
        class_prior[str(cls)] = X_cls.shape[0] / num_examples
    return class_mean, class_var, class_prior# output of function 
cm, var, cp = get_stats_by_class(X_train, y_train)
cm, var, cp# output of function 
cm, var, cp = get_stats_by_class(X_train, y_train)
print(f"mean: {cm}\n\nvariance: {var}\n\npriors: {cp}")mean: {'0': array([5.06111111, 3.48611111, 1.44722222, 0.25833333]), '1': array([5.90952381, 2.80714286, 4.25238095, 1.33809524]), '2': array([6.61904762, 2.97857143, 5.58571429, 2.02142857])}

variance: {'0': array([0.12570988, 0.15564043, 0.0286034 , 0.01243056]), '1': array([0.26324263, 0.08542517, 0.24582766, 0.04045351]), '2': array([0.43678005, 0.10930272, 0.31884354, 0.0802551 ])}

priors: {'0': 0.3, '1': 0.35, '2': 0.35}
```

我们将从`get_params`获得的`num_classes`和`num_examples`传递给函数，因为它们需要按类分离数据并按类计算先验。既然我们已经有了足够的信息来计算类别概率——嗯，不完全是，我们正在处理连续数据，一个典型的假设是与每个类别相关的连续值按照高斯分布分布(**来源** : [维基百科](https://en.wikipedia.org/wiki/Naive_Bayes_classifier#Gaussian_na%C3%AFve_Bayes))。因此，我们建立了一个函数来计算密度函数，它将帮助我们计算高斯分布的概率。

```
def gaussian_density_function(X, mean, std, num_examples=num_examples, num_features=num_features, eps=1e-6): 
    num_exambles, num_features = X_train.shape
    const = -num_features/2 * np.log(2*np.pi) - 0.5 * np.sum(np.log(std + eps))
    probs = 0.5 * np.sum(np.power(X - mean, 2)/(std + eps), 1)
    return const - probsgaussian_density_function(X_train, cm[str(0)], var[str(0)])array([-4.34046349e+02, -1.59180054e+02, -1.61095055e+02,  9.25593725e-01,
       -2.40503860e+02, -4.94829021e+02, -8.44007497e+01, -1.24647713e+02,
       -2.85653665e+00, -5.72257925e+02, -3.88046018e+02, -2.24563508e+02,
        2.14664687e+00, -6.59682718e+02, -1.42720100e+02, -4.38322421e+02,
       -2.27259034e+02, -2.43243607e+02, -2.60192759e+02, -6.69113243e-01,
       -2.12744190e+02, -1.96296373e+00,  5.27718947e-01, -8.37591818e+01,
       -3.74910393e+02, -4.12550151e+02, -5.26784003e+02,  2.02972576e+00,
       -7.15335962e+02, -4.20276820e+02,  1.96012133e+00, -3.00593481e+02,
       -2.47461333e+02, -1.60575712e+02, -2.89201209e+02, -2.92885637e+02,
       -3.13408398e+02, -3.58425796e+02, -3.91682377e+00,  1.39469746e+00,
       -5.96494272e+02, -2.28962605e+02, -3.30798243e+02, -6.31249585e+02,
       -2.13727911e+02, -3.30118570e+02, -1.67525014e+02, -1.76565131e+02,
        9.43246044e-01,  1.79792264e+00, -5.80893842e+02, -4.89795508e+02,
       -1.52006930e+02, -2.23865257e+02, -3.95841849e+00, -2.96494860e+02,
       -9.76659579e+01, -3.45123893e+02, -2.61299515e+02,  7.51925529e-01,
       -1.57383774e+02, -1.13127846e+02,  6.89240784e-02, -4.32253752e+02,
       -2.25822704e+00, -1.95763452e+02, -2.54997829e-01, -1.66303411e+02,
       -2.94088881e+02, -1.47028139e+02, -4.89549541e+02, -4.61090964e+02,
        1.22387847e+00, -8.22913900e-02,  9.67128415e-01, -2.30042263e+02,
       -2.90035079e+00, -2.36569499e+02,  1.42223431e+00,  9.35599166e-01,
       -3.74718213e+02, -2.07417873e+02, -4.19130888e+02,  7.79051525e-01,
        1.82103882e+00, -2.77364308e+02,  9.64732218e-01, -7.15058948e+01,
       -2.82064236e+02, -1.89898997e+02,  9.79605922e-01, -6.24660543e+02,
        1.70258877e+00, -3.17104964e-01, -4.23008651e+02, -1.32107552e+00,
       -3.09809542e+02, -4.01988565e+02, -2.55855351e+02, -2.25652042e+02,
        1.00821726e+00, -2.24154135e+02,  2.07961315e+00, -3.08858104e+02,
       -4.95246865e+02, -4.74107852e+02, -5.24258175e+02, -5.26011925e+02,
       -3.43520576e+02, -4.59462733e+02, -1.68243666e+02,  1.06990125e+00,
        2.04670066e+00, -8.64641201e-01, -3.89431048e+02, -1.00629804e+02,
        1.25321722e+00, -5.07813723e+02, -1.27546482e+02, -4.43687565e+02])
```

这是一个计算类概率的函数…

```
def class_probabilities(X, class_mean, class_var, class_prior, num_classes=num_classes):
    """
    calculate the probability of each class given the data
    """
    num_examples = X.shape[0]
    probs = np.zeros((num_examples, num_classes))for cls in range(num_classes): 
        prior = class_prior[str(cls)]
        probs_cls = gaussian_density_function(X, class_mean[str(cls)], class_var[str(cls)])
        probs[:, cls] = probs_cls + np.log(prior)
    return probs
```

现在我们需要使用 MAP 进行预测，所以让我们将所有这些步骤放在一个预测函数中，并输出最大概率类。

```
def predict(X_test, X_train, y_train): 
    num_examples, num_features, num_classes = get_params(X_test, y_train)
    class_mean, class_std, class_prior = get_stats_by_class(X_train, y_train)
    probs = class_probabilities(X_test, class_mean, class_std, class_prior)
    return np.argmax(probs, 1)my_preds = predict(X_test, X_train, y_train)**print**(f"my predictions accuracy:{accuracy_score(y_test, my_preds)}")
**print**(f"predictions: {my_preds}")my predictions accuracy:1.0
predictions: [0 0 2 2 0 1 0 0 1 1 2 1 2 0 1 2 0 0 0 2 1 2 0 0 0 0 1 1 0 2]
```

作为健全检查…

```
sklearn_preds == my_preds**array**([ True,  True,  True,  True,  True,  True,  True,  True,  True,
        True,  True,  True,  True,  True,  True,  True,  True,  True,
        True,  True,  True,  True,  True,  True,  True,  True,  True,
        True,  True,  True])
```

饼干就是这样碎的！

## 赞成的意见

*   需要少量的训练数据来估计必要的参数
*   与复杂的方法相比，速度极快

## 骗局

*   众所周知是一个糟糕的估计器(在 Scikit-Learn 框架中，`predict_proba`的输出没有被太认真对待。
*   独立预测者的假设在现实世界中并不成立(大部分时间)

## 包裹

现在，您已经了解了朴素贝叶斯分类器以及如何使用 Python 从头开始构建一个分类器。是的，该算法有非常过于简化的假设，但它在许多现实世界的应用中仍然非常有效，如果你想要非常快速的预测，值得一试。

让我们继续 LinkedIn 上的对话…

[](https://www.linkedin.com/in/kurtispykes/) [## Kurtis Pykes -人工智能作家-走向数据科学| LinkedIn

### 在世界上最大的职业社区 LinkedIn 上查看 Kurtis Pykes 的个人资料。Kurtis 有两个工作列在他们的…

www.linkedin.com](https://www.linkedin.com/in/kurtispykes/)