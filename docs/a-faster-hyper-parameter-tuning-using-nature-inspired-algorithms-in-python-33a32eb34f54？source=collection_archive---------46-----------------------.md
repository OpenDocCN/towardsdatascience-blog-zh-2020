# Python 中使用自然启发算法的快速超参数调整

> 原文：<https://towardsdatascience.com/a-faster-hyper-parameter-tuning-using-nature-inspired-algorithms-in-python-33a32eb34f54?source=collection_archive---------46----------------------->

## Bat 算法与网格搜索的性能比较

受自然启发的算法非常强大，通常用于解决 NP 难问题(如旅行推销员问题)或其他计算量大的任务。它们也被称为**优化算法。**受自然启发的算法试图找到问题的最佳解决方案，然而**并不保证会找到最佳解决方案**。

超参数调整通常使用网格搜索或随机搜索来完成。*网格搜索*的问题在于它非常昂贵，因为它尝试了所有可能的参数组合。*随机搜索*会尝试一定数量的随机参数组合。它不太可能找到参数的最佳组合，但是，它比网格搜索要快得多。

![](img/dbd9af138f791473829f92e715826fa3.png)

蝙蝠的**回声定位**是蝙蝠算法的基础部分(杨新社 2010)。照片由 [Igam Ogam](https://unsplash.com/@igamogam) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上拍摄

接下来是**自然启发算法**。它们比网格搜索更快，并且有可能找到最佳解决方案——超参数的最佳组合。然而，算法与算法之间的结果会有所不同(有许多受自然启发的算法，例如:[蝙蝠算法](https://en.wikipedia.org/wiki/Bat_algorithm)、[萤火虫算法](https://en.wikipedia.org/wiki/Firefly_algorithm)……)，这些算法也有自己的参数，这些参数控制它们如何搜索解决方案。如果您知道这些算法是如何工作的，您可能希望设置这些参数来改进搜索过程。

# 使用自然启发的算法开始超参数调谐

如果您在您的机器学习项目中使用 [Scikit-Learn](https://scikit-learn.org/) ，您可以使用一个名为 [**Sklearn 自然启发算法**](https://github.com/timzatko/Sklearn-Nature-Inspired-Algorithms) 的 python 库，它将允许您使用自然启发算法进行超参数调整。我们将在本教程中使用这个库，通过 pip 安装它。

```
pip install sklearn-nature-inspired-algorithms
```

假设我们想要优化我们的*随机森林分类器*的参数。

```
from sklearn.ensemble import RandomForestClassifierclf = RandomForestClassifier(random_state=42)
```

现在我们需要定义我们将尝试的参数集，用法类似于 scikit-learn 的 [GridSearchCV](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html) 。这组参数总共有 216 种不同的组合。

```
param_grid = {
   'n_estimators': range(10, 80, 20),
   'max_depth': [2, 4, 6, 8, 10, 20],
   'min_samples_split': range(2, 8, 2),
   'max_features': ["auto", "sqrt", "log2"]
}
```

此外，我们需要一个数据集。我们将使用 *make_classification 人工创建一个。*

```
from sklearn.model_selection import train_test_split
from sklearn.datasets import make_classificationX, y = make_classification(n_samples=1000, n_features=10, class_sep=0.8, n_classes=2)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)print(f'train size - {len(X_train)}\ntest size - {len(X_test)}')
```

**现在我们可以使用自然启发的算法进行超参数调整。**我们正在使用 [Bat 算法](https://en.wikipedia.org/wiki/Bat_algorithm)进行优化。我们将训练 25 个个体的种群规模，如果算法在 10 代内找不到更好的解决方案，我们将停止该算法。我们将这样做 5 次，并将使用 5 倍交叉验证。*要了解关于 NatureInspiredSearchCV 参数的更多信息，请参考其* [*文档*](https://sklearn-nature-inspired-algorithms.readthedocs.io/en/stable/introduction/nature-inspired-search-cv.html) *，您也可以使用 Bat 算法之外的其他算法。*

```
from sklearn_nature_inspired_algorithms.model_selection import NatureInspiredSearchCVnia_search = NatureInspiredSearchCV(
    clf,
    param_grid,
    cv=5,
    verbose=1,
    algorithm='ba',
    population_size=25,
    max_n_gen=100,
    max_stagnating_gen=10,    
    runs=5,
    scoring='f1_macro',
    random_state=42,
)

nia_search.fit(X_train, y_train)
```

花费了一些时间，大约 1 分钟(GridSearch 大约需要 2 分钟，参数网格越大，差异越大)。现在，您可以用找到的最佳参数来拟合您的模型。最佳参数存储在*nia _ search . best _ params _*中。

```
from sklearn.metrics import classification_reportclf = RandomForestClassifier(**nia_search.best_params_, random_state=42)

clf.fit(X_train, y_train)

y_pred = clf.predict(X_test)

print(classification_report(y_test, y_pred, digits=4))
```

现在，您已经使用受自然启发的算法选择的最佳参数成功训练了模型。这是完整的例子。

我还做了一个 *GridSearchCV* 和 NatureInspiredSearchCV 的比较(在本[笔记本](https://github.com/timzatko/Sklearn-Nature-Inspired-Algorithms/blob/master/examples/notebooks/hyper_parameter_tuning_nia_vs_grid_search.ipynb))。我使用了更大的数据集和更多的超参数(总共 1560 个组合)。*natureinspiredsearchv***找到了与 *GridSearchCV* 相同的解决方案**，并且**比**快了 4.5 倍！花了***GridSearchCV*2h 23 分 44 秒**找到了**最佳**方案，***natureinspiredsearchv*在 31 分 58 秒**找到了。

受自然启发的算法非常强大，在超参数调整方面优于网格搜索，因为它们能够更快地找到相同的解决方案(或非常接近)。在本教程中，我们使用 Bat 算法，其默认参数由 [Sklearn 自然启发的算法](https://github.com/timzatko/Sklearn-Nature-Inspired-Algorithms)库设置，但也有许多其他自然启发的算法可用于超参数调整。