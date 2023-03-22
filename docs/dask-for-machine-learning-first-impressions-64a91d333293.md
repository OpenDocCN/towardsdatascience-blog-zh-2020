# 机器学习的 Dask 第一印象

> 原文：<https://towardsdatascience.com/dask-for-machine-learning-first-impressions-64a91d333293?source=collection_archive---------50----------------------->

## 一种在服务器集群中扩展类似 numpy 的阵列、类似 pandas 的数据帧和 scikit-learn 模型的方法

出于研究大数据的不同机器学习工具的好奇心，我上周花了一些时间来熟悉 [Dask](https://dask.org/) 和 [Dask-ML](https://dask-ml.readthedocs.io/en/latest/index.html) ，这是另一种在计算机集群上并行运行机器学习算法的替代工具。以下是对它的一些第一反应的简要总结。

![](img/75c1b610f510dfcbde3eccdcee2a4f30.png)

图片来源:来自 pexels.com 的[的免费库存照片](https://www.pexels.com/photo/abstract-blackboard-bulb-chalk-355948/)

# 达斯克将军

Dask 是一个相当年轻的框架，根据官方记录，它的开发似乎在 2015 年左右就开始了。但它是由一个非常受信任的 NumPy、Pandas、Jupyter 和 Scikit-Learn 开发人员社区维护的，背后有强大的机构支持，并且它在机器学习从业者社区中获得了相当大的关注。

虽然我学习 Dask 的个人动机与机器学习有关，但 Dask 本身更普遍地是一个平台，它将帮助您在一个计算机节点集群上并行处理数据。对于一个有 python 经验的数据科学家或数据工程师来说，设计逻辑和结构看起来非常好，很容易掌握。如果你有任何使用 Hadoop MapReduce、Apache Spark 或 TensorFlow 的经验，那么你会立即认识到有向非循环任务图的惰性分布式执行的熟悉概念。如果您对 python 的 concurrent.futures 模块有任何经验，那么您会认识到正在使用的另一种模式。但对于 numpy/pandas/sklearn 用户来说，这一切都变得更容易理解、更容易和更自然，其数组和数据帧有效地将 numpy 的数组和 pandas 的数据帧纳入一个计算机集群。虽然在看起来非常熟悉的数组和数据帧的层次上学习可能感觉很容易，但是不仅直接进入 Dask-ML，而且熟悉所有一般层次的 Dask 概念和 API 也是值得的。

# Dask-ML

Dask-ML 用两种不同的策略来处理机器学习任务的扩展:

*   **Dask-ML + scikit-learn 组合。**您可以使用 Dask 来跨集群扩展并行化的 scikit-learn 估计器和其他算法(具有 n_jobs 参数的算法)。Scikit-learn 本身能够在依赖 [Joblib](https://joblib.readthedocs.io/en/latest/) 的多核单台机器上并行化其大量算法，通过 Joblib，Dask 提供了一个替代后端，能够跨计算机集群执行并行作业。当处理的数据集和模型适合于单个节点内存时，这是一个强大而合适的设置，但是运行训练和预测对 CPU 来说太重，实际上无法在单台计算机上运行。
*   **估计器和其他算法的 Dask-ML 实现。**这适用于不仅单节点 CPU 太慢，而且单节点内存太少无法加载整个数据集的用例。Dask-ML 自己的估算器实现将数据带到 Dask 的数组和数据帧中，这些数组和数据帧看起来非常像 numpy 数组和 pandas 数据帧，但它们将数据分布在计算机集群中，因此能够处理否则无法放入单机内存中的数据。

不幸的是，Dask-ML 中实现的估计器并不多。根据[当前文档](https://dask-ml.readthedocs.io/en/latest/modules/api.html)，Dask-ML 提供的算法集仅限于以下内容:

*   **预处理:** MinMaxScaler、QuantileTransformer、StandardScaler、LabelEncoder、OneHotEncoder、RobustScaler、分类器、DummyEncoder、OrdinalEncoder、SimpleImputer、PCA、TruncatedSVD
*   **模型选择:** train_test_split，ShuffleSplit，KFold，GridSearchCV，RandomizedSearchCV
*   **估计量:**线性回归、逻辑回归、泊松回归
*   **元估计量:** ParallelPostFit，增量
*   **度量:** MAE，MSE，r2_score，accuracy_score，log_loss
*   **聚类:**k 均值，PartialMiniBatchKMeans 均值，光谱聚类

差不多就是这样——Dask-ML API 参考点的完整列表。

然而，使 Dask-ML 仍然是一个强大的工具的是 scikit-learn 的并行化估计器列表(实现了 n_jobs 参数的列表)，即 Dask 能够跨节点集群并行化的 scikit-learn 估计器列表，非常丰富。仅在 sklearn 中就有略多于 15 个这样的分类器，还不算回归器和其他算法。

# 摘要

Dask-ML 是一个很好的工具，可以将 scikit-learn 的并行估算器从在单台计算机上运行扩展到在计算机集群上运行，Dask 通常是处理大数据的一个很好的替代工具。