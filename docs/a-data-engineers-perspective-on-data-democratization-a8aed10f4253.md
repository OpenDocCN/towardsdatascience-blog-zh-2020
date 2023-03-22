# 数据工程师对数据民主化的看法

> 原文：<https://towardsdatascience.com/a-data-engineers-perspective-on-data-democratization-a8aed10f4253?source=collection_archive---------19----------------------->

## 数据工程在数据民主化中的作用

## 如何塑造数据工程工作，以促进数据访问和扩展数据驱动型公司

![](img/8f653c9270b2ea05a860f8c37664769b.png)

照片由[小森正明](https://unsplash.com/@gaspanik?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/locked?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

> 数据民主化是使数据在组织中可访问的过程:消除数据和任何希望在公司中构建产品、分析或决策的人之间的障碍和瓶颈

# 为什么数据民主化是一个新话题？

在过去(云计算出现之前，Hadoop 出现之前)，**计算资源非常昂贵**。数据工程师需要遵循严格的规则和建模技术，以经济可行的方式生成报告(和其他输出)。

在前云时代，公司有一个或几个**集中的**数据团队——充当仁慈的数据看门人。对数据、分析或数据驱动产品的任何请求都需要仔细规划、界定范围并交给这些团队。

虽然这种方法**是必要的**，但如此高水平的控制也带来了一些问题:

*   数据团队成为**瓶颈**
*   组织只能访问**高度处理的** **数据输出**(报告、模型化的表格)，领域专家不能探索数据集来寻找机会。
*   数据的消费者对数据过程几乎没有可见性和理解，这种缺乏透明度导致对数据缺乏信心。
*   领域专家不能参与数据的建模和转换，因此**需求**需要**彻底定义**。

# 您的数据民主化程度如何？

对数据的访问通常是不平等的，这取决于一个人的技术能力和他所在的团队。为了评估给定业务中数据的民主化程度，我想提供以下场景作为起点:

**担任** *【数据工程师|数据科学家|数据分析师|软件工程师|产品经理|主管等..****【移动团队|数据团队|营销团队】等..]，* **我能有多轻松...****

*   ****发现**可用的数据集和见解。**
*   ****浏览** **元数据**关于数据集的信息(列内容、表的更新速率、表的更新策略、表的健康状况，例如 QC 分数度量、表是如何产生的)。**
*   ****探索**原始或相当未加工的数据。**
*   **从现有数据生成新的数据集。**
*   ****创造和分享**见解、发现和实验。**
*   ****摄取**新的数据源。**

**虽然本评估中强调的一些问题有不同的解决方案，这取决于组织的类型:其规模、资源/预算、数据用户的技术能力，但有一些模式和工具有助于*大多数情况*。**

# **在您的组织中民主化数据的方法**

**本节的目的是讨论一些*非互斥*的方法来使组织中的数据民主化。其中一些可能对您的组织有用，一些可能不够，但希望它能帮助您制定自己的解决方案！**

**![](img/c4a05329603984491b722f1757951db9.png)**

**Silas k hler 在 [Unsplash](https://unsplash.com/s/photos/find-key?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片**

# **团队结构**

## **分散数据团队**

**在每个团队中嵌入数据工程师可以让团队自给自足。探索数据和创建数据管道的技术障碍可能仍然存在，但每个团队现在都拥有独立**所需的技能**。**

**这些数据工程师也变得熟悉团队的任务，并获得领域专业知识，这使他们能够更好地理解需求。**

## **数据平台团队**

**专注于数据基础设施和工具的团队可以帮助减少执行常见任务所需的技术技能。这个工具可以是现成的、开源的或者由团队开发的。**

****“数据平台化”项目示例:****

*   **允许任何人将任意文件放入数据湖并定义模式的 Web 服务。**
*   **托管 [Airflow](https://airflow.apache.org/) 并将其作为服务提供给其他团队。**
*   **通过为 [EMR](https://aws.amazon.com/emr/) 提供模板，抽象出为 Spark 定义集群资源的复杂性(例如，我想要一个“小型”集群，而不是:我想要一个按需配备 1 个 r5 . 2x 大型主节点、5 个 r5 . 4x 大型 Spot 机群的集群……)。**

# **元数据中心**

**“元数据中心”通常采用内部搜索引擎的形式，对数据集进行分类，并公开潜在数据消费者使用表格所需的信息。至少，它应该回答以下问题:**

*   ****表内容**是什么(包括列级信息)。**
*   ****桌主**是谁(团队)。**
*   **该表的健康程度(对该表进行评论，或 QC 评分)。**
*   ****数据谱系**是什么(例如，链接到气流 DAG，或[图谱](https://atlas.apache.org/#/))。**
*   **表中的**更新策略**是什么(如每日快照、每周增量更新)。**

## **元数据中心的实现**

**许多公司已经实施了元数据中心解决方案:**

> **[AirBnb 的数据门户](https://medium.com/airbnb-engineering/democratizing-data-at-airbnb-852d76c51770)，[优步的数据手册](https://eng.uber.com/databook/)，[网飞的 Metacat](https://medium.com/netflix-techblog/metacat-making-big-data-discoverable-and-meaningful-at-netflix-56fb36a53520) ， [Lyft 的阿蒙森](https://eng.lyft.com/amundsen-lyfts-data-discovery-metadata-engine-62d27254fbb9)，以及最近谷歌的[数据目录](https://cloud.google.com/data-catalog/)。[以及 [LinkedIn 的数据中心](https://engineering.linkedin.com/blog/2019/data-hub)**
> 
> **[(来源](https://engineering.linkedin.com/blog/2019/data-hub))**

**[Amundsen、](https://github.com/Lyft/amundsen) [DataHub](https://github.com/linkedin/datahub) 和 [Metacat](https://github.com/Netflix/metacat) 是开源的，可以在 GitHub 上获得，利用这些开源工具可以使资源有限的数据工程团队更容易支持元数据中心。**

**下面的文章更深入地介绍了每个项目的架构:**

**[](/how-linkedin-uber-lyft-airbnb-and-netflix-are-solving-data-management-and-discovery-for-machine-9b79ee9184bb) [## LinkedIn、优步、Lyft、Airbnb 和网飞如何为机器解决数据管理和发现问题…

### 当谈到机器学习时，数据无疑是新的石油。管理数据集生命周期的流程是…

towardsdatascience.com](/how-linkedin-uber-lyft-airbnb-and-netflix-are-solving-data-management-and-discovery-for-machine-9b79ee9184bb) 

# 数据质量检查(QC)

当表的创建是分布式的时，确保高数据质量是一个挑战。QC 过程也需要分布，创建表的人应该能够对他们自己的表进行 QC！

在 [Earnest](https://www.earnestresearch.com/) 时，我们有 2 个内部 QC 工具+ 1 个机器学习使能的异常检测工具，它们分发电子邮件和关于桌子健康的信息。然而，我们发现**很难将**扩展到分析师要求的更多 QC 检查，因为它们需要工程时间。

我们已经开始探索 Great Expectations (GE)，我们希望通过让分析师参与到 QC 工作中来扩大规模。

点击此处了解更多关于远大前程的信息:

[](https://medium.com/@expectgreatdata/down-with-pipeline-debt-introducing-great-expectations-862ddc46782a) [## 打倒管道债务/引入更高的期望

### TL；DR:管道债务是一种寄生于后端数据系统的技术债务。它会降低生产率，并且…

medium.com](https://medium.com/@expectgreatdata/down-with-pipeline-debt-introducing-great-expectations-862ddc46782a) 

# 数据工程角色

从理论上讲，如果公司里的任何潜在数据消费者都获得了编写 ETL 所需的工具，这将是数据民主化的**最大推动者**之一。所以，自然，这是我非常感兴趣的事情(你也应该！).

> *“工程师不应该写 ETL。*出于对职业中一切神圣不可侵犯的事物的热爱，这不应该是一个专注或专门的角色。没有什么比编写、维护、修改和支持 ETL 来产生您自己从未使用或消费过的数据更令人神魂颠倒的了。”
> 
> [(来源)](https://multithreaded.stitchfix.com/blog/2016/03/16/engineers-shouldnt-write-etl/)

[](https://multithreaded.stitchfix.com/blog/2016/03/16/engineers-shouldnt-write-etl/) [## 工程师不应该写 ETL:建立高功能数据科学部门指南

### “你的团队和数据科学家之间的关系是什么样的？”毫无疑问，这是我的问题…

multithreaded.stitchfix.com](https://multithreaded.stitchfix.com/blog/2016/03/16/engineers-shouldnt-write-etl/) 

数据工程师应该帮助**简化**编写 ETL 的过程，组织应该**培训**数据消费者使用数据工程师提供的工具。

一家公司需要在以下两者之间找到一个中间点:

*   数据工程师创建完全抽象的工具，没有技术知识的数据消费者可以使用，但是需要大量的工程工作来创建。
*   数据工程师提供的工具**需要如此多的技术知识**，以至于培训数据消费者使用它们**是不可行的**。

数据构建工具(DBT)是编写转换逻辑工具的一个很好的例子。它将一个结构强加到基于 SQL 的项目中，并使用 [Jinja](https://jinja.palletsprojects.com/en/2.11.x/) (Python)提供 SQL 的模板。假设数据消费者知道 SQL，培训他们了解 DBT 的细节将允许他们在编写可维护的转换时能够**自主**。

阅读更多关于 DBT 的信息:

[](https://blog.getdbt.com/what--exactly--is-dbt-/) [## dbt 到底是什么？

### 自从 2017 年写这篇文章以来，dbt 已经发生了巨大的变化。最大的变化是发布了一个…

blog.getdbt.com](https://blog.getdbt.com/what--exactly--is-dbt-/) 

## 数据工程师不编写数据管道时的角色

数据工程师提供并支持组成管道的单个**组件**。例如，在 [Earnest](https://www.earnestresearch.com/) 中，我们在 Airflow 上运行大多数容器化的任务，因此作为数据工程师，我们维护多个容器化的 CLI 工具来执行任务，如将 Hive 模式转换为红移模式、在 [Livy](https://livy.apache.org/) 上运行 Spark 作业、运行我们的自定义 QC 任务等..

数据工程师还**提供工具**(例如元数据中心、气流、远大前程、雪花),他们支持、扩展和创建抽象，以**提高数据消费者的生产力**。

一旦数据工程师从制作**单个管道**中解脱出来，他们就可以研究有益于整个**管道的工具**。LinkedIn 的 [Dr-Elephant，](https://github.com/linkedin/dr-elephant)是一个检测 Spark 和 Hadoop 作业上常见优化机会的工具，是这种工具的一个很好的例子。

此外，数据工程师需要通过确保他们的工具可以被数据消费者直接使用来保持数据的可访问性，要求**最少或没有工程**的干预。

> “数据工程师可以专注于管道等幂、数据湖中新资源的集成、数据血统和工具。”
> 
> [(来源](https://medium.com/dailymotion/collaboration-between-data-engineers-data-analysts-and-data-scientists-97c00ab1211f))

[](https://medium.com/dailymotion/collaboration-between-data-engineers-data-analysts-and-data-scientists-97c00ab1211f) [## 数据工程师、数据分析师和数据科学家之间的协作

### 如何在生产中高效发布？

medium.com](https://medium.com/dailymotion/collaboration-between-data-engineers-data-analysts-and-data-scientists-97c00ab1211f) 

# 最后

数据民主化是关于增加对数据的访问，确保拥有不同技能的不同团队同样有能力提供见解和数据产品。

成功的数据民主化的**关键**很可能是确保您的不同过程**随着数据需求的增加而扩展**。

你的目标应该是识别并消除**瓶颈**。

## 这篇文章中的问题和建议解决方案的摘要:

*   **问题:**团队没有自主创建数据计划所需的技能| **解决方案:**在团队中嵌入数据工程师
*   **问题:**团队缺少执行常见数据任务的工具| **解决方案:**创建数据平台计划
*   **问题:**数据消费者很难找到他们需要的信息，以决定是否可能使用现有的表/数据集| **解决方案:**创建一个中心位置来查找这些信息(“元数据中心”)
*   **问题:**由于表的创建是分布式的，数据质量正在下降| **解决方案:**提供工具，使团队能够拥有他们创建的表的 QC
*   **问题:**创建 ETL 管道很慢，因为数据工程资源需求过大| **解决方案:**使管道的创建民主化和分布化**