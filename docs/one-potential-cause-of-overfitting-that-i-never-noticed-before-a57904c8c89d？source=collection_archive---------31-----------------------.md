# 我以前从未注意到的过度拟合的一个潜在原因

> 原文：<https://towardsdatascience.com/one-potential-cause-of-overfitting-that-i-never-noticed-before-a57904c8c89d?source=collection_archive---------31----------------------->

## 机器学习

## 当训练数据中的性能比测试数据中的性能好得多时，就会发生过度拟合。机器学习包中的默认超参数可能会让你陷入过度拟合的问题。

![](img/ba5796017a189deb2bdef5028739f104.png)

[乔·塞拉斯](https://unsplash.com/@joaosilas?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

> “为什么我调好的模型还是过拟合？”
> 
> "你使用交叉验证了吗？"
> 
> “当然可以。”
> 
> "你用的是什么模型，你调的超参数是什么？"
> 
> "随机森林回归器和我调整了每棵树的树数和最大特征数."
> 
> “我需要检查你的代码。”

这是我和妻子在做一个迷你项目时的对话。

老实说，根据她的描述，我想不出程序中有什么错误。然而，在我一行一行地检查她的代码后，我发现了导致过度拟合的问题，这是我以前从未想到过的。

我们一起来解剖一下她的代码。

这是在训练数据集中调整模型的代码块，也是问题发生的*。*

```
*def train_pipeline_rf(X,y):
    # X are factors
    # y is output
    # impute missing X by median
    X_prepared = pre_pipeline.fit_transform(X)
    # set cross-validation
    tscv = TimeSeriesSplit(n_splits=10)
    data_split = tscv.split(X_prepared)
    # hyper-parameter space
    param_grid_RF = {
        'n_estimators' : [10,20,50,100,200,500,1000],
        'max_features' : [0.6,0.8,"auto","sqrt"]
    }
    # build random forest model
    rf_model = RandomForestRegressor(random_state=42,n_jobs=-1)
    # gridsearch for the best hyper-parameter
    gs_rf = GridSearchCV(rf_model, param_grid=param_grid_RF, cv=data_split, scoring='neg_mean_squared_error', n_jobs=-1)
    # fit dataset
    gs_rf.fit(X_prepared, y)
    return gs_rf*
```

## *预处理*

*代码中的 ***pre_pipeline*** 是用于缺失值插补和特征缩放的*流水线*，两者都是数据*预处理*中必不可少的步骤。它看起来是这样的:*

```
*pre_pipeline = Pipeline([
        ('imputer', SimpleImputer(strategy="median")),
        ('std_scaler', StandardScaler()),
    ])*
```

*她在这里使用的 [*估算器*](https://scikit-learn.org/stable/modules/preprocessing.html#imputation-of-missing-values) 是用列中值替换 ***NA*** 值，而 [*定标器*](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html#sklearn.preprocessing.StandardScaler) 是标准定标器，它将列归一化为:*

> **z = (x — u) / s**

*其中 ***u*** 为均值， ***s*** 为列[1]的标准差。也可以使用[***sk learn***](https://scikit-learn.org/stable/modules/preprocessing.html)包中的其他一些 ***估算器*** 和 ***缩放器*** 。但这部分与过拟合问题无关。*

## *在交叉验证中设置数据拆分*

```
 *# set cross-validation
    tscv = TimeSeriesSplit(n_splits=10)
    data_split = tscv.split(X_prepared)*
```

*这里她使用了一种专门为时序数据设计的方法，[](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.TimeSeriesSplit.html)****。*****

**通常，人们使用 [k-fold](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.KFold.html) 交叉验证来随机分割训练数据，或者使用[分层 k-fold](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.StratifiedKFold.html) 交叉验证来保留每个类别的样本百分比[2]。但是这两种方法不适合时间序列数据，因为它们没有保持数据点的原始顺序。**

**这一步非常标准，与过度拟合问题无关。**

## **超参数空间和定义模型**

```
 **# hyper-parameter space
    param_grid_RF = {
        'n_estimators' : [10,20,50,100,200,500,1000],
        'max_features' : [0.6,0.8,"auto","sqrt"]
    }
    # build random forest model
    rf_model = RandomForestRegressor(random_state=42,n_jobs=-1)**
```

**关于超参数调优，要给出一组候选，通常在[***sk learn***](https://scikit-learn.org/stable/modules/preprocessing.html)包中定义为字典格式。超参数的名称是从模型中建立的，例如，我妻子的代码中的[***【RandomForestRegressor】***](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestRegressor.html)。**

**所谓的*“tuning-hyperparameter”*步骤是在交叉验证过程中，从你给定的候选项中选择出优于其他超参数组合的最佳超参数组合。**

****这是导致过度拟合问题的零件。****

## **解决过拟合问题。**

**如果参考 ***sklearn*** 中随机森林回归器的手册页，可以看到函数[***RandomForestRegressor***](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestRegressor.html)*(此处未列出，有兴趣请参考网页) ***。******

**然而，在上面的代码中，在调优过程中，只有 ***n_estimators*** 和 ***max_features*** 被传递给函数，这导致所有其他参数都采用默认的**值。****

**函数中有一个参数叫做 ***max_depth*** ，是你的随机森林模型中[树的最大深度。它的缺省值是“无”，这意味着决策树将深入到每个叶子是纯的或者所有叶子最多有 m 个样本的深度](https://en.wikipedia.org/wiki/Random_forest)[，其中 m 由 ***min_samples_split(缺省值= 2)*** 定义。](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestRegressor.html)**

**该设置显著增加了训练模型的复杂性。并且基于 [*偏倚和方差*](https://en.wikipedia.org/wiki/Bias%E2%80%93variance_tradeoff) 之间的权衡，在上面的代码中训练的模型具有低偏倚和高方差。当然最后导致了过拟合问题。**

**我通过添加 ***max_depth*** 到超参数空间解决了这个问题。**

```
 **# hyper-parameter space
    param_grid_RF = {
        'n_estimators' : [10,20,50,100,200,500,1000],
        'max_features' : [0.6,0.8,"auto","sqrt"],
        'max_depth' : [4,5,6]
    }**
```

**其实[***RandomForestRegressor***](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestRegressor.html)中还有一些其他参数可以影响模型的复杂度，但不建议全部放入超参数空间。**

**上面代码中额外的超参数已经将计算成本增加了三倍。因此，仅仅调整更多参数而成倍增加运行时间**是不值得的。****

**相反，您可以像这样在模型初始化中设置它们中的一些。**

```
**# hyper-parameter space
    param_grid_RF = {
        'n_estimators' : [10,20,50,100,200,500,1000]
    }
# build random forest model
    rf_model = RandomForestRegressor(random_state=42,n_jobs=-1,max_features=0.6,max_depth=5)**
```

**在上面的代码中，唯一调整过的超参数是****max _ features、*** 以及 ***max_depth、*** 在训练过程中是固定的。调谐的或固定的参数不必如此。***

**当 ***已经*** 对一些超参数的选择有所了解时，设置的基本原理是减少计算成本。**

**是的，我妻子的问题已经解决了。然而，老实说，我不可能找到它**，直到我检查了软件包中所有的默认参数。**这就是为什么我想与你分享这个经验，以便你可以在你的机器学习模型训练过程中更加小心默认参数。**

***过拟合*是数据科学中最常见的问题之一，主要来源于模型的 ***高复杂度*** 和数据点的*缺失。***

***为了避免它，最好完全控制你使用的软件包。***

***希望这条建议能帮到你。***

***![](img/f82b5765fefe03d8a5c78205fa47aee0.png)***

***Joshua Sortino 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片***

## ***参考资料:***

1.  ***[https://sci kit-learn . org/stable/modules/generated/sk learn . preprocessing . standard scaler . html # sk learn . preprocessing . standard scaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html#sklearn.preprocessing.StandardScaler)***
2.  ***[https://sci kit-learn . org/stable/modules/generated/sk learn . model _ selection。StratifiedKFold.html](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.StratifiedKFold.html)***
3.  ***[https://en.wikipedia.org/wiki/Random_forest](https://en.wikipedia.org/wiki/Random_forest)***

## ***更正:***

***我要感谢 Antonio Carlos 在我最初的帖子中指出了这个问题。他还建议了一个不错的帖子“[用管道预处理数据，以防止交叉验证过程中的数据泄漏](/pre-process-data-with-pipeline-to-prevent-data-leakage-during-cross-validation-e3442cca7fdc)”到题。我真的很感激。***

***它与 ***预处理*** 部分的流水线有关。它不应该在整个训练数据集上进行，而应该在交叉验证步骤的每次迭代中进行，因为训练数据的整体归一化将导致 CV 步骤中训练和验证数据集之间的泄漏。***

**因此，我将相应的修正代码放在下面:**

```
**def train_pipeline_rf(X,y):
    # X are factors
    # y is output
    # set cross-validation
    tscv = TimeSeriesSplit(n_splits=10)
    data_split = tscv.split(X)

    # build a pipeline of pre-processing and random forest model
    my_pipe = Pipeline([
        ('imputer', SimpleImputer(strategy="median")), 
        ('std_scaler', StandardScaler()),
        ('rf_model', RandomForestRegressor(random_state=42,n_jobs=-1))
    ]) # hyper-parameter space
    param_grid_RF = {
        'rf_model__n_estimators' : [10,20,50,100,200,500,1000],
        'rf_model__max_features' : [0.6,0.8,"auto","sqrt"],
        'rf_model__max_depth' : [4,5,6]
    }
    # gridsearch for the best hyper-parameter within the pipeline.
    gs_rf = GridSearchCV(my_pipe, param_grid=param_grid_RF, cv=data_split, scoring='neg_mean_squared_error', n_jobs=-1)
    # fit dataset
    gs_rf.fit(X, y)
    return gs_rf**
```