# 商业智能与新兴技术的数据工程相结合

> 原文：<https://towardsdatascience.com/business-intelligence-meets-data-engineering-with-emerging-technologies-8810c3eed8b1?source=collection_archive---------14----------------------->

## 如何用新兴技术和十二种数据工程方法让 BI 变得更好？

![](img/bca50d63686e490cb0d4d7f71843f2de.png)

如今，我们对不断增长的工具和框架、复杂的云架构以及快速变化的数据堆栈有了更多的要求。我听到有人说:“商业智能(BI)需要太长时间来整合新数据”，或者“理解数字如何匹配非常困难，需要大量的分析”。本文的目标是用数据工程领域的技术使商业智能变得更容易、更快、更容易获得。

在之前的一篇文章中，我指出了什么是数据工程，以及为什么它是商业智能和数据仓库的继承者。什么时候需要一个数据工程师，他在做什么。数据工程师工具语言 python 和 ETL 中的变化。在这篇文章中，我将重点关注 BI 中的挑战，以及如何用数据工程来解决它们。

# 商业智能的目标

但首先，让我们先讨论一下“BI 应该为我们做些什么？”

用我的话来说，BI 应该生成一个简单的业务概览，提高效率，并自动化整个组织的重复性任务。更详细地说:

*   **卷起能力**——(数据)最重要的[关键绩效指标](https://en.wikipedia.org/wiki/Performance_indicator)(聚合)的可视化——就像飞机上的驾驶舱，让你一眼就能看到最重要的信息。
*   **向下钻取的可能性** —从上面的高层次概述向下钻取非常详细的信息，找出某些事情没有按计划执行的原因。**切片切割或** **从不同角度旋转**您的数据。
*   **单一事实来源** —取代了多个电子表格或其他不同编号的工具，该流程是自动化的，并且对所有人都是统一的。员工可以谈论业务问题，而不是每个人都有的各种数字。报告、预算和预测会自动更新，并保持一致、准确和及时。
*   **支持用户**:借助所谓的[自助 BI](https://www.sisense.com/glossary/self-service-bi/) ，每个用户都可以分析他们的数据，而不仅仅是 BI 或 IT 人员。

# 商业智能问题

另一方面，BI 在速度和透明度方面存在一些实质性的问题。我试图总结我在作为一名从事 Oracle 和 SQL Server 工作的 BI 工程师和专家的职业生涯中了解到或听到的问题:

*   **整合额外资源需要太长时间，**和 BI 工程师**超负荷工作**。
    —这就是为什么每个部门都使用互不关联的 Excel 电子表格创建数据仓库和分析的原因之一，这些电子表格总是过时，需要大量的操作和协调。
    —**缺乏速度**是一个显著的缺点，可以通过 [**数据仓库自动化**](https://www.sspaeti.com/blog/why-automate-what-does-dwa-for-us/) 来缓解。我在一篇关于数据仓库自动化的 [Quora-post](https://qr.ae/pNrL5t) 或我的[系列文章](https://www.sspaeti.com/blog/data-warehouse-automation-dwa/)中总结了自动化可以减轻的更多细节。
*   **透明性**对于 BI 工程师以外的其他用户来说是个问题。只有他们能够看到转换逻辑的内部，这些逻辑大多隐藏在专有的 ETL 工具中。
*   商务人员或经理**依赖商务智能工程师**。没有简单的方法来访问 ETL 或获取任何实时数据。
*   商务智能部门**使得成为**变得更加复杂。给人的印象总是它不应该如此复杂。对我们来说，所有的转换都很清楚，业务逻辑清理、星型架构转换、性能调整、使用大数据，等等。但是对于非双语者来说，这很难理解。
*   **处理困难(**[**semi**](https://en.wikipedia.org/wiki/Semi-structured_data)**-)**[**非结构化**](https://en.wikipedia.org/wiki/Unstructured_data) **数据格式**如 JSON、图像、音频、视频、电子邮件、文档等。
    —这归结为 **ETL，加载前转换，**传统上是一个数据仓库，而 **ELT(首先将数据加载到存储中，并且仅在决定如何处理它之后)—也称为写时模式与读时模式**。ELT 为您提供了速度上的显著优势，这是更现代的数据湖或 NoSQL 数据库所能做到的。如果你想知道更多关于数据仓库和数据湖(ETL 和 ELT)的区别，我推荐[我之前的帖子](https://www.sspaeti.com/blog/data-warehouse-vs-data-lake-etl-vs-elt/)。
    —另一点是，切片和切块是在聚合数据上完成的，上面提到的非结构化数据实际上做得不好。
    —最重要的是，这些非结构化数据延长了夜间 ETL 作业的时间，因为它们需要更长的处理时间。
*   **一般数据每天只提供一次**(传统上)。我们在私人生活中实时获取一切，每个人对现代 BI 系统都有同样的要求。

这个列表无论如何都是不完整的。此外，是否可以通过特殊的解决方案(例如，云解决方案使用 [SnowflakeDB](https://www.snowflake.com/) 和 [Variant](https://docs.snowflake.com/en/sql-reference/data-types-semistructured.html#variant) 数据类型用于半结构化数据)或不同的方法(使用 [data vault](http://danlinstedt.com/solutions-2/data-vault-basics/) 用于快速集成)来缓解任何问题。然而，成见根深蒂固，据我所知，依然存在。

# 数据工程方法

因为我自己也遇到了这些瓶颈，最近更频繁地，我问自己:“我们如何:

*   让 BI *更容易*？
*   对所有人透明？
*   毫不费力地更改或添加新数据或转换，但仍然有一些治理和测试？
*   在即席查询中快速探索和分割您的数据？
*   有更频繁的数据加载？
*   简化所有精通数据的人的转换过程，而不仅仅是相关的 BI 工程师？
*   顺利扩展额外的机器学习能力？

我知道现在发生了很多事情，尤其是围绕开源工具和框架、数据操作和使用容器编排系统的部署等等。

然而，我试图收集一些方法，帮助我使这个复杂的结构更加开放，并减轻整体体验。有些在短期内会变得更加复杂，但随着时间的推移，会变得更加精简和简单。你可以分别应用它们，但是你用得越多，整体的流动就越明显。

> "使用数据湖."

让我们从第一个开始:使用数据湖或[湖屋](https://databricks.com/blog/2020/01/30/what-is-a-data-lakehouse.html)代替数据仓库(DWH)。**这为您提供了速度、拥有(半)非结构化数据的能力**，并在转换期间定义模式(ELT 而不是 ETL)。同时，它将为你提供**高透明度**，因为数据被存储到一个开放的数据湖中，供每个人访问或分析。添加新栏目或与同事分享数据很容易。您可以使用广泛的分布式计算，如 [Spark SQL](https://databricks.com/glossary/what-is-spark-sql) 或 [Presto](https://prestodb.io/) 通过即席查询即时探索、连接和转换您的数据。

数据可用性很快，不需要每天晚上进行批处理，因为数据首先会被存入湖中。它可能还不干净，但是您可以直接开始探索并添加转换来实现。一种常见的方法是在数据仓库前面添加一个数据湖。通过这种方式，您可以同时受益于两者，即湖中的即时数据，也可以在数据仓库的末端进行结构化和清理。

一个实际的概述很好地说明了所涉及的组件，这就是从数据湖之上的数据仓库到湖边小屋的演变。要注意的最重要的部分是 ETL，它也是整个数据工程的核心组件。您可以看到，它从隐藏在[数据集市](https://en.wikipedia.org/wiki/Data_mart)后面转移到数据湖体系结构，再转移到**一个统一数据块中的主要转换层。**

![](img/f913feb60eec3de81bebb51c0c879e21.png)

摘自[“什么是湖畔小屋？”](https://databricks.com/blog/2020/01/30/what-is-a-data-lakehouse.html) *由* [*数据块*](https://databricks.com/)

你可以在我的 [LinkedIn 帖子](https://www.linkedin.com/posts/sspaeti-com_four-different-data-architectures-activity-6660097384279744512--5e1)中找到更多从不同角度体现组件的数据架构，或者，有关数据仓库及其与数据湖的比较的更多信息，请查看我的[早期博客帖子](https://www.sspaeti.com/blog/data-warehouse-vs-data-lake-etl-vs-elt/)。

> "使用事务处理."

为了支持数据湖中关系数据库的各种特性，您需要事务处理。幸好 [**三角洲湖泊**](https://delta.io/) 来救你了。 **Delta 拥有许多令人敬畏的特性，如 ACID 事务、**[](https://databricks.com/blog/2019/02/04/introducing-delta-time-travel-for-large-scale-data-lakes.html)****、保留一个** [**事务日志**](https://databricks.com/blog/2019/08/21/diving-into-delta-lake-unpacking-the-transaction-log.html) **、SQL API 编写原生 SQL** **作为插入、更新、删除甚至合并语句、open-format(**[**【Apache Parquet**](https://parquet.apache.org/)**)、统一的批处理和流源与汇(no 关于 [delta.io](https://delta.io/) 的完整列表和更多信息。此外，查看苹果公司关于[大规模威胁检测](https://databricks.com/session/keynote-from-apple)的优秀客户示例。****

**![](img/9bc04e687072df6944bf3162d294d724.png)**

**【2020 年 6 月三角洲湖泊的主要特征( [*三角洲 io*](https://delta.io/) *)***

**在数据湖中，我们通常有分布式文件，很难对它们进行结构化和整理。尤其是当您想要插入、更新或删除行时。Delta 有**不同的 API，除了 scala 和 python，它还给了你 SQL API** (来自 Spark 3.0 on 或 in [databricks](https://databricks.com/) )，在这里你可以很容易地在你的分布式文件上写一个 *update* 甚至 *merge* 语句。由于它由 Spark 提供动力，因此您可以完全规模化地完成这项工作。**

**幕后是 raw[**Apache Parquet**](https://parquet.apache.org/)**，优化的柱状存储，高度压缩。**这让你能够有效地直接从数据湖中查询数据。那里不需要初始转换。**

**随着**缓慢变化的维度(SCD)在某些情况下仍然是一件事，达美航空有时间旅行**来解决这个问题。这类似于我们早期所做的快照。但这一次，与固态硬盘相比，Delta 使用了相当便宜的 blob 存储，而不是每天或每月的快照，Delta 将每一批更改存储为一个单独的版本。这给了你及时回到旧版本的能力，以防你错误地删除了一些东西，或者如果你需要分析指向特定的版本。只要设置了保留时间，就会出现这种情况。**

**这些更改和快照存储在由 Delta 维护的**事务日志中。除了时间旅行，这是一种**变更数据捕获(CDC ),根据表**跟踪所有变更。您可以看到哪些文件受到了影响，什么操作，谁做的等等，以及每个事务的情况。这是以 JSON 格式单独存储的。事务数据也可能变得很大，Delta 每十次提交就创建一个检查点文件，其中包含表在某个时间点的完整状态，采用 Parquet 格式，对于 Spark 来说读取起来既快又简单。****

> **"少用代理键，而是回到每个人都理解的业务键."**

**在数据仓库中，通常的做法是使用[代理键](https://en.wikipedia.org/wiki/Surrogate_key)通过单个人工键来寻址一行。当这对于支持[维度模型](https://data-warehouses.net/glossary/dimensionalmodel.html)并确保一个行键有一个惟一的 ID 是有意义的时候，在数据湖中你不需要这样做。我认为应该尽可能地减少**它让数据读取在理解和交流方面变得更加复杂**。代理键是随机 id，对任何人都没有意义(这也是为什么大多数时候在后台使用或看到的原因)。**

**在契约式分布式系统中，随着数据变得越来越大，在整个数据集上拥有唯一的键不再实用。尤其是并行，很难并行化的同时还具有唯一性。最好使用[散列键](https://danlinstedt.com/allposts/datavaultcat/dv2-keys-pros-cons/)来缓解这个问题。缺点是，它们更长，可读性更差。那么，为什么不回到您的源系统已经为您生成的[业务密钥](https://en.wikipedia.org/wiki/Natural_key) (BK，也称为自然密钥)呢？**业务键已经被每个人所理解，在某种程度上是唯一的(作为源系统中的序列创建)，在数据集的生命周期中保持相同的值，因此可以一直追溯到**进行跟踪和比较。**

**当然，代理键有很大的理由。它们在某些方面给了我们很大的优势，但代价是什么呢？我们需要做大量的工作来确保事实的正确粒度，并使用唯一的 ID 作为一行的代表来合并和清理维度。但同样，在数据湖中，我们的规则更少，也更不严格。例如，我们可以首先创建副本，然后只清理和删除它们。**

**这个话题也与“Kimball 在现代数据仓库中是否仍然相关？”这个问题密切相关和“[规格化](https://en.wikipedia.org/wiki/Database_normalization)及其范式？”。至于后者，就存储而言，它不再像 blobs 和类似存储那样重要，但对于分析目的仍然有效。要了解更多细节和见解，我鼓励你去看看西蒙·怀特利关于这个话题的[博客帖子](https://www.advancinganalytics.co.uk/blog/2019/6/17/is-kimball-still-relevant-in-the-modern-data-warehouse)。**

> **“使用笔记本来打开数据仓库。”**

**使用**笔记本** ( [jupyter](https://jupyter.org/) ， [zeppelin](https://zeppelin.apache.org/) ， [databricks](https://docs.databricks.com/notebooks/index.html) ) **每个人都可以访问数据，可以探索和编写所有高级统计和数据科学库的分析**。设置很简单，不需要安装本地环境或 IDE，它可以与你选择的浏览器一起工作。你可以通过快速分享链接来传播你的观想。也可以在同一个笔记本上一起工作。通过整合降价，你可以自然地向人们解释数字背后的整个故事和思维过程。**

****笔记本的** **缺点是你的代码会被复制并分散在各处**。有点像每个人都有自己的擅长，每个人都会开始自己的笔记本(但当然不同的数据，因为数据是集中存储的)。我建议您围绕笔记本电脑制定严格而明确的规则或管理，并开始将稳定的笔记本电脑集成到您的公共数据管道中。**

**为了快速平稳地过渡，您应该看看 [papermill](https://github.com/nteract/papermill) ，它允许您参数化、执行和分析笔记本。甚至**更进一步，使用** [**dagster**](https://dagster.io/) **，或者使用笔记本作为你的管道** (dagster 与 papermill 集成)**的一个步骤，或者用专用的[实体](https://dagster.readthedocs.io/en/latest/sections/learn/guides/solid/solid.html)将其完全纳入你的管道**。这样，您可以避免代码重复和重用实体。**

> **"使用 python(和 SQL，如果可能的话)."**

****Python 就是这几天数据工程师**的 [**工具语言**](https://www.sspaeti.com/blog/data-engineering-the-future-of-data-warehousing/#The_tool_language_Python) **。当然还有 [golang](https://golang.org/) 、 [javascript](https://en.wikipedia.org/wiki/JavaScript) 、 [scala](https://www.scala-lang.org/) 等流行语言，但是在简单性和多用途能力(数据科学、web 等)方面，python 还是很难被打败的。SQL 再次获得了更多的关注，这是非常受欢迎的。尽管如此，SQL 永远也做不到面向对象编程语言正在做的事情，这就是为什么我认为 python 会在构建数据管道方面停留很多年。****

**因此**使用用 python 编写的框架或工具来编码你的管道**。笔记本已经支持 SQL 和 python，但它们不支持组织或编排。更好的方法是使用像广受欢迎的和众所周知的[阿帕奇气流](https://airflow.apache.org/)这样的工具。街区里有新的孩子(一如既往)，他们很有前途，其中一个就是上面提到的达格斯特。如果你感兴趣的话，在我的 Quora 小回答[中有更多关于气流的常见替代品](https://qr.ae/pNrIPi)。**

> **“使用开源工具。”**

**每个人都可以免费获得开源软件，不涉及任何许可，并且很容易立即开始使用。在购买一个闭源项目或产品之前，不存在需要整个团队讨论的供应商锁定。**

**另一方面，一个开源工具很少单独出现，要适应和评估开源动物园中你最喜欢的工具和应用程序可能会很累。此外，你需要跟进那些可能出现的对你有价值的永无止境的版本或错误修复。这里的想法是，你可能通过其他公司或一些好的咨询公司的一些建议，知道使用哪些工具。并且**对沿途的尝试保持开放，因为你没有被锁定，你可以随时替换或适应稍后的**。学习过程与首先选择正确的工具一样有价值。**

**![](img/b22c3b7e80a50d22fa4271219fcb4b1b.png)**

***大数据与人工智能 2018 年展望作者* [*马特图尔克*](https://mattturck.com/bigdata2018/) *摘自* [伟大的力量，伟大的责任](https://mattturck.com/bigdata2018/)**

****你也可以选择一种中间的方法，像**[**Databricks**](https://databricks.com/)**用于 Spark 和机器学习功能。您可以在瞬间启动并运行，而自己却被锁在半路上。借助 Spark 和笔记本电脑这两种开源技术，您可以随时切换到您的集群。你失去了高级功能和速度，你也可以购买并在以后添加。****

******我在之前的** [**帖子**](https://www.sspaeti.com/blog/open-source-data-warehousing-druid-airflow-superset/) **中收集了一个实际例子，其中我使用开源技术构建了一个完整的数据仓库**。那次是在 2018 年，当时没有 Delta Lake 和上面提到的其他工具，因为当时没有发布。我用了[阿帕奇气流](https://airflow.apache.org/)、[阿帕奇德鲁伊](https://druid.apache.org/)和[阿帕奇超集](https://superset.incubator.apache.org/)。****

> ****"加载增量和等幂."****

****与传统数据仓库的夜间加载相比，我们需要增量加载**。这使得你的数据更加模块化和易于管理**，尤其是当你有一个[星型模式](https://en.wikipedia.org/wiki/Star_schema)的时候。事实表只能追加，维度只需要扫描最新的事务，而不是整个事实表。****

****使用增量方法，您**从批处理切换到事件驱动**。您的更新和插入是独立的，您可以获得自治的批处理。如果您成功切换，您将获得一个近乎实时的分析解决方案，您可以对这些批次进行扩展和并行处理。****

****另一种方法是加强[幂等性](https://en.wikipedia.org/wiki/Idempotence)，这对于管道的可操作性至关重要，主要有两方面的帮助。它保证您可以重新运行您的管道，并且每次都会产生相同的结果。另一方面，数据科学家和分析人员依赖时间点快照并执行历史分析。这意味着您的数据不应该随着时间的推移而变化。否则，随着时间的推移，我们会得到不同的结果。这就是为什么管道应该被构建为在以相同的(业务)逻辑和时间间隔运行时再现相同的输出。这被称为幂等性，用于[函数式编程](https://medium.com/@maximebeauchemin/functional-data-engineering-a-modern-paradigm-for-batch-data-processing-2327ec32c42a)，它是幂等性的角色模型。****

******事件驱动和增量加载的一个很好的副作用是，你可以消除**[**λ架构**](https://en.wikipedia.org/wiki/Lambda_architecture) **。** **一个用于批处理和流**的单一数据流，其与**三角洲湖**完全一致。具有集成选项[微批处理](https://databricks.com/blog/2018/03/20/low-latency-continuous-processing-mode-in-structured-streaming-in-apache-spark-2-3-0.html)的 [Spark 结构化流](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html)非常适合这一目的。这样，您可以从两个方面受益，因为您可以进行流式处理，并且可以将延迟设置为 0，但也可以降低处理速度。例如，当您每小时有一个批处理来减少查找维度或特定聚合的开销时，您需要为每个流执行其他操作(这也可能需要时间)。****

> ****"不要以传统的 DDL 方式改变结构."****

****每个处理数据的人都知道更改数据类型或重命名列是多么痛苦。有一个完整的依赖链，有很多没有真正价值或影响的工作。使用上面的增量和等幂模式，您可以免费得到它。这意味着您的更改不是重命名或更改某个字段的业务逻辑，**最好是添加新字段，并在稍后** **更改您的管道和分析，而不破坏任何东西**。****

****通过使用 Delta Lake，您还可以获得[模式进化](https://databricks.com/blog/2019/09/24/diving-into-delta-lake-schema-enforcement-evolution.html)和[乐观并发](https://docs.databricks.com/delta/concurrency-control.html)，这两者都可以帮助您解决这个问题。因为第一个会自动更新模式更改，而不会破坏任何东西。第二个将确保两个或更多的用户可以更新同一个数据集而不会失败，只要不同的列和相应的数据不会被同时更改。****

> ****"使用容器编排系统."****

****[Kubernetes](https://stackoverflow.blog/2020/05/29/why-kubernetes-getting-so-popular/) 已经成为云原生应用的事实上的标准，用于(自动)[横向扩展](https://stackoverflow.com/a/11715598/5246670)(水平而非垂直)和快速部署开源动物园，独立于云提供商。这里也没有锁定。你可以使用[开放式换档](https://www.openshift.com/)或 [OKD](https://www.okd.io/) 。在最新版本中，他们**增加了**[**operator hub**](https://operatorhub.io/)**从今天开始，你只需点击几下**就可以安装 133 个项目。安装起来复杂的数据库和机器学习应用程序变得非常简单。****

****Kubernetes 的更多原因是**从基础设施即代码向基础设施即数据的转变，具体来说就是** [**YAML**](https://en.wikipedia.org/wiki/YAML) 。Kubernetes 中的所有资源，包括 pod、配置、部署、卷等。，可以简单地用 YAML 文件表示。开发人员快速编写跨多个操作环境运行的应用程序。可以通过缩减规模(例如使用 [Knative](https://medium.com/knative/knative-v0-3-autoscaling-a-love-story-d6954279a67a) 甚至到零)来降低成本，也可以通过使用普通 python 或其他编程语言来降低成本，而不是为 Azure、AWS、Google Cloud 上的服务付费。通过模块化和抽象化，以及使用容器( [Docker](https://en.wikipedia.org/wiki/Docker_(software)) 或 [Rocket](https://coreos.com/rkt/) )，它的管理变得很容易，并且您可以在一个地方监控您的所有应用程序。****

> ****"使用声明式管道而不是命令式管道."****

****与 Kubernetes YAML 文件一样，你开始描述性地工作，意味着你定义*应该做什么*而不是*如何做*。同样，我们应该致力于在数据管道中消除 Dag 中的 how(胶水),只声明 what。工具、框架和平台应该注意如何做。****

****统一数据管道解决方案 [Ascend](https://www.ascend.io/) 的创始人肖恩·纳普引用道:“声明式编程是一种表达计算逻辑而不描述其控制流的范式……试图通过描述程序必须完成的事情来最小化或消除副作用。”查看他关于[智能编排:数据缺失的环节](https://www.datacouncil.ai/hubfs/Data%20Council/slides/nyc19/Sean-Knapp-Ascend%20-%20Intelligent%20Orchestration%20-%20Data%20Council%20NY%202019.pdf)的精彩演讲中的更多细节。****

****![](img/40d670afcc9f90d70a509afe3da9288d.png)****

*****陈述性 vs 命令性 by* [*肖恩·纳普*](https://www.linkedin.com/in/seanknapp)*&*[*ascend . io*](https://www.ascend.io/)*摘自* [智能编排](https://www.datacouncil.ai/hubfs/Data%20Council/slides/nyc19/Sean-Knapp-Ascend%20-%20Intelligent%20Orchestration%20-%20Data%20Council%20NY%202019.pdf)****

****这可能是显而易见的，但很难实现。这就是为什么 [Ascend](https://www.ascend.io/) 提供了一个卓越的一体化平台，让你可以做到这一点。它为你理清了所有的方法，而你关注的是什么。****

****如果碰巧你没有这样的平台，你可能需要一个支持它的架构。我会说，这不是自然而然的事情，你需要围绕这个范式来构建。但是更快的周期和更多的解决问题的方法可能是值得的。****

## ****管道编排的演变****

****快速概述我们在流程编排和管道工具类型方面的进展:****

1.  ****从所有调度工具之母 [cron](https://en.wikipedia.org/wiki/Cron) 到更****
2.  ****更多的图形 ETL 工具，如 [Oracle OWB](https://en.wikipedia.org/wiki/Oracle_Warehouse_Builder) 、 [SQL Server 集成服务](https://docs.microsoft.com/en-us/sql/integration-services/sql-server-integration-services?view=sql-server-ver15)、 [Informatica](https://www.informatica.com/) 等等****
3.  ****以代码为中心的工具，如[气流](https://airflow.apache.org/)、[路易吉](https://github.com/spotify/luigi)、 [Oozie](https://oozie.apache.org/)****
4.  ****到 python 框架如[提督](https://www.prefect.io/)、 [Kedro](https://github.com/quantumblacklabs/kedro) 、 [Dagster](https://github.com/dagster-io/dagster/) 甚至完全 SQL 框架 [dbt](https://www.getdbt.com/)****
5.  ****到声明管道完全管理到 [Ascend](https://www.ascend.io/) 、 [Palantir Foundry](https://www.palantir.com/palantir-foundry/) 和其他数据湖解决方案****

******如果您感兴趣，可以查看** [**令人敬畏的管道列表**](https://github.com/pditommaso/awesome-pipeline#pipeline-frameworks--libraries) 。在我看来，如果你对目前最流行的 Apache 气流的替代品感兴趣，可以看看常见的气流替代品。****

> ****"使用数据目录拥有一个中央元数据存储."****

****[数据目录](http://cidrdb.org/cidr2017/papers/p111-hellerstein-cidr17.pdf)是一个集中的存储器，里面存放着你所有的元数据数据。现在与元数据存储、数据发现或类似的同义词。这是至关重要的，因为有了数据湖和其他数据存储库，你需要保持一个概览和搜索数据的能力。****

****Lyft 提供了一个完美的例子。他们在一个名为[阿蒙森](https://github.com/lyft/amundsen)的数据目录上实现了一个应用程序。Amundsen 不仅显示了可用的数据集，还显示了谁在哪一年创建了它。此外，关于多少行、最小/最大条目等的元数据。如果支持连接的数据库，将显示一个关于表。它甚至整合了一个[评级](https://eng.lyft.com/amundsen-lyfts-data-discovery-metadata-engine-62d27254fbb9)系统，用户可以对一个数据集给出反馈，让你感受一下数据质量以及使用这个数据集的有效性。****

****最重要的是，Amundsen 将数据集与仪表板和笔记本连接起来，以显示特定数据集在其中的哪些地方被使用过。这避免了重复工作，并且您可以很快找到数据问题的答案。****

****![](img/291d719986241f8d140c63a21ba1f5a7.png)****

*****Ammundsen 2019 年 4 月 2 日的一个例子，发布在“* [Lyft 的数据发现&元数据引擎](https://eng.lyft.com/amundsen-lyfts-data-discovery-metadata-engine-62d27254fbb9)*作者* [Mark Grover](https://eng.lyft.com/@mark_grover)****

> ****“如果你没有开发人员或时间，就使用闭源软件。”****

****由于上面提到的并非都是在一天之内构建的，如果没有必要的团队或财务，可能不会如此温和，所以我还包括了封闭源代码的[平台即服务(PaaS)](https://en.wikipedia.org/wiki/Platform_as_a_service) 解决方案，这些解决方案会让您付出一定的成本，但会立即提供开箱即用的所有优势。我自己使用的两种解决方案是提到的 [Ascend](https://www.ascend.io/) 或 [Palantir Foundry](https://www.palantir.com/palantir-foundry/) 。肯定还有更多，如果你知道任何真正的，让我知道。****

****对于更具体的解决方案，我看到以下几点:****

*   ****如果使用[](https://en.wikipedia.org/wiki/Online_analytical_processing)**(slice and dice 即席查询)，从 [Imply.io](https://imply.io/) 中选择一个托管[德鲁伊](https://druid.apache.org/)集群。******
*   ******如果需要**云数据仓库**，选择[雪花 DB](https://www.snowflake.com/product/) 。******
*   ****如果你已经有许多不同的系统，并且你想把它们整合在一起，选择**数据虚拟化**解决方案 [Dremio](https://www.dremio.com/) ，它使用 [Apache Arrow](https://arrow.apache.org/) 作为动力。****
*   ****如果你主要想关注**仪表盘和**报表，选择 [Looker](https://looker.com/) 或 [Sisense](https://www.sisense.com/) ，这给你一个托管立方体类型的解决方案。****

****在我之前的[帖子](https://www.sspaeti.com/blog/olap-whats-coming-next/#List_of_Cube-Replacements)中也可以看到一个更完整的列表，其中包含更多替换立方体的选项(尽管从 2018 年 11 月起不再完全更新)。****

# ****结论****

****我们已经看到，商业智能的目标是生成业务和组织范围的概览，以及创建透明、处理非结构化数据格式或实时数据可用性的数据仓库的最具挑战性的问题。然后我们如何用十二种新兴的数据工程技术和方法来解决它们。****

****我希望这些方法是有帮助的，并将解决您的 BI 系统的一些挑战。或者帮助您构建数据架构，或者降低一些复杂性。做这一切是一项相当大的工作。然而，如果你开始挑选对你最相关的，我想你是在正确的道路上前进。可能也是最大的优势是，您的架构将面向未来，并为大数据和云原生解决方案做好准备。****

****如果你既没有时间也没有资源，你可以选择一个闭源平台作为服务，就像我们在最后一种方法中看到的那样，这也很好。从长远来看，这将增加您的成本，但是您可以立即开始运行。****

****要查看实际使用的工具，请查看我的实践帖子[在 20 分钟内构建一个数据工程项目](https://www.sspaeti.com/blog/data-engineering-project-in-twenty-minutes/)。暂时就这样了。让我知道你对此的想法，你有什么工具和框架来解决一些挑战？我将感谢你的评论。****

*****原载于 2020 年 6 月 14 日*[*sspaeti.com*](https://www.sspaeti.com/blog/business-intelligence-meets-data-engineering/)T22。****