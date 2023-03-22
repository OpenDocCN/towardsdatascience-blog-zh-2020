# 用位置信息丰富 shapelets 用于时间序列分类

> 原文：<https://towardsdatascience.com/enriching-shapelets-with-positional-information-for-timeseries-classification-42b6009a54a8?source=collection_archive---------21----------------------->

## 一个简洁的技巧如何提高预测性能和可解释性。

## 时间序列分类

许多现实世界的流程会随着时间的推移产生数据，从而产生时态数据或时间序列。与表格数据相反，相邻观测值(即时间上接近的观测值)高度相关，在分析时间序列时需要特别的努力。可以对时间序列执行的一个可能的任务是对它们进行分类。示例用例包括基于加速度计数据的表面检测、基于用电量的电气设备类型分类或基于轮廓信息的树叶类型分类。

![](img/4bb3802d8bcec177758e44c43ee7805e.png)

使用加速度计数据的 X 轴检测机器人(索尼 AIBO)行走的表面。图片取自[http://www.timeseriesclassification.com/description.php?dataset = sonyaiborobotsurface 1](http://www.timeseriesclassification.com/description.php?Dataset=SonyAIBORobotSurface1)

## Shapelets

Shapelets 是小的子序列，或者时间序列的一部分，对于某个类是有信息的或者有区别的。通过计算要分类到 shapelet 的每个时间序列的距离，它们可用于将时间序列转换为要素。

![](img/6ce2fd5ad6d5346d9bb0848a3b7991b8.png)

从 ItalyPowerDemand 数据集中提取两个 shapelets，以便将时间序列转换到二维特征空间。特征空间中的每个轴代表到两个 shapelets 之一的距离。可以看出，使用这两个 shapelets 已经可以实现良好的线性分离。

## 位置信息

Guillemé等人最近发表了“[局部随机 shape let](https://link.springer.com/chapter/10.1007/978-3-030-39098-3_7)”，其中他们提出了一种提取 shape let 的方法，可用于创建由**基于距离的特征和基于位置的特征组成的特征矩阵，从而获得更好的预测性能。**

![](img/ee76b0283c114d9b0f91a47119e6fad8.png)

他们提出的技术将挖掘 K 个 shapelets，以便将 N 个时间序列转换成 N×2K 特征矩阵。该特征矩阵包括到每个小形状的距离(d(Ti，Si))和位置(l(Ti，Si))。

![](img/b0a0e8c2ff8f28dfc99326445d07dac0.png)

每个时间序列的特征通过在时间序列上滑动 shapelet 并计算每个位置的 shape let 到时间序列的欧几里德距离来计算。最后，返回最小距离和相应的位置。指定的距离和位置纯粹是概念上的，远非精确。

## GENDIS:有效地提取精确的 shapelets

我们扩展了我们的技术，[GENDIS](https://github.com/IBCNServices/GENDIS/)(shape let 的基因发现)来提取除了到 shape let 的距离之外的位置信息。由于 GENDIS 是一种遗传算法，它可以优化任何东西(它的目标函数不需要可微)。因此，这需要最少的努力，但初步的实验表明，预测性能显著提高。**通过这一新的更新，GENDIS 能够提取非常容易解释的 shapelets，并能够在输入机器学习分类器时实现最先进的预测性能。此外，与实现类似性能的技术相反，GENDIS 不需要执行强力搜索，并且不需要调整 shapelets 的数量和相应的长度。**

让我们通过创建一个位置信息是关键区别属性的人工数据集来演示新功能(因为两个类的模式是相同的):

![](img/5e4ff59a87c4e43f00b3616c2e667881.png)

生成的时间序列。这两个类的模式是相同的。区分这两个类的唯一方法是使用发现模式的位置。

我们使用这两个版本提取 shapelet，并测量我们使用具有逻辑回归分类器和默认超参数的单个 shape let 所能达到的准确度。旧版本只能达到 81%的准确率，而新版本达到 100%的准确率。我们还在三个较小的数据集上快速比较了新版本和旧版本，每一次，位置信息都显著提高了准确性。

## shapelets 的可解释性

shapelets 最有趣的特性之一是它们非常容易解释。提取的 shapelets 可以很容易地呈现给领域专家，以显示时间序列的哪些部分被准确地使用，以便做出决定。此外，通过优化小形状的位置和距离信息，提取的小形状也更容易解释。没有位置信息，GENDIS 和相关技术，如[学习时间序列 Shapelets](https://www.ismll.uni-hildesheim.de/pub/pdfs/grabocka2014e-kdd.pdf) ，能够经常“破解”那里的位置信息。Guillemé等人在他们的论文中证明:

![](img/403ada35ef217394b4d356d54a008b0e.png)

在上面的图像中，我们看到一个长时间序列(浅蓝色)，对应于地震读数，以及由 LS 学习的 shapelets，它不包含位置信息，以彩色显示。提取的 shapelet 确实实现了优秀的预测性能，但是与原始时间序列没有任何关系，使得它们难以解释(它们位于从时间序列到 shape let 的距离最小的位置)。另一方面，对位置信息进行编码的 LRS 实现了出色的预测性能，并提取了与原始时间序列非常相关的 shapelets(正如我们可以看到的，彩色子序列与原始浅蓝色时间序列非常匹配)。

这篇短文到此结束！通过扩展我们的 shapelet 提取框架，除了提取到每个 shape let 的距离之外，还提取位置信息，我们能够以有限的努力在预测性能方面获得一些显著的收益！

**如果您对 shapelets 有更多问题，或者您有一个涉及时间序列分类的用例？与我们取得联系！**

## 来源

[1] [叶，李，&等(2009 年 6 月)。一种新的数据挖掘原语。第 15 届 ACM SIGKDD 知识发现和数据挖掘国际会议论文集*。*](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.441.710&rep=rep1&type=pdf) *【2】[Lines，j .，Davis，L. M .，Hills，j .，& Bagnall，A. (2012，8 月)。用于时间序列分类的 shapelet 变换。第 18 届 ACM SIGKDD 知识发现和数据挖掘国际会议论文集*。*](https://ueaeprints.uea.ac.uk/id/eprint/40201/1/LinesKDD2012.pdf)
【3】[格拉博卡，j .，席林，n .，威斯特巴，m .，&施密特-蒂梅，L. (2014 年 8 月)。学习时序 shapelets。第 20 届 ACM SIGKDD 知识发现和数据挖掘国际会议论文集*。*](https://www.ismll.uni-hildesheim.de/pub/pdfs/grabocka2014e-kdd.pdf)
【4】[m .吉列梅，s .马林诺夫斯基，r .&x .勒纳尔(2019，9 月)。局部随机形状。在*时态数据高级分析和学习国际研讨会上*。斯普林格，查姆。](https://project.inria.fr/aaltd19/files/2019/08/AALTD_19_Guilleme.pdf)
【5】[范德维尔(Vandewiele，g .)，翁格纳内(Ongenae，f .)，&德图尔克(De Turck，f .)(2019)。GENDIS:身材的基因发现。 *arXiv 预印本 arXiv:1910.12948* 。](https://arxiv.org/pdf/1910.12948.pdf)
[www.timeseriesclassification.com](http://timeseriesclassification.com/)*