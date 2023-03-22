# 数据湖指南——现代批量数据仓库

> 原文：<https://towardsdatascience.com/a-guide-to-modern-batch-data-warehousing-extraction-f63bfa6ef878?source=collection_archive---------10----------------------->

![](img/e9df428b923241482024963c460ce7b6.png)

照片由 [**弗拉德 Chețan**](https://www.pexels.com/@chetanvlad?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 发自 [**像素**](https://www.pexels.com/photo/water-flowing-down-on-mossy-rock-2957464/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

## 使用“功能数据工程”重新定义批量数据提取模式和数据湖

过去几十年，数据分析领域发生了巨大的变革。随着存储成本的降低和云计算的采用，指导数据工具设计的限制变得过时，因此—数据工程工具和技术**必须**发展。

我怀疑许多数据团队正在进行复杂的项目，以使他们的数据工程栈现代化，并使用他们所掌握的新技术。许多其他公司正在从零开始设计新的数据生态系统，这些公司正在寻求机器学习和云计算的进步所带来的新商机。

*pre-*[*Hadoop*](https://hadoop.apache.org/)批处理数据基础架构通常由与其存储紧密耦合的数据仓库(DW)设备(例如 Oracle 或 Teradata DW)、提取转换加载(ETL)工具(例如 SSIS 或 Informatica)和商业智能(BI)工具(例如 Looker 或 MicroStrategy)组成。在这种情况下，数据组织的哲学和设计原则是由诸如 Ralph Kimball 的*The Data Warehouse Toolkit*(1996)或比尔·恩门的*Building The Data Warehouse*(1992)等书中概述的成熟方法所驱动的。

我将这种方法与其现代版本进行了对比，后者诞生于云技术创新和降低存储成本。在现代堆栈中，由数据仓库设备处理的角色现在由专门的**组件处理，如文件格式(如 [Parquet](http://parquet.apache.org/) 、 [Avro](https://avro.apache.org/) 、[胡迪](https://hudi.incubator.apache.org/))、廉价云存储(如 [AWS S3](https://aws.amazon.com/s3/) 、 [GS](https://cloud.google.com/storage/) )、元数据引擎(如 [Hive](https://hive.apache.org/) metastore)、查询/计算引擎(如 Hive、*拖放* ETL 工具不太常见，取而代之的是一个调度器/指挥器(例如 [Airflow](https://airflow.apache.org/) 、 [Luigi](https://luigi.readthedocs.io/en/stable/) )和“特设”软件逻辑来承担这个角色。“ad-hoc”ETL 软件有时出现在单独的应用程序中，有时出现在调度程序框架中，该框架通过设计**是可扩展的**(气流中的操作符，Luigi 中的任务)。它通常依赖 Spark clusters 或 DWs 等外部计算系统进行大量转换。BI 方面也看到了称为[超集](https://superset.incubator.apache.org/)的开源替代方案的兴起，有时由 [Druid](https://druid.apache.org/) 补充，以创建汇总、在线分析处理(OLAP)立方体，并提供快速只读存储和查询引擎。**

# 存在的理由

我发现自己正在从事从*前 Hadoop* 堆栈到现代堆栈的迁移。这不仅是一次技术转变，也是一次重大的**范式转变**。在某些情况下，在建模和架构决策中，我们应该远离*过时的*智慧和最佳实践，理解我们为什么这样做是很重要的。我发现 [Maxime Beauchemin](https://medium.com/u/9f4d525c99e2?source=post_page-----f63bfa6ef878--------------------------------) 的资源非常有帮助，学习/理解 Apache Airflow 的设计选择在实现他提倡的方法(Airflow 是由 Maxime 创建的)时带来了很多实际的理解。本指南旨在采用一种*固执己见的*方法来定义和设计数据湖。

我挑选了一些特定的技术来使本指南更加实用，我希望其中的大部分技术也适用于现代堆栈中的其他工具。选择一种技术而不是另一种技术的动机通常是我的经验(或缺乏经验)的结果。例如，我会提到 AWS 工具，因为这是我有经验的云提供商。

这篇博文定义了 ETL 的 E，并描述了**数据湖**的角色。

# 提取，血统

在数据工程中，提取应该是在给定时间点*实体*状态的**未改变快照**。

具体来说，它通常涉及调用 API、抓取网站、从安全文件传输协议(SFTP)服务器获取文件、定期对运行数据库的副本运行查询以及从 S3 帐户复制文件。提取的结果存储在一个便宜的、可扩展的、高可用性的云存储中，比如 S3，可以永久保存*——或者只要合规允许就保存。这些提取产生的数据构成了数据湖。*

## *数据湖*

*不同的作家、博客作者和专业人士对“数据湖”和“数据仓库”有不同的定义。有时，他们的角色没有被清楚地陈述或者有重叠——以至于造成混乱，这两个词可以互换使用。数据湖的以下定义很简单，它清楚地将数据湖从数据仓库中分离出来，并将原始数据(数据湖的一部分)从派生的数据集(数据仓库/数据集市的一部分)中分离出来。*

*数据湖是一个存储库，包含所有由业务产生或收集的未处理的数据。因为此时没有业务逻辑应用于数据，**保持不变**，如果业务需求发生变化，任何分析(表格、数据科学模型)都可以从该来源重新创建。从不同来源提取数据是必要的，因为从来源获取数据通常是昂贵的(API 调用、缓慢的 SFTP、操作数据库转储)，有时是不可能的(API 发展、sftp 变空、操作数据库就地改变记录)。*

## *结构*

*随着数据的民主化和分析的去中心化，发现**湖泊变得非常重要**，考虑以下结构:*

```
*s3://myorg-data-lake
├── s3://myorg-data-lake/tweets_mentioning_myorg
└── s3://myorg-data-lake/salesforce_clients*
```

*数据湖消费者希望任何摘录都是“my org-Data-Lake”S3 存储桶中的顶级前缀，以便于浏览。然而，数据不需要放在同一个桶中，因为我们可以使用 Hive metastore 将任何提取注册为同一个模式中的表，提供一个到数据湖的**中央接口**。*

```
*data_lake
├── data_lake.tweets_mentioning_myorg → s3://myorg-twitter-extracts/tweets_mentioning_myorg
└── data_lake.salesforce_clients → s3://myorg-salesforce-extracts/salesforce_clients*
```

## *格式*

*半结构化和非结构化数据经常被认为是数据湖的一个特征。然而，我相信在提取过程中，转换成一种嵌入了模式的文件格式(比如 Parquet 和 Avro)有很大的好处。模式是定义数据集接口的一种方式，使得多个团队在需要最少通信的情况下更容易使用它。这也是一种执行轻量级验证的方式，验证源系统仍然在生成预期的数据。当模式不再有效时，提取*中断*，但是它降低了产生错误分析或转换过程中隐藏错误的风险。提取的数据可能已经有了某种模式(数据库和 API 提取)，存储为 JSON / CSV 将意味着丢失一些有价值的元数据。*

*在源系统使以前的数据很快不可用的情况下，提取一次**而不进行任何转换**，并在原始副本上运行转换。然后，配置单元表可以指向转换后的副本。例如，如果 we 文件来自 SFTP 服务器，并且这些文件在大约 1 小时后消失:*

```
*SFTP
├── file1.csv
└── file2.csvs3://myorg-data-lake
├── s3://myorg-data-lake/sftp_clients/raw/file1.csv
├── s3://myorg-data-lake/sftp_clients/raw/file2.csv
└── s3://myorg-data-lake/sftp_clients/parquet/…*
```

*通过这种方式，文件在 S3 上，模式可以被修复，而不用担心丢失任何数据。*

*由于大数据框架利用文件元数据和特殊数据布局来优化计算，因此生成的文件处理速度通常会更快。使用这种文件格式的另一个好处是**减小了文件大小**。模式、编码技术和特殊的*数据布局*，如列存储，允许这些库避免冗余——减少表示相同信息所需的字节量。*

## *文件大小*

*许多 Hadoop 系列框架在处理少量大文件时比处理大量小文件时效率更高。原因是读取每个文件的元数据有开销，启动并行下载文件的进程也有开销。因此，**合并从数据库的多个查询、多个 API 调用或多个 SFTP 文件中提取的数据**以减少文件数量，可以为下游转换节省大量时间。像 Snappy 这样的**压缩**库也可以用来减少通过网络的数据量，并且通常值得这样做，因为引入的 CPU 负载可以忽略不计。*

## *没有变化*

*数据湖应该包含在给定时刻**的*状态*的不变**真值** — 未修改快照。**此时任何转型都是不可取的— 合规流程除外(如匿名化)。*

*如果数据在被复制到数据湖之前被更改，它将偏离其在源系统中捕获的状态。如果源系统使以前的数据不可用，并且应用的逻辑需要撤销，数据将不得不进一步变异。这是危险的，因为它可能会导致**无法回滚更改**来获取原始提取。*

*有时，源系统中的错误会导致产生不正确的数据。在这些方面，我们可能希望它反映在我们的捕获(和分析)中，或者我们可能希望它得到纠正。在后一种情况下，重新运行相关时间窗口的提取过程可能就足够了，或者可能需要人工干预，但是在这两种情况下，*物理分区*仅用于**覆盖**相关数据。*

## *物理分区*

*对提取的数据进行分区对于实现幂等性和优化提取大小非常重要。Maxime Beauchemin 在他的[功能数据工程博客](https://medium.com/@maximebeauchemin/functional-data-engineering-a-modern-paradigm-for-batch-data-processing-2327ec32c42a)中为幂等 ETL 提供了一个强有力的案例。给定一个“预定的执行日期”，提取应该总是产生相同的结果。这有时*是不可能的*，因为我们依赖于我们无法控制的外部资源，但是这些提取仍然应该被分组到分区中，以便下游的转换可以是等幂的。*

*这是一个幂等每日摘录的简单示例:*

```
*SELECT * FROM customers
WHERE last_modified_date >= 2020–01–01
  AND last_modified_date <  2020–01–02*
```

*在实践中，我们将参数化上面的 SQL 以从执行日期导出下限和上限*

```
*SELECT * FROM customers
WHERE last_modified_date >= {{ execution date - 1 day }}
  AND last_modified_date <  {{ execution date }}*
```

*将其与下面的**错误示例**进行比较，其中结果将根据外部因素(今天的日期)而变化:*

```
*SELECT * FROM customers
WHERE last_modified_date > 2019–01–01*
```

*在 Airflow 中，[执行日期](https://airflow.apache.org/docs/stable/macros.html#default-variables)通常用于实现幂等。执行日期是一个强大的概念，它是一个**不可变的日期**在运行时赋予一个*管道*(气流中的有向无环图)。如果 DAG 计划在*2019–01–01 00:00:00*运行，这就是执行日期。如果该管道失败，或者需要重新运行，则可以清除该特定运行的状态，然后它将获得**相同的执行日期**并产生相同的提取，条件是提取基于执行日期和幂等。这使得并行运行多个提取过程成为可能，并且通常用于回填数据。*

*例如，我使用相同的(简单的)技术从 API 回填数据，并使用 Airflow 的特性来限制并行性，并自动重试或超时调用。这是气流的一个常见用例，通过幂等(非重叠)提取使其成为可能。*

*类似地，我们可以运行端到端 ETL 的多个实例。当试图重新处理一个*不寻常的*时间范围内的数据时，这尤其有用。例如，如果转换代码和定义的资源是针对每天的数据量进行测试的，那么很难知道相同的设置对于整个月的数据量会有什么样的表现。相反，ETL 的一个实例可以在被重新处理的一个月中的每一天启动——可能并行执行(这在 Airflow 中很容易实现)。*

*提取的物理分区应基于提取处理的计划运行日期。最简单的例子是每日批量处理的每日提取物:*

```
*s3://myorg-data-lake/sftp_clients/parquet/ds=2020-01-01
s3://myorg-data-lake/sftp_clients/parquet/ds=2020-01-02
s3://myorg-data-lake/sftp_clients/parquet/ds=2020-01-03*
```

**ds 代表日期戳，这里:执行日期**

*请注意表示 Hadoop 生态系统中的物理分区的`key=value`格式——物理分区被转换为数据集的一列，当在 WHERE 子句中使用时，允许跳过所选分区之外的所有文件(分区修剪)。*

*提取过程应该以这样的方式编写，即根据给定的执行日期覆盖分区**。然后，转换过程可以使用`ds`作为过滤器来获取它们需要处理的数据。***

*其他时间框架遵循相同的原则，但在每小时的情况下，我们可能要考虑*流*——批处理系统往往会有开销，使它们无法用于非常短的批处理。*

*另一个有趣且常见的用例是，我们在*提取数据的频率高于我们在*转换数据的频率。当从源中提取一个大的时间范围使其超负荷时，就会出现这种需要，因为数据有以后不可用的风险，或者在转换之前一次提取所有数据会延迟下游过程。例如，我们可能希望创建每小时的提取，但是每 12 小时处理一次数据。为了设计这样一个需要以不同节奏调度任务的流水线，我们可以使用[逻辑分支](https://airflow.apache.org/docs/stable/concepts.html?highlight=branch#branching) 到在我们需要的时候选择性地运行转换过程。*

*这种较高的提取频率会产生不希望的小文件，在这种情况下，在转换之前，我们合并要处理的批处理。当文件非常小，以至于将元数据(模式、统计数据)写入每一个文件会产生很大的开销时，应该在文件合并后转换为模式嵌入文件。在这种情况下，请考虑以下结构:*

```
*s3://myorg-data-lake/sftp_clients/raw/ds=2020-01-01<space>00:00:00
s3://myorg-data-lake/sftp_clients/raw/ds=2020-01-01<space>01:00:00
s3://myorg-data-lake/sftp_clients/raw/ds=2020-01-01<space>02:00:00
...
s3://myorg-data-lake/sftp_clients/raw/ds=2020-01-01<space>12:00:00
--
s3://myorg-data-lake/sftp_clients/parquet/ds=2020-01-01<space>12:00:00*
```

*其中合并+转换+压缩过程将把 12 个分区变成 1 个，并且`sftp_clients`表将指向拼花版本而不是原始副本。*

# *最后*

*在这篇文章中，我试图给数据湖和提供数据的提取过程添加一些结构。希望你觉得有用！*