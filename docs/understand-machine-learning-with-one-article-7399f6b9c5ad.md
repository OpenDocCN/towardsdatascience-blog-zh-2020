# 用一篇文章理解机器学习

> 原文：<https://towardsdatascience.com/understand-machine-learning-with-one-article-7399f6b9c5ad?source=collection_archive---------27----------------------->

## 让我们深入了解市场上最令人兴奋、要求最高的学科之一。

![](img/d1b8452a378fd9ed4041b4c013122f33.png)

摄影爱好在 [Unsplash](https://unsplash.com/s/photos/machine-learning?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上

> 我们所知道的事情中，很少有不能简化为数学推理的。当他们不能时，这表明我们对他们的了解非常少而且混乱。
> 
> 《机会法则》，序言(1962)——约翰·阿巴斯诺特

与其他任何数学学科相比，**统计学**更大程度上是时间的产物。如果前几个世纪的科学家能够接触到真正的计算能力，甚至是计算机，他们的研究和当前的领域作为一个整体将会描绘出一幅完全不同的画面。

虽然不常讲，但统计学是今天已知的机器学习的基础。如今，很难找到一个用例，其中机器学习没有以某种形式应用，统计数据被放在一边。随着越来越多的人意识到 ML 应用程序的好处，并获得其部署的简单性，这种趋势可能会继续增长。如果你喜欢学习更多这方面的知识，或者你只是好奇它是如何工作的，这篇文章可能适合你。

# 目录:

1.  什么是机器学习？我为什么要用它？(2 分钟读取)
2.  流行的误解(1 分钟阅读)
3.  机器学习和计量经济学有什么不同？(1 分钟读取)
4.  必读的机器学习书籍(1 分钟阅读)
5.  金融中的机器学习(1 分钟阅读)
6.  用 Python 编程支持向量机和线性回归模型(4 分钟阅读)

# 1.什么是机器学习？

![](img/823c9752104e12a2f61e6c67e1d855ba.png)

由[阿瑟尼·托古列夫](https://unsplash.com/@tetrakiss?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/machine-learning?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

**广义而言，机器学习是指在高维空间中学习复杂模式的一组算法。我们来澄清一下这个说法:**

*   ML 模型在没有接受任何特定指导的情况下学习模式，因为研究人员对数据施加很少的结构。相反，**算法从数据中导出结构。**
*   我们说它学习复杂的模式，因为由算法**识别的**结构**可能无法表示为有限的方程组**。
*   高维空间这个术语指的是在处理大量变量的同时寻找解，以及变量之间的相互作用。

例如，我们可以通过展示示例并让模型推断人脸的模式和结构，来训练 ML 算法识别人脸。我们没有定义面部，因此算法在没有我们指导的情况下学习。

ML 涉及三种类型的学习:

*   **监督学习**:学习方法，包括手动告诉模型用户想要为训练数据集预测什么标签。起点是程序员希望算法尊重的一组特征和结果标签。*例如:回归，分类。*
*   **无监督学习**:学习方法，包括让模型根据每个元素具有的更明显的特征来确定数据集的标签和分组元素。例如:聚类、表示。
*   **强化学习**:智能体通过与环境互动，从环境中学习并因执行动作而获得奖励的学习方法。类似于监督学习，它接收一种指南，但它不是来自程序员的输入。

## **为什么要用？**

机器学习正在改变我们生活的方方面面。如今，算法可以完成直到最近只有专家才能完成的任务，即使没有这样的专业水平也逐渐可以实现。另一方面，**传统的数据分析**技术**保持静态**，尽管在定义了数据结构的情况下非常有用，例如:

[](/airbnb-rental-analysis-of-new-york-using-python-a6e1b2ecd7dc) [## Airbnb 租房——使用 Python 分析纽约

### 发现最方便的租赁方式，以便继续实施具有良好可视化效果的数据分析。

towardsdatascience.com](/airbnb-rental-analysis-of-new-york-using-python-a6e1b2ecd7dc) 

**在快速变化的非结构化数据输入的情况下，传统数据分析的用途有限**，而非结构化数据输入最适合监督和非监督学习等技术的应用。这时，能够分析数十种输入和变量的自动化流程成为宝贵的工具。

除此之外，ML 技术实现的**解决过程****与传统的统计和数据分析**有很大不同，因为它们专注于从用户那里接收确定目标的输入，并了解哪些因素对实现该目标很重要，而不是由用户来设置将决定目标变量结果的因素。

> 它不仅允许算法进行预测，还允许算法与预测进行比较，并调整结果的准确性。

# 2.流行的误解

> ML 是圣杯还是没用？

围绕着 ML 的**炒作**和**反炒作**多得令人难以置信。第一个创造了在可预见的未来可能无法实现的期望。相反，反宣传试图让观众相信 ML 没有什么特别之处，经典统计学产生的结果与 ML 从业者和爱好者声称的一样。这两个极端都阻止了用户和爱好者认识到它今天所提供的真正的和不同的价值，因为它有助于克服经典技术的许多限制。

> ML 是一个黑盒

这是流传最广的神话。显然，这并不像马科斯·洛佩斯·德·普拉多 在其著作《*资产管理者的机器学习》*中的论点那样，他指出，在某种程度上，ML 技术与科学方法是兼容的，因为它们已经被应用于世界上的每个实验室，用于药物开发、基因组研究和高能物理等实践。无论是否有人在黑盒中应用它，这都是个人的选择，当然，如果理论在最初被解释的模型后面，更好的使用应用将被设计出来。

# 3.ML 和计量经济学有什么区别？

**计量经济学**的目标是推断统计模型的参数，以便能够解释变量并根据这些参数得出结论，因为它们本身就有意义。例如，**线性回归**系数表示每个自变量和因变量之间是正相关还是负相关。计量经济学既不是专注于用模型预测变量的值，也不是试图让模型在这个意义上竞争来增强预测。

硬币的另一面是 **ML 分析师**，他们的分析纯粹是为了优化预测过程而设计的，与参数解释或模型解释无关，如上所述，这有时是不可能做到的。虽然它们的目标不同，但这两个学科是互补的，并且具有可相互应用的宝贵工具。

# 4.必读的机器学习书籍

![](img/2525e372037c5e6d22260d8a214de5fe.png)

照片由 [**克里斯蒂娜·莫里洛**](https://www.pexels.com/@divinetechygirl?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 发自 [**像素**](https://www.pexels.com/photo/woman-programming-on-a-notebook-1181359/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

机器学习潜在的读书人有两种:**从业者**和**学院派**。对于他们中的每一个人，都有一系列的文献要处理，考虑到第一组人本质上是在寻找更多的应用技术，而不是专注于模型背后的数学。另一方面，学者们可能会在他们将利用的算法背后寻找严格而有力的实证。

以下是对每组的简要建议:

**学术界**:

*   詹姆斯，g .等人(2013)——[统计学习介绍](https://faculty.marshall.usc.edu/gareth-james/ISL/)——施普林格
*   Bishop，c .(2011)——[模式识别和机器学习](http://users.isr.ist.utl.pt/~wurmd/Livros/school/Bishop%20-%20Pattern%20Recognition%20And%20Machine%20Learning%20-%20Springer%20%202006.pdf)——Springer
*   马斯兰德，s .(2014)——[机器学习:算法视角](https://doc.lagout.org/science/Artificial%20Intelligence/Machine%20learning/Machine%20Learning_%20An%20Algorithmic%20Perspective%20%282nd%20ed.%29%20%5BMarsland%202014-10-08%5D.pdf)——第二版。—查普曼&大厅
*   萨顿，r .和巴尔托，a .(2018)——[强化学习:介绍](https://inst.eecs.berkeley.edu/~cs188/sp20/assets/files/SuttonBartoIPRLBook2ndEd.pdf)——第二版——麻省理工学院出版社
*   Hastie，t .等人(2009 年)——[统计学习的要素](https://web.stanford.edu/~hastie/ElemStatLearn/)——第二版——施普林格
*   Aggarwal，C. (2018) — [神经网络和深度学习](http://www.charuaggarwal.net/neural.htm) — Springer

**从业者:**

*   Geron，a .(2017)——[用 Scikit-learn&tensor flow](https://www.academia.edu/37010160/Hands_On_Machine_Learning_with_Scikit_Learn_and_TensorFlow)——O ' Reilly 进行动手机器学习
*   Chollet，F. (2018) — [用 Python 进行深度学习](https://www.manning.com/books/deep-learning-with-python-second-edition) — Manning
*   洛佩兹·德·普拉多(2018)——[金融机器学习的进展](https://www.oreilly.com/library/view/advances-in-financial/9781119482086/)——威利

# 5.金融中的机器学习

金融应用对统计学和 ML 提出了完全不同的挑战，因为经济系统表现出一定程度的复杂性，超出了经典统计工具的掌握范围。因此，随着大型数据集、更强大的计算能力和更高效的算法交付给学生、分析师和整个社区，ML 将越来越多地在该领域发挥重要作用，这一趋势不太可能改变。

金融应用程序的主要区别在于:

*   **平稳性**:在金融领域，我们使用**非平稳数据**，例如资产价格和其他时间序列数据集，而在其他科学领域，数据往往是平稳的，这意味着信息取决于时间段。此外，财务数据往往至少部分相关，这增加了分析的复杂性。
*   **信噪比**:基本上，这方面指的是**根据当前输入，我们可以预测多少未来数据**。在金融领域，今天的数据对未来结果的预测能力往往较低，而在药物开发或医疗应用等普通科学领域，情况并非如此。在这些领域中，实验程序在一致和不变的条件下进行，以消除波动性给等式带来的不确定性，例如市场每天必须处理的无数信息源。
    这个问题并不意味着 ML 不能应用于金融，而是它必须以不同的方式应用。
*   **结果的可解释性**:由于金融法规和机构的合规义务，与其他科学领域面临的审查水平相比，在金融战略中论证 ML 模型的应用对项目经理和资产经理来说是一个挑战。

在最后一部分中，我将执行两个样本模型的 Python 应用程序，支持向量机和线性回归，以便比较它们，并在实践中展示 ML 脚本是如何实现的。如果你不想进入编码领域，也许你会发现这篇文章很有用:

[](/is-it-possible-to-make-machine-learning-algorithms-without-coding-cb1aadf72f5a) [## 有没有可能不用编码就做出机器学习算法？

### 我准备了一个简单的应用程序，向您展示如何借助一个有趣的工具来实现这一点，这个工具叫做…

towardsdatascience.com](/is-it-possible-to-make-machine-learning-algorithms-without-coding-cb1aadf72f5a) 

# 6.用 Python 实现支持向量机和线性回归；

让我们深入编码吧！

![](img/9ffaf9b0ba4968247576004241e9fc8e.png)

照片由 [**希泰什·乔杜里**](https://www.pexels.com/@hiteshchoudhary?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 发自 [**佩克斯**](https://www.pexels.com/photo/man-in-grey-sweater-holding-yellow-sticky-note-879109/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

我们将开始导入必要的库: **Scikit-Learn** 和 **Numpy** 。

[**Scikit-Learn**](https://scikit-learn.org/stable/getting_started.html) 是一个开源的机器学习库，支持**监督**和**非监督学习**。它还提供了各种工具，用于模型拟合、数据预处理以及模型选择和评估等。在这种情况下，我们将利用:

*   [**Model_selection 类**](https://scikit-learn.org/stable/model_selection.html) :包含用于在训练和测试样本中拆分数据集的工具、超参数优化器和模型验证器。
*   [**Linear_model 类**](https://scikit-learn.org/stable/modules/classes.html#module-sklearn.linear_model) :用于实现各种线性模型，如逻辑回归、线性回归等。
*   [**svm 类**](https://scikit-learn.org/stable/modules/svm.html) :用于实现用于分类、回归和离群点检测的监督学习方法。

另一方面， [**Numpy**](https://numpy.org/) 是一个库，它提供了对大型多维数组和矩阵的支持，以及对这些数组进行操作的大量高级数学函数。这是机器学习堆栈基础的一部分。

```
# Imports
import numpy as np
import sklearn.model_selection as sk_ms
import sklearn.linear_model as sk_lm
import sklearn.svm as sk_sv
```

在那之后，我们继续创建我们将要使用的 Numpy 数组。考虑这些将作为“合成”数据集，因为这个过程也可以用. csv 文件或数据库来执行。

首先，我将生成 1000 行和 5 个要素的随机样本，作为模型的输入。除此之外，我还特意包含了一个带有预定数组的示例函数。这个函数执行的过程包括计算输入数组中每个元素与 X 数组中每个元素的乘积。

```
# Setting seed to lock random selection
np.random.seed(1234)# Generate random uniform-distributed sample 
X = np.random.uniform(size=(1000,5))
y = 2\. + X @ np.array([1., 3., -2., 7., 5.]) + np.random.normal(size=1000)
```

为了澄清前面的代码，请参考下面的笔记本，查看所创建变量的输出:

这些是每个阵列的形状(长度和尺寸):

在我们创建了样本数据集之后，我们必须继续进行我在这篇[文章](/is-it-possible-to-make-machine-learning-algorithms-without-coding-cb1aadf72f5a)中解释的训练和测试子集的划分。这个任务可以用已经引入的 **scikit-learn** 包的类来执行，称为 *model_selection* ， *train_test_split* 方法:

![](img/309c436984e1fa3f5bdd060442dc0c1a.png)

在接下来的步骤中，我将实现两个模型，即**线性回归**和**支持向量回归**。作为每个模型的概述:

*   **线性回归**:这是一种统计模型，假设响应变量(y)是权重的线性组合，也称为参数，乘以一组预测变量(x)。该模型包括考虑随机采样噪声的误差。对于术语响应变量，我的意思是由输出值显示的行为取决于另一组因素，称为预测因素。
    在下面的等式中，您将看到包含的β是权重，x 是预测变量的值，ε是误差项，表示随机采样噪声或模型中未包含的变量的影响。

![](img/b6c7dba87a8b92819370f46e8c373afe.png)

传统线性回归方程

*   **支持向量机:**这也是一个用于分类和回归问题的线性模型，它基本上由一个算法组成，该算法将数据作为输入，并输出一条线，该线将观察结果分成两类。

在下面的要点中，您将找到编写两个引入模型的脚本，在其中我们可以看到每个模型的结果平均值。为了比较这些模型，我运行了一个 [**交叉验证**](/train-test-split-and-cross-validation-in-python-80b61beca4b6) 过程，这是一种将我们的数据分割成一定数量的子集的方法，称为折叠(在本例中为五个)，以避免过度拟合我们的模型:

最后，我们开始评估测试样本上的模型，并获得系数和截距的输出:

![](img/821b7920e974f10c3aa9fe7c1eaad091.png)![](img/f09b8b8eb95fe8d5a77518b713d88f33.png)

# 结论

我写这篇文章的目的是以一种简单的方式传递一些机器学习的基础知识，并稍微关注一下金融，因为这是我个人感兴趣的一个主题。我相信，投资者将逐步被引入算法交易和涉及人工智能和机器学习过程的智能资产管理实践，因此在某种程度上，本文试图诱导更多的人参与这场运动，以便成为变革的一部分。

我希望我已经实现了我的目标，并且对你有用。如果你喜欢这篇文章中的信息，不要犹豫，联系我分享你的想法。它激励我继续分享！

# 参考

*   [1][Numpy 的基本指南](https://becominghuman.ai/an-essential-guide-to-numpy-for-machine-learning-in-python-5615e1758301) —西达尔特·迪克西特— 2018
*   [2] [支持向量机](/https-medium-com-pupalerushikesh-svm-f4b42800e989) — Rushikesh Pupale — 2018

感谢您花时间阅读我的文章！如果您有任何问题或想法要分享，请随时通过我的[电子邮件](http://herrera.ajulian@gmail.com)联系我，或者您可以在以下社交网络中找到我以了解更多相关内容:

*   [**LinkedIn**](https://www.linkedin.com/in/juli%C3%A1n-alfredo-herrera-08531559/)**。**
*   [**GitHub**](https://github.com/Jotaherrer) **。**