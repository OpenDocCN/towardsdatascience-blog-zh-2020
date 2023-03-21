# 综合表格数据工具和模型综述

> 原文：<https://towardsdatascience.com/a-review-of-synthetic-tabular-data-tools-and-models-d83b232aae25?source=collection_archive---------38----------------------->

## 正在彻底改变我们共享数据方式的匿名化方法

![](img/188e54a69d2943052f459671005b40e0.png)

米卡·鲍梅斯特的图片 [@mbaumi](http://twitter.com/mbaumi) 。https://unsplash.com/photos/Wpnoqo2plFA

# 数据隐私的重要性

我们生活在一个数据驱动的时代，大数据、数据挖掘和人工智能(以及其他时髦词汇)正在彻底改变我们从数据中获取价值的方式。挑战在于，私营公司和公共实体都没有办法在内部或外部轻松共享这些数据。主要障碍是:**合规法律**、**对数据滥用的担忧**、**患者/客户隐私**以及**无法安全传输数据**。如果没有这些限制，数据科学家、开发运营团队、研究小组和其他数据专业人员可以提供更高效的问题解决方案。

# 传统的数据匿名方法:数据屏蔽

在机器学习流行之前，匿名化数据的原始方法通常以牺牲数据效用为代价来产生匿名化的数据。统计属性经常被部分或完全破坏，匿名方法通常也很薄弱，容易被逆向工程破坏，从而暴露 **PII(个人身份信息)。**

# 代替

数据中的真实值被替换为不同的“真实”值。例如，用从外部姓名列表中随机选择的姓名替换一列中的所有真实姓名。在某些情况下，替换可能涉及用随机编码的字符串替换 PII，只有原始数据管理员才能将其匹配回原始记录。例如，将“John Smith”替换为“R7JxvOAjtT”。

**Pro:** 对于替换值，任何人都不可能知道真正的值是什么，因为原始值已经完全从数据集中删除了。

**Con:** 需要一个可访问的替代“真实”数据列表来执行替换。这可能需要购买精心策划的数据集，如假姓名、电话号码、地址等，这可能很昂贵。

**反对:逆向工程。**参见下面的案例研究。

# 随机化算法“洗牌”

旨在随机排列同一列中的数据顺序。与用来自*外部*源的相似值替换值的替换不同，混排可以被认为是内部替换的一种形式，它只替换同一列中的值。

**Pro:** 与其他数据屏蔽方法结合使用时可以有效。例如，在供应商名称和合同价值的给定数据集中，简单地打乱供应商名称是无效的，因为任何具有行业特定知识的人都可以拼凑出合同规模和供应商的可能组合。如果供应商名称被掩盖*和*被打乱，那么几乎不可能找出供应商。

**弊:**依赖与其他有效的掩蔽方法一起使用会破坏它的使用。

**弊:逆向工程。**当单独使用时，具有数据领域知识的攻击者可以简单地交换列值以获得原始值。

# 指零

简单地将机密数据替换为“空”值，如“NA”、“null”、“Missing”等。

**Pro:** 跨不同列实施的简单解决方案。

**反对意见:**很明显，数据被修改过，并不完全是原始的。还让用户确切地知道哪些单元丢失，如果不使用其他屏蔽方法，这使得有针对性的逆向工程攻击更容易。

**弊:**空值会使数据集难以分析，因为空的纯数字列会变成文本或字符串。

# 删除

从数据集中删除 PII 列。

**Pro:** 快速实现。

**反对:**对于应该删除哪些列，需要一定程度的主观性。定义哪些列被认为是 PII 并不总是那么简单，因为攻击者可能使用几个非 PII 列的组合来推断 PII。

**缺点**:删除几列可能会严重影响屏蔽数据集的效用。

# 掩饰

用类似“X”的替代符号模糊部分数据值。例如，信用卡号码 XXX — XXXX — XXXX — 9823。屏蔽和置零的主要区别在于，屏蔽保留了原始数据的一般格式。例如，我仍然可以看到信用卡号码是由 4 个数字“XXXX”组成的，即使我不知道它们的值。

**Pro:** 隐藏敏感数据，无需完全删除或使用置零。

**弊:**可能会遇到与零化相同的数据分析问题，因为替代符号会降低数据的效用。

# 摄动

**推荐阅读**:[https://pdfs . semantic scholar . org/f541/758 a 9179998 a1 b21d 28 D1 feb 90428 dafad 90 . pdf](https://pdfs.semanticscholar.org/f541/758a9179998a1b21d28d1feb90428dafad90.pdf)

数据扰动是一种隐私保护方法，最初是为电子健康记录设计的[1]。目标是使用假数据或重用同一数据集中的数据，将“噪声”注入数据。噪音意味着你引入了关于数据集中真实值的不确定性。**不确定性=数据屏蔽中似是而非的可否认性**因为攻击者无法知道他看到的是否是真实的数据值。有两种扰动方法。 **1)** **分布方法** —用仍然允许数据集保持相似统计属性的假值替换数据中的值。可以通过替换同一分布样本或分布本身的值来实现这一点。 **2)** **值失真** —使用乘法或加法或您选择的其他随机过程添加噪声。例如，将 1-5 之间的随机数添加到人的年龄中，或者将列表中的每个工资随机更改为真实值的+/- 10%，这样真实工资不会被披露，但趋势仍然可以观察到。

**Pro:** 高度灵活的解决方案，允许以无限多种方式向数据添加噪声，同时保持统计属性。

**反对:逆向工程。**参见下面的案例研究。

# 传统方法导致的数据泄漏:案例研究

# Netflix 奖

2006 年，网飞发起了“Netflix 奖”,这是一项设计算法来预测顾客对电影评价的在线竞赛。网飞提供了一个由 48 万用户为 17k 部电影制作的 1 亿收视率数据集。网飞对数据集进行了匿名化处理，用替换法将用户名替换成编码字符串，并用虚假评分扰乱一些评分。2008 年，德克萨斯大学奥斯汀分校的两名学生发表了“大型稀疏数据集的稳健去匿名化”一文，详细介绍了针对高维微观数据的一类新的统计去匿名化攻击。通过结合网飞的数据和 IMBD 的数据，学生们能够揭示谁是用户。在数据泄露的例子中，Netflix 奖现在无处不在。

# 斯威尼对州长维尔德[3]

**推荐阅读**:[https://dataprivacylab . org/projects/identificability/paper 1 . pdf](https://dataprivacylab.org/projects/identifiability/paper1.pdf)

1997 年，马萨诸塞州团体保险委员会公布了医院就诊数据，目的是改善医疗保健和控制成本。这篇文章引起了 Latanya Sweeney 的注意，她当时是麻省理工学院研究计算披露的研究生。时任马萨诸塞州州长的威廉·韦尔德向公众保证，PII 等名字已经从数据中删除。Sweeney 知道 Weld 州长住在剑桥，一个只有 7 个邮政编码的小镇，他有一种直觉，医院的数据可以很容易地追溯到对目标只有基本了解的个人。花 20 美元从市里买了一份选民名单，包括:姓名、地址、邮政编码、出生日期和性别。Sweeney 发现只有少数人知道 Weld 州长公开的出生日期，其中只有一个人的邮政编码与 Weld 居住的城镇相匹配。斯威尼把他自己的诊断和处方清单寄给了州长的办公室。#野蛮人。2000 年，Sweeney 发表了另一篇论文[4],指出只需要 3 条信息:邮政编码、出生日期和性别就可以识别 87%的美国人。

# 匿名数据的现代方法:差分隐私

**推荐阅读**:【https://arxiv.org/pdf/1911.12704.pdf】T4

今天，最新和最流行的数据匿名形式之一是差分隐私，它本质上是一种更加形式化和参数化的扰动形式[5]。具体来说，就是**的分配方式**。差分隐私提供了一个数学框架来量化必须注入数据集的最小噪声量，以确保数据泄漏不超过某个限制“ε”。

创建一个私人系统(一个可以匿名化你的数据的系统)的过程包括思考你的最终目标，然后逆向工作。您首先要考虑您希望共享的特定功能/数据列。然后，你决定一个正式的隐私系统，这是一套数学标准，当满足时，意味着你的数据集不能泄漏超过极限ε的数据。然后你选择你的噪声注入方法。噪声注入的方法是任意的，有许多方法和算法可以使用。重要的是，无论您选择哪种方法，您都可以将它参数化，以便它可以用来证明它满足您在隐私系统中指定的条件。当这些条件得到满足时，该系统被称为**正式私有。**

# “差异”一词在哪里起作用？

差分指的是这样一个事实，即对于给定的将噪声注入数据集的计算任务“T ”,有许多算法可以实现给定ε的期望噪声。因为有许多不同的方式来实现 epsilon 隐私，我们说数据是不同的隐私。因此，**差分隐私是一个定义，*不是*一种算法。**因此，有可能创建一个正式的私有系统(满足您的隐私系统的条件)，但*不是*差别私有，并且只有一种方式满足您的系统的噪音标准。选择使用差分隐私还是正式的非差分隐私系统取决于使用案例。此外，也可以选择使用非形式化的“特定”噪声注入，但存在创建匿名化较差的数据的风险，这些数据很容易被攻击者(例如 Netflix Prize)进行逆向工程。

# 为什么有人会选择特别的“非形式化”数据隐私？

差分隐私设置可能很复杂，也很及时。正如在匿名化水平和数据效用之间存在权衡一样，在隐私系统的努力和复杂性之间也存在权衡。差分隐私的最大问题是，当ε很小时(少量数据泄漏)，为‘T’找到精确的(很好地保持统计特性)差分隐私算法变得越来越困难。在这种情况下，有人可能会选择一个正式的无差别的私有系统。如果无法找到一个单一的算法来满足一个非差异私有系统，那么您可能会默认一个特定的、非形式化的系统作为最后的手段。

可以说，一个正式的隐私系统的最大好处之一是，它作为一种质量控制的形式，因为该系统及其标准是有文件证明的。这在医疗保健、金融和银行等隐私敏感行业非常重要。对于许多公司机构来说，除非有明确的理由、合理的理论和可追溯的实施，以经受住行业和政府审计的严格性，否则什么都不能实施。坦率地说，就今天的隐私要求而言，网飞在 12 年前实施的特设隐私系统是可笑的。然而，它是一个重要的提醒，提醒我们隐私系统已经发展到了什么程度。

> 差异隐私解决了这样一个悖论:在学习关于一个群体的有用信息的同时，却对一个个体一无所知[5]

# 差分隐私工具箱

**DP-SYN**

[https://github . com/usnistgov/privaceengcollabspace/tree/master/tools/de-identificati on/Differential-Privacy-Synthetic-Data-Challenge-Algorithms/DP syn](https://github.com/usnistgov/PrivacyEngCollabSpace/tree/master/tools/de-identification/Differential-Privacy-Synthetic-Data-Challenge-Algorithms/DPSyn)

论文:[https://github . com/usnistgov/privaceengcollabspace/blob/master/tools/de-identificati on/Differential-Privacy-Synthetic-Data-Challenge-Algorithms/DP syn/document/algorithm % 20 description . pdf](https://github.com/usnistgov/PrivacyEngCollabSpace/blob/master/tools/de-identification/Differential-Privacy-Synthetic-Data-Challenge-Algorithms/DPSyn/document/algorithm%20description.pdf)

**DP-field groups**[https://github . com/gardn 999/privaceengcollabspace/tree/master/tools/de-identificati on/Differential-Privacy-Synthetic-Data-Challenge-Algorithms/DP field groups](https://github.com/gardn999/PrivacyEngCollabSpace/tree/master/tools/de-identification/Differential-Privacy-Synthetic-Data-Challenge-Algorithms/DPFieldGroups)

论文:[https://github . com/gardn 999/privaceengcollabspace/blob/master/tools/de-identificati on/Differential-Privacy-Synthetic-Data-Challenge-Algorithms/DPFieldGroups/gardn 999 _ nistdp 3 _ writeup _ and _ Privacy _ proof . pdf](https://github.com/gardn999/PrivacyEngCollabSpace/blob/master/tools/de-identification/Differential-Privacy-Synthetic-Data-Challenge-Algorithms/DPFieldGroups/gardn999_NistDp3_writeup_and_privacy_proof.pdf)

**基于图形模型的评估**

[https://github.com/ryan112358/private-pgm](https://github.com/ryan112358/private-pgm)

论文:[https://arxiv.org/abs/1901.09136](https://arxiv.org/abs/1901.09136)

**DP-SGD**

[https://github.com/tensorflow/privacy](https://github.com/tensorflow/privacy)

论文:[https://arxiv.org/abs/1607.00133](https://arxiv.org/abs/1607.00133)

**古普特**https://github.com/prashmohan/GUPT

论文:[https://www.cs.umd.edu/~elaine/docs/gupt.pdf](https://www.cs.umd.edu/~elaine/docs/gupt.pdf)

**ARX 数据匿名工具**

[https://arx.deidentifier.org](https://arx.deidentifier.org)

**差分私有凸优化基准——各种差分私有凸优化算法的集合**

https://github.com/sunblaze-ucb/dpml-benchmark

*   **近似极小值扰动**

论文:[http://www.uvm.edu/~jnear/papers/TPDPCO.pdf](http://www.uvm.edu/~jnear/papers/TPDPCO.pdf)

*   **私有随机梯度下降**

论文:[https://arxiv.org/abs/1405.7085](https://arxiv.org/abs/1405.7085)

论文:[https://arxiv.org/pdf/1607.00133.pdf](https://arxiv.org/pdf/1607.00133.pdf)

*   **基于私有凸扰动的随机梯度下降**

论文:[https://arxiv.org/pdf/1606.04722.pdf](https://arxiv.org/pdf/1606.04722.pdf)

*   二等兵弗兰克-沃尔夫

论文:[https://arxiv.org/pdf/1411.5417.pdf](https://arxiv.org/pdf/1411.5417.pdf)

**二重唱**

[https://github.com/uvm-plaid/duet](https://github.com/uvm-plaid/duet)

论文:[https://arxiv.org/abs/1909.02481](https://arxiv.org/abs/1909.02481)

埃克泰罗

[https://github.com/ektelo/ektelo](https://github.com/ektelo/ektelo)

论文:[https://dl.acm.org/citation.cfm?id=3196921](https://dl.acm.org/citation.cfm?id=3196921)

**隐私保护应用**

[https://github . com/us dot-its-JPO-data-portal/privacy-protection-application](https://github.com/usdot-its-jpo-data-portal/privacy-protection-application)

**教师集体的私人聚集**[https://github.com/tensorflow/privacy/tree/master/research](https://github.com/tensorflow/privacy/tree/master/research)

论文:【https://arxiv.org/abs/1610.05755】T42

论文:[https://arxiv.org/abs/1802.08908](https://arxiv.org/abs/1802.08908)

**DP comp——基于网络的工具，旨在帮助从业者和研究人员评估基于 DPBench 的最新差分私有算法的准确性**

https://www.dpcomp.org

【https://github.com/dpcomp-org/dpcomp_core#dpbench 

论文:[https://people.cs.umass.edu/~dzhang/dpcomp_demo.pdf](https://people.cs.umass.edu/~dzhang/dpcomp_demo.pdf)

**DPBench —隐私算法标准化评估框架**

论文:[https://arxiv.org/abs/1512.04817](https://arxiv.org/abs/1512.04817)

**Gretel —使用神经网络创建差分私有数据的包**

[https://github.com/gretelai/gretel-synthetics](https://github.com/gretelai/gretel-synthetics)

[https://medium . com/Gretel-ai/using-generative-differential-private-models-to-build-privacy-enhanced-synthetic-datasets-c 0633856184](https://medium.com/gretel-ai/using-generative-differentially-private-models-to-build-privacy-enhancing-synthetic-datasets-c0633856184)

**推荐阅读:**

[https://towards data science . com/understanding-differential-privacy-85ce 191 e 198 a](/understanding-differential-privacy-85ce191e198a)

[https://digitalcommons.ilr.cornell.edu/ldi/49/](https://digitalcommons.ilr.cornell.edu/ldi/49/)

[https://www . science mag . org/news/2019/01/can-set-equations-keep-us-census-data-private](https://www.sciencemag.org/news/2019/01/can-set-equations-keep-us-census-data-private)

[https://www . ijstr . org/final-print/mar 2017/A-Review-Of-Synthetic-Data-Generation-Methods-For-Privacy-Preserving-Data-publishing . pdf](https://www.ijstr.org/final-print/mar2017/A-Review-Of-Synthetic-Data-Generation-Methods-For-Privacy-Preserving-Data-Publishing.pdf)

**MWEM**https://arxiv.org/abs/1012.4763

**双重查询**https://arxiv.org/abs/1402.1526

**HDMM**https://arxiv.org/abs/1808.03537

# 远离匿名方法:合成数据

随着机器学习模型变得更加复杂，关于数据匿名化的想法也发生了变化。研究小组没有将复杂的算法应用于数据集，而是尝试着教导模型识别数据集内的**模式**，然后根据模型所学生成“综合”数据。

**决策树**和**贝叶斯网络**通过对表格数据中的**离散变量**建模，提供了一种新的匿名化数据的方法，并且效果良好。随后 **copulas** 用于建模**非线性相关连续变量**。使用 copulas 的一个好处是能够对各种分布进行建模，例如**单变量数据**(高斯、贝塔、伽马、高斯 KDE、截断高斯)**双变量数据** (Clayton、Frank、Gumbel、Ali–Mikhail–Haq、Joe)和**多变量数据** (Guassian、D-Vine、C-Vine、R-Vine)。

不要脸的塞:[https://medium . com/@ Timothy pillow/introduction-to-copulas-ad 1 a3 b 83 a 297](https://medium.com/@timothypillow/introduction-to-copulas-ad1a3b83a297)

# 合成数据和匿名数据有什么区别？

**推荐阅读:**[https://www-cdn . law . Stanford . edu/WP-content/uploads/2019/01/bello vin _ 2019 01 29-1 . pdf](https://www-cdn.law.stanford.edu/wp-content/uploads/2019/01/Bellovin_20190129-1.pdf)

包括形式化隐私系统在内的传统匿名方法可以被认为是**扭曲**或**净化**技术，旨在将扭曲和不确定性直接应用于原始数据集。在某种意义上，你可以说你“将匿名化应用到衣服本身”。

合成数据的关键区别在于，结果是由原始数据集间接生成的，因为它是“学习”分布的结果。因此，尽管合成数据具有与原始数据集相似的属性，但是可以将其视为与原始数据不同的**。**

合成数据的一个问题是，如果模型很好地学习了**联合分布**，则*由于合成数据由从模型学习的分布中随机采样的**组成，因此合成数据集包含值的组合的可能性非常小，这些值完全可能对应于:与原始数据集相同的行，或者更可能对应于:可以在原始数据集中找到的行内的部分值。如果这些值对应于任何高风险 PII 数据，如邮政编码、出生日期或性别的真实组合，这尤其是个问题。因此，**将合成数据与差分隐私相结合可能会两全其美。*****

# 关于命名的快速警告

术语“合成数据”在差分隐私中被随意使用。常见术语，如“差分隐私合成数据”或“差分隐私生成的合成数据”是不明确的，因为它要么意味着 1)合成数据是使用“不同布”的方法生成的，并使用差分隐私进一步净化*或 2)* 差分隐私已用于创建对原始数据唯一的匿名化数据集，因此被假定为“合成的”。不幸的是，后一个定义是最常用的，我认为它是最模糊的。我个人认为，如果数据是“相同的布料”,它就没有被合成；已经被扭曲/消毒了。无论如何，当你看到有差别的私有数据时，要记住一点:仅仅因为它是合成的并不意味着它是有差别的私有的，仅仅因为它是有差别的私有的并不意味着数据最初是使用“不同布料”的方法合成的。

# 综合数据工具箱

# 决策树

**CART(分类和回归树)—离散变量**

[https://github.com/ColleenBobbie/Cancer-Prediction](https://github.com/ColleenBobbie/Cancer-Prediction)

论文:[https://www . SCB . se/content assets/ca 21 efb 41 fee 47d 293 bbee 5b f 7 be 7 FB 3/using-cart-to-generate-partial-synthetic-public-use-microdata . pdf](https://www.scb.se/contentassets/ca21efb41fee47d293bbee5bf7be7fb3/using-cart-to-generate-partially-synthetic-public-use-microdata.pdf)

**SDT(空间分解树)—空间数据**

论文:[https://arxiv.org/abs/1103.5170](https://arxiv.org/abs/1103.5170)

**隐私树—空间数据**

论文:[https://arxiv.org/abs/1601.03229](https://arxiv.org/abs/1601.03229)

# 贝叶斯网络

**CLBN**

论文:【https://ieeexplore.ieee.org/document/1054142 

**PrivBN(通过贝叶斯网络发布私人数据)**

论文:[http://dimacs.rutgers.edu/~graham/pubs/papers/PrivBayes.pdf](http://dimacs.rutgers.edu/~graham/pubs/papers/PrivBayes.pdf)

**数据合成器**

[https://github.com/DataResponsibly/DataSynthesizer](https://github.com/DataResponsibly/DataSynthesizer)

论文:[https://faculty . Washington . edu/bill Howe/publications/pdf/ping 17 data synthesizer . pdf](https://faculty.washington.edu/billhowe/publications/pdfs/ping17datasynthesizer.pdf)

# 连系

**SDV-Copulas(综合数据仓库)**

[https://github.com/sdv-dev/Copulas](https://github.com/sdv-dev/Copulas)

**SDV——建模和采样关系数据库**

[https://github.com/sdv-dev/SDV](https://github.com/sdv-dev/SDV)

论文:[https://ieeexplore.ieee.org/document/7796926](https://ieeexplore.ieee.org/document/7796926)

# 合成数据的最新方法:GANs

**推荐阅读:**[https://towardsdatascience . com/review-of-gans-for-tabular-data-a30a 2199342](/review-of-gans-for-tabular-data-a30a2199342)

生成性对抗网络(GANs)由两个在迭代循环中相互对抗的模型组成。一个模型扮演“发生器”，另一个扮演“鉴别器”。与使用 GANs 生成深度赝品的方式类似，它们也可以用于生成与原始数据非常相似的合成表格数据。除了使用应用于神经网络的权重和过滤器来添加噪声之外，将噪声添加到数据集的相同基础也适用于 gan。用于图像的 GANs 和用于合成数据的 GANs 之间的另一个区别是，用于图像的 GANs 通常使用**卷积神经网络(CNN)**，因为生成/检测伪图像的环境要求模型迭代图像的层，而一层不影响下一层的生成。例如，如果鉴别器使用边缘检测滤波器，那么您不希望边缘滤波器的检测能力基于应用于图像的先前滤波器而改变[6]。本质上，你不希望过滤器有记忆。

用于合成表格数据的 GAN 通常将使用**递归神经网络(RNN)** 作为架构，因为您可以实现**长短期记忆(LSTM)。**LSTM 网络和 CNN 之间的一个关键区别是，LSTM 能够为网络增加内存，从而可以学习**的长期依赖性，**当您希望 GAN 能够**识别数据集内的相关属性**时，这一点尤为重要。

# GAN 工具箱

**梅德根**

[https://github.com/mp2893/medgan](https://github.com/mp2893/medgan)

论文:[https://arxiv.org/abs/1806.06397](https://arxiv.org/abs/1806.06397)

相关论文:[https://arxiv.org/abs/1703.06490](https://arxiv.org/abs/1703.06490)

**维根**

[https://github.com/akashgit/VEEGAN](https://github.com/akashgit/VEEGAN)

论文:[https://arxiv.org/abs/1705.07761](https://arxiv.org/abs/1705.07761)

**TableGAN**

[https://github.com/mahmoodm2/tableGAN](https://github.com/mahmoodm2/tableGAN)

论文:[http://www.vldb.org/pvldb/vol11/p1071-park.pdf](http://www.vldb.org/pvldb/vol11/p1071-park.pdf)

埃尔根

论文:[https://arxiv.org/abs/1709.01648](https://arxiv.org/abs/1709.01648)

**肉酱**

论文:[https://openreview.net/pdf?id=S1zk9iRqF7](https://openreview.net/pdf?id=S1zk9iRqF7)

相关论文:[https://arxiv.org/pdf/1906.09338.pdf](https://arxiv.org/pdf/1906.09338.pdf)

**DP-WGAN**

[https://github . com/nesl/NIST _ differential _ privacy _ synthetic _ data _ challenge](https://github.com/nesl/nist_differential_privacy_synthetic_data_challenge)

论文:https://github . com/nesl/NIST _ differential _ privacy _ synthetic _ data _ challenge/blob/master/reports/UCLANESL _ solution _ privacy _ proof . pdf

相关论文:[https://papers . nips . cc/Paper/7159-improved-training-of-wasser stein-gans . pdf](https://papers.nips.cc/paper/7159-improved-training-of-wasserstein-gans.pdf)

**DP-GAN(张炘炀、纪守灵、)**

[https://github.com/alps-lab/dpgan](https://github.com/alps-lab/dpgan)

论文:【https://arxiv.org/pdf/1801.01594.pdf 

【DP-GAN(李阳谢，林凯翔，，，)

[https://github.com/illidanlab/dpgan](https://github.com/illidanlab/dpgan)

论文:[https://arxiv.org/pdf/1802.06739.pdf](https://arxiv.org/pdf/1802.06739.pdf)

**TGAN**

[https://github.com/sdv-dev/TGAN](https://github.com/sdv-dev/TGAN)

论文:[https://arxiv.org/abs/1811.11264](https://arxiv.org/abs/1811.11264)

**CTGAN**

[https://github.com/sdv-dev/CTGAN](https://github.com/sdv-dev/CTGAN)

论文:[https://arxiv.org/abs/1907.00503](https://arxiv.org/abs/1907.00503)

# 合成数据的未来？

到目前为止，我们已经看到，机器学习在从数据中学习方面非常有效，并且能够复制合成数据，这些数据:

-保持与原始数据相同的统计分布

-了解不同列之间的相关性

-提供出色的数据匿名功能

-可缩放至任何尺寸

-可以无限次取样

与数据监管服务和使用传统方法泄露数据时的法律诉讼成本相比，生成合成数据极具成本效益。正在进行越来越多的研究来比较对原始数据集和合成数据集进行的数据分析的质量。此外，一些研究人员对合成数据如此有信心，以至于“科学家可以像使用控制数据一样使用合成数据”。

随着数据隐私越来越受到公众的重视，这是一个可能会以指数速度增长的研究领域。

## **51%的高级企业受访者表示，部门之间缺乏数据共享是数据战略中的一个关键问题。**【7】

## **消费者数据显示…**

86%的人希望对公司掌握的数据行使更大的控制权

**76%的人担心分享数据会让他们成为营销活动的目标**

**34%的人提供了伪造的个人信息，以避免泄露个人信息** [8]

## **数据屏蔽市场预计将以 14.8%的复合年增长率增长(CAGR)，到 2022 年价值将达到 7.67 亿美元** [9]

## **全球隐私管理软件市场预计将增长 33.1%(CAGR)。**【10】

请随时给我发消息，我会把它们添加到列表中。

[1]D . v .，Kumar N . k .，& R.Lakshmi Tulasi，D. (2018)。基于综合数据扰动的云增量数据集隐私保护技术。国际工程与技术杂志，7(3.34)，331–334。http://dx.doi.org/10.14419/ijet.v7i3.34.19219

[2] A. Narayanan 和 V. Shmatikov，“大型稀疏数据集的稳健去匿名化”， *2008 年 IEEE 安全和隐私研讨会(sp 2008)* ，加利福尼亚州奥克兰，2008 年，第 111–125 页。[https://ieeexplore.ieee.org/document/4531148](https://ieeexplore.ieee.org/document/4531148)

[3] Barth-Jones，Daniel,“威廉·韦尔德州长医疗信息的‘重新识别’:对过去和现在的健康数据识别风险和隐私保护的关键重新检查”( 2012 年 7 月)。[http://dx.doi.org/10.2139/ssrn.2076397](http://dx.doi.org/10.2139/ssrn.2076397)

[4] L. Sweeney，简单的人口统计数据通常可以唯一地识别人。卡内基梅隆大学，数据隐私工作文件 3。匹兹堡 2000。[https://dataprivacylab . org/projects/identificability/paper 1 . pdf](https://dataprivacylab.org/projects/identifiability/paper1.pdf)

[5]辛西娅·德沃克和亚伦·罗斯。2014.差分隐私的算法基础。找到了。趋势理论。计算机。Sci。9，3–4(2014 年 8 月)，211–407。https://doi.org/10.1561/0400000042

[6] Bellovin，Steven M .和 Dutta，Preetam K .和 Reitinger，Nathan，《隐私和合成数据集》(2018 年 8 月 20 日)。斯坦福技术法律评论，即将出版。[http://dx.doi.org/10.2139/ssrn.3255766](http://dx.doi.org/10.2139/ssrn.3255766)

[7]大卫·罗杰斯，唐·塞克斯顿。大数据时代的营销投资回报率:2012 年 BRITE/尼亚马转型营销研究。哥伦比亚商学院。2012.[https://www 8 . gsb . Columbia . edu/global brands/sites/global brands/files/images/2012-BRITE-尼亚马-营销-投资回报-研究. pdf](https://www8.gsb.columbia.edu/globalbrands/sites/globalbrands/files/images/2012-BRITE-NYAMA-Marketing-ROI-Study.pdf)

[8]马修·金特和大卫·罗杰斯。数据共享的未来是什么？消费者心态和品牌的力量。哥伦比亚商学院。2015 年 10 月[https://www8 . gsb . Columbia . edu/global brands/sites/global brands/files/images/The _ Future _ of _ Data _ Sharing _ Columbia-Aimia _ December _ 2015 . pdf](https://www8.gsb.columbia.edu/globalbrands/sites/globalbrands/files/images/The_Future_of_Data_Sharing_Columbia-Aimia_October_2015.pdf)

[9]市场与市场。按数据屏蔽类型(静态和动态)、组件(软件和服务)、部署类型、组织规模、业务职能(财务、营销和销售、运营和法律)、垂直市场和地区划分的数据屏蔽市场—到 2022 年的全球预测。2017 年 12 月[https://www . marketsandmarkets . com/Market-Reports/data-masking-Market-24977919 . html](https://www.marketsandmarkets.com/Market-Reports/data-masking-market-24977919.html)

[10]市场观察。CAGR 占 33.1%，到 2025 年隐私管理软件市场规模将达到 32.9 亿美元。2020 年 4 月 6 日。[https://www . market watch . com/press-release/at-331-cagr-privacy-management-software-market-size-set-to-register-32.9 亿美元-by-2025-2020-04-06](https://www.marketwatch.com/press-release/at-331-cagr-privacy-management-software-market-size-set-to-register-3290-million-usd-by-2025-2020-04-06)