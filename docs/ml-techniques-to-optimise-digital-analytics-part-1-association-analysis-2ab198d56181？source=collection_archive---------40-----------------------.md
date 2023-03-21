# 数字分析师必备的 ML 技术第 1 部分:关联分析

> 原文：<https://towardsdatascience.com/ml-techniques-to-optimise-digital-analytics-part-1-association-analysis-2ab198d56181?source=collection_archive---------40----------------------->

## 介绍可以帮助您优化数字分析价值链的机器学习技术

![](img/48a6013948ef1a67d4a9af57a650add0.png)

跨数字分析价值链的数据科学流程。作者图片

企业用户越来越倾向于自助进行基本的数字分析和报告。如果你是一名数字分析师，现在就开始扩展/转向数据科学将是一个明智的举动，并且能够提供比基本分析和仪表板更多的东西。

这是我学习、应用并最终分享这些数据科学技术知识的动机，在“数据科学”成为热门词汇之前，这些技术已经成为增量数据分析的一部分。

我的方法是解释这个概念和它所涵盖的用例。我将强调先决条件，并通过一个很好的例子来跟进。我的示例实现将用 r 编程。

在第 1 部分中，我将谈论**关联分析**更通俗地称为**购物篮分析**。这种分析与零售和其他设置相关，用户可以从购物车中添加并最终购买多种产品。目的是更好地了解什么样的产品或物品搭配得好，并利用这些信息进行更好的交叉销售、商品推销或有针对性的优惠。

# 关联:查找搭配在一起的项目

![](img/0f4c9431acc017c740006b4cadde7af4.png)

照片由[大卫·维克斯列尔](https://unsplash.com/@davidveksler)在 [Unsplash](https://unsplash.com/) 上拍摄

关联分析是一种统计技术，可以帮助您识别产品之间的顶级关联规则。

> 什么是关联规则？食品杂货交易的一个例子是，关联规则是{花生酱，果冻} => {面包}形式的建议。它说，根据交易，预计面包最有可能出现在包含花生酱和果冻的交易中。这是对零售商的建议，数据库中有足够的证据表明，购买花生酱和果冻的客户最有可能购买面包。

关联分析只是在数据中搜索其统计数据令人感兴趣的项目组合。它帮助我们建立规则，规定类似于*“如果 A 发生，那么 B 也可能发生。”*

但是，我们必须寻找的这些有趣的统计数据是什么，我们应该如何设置它们的值/阈值？

# **关联参数(有趣的统计！)**

首先，我们需要考虑复杂性控制:可能会有大量的同时发生，其中许多可能只是由于偶然，而不是一个可概括的模式。控制复杂性的一个简单方法是设置一个约束，即这些规则必须应用于某个最小百分比的数据——假设我们要求规则至少应用于所有事务的 0.01%。这被称为协会的**支持**。

我们在联想中也有“可能”的概念。如果顾客买了果冻，那么她可能会买面包。同样，我们可能希望为我们找到的关联要求某个最低程度的可能性。当 A 出现时 B 出现的概率；它是 p(B|A)，在关联挖掘中被称为规则的**置信度**(不要与统计置信度混淆)。因此，我们可能会说，我们要求置信度高于某个阈值，比如 5%(这样，在 5%或更多的时间里，A 的买家也会购买 B)。

仅仅将支持和信心作为一个参数可能会误导篮子中过于常见/受欢迎的项目。更有可能的是，受欢迎的项目是同一个篮子的一部分，只是因为它们受欢迎，而不是其他任何东西。我们需要某种程度的“惊奇”来进行关联分析。升力和杠杆是提供这种情况的两个参数。A 和 B 共现的**提升**是我们实际上看到两者在一起的概率，相比之下，如果它们彼此无关(独立)，我们会看到两者在一起的概率。就像我们看到的升力的其他用途一样，大于 1 的升力是看到 A 也“增加”看到 B 的可能性的因素。另一种方法是查看这些量之间的差异，而不是它们的比率。这个措施叫做**杠杆**。

所以，对于交易中的任何 A 和 B 项

Support=p(A⋂B)

置信度=p(A|B)或 p(A⋂B)/p(A)

Lift(A,B)=p(A⋂B)/p(A)p(B)

Leverage(A,B)=p(A⋂B)−p(A)p(B)

> **作为一名购物篮分析师，你的工作是寻找提升值大于 1 的规则，这些规则有高置信度值支持，通常也有高支持度。**

# 其他应用

因为我们在这一点上使用市场篮子作为类比，我们应该考虑拓宽我们对可能是一个项目的想法。为什么我们不能把任何我们可能感兴趣的东西放进我们的“篮子”里呢？例如，我们可以将用户的位置放入购物篮，然后我们可以看到购买行为和位置之间的关联。对于实际的购物篮数据，这些有时被称为虚拟商品，以区别于人们在商店放入购物篮的实际商品。关联分析发现并告诉我们具有统计意义的观察结果，如*“如果 A 发生了，那么 B 也可能发生。”*现在，我们可以替换 A 和 B 的任何东西，前提是它们发生在一起(可以被晒)。

根据上述逻辑，除了在线电子商务中的交叉销售机会，我们还有其他几个关联分析的应用。它可以帮助我们回答以下问题:

*   篮子中的产品组合有哪些季节性或品牌因素？关联分析强调的产品组合可能因不同行业而异。例如，它可以是旅游运营商的出发地和目的地城市。
*   对于在移动设备上购物的客户来说，产品组合是否有所不同？他们或多或少会购买哪些产品？

# Apriori 算法

关联规则挖掘有几种算法实现。其中最关键的是 Rakesh Agrawal 和 Ramakrishnan Srikanth 的 apriori 算法，他们在论文*中介绍了这种算法。*

> Apriori 算法是计算统计中一种常用的技术，它识别支持度大于预定义值(频率)的项集，并根据这些项集计算所有可能规则的可信度。

Apriori 算法在 *arules* 包中实现，可以在 r

# 处理

该算法将事务数据作为输入。交易是指顾客在零售店的一次购物。通常，交易数据可以包括购买的产品、购买的数量、价格、折扣(如果适用)和时间戳。一项交易可以包括多种产品。在某些情况下，它可以注册进行交易的用户的信息，其中客户允许零售商通过加入奖励计划来存储他的信息。对于挖掘，首先将交易数据转换为二进制购买关联矩阵，其中列对应于不同的项目，行对应于交易。矩阵条目表示特定交易中某个项目的存在(1)或不存在(0)。

# 示例实现

关联挖掘基于概率度量，因此从分析中生成可靠的见解通常需要大量的事务性数据。如果没有高度可扩展的存储和计算资源，大型数据集很难处理。通常，您将使用基于云的架构从您的数据湖中获取数据，但是内在的原则将保持不变，您的目标将是以事务格式获取数据以应用此规则。r 有连接到大多数系统的包，您甚至可以使用 SQL 进行数据交换。

让我们考虑下面的场景-

一家零售商正在策划一场大规模的营销活动来促进销售。他竞选的一个方面是交叉销售策略。交叉销售是向客户销售额外产品的做法。为了做到这一点，他想知道哪些物品/产品可以搭配在一起。有了这些信息，他现在可以设计他的交叉销售策略。他希望我们为他提供前 N 名产品协会的推荐，这样他就可以从中挑选并加入到他的活动中。

我们将实施一个项目，将关联规则挖掘应用于零售数据集，最终目标是推荐交叉销售商品。这个项目基于 Instacart 在 2017 年发布的一个数据集。他们发布了超过 300 万份匿名订单，供机器学习社区亲自尝试。我将使用子集训练数据集进行关联规则挖掘(假设我们的交叉销售用例)。我们将不得不进行一些初始的数据争论，以获得期望的事务格式。有关数据集的相关信息，请参考以下引文。

该项目的完整代码参见 [github](https://github.com/abhinav-sharma15/market-basket-analysis-instacart) 。

这是代码的分解-

加载所需的库

```
**library**(dplyr)
**library**(arules)
**library**(arulesViz)
```

# 数据争论

我们提供了两个文件。一个订单 csv，包含约 131，000 个订单，带有订单 ID 和产品 ID 观察结果，以及一个带有产品 ID 和产品名称映射的产品文件。首先，我们将创建一个包含订单 ID 和相关产品名称的事务数据集。

```
*# Orders csv*
file1.path = "./order_products__train.csv"
orders = read.csv(file1.path)
head(orders)
```

![](img/8cb2092d18ca96d63036b959df1524b2.png)

作者图片

```
*# Products csv*
file2.path = "./products.csv"
products = read.csv(file2.path)
head(products)
```

![](img/bedb4a959a3d5380b1710f1baee014f2.png)

作者图片

将两者结合起来，形成一个单一的交易数据集-

```
*# Combining both of them and forming a single transaction dataset -*
data = left_join(orders, products, by = "product_id") %>% select(order_id, product_name)
head(data,50)
```

![](img/8fd247cf8daba3dd8767e0f0d88e8d25.png)

作者图片

让我们快速浏览一下我们的数据。我们可以计算独特交易的数量和独特产品的数量:

```
*# We can count the number of unique transactions and the number of unique products*
data %>%
 group_by('order_id') %>%
 summarize(order.count = n_distinct(order_id))
```

![](img/d66d9070748a8ad840b3c41dc9cac683.png)

作者图片

```
data %>%
 group_by('product_name') %>%
 summarize(product.count = n_distinct(product_name))
```

![](img/89a9768300faf05dc304a1b9fc6068aa.png)

作者图片

我们有 131209 笔交易和 39123 个单个产品。没有关于交易中购买的产品数量的信息。我们使用 dplyr 库来执行这些聚合计算，这是一个用于在数据帧上执行高效数据辩论的库。

将其写回 csv

```
*# writing it back to csv*
write.table(data,file =  "./data.csv", row.names = FALSE, sep = ";", quote = FALSE)
```

# 关联规则挖掘

我们从读取存储在数据框中的事务开始，并创建一个名为 transactions 的 arules 数据结构。

```
*# create an arules data structure called transactions*
data.path = "./data.csv"
transactions.obj <- read.transactions(file = data.path, format = "single",
 sep = ";",
 header = TRUE,
 cols = c("order_id", "product_name"),
 rm.duplicates = FALSE,
 quote = "", skip = 0,
 encoding = "unknown")
```

查看用于创建 transactions 对象的函数 read.transactions 的参数。对于第一个参数 file，我们传递来自零售商的交易的文件。第二个参数 format 可以取两个值中的任意一个，single 或 basket，这取决于输入数据的组织方式。在我们的例子中，我们有一个包含两列的表格格式——一列代表我们事务的唯一标识符，另一列代表我们事务中存在的产品的唯一标识符。这种格式被 arules 命名为 single。有关所有参数的详细描述，请参考 arules 文档。

在检查新创建的事务对象 transaction.obj 时:

```
*# inspecting the newly created transactions object transaction.obj*
transactions.obj## transactions in sparse format with
##  131209 transactions (rows) and
##  39121 items (columns)
```

我们可以看到有 131209 笔交易，39121 个产品。它们与 dplyr 输出的先前计数值相匹配。

我们可以探索最频繁的项目，即出现在大多数事务中的项目，反之亦然——最不频繁的项目和出现在更少事务中的项目？

arules 包中的 itemFrequency 函数帮助了我们。此函数将交易对象作为输入，并生成各个产品的频率计数(包含此产品的交易数量):

```
data.frame(head(sort(itemFrequency(transactions.obj, type = "absolute"), decreasing = TRUE), 10)) *# Most frequent*
```

![](img/067de4ca322ea3156f13770b4357194f.png)

作者图片

```
data.frame(head(sort(itemFrequency(transactions.obj, type = "absolute"), decreasing = FALSE), 10)) *# Least frequent*
```

![](img/b43e9051404c458944c1c9d7c6c410f1.png)

作者图片

在前面的代码中，我们使用 itemFrequency 函数打印了数据库中最常用和最不常用的项目。itemFrequency 函数生成所有项目及其相应的出现频率和事务数量。我们将排序函数包装在 itemFrequency 上来对输出进行排序；排序顺序由递减参数决定。当设置为 TRUE 时，它根据项目的事务频率按降序对项目进行排序。最后，我们使用 head 函数包装排序函数，以获得前 10 个最频繁/最不频繁的项目。

香蕉产品是 18726 笔交易中出现频率最高的。如果我们将 type 参数设置为 relative 而不是 absolute，itemFrequency 方法也可以返回事务的百分比，而不是绝对值。

这个项目的目的是关注方法而不是结果。如果你要参考数据集来源，数据集包括来自许多不同零售商的订单，是 Instacart 生产数据的严重偏向子集，因此不是他们的产品、用户或购买行为的代表性样本。

检查项目频率分布的另一个方便的方法是将它们直观地绘制成直方图。arules 包提供了 itemFrequencyPlot 函数来可视化项目频率:

```
*# itemFrequencyPlot function to visualize the item frequency*
itemFrequencyPlot(transactions.obj,topN = 25)
```

![](img/4971ee24012f205816978cbb9810eb02.png)

作者图片

> 项目频率图应该给我们一些关于我们应该保持的支持阈值的概念。通常，我们应该在长尾开始的地方选择一个支持阈值。

现在我们已经成功地创建了事务对象，让我们继续将 apriori 算法应用于这个事务对象。

apriori 算法分两个阶段工作。发现频繁项集是关联规则挖掘算法的第一步。一组产品 id 称为一个项目集。该算法多次进入数据库；在第一遍中，它找出所有单个项目的交易频率。这些是阶为 1 的项目集。我们将在这里介绍第一个利息衡量标准，Support。

现在，在第一遍中，算法计算每个产品的交易频率。在这个阶段，我们有 1 阶项目集。我们将丢弃所有低于支持阈值的项目集。这里的假设是，交易频率高的项目比交易频率非常低的项目更有趣。支持度非常低的项目不会在未来产生有趣的规则。使用最频繁的项目，我们可以将项目集构造为具有两个乘积，并找到它们的事务频率，即两个项目都存在的事务的数量。我们再次丢弃低于给定支持阈值的所有两个乘积项集(阶数为 2 的项集)。我们继续这种方式，直到我们用尽他们。

```
*# Interest Measures*
 support <- 0.005
*# Frequent item sets*
 parameters = list(
 support = support,
 minlen = 2, *# Minimal number of items per item set*
 maxlen = 10, *# Maximal number of items per item set*
 target = "frequent itemsets")
 freq.items <- apriori(transactions.obj, parameter = parameters)
```

![](img/cfe6b1dec5114a3a057757f8304a1c12.png)

作者图片

arules 使用 apriori 方法来获取最频繁的项目。这个方法有两个参数，一个是 transaction.obj，另一个是一个命名列表。我们创建一个名为 parameters 的命名列表。在命名列表中，我们有一个支持阈值条目。我们将支持阈值设置为 0.005，即交易的 1%。我们通过查看之前绘制的直方图确定了这个值。通过将目标参数的值设置为频繁项集，我们指定我们期望该方法返回最终的频繁项集。Minlen 和 maxlen 设置了我们期望在项目集中有多少项目的下限和上限。通过将我们的 minlen 设置为 2，我们说我们不想要阶数为 1 的项集。在第 1 阶段解释 apriori 时，我们说过该算法可以对数据库进行多次遍历，并且每次后续遍历都会创建阶为 1 的项集，大于前一次遍历。我们还说过，当找不到更高阶的项目集时，apriori 就结束了。我们不希望我们的方法运行到最后，因此通过使用 maxlen，我们说如果我们到达 10 阶的项目集，我们就停止。apriori 函数返回项集类型的对象。

检查创建的对象(在这种情况下是 itemset)是一种很好的做法。仔细观察 itemset 对象，应该可以了解我们最终是如何使用它的属性来创建项目集的数据框架的:

```
str(freq.items)
```

![](img/1ea8bbbfa7db3b4c5e1ab46c5a9c9a28.png)

作者图片

通过调用函数 label 并传递 freq.items 对象，我们检索项目名称:

```
*# Let us examine our freq item sites*
 freq.items.df <- data.frame(item_set = labels(freq.items)
 , support = freq.items@quality)
head(freq.items.df,10)
```

![](img/021c81acd9db6f10a8d8d55d763fa308.png)

作者图片

让我们进入第二阶段，我们将从这些项目集中归纳出规则。是时候引入我们的第二个兴趣指标了，信心。让我们从算法第一阶段给出的列表中选取一个项目集，{Banana，Blueberries}。

这里我们有两条可能的规则:

香蕉= >蓝莓:香蕉出现在一个事务中强烈表明蓝莓也会出现在同一个事务中。蓝莓= >香蕉:蓝莓出现在一个事务中强烈暗示香蕉也会出现在同一个事务中。在我们的数据库中，这两条规则被发现为真的几率有多大？我们的下一个兴趣指标——信心指数，将帮助我们衡量这一点:

```
confidence <- 0.2 *# Interest Measure*

 parameters = list(
 support = support,
 confidence = confidence,
 minlen = 2, *# Minimal number of items per item set*
 maxlen = 10, *# Maximal number of items per item set*
 target = "rules"
 )
rules <- apriori(transactions.obj, parameter = parameters)
```

![](img/37c9511079b99e0d90f556e55f57c014.png)

作者图片

我们再次使用先验方法；但是，我们将参数命名列表中的目标参数设置为 rules。此外，我们还提供了一个置信度阈值。在使用返回的对象规则调用方法 apriori 之后，我们最终构建了我们的数据框架 rules.df，以便方便地浏览/查看我们的规则。让我们看看我们的输出数据帧 rules.df。对于给定的置信度阈值，我们可以看到算法抛出的规则集:

```
*# output data frame, rules.df*
rules.df <- data.frame(rules = labels(rules), rules@quality)
head(rules.df)
```

![](img/848bbe8716e47d20c1929ffa9f6457a5.png)

作者图片

Lift 也反映为数据框架中的另一个兴趣指标。

好了，我们已经成功地实现了我们的关联规则挖掘算法；我们深入了解了算法如何分两个阶段生成规则。我们已经研究了三个利益衡量标准:支持、信心和提升。最后，我们知道 lift 可以用来向我们的零售客户进行交叉销售推荐。

给定规则 A => B，我们解释了 lift 计算 A 和 B 一起出现的次数比预期的要多。还有其他方法来测试这种独立性，如卡方检验或费雪检验。arules 软件包提供了进行 Fisher 或卡方独立性检验的重要方法。根据我们希望执行的测试，参数方法可以采用 fisher 或 chisq 的值。

```
*# is.significant method to do a Fisher test of independence*
is.significant(rules, transactions.obj, method = "fisher")##  [1] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
## [15] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
## [29] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
## [43] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
```

我们已经编写了一个名为 find.rules 的函数，该函数返回给定事务和支持度/置信度阈值的前 N 个规则的列表。我们对前 10 条规则感兴趣。我们将使用杠杆值进行推荐。

```
*# top N rules*
find.rules <- **function**(transactions,topN = 10){

 other.im <- interestMeasure(rules, transactions = transactions)

 rules.df <- cbind(rules.df, other.im[,c('conviction','leverage')])

 *# Keep the best rule based on the interest measure*
 best.rules.df <- head(rules.df[order(-rules.df$leverage),],topN)

 **return**(best.rules.df)
 }cross.sell.rules <- find.rules(transactions.obj)
cross.sell.rules$rules <- as.character(cross.sell.rules$rules)
cross.sell.rules
```

![](img/43b1fd93ae1bad18d41e0eda443eedf1.png)

作者图片

前四个条目的提升值为 2 到 4，表明产品不是独立的。这些规则有大约 2%的支持度，系统对这些规则有 30%的置信度。但是等等，杠杆呢？这些项目有大约 1 个百分点的杠杆作用。无论是什么推动了这种同时出现，都会导致一起购买这两种商品的可能性增加一个百分点，超出我们的预期，仅仅因为它们是受欢迎的商品。这对于交叉销售决策来说足够了吗？也许是或不是...这是一个商业决定。

在这个例子中，我们建议零售商在交叉销售活动中使用这些顶级产品，因为考虑到 lift 值，如果顾客拿起一个{Organic Hass Avocado}，他很可能会拿起一袋{Organic 香蕉}。

我们还包括了另一个利益衡量标准——信念。

> 定罪:定罪是确定规则方向的一种措施。与 lift 不同，定罪对规则方向很敏感。定罪(A => B)不同于定罪(B => A)。信念，以其方向感，给了我们一个暗示，将有机哈斯鳄梨的顾客作为交叉销售的目标，会产生更多的有机香蕉袋的销售，而不是相反。

# 想象规则

plot.graph 函数用于可视化我们根据杠杆值列出的规则。它在内部使用一个名为 igraph 的包来创建规则的图形表示:

```
**library**(igraph)*# visualize the rules*
plot.graph <- **function**(cross.sell.rules){
 edges <- unlist(lapply(cross.sell.rules['rules'], strsplit, split='=>'))

 g <- graph(edges = edges)
 plot(g)
}
plot.graph(cross.sell.rules)
```

![](img/5186dae56e98442b29d213399f7db52d.png)

作者图片

# 加权交易

在纯粹的使用中，arules 包使用项目集中项目的频率来度量支持度。我们可以通过明确地为不同的交易提供权重来取代这一点，然后可以取代支持措施。这样做可以允许我们在关联规则中硬编码某些产品，即使它们可能不频繁(通过给包含它们的事务分配更多的权重)。

在 arules 包中，weclat 方法允许我们使用加权事务来基于这些权重生成频繁项集。我们通过 str(transactions.obj)事务对象中的 itemsetinfo 数据帧引入权重。

如果显式权重不可用，我们可以使用一种叫做超链接诱导主题搜索(HITS)的算法来为我们生成一个。HITS 算法的基本思想是分配权重，使得具有许多项目的事务被认为比具有单个项目的事务更重要。

arules 包提供了方法(HITS)。例如，我们可以使用点击量生成权重，然后使用 weclat 方法进行加权关联规则…

```
*# The arules package provides the method (HITS)*
weights.vector <- hits( transactions.obj, type = "relative")
weights.df <- data.frame(transactionID = labels(weights.vector), weight = weights.vector)head(weights.df)
```

![](img/db6ba171ab96d6a282f0763ffa616a41.png)

作者图片

# 引用

*   Instacart 在线杂货购物数据集 2017”，于 2020 年 3 月 9 日从[https://www.instacart.com/datasets/grocery-shopping-2017](https://www.instacart.com/datasets/grocery-shopping-2017)访问