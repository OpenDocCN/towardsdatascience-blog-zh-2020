# DBSCAN 方法实用指南

> 原文：<https://towardsdatascience.com/a-practical-guide-to-dbscan-method-d4ec5ab2bc99?source=collection_archive---------11----------------------->

## 流行的聚类方法 DBSCAN 的综合指南

![](img/4dd49e4feb2023e7ae37752937785768.png)

*检索 2020 年 4 月 19 日，来自*[](https://www.irishtimes.com/life-and-style/health-family/rejection-of-an-individual-by-a-group-is-the-worst-kind-of-bullying-1.3441864)*[*【gloria.com】*](https://gdoria.com/Exploring-the-Doc2Vec-and-Creating-a-Map-of-Video-Games.html)*

*当我在处理我的第一个数据科学任务时，我想使用 DBSCAN(带噪声的基于密度的应用程序空间聚类)进行聚类，我多次搜索以下问题的答案:*

*   *如何选择 DBSCAN 参数？*
*   *DBSCAN 如何工作？*
*   *DBSCAN 如何选择哪个点将属于哪个集群？*
*   *为什么用 DBSCAN 而不用 K-means？*

*当然，要找到所有这些问题以及更多问题的答案并不困难。然而，可能很难找到一篇总结所有这些和许多其他基本问题的文章。此外，你可能会感到惊讶，但一些重要的问题没有这样容易得到的答案。*

> *我的目标是编写一个指南，总结 DBSCAN 方法，回答所有这些问题以及更多问题。我发现 DBSCAN 算法不如 K-means 和层次聚类等其他流行的聚类方法直观，所以我将使用许多示例，并且我保证在本文结束时，您会理解这种方法。*

# *基于密度的聚类的动机*

*两种流行的聚类方法是:分区和层次方法。*

***划分方法**将数据集划分为 k(方法的主输入)个组(簇)。分区迭代过程将数据集中的每个点或对象(从现在起我将把它称为点)分配给它所属的组。将点分配到组中后，通过取组中所有点的平均值来计算组的平均值(质心)。最广为人知的划分方法是 [K-means](https://www.youtube.com/watch?v=4b5d3muPQmA) 。分区方法有一些明显的缺点:您应该预先知道要将数据库分成多少个组(K 值)。另一个重要的缺点是 K-means 在发现非凸/非球形的簇时表现不佳(图 1)。此外，K-means 对噪声数据很敏感。*

*![](img/ceadf573b424aabaf311709f8301cd35.png)*

*图 1-K-means 不能识别的聚类形状。检索自[数据挖掘简介](https://slideplayer.com/slide/4706420/)(谭，Steinbach，Kumar，2004)*

***分层方法**使用特殊的树(树状图)为数据创建一个分层的可视化表示。[凝聚层次聚类法](https://www.youtube.com/watch?v=7xHsRkOdVwo)始于其聚类中的每个点，在每一步中，它将相似或接近的聚类合并为一个聚类。在最后一步中，所有点都在同一个簇中。与 K-means 相反，您不需要决定应该有多少个聚类，但它也有一些严重的缺点:它不适合大数据集，计算复杂性高，您需要选择影响聚类结果的合并聚类的度量(链接)。此外，每个度量都有其缺点:对噪声敏感，难以处理簇密度的严重差异，以及对簇的形状和大小敏感。*

*正如我们所看到的，划分和分级方法的主要缺点是:处理噪声，并且在寻找非球形的簇时得到不好的结果。*

> *DBSCAN 聚类方法能够表示任意形状的聚类并处理噪声。*

*![](img/94e5b9dea026fcb1892041e42d591133.png)*

*图 2-任意形状的簇，例如“S”形和椭圆形簇。检索自[数据挖掘:概念与技术](http://myweb.sabanciuniv.edu/rdehkharghani/files/2016/02/The-Morgan-Kaufmann-Series-in-Data-Management-Systems-Jiawei-Han-Micheline-Kamber-Jian-Pei-Data-Mining.-Concepts-and-Techniques-3rd-Edition-Morgan-Kaufmann-2011.pdf) s(韩，Peri，Kamner，2011)。*

# *DBSCAN 直觉*

*让我们想象一个有很多居民和游客的大城市。想象一下，在不远的地方有一个小村庄。如果我们带一个外星人去两个地方，尽管它们离得很近，他也能很容易地分辨出它们是完全不同的地方。是的，景色、面积、建筑和其他许多方面都完全不同。但是有一个方面与我们的案例相关——地方的密度。城市很拥挤，有很多当地人和游客，而村庄很小，人少得多。*

> *根据密度来区分点组是 DBSCAN 的主要思想。*

# *DBSCAN 概念*

> *DBSCAN 将具有密集邻域的点组合成簇。*

*如果一个点附近有许多其他相邻点，则该点将被认为是拥挤的。DBSCAN 找到这些拥挤的点，并将它们和它们的邻居放在一个簇中。*

*DBSCAN 有两个主要参数-*

*   ***ε** (或 eps 或 epsilon)——定义每个邻域的大小和边界。ε(必须大于 0)是半径。点 x 的邻域称为 x 的ε-邻域，是围绕点 x 的半径为ε的圆/球。*

*一些书籍和文章将 x 的ε-邻域描述为:*

*![](img/8b03d3607ff2f7a52e93cf37ec11d79f.png)*

*从[数据挖掘和分析中检索的公式。基本概念和算法(扎基，梅拉，梅拉，2014 年](https://books.google.com/books?hl=en&lr=&id=Gh9GAwAAQBAJ&oi=fnd&pg=PR9&ots=JmG7PES-qp&sig=-pYKMgBCYkCp5x6yDqQutHnekEs)*

*在图 3 中，我们可以看到不同大小的ε如何定义 x 的ε-邻域。*

*![](img/d8a733c7f8aec4c900b3ad6ff79c5919.png)*

*图 3 —我们可以看到ε大小如何影响 x 的ε邻域的大小。*

*现在，您可能会问自己，为什么它是基于密度的集群？一个非常密集的邻域和一个稀疏的邻域会被认为是两个不同的邻域(图 4)，然后被分成两个不同的簇吗？*

*![](img/7ea64efc1597c8c209f3d17b1e7e5b66.png)*

*图 4 —密集邻域和稀疏邻域。*

*点 x 和它的邻居会在一个邻域内，而 y 和它的几个邻居会在另一个邻域内。然而，最终，点 x 和它的邻居可能会在一个聚类中，而点 y 和它的邻居会被认为是异常值或噪声。这是因为 y 的ε-邻域不够稠密。邻域包含的点越多，密度就越大。*

> *如何定义一个邻域是否足够密集？为此，DBSCAN 使用 MinPts 参数。*

***MinPts —** 密度阈值。如果一个邻域至少包含 MinPts 个点，它将被视为一个密集区域。或者，如果一个点的ε-邻域中至少有 MinPts 个点的值，则该点将被认为是稠密的。这些密集的点被称为**核心点**。*

*让我们再次检查图 4，如果 MinPts 参数是 3，点 x 将是核心点，因为它的ε-邻域的大小是 9，并且它大于 3。点 y 不会是核心点，因为它的ε-邻域包含两个点。*

** *点 x 的ε-邻域内的点数包含 x 本身。**

***一个边界点**有包含少于 MinPts 个点的ε-邻域(所以它不是一个核心点)，但它属于另一个核心点的ε-邻域。*

*如果一个点不是核心点，也不是边界点，那么它就是一个**噪声点**或者一个离群点。*

*在图 5 中我们可以看到点x 是一个核心点，因为在其ε-邻域中它有超过 11 个点。点 y 不是核心点，因为它的ε-邻域中的点少于 11 个，但是因为它属于点 x 的ε-邻域，并且点 x 是核心点，点 y 是边界点。我们很容易看出 z 点不是核心点。它属于 y 点的ε-邻域，而 y 点不是核心点，因此 z 点是一个噪声点。*

*![](img/5837528cc1ccc86feb6c30aebfdfa296.png)*

*图 5-在这个图中我们可以看到三种类型的点:x 是核心点，y 是边界点，z 是噪声点。*

> *既然我们看到核心点和边界点将在一个集群中，那么 DBSCAN 如何知道哪个点去哪个集群呢？为了回答这个问题，我们需要定义一些定义:*

***直接** **密度可达—** 点 y 是从点 x 直接密度可达的，如果:*

*   *点 y 属于点 x 的ε-邻域*
*   *点 x 是一个核心点。*

*在图 5 中，点 y 是从点 x 可直接密度到达的点。注意，点 x 不是从点 y 可直接密度到达的点，因为点 y 不是核心点。*

***密度可达—** 点 y 是从点 x 可达的密度，如果在点 x 和点 y 之间有一条点的路径，其中路径中的每一个点都可以从前一个点直接到达。这意味着路径上的所有点都是核心点，除了 y 点。我们可以在图 6 中看到一个例子。*

*据[维基百科](https://en.wikipedia.org/wiki/DBSCAN):*

> **如果有一条路径* P₁ *，* P₂ *…，* Pₙ *与* P₁ *= p，* Pₙ *= q，则从核心点 p 到点 q 是可达的，这条路径上的所有点都是核心点，除了点 q 可能例外**

*![](img/662a5b07c68df1b75b4f0f00a5ffeaa1.png)*

*图 6-点 y 是从点 x 可达到的密度。点 y 是边界点，点:x、z 和 p 是核心点。*

***密度连通-** 一个点 x 与一个点 y 是密度连通的，如果有一个点 o 使得 x 和 y 都是密度可达的。通过这种方式，DBSCAN 在密集区域中连接核心对象和它们的邻居。我们可以在图 7 中看到一个例子。*

*![](img/6b38b81f5f3c5b025ff3286d6b590e3b.png)*

*图 7-点 x 和点 y 是从点 o 密度可达的。因此，点 x 是密度连接到点 y 的。图从[数据库中的知识发现(Seidl，2016)](https://www.dbs.ifi.lmu.de/Lehre/KDD/SS16/skript/4_Clustering.pdf) 检索并由我编辑。*

***基于密度的聚类:**聚类 C 是一个非空的点组，给定:*

*   *点 x 在 C 中，y 是从 x 密度可达的，在这种情况下，y 将在 C 中*
*   *C 中的所有点都是密度相关的*

*在图 6 中，点 x、y、z、p 和 y 将在同一个群中。*

# *DBSCAN 算法*

**输入:**

*   **D —具有 n 个点的数据集**
*   **min pts——*邻域密度阈值*
*   *ε-邻域半径*

*方法:*

*1)我们将数据中的所有点标记为未访问。*

*2)我们随机选择一个未访问的点进行访问，并将其标记为已访问。姑且称之为‘p’吧。*

*3)我们检查 p 的ε-邻域是否至少有 MinPts 个点。*

*如果‘p’在其ε-邻域中没有足够的点(图 8)，我们将其标记为噪声，并继续执行步骤 4。*

***注意，在稍后的迭代中，在步骤 3.d 中，该点可能被包括在一个聚类中。**

*![](img/4702b2a49506668ba3e14292365c934b.png)*

*图 8-点“p”将被视为噪声，因为其ε-邻域包含的点少于 MinPts 点。*

*如果点‘p’在其ε-邻域内有足够多的点(图 9 ),我们继续步骤 3.a*

*![](img/144738b280fc57762f81de3b88ffba85.png)*

*图 9—‘p’**在它的ε-邻域中有超过个 MinPts***

***3.a)我们将创建一个新的集群 C，并将“p”添加到该集群中。***

***3.b)我们将给 p 的ε-邻域取一个新名字——N(图 10)。n 将包括这些点:{a，b，c，d，e，f，g，h，i}***

***![](img/a7c0c21152c137c035121f9e76607c67.png)***

***图 10-我们将赋予 p 的ε-邻域一个新的名字-N. *图 8-10 检索于 2020 年 4 月 19 日，来自* [*爱尔兰时报*](https://www.irishtimes.com/life-and-style/health-family/rejection-of-an-individual-by-a-group-is-the-worst-kind-of-bullying-1.3441864) *，*由我编辑***

***3.c)我们将对 n 中的每个点执行以下步骤。第一个点是“a”。如果“a”未被访问，我们将执行步骤 3.c.1 和 3.c.2。否则，我们将继续执行步骤 3.d。***

***3.c.1)我们将点‘a’**标记为已访问。*****

*****3.c.2)我们将检查点‘a’在其ε-邻域中是否至少有 MinPts。如果是这样，我们将把这些点加到 N 上。例如，如果‘a’的ε-邻域包括点{j，k，l}并且 MinPts 是 3，则新的 N 将包括点:{a，b，c，d，e，f，g，h，I，j，k，l }。但是，如果‘a’的ε-邻域仅包括点{j}，N 将保持不变。*****

*****3.d)如果点‘a’不属于任何聚类，我们将把它添加到聚类 C，现在聚类 C 将包括点‘a’和‘p’。*****

*****3.e)我们将移动到 N 中的下一个点(点‘b’)并返回到步骤 3.c。在检查了 N 中的所有点之后，我们将完成重复步骤 3.c-3.e。记住，N 可能随时更新(步骤 3.c.2) **。*******

*****4.我们已经完成了 p。我们将返回到步骤 2，访问下一个未访问的点，并重复这些步骤。当访问完数据集中的所有点时，我们就结束了。*****

*****算法的一个例子-*****

*****![](img/174c35e32aae3f17dd22c02f7f5198ec.png)*****

*****图 11。检索于 2020 年 4 月 19 日，来自[走向数据科学(Seif，2018)](/the-5-clustering-algorithms-data-scientists-need-to-know-a36d136ef68)*****

******我推荐查一下* [*这个*](https://dbs.ifi.uni-heidelberg.de/files/Team/eschubert/lectures/KDDClusterAnalysis17-screen.pdf#page=214) *DBSCAN 玩具例子(舒伯特，2018)******

# *****MinPts 和ε怎么选？*****

*****这是一个价值百万的问题。首先，我将定义一些术语:*****

*******函数 k-distance(p)** :点 p 到其 K 最近邻的距离(用户选择 K 值)。例如，如果用户选择 k 值为 4(图 12)，那么从点 p 到 4 最近邻的距离就是从 p 到它的 4 最近邻的距离。*****

*****![](img/b3fc9425a5d4042e480753975f037b31.png)*****

*****图 12- k 距离。检索自[数据库中的知识发现(Seidl，2016)](https://www.dbs.ifi.lmu.de/Lehre/KDD/SS16/skript/4_Clustering.pdf)*****

*******K 距离图**:所有物体的 K 距离，按降序排列。也称为排序 k 线图。*****

## *****参数估计*****

*****如果ε值很小，许多点可能会被视为异常值，因为它们不是核心点或边界点(ε邻域将非常小)。较大的ε值可能会导致大量的点位于同一个聚类中。*****

*****如前所述，DBSCAN 的主要优势之一是它可以检测噪声。根据舒伯特、桑德等人在 2017 年进行的一项[研究](https://dl.acm.org/doi/pdf/10.1145/3068335)，理想的噪音量通常在 1%到 30%之间。该研究的另一个启示是，如果一个聚类包含数据集的许多(20%-50%)点，则表明您应该为ε选择一个较小的值，或者尝试另一种聚类方法。*****

> *****一般情况下，ε应选择尽可能小的。*****

*****根据 DBSCAN 算法的[发起人](https://www.aaai.org/Papers/KDD/1996/KDD96-037.pdf?source=post_page---------------------------)(Ester，Kriegel，Sander 和 Xu，1996)，我们可以使用这种启发式算法来找到ε和 MinPts:*****

*****对于给定的 k，我们建立排序的 k-dist 图。阈值点是排序的 k-dist 图的第一个“谷”中的第一个点。阈值的 k-dist 值将是ε值。研究表明，对于 k > 4 的 k-dist 图与 4-dist 图没有显著不同，并且它们需要相当多的计算。因此，对于所有数据库(对于二维数据)，他们通过将参数 MinPts 设置为 4 来消除该参数。阈值点的 4-dist 值用作 DBSCAN 的ε值。*****

*****![](img/a579e6e218891ec17d1084818a6eb53a.png)*****

*****图 13 — K 分布图(对于 k=4) ( [Ester，Kriegel，Sander 和 Xu，1996)](https://www.aaai.org/Papers/KDD/1996/KDD96-037.pdf?source=post_page---------------------------)*****

*****如果不希望 MinPts 值为 4，可以决定 MinPts = k+1。选择 k 的一个启发式方法是将 k 设为 2∫维数-1 [(Sander，Ester 等人，1998)。](https://s3.amazonaws.com/academia.edu.documents/49949929/a_3A100974521941920161028-6495-1racv9j.pdf?response-content-disposition=inline%3B%20filename%3DDensity-Based_Clustering_in_Spatial_Data.pdf&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=ASIATUSBJ6BAG3OEV5TH%2F20200420%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20200420T102132Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLWVhc3QtMSJHMEUCIH0BDQ%2Bu4%2B4Gn2EYyNJb7mXMz9ICyY6nGWlN1AGAqwiAAiEAq7WYBwhWCSfMR%2Fk2y7EBKCuUGdZKVVCNxWEJafNNneMqtAMIEhAAGgwyNTAzMTg4MTEyMDAiDDVqaIEkFqUHl40KKSqRA0ibNBxUVWpXyxUnNcrD1QJ5vkKZjbPCPJDOhOdWbkcHXzSgkrlUPWYznXPIKCYtum2gQnN4dCfzihWiYaaQdfdRZIw6z1wm38GPDolFJU5gdZtX2y5bFSg9v3H1BSjD6kBf3UHTmbv3ebD3zQAbqkJ2jZPd2VfRUNbSObOhS3N2PU7m2Y5YYfEdDh7yyq9GUGeVFRaNQqFsQDyW5viARJJtpnzJtSiKd3uuZyZ7FJNUQnyHprLI8tV0BrzfQuXS50TL9KkgCmDyihWWiSPYJEP5EerVURa5FwdySl2bpHx7kH4GyPE0fFwCvTnkPxh1pFpmSIdoL0713IFyruacO3aBxg5N9H3rUO8gCDuFMsHu%2Fmp363H2bfVJbR1DZemYj7%2BiSy0DP8stII%2FYz1I8CZ2fdj4CNZvUQVj%2FkxCKGp72HccgptUDuqk9MYpdMDMxRZpuTXEBAWSNXcw5beDExvY3qmerbz6M1j6RYM4vxBtNkD95S9xCrBPX6CPI2ronZC68fbXoUYxmicjGR509yFo4MLzL9fQFOusBcr%2BPj%2FTtuyXCgwsSWKTTb9CutvWs7Qm1UtEBAhKNY4bM1fvgXNfedXe7nIUrHx3TcBTQc%2Bc1GhuH89OCBapsUUaUwZs8TamygNpwxObOIrFYKbjVryH5%2BemUJFUcUCu45IxD2%2BdW1A2yNykqDMff7lgg1hliHxIoDoecU36sAKPdjTE55O8gyD4cU3OP%2F6LBfT0y2kUzXttM%2BYmDsU9lIiHOQXYB52KsYF%2FJNiIprxM2g4RZwM92EPVBN9pNwNj5aOKKAZ4nsXHmncw60aSrGgowcQNckTbTuOg1PaGaUrTCJwmmiv%2F0dRvwCQ%3D%3D&X-Amz-SignedHeaders=host&X-Amz-Signature=b02ed8e6de1a562c2ff34a9971cbd06dc6c161c47af5c4e728c68c4b9fda8e83)*****

*****另一种选择 MinPts 值的启发式方法是*****

*****![](img/acef5a94d55e35ba6a5e11336349a4d0.png)*****

*****其中，Pᵢ是点 I 的ε-邻域中的点数，n 是数据集中的点数。对于ε的每个不同值，我们将得到相应的 MinPts 值([萨万特，2014)](http://ijiset.com/v1s4/IJISET_V1_I4_48.pdf) 。*****

*****在下一个例子中(图 15 ),我们:*****

1.  *****选择 K =3。*****
2.  *****计算每个点的第三近邻距离。例如，对于点 n，这个距离稍大于 5。*****
3.  *****对距离进行排序(图 15 中的柱状图是在我们对所有的点距离进行排序之后)。*****
4.  *****选择ε为第一个膝盖的值，所以ε ≈ 2.5。*****

*****![](img/432451de38c0d1007c9bdca235b39afd.png)*****

*****图 14-使用 k 线图选择参数。检索自埃里希·舒伯特 2018 年出版的[数据库中的知识发现](https://dbs.ifi.uni-heidelberg.de/files/Team/eschubert/lectures/KDDClusterAnalysis17-screen.pdf)。由我编辑*****

# *****计算的复杂性*****

*****如前所述，DBSCAN 检查数据中每个点的ε-邻域。检查它需要 O(n)。*****

> *****因此，在最坏的情况下，DBSCAN 的总复杂度是 O(n)。*****

*****在某些情况下，复杂度可以降低到 O(nlogn)。*****

# *****DBSCAN PROS*****

*   *****识别随机形状的簇*****
*   *****不需要事先知道数据中的聚类数(与 K-means 相反)*****
*   *****处理噪音*****

# *****DBSCAN*****

*   *****具有不同密度的数据集是有问题的*****
*   *****输入参数(ε和 MinPts)可能难以确定*****
*   *****计算复杂度——当维数很高时，计算复杂度为 O(n)*****

# *****摘要*****

> *****DBSCAN 是一种基于密度的聚类方法，可以发现非球形的聚类。*****

*****它的主要参数是ε和 Minpts。ε是邻域(一组相互靠近的点)的半径。如果邻域将至少包括 MinPts，则它将被视为密集区域，并将成为聚类的一部分。*****

*****有许多选择参数的方法。一种流行的启发式方法是选择一个 k 值并构建一个排序的 k-dist 图。阈值点是排序的 k-dist 图的第一个“谷”中的第一个点。阈值的 k-dist 值将是ε值**。**min pts 值将是 k+1 的值。您可以选择 4 作为 K 和 MinPts 值的默认值。*****

*****DBSCAN 的主要优点是不需要预先知道簇的数量，它可以识别随机形状的簇。主要缺点是计算复杂度高，需要为ε和 MinPts 选择好的值，以及处理不同密度的数据集。*****

# *****参考*****

1.  *****[艾斯特，m .，克里格尔，H. P .，桑德，j .，&徐，X. (1996 年 8 月)。一种基于密度的发现带噪声的大型空间数据库中聚类的算法。在 *Kdd* (第 96 卷，№34，第 226–231 页)。](https://www.aaai.org/Papers/KDD/1996/KDD96-037.pdf?source=post_page---------------------------)*****
2.  *****[韩，j .，Kamber，m .，T13 裴，J. (2011)。数据挖掘概念和技术第三版。摩根·考夫曼。](http://myweb.sabanciuniv.edu/rdehkharghani/files/2016/02/The-Morgan-Kaufmann-Series-in-Data-Management-Systems-Jiawei-Han-Micheline-Kamber-Jian-Pei-Data-Mining.-Concepts-and-Techniques-3rd-Edition-Morgan-Kaufmann-2011.pdf)*****
3.  *****[DBSCAN。(2020).于 2020 年 4 月 8 日从 https://en.wikipedia.org/wiki/DBSCAN 检索](https://en.wikipedia.org/wiki/DBSCAN)*****
4.  *****[普拉多，K. (2017)。DBSCAN 如何工作，为什么要使用它？。检索于 2020 年 3 月 11 日，来自 https://towards data science . com/how-DBS can-works-and-why-should-I-use-it-443 B4 a191c 80](/how-dbscan-works-and-why-should-i-use-it-443b4a191c80)*****
5.  *****萨万特，K. (2014 年)。确定 DBSCAN 参数的自适应方法。*国际创新科学杂志，工程&技术*， *1* (4)，329–334 页。*****
6.  *****[桑德，j .，埃斯特，m .，克里格尔，H. P .，T23 徐，X. (1998)。空间数据库中基于密度的聚类:gdbscan 算法及其应用。*数据挖掘与知识发现*， *2* (2)，169–194。](https://s3.amazonaws.com/academia.edu.documents/49949929/a_3A100974521941920161028-6495-1racv9j.pdf?response-content-disposition=inline%3B%20filename%3DDensity-Based_Clustering_in_Spatial_Data.pdf&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=ASIATUSBJ6BAG3OEV5TH%2F20200420%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20200420T102132Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLWVhc3QtMSJHMEUCIH0BDQ%2Bu4%2B4Gn2EYyNJb7mXMz9ICyY6nGWlN1AGAqwiAAiEAq7WYBwhWCSfMR%2Fk2y7EBKCuUGdZKVVCNxWEJafNNneMqtAMIEhAAGgwyNTAzMTg4MTEyMDAiDDVqaIEkFqUHl40KKSqRA0ibNBxUVWpXyxUnNcrD1QJ5vkKZjbPCPJDOhOdWbkcHXzSgkrlUPWYznXPIKCYtum2gQnN4dCfzihWiYaaQdfdRZIw6z1wm38GPDolFJU5gdZtX2y5bFSg9v3H1BSjD6kBf3UHTmbv3ebD3zQAbqkJ2jZPd2VfRUNbSObOhS3N2PU7m2Y5YYfEdDh7yyq9GUGeVFRaNQqFsQDyW5viARJJtpnzJtSiKd3uuZyZ7FJNUQnyHprLI8tV0BrzfQuXS50TL9KkgCmDyihWWiSPYJEP5EerVURa5FwdySl2bpHx7kH4GyPE0fFwCvTnkPxh1pFpmSIdoL0713IFyruacO3aBxg5N9H3rUO8gCDuFMsHu%2Fmp363H2bfVJbR1DZemYj7%2BiSy0DP8stII%2FYz1I8CZ2fdj4CNZvUQVj%2FkxCKGp72HccgptUDuqk9MYpdMDMxRZpuTXEBAWSNXcw5beDExvY3qmerbz6M1j6RYM4vxBtNkD95S9xCrBPX6CPI2ronZC68fbXoUYxmicjGR509yFo4MLzL9fQFOusBcr%2BPj%2FTtuyXCgwsSWKTTb9CutvWs7Qm1UtEBAhKNY4bM1fvgXNfedXe7nIUrHx3TcBTQc%2Bc1GhuH89OCBapsUUaUwZs8TamygNpwxObOIrFYKbjVryH5%2BemUJFUcUCu45IxD2%2BdW1A2yNykqDMff7lgg1hliHxIoDoecU36sAKPdjTE55O8gyD4cU3OP%2F6LBfT0y2kUzXttM%2BYmDsU9lIiHOQXYB52KsYF%2FJNiIprxM2g4RZwM92EPVBN9pNwNj5aOKKAZ4nsXHmncw60aSrGgowcQNckTbTuOg1PaGaUrTCJwmmiv%2F0dRvwCQ%3D%3D&X-Amz-SignedHeaders=host&X-Amz-Signature=b02ed8e6de1a562c2ff34a9971cbd06dc6c161c47af5c4e728c68c4b9fda8e83)*****
7.  *****[塞德尔，T. (2016)。*数据库中的知识发现——第 4 章:聚类*。介绍，路德维希-马克西米利安-慕尼黑大学。](https://www.dbs.ifi.lmu.de/Lehre/KDD/SS16/skript/4_Clustering.pdf)*****
8.  *****[舒伯特，e，桑德，j，埃斯特，m，克里格尔，h . p .&徐，X. (2017)。DBSCAN 重温，重温:为什么和如何你应该(仍然)使用 DBSCAN。*美国计算机学会数据库系统汇刊(TODS)* ， *42* (3)，1–21。](https://dl.acm.org/doi/pdf/10.1145/3068335)*****
9.  *****拉夫桑贾尼、扎拉·阿斯加里·瓦尔扎内·纳西贝·埃马米·楚坎托。层次聚类算法综述。数学和计算机科学杂志，229–240 页。*****
10.  *****[m . j .扎基，&w .梅拉(2014)。*数据挖掘和分析:基本概念和算法*。剑桥大学出版社。](https://books.google.co.il/books?hl=en&lr=&id=Gh9GAwAAQBAJ&oi=fnd&pg=PR9&ots=JmG7PES-qp&sig=-pYKMgBCYkCp5x6yDqQutHnekEs&redir_esc=y#v=onepage&q&f=false)*****