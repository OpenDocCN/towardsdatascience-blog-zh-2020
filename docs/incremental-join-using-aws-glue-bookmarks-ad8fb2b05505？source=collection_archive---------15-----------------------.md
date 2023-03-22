# 使用 AWS 粘合书签的增量连接

> 原文：<https://towardsdatascience.com/incremental-join-using-aws-glue-bookmarks-ad8fb2b05505?source=collection_archive---------15----------------------->

## [实践教程](https://towardsdatascience.com/tagged/hands-on-tutorials)

# 问题是

最近，我遇到了一个挑战，即在时间戳上将两个时间序列数据集连接在一起，而不要求两个数据集中的相应数据同时到达。例如，一个数据集上个月某一天的数据可能在一周前到达 S3，而另一个数据集上个月该天的相应数据可能在昨天到达。这是一个增量连接问题。

![](img/ad46496dd1a749f3a38486dcc214b494.png)

由[沙恩·朗斯](https://unsplash.com/@shanerounce?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 潜在的解决方案

*   也许可以通过在查询之前不连接数据来解决这个问题，但是我想对连接进行预处理，以便可以在任何比例下直接查询连接的数据。
*   您可以在每次管道运行时重新处理整个数据集。对于每天都在不断变大的数据集来说，这不是一个。
*   也可以手动实现一个系统，在两个表都着陆之前，不处理来自任何一个表的数据。这实际上是重新实现 AWS Glue 已经提供的一个特性:[书签](https://docs.aws.amazon.com/glue/latest/dg/monitor-continuations.html)，我们将在下面利用它。

# 使用 AWS 粘合书签和谓词下推

AWS 粘合书签允许您仅处理自管道之前运行以来到达数据管道的新数据。在上述增量连接问题中，需要处理的相应数据可能已经到达并且已经在流水线的不同运行中被处理，这并没有完全解决问题，因为相应的数据将由书签馈送以在不同的作业中被处理，因此不会被连接。

我们提出的解决方案利用了 AWS Glue 的另一个特性，即使用[谓词下推](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-etl-partitions.html)加载表的子集的能力。我们在 Glue ETL 工作中完成了以下 ETL 步骤:

```
# setup Glue ETL environmentimport sysfrom awsglue.transforms import *from awsglue.utils import getResolvedOptionsfrom pyspark.context import SparkContextfrom awsglue.context import GlueContextfrom awsglue.job import Jobfrom pyspark.sql.functions import split, colfrom awsglue.dynamicframe import DynamicFrame## @params: [JOB_NAME]args = getResolvedOptions(sys.argv, ['JOB_NAME'])sc = SparkContext()glueContext = GlueContext(sc)spark = glueContext.spark_sessionjob = Job(glueContext)job.init(args['JOB_NAME'], args)
```

使用 AWS Glue 目录中的 Glue 书签加载自上次管道运行以来到达的新数据。

```
table1_new = glueContext.create_dynamic_frame.from_catalog(database="db", table_name="table1", transformation_ctx='table1_new')table2_new = glueContext.create_dynamic_frame.from_catalog(database="db", table_name="table1", transformation_ctx='table2_new')
```

在新数据中找到受影响的分区。因为这是时间序列数据，所以按日期时间进行分区是有意义的。将这些分区写入可以在整个数据集上查询的下推谓词。

已经构建了谓词字符串，该字符串列出了在一个或两个表中有新数据的每个分区，现在我们可以从源数据表中加载“解锁”该数据。

```
we use the predicate string we previously built and load the table without using bookmarkstable1_unlock = glueContext.create_dynamic_frame.from_catalog(database="db", table_name="table1", push_down_predicate=table1_predicate)table2_unlock = glueContext.create_dynamic_frame.from_catalog(database="db", table_name="table2", push_down_predicate=table2_predicate)
```

现在，我们可以使用这两个表运行我们想要的任何连接转换。

然后，我们可以将这些表写入数据库，在我们的例子中是 S3。根据您正在进行的连接转换的类型，我们发现最好在“覆盖”模式下使用 Spark API 编写器，而不是 Glue DynamicFrame 编写器，因为我们希望删除以前运行时写入分区的任何旧数据，并只写入新处理的数据。

```
# set the overwrite mode to dynamicspark.conf.set("spark.sql.sources.partitionOverwriteMode","dynamic")final_df.write.partitionBy(["partition1", "partition2"]).saveAsTable("db.output", format='parquet', mode='overwrite', path='s3://your-s3-path')
```

注意，写入 Glue 目录的 PySpark API 模式似乎偶尔会导致表在被写入时变得不可用。

# 结论

将 AWS 粘合书签与谓词下推结合使用，可以在 ETL 管道中实现数据的增量连接，而无需每次都重新处理所有数据。选择一个好的分区模式可以确保您的增量连接作业处理接近所需的最小数据量。

*原载于*[*https://data munch . tech*](https://datamunch.tech/content/posts/incremental-join/)*。*