# 关于波士顿住房数据集，你不知道的是

> 原文：<https://towardsdatascience.com/things-you-didnt-know-about-the-boston-housing-dataset-2e87a6f960e8?source=collection_archive---------9----------------------->

## 我们都一直在用不正确的数据训练我们的机器学习模型。哎呀！

![](img/b962a05dc754c48e48b4c1104db56f2a.png)

晚上的波士顿。照片由莫希特·辛格在 Unsplash 拍摄

如果你正在学习数据科学，你可能会碰到波士顿住房数据集。事实上，我敢打赌你会尝试谷歌如何拟合一个线性回归模型，而不是 T2。Scikit-learn 甚至允许您直接使用`sklearn.datasets`导入它，以及其他经典数据集。

波士顿住房数据集很小，尤其是在当今的大数据时代。但是曾经有一段时间，整齐地收集和标记的数据是非常难以访问的，所以像这样一个公开可用的数据集对研究人员来说是非常有价值的。虽然我们现在有像 Kaggle 和开放政府倡议这样的东西，给了我们大量的数据集可供选择，但这一个是机器学习实践的主要内容，就像巧克力是分手。

数据集中的 506 行中的每一行都描述了波士顿的一个郊区或城镇，它有 14 列，包含诸如每个住宅的平均房间数、师生比和人均犯罪率等信息。最后一行描述了业主自住房屋的中值价格(这不包括出租的房屋)，当我们使用它进行回归任务时，它通常是我们试图预测的行。

![](img/ee7341da1fe0b76bf0b24c8674eb67e6.png)

scikit-learn 对数据集的描述，包括列的名称和描述。看到语言有什么问题吗？我们将在下面解决这个问题。[来源](https://scikit-learn.org/stable/datasets/index.html)

# 怎么变得如此无处不在？

这个数据集中包含的数据是由美国人口普查局收集的，它首次出现在统计分析的历史上，是由小大卫·哈里森和丹尼尔·l·鲁宾菲尔德撰写的一篇论文，名为[享乐房价和对清洁空气的需求](https://www.researchgate.net/publication/4974606_Hedonic_housing_prices_and_the_demand_for_clean_air) [1]。研究人员有一个假设，即人们愿意为清洁的空气支付更多的钱——因此有了“享乐定价”这个术语，在这种情况下，它被用来描述人们分配给因素的货币价值，这些因素不是房地产固有的，而是其周围地区的——但对于如何衡量它存在争议。哈里森和鲁宾菲尔德担心:

> *虽然有几项研究使用了[住房市场方法]来估计空气质量改善的需求，但他们很少关注结果对程序中隐含的假设的敏感性。*

![](img/dd9a28c90e1e58edc5c0d8ff5ce44755.png)

一切开始的地方。[来源](https://www.researchgate.net/publication/4974606_Hedonic_housing_prices_and_the_demand_for_clean_air)

为了证明他们的观点，他们需要一个包含大量变量的干净的数据库和一个精确的空气污染测量方法。他们在“1970 年波斯顿标准大都市统计区(SMSA)人口普查区域的数据”中发现了这一点。他们强调数据的质量以符合他们的目的:

> 我们的数据库优于其他数据库，因为它包含大量的邻域变量(隔离空气污染的独立影响所必需的)和更可靠的空气污染数据。

这可能是它如此受欢迎的原因。接下来它被用于 Belsley，Kuh & Welsch (1980)，[‘回归诊断:识别共线性的有影响的数据和来源’](https://onlinelibrary.wiley.com/doi/book/10.1002/0471725153)[2]，并且从那以后它一直被用于基准算法。

# 原始数据集的一些问题以及如何修复它们

## 语言

首先，一个社会学的考虑。当我们转到数据集的规范描述时，在列**‘B’下，它说:*‘1000(Bk—0.63)其中 Bk 是城镇中*黑人(原文如此)*的比例*。这种语言已经过时，可能具有攻击性，因此在描述该数据集时请避免使用。“非裔美国人”或“黑人”是更容易接受的称呼。**

## **经过检查的数据**

**至于数据的质量，数据集相当强大，但并不完美。当你绘制数据散点图时，你会很快注意到房价似乎被限制在 50 英镑。这是因为人口普查局审查了数据。审查数据意味着限制变量可能值的范围。在这种情况下，他们决定将价格变量的最大值设置为 50k USD，因此任何价格都不能超过该值。**

**![](img/825b7de7875618d7d582cbbda133110b.png)**

**来源:[我自己的分析](https://github.com/martinacantaro/data_science/blob/master/datacamp_machine_learning_scientist_with_python/01_supervised_learning_with_scikit-learn/03_Boston_housing.ipynb)**

**这就是为什么，当你可视化数据时，你会看到一个天花板，将你的数据点拉平到 50。实际上，这些价格可能更高。奥蒂斯·w·吉利在他关于哈里森和鲁宾菲尔德数据的论文[中列出了属于这一类别的 16 个案例](https://spatial-statistics.com/pace_manuscripts/jeem_ms_dir/pdf/fin_jeem.pdf)【3】。**

**几十年后，很可能无法找出这些房产的实际价格。尽管这在总数中只占很小的比例，但注意它对算法训练结果的潜在影响是很重要的。**

## **数据不正确**

**更重要的是，Otis W. Gilley 还根据原始普查数据重新检查了所有数据，并发现中值列中的八个中值是简单的，根本就是错误的！**

**这是他的发现:**

**![](img/9e22ed5ccf58ed6a0c309be6b71416fa.png)**

**资料来源:Gilley (1996 年)[关于哈里森和 Rubinfeld 的数据](https://spatial-statistics.com/pace_manuscripts/jeem_ms_dir/pdf/fin_jeem.pdf)**

**吉利继续修正数据集，运行快乐定价的原始论文的计算，并检查结果是否仍然成立。幸运的是，对于数据科学的历史来说，没有重大的变化。**

> ***R2 测量的拟合优度在使用修正的观测值时有所上升。然而，系数的大小变化不大，原始回归的定性结果仍然有效。***

**尽管如此，从现在开始，数据科学和研究团体最好使用修正的数据，以避免对结果的任何潜在影响。但是，有没有一种简单的方法可以做到这一点呢？是啊！**

**好消息是，罗杰·比万、雅各布·诺沃萨德和罗宾·洛夫莱斯发布了一个[修正的波士顿住房数据库](https://nowosad.github.io/spData/reference/boston.html)，其中包括原始中值价格(在原始列名“MEDV”下)和修正后的中值价格(称为“CMEDV”)。它们还包括以前没有的数据，即每次观测的纬度和经度，这使得更多的绘图成为可能。我认为这应该是我们未来几年新的波士顿住房参考数据集，这就是为什么我在 scikit-learn 的 git 存储库上做一个 pull 请求来解决这个问题。希望它能很快为每个人所用。**

# **波士顿住房数据集的未来**

**像“hello world”或“foo，bar，baz”一样，波士顿住房数据集已经成为常用词汇的一部分。它将继续如此，不仅因为完全标记的机器学习数据集仍然不容易找到，而且因为使用相同的数据集几十年来测试不同的算法允许科学家控制那个变量并强调算法性能的差异。**

**通过本文中描述的小改进和考虑，这个经典数据集在统计学和数据科学的未来仍然有一席之地，并且肯定会在未来几年继续作为一块画布来磨练新手和有经验的实践者的技能。**

**![](img/b4fa3e37cfe48bcf1ca8c92465dd417e.png)**

**波士顿橡子街。[来源](https://unsplash.com/photos/ZLN2WOVpjCo)**

# **参考资料:**

**[1] Harrison，David & Rubinfeld，Daniel，[享乐房价与对清洁空气的需求。环境经济与管理杂志](https://www.researchgate.net/publication/4974606_Hedonic_housing_prices_and_the_demand_for_clean_air) (1978)，环境经济与管理杂志**

**[2] D. A. Belsley，E. Kuh，R. E. Welsch，[回归诊断:识别有影响的数据和共线性来源](https://onlinelibrary.wiley.com/doi/book/10.1002/0471725153) (1980)，Wiley 编辑。**

**[3] O. W. Gilley，[关于 Harrison 和 Rubinfeld 的数据](https://spatial-statistics.com/pace_manuscripts/jeem_ms_dir/pdf/fin_jeem.pdf) (1996)，环境经济与管理杂志**