# 隔离森林和 Pyspark 第 2 部分

> 原文：<https://towardsdatascience.com/isolation-forest-and-pyspark-part-2-76f7cd9cee56?source=collection_archive---------28----------------------->

## 经验教训

![](img/182db3296fb95ef6458f435f5d46573b.png)

调试 PySpark 和隔离森林—图片由作者提供

因此，在使用隔离森林的 PySpark ml 实现运行了几次之后，我偶然发现了一些事情，我想我应该写下来，这样你就不会浪费我在故障诊断上浪费的时间。

# 只有密集向量

在上一篇文章中，我使用了`VectorAssembler`来收集特征向量。碰巧我的测试数据只创建了`DenseVectors`，但是当我在不同的数据集上尝试这个例子时，我意识到:

*   `**VectorAssembler**`可以在同一个数据帧中创建**密集和稀疏向量**(这很聪明，其他 spark ml 算法可以利用它并使用它)
*   `**Isolation Forest**` (或者至少实现发现[这里的](https://github.com/titicaca/spark-iforest) ) **不支持上面的**，所以输入必须只有`DenseVectors`。

要演示该问题:

目前的解决方法是将所有矢量转换为密集矢量，不幸的是使用了 udf。

# 避免 OOM 和执行者的沟通问题

> 为大型数据集设置**approxquantilerrelativeerror***或* ***阈值*** 参数

*如果你计划在一个大数据集上进行训练，比如超过 10M 行，即使你设置了`maxSamples`和`maxFeatures`这样的参数来降低维数，你也需要将`approxQuantileRelativeError`参数设置得合理一些，比如`0.2`。原因是`approxQuantile`函数是一个使用起来非常昂贵的函数，尤其是如果我们期望`approxQuantileRelativeError`是`0.`(这是默认值)。这样做，大大减少了训练时间和 OOM 与执行者的沟通问题，而且，到目前为止，我还没有看到预测准确性的任何下降。*

*从实施的评论来看:*

```
*** The proportion of outliers in the data set (0< contamination < 1).
* It will be used in the prediction. In order to enhance performance,
* Our method to get anomaly score threshold adopts DataFrameStsFunctions.approxQuantile,
* which is designed for performance with some extent accuracy loss.
* Set the param approxQuantileRelativeError (0 < e < 1) to calculate
* an approximate quantile threshold of anomaly scores for large dataset.**
```

*或者，在 fit 中预先设置`threshold`并避免`approxQuantile`计算，如下所示:*

```
*model = iforest.fit(df, {**'threshold'**: 0.5})
print(model.getThreshold())> 0.49272560194039505*
```

*希望这对您有所帮助并节省一些时间:)如果您有任何建议、更正或想法，请告诉我。*

## *在此阅读有关如何将隔离林与 pyspark 一起使用的更多信息:*

*[](/isolation-forest-and-spark-b88ade6c63ff) [## 隔离森林和火花

### PySpark 隔离带的主要特点和使用方法

towardsdatascience.com](/isolation-forest-and-spark-b88ade6c63ff) 

# 接下来去哪里？

理解模型的预测:

[](https://medium.com/mlearning-ai/machine-learning-interpretability-shapley-values-with-pyspark-16ffd87227e3) [## 机器学习的可解释性——带有 PySpark 的 Shapley 值

### 解读隔离森林的预测——不仅仅是

medium.com](https://medium.com/mlearning-ai/machine-learning-interpretability-shapley-values-with-pyspark-16ffd87227e3)*