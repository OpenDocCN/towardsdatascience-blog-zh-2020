# 每个数据科学家都应该知道的 4 个 Python AutoML 库

> 原文：<https://towardsdatascience.com/4-python-automl-libraries-every-data-scientist-should-know-680ff5d6ad08?source=collection_archive---------6----------------------->

![](img/669248ca8252f7c682ff45415a067c58.png)

来源: [Unsplash](https://unsplash.com/photos/pjAH2Ax4uWk)

## 让你的生活更轻松

自动机器学习，通常缩写为 AutoML，是一个新兴领域，其中建立机器学习模型以对数据建模的过程是自动化的。AutoML 有能力使建模变得更容易，更容易为每个人所接受。

如果您对查看 AutoML 感兴趣，这四个 Python 库是不错的选择。最后将提供一个比较。

## 1 |自动 sklearn

`auto-sklearn`是一个自动化的机器学习工具包，它与社区中许多人熟悉的标准`sklearn`接口无缝集成。随着贝叶斯优化等最新方法的使用，该库被构建为在可能的模型空间中导航，并学习推断特定配置是否将在给定任务中工作良好。

由马蒂亚斯·福雷尔等人创建的这个库的技术细节在一篇论文《高效和健壮的机器学习》中有所描述。福雷尔写道:

> …我们引入了一个基于 scikit-learn 的强大的新 AutoML 系统(使用 15 个分类器、14 种特征预处理方法和 4 种数据预处理方法，产生了一个具有 110 个超参数的结构化假设空间)。

`auto-sklearn`也许是开始使用 AutoML 的最好的库。除了发现数据集的数据准备和模型选择，它还从在类似数据集上表现良好的模型中学习。表现最好的模型聚集在一个集合中。

![](img/b2d2d18873e720ee23ad2abc04624d54.png)

来源:[高效健壮的自动化机器学习](http://papers.nips.cc/paper/5872-efficient-and-robust-automated-machine-learning.pdf)，2015。图片免费分享。

在高效的实现之上，`auto-sklearn`需要最少的用户交互。用`pip install auto-sklearn`安装库。

可以使用的主要类是`AutoSklearnClassifier`和`AutoSklearnRegressor`，它们分别操作分类和回归任务。两者都具有相同的用户指定参数，其中最重要的参数涉及时间约束和集合大小。

点击查看更多 AutoSklearn 文档[。如果您遇到安装问题，请查看一些线程:](https://automl.github.io/auto-sklearn/master/)[安装 pyfyr](https://github.com/automl/random_forest_run/issues/19) 的问题，[pyfyr](https://github.com/automl/auto-sklearn/issues/523)的构建轮失败。

## 2 | TPOT

TPOT 是另一个自动化建模管道的 Python 库，更强调数据准备以及建模算法和模型超参数。它通过一种称为基于树的管道优化工具(TPOT)的基于进化树的结构来自动进行特征选择、预处理和构建，该工具可以自动设计和优化机器学习管道 (TPOT 纸)

![](img/30d2d47a12450557f1d4557bfc9c0789.png)

来源:[基于树的自动化数据科学管道优化工具评估](https://arxiv.org/pdf/1603.06212v1.pdf)，2016。图片免费分享。

程序或管道用树来表示。遗传程序选择和进化某些程序，以最大化每个自动化机器学习管道的最终结果。

正如佩德罗·多明戈斯所说，“拥有大量数据的愚蠢算法胜过拥有有限数据的聪明算法。”事实的确如此:TPOT 可以生成复杂的数据预处理管道。

![](img/dd6b356e598322115f4cac775d5c3c21.png)

潜在管道。来源: [TPOT 文件](https://epistasislab.github.io/tpot/)。图片免费分享。

正如许多 AutoML 算法一样，TPOT 管道优化器可能需要几个小时才能产生很好的结果(除非数据集很小)。例如，你可以在 Kaggle commits 或 Google Colab 中运行这些长程序。

也许 TPOT 最好的特性是它将你的模型导出为 Python 代码文件，供以后使用。

点击查看 TPOT 文档[。这个库提供了很多可定制性和深度特性。在此](https://epistasislab.github.io/tpot/)查看使用 TPOT [的示例，包括在没有任何深度学习的情况下，在 MNIST 数据集的子样本上实现了 98%的准确率——只有标准`sklearn`模型。](https://epistasislab.github.io/tpot/examples/)

## 3 |远视

HyperOpt 是一个用于贝叶斯优化的 Python 库，由 James Bergstra 开发。该库专为大规模优化具有数百个参数的模型而设计，明确用于优化机器学习管道，并提供跨几个内核和机器扩展优化过程的选项。

> 我们的方法是揭示如何从超参数计算性能度量(例如，验证示例的分类准确度)的基础表达式图，超参数不仅控制如何应用各个处理步骤，甚至控制包括哪些处理步骤。 *-来源:* [*做模型搜索的科学:视觉架构的数百维超参数优化*](https://dl.acm.org/doi/10.5555/3042817.3042832) *。*

然而，[hyperpt](http://hyperopt.github.io/hyperopt/)很难直接使用，因为它技术性很强，需要仔细指定优化程序和参数。相反，建议使用 HyperOpt-sklearn，它是 HyperOpt 的一个包装器，包含了 sklearn 库。

具体来说，HyperOpt 非常关注进入特定模型的几十个超参数，尽管它支持预处理。考虑一个 HyperOpt-sklearn 搜索的结果，该结果导致没有预处理的梯度增强分类器:

构建 HyperOpt-sklearn 模型的文档可以在[这里](http://hyperopt.github.io/hyperopt-sklearn/)找到。它比 auto-sklearn 要复杂得多，也比 TPOT 多一点，但是如果超参数很重要的话，这可能是值得的。

## 4 | AutoKeras

神经网络和深度学习明显比标准的机器学习库更强大，因此更难以自动化。

*   使用 AutoKeras，神经架构搜索算法可以找到最佳架构，如一层中的神经元数量、层数、要合并哪些层、特定于层的参数，如过滤器大小或丢弃的神经元百分比等。搜索完成后，您可以将该模型用作普通的 TensorFlow/Keras 模型。
*   通过使用 AutoKeras，您可以构建一个包含嵌入和空间缩减等复杂元素的模型，否则那些仍在学习深度学习的人就不太容易获得这些元素。
*   当 AutoKeras 为您创建模型时，许多预处理(如矢量化或清理文本数据)都已为您完成并优化。
*   启动和训练搜索需要两行代码。AutoKeras 拥有一个类似 Keras 的界面，所以根本不难记忆和使用。

凭借对文本、图像和结构化数据的支持，以及面向初学者和寻求更多技术知识的人的界面，AutoKeras 使用进化的[神经架构搜索](/if-youre-hyped-about-gpt-3-writing-code-you-haven-t-heard-of-nas-19c8c30fcc8a?source=your_stories_page---------------------------)方法来为您消除困难和歧义。

尽管运行 AutoKeras 需要很长时间，但有许多用户指定的参数可用于控制运行时间、探索的模型数量、搜索空间大小等。

考虑这个使用 AutoKeras 生成的文本分类任务的架构。

点击阅读关于在结构化、图像和文本数据上使用 AutoKeras 的教程[。点击](/automl-creating-top-performing-neural-networks-without-defining-architectures-c7d3b08cddc)查看 AutoKeras 文档[。](https://autokeras.com/)

## 比较—我应该使用哪一个？

*   如果你优先考虑一个干净、简单的界面和相对快速的结果，使用`auto-sklearn`。另外:与`sklearn`的自然集成，与常用的模型和方法一起工作，对时间有很多控制。
*   如果你优先考虑的是高精度，不考虑潜在的长训练时间，使用`TPOT`。强调高级预处理方法，通过将管道表示为树结构来实现。额外收获:为最佳模型输出 Python 代码。
*   如果您优先考虑的是高精度，而不考虑潜在的长训练时间，请使用`HyperOpt-sklearn`。强调模型的超参数优化，这可能是也可能不是有效的，取决于数据集和算法。
*   如果你的问题需要神经网络，使用`AutoKeras`(警告:[不要高估它们的能力](/when-and-why-tree-based-models-often-outperform-neural-networks-ceba9ecd0fd8?source=---------5------------------))，特别是如果它以文本或图像的形式出现。训练确实需要很长时间，但是提供了广泛的措施来控制时间和搜索空间的大小。

感谢阅读！祝你一路顺风。😃