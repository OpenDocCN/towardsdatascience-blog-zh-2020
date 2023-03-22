# 10 个鲜为人知的用于机器学习的 Python 库

> 原文：<https://towardsdatascience.com/10-lesser-known-python-libraries-for-machine-learning-fca7ad32e53c?source=collection_archive---------48----------------------->

## 十个你可能不知道的简化机器学习过程的工具

![](img/f29c113ba8df7caef9acb83ac6e871d1.png)

克里斯娅·克鲁兹在 [Unsplash](https://unsplash.com/s/photos/ten?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

Python 凭借 Scikit-learn、Tensorflow 和 Keras 等非常受欢迎的核心包主导了机器学习领域。有一个非常活跃的开发人员社区，致力于开发其他不太知名的库，这些库不仅为模型开发，也为围绕模型开发的许多过程提供了实用工具。包括数据预处理、特征工程和模型解释。

在这篇文章中，我想分享十个我最喜欢的不太为人所知的 Python 库，它们会让你的机器学习生活变得更加容易！

# 1.熊猫-ml

 [## 欢迎来到熊猫-ml 的文档！- pandas_ml 0.3.0 文档

### 编辑描述

熊猫-ml.readthedocs.io](https://pandas-ml.readthedocs.io/en/latest/index.html) 

该库将 pandas、Scikit-learn、XGBoost 和 Matplotlib 功能集成在一起，以简化数据准备和模型评分。

在数据处理方面，pandas-ml 使用一种称为 ModelFrame 的数据格式，其中包含元数据，包括关于要素和目标的信息，以便可以更容易地应用统计数据和模型函数。

因为 ModelFrame 继承了熊猫的所有特征。DataFrame 所有熊猫的方法都可以直接应用到你的机器学习数据上。

# 2.类别编码器

 [## 类别编码器-类别编码器 2.2.2 文档

### 一套 scikit-learn 风格的转换器，用于使用不同的技术将分类变量编码成数字…

contrib.scikit-learn.org](http://contrib.scikit-learn.org/category_encoders/index.html) 

类别编码器库简化了机器学习中分类变量的处理。它可以单独用于转换变量，但也可以与 Scikit-learn 集成，并可以在 Sckit-learn 管道中使用。

尽管 Scikit-learn 有一些将分类特征转换成数字的功能，比如一键编码和序号编码器，但是分类编码器提供了更广泛的方法来处理这些数据类型。特别是，它包含了许多处理高基数特性(具有大量唯一值的特性)的方法，例如证据转换器的权重。

# 3.黄砖

 [## Yellowbrick:机器学习可视化- Yellowbrick v1.1 文档

### 不管你的技术水平如何，你都可以提供帮助。我们感谢错误报告、用户测试、功能请求…

www.scikit-yb.org](https://www.scikit-yb.org/en/latest/) 

Yellowbrick 是一个专门为 Scikit-learn 开发的机器学习模型设计的可视化库。该库提供了广泛的易于使用的可视化，有助于机器学习过程的各个方面，包括模型选择、特征提取以及模型评估和解释。

# 4.Shap

[](https://github.com/slundberg/shap) [## slundberg/shap

### SHAP 是一种博弈论的方法来解释任何机器学习模型的输出…

github.com](https://github.com/slundberg/shap) 

Shap 库使用一种基于博弈论的技术来为任何机器学习模型的输出提供解释。它可以为使用最流行的 python 机器学习库(包括 Scikit-learn、XGBoost、Pyspark、Tensorflow 和 Keras)开发的模型提供可解释性。

# 5.特征引擎

 [## 快速启动-功能-引擎 0.4.3 文档

### 如果您是功能引擎的新手，本指南将帮助您入门。特征引擎转换器有 fit()和…

feature-engine.readthedocs.io](https://feature-engine.readthedocs.io/en/latest/quickstart.html) 

Feature engine 是一个开源 python 库，旨在使各种各样的功能工程技术易于使用。特征工程通常包括以下步骤:

*   输入缺失值
*   离群点去除
*   编码分类变量
*   离散化和标准化
*   工程新功能

特征引擎库包含执行大多数这些任务的函数和方法。该代码使用 fit()和 transform()方法遵循 Scikit-learn 功能，并且可以在 Scikit-learn 管道中使用。

# 6.功能工具

要素工具是用于自动化要素工程的 Python 框架。该库采用单个数据集或一组关系数据集，并运行深度特征合成(DFS)来创建现有和新生成特征的矩阵。该工具可以在特征工程过程中节省大量时间。

# 7.Dabl

 [## 欢迎来到 dabl，数据分析基线库- dabl 文档

### 欢迎来到数据分析基线库 dabl

- dabl 文档欢迎来到数据分析基线库 dabl.github.io](https://dabl.github.io/dev/) 

Dabl 包旨在自动化一些常见的重复性机器学习任务，如数据清理和基本分析。Dabl 使用“最佳猜测”理念来应用数据清理流程，但允许用户在必要时检查和修改流程。

我之前在去年的这个[帖子](/human-in-the-loop-auto-machine-learning-with-dabl-2fe9c9d1736f)中写过一篇更全面的 Dabl 指南。

# 8.惊喜

 [## 主页

### Surprise 是一个 Python scikit，它构建并分析处理显式评级数据的推荐系统。惊喜…

surpriselib.com](http://surpriselib.com/) 

这个库是为 Python 中显式推荐引擎的简单实现而设计的。它有一个非常类似于 scikit 的界面——如果您已经是该库的用户，那么学习起来非常直观。它有广泛的内置算法，你可以评估你的数据，但你也可以建立自己的。还有用于交叉验证和超参数优化的工具。

# 9.Pycaret

[](https://pycaret.org/) [## 主页- PyCaret

### 无论是输入缺失值、转换分类数据、特征工程还是超参数调整…

pycaret.org](https://pycaret.org/) 

Pycaret 被设计成一个极低代码的 Python 机器学习库。它既面向希望更快地构建模型的数据科学家，也面向对构建简单模型感兴趣的非数据科学家。

该库包含整个机器学习过程的低代码示例，包括预处理、模型训练、评估和调整。包括所有常见的估计器类型，如逻辑回归、决策树、梯度推进分类器和 cat boost。这个库还包含一个非常简单的部署解决方案，它将在 AWS S3 桶上部署最终模型。模型总是可以保存为 pickle 文件，用于替代部署解决方案。

# 10.先知

[](https://facebook.github.io/prophet/docs/quick_start.html) [## 快速启动

### Prophet 遵循 sklearn 模型 API。我们创建一个 Prophet 类的实例，然后调用它的 fit 和 predict…

facebook.github.io](https://facebook.github.io/prophet/docs/quick_start.html) 

Prophet 是一个 python 库，旨在大大简化时间序列预测，由脸书开源。该库使用与 Scikit-learn 类似的界面，极大地简化了时间序列预测的过程。有一些有用的绘图功能，包括可视化和评估模型。它也很容易模拟季节性和假日效应，包括你自己定制的日期周期。

Python 生态系统有一个非常活跃的社区，人们开发库来使机器学习更简单和更容易。这篇文章是我经常使用的十个不太知名的库的选集，但是对于一个更全面的列表，这是一个非常棒的资源。

感谢阅读！

[**我每月发一份简讯，如果你想加入请通过这个链接注册。期待成为您学习旅程的一部分！**](https://mailchi.mp/ce8ccd91d6d5/datacademy-signup)