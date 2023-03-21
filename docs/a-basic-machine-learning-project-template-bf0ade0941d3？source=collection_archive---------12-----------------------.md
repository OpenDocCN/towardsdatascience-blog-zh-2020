# 机器学习项目模板

> 原文：<https://towardsdatascience.com/a-basic-machine-learning-project-template-bf0ade0941d3?source=collection_archive---------12----------------------->

## 总结基本 ML 项目中遵循的步骤

![](img/ecc48ecea144d421923b213764820e1a.png)

[万花筒](https://unsplash.com/@kaleidico?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

*每个机器学习项目都有其独特之处。尽管对于每个这样的项目，可以遵循一组预定义的步骤。没有那么严格的流程可循，但是可以提出一个通用的模板。*

# 1.准备问题

不仅仅是 ML，任何项目的第一步都是简单地定义手头的问题。你首先需要了解情况和需要解决的问题。然后设计机器学习如何有效地解决这个问题。一旦你很好地了解了问题，你就可以着手解决它。

## 加载库

在本文中，我将坚持使用 Python *(原因很明显)*。第一步是加载或[导入](https://docs.python.org/2.0/ref/import.html)获得想要的结果所需的所有库和包。机器学习的一些非常初级且几乎必不可少的包有— [NumPy](https://numpy.org/doc/) 、 [Pandas](https://pandas.pydata.org/docs/) 、 [Matplotlib](https://matplotlib.org/contents.html) 和 [Scikit-Learn](https://scikit-learn.org/stable/) 。

## 加载数据集

一旦加载了库，就需要加载数据。Pandas 有一个非常简单的函数来执行这个任务— [pandas.read_csv](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html) 。read.csv 函数不仅限于 csv 文件，还可以读取其他基于文本的文件。其他格式也可以使用熊猫阅读功能来阅读，如 html，json，腌渍文件等。需要记住的一点是，您的数据需要位于与当前工作目录相同的工作目录中，否则您将需要在函数中提供以“/”为前缀的完整路径。

# 2.汇总数据

好了，数据已经加载完毕，可以开始行动了。但是您首先需要检查数据的外观以及它包含的所有内容。首先，您会希望看到数据有多少行和列，以及每一列的数据类型是什么(pandas 认为它们是什么)。

查看数据类型和形状的快速方法是— [熊猫。DataFrame.info](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.info.html) 。这会告诉您数据帧有多少行和列，以及它们包含哪些数据类型和值。

## 描述统计学

描述统计学，顾名思义，是用统计学的术语来描述数据——均值、标准差、[分位数](https://stats.stackexchange.com/questions/156778/percentile-vs-quantile-vs-quartile)等。获得完整描述的最简单方法是通过[熊猫。data frame . description](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.describe.html)。您可以很容易地判断出您的数据是否需要缩放，或者是否需要添加缺失的值，等等。(稍后将详细介绍)。

## 数据可视化

数据可视化非常重要，因为这是了解数据和模式的最快方法——不管它们是否存在。您的数据可能有数千个要素，甚至更多实例。不可能对所有的数字数据进行分析。如果你这样做了，那么拥有像 **Matplotlib** 和 **Seaborn** 这样强大的可视化软件包还有什么意义呢？

使用 Matplotlib、Seaborn 的可视化可用于检查特征内的[相关性](/let-us-understand-the-correlation-matrix-and-covariance-matrix-d42e6b643c22)以及与目标、散点图数据、直方图和[箱线图](/understanding-boxplots-5e2df7bcbd51)的相关性，用于检查分布和 [**偏斜度**](https://www.youtube.com/watch?v=XSSRrVMOqlQ) 等等。甚至 pandas 也有自己的内置可视化库——pandas。DataFrame.plot 包含条形图、散点图、直方图等。

Seaborn 本质上是 matplotlib 的一个变形，因为它建立在 matplotlib 的基础上，使绘图更漂亮，绘图过程更快。[热图](https://seaborn.pydata.org/generated/seaborn.heatmap.html)和 [pairplot](https://seaborn.pydata.org/generated/seaborn.pairplot.html) 是 Seaborn 快速绘制整个数据可视化的例子，以检查[多重共线性](/multicollinearity-why-is-it-a-problem-398b010b77ac)，缺失值等。

一个非常有效的方法是通过[**Pandas Profiling**](https://pandas-profiling.github.io/pandas-profiling/docs/)获得上述大部分描述性和推断性统计数据。概要分析会生成一个漂亮的数据报告，包含上面提到的所有细节，让您可以在一个报告中进行分析。

# 3.准备数据

一旦你知道你的数据有什么和看起来像什么，你将必须转换它，以便使它适合算法处理和更有效地工作，以便给出更准确和精确的结果。这本质上是数据预处理，是任何 ML 项目中最重要也是最耗时的阶段。

## 数据清理

现实生活中的数据并没有被很好地安排和呈现在你面前，没有任何异常。数据通常有许多所谓的异常，如缺失值、许多格式不正确的要素、不同比例的要素等。所有这些都需要手动处理，这需要大量的时间和编码技能(主要是 python 和熊猫:D)！

熊猫有各种功能来检查像[熊猫这样的异常。DataFrame.isna](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.isna.html) 检查带有 nan 等的值。您可能还需要转换数据格式，以便消除无用的信息，例如当存在单独的性别特征时，从姓名中删除“先生”和“夫人”。您可能需要使用函数 [pandas 在整个数据帧中以标准格式获取它。DataFrame .使用](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.replace.html)[熊猫替换](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.drop.html)或删除不相关的功能。DataFrame.drop 。

## 特征选择

特征选择是选择一定数量的最有用的特征用于训练模型的过程。这样做是为了在大多数特征对总体方差的贡献不足时减少维数。如果你的数据中有 300 个特征，97%的差异是由前 120 个特征解释的，那么用这么多无用的特征来敲打你的算法是没有意义的。减少功能不仅可以节省时间，还可以节省成本。

一些流行的特征选择技术有[选择测试](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.SelectKBest.html)，特征消除方法如 [RFE](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.RFE.html) (递归特征消除)和嵌入方法如[拉索克](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LassoCV.html)。

## 特征工程

所有功能可能都不在最佳状态。意思是——它们可以通过使用一组函数转换到不同的尺度上。这是为了增加与目标的相关性，从而增加准确性/得分。其中一些转换与 [**缩放**](/scale-standardize-or-normalize-with-scikit-learn-6ccc7d176a02) 相关，如 StandardScaler、Normalizer、MinMaxScaler 等。甚至可以通过对一些特征进行线性/二次组合来增加特征，以提高性能。对数变换、交互和 Box-Cox 变换是数字数据的一些其他有用的变换。

对于分类数据，有必要[将类别编码](https://www.geeksforgeeks.org/ml-label-encoding-of-datasets-in-python/)成数字，这样算法就可以理解它。一些最有用的编码技术是——labelencorder、OneHotEncoder 和 Binarizer。

# 4.评估算法

一旦你的数据准备好了，继续检查各种回归/分类算法的性能(基于问题的类型)。您可以首先创建一个基础模型来设置一个比较基准。

## 分离验证数据集

一旦模型被训练，它也需要被验证，以查看它是否真的概括了数据或者它是否过度/不足拟合。手头的数据可以预先分成训练集和验证集。这种拆分有多种技术——训练测试拆分、洗牌拆分等。您还可以在整个数据集上运行[交叉验证](/train-test-split-and-cross-validation-in-python-80b61beca4b6)，以获得更可靠的验证。交叉验证、留一法是最常用的方法。

## 测试选项和评估指标

模型需要基于一组需要定义的评估指标[](/metrics-to-evaluate-your-machine-learning-algorithm-f10ba6e38234)****进行评估。对于回归算法，一些常见的指标是 MSE 和 R 平方。****

****与分类相关的评估指标更加多样化——混淆矩阵、F1 得分、AUC/ROC 曲线等。对每种算法的这些分数进行比较，以检查哪种算法比其他算法执行得更好。****

## ****抽查算法****

****一旦数据被分割并且评估指标被定义，您需要运行一组算法，比如说，在一个 for 循环中检查哪一个执行得最好。这是一个反复试验的过程，目的是找到一个能很好地解决你的问题的算法列表，这样你就可以加倍努力，进一步优化它们。****

****可以制作 [**流水线**](/pre-process-data-with-pipeline-to-prevent-data-leakage-during-cross-validation-e3442cca7fdc) ，并且可以设置线性和非线性算法的混合来检查性能。****

## ****比较算法****

****一旦您现场运行了*测试线束*，您可以很容易地看到哪些线束对您的数据表现最佳。持续给出高分的算法应该是你的目标。然后，您可以选择最优秀的，并进一步调整它们，以提高它们的性能。****

# ****5.提高准确性****

****有了性能最好的算法后，可以调整它们的参数和超参数，以获得最佳结果。多个算法也可以链接在一起。****

## ****算法调整****

****[维基百科](https://en.wikipedia.org/wiki/Hyperparameter_optimization)声明“超参数调优就是为一个学习算法选择一组最优的超参数”。超参数是未学习的参数，必须在运行算法之前设置。超参数的一些例子包括逻辑回归中的惩罚、随机梯度下降中的损失和 SVM 的核。****

****这些参数可以在数组中传递，算法可以递归运行，直到找到完美的超参数。这可以通过像[网格搜索](/grid-search-for-model-tuning-3319b259367e)和随机搜索这样的方法来实现。****

## ****合奏****

****可以将多种机器学习算法结合起来，以建立一个比单一算法更稳健、更优化的模型，从而提供更好的预测。这被称为[合奏](/ensemble-methods-in-machine-learning-what-are-they-and-why-use-them-68ec3f9fef5f)。****

****基本上有两种类型的集成— [Bagging(自举聚合)和 Boosting](/ensemble-methods-bagging-boosting-and-stacking-c9214a10a205) 。例如，随机森林是一种 Bagging ensemble，它结合了多个决策树，并对总输出进行汇总。****

****另一方面，Boosting 通过以一种自适应的方式学习来组合一组弱学习者:集合中的每个模型都被拟合，从而给予数据集中与序列中的先前模型有较大误差的实例更大的重要性。XGBoost、AdaBoost、CatBoost 就是一些例子。****

# ****6.最终确定模型****

## ****验证数据集的预测****

****当您获得了具有最佳超参数和集成的最佳性能模型时，您可以在看不见的测试数据集上验证它。****

## ****在整个训练数据集上创建独立模型****

****验证后，对整个数据集运行一次模型，以确保在训练/测试时没有数据点丢失。现在，你的模型处于最佳位置。****

## ****保存模型以备后用****

****一旦您有了准确的模型，您仍然需要保存并加载它，以便在将来需要时可以使用它。最常见的方法是[腌制](https://www.geeksforgeeks.org/understanding-python-pickling-example/)。****

****这就是本文的全部内容。当然，这并不是机器学习的全部。但是这可以作为一个很好的路线图来遵循。定制，重复的过程将需要为不同类型的数据/问题。如果你知道更好更关键的技术，请在下面评论你的想法或突出显示。****