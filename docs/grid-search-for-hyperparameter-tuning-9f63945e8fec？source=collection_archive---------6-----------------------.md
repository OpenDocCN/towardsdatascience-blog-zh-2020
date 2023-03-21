# 超参数调谐的网格搜索

> 原文：<https://towardsdatascience.com/grid-search-for-hyperparameter-tuning-9f63945e8fec?source=collection_archive---------6----------------------->

![](img/e6c0aa1acef79f8de12affdf5df4a6e1.png)

[图像来源](https://www.flickr.com/photos/gedankenstuecke/186651353/)

各位机器学习爱好者好！由于实习的原因，我已经有一段时间没有在 Medium 上写东西了。当我在做我的项目时，我面临着一种情况，我需要用不同的超参数来尝试不同的分类器。我发现每次都很难手动更改超参数并使它们适合我的训练数据。原因如下:

*   这很费时间
*   很难跟踪我们尝试过的超参数，我们仍然必须尝试

因此，我很快询问谷歌是否有解决我的问题的方法，谷歌向我展示了来自 Sklearn 的名为**的东西。让我用一个简单的例子来分享我是如何利用这个 GridSearchCV 来解决我的问题的。**

## 什么是 GridSearchCV？

GridSearchCV 是一个库函数，是 sklearn 的 model_selection 包的成员。它有助于遍历预定义的超参数，并使您的估计器(模型)适合您的训练集。因此，最后，您可以从列出的超参数中选择最佳参数。

除此之外，您可以为每组超参数指定交叉验证的次数。

## 带解释的示例

```
from sklearn.model_selection import GridSearchCV
from sklearn.neighbors import KNeighborsClassifierkn = KNeighborsClassifier()params = {
    'n_neighbors' : [5, 25],
    'weights': ['uniform', 'distance'],
    'algorithm': ['auto', 'ball_tree', 'kd_tree', 'brute']
}grid_kn = GridSearchCV(estimator = kn,
                        param_grid = params,
                        scoring = 'accuracy', 
                        cv = 5, 
                        verbose = 1,
                        n_jobs = -1)grid_kn.fit(X_train, y_train)
```

我们来分解一下上面的代码块。像往常一样，您需要从 sklearn 库中导入 **GridSearchCV** 和**估计器**/模型(在我的例子中是 KNClassifier)。

下一步是定义您想要尝试的超参数。这取决于你选择的评估者。您所需要做的就是创建一个字典(我的代码中的变量 params ),它将超参数作为键，并有一个 iterable 来保存您需要尝试的选项。

然后你要做的就是创建一个 GridSearchCV 的对象。这里基本上需要定义几个命名参数:

1.  **估计器**:您创建的估计器对象
2.  **params_grid** :保存您想要尝试的超参数的字典对象
3.  **评分**:您想要使用的评估指标，您可以简单地传递一个有效的评估指标字符串/对象
4.  **cv** :您必须为每组选定的超参数尝试交叉验证的次数
5.  **verbose** :您可以将它设置为 1，以便在将数据放入 GridSearchCV 时得到详细的打印结果
6.  **n_jobs** :如果 it -1 将使用所有可用的处理器，您希望为该任务并行运行的进程数。

这些都是你需要定义的。然后你要像平时一样拟合你的训练数据。您将得到如下所示的第一行:

```
Fitting 5 folds for each of 16 candidates, totalling 80 fits
...
...
...
```

你不明白这是什么意思吗？简单！因为我们必须为 n_neighbors 尝试两个选项，两个用于权重，四个用于算法，所以总共有 16 种不同的组合我们应该尝试。对于每个组合，我们有 5 个 CV 拟合，因此我们的 GridSearcCV 对象将测试 80 个不同的拟合。

这种适合的时间取决于你正在尝试的超参数的数量。一旦一切完成，您将得到如下输出:

```
[Parallel(n_jobs=1)]: Done  80 out of  80 | elapsed: 74.1min finished]
```

然后要知道什么是最好的参数，你可以简单地打印出来

```
# extract best estimator
print(grid_kn.best_estimator_)Output:
KNeighborsClassifier(algorithm='auto', 
leaf_size=30, metric='minkowski',metric_params=None, n_jobs=-1, n_neighbors=25, p=2, weights='distance')# to test the bestfit
print(grid_kn.score(X_test, y_test))Output:
0.9524753
```

很酷不是吗？现在你所要做的就是改变估计量，定义一个你必须尝试的超参数字典。希望这对你有帮助。要了解更多关于 GridSearchCV 的内容，请查看 sklearn 的官方文档。