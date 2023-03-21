# 使用 Spark 构建 AWS 数据湖时的一些问题以及如何处理这些问题

> 原文：<https://towardsdatascience.com/some-issues-when-building-an-aws-data-lake-using-spark-and-how-to-deal-with-these-issues-529ce246ba59?source=collection_archive---------17----------------------->

## 技术提示

## 这是一篇给所有为 Spark 和 Data Lake 而奋斗的人的长文

# 介绍

起初，编写和运行 Spark 应用程序似乎很容易。如果您在使用 pandas、NumPy 和 Python 中的其他包以及/或者 SQL 语言操作数据帧方面经验丰富，那么使用 Spark 为我们的数据创建 ETL 管道是非常相似的，甚至比我想象的要容易得多。与其他数据库(如 Postgres、Cassandra、AWS DWH 红移)相比，使用 Spark 创建数据湖数据库似乎是一个轻松的项目。

但是，当您在云服务 AWS 上部署 Spark 应用程序和您的完整数据集时，应用程序开始变慢并出现故障。您的应用程序永远在运行，当您观察 AWS EMR 控制台时，您甚至不知道它是否在运行。您可能不知道问题出在哪里:很难调试。Spark 应用程序在本地模式和独立模式之间、测试集(数据集的一小部分)和完整数据集之间的行为是不同的。问题的清单越来越长。你感到沮丧。真的，你意识到你对 Spark 一无所知。好吧，乐观地说，这确实是一个非常好的机会来了解更多关于 Spark 的知识。无论如何，遇到问题是编程中的正常现象。但是，如何快速解决问题呢？从哪里开始？

在使用 Spark 创建了一个数据湖数据库之后，我迫切地想要分享我所遇到的问题以及我是如何解决这些问题的。希望对你们有些帮助。如果我错了，请纠正我。反正我还是 Spark 的新手。现在，让我们开始吧！

> **注意事项**
> 
> 1.本文假设你已经具备一些 Spark 的工作知识，尤其是 PySpark、命令行环境、Jupyter 笔记本和 AWS。更多关于 Spark 的内容，请阅读参考文献[这里](https://spark.apache.org/docs/latest/)。
> 
> 2.您有责任监控您使用的 AWS 帐户的使用费。每次完成工作时，请记住终止集群和其他相关资源。EMR 集群成本高昂。
> 
> 3.这是优达城数据工程纳米学位评估项目之一。所以为了尊重 Udacity 荣誉代码，我不会在工作流中包含完整的笔记本来探索和构建项目的 ETL 管道。本教程的 Jupyter 笔记本版本的一部分，以及关于 Spark 的其他教程和更多数据科学教程可以在我的 [github 上找到。](https://github.com/nhntran)

## 参考

*   一些材料来自 Udacity 上的数据工程纳米学位项目。
*   一些想法和问题是从 Knowledge-uda city 问答平台和学生中心-uda city 聊天平台收集的。感谢大家对我们的付出和巨大贡献。

# 项目介绍

## 项目目标

Sparkify 是一家致力于音乐流媒体应用的初创公司。通过 app，Sparkify 已经收集了关于用户活动和歌曲的信息，这些信息被存储为 JSON 日志的目录(`log-data` -用户活动)和 JSON 元数据文件的目录(`song_data` -歌曲信息)。这些数据位于 AWS 上的公共 S3 存储桶中。

为了促进业务增长，Sparkify 希望将他们的流程和数据转移到云上的数据湖。

这个项目将是一个工作流程，探索和建立一个 ETL(提取—转换—加载)管道,它:

*   从 S3 提取数据
*   在 AWS 集群上使用 Spark 将数据处理到分析表中
*   将数据作为一组维度和事实表加载回 S3，供 Sparkify 分析团队继续深入了解用户正在收听的歌曲。

以下是来自 JSON 日志文件和 JSON 歌曲文件的示例:

![](img/2dc7e19a83479df8a9a57f71f395a2f4.png)

log_data json 文件的示例

![](img/12570099efc3e858802cbb9883f97004.png)

song_data json 文件示例

该数据库的维度和事实表设计如下:
字段以**粗体显示**:分区键。

![](img/d224ea2e31a9a834a4693689d281e192.png)

(使用[https://dbdiagram.io/](https://dbdiagram.io/)制作 ERD 图)

**项目工作流程**

这是我的项目工作流程。一个有经验的数据工程师可能会跳过这些步骤，但对我来说，我宁愿慢慢来，学习更多:

*   使用 Jupyter 笔记本对本地目录中的样本数据逐步构建 ETL 流程；将输出写入本地目录。
*   使用 AWS S3 上的子数据集验证 ETL 过程；将输出写入 AWS S3。
*   将所有代码放在一起构建脚本`etl.py`，并在 Spark 本地模式下运行，测试本地数据和`s3//udacity-den`上的数据子集。任务的输出结果可以用 Jupyter 笔记本`test_data_lake.ipynb`来测试。
*   构建并启动 EMR 集群。据我所知，你可以在 Udacity 上提交项目而不使用 EMR，但我强烈建议你在 AWS 上的 Spark 独立模式下运行它，看看它是如何工作的。你肯定会学到更多。
*   使用`s3//udacity-den`上的数据子集，在 EMR 集群上为`etl.py`提交一个 Spark 作业。
*   最后，使用`s3//udacity-den`上的完整数据集，在 EMR 集群上为`etl.py`提交一个 Spark 作业。
*   尝试使用各种选项来优化火花性能。
*   为歌曲播放分析提供示例查询和结果。这一部分在另一本叫做`sparkifydb_data_lake_demo.ipynb`的木星笔记本中有所描述。

验证和演示部分可以在 [my Github](https://github.com/nhntran/create-datalake-spark) 上找到。其他脚本文件 etl.py 和 my detailed `sparkifydb_data_lake_etl.ipynb`在 Udacity 荣誉代码方面不可用。

# 项目中的一些提示和问题

## 技巧 1 —在构建 ETL 管道以使用脚本处理整个数据集之前，在 Jupyter notebook 中逐步构建 ETL 过程。

*   Jupyter notebook 是探索性数据分析(EDA)的一个很好的环境(T7 ),可以进行测试并及时验证结果。由于调试和优化 Spark 应用程序相当具有挑战性，强烈建议在将所有代码放在一起之前逐步构建 ETL 过程。当我们谈到其他技巧时，你会看到它的优势。
*   使用 Jupyter notebook 的另一个重要原因是:创建 etl.py 脚本然后尝试调试它是不切实际的，因为每次运行 etl.py 文件时都必须创建一个 spark 会话。有了笔记本，spark 会话始终可用。

## **技巧 2——仔细研究数据集。**如果数据集很大，从一个小的子集开始项目。

为了进行项目工作，首先，我们需要知道**数据集**的概况，比如文件的数量，每个文件的行数，数据集的总大小，文件的结构等。如果我们在云上工作，这一点尤其重要，因为云上的请求会耗费大量的时间和金钱。

要做到这一点，我们可以使用**[**boto 3**](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)**、用于 Python 的亚马逊 Web 服务(AWS)SDK**。boto3 允许我们通过 IAM 用户访问 AWS。关于如何创建 IAM 用户的详细信息可以在[这里找到，步骤 2:创建 IAM 用户。](/how-to-create-and-run-an-emr-cluster-using-aws-cli-3a78977dc7f0#c46c)**

**以下是在 Jupyter 笔记本上为 S3 设置客户端的方法:**

**从 IAM 用户获得的密钥和访问密钥可以保存到本地目录下的 credentials.cfg 文件中，如下所示。**请注意，如果您将密钥和私钥放在“”或“”中，或者如果文件没有[AWS]** 这样的头，您可能会遇到“配置文件解析错误”**

**![](img/7d51914649828c6e2112a5137cfdbcf0.png)**

**credentials.cfg 文件的内容。请注意，如果将您的密钥和私钥放在“”或“”中，您可能会遇到“配置文件解析错误”。**

**使用 boto3 创建的 S3 客户端，我们可以访问项目的数据集，并查看**日志数据**和 **song_dat** a:**

**探索过程的结果是:**

**![](img/e82f92e9cb8584320bb386f1b860e2f8.png)****![](img/de8d7e55fe3bca6456a601bfec4f4642.png)**

****数据集不大，大概 3.6MB，但是，song_data 有大概 15000 个文件。最好先使用 song_data 的子集，比如' song_data/A/A/A/'或' song_data/A/'来探索/创建/调试 ETL 管道。****

## **技巧 3—在 Spark 中将文件读取到数据框时包含定义的模式**

**我的 ETL 管道在数据子集上工作得非常好。然而，当我在整个数据集上运行它时，Spark 应用程序一直冻结，没有任何错误通知。我不得不减少/增加子数据集以实际查看错误并修复问题，例如从*' song _ data/A/A/A '***更改为***' song _ data/A/'*，反之亦然。那么这里的问题是什么呢？******

*   ******结果是，在这个特定的数据上，在这个小数据集上，我的 Spark 应用程序可以自动找出模式。但是在更大的数据集上却不行，可能是由于文件之间的不一致和/或不兼容的数据类型。******
*   ******此外，使用已定义的模式，加载会花费更少的时间。******

********如何设计正确的模式:********

*   ****您可以通过查看 log_data JSON 文件和 song_data JSON 文件的结构来手动创建一个模式。为了简单的可视化，我使用熊猫数据框生成了视图，如下所示****

****![](img/2dc7e19a83479df8a9a57f71f395a2f4.png)****

****log_data json 文件的示例****

****![](img/12570099efc3e858802cbb9883f97004.png)****

****song_data JSON 文件示例****

*   ****对我来说，诀窍是**让 Spark 通过将文件的小子集读入数据帧来自己读取和理解模式，然后使用它来创建正确的模式。有了它，我们不需要猜测任何类型的数据，不管是字符串型、双精度型还是长型等等。该技巧的演示如下:******

****![](img/5226043780e2f4514bb0c4d477da13a1.png)****

## ****提示 4——打印出任务，并记录每项任务的时间****

****尽管这是编程中的最佳实践，但我们有时会忘记这样做。对于大型数据集，观察每个任务的时间对于调试和优化 Spark 应用程序非常重要。****

****![](img/e20b0e4725d27f561c32f75cf03cdd7c.png)****

****通过记录时间，我们知道阅读所有的大约需要 9 分钟。使用 spark on local 模式将 json 文件从 S3 上的 song_data 传输到 Spark 数据帧****

****除非您关闭 Spark 中的信息记录，否则很难(如果不是不可能的话)知道 Spark 应用程序在终端上的进度，因为信息记录太多了。通过打印出任务名称并记录时间，一切都会变得更好:****

****![](img/976bc0d385f7f3faf55997a2714c2dac.png)****

****打印出任务名称并记录时间有助于我们跟踪应用程序的进度****

## ****技巧 5 —在 pyspark 包中导入和使用函数的最佳方式是什么？****

****导入和使用函数至少有两种方式，例如:****

*   ****`from pyspark.sql.functions import max`****
*   ****或者`import pyspark.sql.functions as F`然后使用`F.max`****

****哪一个都可以。我更喜欢第二种方法，因为我不需要在我的脚本 etl.py 上面列出所有的函数。****

****注意，`max`函数是一个例外，因为它也是 Python 中内置的 max 函数。要使用`pyspark.sql.functions`模块的`max`功能，必须使用`F.max`或使用别名，如`from pyspark.sql.functions import max as max_`****

## ****技巧 6 —当我的 Spark 应用程序冻结时，出现了什么问题？****

****可能会有很多问题。我自己也有一些:****

1.  ******AWS 地区的区别:**设置 boto3/EMR 集群/S3 输出桶等时请务必使用 **us-west-2** 。因为可用数据集在该 AWS 区域上。****
2.  ******将文件读取到数据框时未包含定义的模式:**使用提示 3 修复。****
3.  ******在整个数据集上运行 ETL 管道需要这么长时间:**这个项目相当不切实际，因为[从 EMR/Spark 读写 S3 极其缓慢](https://stackoverflow.com/questions/42822483/extremely-slow-s3-write-times-from-emr-spark/42834182#42834182)。当在一个小的子数据集上运行 ETL 管道时，您可以看到相同的信息日志记录模式在终端上一次又一次地重复，如下所示:****

****![](img/058273e4bac9e8654ef91137fa2122bf.png)****

****这是在**的“INFO context cleaner:Cleaned accumulator XXX”**上，我发现我的 Spark 应用程序似乎一次又一次地冻结。预计这将是一个长时间运行的工作，仅将歌曲表写入 s3 存储桶就花了我 115 分钟。因此，如果你确定你的端到端流程运行良好，那么耐心等待两个小时，看看它是如何工作的。这个过程可以加快，请看下面的[提示 9](#bc52) 。****

******4。在 AWS EMR 控制台上检查运行时间**:当在 EMR 控制台上选择集群上的**应用程序用户界面**选项卡时，您可以看到 Spark 应用程序运行了多长时间。可在页面末尾找到应用程序列表:****

****![](img/97e897c39f35b10e8ef19490451e1790.png)********![](img/70de95c82e558d0398ee77b1d99f9eb2.png)****

****我在整个数据集上的 ETL 管道在 EMR 集群(1 个主节点和 2 个 m5.xlarge 类型的核心节点)上花了大约 2.1 小时完成。****

## ****技巧 7 —使用 Spark 自动递增 songplays _ id 这不是一个小问题。****

****这个问题在其他数据库中是微不足道的:在 Postgres 中，我们可以使用`SERIAL`来自动增加一列，比如`songplays_id SERIAL PRIMARY KEY`。在 AWS 红移中，我们可以使用`IDENTITY(seed, step)`。****

******使用 Spark 对表执行自动递增并不简单，至少当你试图深入理解它并考虑 Spark 性能时是如此。** [这里有一个很好的参考](/adding-sequential-ids-to-a-spark-dataframe-fa0df5566ff6)来理解 Spark 中的自动递增。****

****[该任务有三种方法](https://stackoverflow.com/questions/48209667/using-monotonically-increasing-id-for-assigning-row-number-to-pyspark-datafram):****

*   ****使用 row_number()函数使用 SparkSQL****
*   ****使用 rdd 创建索引，然后使用 rdd.zipWithIndex()函数将其转换回数据框****
*   ****使用单调递增 id()****

******我更喜欢 rdd.zipWithIndex()函数:******

****步骤 1:从`songplays_table`数据框架中，使用 rdd 接口通过`zipWithIndex()`创建索引。结果是一个行列表，每行包含 2 个元素:(I)来自旧数据框的所有列被压缩成一个“行”，以及(ii)自动递增索引:****

****![](img/7f8191c531493a5ae1a1e4b8bce63909.png)****

****第二步:把它返回到 data frame——我们需要为它写一个 lambda 函数。****

****![](img/8a8cf38f2507a82095e00f2e43f9d914.png)****

## ****技巧 8—说到时间，加载和写入每个表需要多长时间？****

****下面是在 AWS EMR 集群上运行 Spark 应用程序、读取和写入 S3 的时间:****

****![](img/432535fc0549e310aa389070fe2b8020.png)****

****我的 EMR 集群有 1 个主节点和 2 个 m5.xlarge 类型的核心节点，如下所示:****

```
**aws emr create-cluster --name test-emr-cluster --use-default-roles --release-label emr-5.28.0 --instance-count 3 --instance-type m5.xlarge --applications Name=JupyterHub Name=Spark Name=Hadoop --ec2-attributes KeyName=emr-cluster  --log-uri s3://s3-for-emr-cluster/**
```

## ****技巧 9——如何加速 ETL 管道？****

****我们当然喜欢优化 Spark 应用程序，因为读写 S3 需要很长时间。以下是我尝试过的一些优化:****

******设置** `**spark.hadoop.mapreduce.fileoutputcommitter.algorithm.version**` **为 2******

****你可以在这里详细了解[。只需将`spark.conf.set("mapreduce.fileoutputcommitter.algorithm.version", "2")`添加到 spark 会话中即可。](https://kb.databricks.com/data/append-slow-with-spark-2.0.0.html)****

****通过这种优化，总 ETL 时间从大约 2.1 小时大幅减少到仅 30 分钟。****

******使用 HDFS 加速进程******

****- [“在每个节点上，HDFS 的读取吞吐量比 S3 高 6 倍”。](https://databricks.com/blog/2017/05/31/top-5-reasons-for-choosing-s3-over-hdfs.html)这样我们可以将分析表保存到 HDFS，然后从 HDFS 复制到 S3。我们可以使用 s3-dist-cp 从 HDFS 复制到 s3。****

## ****技巧 10——输出如何？S3 的分析表结果如何？****

****这个 ETL 管道是一个长期运行的工作，其中写`song`表的任务花费了大部分时间。歌曲表是按“年份”和“艺术家”进行分区的，这可能会产生扭曲的数据，因为与 200x 年相比，一些早期年份(1961 年到 199x 年)不包含很多歌曲。****

****![](img/22aedaf263a9cdf87d51ab9aee76b1da.png)****

****[数据质量检查](https://github.com/nhntran/create-datalake-spark/blob/master/test_data_lake.ipynb)以确保 ETL 管道是否成功地将所有记录添加到表中，以及[歌曲播放分析的一些示例查询和结果](https://github.com/nhntran/create-datalake-spark/blob/master/sparkifydb_data_lake_demo.ipynb)可以在 [Github](https://github.com/nhntran/create-datalake-spark) 上的我的笔记本中找到。****

## ****提示 11 —不要让 AWS 计费仪表板迷惑了你****

****虽然我“经常”使用 AWS，并且已经达到了该帐户的免费层使用限额，但每当我来到计费仪表板时，**应付总额**为 0。****

****![](img/d25622b1e4347124804d7e0d7d8dfa77.png)****

******不要让 AWS 计费仪表板迷惑了你。它显示的是总余额，而不是您的 AWS 费用。**是**余额**，根据[维基百科](https://en.wikipedia.org/wiki/Balance_(accounting)#:~:text=In%20banking%20and%20accounting%2C%20the,account%20during%20a%20financial%20period.)——是**“**在一个财务周期内记入账户的借方分录的**总和**与贷方分录的**总和**之间的差额。”****

****我想当我查看 AWS 账单面板时，我会看到我到目前为止已经花了多少钱，我的 AWS 费用。但是没有。即使当点击账单详情时，一切都是 0。所以我想我没怎么用 AWS。我的促销信用仍然是安全的。****

****![](img/63960bd0f7ddb1aeb083031b3cbb920a.png)****

******直到有一天，我点击了** `**Expand All**` **按钮，我惊讶地意识到我的推广信用几乎没有了！！！**所以，你在仪表盘上看到的是余额，而不是费用。使用 EMR 和 EC 集群时要小心。它可能比你想象的要花更多的钱。(嗯，虽然我承认获得 AWS 经验是如此的值得)。****

****![](img/e93771a20af6b90dc3e823f58fc197ab.png)****

****非常感谢您阅读这篇冗长的帖子。我知道人们很容易因为长篇大论而气馁，但是我想给你一份综合报告。祝你的项目好运，我非常乐意参与任何讨论。****

*******这篇文章的 Jupyter 笔记本版本，以及 Spark 上的其他教程和更多数据科学教程可以在***[***my Github***](https://github.com/nhntran)***上找到。*******