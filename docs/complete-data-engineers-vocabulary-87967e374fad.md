# 完整的数据工程师词汇

> 原文：<https://towardsdatascience.com/complete-data-engineers-vocabulary-87967e374fad?source=collection_archive---------10----------------------->

![](img/376c6b2223d00d7da978dda7bb6dba8a.png)

阿里安·达尔维什在 [Unsplash](https://unsplash.com/s/photos/programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

## 数据工程

## 数据工程师必须知道的 10 个单词以内的概念

*更新—建议由* [*乔治*](https://medium.com/u/3acf147e0e98?source=post_page-----87967e374fad--------------------------------)*[*阿克塞尔【弗尔兰】*](https://medium.com/u/4b2ad5f659bd?source=post_page-----87967e374fad--------------------------------) *和* [*妮莎*](https://medium.com/u/a28d7a368f6d?source=post_page-----87967e374fad--------------------------------) *。最近更新—2020 年 7 月 15 日**

*[这些年来，我使用了许多这样的技术](https://linktr.ee/kovid)处理数据库、数据仓库、构建数据管道、使用 ETL 框架和处理大数据——甚至处理电子表格。所有这些技术都需要数周的学习和数月的实践来完善。*

*我认为用 10 个字来总结或介绍一项技术会非常有趣。我在这里列出了大约 150 个概念和技术，以及不超过 10 个字的解释。显然，这个列表受限于我的领域知识，但我认为如果你想在 2020 年成为一名数据工程师，这是一个很好的列表。*

*几乎所有的链接都指向该技术的主页或者一篇有趣的博客文章。使用名称中嵌入的链接。开始了。*

*A a*

*[](https://mariadb.com/resources/blog/acid-compliance-what-it-means-and-why-you-should-care/)**—[交易](https://vladmihalcea.com/a-beginners-guide-to-acid-and-database-transactions/)强制执行数据库属性；[原子性](https://en.wikipedia.org/wiki/Atomicity_(database_systems))、[一致性](https://en.wikipedia.org/wiki/Consistency_(database_systems))、[隔离性](https://dev.mysql.com/doc/refman/8.0/en/innodb-transaction-isolation-levels.html)、[耐久性](https://en.wikipedia.org/wiki/Durability_(database_systems))***

***[**原子性**](https://en.wikipedia.org/wiki/Atomicity_(database_systems))——如果(多步任务中的)一步失败，则整个事务失败***

***[**Avro**](https://avro.apache.org/docs/1.9.2/) —紧凑、面向行、基于 JSON 的数据格式***

***[**阿兹卡班**](https://azkaban.github.io) — LinkedIn 的 Hadoop 作业批处理调度器***

***[**气流**](https://airflow.apache.org)——Airbnb 基于 DAG 的可编程作业调度器。非常受欢迎的 Apache 项目***

***[**解析** **函数**](https://docs.oracle.com/cd/E11882_01/server.112/e41084/functions004.htm) —对一组行进行运算的聚合函数***

***B b***

***[**大数据**](https://en.wikipedia.org/wiki/Big_data) —大到传统系统无法处理的程度***

***[**BI**](https://www.tableau.com/learn/articles/business-intelligence) —从数据中获取最佳信息的技术和流程***

***[](https://en.wikipedia.org/wiki/Batch_processing)****—一次完成多项任务，即加载 100M 条记录*******

*******[**BigQuery**](https://cloud.google.com/bigquery) —谷歌的无服务器数据仓库，与 Redshift 和 Azure DW 竞争*******

*****[**BigTable**](https://cloud.google.com/bigtable) —由 Google 提供的千兆级 NoSQL 数据库*****

*****C c*****

*****[**Cassandra**](http://cassandra.apache.org) —分布式 NoSQL 数据库因其列存储功能而广受欢迎*****

*****[**CTE**](https://mariadb.com/kb/en/common-table-expressions/) —可以在 SQL 中通过名称引用的结果集缓存*****

*****[**缓存**](https://aws.amazon.com/caching/database-caching/) —暂时存储数据以备将来再次使用*****

*****[**面向列的数据库**](https://en.wikipedia.org/wiki/Column-oriented_DBMS) —在磁盘上连续存储列值的存储器*****

*****[**云作曲**](https://cloud.google.com/composer) —谷歌的气流实现*****

*****[**立方体**](https://www.holistics.io/blog/the-rise-and-fall-of-the-olap-cube/) —多维数据；数据仓库中使用的术语*****

*****[**目录**](https://www.talend.com/resources/what-is-data-catalog/) —具有管理和搜索能力的元数据的组织*****

*****[**云功能**](https://cloud.google.com/functions) —谷歌的无服务器计算选项，如 AWS Lambda*****

*****D d*****

*****[**DynamoDB**](https://aws.amazon.com/dynamodb/)—Pb 级键值，AWS 文档数据库*****

*****[**德鲁伊**](https://druid.apache.org) —分布式柱状数据存储*****

*****[**Drill**](http://drill.apache.org) —用于 Hadoop、NoSQL 数据库中非关系、JSON、嵌套数据的 SQL*****

*****[](https://research.google/pubs/pub36632/)**—谷歌大规模专有互动查询引擎*******

*******[**分布式处理**](https://www.sciencedirect.com/topics/computer-science/distributed-processing) —在不同的计算单元上拆分任务和处理，以获得更快的结果*******

*****[**DataFrame**](https://pandas.pydata.org/pandas-docs/stable/getting_started/dsintro.html) —关系数据库表，类似编程语言中的构造*****

*****[**DW**](https://www.oracle.com/au/database/what-is-a-data-warehouse/) —用于业务报告和分析的真实数据存储的单一来源*****

*****[**DAG**](https://cran.r-project.org/web/packages/ggdag/vignettes/intro-to-dags.html) —没有循环依赖关系的依赖关系图—在编排引擎中使用*****

*****[**数据流**](https://cloud.google.com/dataflow) —谷歌托管数据流水线解决方案*****

*****[**data prep**](https://cloud.google.com/dataprep)—Google 的数据准备(清理、扯皮等。)解决方案*****

*****[**Dataproc**](https://cloud.google.com/dataproc) —谷歌完全托管的 Spark/Hadoop 产品*****

*******仪表板** —用于跟踪绩效、KPI 等的可视化设置。*****

*****[**数据字典**](https://www.sciencedirect.com/topics/computer-science/data-dictionary) —揭示数据源结构和用途的深度元数据*****

*****[**数据停机**](/the-rise-of-data-downtime-841650cedfd5) —数据不完整、错误、丢失或不准确的时间*****

*****[**数据集市**](https://panoply.io/data-warehouse-guide/data-mart-vs-data-warehouse/) —数据仓库的子集，通常用于特定的业务功能*****

*****[**维度**](https://en.wikipedia.org/wiki/Dimension_(data_warehouse)) —数据的描述符或分类器*****

*****[](https://www.oreilly.com/library/view/database-reliability-engineering/9781491925935/ch01.html)**—SRE 关于数据库可靠性的一个分支*******

*******[**数据保管人**](https://en.wikipedia.org/wiki/Data_custodian) —万物数据的拥有者*******

*****[**DataVault**](https://www.talend.com/blog/2015/03/27/what-is-the-data-vault-and-why-do-we-need-it/) —数据仓库设计方法论*****

*****[**DBT**](https://www.getdbt.com) —基于 SQL 的数据流水线和工作流解决方案*****

*****[**Docker**](https://www.docker.com) —OS 级虚拟化成型容器*****

*****E e*****

*****[**elastic search**](https://www.elastic.co/enterprise-search)—事实上的全文搜索引擎*****

*****[**EMR**](https://aws.amazon.com/emr/)—AWS 上的 MapReduce*****

*****[**丰富**](https://www.sciencedirect.com/topics/computer-science/data-enrichment) —用更多数据丰富数据的过程*****

*****[](https://blog.panoply.io/etl-vs-elt-the-difference-is-in-the-how)**—从源提取，转换，加载到目的地*******

*******[**ELT**](https://blog.panoply.io/etl-vs-elt-the-difference-is-in-the-how) —从源提取，加载到目的地并转换*******

*****[**ER 图**](https://en.wikipedia.org/wiki/Entity%E2%80%93relationship_model) —可视化数据库实体间关系的图*****

*****[**麋鹿**](https://www.elastic.co/what-is/elk-stack) —事实上的开源应用日志处理&监控解决方案*****

*****F f*****

*****[**水槽**](https://flume.apache.org) —面向事件流数据的大规模数据流水线*****

*****[](https://flink.apache.org/flink-architecture.html)**—数据流分布式处理引擎*******

*******[**平面文件**](https://en.wikipedia.org/wiki/Flat-file_database)——通常是文本或二进制文件*******

*******事实** —业务流程的度量，例如总销售额*****

*****[**故障转移**](https://en.wikipedia.org/wiki/Failover) —从故障机器转移到正常工作的机器*****

*****G g*****

*****[**胶水**](https://aws.amazon.com/glue/)—AWS 的大规模无服务器 ETL、数据流水线解决方案*****

*****[**金录**](https://blogs.informatica.com/2015/05/08/golden-record/) —单一来源的真相*****

*****H h*****

*****[**Hadoop**](https://hadoop.apache.org) —由 MapReduce、YARN 和 HDFS 组成的大数据处理框架*****

*****[**Hive**](https://hive.apache.org) —类似 SQL 的查询引擎，用于访问存储在 Hadoop 生态系统上的数据*****

*****[**HBase**](https://hbase.apache.org) —运行在 Hadoop 之上的非关系型分布式数据库*****

*****[**HDFS**](https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html) — Hadoop 的分布式文件系统*****

*****我我*****

*****[**influx db**](https://www.influxdata.com)—非常流行的时间序列数据库*****

*****[**摄取**](https://www.alooma.com/blog/what-is-data-ingestion) —将数据插入系统，即数据摄取*****

*****[**集成**](https://www.talend.com/resources/what-is-data-integration/) —汇集多个数据源*****

*****[](https://www.gigaspaces.com/blog/in-memory-computing/)**—内存存储，内存计算，即不在磁盘上*******

*******J j*******

*******[**JSON**](https://www.json.org/json-en.html)—互联网上事实上的数据传输格式*******

*****K K*****

*****[**卡夫卡**](https://kafka.apache.org) — LinkedIn 的分布式流媒体框架*****

*****[**Kinesis**](https://aws.amazon.com/kinesis/) — AWS 的托管卡夫卡式流媒体服务*****

*****[](https://www.elastic.co/kibana)**—ETK 堆栈中的监控&可视化解决方案*******

*******[**键值存储**](https://en.wikipedia.org/wiki/Key-value_database) —数据存储在类似字典或散列表的结构中*******

*****[**Kubernetes**](https://kubernetes.io)——谷歌的容器编排服务，爱称 K8s*****

*****L L*****

*****[**Looker**](https://looker.com) —谷歌最新基于浏览器的商务智能工具收购*****

*****[**Luigi**](https://github.com/spotify/luigi)—Spotify 的作业编排引擎*****

*****[](https://azure.microsoft.com/en-au/solutions/data-lake/)**—原始存储的所有业务数据*******

*******[**λ**](https://aws.amazon.com/lambda/)——AWS 的 FaaS 发售——非常受欢迎*******

*****[**Logstash**](https://www.elastic.co/logstash) —记录 ELK 堆栈中的分析解决方案*****

*****[**谱系**](https://www.sciencedirect.com/topics/computer-science/data-lineage)——从原始到加工、再加工到最终的旅程*****

*****米米*****

*****[**MySQL**](https://www.mysql.com)——非常流行的开源关系数据库*****

*****[**【MongoDB**](https://www.mongodb.com/cloud/atlas)——非常流行的开源 NoSQL 数据库*****

*****[**MariaDB**](https://mariadb.com) —原 MySQL 团队维护的 MySQL 的 fork*****

*****[**MapReduce**](https://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html) —分布式计算的编程模型；Hadoop 的基石*****

*****[**MLlib**](https://spark.apache.org/mllib/) — Spark 的机器学习库*****

*****[**元数据库**](https://www.metabase.com) —流行的开源数据可视化解决方案*****

*******MDM** —对关键业务数据进行单点管理*****

*****[**MDX**](https://docs.microsoft.com/en-us/analysis-services/multidimensional-models/mdx/mdx-query-the-basic-query?view=asallproducts-allversions)—MS SQL Server 对 OLAP 查询的特性*****

*******测量** —参见**事实*******

*****[**物化视图**](https://www.postgresql.org/docs/9.3/rules-materializedviews.html) —查询结果集作为数据库对象持久存储在磁盘上*****

*****[**MPP**](https://databases.looker.com/analytical) —大量处理器并行协同计算*****

*******元数据** —参见**目录*******

*****N n*****

*****[**NoSQL**](https://www.mongodb.com/nosql-explained)——一组数据库技术，不仅仅是关系数据库*****

*****[**Neo4j**](https://neo4j.com) —事实上的面向图形的数据库*****

*****[**Nomad**](https://www.nomadproject.io) —哈希公司的调度解决方案*****

*****[**接近实时**](https://blog.syncsort.com/2015/11/big-data/the-difference-between-real-time-near-real-time-and-batch-processing-in-big-data/)——几乎实时，仅因物理限制而有延迟*****

*****[**规范化**](https://en.wikipedia.org/wiki/Database_normalization) —关系数据库中的组织概念*****

*****O o*****

*****[**ORC**](https://orc.apache.org) —高性能开源柱状存储格式*****

*****[**Oozie**](https://oozie.apache.org) —基于 DAG 的 Hadoop 作业工作流调度程序*****

*****[**OLAP**](https://en.wikipedia.org/wiki/Online_analytical_processing) —没有事务的分析处理，通常用于数据仓库*****

*****[**OLTP**](https://en.wikipedia.org/wiki/Online_transaction_processing) —事务处理，具有 ACID 属性*****

*****[**ODS**](https://en.wikipedia.org/wiki/Operational_data_store) —运营数据存储，目的类似于 DW*****

*****P p*****

*****[**PostgreSQL**](https://www.postgresql.org) —程序员最爱的开源数据库*****

*****[**PostGIS**](https://postgis.net)—PostgreSQL 中地理空间数据支持的扩展*****

*****[**Percona**](https://www.percona.com/blog/) —提供 MySQL、MongoDB 等的分支。具有高级功能*****

*****[](https://parquet.apache.org)**—事实上的开源柱状存储格式*******

*******[**Protobuf**](https://developers.google.com/protocol-buffers) —*******

*****[**py Spark**](https://spark.apache.org/docs/latest/api/python/index.html)—Spark 上的 Python 包装器*****

*****[**Presto**](https://prestodb.io) —脸书开源分布式查询引擎*****

*****[**Plotly**](https://plotly.com) —数据科学家& ML 工程师事实上的可视化库*****

*****[**流水线**](https://www.xplenty.com/blog/what-is-a-data-pipeline/) —转换&传输数据的一系列过程&计算*****

*****[**分区**](https://docs.oracle.com/cd/B28359_01/server.111/b32024/partition.htm) —将一张表拆分成多个部分，以查询更少的数据*****

*****[**熊猫**](https://pandas.pydata.org)—Python 中事实上的数据处理库*****

*****[**发布/订阅**](https://cloud.google.com/pubsub/docs/overview) —发布、订阅事件流处理的模型*****

*****[**Python**](https://www.python.org) —数据工程、数据科学的事实语言*****

*****问*****

*****[**查询引擎**](https://www.alluxio.io/learn/presto/query/) —对数据集执行查询的软件*****

*****[**查询优化器**](https://logicalread.com/sql-server-query-optimizer-mc11/) —优化器&重写 SQL 查询的数据库组件*****

*****[**查询计划**](https://dataschool.com/sql-optimization/what-is-a-query-plan/) —查询引擎执行查询的执行计划*****

*****[**查询成本**](https://www.brentozar.com/archive/2018/12/never-judge-a-query-by-its-cost/) —运行一个查询在处理方面的成本*****

*****R*****

*****[**RDS**](https://aws.amazon.com/rds/)—AWS 的托管关系数据库服务*****

*****[**【RBAC】**](https://auth0.com/docs/authorization/concepts/rbac)—基于工作角色和某人权限的系统访问*****

*****[**RabbitMQ**](https://www.rabbitmq.com) —事实上的[消息代理](https://en.wikipedia.org/wiki/Message_broker)。*****

*****[**红移**](https://aws.amazon.com/redshift/) —最受欢迎的托管、Pb 级数据仓库解决方案*****

*****[**Redis**](https://redis.io) —面向应用的流行缓存解决方案*****

*****[**Redash**](https://redash.io) —用于基本报告和分析的出色可视化工具*****

*****[**复制**](https://en.wikipedia.org/wiki/Replication_(computing)) —制作数据库的冗余副本，用于故障转移、负载分配*****

*****[](https://spark.apache.org/docs/1.6.2/api/java/org/apache/spark/rdd/RDD.html)**—火花内主要加工构造。了解更多[在这里](https://spark.apache.org/docs/1.6.2/api/java/org/apache/spark/rdd/RDD.html)。*******

*******[**面向行的数据库**](https://dataschool.com/data-modeling-101/row-vs-column-oriented-databases/)——数据记录/行级数据连续存储在磁盘/内存中*******

*****S s*****

*****[**S3**](https://aws.amazon.com/s3/) —最受欢迎的云存储服务，由 AWS 提供*****

*******SQL** — [数据说的语言](/well-always-have-sequel-55325432174)*****

*****[**Sqoop**](https://sqoop.apache.org) —与 Hadoop 之间的批量数据传输*****

*****[**爽快**](https://github.com/google/snappy) —谷歌的压缩器/解压器*****

*****[**Spark**](https://spark.apache.org) —事实上的 SQL 支持的大数据处理引擎*****

*****[**风暴**](https://storm.apache.org) —开源、实时计算引擎*****

*****[**超集**](https://airbnb.io/projects/superset/) — Airbnb 基于网络的数据可视化& BI 工具，孵化于 Apache*****

*****[**分片**](https://www.digitalocean.com/community/tutorials/understanding-database-sharding) —在多台机器上拆分和存储一个数据库*****

*****[**存储引擎**](https://dev.mysql.com/doc/refman/8.0/en/storage-engines.html) —数据库的数据存储操作层*****

*****[**星型模式**](https://www.xplenty.com/blog/snowflake-schemas-vs-star-schemas-what-are-they-and-how-are-they-different/) —流行的数据仓库设计方法论。在这里阅读更多*****

*****[**雪花模式**](https://www.xplenty.com/blog/snowflake-schemas-vs-star-schemas-what-are-they-and-how-are-they-different/) —流行的数据仓库设计方法论。在这里阅读更多*****

*****[**SCD**](https://www.talend.com/resources/single-source-truth/) —随时间缓慢变化的数据类别值，如居住城市*****

*****[**单一真实来源**](https://www.talend.com/resources/single-source-truth/) —被认为反映业务真实情况的数据源*****

*****[**结构化数据**](https://developers.google.com/search/docs/guides/intro-structured-data) —遵循某种预定义模式或格式的数据*****

*****[**半结构化数据**](https://en.wikipedia.org/wiki/Semi-structured_data) —不遵循正式结构，但仍包含其他标记*****

*****[**SRE**](https://landing.google.com/sre/)——谷歌对可靠性工程师 DevOps 的重塑*****

*****T t*****

*****[**terra form**](https://www.terraform.io)——哈希公司事实上的基础设施即代码产品*****

*****[](https://thrift.apache.org)**—支持跨语言开发的框架*******

*******U u*******

*******[**非结构化数据**](https://en.wikipedia.org/wiki/Unstructured_data) —没有模式或预定义结构的数据*******

*****V v*****

*****[**Vertica**](https://www.vertica.com)——相当流行的数据仓储 so*****

*****[**VoltDB**](https://www.voltdb.com) —具有序列化事务的内存数据库*****

*****[**金库**](https://www.vaultproject.io) —哈希公司的秘密经理*****

*****[**视图**](https://docs.microsoft.com/en-us/sql/t-sql/statements/create-view-transact-sql?view=sql-server-ver15)—由 SQL 查询表示的未持久化的数据库对象*****

*****W w*****

*****[](https://www.springboard.com/blog/data-wrangling/)**—清理数据，为分析做好准备*******

*******[**窗口函数**](https://www.postgresql.org/docs/9.1/tutorial-window.html) —与当前行相关的多行 SQL 计算*******

*****X x*****

*****这里什么都没有。或许有。*****

*****Y y*****

*****[**YARN**](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html)—Hadoop 生态系统的资源管理器(用于 MapReduce 和 Spark)*****

*****Z z*****

*****[](https://zookeeper.apache.org)**—集中配置管理服务*******