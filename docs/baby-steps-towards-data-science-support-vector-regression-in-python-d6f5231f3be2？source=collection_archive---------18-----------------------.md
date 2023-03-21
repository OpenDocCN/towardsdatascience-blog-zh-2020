# 迈向数据科学的一小步:Python 中的支持向量回归

> 原文：<https://towardsdatascience.com/baby-steps-towards-data-science-support-vector-regression-in-python-d6f5231f3be2?source=collection_archive---------18----------------------->

## 理解支持向量回归背后的直觉，并用 python 实现。提供源代码和数据集。

# 什么是支持向量回归？

支持向量回归是一种特殊的回归，它为您提供了某种缓冲或误差灵活性。它是怎么做到的？我将通过展示两张不同的图表，用简单的术语向你解释。

![](img/ee3c841847dbabeb56ded5f4577f5106.png)

作者图片

以上是假设的线性回归图。你可以看到回归线画在一个最小平方误差的位置。误差基本上是原始数据点(黑色点)和回归线(预测值)之间距离差的平方。

![](img/e8c8ed750f75adc1327ea988e63d15d0.png)

作者图片

以上与 SVR(支持向量回归)的设置相同。您可以观察到回归线周围有两条边界。这是回归线上下垂直距离为ε的管。实际上，它被称为ε不敏感管。这个管子的作用是为误差创造一个缓冲。具体来说，该管内的所有数据点被认为与回归线的误差为零。计算误差时，只考虑该管外的点。误差计算为数据点到管边界的距离，而不是数据点到回归线的距离(如线性回归所示)

为什么选择支持向量？

管外的所有点都称为松弛点，它们本质上是二维空间中的向量。想象一下，画出从原点到各个松弛点的向量，你就可以看到图中所有的向量。这些向量支持该管的结构或形成，因此它被称为支持向量回归。你可以从下面的图表中理解它。

![](img/a2ce226e0b83a2dda1d3086d81ddc91c.png)

# 用 Python 实现

让我们深入研究 python，建立一个随机森林回归模型，并尝试预测一个 6.5 级别的员工的工资(假设)。

在您继续之前，请从我的 GitHub Gist 下载 CSV 数据文件。

```
[https://gist.github.com/tharunpeddisetty/3fe7c29e9e56c3e17eb41a376e666083](https://gist.github.com/tharunpeddisetty/3fe7c29e9e56c3e17eb41a376e666083)
Once you open the link, you can find "Download Zip" button on the top right corner of the window. Go ahead and download the files.
You can download 1) python file 2)data file (.csv)
Rename the folder accordingly and store it in desired location and you are all set.If you are a beginner I highly recommend you to open your python IDE and follow the steps below because here, I write detailed comments(statements after #.., these do not compile when our run the code) on the working of code. You can use the actual python as your backup file or for your future reference.
```

***导入库***

```
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
```

***导入数据并定义 X 和 Y 变量***

```
dataset = pd.read_csv(‘/Users/tharunpeddisetty/Desktop/Position_Salaries.csv’) #add your file pathX = dataset.iloc[:,1:-1].values
y = dataset.iloc[:, -1].values#iloc takes the values from the specified index locations and stores them in the assigned variable as an array
```

**让我们看看我们的数据，了解变量:**

![](img/2e7b6c01f37c646b6c4973e7acef3f3f.png)

该数据描述了员工的职位/级别及其工资。这与我在决策树回归文章中使用的数据集相同。

***特征缩放***

```
#Feature Scaling. Required for SVR. Since there’s no concept of coefficients
print(y) 
#we need to reshape y because standard scaler class expects a 2D array
y=y.reshape(len(y),1)from sklearn.preprocessing import StandardScaler
sc_X = StandardScaler()
sc_y = StandardScaler()
X= sc_X.fit_transform(X)
# create a new sc object because the first one calcualtes the mean and standard deviation of X. We need different values of mean and standard deviation for Y
y= sc_y.fit_transform(y) print(y)
```

SVR 中没有类似线性回归的系数概念，因此为了减少高值特征的影响，我们需要对特征进行缩放，或者换句话说，在一个缩放比例下获取所有值。我们通过标准化价值观来实现这一目标。因为在这个例子中我们只有一个特性，所以无论如何我们都要应用它。我们使用 sklearn 的 StandardScaler()函数来实现。但是，对于其他数据集，不要忘记缩放所有要素和因变量。此外，请记住对 Y(因变量即薪水)进行整形，这纯粹是为了通过 python 中的标准缩放器来传递它。

***训练 SVR 模型***

```
from sklearn.svm import SVR
regressor = SVR(kernel = 'rbf')
regressor.fit(X, y)
```

很简单，不是吗？我们将使用径向基函数作为支持向量回归算法的核心。这意味着我们使用一个叫做“rbf”的函数来将数据从一个空间映射到另一个空间。解释这是如何工作的超出了本文的范围。但是，你可以在网上研究一下。核函数的选择随着数据的分布而变化。我建议你在用 python 实现这个基本程序后研究一下它们。

***可视化 SVR 回归的结果***

```
X_grid = np.arange(min(sc_X.inverse_transform(X)), max(sc_X.inverse_transform(X)), 0.1)
X_grid = X_grid.reshape((len(X_grid), 1))
plt.scatter(sc_X.inverse_transform(X), sc_y.inverse_transform(y), color = 'red')
plt.plot(X_grid, sc_y.inverse_transform(regressor.predict(sc_X.transform(X_grid))), color = 'blue')
plt.title('Support Vector Regression')
plt.xlabel('Position level')
plt.ylabel('Salary')
plt.show()
```

![](img/b7202aecddf7cb67d9c6386579dd1a5d.png)

作者图片

你可以看到这个模型是如何符合数据的。你认为它做得很好吗？将这一结果与我以前文章中对相同数据进行的其他回归进行比较，您就可以看到差异，或者等到本文结束时再看。

***使用决策树回归预测 6.5 级结果***

```
print(sc_y.inverse_transform(regressor.predict(sc_X.transform([[6.5]])))
)
#We also need to inverse transform in order to get the final result
```

确保将所有转换应用为初始数据的转换，以便模型更容易识别数据并生成相关结果。

***结果***

让我总结一下各种回归模型的所有结果，以便于我们进行比较。

支持向量回归机:17560 . 486868686686

随机森林回归:167000(输出不是代码的一部分)

决策树回归:150000(输出不是代码的一部分)

多项式线性回归:158862.45(输出不是代码的一部分)

线性回归预测:330378.79(输出不是代码的一部分)

# 结论

你面前有数据。现在，作为一名经理，自己做决定。你会给一个 6.5 级的员工多少薪水(把级别看作是工作经验的年限)？你看，在数据科学中没有绝对的答案。我不能说 SVR 比其他模型表现得更好，所以它是预测工资的最佳模型。如果你问我的想法，我觉得随机森林回归的预测结果比 SVR 更真实。但是，这也是我的感觉。请记住，很多因素都会发挥作用，如员工的职位、该职位在该地区的平均工资以及员工以前的工资等。所以，如果我说随机森林结果是最好的，你也不要相信我。我只说比别人更现实。最终的决定取决于组织的商业案例，而且没有一个完美的模型可以完美地预测员工的工资。

恭喜你！您已经用最少的代码行实现了支持向量回归。现在您有了代码的模板，您可以在其他数据集上实现它并观察结果。这标志着我关于回归的文章的结束。下一站是分类模型。感谢阅读。机器学习快乐！