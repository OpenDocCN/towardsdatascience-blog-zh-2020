# 每个数据科学家都应该避免的 5 个错误

> 原文：<https://towardsdatascience.com/5-mistakes-every-data-scientist-should-avoid-bcc8142d7693?source=collection_archive---------30----------------------->

## 数据科学项目中需要采取的预防措施

![](img/3ecc749a38e08f83a15fb85879a7e8b3.png)

[图像来源](https://images.unsplash.com/photo-1506702315536-dd8b83e2dcf9?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1500&q=80)

当一个具有挑战性的数据科学项目到来时，我们有时会在兴奋或试图做出一个完美的模型时，以犯错误而告终。不时地反复检查工作并对工作保持谨慎是很重要的。在这篇博客中，我将讨论数据科学家经常犯的五个错误。

# 1.数据泄露

数据泄漏是当模型根据预测时不可用的信息进行训练时发生的情况。结果可能是一个模型在训练和测试时表现很好，但在现实生活中性能会急剧下降。原因有时是意外地在测试和训练数据集之间共享信息，而不是保持测试数据不可见和不被触及。

一些可能的数据泄漏包括:

## 在预处理期间

当使用 [PCA](https://en.wikipedia.org/wiki/Principal_component_analysis) (主成分分析)进行降维时，我们通过从使用训练数据创建的协方差矩阵中学习特征向量，将 n 维数据降维到 k 维(k < n)。通过在前 k 个特征向量的方向上投影 n 维数据，训练数据被变换到 k 维空间。然后我们训练模型。

在测试过程中，正确的方法是通过将 n 维测试数据投影到前一步学习的前 k 个特征向量的方向上，将测试数据转换到 k 维空间，然后进行模型预测。**但是，如果错误地将 PCA 应用于整个数据(训练+测试数据)**，会造成不必要的数据泄漏，模型性能会受到影响。

类似地，当应用像 [NMF](https://en.wikipedia.org/wiki/Non-negative_matrix_factorization) (非负矩阵分解)这样的算法时，关于如何学习这两个矩阵的训练发生在训练期间。在测试时，只使用学习将测试数据转换成所需的矩阵。**但是，如果错误地将 NMF 应用于整个数据(训练+测试数据)**，**就会造成不必要的数据泄漏。**阅读这篇写得非常好的[博客](/data-leakage-in-machine-learning-10bdd3eec742)了解更多关于数据泄露的细节。

# 2.过多的冗余功能

具有高度相关的要素会影响模型的性能。这也被称为维数灾难。由于拥有过多冗余功能而面临的主要问题是:

1.  [**过拟合**](https://en.wikipedia.org/wiki/Overfitting) :多余的特征试图过拟合模型，导致高方差。这也会影响模型在测试期间的准确性。
2.  **没有清晰的特征重要性图**:需要了解哪些预测因子对理解特征重要性贡献更大。这些信息有助于数据科学故事讲述和要素排名。由于具有冗余特征，模型表现异常，并且特征重要性的清晰图像丢失。
3.  **计算时间**:随着冗余特征导致的特征空间增加，计算时间增加。在我们需要快速结果的情况下，这可能会导致一个问题，因为许多模型的运行时间与特征的数量成平方关系。

冗余功能过多的问题可以通过以下方式避免:

1.  **增加特性**:主要思想是保持模型简单。使用向前和向后选择等技术逐步添加要素。我们只希望重要的特性在最后成为模型的一部分。
2.  **检查** [**多重共线性**](https://en.wikipedia.org/wiki/Multicollinearity) **并删除冗余特征**:我们可以检查特征的相关性并删除冗余特征。
3.  **执行降维技术**:像 [PCA](https://en.wikipedia.org/wiki/Principal_component_analysis?wprov=srpw1_0) 、[自动编码器](https://en.wikipedia.org/wiki/Autoencoder?wprov=srpw1_0)、 [NMF](https://en.wikipedia.org/wiki/Non-negative_matrix_factorization) 这样的降维技术有时在去除冗余信息时非常有用。
4.  正则化:每当模型试图过度拟合时，正则化技术就会惩罚它。点击阅读更多信息[。](https://www.analyticsvidhya.com/blog/2015/02/avoid-over-fitting-regularization/)

# 3.未来不可用的功能

很多时候，为了改进训练数据，我们会使用在测试过程中可能无法直接或立即获得的特性。

我在工作中遇到的一个这样的例子是天气数据。如果我们将天气数据用于预测应用，我们应该记住，在预测期间，未来的实际天气数据是不可用的，我们能得到的最好的是天气预测。训练期间的实际天气数据和测试/实际运行期间的可用天气预测可能会有很大偏差，从而降低模型性能。

# 4.错误的数据结构选择和糟糕的编码风格

我曾经遇到过由于错误的数据结构选择导致应用程序速度大幅下降的问题。

> 如果应用程序需要快速查找元素的存在，请使用 Dictionary 或 python 中的 Set，避免使用 List。

字典/集合是具有快速搜索需求的应用程序的正确数据结构

正确的数据结构使具有搜索功能的应用程序速度更快

谈到编码风格，最佳实践是拥有模块化的代码以及良好的变量和函数命名风格。它增加了可理解性、可解释性，并更容易地合并新的需求和变更。查看[此链接](https://en.wikipedia.org/wiki/Best_coding_practices)了解最佳编码实践。

# 5.在验证多个测试周期、数据和人口统计数据的结果之前，报告模型性能

很多时候，数据科学家在有限的数据中进行测试后，最终会报告模型性能。在满怀信心地揭示模型性能之前，模型需要在生产中的大量测试数据中进行测试。

1.  如果这是一个预测项目，测试模型在多个时间段的性能/预测。
2.  如果这是一个金融营销模型，测试模型在不同人口统计的不同市场中的性能。
3.  如果是零售领域模型，测试跨类别、部门等的模型性能

# 结论

在这篇博文中，我想指出数据科学家最终会犯的 5 个常见但关键的错误。通过这个博客，一点点的谨慎和意识可以拯救你，并有望让事情变得简单一点。如果你觉得还有更多的东西可以添加到常犯的错误列表中，请发表评论。

***我的 Youtube 频道更多内容:***

[](https://www.youtube.com/channel/UCg0PxC9ThQrbD9nM_FU1vWA) [## 阿布舍克·蒙戈利

### 嗨，伙计们，欢迎来到频道。该频道旨在涵盖各种主题，从机器学习，数据科学…

www.youtube.com](https://www.youtube.com/channel/UCg0PxC9ThQrbD9nM_FU1vWA) 

> **关于作者-:**
> 
> Abhishek Mungoli 是一位经验丰富的数据科学家，拥有 ML 领域的经验和计算机科学背景，跨越多个领域并具有解决问题的思维方式。擅长各种机器学习和零售业特有的优化问题。热衷于大规模实现机器学习模型，并通过博客、讲座、聚会和论文等方式分享知识。
> 
> 我的动机总是把最困难的事情简化成最简单的版本。我喜欢解决问题、数据科学、产品开发和扩展解决方案。我喜欢在闲暇时间探索新的地方和健身。在 [**中**](https://medium.com/@mungoliabhishek81) 、**[**Linkedin**](https://www.linkedin.com/in/abhishek-mungoli-39048355/)**或**[**insta gram**](https://www.instagram.com/simplyspartanx/)**关注我，查看我[以前的帖子](https://medium.com/@mungoliabhishek81)。我欢迎反馈和建设性的批评。我的一些博客-********

*   ******[以简单&直观的方式分解时间序列](/decomposing-a-time-series-in-a-simple-and-intuitive-way-19d3213c420b?source=---------7------------------)******
*   ******[GPU 计算如何在工作中真正拯救了我？](https://medium.com/walmartlabs/how-gpu-computing-literally-saved-me-at-work-fc1dc70f48b6)******
*   ******信息论& KL 分歧[第一部分](/part-i-a-new-tool-to-your-toolkit-kl-divergence-5b887b5b420e)和[第二部分](/part-2-a-new-tool-to-your-toolkit-kl-divergence-736c134baa3d)******
*   ******[使用 Apache Spark 处理维基百科，创建热点数据集](/process-wikipedia-using-apache-spark-to-create-spicy-hot-datasets-1a59720e6e25)******
*   ******[基于半监督嵌入的模糊聚类](/a-semi-supervised-embedding-based-fuzzy-clustering-b2023c0fde7c)******
*   ******[比较哪种机器学习模型表现更好](/compare-which-machine-learning-model-performs-better-4912b2ed597d)******