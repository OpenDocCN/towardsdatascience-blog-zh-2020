# 使用 imblearn 管道中的自定义采样器丰富您的火车文件夹

> 原文：<https://towardsdatascience.com/enrich-your-train-fold-with-a-custom-sampler-inside-an-imblearn-pipeline-68f6dff964bf?source=collection_archive---------39----------------------->

## 在交叉验证中使用扩充数据并不像看起来那么简单。这是如何在 sklearn 中做到的。

![](img/0311199116d6fe7bce38fed5c6e48434.png)

艾德亚多·桑奇兹在 Unsplash 上拍摄的照片

当涉及到小数据集时，生活会变得复杂。在医学中，一个数据集很容易包含不到 100 个患者/行。但是在另一个维度上，它可以变得非常大——轻松超过 3000 个特征。

但是，有时您会找到扩充数据的方法，在我的例子中，这意味着您将数据集乘以稍微不同的特征值。这样你就可以增加你的训练数据。当然这是我真正做的事情的简化版本，但那是另外一个故事了。有不同的方法来扩充您的数据，但是本文并不打算涵盖数据扩充的广泛领域。

但你必须小心，数据增强是一种强大的武器，必须谨慎使用。即使正确使用，也不能保证提高估计器的性能。

顺便说一句，如果没有我的同事和 StackOverflow 的人的帮助，我是写不出这篇文章的！

## 在您的流程中何处使用增强数据

一旦你有了一组扩充的数据来丰富你的原始数据集，你就会问自己如何以及在哪个点上合并它们。通常，您使用 sklearn 及其模块来评估您的估计器或搜索最佳超参数。包括`RandomizedSearchCV` 或`cross_validate` 在内的流行模块可以选择通过类似`KFold`的交叉验证方法。通过利用交叉验证方法来测量估计器的性能，您的数据被分成一个训练集和一个测试集。这是在 sklearn 方法下动态完成的。

这通常是好的，这意味着你不需要过多的麻烦。当您想要将扩充数据用于交叉验证方法时，只有一个问题——您不希望在您的测试文件夹中有扩充数据。这是为什么呢？你想知道你的估计器在现实中表现如何，而你的增广数据并没有反映现实。此外，您希望只增加训练集中的数据，而不希望在训练文件夹中增加数据。

## 如何只在需要扩充的地方扩充

回到问题，在你的交叉验证中，有没有可能影响训练测试拆分？是的，`imblearn.pipeline.Pipeline` 来救援了。这个管道类似于你可能从 sklearn 了解到的管道，你可以在一个所谓的管道中链接处理步骤和估计器。对我们来说，**的巨大差异和优势在于它在交叉验证中的工作方式。它只在列车上运行！**

这是一个好消息，但是您仍然需要定义一个方法来丰富您的原始数据并在管道中传递它。最简单的方法是使用 imblearn 的`FunctionSampler` 将任何函数转换成可以在管道中传递的采样器。imblearn 网站上有大量的文档。但是，也许你想做一些更复杂的事情，并建立自己的采样器。这就是 sklearn 的`BaseEstimator` 发挥作用的地方，它是一个定制估算器的基类。我们可以用它来构建我们的采样器。

```
import pandas as pd
from sklearn.base import BaseEstimatorclass EnrichWithAugmentedData(BaseEstimator):
    """
    Resampler to pass augmented data in a imblearn pipeline
    In this example I pass a list of augmented data frames with identical endpoints y to be merged with the original data X
    """

    def __init__(self, augmented_sets):
        self.augmented_sets = augmented_sets

    def fit_resample(self, X, y):
        return self.resample(X, y)

    def resample(self, X, y):
        self.data = []
        for i, df in enumerate(self.augmented_sets):
            self.data.append(self.augmented_sets[i].copy())
        for i, df in enumerate(self.data):
            self.data[i] = self.data[i].loc[X.index, :]
        X = pd.concat([X, *self.data], axis=0)
        n = len(self.data) + 1
        y = pd.concat([y]*n, axis=0)
        return X, y# Feel free to comment my code, I am a physicist :D
```

现在，我们可以使用我们的采样器来构建具有任何估计器的管道:

```
from xgboost import XGBClassifier
from imblearn.pipeline import Pipelineaugmented_sets = [df_augmented_01, df_augmented_02]model = XGBClassifier()
ewad = EnrichWithAugmentedData(augmented_sets)
pipeline = Pipeline([('ewad', ewad), ('model', model)])
```

假设我们想利用管道中的`RepeatedStratifiedKFold` 进行一次`RandomizedSearchCV`:

```
cv = RepeatedStratifiedKFold(
    n_splits=5, 
    n_repeats=10, 
    random_state=101
    )# make sure that you have the suffix with the name of your pipeline step in front of your parameter name!
parameter_grid = {
    'model__max_depth':[2, 3, 4],
    'model__learning_rate':np.arange(0.005, 0.5, 0.05),
}# make sure that you do not refit, because the refit will be without your augmented data!
gs = RandomizedSearchCV(
    estimator=pipeline, 
    n_iter=3000,
    param_distributions=parameter_grid, 
    scoring='roc_auc', 
    n_jobs=-1, 
    cv=cv, 
    verbose=1,
    random_state=101,
    refit=False
)
grid_result = gs.fit(X, y)print("Best: {:.2f} using {}".format(
    grid_result.best_score_, 
    grid_result.best_params_
))
```

您也可以用`cross_validate`使用相同的程序，用`pipeline.steps[1][1]`获得最佳模型和特征重要性，作为特征选择方法。如果您使用扩充数据进行训练，那么它很有可能会影响评估者选择的特征。

我希望这篇文章能帮助一些数据科学家同行。