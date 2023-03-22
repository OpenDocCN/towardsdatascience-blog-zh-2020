# scikit 中的 XGBoost 入门-学习

> 原文：<https://towardsdatascience.com/getting-started-with-xgboost-in-scikit-learn-f69f5f470a97?source=collection_archive---------0----------------------->

## [入门](https://towardsdatascience.com/tagged/getting-started)

## 每一段旅程都从一次助推开始

![](img/4b29e7d27e01623dbe40eff0fafc249e.png)

图片来源:【https://www.pxfuel.com/en/free-photo-juges 

*这篇文章解释了 XGBoost 是什么，为什么 XGBoost 应该是你的首选机器学习算法，以及在 Colab 或 Jupyter 笔记本上启动和运行 XGBoost 所需的代码。假设您对机器学习和 Python 有基本的了解。*

## XGBoost 是什么

在机器学习中，集成模型比单个模型有更高的概率表现更好。集成模型将不同的机器学习模型组合成一个。随机森林是一个流行的集合，它通过装袋取许多决策树的平均值。Bagging 是“bootstrap aggregation”的缩写，意思是通过替换(bootstrapping)选择样本，并通过取其平均值进行组合(聚合)。

助推是装袋的一种强有力的替代方法。助推器不是聚集预测，而是通过关注单个模型(通常是决策树)哪里出错，将弱学习者变成强学习者。在梯度推进中，单个模型基于残差训练，残差是预测和实际结果之间的差异。梯度提升树不是聚集树，而是在每一轮提升过程中从错误中学习。

XGBoost 是“极端梯度推进”的缩写。“极致”是指并行计算和缓存感知等速度增强，使 XGBoost 比传统的梯度提升快大约 10 倍。此外，XGBoost 包括一个独特的分裂查找算法来优化树，以及内置的正则化来减少过度拟合。一般来说，XGBoost 是一个更快、更精确的渐变增强版本。

平均而言，boosting 比 bagging 表现得更好，梯度 Boosting 可以说是最好的 Boosting 系综。由于 XGBoost 是梯度增强的高级版本，其结果是无与伦比的，因此它可以说是我们拥有的最好的机器学习组合。

## 为什么 XGBoost 应该是您的首选机器学习算法

从 2014 年的希格斯玻色子 Kaggle 比赛开始，XGBoost 在机器学习领域掀起了风暴，经常在 Kaggle 比赛中获得一等奖。XGBoost 的受欢迎程度飙升，因为在竞争环境中，当从表格数据(行和列的表格)进行预测时，它一直优于可比的机器学习算法。

从表格数据进行预测时，XGBoost 可能是您的最佳起点，原因如下:

*   XGBoost 很容易在 scikit-learn 中实现。
*   XGBoost 是一个系综，所以比单个模型得分高。
*   XGBoost 是正则化的，所以默认模型通常不会过度拟合。
*   XGBoost 非常快(对于系综而言)。
*   XGBoost 从错误中学习(梯度提升)。
*   XGBoost 有大量用于微调的超参数。
*   XGBoost 包括超参数来缩放不平衡的数据和填充空值。

现在，您对 XGBoost 是什么以及为什么 XGBoost 应该是处理表格数据时的首选机器学习算法有了更好的了解(与神经网络工作得更好的图像或文本等非结构化数据相比)，让我们建立一些模型。

## 安装 XGBoost

如果您运行的是 Colab 笔记本，XGBoost 是一个选项。如果你在 Jupyter 笔记本上运行 Anaconda，你可能需要先安装它。打开您的终端并运行以下命令来安装 XGBoost 和 Anaconda:

`conda install -c conda-forge xgboost`

如果您想要验证安装或您的 XGBoost 版本，请运行以下命令:

`import xgboost; print(xgboost.__version__)`

如需更多选项，请查看 [XGBoost 安装指南。](https://xgboost.readthedocs.io/en/release_0.72/build.html)

接下来我们拿一些数据来做预测。

## scikit-learn 中的 XGBRegressor

Scikit-learn 带有几个内置数据集，您可以访问这些数据集来快速对模型进行评分。以下代码加载 scikit-learn 糖尿病数据集，该数据集测量一年后疾病的传播程度。更多信息请参见[sci kit-学习数据集加载页面](https://scikit-learn.org/stable/datasets/index.html#diabetes-dataset)。

```
from sklearn import datasets
X,y = datasets.load_diabetes(return_X_y=True)
```

衡量糖尿病传播程度的指标可能会采用连续值，因此我们需要一个机器学习回归器来进行预测。XGBoost 回归器称为 **XGBRegressor** ，可按如下方式导入:

```
from xgboost import XGBRegressor
```

我们可以使用交叉验证在多个折叠上构建模型并对其评分，这总是一个好主意。使用交叉验证的一个优点是它会为您拆分数据(默认情况下是 5 次)。首先导入 **cross_val_score** 。

```
from sklearn.model_selection import cross_val_score
```

要使用 XGBoost，只需将 XGBRegressor 放在 cross_val_score 中，以及 X、y 和您首选的回归得分指标。我更喜欢均方根误差，但这需要转换负的均方误差作为一个额外的步骤。

```
scores = cross_val_score(XGBRegressor(), X, y, scoring='neg_mean_squared_error')
```

如果您得到警告，那是因为 XGBoost 最近更改了他们默认回归目标的名称，他们希望您知道。要消除警告，请尝试以下方法，结果相同:

```
*# Alternative code to silence potential errors* scores = cross_val_score(**XGBRegressor(objective='reg:squarederror')**, X, y, scoring='neg_mean_squared_error')
```

求均方根误差，取 5 个分数的负平方根即可。

```
(-scores)**0.5
```

回想一下，在 Python 中，语法 x**0.5 表示 x 的 1/2 次方，也就是平方根。

我的 [Colab 笔记本](https://colab.research.google.com/drive/1HLZTwEWrzVVhsVZUkAeJcCJ-Ayrx4CHE?usp=sharing)结果如下。

```
array([56.04057166, 56.14039793, 60.3213523 , 59.67532995, 60.7722925 ])
```

如果你喜欢一个分数，试试 **scores.mean()** 求平均值。

![](img/8b1b0b6a5595a8662a26f36350e8ec0c.png)

预期的希格斯玻色子衰变。欧洲核子研究中心制作的照片。通用创作许可证。[链接到维基百科。](https://commons.wikimedia.org/wiki/File:3D_view_of_an_event_recorded_with_the_CMS_detector_in_2012_at_a_proton-proton_centre_of_mass_energy_of_8_TeV.png)

## xgb 回归码

下面是使用 scikit-learn 中的 XGBoost 回归器预测糖尿病进展的所有代码。

```
from sklearn import datasets
X,y = datasets.load_diabetes(return_X_y=True)
from xgboost import XGBRegressor
from sklearn.model_selection import cross_val_score
scores = cross_val_score(**XGBRegressor(objective='reg:squarederror')**, X, y, scoring='neg_mean_squared_error')
(-scores)**0.5
```

正如你所看到的，由于 2019 年推出的新 scikit-learn 包装器，XGBoost 与其他 scikit-learn 机器学习算法的工作原理相同。

## scikit-learn 中的 XGBClassifier

接下来，让我们使用类似的步骤构建一个 XGBoost 分类器并进行评分。

以下 url 包含可用于预测患者是否患有心脏病的心脏病数据集。

```
url = ‘https://media.githubusercontent.com/media/PacktPublishing/Hands-On-Gradient-Boosting-with-XGBoost-and-Scikit-learn/master/Chapter02/heart_disease.csv'
```

该数据集包含 13 个预测值列，如胆固醇水平和胸痛。最后一栏标有“目标”，确定患者是否患有心脏病。原始数据集的来源位于 [UCI 机器学习库](https://archive.ics.uci.edu/ml/datasets/heart+disease)。

导入 pandas 来读取 csv 链接并将其存储为 DataFrame，df。

```
import pandas as pd
df = pd.read_csv(url)
```

由于目标列是最后一列，并且该数据集已经过预清理，因此可以使用索引位置将数据拆分为 X 和 y，如下所示:

```
X = df.iloc[:, :-1]
y = df.iloc[:, -1]
```

最后，导入 XGBClassifier 并使用 cross_val_score 对模型进行评分，将准确性作为默认的评分标准。

```
from xgboost import XGBClassifier
from sklearn.model_selection import cross_val_score
cross_val_score(XGBClassifier(), X, y)
```

以下是我从我的 [Colab 笔记本](https://colab.research.google.com/drive/1HLZTwEWrzVVhsVZUkAeJcCJ-Ayrx4CHE?usp=sharing)中得到的结果。

```
array([0.85245902, 0.85245902, 0.7704918 , 0.78333333, 0.76666667])
```

## xgb 分类器代码

以下是使用 scikit 中的 XGBClassifier 预测患者是否患有心脏病的所有代码——了解五个方面:

```
url = 'https://media.githubusercontent.com/media/PacktPublishing/Hands-On-Gradient-Boosting-with-XGBoost-and-Scikit-learn/master/Chapter02/heart_disease.csv'import pandas as pd
df = pd.read_csv(url)
X = df.iloc[:, :-1]
y = df.iloc[:, -1]from xgboost import XGBClassifier
from sklearn.model_selection import cross_val_score
cross_val_score(XGBClassifier(), X, y)
```

您知道如何在 scikit 中构建 XGBoost 分类器和回归器并对其评分——轻松学习。

## XGBoost 后续步骤

从 XGBoost 中获得更多需要微调超参数。XGBoost 由许多决策树组成，因此除了集成超参数之外，还有决策树超参数可以进行微调。查看这篇 [Analytics Vidhya 文章](https://www.analyticsvidhya.com/blog/2016/03/complete-guide-parameter-tuning-xgboost-with-codes-python/)，以及[官方 XGBoost 参数文档](https://xgboost.readthedocs.io/en/latest/parameter.html)开始使用。

如果你正在寻找更多的深度，我的书 [*用 XGBoost 和 scikit 手动渐变增强-从*](https://www.amazon.com/Hands-Gradient-Boosting-XGBoost-scikit-learn/dp/1839218355/ref=sr_1_1?dchild=1&keywords=xgboost&qid=1604947393&sr=8-1) *[Packt Publishing](https://www.packtpub.com/product/hands-on-gradient-boosting-with-xgboost-and-scikit-learn/9781839218354) 学习* 是一个很好的选择。除了广泛的超参数微调，您将了解 XGBoost 在机器学习领域的历史背景，XGBoost 案例研究的详细信息，如 Higgs boson Kaggle 竞赛，以及高级主题，如调整备选基础学习器(gblinear，DART，XGBoost 随机森林)和为行业部署模型。

编码快乐！

*科里·韦德(Corey Wade)是* [*伯克利编码学院*](http://berkeleycodingacademy.com) *的创始人和主管，在这里他向来自世界各地的学生教授机器学习。此外，Corey 在伯克利高中的独立学习项目中教授数学和编程。著有两本书，* [*用 XGBoost 和 scikit-learn*](https://www.amazon.com/Hands-Gradient-Boosting-XGBoost-scikit-learn/dp/1839218355/ref=sr_1_1?dchild=1&keywords=xgboost&qid=1604947393&sr=8-1) *和*[*The Python Workshop*](https://www.amazon.com/Python-Workshop-Interactive-Approach-Learning/dp/1839218851/ref=sr_1_1_sspa?dchild=1&keywords=The+Python+Workshop&qid=1604971133&sr=8-1-spons&psc=1&spLa=ZW5jcnlwdGVkUXVhbGlmaWVyPUE3R1dTUDlVVVVDU1omZW5jcnlwdGVkSWQ9QTA5NjYyNjYxQzdQSDhZTzJUVklUJmVuY3J5cHRlZEFkSWQ9QTAzNTAyNjQyN0E5NUlRNDFNU0NCJndpZGdldE5hbWU9c3BfYXRmJmFjdGlvbj1jbGlja1JlZGlyZWN0JmRvTm90TG9nQ2xpY2s9dHJ1ZQ==)*。*

*本文中的代码可能是直接抄袭了* [*科里的 Colab 笔记本*](https://colab.research.google.com/drive/1HLZTwEWrzVVhsVZUkAeJcCJ-Ayrx4CHE?usp=sharing) *。*

![](img/0406f2d551aac87ca42fc07c3c6f113a.png)

亚历山大·辛恩摄影:[https://unsplash.com/photos/KgLtFCgfC28](https://unsplash.com/photos/KgLtFCgfC28)