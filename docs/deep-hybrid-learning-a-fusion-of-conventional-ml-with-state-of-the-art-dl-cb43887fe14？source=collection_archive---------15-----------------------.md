# 深度混合学习——传统 ML 与最先进 DL 的融合

> 原文：<https://towardsdatascience.com/deep-hybrid-learning-a-fusion-of-conventional-ml-with-state-of-the-art-dl-cb43887fe14?source=collection_archive---------15----------------------->

## 一种融合机器学习和深度学习的方法

![](img/b9bd8947031c0d33bc82baf202b7caf1.png)

图片来源: [Pixabay](https://pixabay.com/illustrations/hexahedron-atoms-models-balls-2070931/)

考虑到非结构化数据分析的最新方法，深度学习在提出复杂的算法和模型架构、从非结构化数据中自动展开特征以及为现实世界的问题提供更现实的解决方案方面发挥着极其重要的作用。在我们开始讨论之前，如果你想看一看我在 [GIDS AI/ML 2020 大会](https://aditya-bhattacharya.net/events/2/)上展示的与深度混合学习相关的会议记录，请看看:

[](https://www.amazon.com/Applied-Machine-Learning-Explainability-Techniques/dp/1803246154?_encoding=UTF8&pd_rd_w=Wr6SJ&content-id=amzn1.sym.716a1ed9-074f-4780-9325-0019fece3c64&pf_rd_p=716a1ed9-074f-4780-9325-0019fece3c64&pf_rd_r=6P2PM599T97MRG7NZD9J&pd_rd_wg=m4qUW&pd_rd_r=6e349d93-5ba0-4bfe-9055-905c0153fe58&linkCode=li3&tag=adib0073-20&linkId=35506e1847de5c011fc57aa66c2b1d8e&language=en_US&ref_=as_li_ss_il) [## 应用机器学习可解释技术:使 ML 模型可解释和可信…

### 应用机器学习可解释技术:使 ML 模型可解释和可信赖的实践…

www.amazon.com](https://www.amazon.com/Applied-Machine-Learning-Explainability-Techniques/dp/1803246154?_encoding=UTF8&pd_rd_w=Wr6SJ&content-id=amzn1.sym.716a1ed9-074f-4780-9325-0019fece3c64&pf_rd_p=716a1ed9-074f-4780-9325-0019fece3c64&pf_rd_r=6P2PM599T97MRG7NZD9J&pd_rd_wg=m4qUW&pd_rd_r=6e349d93-5ba0-4bfe-9055-905c0153fe58&linkCode=li3&tag=adib0073-20&linkId=35506e1847de5c011fc57aa66c2b1d8e&language=en_US&ref_=as_li_ss_il) 

现在，深度学习受欢迎的主要原因之一是因为它消除了对非结构化数据进行手动特征工程的需要，这是非常困难的，并且几乎所有经典的机器学习算法都依赖于此。在经典的机器学习方法中，对数据集的理解和对数据集进行特征工程的能力决定了算法的最终性能和准确性。另一方面，由完全连接的神经网络层驱动的深度学习模型的最终分类或聚类层可能会在被馈送“较少”数据时导致过拟合，或者甚至在大多数时候，这些模型需要不必要的计算能力和资源的使用，这在经典的机器学习算法中是不存在的。

[](https://www.amazon.com/Applied-Machine-Learning-Explainability-Techniques/dp/1803246154?_encoding=UTF8&pd_rd_w=Wr6SJ&content-id=amzn1.sym.716a1ed9-074f-4780-9325-0019fece3c64&pf_rd_p=716a1ed9-074f-4780-9325-0019fece3c64&pf_rd_r=6P2PM599T97MRG7NZD9J&pd_rd_wg=m4qUW&pd_rd_r=6e349d93-5ba0-4bfe-9055-905c0153fe58&linkCode=li3&tag=adib0073-20&linkId=35506e1847de5c011fc57aa66c2b1d8e&language=en_US&ref_=as_li_ss_il) [## 应用机器学习可解释技术:使 ML 模型可解释和可信…

### 应用机器学习可解释技术:使 ML 模型可解释和可信赖的实践…

www.amazon.com](https://www.amazon.com/Applied-Machine-Learning-Explainability-Techniques/dp/1803246154?_encoding=UTF8&pd_rd_w=Wr6SJ&content-id=amzn1.sym.716a1ed9-074f-4780-9325-0019fece3c64&pf_rd_p=716a1ed9-074f-4780-9325-0019fece3c64&pf_rd_r=6P2PM599T97MRG7NZD9J&pd_rd_wg=m4qUW&pd_rd_r=6e349d93-5ba0-4bfe-9055-905c0153fe58&linkCode=li3&tag=adib0073-20&linkId=35506e1847de5c011fc57aa66c2b1d8e&language=en_US&ref_=as_li_ss_il) 

> 那么，如果我们尝试利用这两种方法的优点，并将它们融合在一起，会怎么样呢？

这可以通过**深度混合学习**来实现，深度混合学习是合成的融合网络，可以通过深度学习和机器学习相结合来实现。在本文中，我们将学习如何使用**深度混合学习**，其中我们将使用*深度学习方法从非结构化数据中生成或提取特征*和*使用经典机器学习方法使用非结构化数据构建高度准确的分类模型。*因此，使用**深度混合学习(DHL)** —我们可以利用 DL 和 ML 的优点，减少这两种技术的缺点，并提供更准确、计算成本更低的解决方案。

我们将尝试应用 **DHL** 来解决斯坦福大学 ML 小组公开发布的 [CheXpert 数据集](https://stanfordmlgroup.github.io/competitions/chexpert/)的图像分类问题。简而言之，该项目是关于*自动放射图形图像分析，以诊断各种病理。*换句话说，我试图从胸部 x 光图像中对不同的疾病状况进行分类*。* [**代码演练在我的个人网站有。**](https://aditya-bhattacharya.net/2020/07/27/radiographic-image-analysis/2/)

![](img/81ee7eb83517b4dd7c9640a8983979fc.png)

来自 [CheXpert 数据集](https://stanfordmlgroup.github.io/competitions/chexpert/)的胸部 x 光图像

根据我们的方法，我们将主要尝试检查两件事:

> 1.使用独立的 DNN 或 ML 模型检查 DHL 的准确性
> 
> 2.用 DNN 模型检查算法的计算时间

但是让我们讨论一个简单的 DHL 的模型架构，如下所示:

![](img/4c44691e2cb82e4c046b99655082ed9c.png)

简单的深度混合网络模型架构(图片由作者提供)

从上图中，我们可以看到一个简单的深度混合网络模型架构。这里，DNN 层只有 4 层，然后是 ML 分类层，我在这里试验了像 AdaBoost，XGBoost 和 Random Forest 这样的 ML 分类算法。对于更复杂的问题解决和更好的模型性能，我建议使用更复杂的 DNN 层，但对于本文，我们的目的不是建立一个超级精确的模型，而是研究 DHL 相对于 ML 和 DL 的改进。

现在，简单的独立 DNN 模型架构如下所示:

![](img/5cad9c34ff8bd4908f63377148d13f44.png)

简单的 DNN 模型建筑，这将是与 DHN 进行比较的基础

如果我们仔细观察，DNN 层在两种情况下实际上是相同的。 [**代码演练在我个人网站有。**](https://aditya-bhattacharya.net/2020/07/27/radiographic-image-analysis/2/) 现在，当我们在大约 10，000 幅图像上训练这两个模型并在 2000 幅图像上检查模型性能时，考虑到在两种情况下用于训练模型和生成特征的时期数是相同的(60 个时期)并且在两种情况下其他超参数调整是一致的，以下是对两种方法观察到的混淆矩阵和 AUC 分数:

1.  **对于独立的 DNN 型号**

![](img/f692eebb1e02d7039a4ada398742f44e.png)

独立 DNN 模型的模型评估指标(图片由作者提供)

使用准确度、精确度、召回率、F1 分数、AUC 分数和混淆度量的模型评估度量在 2000 幅测试图像上获得的结果看起来确实很有希望，但是我们将主要使用 AUC 分数和混淆矩阵结果来比较这两种方法。

在我的系统中，60 个时期的总训练时间花费了将近 45 分钟用于训练数据。

2.**对于 DHN-阿达布模型**

![](img/8654b13d34c3475298e05e622163e3ea.png)

DHN-AdaBoost 模型的模型评估指标(图片由作者提供)

我们确实看到了更好的结果！几乎所有的参数都增加了 6–7 %,这实际上是性能上的重大改进。

现在，您是否想知道在同一台机器上完成培训过程需要多长时间？大概花了 33 分钟！所以，现在我们看到这种方法稍微快一点！

如果我们尝试 DHN 的另一种变体呢？

3.**针对 DHN-XGBoost 车型**

![](img/fd0873e5db6cd9dcdfe22b73d2a09b57.png)

DHN-XGBoost 模型的模型评估指标(图片由作者提供)

我们再次看到一些有趣的结果。这种变体，虽然表现略好于独立的 DNN 模型，但不知何故，这种方法的训练时间几乎是 48 分钟，这比我们看到的 DHN-阿达布斯和独立的 DNN 模型。

现在，如果我们尝试一种像随机森林这样的非增强技术作为最终分类层，会怎么样呢？模型的表现会更好吗？让我们来了解一下！

4.**对于 DHN-随机森林模型**

![](img/fe04be8cb0632f7a2b4a15065b2e35bd.png)

DHN-RF 模型的模型评估指标(图片由作者提供)

不幸的是，在这种情况下，与其他变体相比，结果相当差。但是训练时间很少！同样的训练系统，同样的配置，整个训练过程在 30 分钟内就结束了！ [**代码演练在我个人网站有。**](https://aditya-bhattacharya.net/2020/07/27/radiographic-image-analysis/2/)

那么我们可以完全排除这个变种吗？

嗯，也许不是，因为可能有某些数据集，这个变种的 DHN 可能会工作得很好！ [**如果你想看看代码片段，请访问我的博客文章。**](https://aditya-bhattacharya.net/2020/07/27/radiographic-image-analysis/2/)

总的来说，我们确实看到[深度混合网络](https://aditya-bhattacharya.net/2020/07/27/radiographic-image-analysis/2/)在 AUC 指标和模型训练时间方面实际上比独立的 DNN 模型表现得更好。并且 DHN-AdaBoost 变体表现最好。但是你对这个概念和过程感兴趣吗？你有兴趣自己实现它并检查结果吗？如果这两个问题的答案都是肯定的，请给我留言或评论，看看这篇文章的[代码演练](https://aditya-bhattacharya.net/2020/07/27/radiographic-image-analysis/2/)，您就可以亲自尝试一下，充分利用深度混合学习的优势！

*因此，这就把我们带到了本文的结尾。在* [*我的个人网站*](https://aditya-bhattacharya.net/) *中，我试图提供更多关于* [*深度杂交网络*](https://aditya-bhattacharya.net/2020/07/27/radiographic-image-analysis/2/) *关于其他数据集和其他用途的细节。希望我能帮上忙！继续关注:【https://medium.com/@adib0073】[](https://medium.com/@adib0073)**和我的网站:*[https://www.aditya-bhattacharya.net/](https://aditya-bhattacharya.net/)**

*[](https://www.amazon.com/Applied-Machine-Learning-Explainability-Techniques/dp/1803246154?_encoding=UTF8&pd_rd_w=Wr6SJ&content-id=amzn1.sym.716a1ed9-074f-4780-9325-0019fece3c64&pf_rd_p=716a1ed9-074f-4780-9325-0019fece3c64&pf_rd_r=6P2PM599T97MRG7NZD9J&pd_rd_wg=m4qUW&pd_rd_r=6e349d93-5ba0-4bfe-9055-905c0153fe58&linkCode=li3&tag=adib0073-20&linkId=35506e1847de5c011fc57aa66c2b1d8e&language=en_US&ref_=as_li_ss_il) [## 应用机器学习可解释技术:使 ML 模型可解释和可信…

### 应用机器学习可解释技术:使 ML 模型可解释和可信赖的实践…

www.amazon.com](https://www.amazon.com/Applied-Machine-Learning-Explainability-Techniques/dp/1803246154?_encoding=UTF8&pd_rd_w=Wr6SJ&content-id=amzn1.sym.716a1ed9-074f-4780-9325-0019fece3c64&pf_rd_p=716a1ed9-074f-4780-9325-0019fece3c64&pf_rd_r=6P2PM599T97MRG7NZD9J&pd_rd_wg=m4qUW&pd_rd_r=6e349d93-5ba0-4bfe-9055-905c0153fe58&linkCode=li3&tag=adib0073-20&linkId=35506e1847de5c011fc57aa66c2b1d8e&language=en_US&ref_=as_li_ss_il) 

****更新**如果你喜欢这篇文章，并希望更多地支持我为社区所做的贡献，请看看我的书“** [**【应用机器学习可解释技术**](https://www.amazon.com/dp/1803246154/ref=as_sl_pc_as_ss_li_til?tag=adib0073-20&linkCode=w00&linkId=74817cc3f53ad04fe861a6ed9f619830&creativeASIN=1803246154) **”，这是 GitHub 资源库，其中包含许多关于书中各个章节的实践教程:**[**https://GitHub . com/packt publishing/Applied-Machine-Learning-explability-Techniques**](https://github.com/PacktPublishing/Applied-Machine-Learning-Explainability-Techniques)**。如果你喜欢 GitHub 资源库中提供的教程，请在资源库中做 fork 和 star，以示你对这个项目的支持！请订购本书的** [**实体本**](https://www.amazon.com/dp/1803246154/ref=as_sl_pc_as_ss_li_til?tag=adib0073-20&linkCode=w00&linkId=74817cc3f53ad04fe861a6ed9f619830&creativeASIN=1803246154) **或** [**电子本**](https://www.amazon.com/dp/B0B2PTF5PC/ref=as_sl_pc_as_ss_li_til?tag=adib0073-20&linkCode=w00&linkId=e180f73d5590baee93582fc1a8b17d15&creativeASIN=B0B2PTF5PC) **以示支持。***