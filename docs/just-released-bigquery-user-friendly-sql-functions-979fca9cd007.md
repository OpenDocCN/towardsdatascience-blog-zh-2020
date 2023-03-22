# 刚刚发布:BigQuery 用户友好的 SQL 函数

> 原文：<https://towardsdatascience.com/just-released-bigquery-user-friendly-sql-functions-979fca9cd007?source=collection_archive---------14----------------------->

## 从 BigQuery 截断表到动态 SQL 支持；我们涵盖了 Google Cloud 发布的 12 个用户友好的 BigQuery 函数。

![](img/eb9ad3974709f6339f2e1a229c5ccf6e.png)

授权给作者的图像

随着 **12 个新的 BigQuery SQL 特性**的发布，我们很兴奋地看到 [BigQuery](https://www.ancoris.com/google-bigquery) 的能力得到了进一步的提升。谷歌云将这些描述为“*用户友好的 SQL 功能*”。那么，让我们来看看现在有什么可能。

# 1.通过 DDL 添加表列

BigQuery 的新特性是能够通过 ALTER TABLE DDL 语句添加新列。这是具有传统本地数据库平台背景的数据专业人员所期望的标准，所以很高兴看到 Google Cloud 已经承认了这一点。

```
alter table mydataset.mytable
add column a string,
add column if not exists b geography,
add column c array<numeric>,
add column d date options(description="my description")
```

我们喜欢这样的语法:如果一个列还不存在，就只添加一个，这对于幂等部署来说很方便。也完全支持记录。从 [*BigQuery 文档中了解更多信息。*](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language#alter_table_add_column_statement)

# 2.截断表格

现在支持 TRUNCATE TABLE，这将满足那些来自本地后台的用户。与 BigQuery 中会产生扫描成本的 DELETE DML 语句不同，TRUNCATE 属于不断增长的 [BigQuery 自由操作](https://cloud.google.com/bigquery/pricing#free)列表。我们肯定会经常用到这个。

```
truncate table [project_name.] dataset_name.] table_name
```

*为了降低查询扫描成本，我们建议在需要删除表的全部内容时使用 TRUNCATE 而不是 delete。*

# 3.Unicode 表命名

BigQuery 现在支持 Unicode 表命名。由于支持多语言，表名不再仅限于字母、数字和下划线。

*小心表格命名，确保表格一致、易读，并遵循组织内的命名惯例。*

# 4.BigQuery 联邦表变得(甚至)更好了

在 Ancoris，我们喜欢 BigQuery 中的联邦(外部)表；它们作为一种强大的零数据工程方法，可以轻松地从 Google 云存储中摄取文件，包括常见的格式，如 JSON 和 CSV。

*很高兴看到您现在可以使用 DDL 语句创建外部表了。*

下面的示例创建了一个从两个不同的存储 URIs 读取 CSV 的表:

```
create external table dataset.CsvTable options(
format = 'CSV',
uris = ['gs://bucket/path1.csv', 'gs://bucket/path2.csv']);
```

要了解更多信息，请参见这些创建联邦表的例子。我们计划在接下来的几周内做一个技术指导来展示什么是可能的。

# 5.使用 SQL 将数据从 BigQuery 导出到 Google 云存储

这在发行说明中被掩盖了一点，但是在我们看来这是一个非常强大的特性。您现在可以使用新的 EXPORT DATA SQL 命令将 BigQuery 查询的结果导出到 Google 云存储中；支持所有 [Bigquery 支持的数据格式和压缩类型](https://cloud.google.com/bigquery/docs/exporting-data)。

这里有一个取自 [BigQuery 导出数据文档](https://cloud.google.com/bigquery/docs/reference/standard-sql/other-statements#export_data_statement)的例子。

```
export data options(
uri='gs://bucket/folder/*.csv',
format='CSV',
overwrite=true,
header=true,
field_delimiter=';') as
select field1, field2
from table1
order by field1 limit 10
```

*很高兴看到没有外出费用；您只需为任何扫描的 BigQuery 数据和存储在 GCS 中的数据付费。*

这似乎加强了谷歌云减少复杂数据工程需求的愿望，这是我们(和我们的许多客户)所共有的。如果将它与从目标 GCS bucket 触发的云函数结合起来，就可以提供一种简单的机制，将数据传递到 BigQuery 之外的数据管道中。

# 6.立即执行(动态 SQL)

BigQuery 现在支持动态 SQL 的执行。语法非常熟悉，特别是对于那些有 MS SQL 背景的人来说。

```
-- create a temporary table called Books.
execute immediate
‘create temp table books (title string, publish_date int64)’;-- add a row for Hamlet 
execute immediate
‘insert into books (title, publish_date) values('Hamlet', 1599)’;
```

更多例子见 [BigQuery 动态 SQL 支持](https://cloud.google.com/bigquery/docs/reference/standard-sql/scripting#execute_immediate)。

*一般来说，我们非常谨慎地使用动态 SQL，并尽可能避免使用它(主要是因为 SQL 注入的风险，它会使代码更难阅读和调试)。*

# 7.授权的用户定义函数(UDF)

对于那些不熟悉[BigQuery UDF](https://cloud.google.com/bigquery/docs/reference/standard-sql/user-defined-functions#temporary-udf-syntax)的人来说，这些是 big query 中的标量函数，允许您使用 SQL 或 Javascript(以及相关的 Javascript 库)对数据进行操作。

BigQuery 现在支持授权的 UDF，这允许授权的使用者(通过 IAM)查询数据集中的表，即使调用 UDF 的用户无权访问这些表。你们中有 MS SQL 背景的人，这类似于 SQL Server 中的表值函数的许可；这些通常用作(锁定的)表的安全层，并加强行/列级别的安全性。

# 8.查询结果中有重复的列名

BigQuery 现在允许您在一个查询中多次选择同一个(未分类的)列，方法是附加一个后缀 _n，其中 n 是观察到的重复次数。

# 9.新的 BigQuery 最后一天日期函数

BigQuery 已经很好地支持了使用 SQL 的日期操作。看起来在这个版本中，谷歌云已经认识到了一个常见的商业主导用例；查找给定期间的最后一天，例如一个月的最后一天或一周的最后一天。

新的 LAST_DAY 函数具有直观的语法，易于阅读:

```
select last_day(’2020-10-23’, week) as last_day_of_week
2020-10-24
```

*注意默认情况下，这将返回下一个星期六，因为在 BigQuery 中一周从星期日开始。*

要强制一周从星期一开始:

```
-- the last day of the week
-- (week starting monday)select last_day(’2020-10-23’,
       week(monday)) as last_day_of_week
2020-10-25
```

# 10.BigQuery 中的日期算法

我们习惯于使用 data_add 和 date_sub 函数进行日期运算，但是现在您可以使用+和-操作符来完成这项工作。

```
-- you can now do this
select ’2020-10-23’ + 2
2020-10-25-- instead of this
select date_add(’2020-10-23’, interval 2 day) 
2020-10-25
```

# 11.新的 BigQuery 字符串函数

许多(总共 14 个)新的用于操作字符串的 [BigQuery SQL 函数](https://cloud.google.com/bigquery/docs/reference/standard-sql/string_functions)。以下是我们特别喜欢的几个:

# 大查询串联运算符“||”

作为一个团队，我们认为这种连接字符串的方法比 concat 函数更容易阅读。

```
-- the new way
select ’The ’||‘quick ’||‘brown ’||‘fox’ as quip-- the old way
select concat(’The ’,‘quick ’,‘brown ’,‘fox’) as quip
```

# BigQuery 指令

这将返回字符串中搜索值的索引。注意，字符串中的第一个字符索引为 1，如果没有找到该字符串，则返回 0。

第三个参数是**位置**，表示从哪里开始搜索。默认值为 1。如果提供了负数，这表示从字符串末尾开始-n 个字符。

最后一个参数是**的出现次数**，如果提供了该参数，则指定要搜索的字符串的出现次数，例如 2 表示查找搜索值的第二个出现次数的索引。

```
select instr(’banana’,‘an’)
2
select instr(’banana’,‘an’,3)
4
select instr(’banana’,‘an’,1, 2)
4
select instr(’banana’,‘an’,1, 3)
0
```

# BigQuery Soundex

这在我们的列表中，因为我们都认为它非常简洁。这个函数返回一个单词的拼音，表示为 soundex 代码(alpha 后跟 3 个数字)。该算法于 1918 年首次获得专利，可用于模糊逻辑匹配，例如检测人名的拼写错误:

```
select soundex(’Brian’)
B650
select soundex(’Bryan’)
B650
select soundex(’Briann’)
B650
select soundex(’Brrian’)
B650
select soundex(’Brenda’)
**B653**
```

# 12.扩展信息模式

ANSI SQL 标准中指定的 INFORMATION_SCHEMA 允许用户查询包含或引用数据的各种对象或实体的元数据，如表、视图、存储过程和用户定义的函数。下面是 [BigQuery INFORMATION_SCHEMA 文档](https://cloud.google.com/bigquery/docs/information-schema-intro)。

BigQuery 引入了许多新的对象，包括表、视图、例程(存储过程、UDF)和数据集，使得这一点变得更加容易。

```
select table_name
from mydataset.INFORMATION_SCHEMA.TABLES
where table_name like 'scenario%'
```

*注意表格是大写的(BigQuery 区分大小写)。*

# 意见

总之，这是对 BigQuery SQL 库的一个很好的补充，我们肯定会在客户端项目中使用它。我们将在即将发布的数据操作系列中讨论其中的一些主题。

# 后续步骤

1.阅读[谷歌云 Bigquery 发布说明](https://cloud.google.com/blog/topics/developers-practitioners/smile-new-user-friendly-sql-capabilities-bigquery)

2.了解更多关于 [Ancoris 数据，分析& AI](https://www.ancoris.com/solutions/data_analytics_ai)

3.与[作者](https://www.linkedin.com/in/google-cloud-platform/)连线