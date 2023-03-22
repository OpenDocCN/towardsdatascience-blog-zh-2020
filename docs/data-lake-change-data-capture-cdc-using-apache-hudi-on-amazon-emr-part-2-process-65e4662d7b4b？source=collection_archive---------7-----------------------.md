# 在 Amazon EMR 上使用 Apache 胡迪进行数据湖变更数据捕获(CDC)——第 2 部分——流程

> 原文：<https://towardsdatascience.com/data-lake-change-data-capture-cdc-using-apache-hudi-on-amazon-emr-part-2-process-65e4662d7b4b?source=collection_archive---------7----------------------->

## 使用 Amazon EMR 上的 Apache 胡迪轻松处理从数据库到数据湖的数据变化

![](img/c5e06ca98371ae11038c31e5590deabf.png)

图片由来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2654130) 的 [Gino Crescoli](https://pixabay.com/users/absolutvision-6158753/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2654130) 拍摄

在下面的前一篇文章中，我们讨论了如何使用亚马逊数据库迁移服务(DMS)无缝地**收集** CDC 数据。

[https://towards data science . com/data-lake-change-data-capture-CDC-using-Amazon-database-migration-service-part-1-capture-b43c 3422 aad 4](/data-lake-change-data-capture-cdc-using-amazon-database-migration-service-part-1-capture-b43c3422aad4)

下一篇文章将展示如何**处理** CDC 数据，以便在数据湖中实现数据库的接近实时的表示。我们将使用 Apache 胡迪和 Amazon EMR 的联合力量来执行此操作。Apache 胡迪是一个开源数据管理框架，用于简化近乎实时的增量数据处理。

我们将通过创建一个新的 EMR 集群来启动该流程

```
$ aws emr create-cluster --auto-scaling-role EMR_AutoScaling_DefaultRole --applications Name=Spark Name=Hive --ebs-root-volume-size 10 --ec2-attributes '{"KeyName":"roopikadf","InstanceProfile":"EMR_EC2_DefaultRole","SubnetId":"subnet-097e5d6e","EmrManagedSlaveSecurityGroup":"sg-088d03d676ac73013","EmrManagedMasterSecurityGroup":"sg-062368f478fb07c11"}' --service-role EMR_DefaultRole --release-label emr-6.0.0 --name 'Training' --instance-groups '[{"InstanceCount":3,"EbsConfiguration":{"EbsBlockDeviceConfigs":[{"VolumeSpecification":{"SizeInGB":32,"VolumeType":"gp2"},"VolumesPerInstance":2}]},"InstanceGroupType":"CORE","InstanceType":"m5.xlarge","Name":"Core - 2"},{"InstanceCount":1,"EbsConfiguration":{"EbsBlockDeviceConfigs":[{"VolumeSpecification":{"SizeInGB":32,"VolumeType":"gp2"},"VolumesPerInstance":2}]},"InstanceGroupType":"MASTER","InstanceType":"m5.xlarge","Name":"Master - 1"}]' --scale-down-behavior TERMINATE_AT_TASK_COMPLETION --region us-east-1 --bootstrap-actions Path=s3://aws-analytics-course/job/energy/emr.sh,Name=InstallPythonLibs
```

创建 EMR 集群后，使用 SSH 登录到主节点，并发出以下命令。这些命令将把 Apache 胡迪 JAR 文件复制到 S3。

```
$ aws s3 cp /usr/lib/hudi/hudi-spark-bundle.jar s3://aws-analytics-course/hudi/jar/   upload: ../../usr/lib/hudi/hudi-spark-bundle.jar to s3://aws-analytics-course/hudi/jar/hudi-spark-bundle.jar$ aws s3 cp /usr/lib/spark/external/lib/spark-avro.jar s3://aws-analytics-course/hudi/jar/
upload: ../../usr/lib/spark/external/lib/spark-avro.jar to s3://aws-analytics-course/hudi/jar/spark-avro.jar$ aws s3 ls s3://aws-analytics-course/hudi/jar/
2020-10-21 17:00:41   23214176 hudi-spark-bundle.jar
2020-10-21 17:00:56     101212 spark-avro.jar
```

现在创建一个新的 EMR 笔记本并上传到以下位置。上传**胡迪/hudi.ipynb**

```
$ git clone https://github.com/mkukreja1/blogs.git
```

使用在上一步中上传到 S3 的胡迪 JAR 文件创建一个 Spark 会话。

```
from pyspark.sql import SparkSession
import pyspark
from pyspark.sql.types import StructType, StructField, IntegerType, StringType, array, ArrayType, DateType, DecimalType
from pyspark.sql.functions import *
from pyspark.sql.functions import concat, lit, colspark = pyspark.sql.SparkSession.builder.appName("Product_Price_Tracking") \
     **.config("spark.jars", "s3://aws-analytics-course/hudi/jar/hudi-spark-bundle.jar,s3://aws-analytics-course/hudi/jar/spark-avro.jar")** \
     .config("spark.serializer", "org.apache.spark.serializer.KryoSerializer") \
     .config("spark.sql.hive.convertMetastoreParquet", "false") \
     .getOrCreate()
```

让我们读取疾病控制中心的文件。我们将从读取**完整的**加载文件开始。

```
TABLE_NAME = "coal_prod"
S3_RAW_DATA = "s3://aws-analytics-course/raw/dms/fossil/coal_prod/LOAD00000001.csv"
S3_HUDI_DATA = "s3://aws-analytics-course/hudi/data/coal_prod"coal_prod_schema = StructType([StructField("Mode", StringType()),
                               StructField("Entity", StringType()),
                               StructField("Code", StringType()),
                               StructField("Year", IntegerType()),
                               StructField("Production", DecimalType(10,2)),
                               StructField("Consumption", DecimalType(10,2))
                               ])
df_coal_prod = spark.read.csv(S3_RAW_DATA, header=False, schema=coal_prod_schema)df_coal_prod.show(5)+----+-----------+----+----+----------+-----------+
|Mode|     Entity|Code|Year|Production|Consumption|
+----+-----------+----+----+----------+-----------+
|   I|Afghanistan| AFG|1949|      0.04|       0.00|
|   I|Afghanistan| AFG|1950|      0.11|       0.00|
|   I|Afghanistan| AFG|1951|      0.12|       0.00|
|   I|Afghanistan| AFG|1952|      0.14|       0.00|
|   I|Afghanistan| AFG|1953|      0.13|       0.00|
+----+-----------+----+----+----------+-----------+
only showing top 5 rows
```

Apache 胡迪需要一个主键来唯一标识每个记录。通常，顺序生成的主键最适合此目的。然而我们的桌子没有。为了解决这个问题，让我们通过使用实体和年份列的组合来生成一个 PK。下面的**键**列将被用作主键。

```
df_coal_prod=df_coal_prod.select("*", concat(col("Entity"),lit(""),col("Year")).alias("key"))
df_coal_prod_f=df_coal_prod.drop(df_coal_prod.Mode)
df_coal_prod_f.show(5)+-----------+----+----+----------+-----------+---------------+
|     Entity|Code|Year|Production|Consumption|            **key**|
+-----------+----+----+----------+-----------+---------------+
|Afghanistan| AFG|1949|      0.04|       0.00|Afghanistan1949|
|Afghanistan| AFG|1950|      0.11|       0.00|Afghanistan1950|
|Afghanistan| AFG|1951|      0.12|       0.00|Afghanistan1951|
|Afghanistan| AFG|1952|      0.14|       0.00|Afghanistan1952|
|Afghanistan| AFG|1953|      0.13|       0.00|Afghanistan1953|
+-----------+----+----+----------+-----------+---------------+
only showing top 5 rows
```

我们现在准备以胡迪格式保存数据。由于这是我们第一次保存该表，我们将使用" **bulk_insert** "操作和**模式=覆盖**。还要注意，我们使用“**键**列作为**记录键**。

```
df_coal_prod_f.write.format("org.apache.hudi") \
            .option("hoodie.table.name", TABLE_NAME) \
            .option("hoodie.datasource.write.storage.type", "COPY_ON_WRITE") \
            **.option("hoodie.datasource.write.operation", "bulk_insert") \
            .option("hoodie.datasource.write.recordkey.field","key")** \
            .option("hoodie.datasource.write.precombine.field", "key") \
            .mode("**overwrite**") \
            .save(S3_HUDI_DATA)
```

我们现在可以读取新创建的胡迪表。

```
df_final = spark.read.format("org.apache.hudi")\
          .load("s3://aws-analytics-course/hudi/data/coal_prod/default/*.parquet")
df_final.registerTempTable("coal_prod")
spark.sql("select count(*) from coal_prod").show(5)
spark.sql("select * from coal_prod where key='India2013'").show(5)+--------+
|count(1)|
+--------+
|    **6282**|
+--------+

+-------------------+--------------------+------------------+----------------------+--------------------+------+----+----+----------+-----------+---------+
|_hoodie_commit_time|_hoodie_commit_seqno|_hoodie_record_key|_hoodie_partition_path|   _hoodie_file_name|Entity|Code|Year|Production|Consumption|      key|
+-------------------+--------------------+------------------+----------------------+--------------------+------+----+----+----------+-----------+---------+
|     20201021215857|20201021215857_54...|         India2013|               default|8fae00ae-34e7-45e...| India| IND|2013|   **2841.01**|       **0.00**|India2013|
+-------------------+--------------------+------------------+----------------------+--------------------+------+----+----+----------+-----------+---------+
```

请注意，我们有来自满载的 **6282** 行和 2013 年关键**印度的数据。**该密钥将在下一次操作中更新，因此记录历史非常重要。我们现在将读取增量数据。

增量数据带有 **4** 行，插入 **2** 行，更新**一**行，删除和**一**行。我们将首先处理插入和更新的行。注意下面的**(“模式输入(' U '，' I ')”)**的过滤器。

```
S3_INCR_RAW_DATA = "s3://aws-analytics-course/raw/dms/fossil/coal_prod/20200808-*.csv"
df_coal_prod_incr = spark.read.csv(S3_INCR_RAW_DATA, header=False, schema=coal_prod_schema)
**df_coal_prod_incr_u_i=df_coal_prod_incr.filter("Mode IN ('U', 'I')")**
df_coal_prod_incr_u_i=df_coal_prod_incr_u_i.select("*", concat(col("Entity"),lit(""),col("Year")).alias("key"))
df_coal_prod_incr_u_i.show(5)df_coal_prod_incr_u_i_f=df_coal_prod_incr_u_i.drop(df_coal_prod_incr_u_i.Mode)
df_coal_prod_incr_u_i_f.show()+----+------+----+----+----------+-----------+---------+
|Mode|Entity|Code|Year|Production|Consumption|      key|
+----+------+----+----+----------+-----------+---------+
|   I| India| IND|2015|   4056.33|       0.00|India2015|
|   I| India| IND|2016|   4890.45|       0.00|India2016|
|   U| India| IND|2013|   2845.66|     145.66|India2013|
+----+------+----+----+----------+-----------+---------+

+------+----+----+----------+-----------+---------+
|Entity|Code|Year|Production|Consumption|      key|
+------+----+----+----------+-----------+---------+
| India| IND|2015|   4056.33|       0.00|India2015|
| India| IND|2016|   4890.45|       0.00|India2016|
| India| IND|2013|   2845.66|     145.66|India2013|
+------+----+----+----------+-----------+---------+
```

我们现在准备对增量数据执行胡迪**上插**操作。由于这个表已经存在，这次我们将使用**追加**选项。

```
df_coal_prod_incr_u_i_f.write.format("org.apache.hudi") \
            .option("hoodie.table.name", TABLE_NAME) \
            .option("hoodie.datasource.write.storage.type", "COPY_ON_WRITE") \
            **.option("hoodie.datasource.write.operation", "upsert")** \
            .option("hoodie.upsert.shuffle.parallelism", 20) \
            .option("hoodie.datasource.write.recordkey.field","key") \
            .option("hoodie.datasource.write.precombine.field", "key") \
            .mode("**append**") \
            .save(S3_HUDI_DATA)
```

检查基础数据。请注意，已经添加了 2 个新行，因此表计数已经从 6282 增加到 6284。另请注意，2013 年印度的关键**行现已更新为生产&消耗列。**

```
df_final = spark.read.format("org.apache.hudi")\
          .load("s3://aws-analytics-course/hudi/data/coal_prod/default/*.parquet")
df_final.registerTempTable("coal_prod")
spark.sql("select count(*) from coal_prod").show(5)
spark.sql("select * from coal_prod where key='India2013'").show(5)+--------+
|count(1)|
+--------+
|    **6284**|
+--------+

+-------------------+--------------------+------------------+----------------------+--------------------+------+----+----+----------+-----------+---------+
|_hoodie_commit_time|_hoodie_commit_seqno|_hoodie_record_key|_hoodie_partition_path|   _hoodie_file_name|Entity|Code|Year|**Production**|**Consumption**|      key|
+-------------------+--------------------+------------------+----------------------+--------------------+------+----+----+----------+-----------+---------+
|     20201021220359|20201021220359_0_...|         India2013|               default|8fae00ae-34e7-45e...| India| IND|2013|   **2845.66**|     **145.66**|India2013|
+-------------------+--------------------+------------------+----------------------+--------------------+------+----+----+----------+-----------+---------+
```

现在我们要处理删除了行的**行。**

```
df_coal_prod_incr_d=df_coal_prod_incr.filter**("Mode IN ('D')")**
df_coal_prod_incr_d=df_coal_prod_incr_d.select("*", concat(col("Entity"),lit(""),col("Year")).alias("key"))
df_coal_prod_incr_d_f=df_coal_prod_incr_d.drop(df_coal_prod_incr_u_i.Mode)
df_coal_prod_incr_d_f.show()+------+----+----+----------+-----------+---------+
|Entity|Code|Year|Production|Consumption|      key|
+------+----+----+----------+-----------+---------+
| India| IND|2010|   2710.54|       0.00|India2010|
+------+----+----+----------+-----------+---------+
```

我们可以通过胡迪 **Upsert** 操作来实现这一点，但是需要使用额外的选项来删除**hoodie . data source . write . payload . class = org . Apache . hudi . emptyhoodierecordpayload**

```
df_coal_prod_incr_d_f.write.format("org.apache.hudi") \
            .option("hoodie.table.name", TABLE_NAME) \
            .option("hoodie.datasource.write.storage.type", "COPY_ON_WRITE") \
            .option("hoodie.datasource.write.operation", "upsert") \
            .option("hoodie.upsert.shuffle.parallelism", 20) \
            .option("hoodie.datasource.write.recordkey.field","key") \
            .option("hoodie.datasource.write.precombine.field", "key") \
           ** .option("hoodie.datasource.write.payload.class", "org.apache.hudi.EmptyHoodieRecordPayload") \**
            .mode("append") \
            .save(S3_HUDI_DATA)
```

我们现在可以检查结果。由于删除了一行，计数从 6284 下降到 6283。此外，对已删除行的查询返回空值。一切都按预期进行。

```
df_final = spark.read.format("org.apache.hudi")\
          .load("s3://aws-analytics-course/hudi/data/coal_prod/default/*.parquet")
df_final.registerTempTable("coal_prod")
spark.sql("select count(*) from coal_prod").show(5)
spark.sql("select * from coal_prod where key='India2010'").show(5)+--------+
|count(1)|
+--------+
|    **6283**|
+--------+

+-------------------+--------------------+------------------+----------------------+-----------------+------+----+----+----------+-----------+---+
|_hoodie_commit_time|_hoodie_commit_seqno|_hoodie_record_key|_hoodie_partition_path|_hoodie_file_name|Entity|Code|Year|Production|Consumption|key|
+-------------------+--------------------+------------------+----------------------+-----------------+------+----+----+----------+-----------+---+
+-------------------+--------------------+------------------+----------------------+-----------------+------+----+----+----------+-----------+---+
```

本文中使用的所有代码都可以在下面的链接中找到:

[](https://github.com/mkukreja1/blogs/tree/master/dms) [## mkukreja 1/博客

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/mkukreja1/blogs/tree/master/dms) 

我希望这篇文章是有帮助的。 **CDC 使用亚马逊数据库迁移服务**是由 [Datafence Cloud Academy](http://www.datafence.com) 提供的 AWS 大数据分析课程的一部分。课程是周末自己在网上教的。