# 机器学习基础:逻辑回归

> 原文：<https://towardsdatascience.com/machine-learning-basics-logistic-regression-890ef5e3a272?source=collection_archive---------16----------------------->

## 学习逻辑回归基本分类模型的算法及其实现

在之前的[故事](/machine-learning-basics-simple-linear-regression-bc83c01baa07)中，我已经解释了各种 ***回归*** 模型的实现程序。当我们继续讨论 ***分类*** 时，为什么这个算法的名称仍然是回归，这难道不令人惊讶吗？让我们了解逻辑回归的机制，并通过一个例子学习建立分类模型。

## 逻辑回归概述

逻辑回归是一种分类模型，在因变量(输出)为二进制格式时使用，如 0(假)或 1(真)。例子包括例如预测是否有肿瘤(1)或没有肿瘤(0)以及电子邮件是否是垃圾邮件(1)或没有垃圾邮件(0)。

逻辑函数，也称为 sigmoid 函数，最初由统计学家用来描述生态学中人口增长的特性。sigmoid 函数是一种数学函数，用于将预测值映射到概率。逻辑回归有一个 S 形曲线，可以取 0 到 1 之间的值，但永远不会精确到这些极限。它有`1 / (1 + e^-value)`的公式。

![](img/ac479a03e25430169bdcc6f3895400ba.png)

Sigmoid 函数([来源](https://www.javatpoint.com/logistic-regression-in-machine-learning))

逻辑回归是线性回归模型的扩展。让我们用一个简单的例子来理解这一点。如果我们想要分类一封电子邮件是否是垃圾邮件，如果我们应用线性回归模型，我们将只得到 0 和 1 之间的连续值，如 0.4、0.7 等。另一方面，逻辑回归通过将阈值设置为 0.5 来扩展该线性回归模型，因此如果输出值大于 0.5，则数据点将被分类为垃圾邮件，如果输出值小于 0.5，则不是垃圾邮件。

这样就可以用逻辑回归对问题进行分类，得到准确的预测。

## 问题分析

为了在实际使用中应用逻辑回归模型，让我们考虑由三列组成的 DMV 测试数据集。前两列由两个 DMV 书面测试( ***DMV_Test_1*** 和 ***DMV_Test_2*** )组成，这两个测试是自变量，最后一列由因变量 ***结果*** 组成，这两个结果表示驾驶员已获得驾照(1)或未获得驾照(0)。

在这种情况下，我们必须使用这些数据建立一个逻辑回归模型，以预测参加了两次 DMV 笔试的司机是否会获得驾照，并使用他们在笔试中获得的分数对结果进行分类。

## 步骤 1:导入库

和往常一样，第一步总是包括导入库，即 NumPy、Pandas 和 Matplotlib。

```
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
```

## 步骤 2:导入数据集

在这一步中，我们将从我的 GitHub 存储库中以“DMVWrittenTests.csv”的形式获取数据集。变量 ***X*** 将存储两个 ***DMV 测试*** ，变量 ***Y*** 将最终输出存储为 ***结果******。***`dataset.head(5)`用于可视化前 5 行数据。

```
dataset = pd.read_csv('[https://raw.githubusercontent.com/mk-gurucharan/Classification/master/DMVWrittenTests.csv'](https://raw.githubusercontent.com/mk-gurucharan/Classification/master/DMVWrittenTests.csv'))X = dataset.iloc[:, [0, 1]].values
y = dataset.iloc[:, 2].valuesdataset.head(5)>>
DMV_Test_1   DMV_Test_2   Results
34.623660    78.024693    0
30.286711    43.894998    0
35.847409    72.902198    0
60.182599    86.308552    1
79.032736    75.344376    1
```

## 步骤 3:将数据集分为训练集和测试集

在这一步中，我们必须将数据集分为训练集和测试集，在训练集上将训练逻辑回归模型，在测试集上将应用训练模型对结果进行分类。其中的`test_size=0.25`表示*数据的 25%将作为 ***测试集*** 保存，剩余的 75%*将作为 ***训练集*** 用于训练。**

```
**from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.25, random_state = 0)**
```

## **步骤 4:特征缩放**

**这是一个附加步骤，用于对特定范围内的数据进行标准化。它也有助于加速计算。由于数据变化很大，我们使用此函数将数据范围限制在一个小范围内(-2，2)。例如，分数 62.0730638 被规范化为-0.21231162，分数 96.51142588 被规范化为 1.55187648。这样 X_train 和 X_test 的分数就归一化到一个更小的范围。**

```
**from sklearn.preprocessing import StandardScaler
sc = StandardScaler()
X_train = sc.fit_transform(X_train)
X_test = sc.transform(X_test)**
```

## **步骤 5:在训练集上训练逻辑回归模型**

**在这一步中，类`LogisticRegression`被导入并分配给变量 ***【分类器】*** 。`classifier.fit()`功能配有 **X_train** 和 ***Y_train*** 对模型进行训练。**

```
**from sklearn.linear_model import LogisticRegression
classifier = LogisticRegression()
classifier.fit(X_train, y_train)**
```

## **步骤 6:预测测试集结果**

**在这一步中，`classifier.predict()`函数用于预测测试集的值，这些值被存储到变量`y_pred.`**

```
**y_pred = classifier.predict(X_test) 
y_pred**
```

## **步骤 7:混淆矩阵和准确性**

**这是分类技术中最常用的一步。在这里，我们看到了训练模型的准确性，并绘制了混淆矩阵。**

**混淆矩阵是一个表，用于显示当测试集的真实值已知时，对分类问题的正确和错误预测的数量。它的格式如下**

**![](img/74881daa5f3eb9f2b0c834afe2b00c55.png)**

**来源—自己**

**真实值是正确预测的次数。**

```
**from sklearn.metrics import confusion_matrix
cm = confusion_matrix(y_test, y_pred)from sklearn.metrics import accuracy_score 
print ("Accuracy : ", accuracy_score(y_test, y_pred))
cm>>Accuracy :  0.88

>>array([[11,  0],
       [ 3, 11]])**
```

**从上面的混淆矩阵，我们推断，在 25 个测试集数据中，22 个被正确分类，3 个被错误分类。很好的开始，不是吗？**

## **步骤 8:将实际值与预测值进行比较**

**在这个步骤中，创建一个 Pandas DataFrame 来比较原始测试集( ***y_test*** )和预测结果( ***y_pred*** )的分类值。**

```
**df = pd.DataFrame({'Real Values':y_test, 'Predicted Values':y_pred})
df>> 
Real Values   Predicted Values
1             1
0             0
0             0
0             0
1             1
1             1
1             0
1             1
0             0
1             1
0             0
0             0
0             0
1             1
1             0
1             1
0             0
1             1
1             0
1             1
0             0
0             0
1             1
1             1
0             0**
```

**虽然这种可视化可能没有回归那么有用，但从这一点上，我们可以看到，该模型能够以 88%的正确率对测试集值进行分类，如上所述。**

## **步骤 9:可视化结果**

**在最后一步中，我们将逻辑回归模型的结果可视化在一个图表上，该图表与两个区域一起绘制。**

```
**from matplotlib.colors import ListedColormap
X_set, y_set = X_test, y_test
X1, X2 = np.meshgrid(np.arange(start = X_set[:, 0].min() - 1, stop = X_set[:, 0].max() + 1, step = 0.01),
                     np.arange(start = X_set[:, 1].min() - 1, stop = X_set[:, 1].max() + 1, step = 0.01))
plt.contourf(X1, X2, classifier.predict(np.array([X1.ravel(), X2.ravel()]).T).reshape(X1.shape),
             alpha = 0.75, cmap = ListedColormap(('red', 'green')))
plt.xlim(X1.min(), X1.max())
plt.ylim(X2.min(), X2.max())
for i, j in enumerate(np.unique(y_set)):
    plt.scatter(X_set[y_set == j, 0], X_set[y_set == j, 1],
                c = ListedColormap(('red', 'green'))(i), label = j)
plt.title('Logistic Regression')
plt.xlabel('DMV_Test_1')
plt.ylabel('DMV_Test_2')
plt.legend()
plt.show()**
```

**![](img/aa7d59d24d5a4b59c513d131b32b70cf.png)**

**逻辑回归**

**在该图中，值 ***1(即，是)*** 以“*红色绘制，值 ***0(即，否)*** 以“*绿色绘制。逻辑回归线将这两个区域分开。因此，具有给定的两个数据点(DMV_Test_1 和 DMV_Test_2)的任何数据都可以绘制在图上，并且根据属于哪个区域，结果(获得驾驶执照)可以分类为是或否。****

***根据上面的计算，我们可以看到测试集中有三个值被错误地归类为“否”,因为它们位于线的另一端。***

***逻辑回归***

## ***结论—***

***因此，在这个故事中，我们已经成功地建立了一个 ***逻辑回归*** 模型，该模型能够预测一个人是否能够通过笔试获得驾驶执照，并可视化结果。***

**我还附上了我的 GitHub 资源库的链接，你可以在那里下载这个 Google Colab 笔记本和数据文件供你参考。**

**[](https://github.com/mk-gurucharan/Classification) [## MK-guru charan/分类

### 这是一个由 Python 代码组成的知识库，用于构建不同类型的分类模型，以评估和…

github.com](https://github.com/mk-gurucharan/Classification) 

您还可以在下面找到该程序对其他分类模型的解释:

*   逻辑回归
*   k-最近邻(KNN)分类(即将推出)
*   支持向量机(SVM)分类(即将推出)
*   朴素贝叶斯分类(即将推出)
*   随机森林分类(即将推出)

在接下来的文章中，我们将会遇到更复杂的回归、分类和聚类模型。到那时，快乐的机器学习！**