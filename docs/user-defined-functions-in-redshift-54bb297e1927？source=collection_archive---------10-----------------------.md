# 红移中的用户定义函数

> 原文：<https://towardsdatascience.com/user-defined-functions-in-redshift-54bb297e1927?source=collection_archive---------10----------------------->

## 红移中用户定义函数的快速入门

![](img/58bec621c7a94673f0a58411e02c61a9.png)

亚马逊红移是一个云数据仓库，有自己的红移 SQL 方言(PostgreSQL 的变种)。由于其低成本和与其他亚马逊网络服务的兼容性，这项服务变得越来越受欢迎。我最喜欢的红移集成是能够从 S3 卸载数据和拷贝数据。

在本文中，我们将重点介绍 [**用户自定义函数**](https://docs.aws.amazon.com/redshift/latest/dg/user-defined-functions.html) **(UDFs)** 。

# 什么是用户定义函数？

UDF 是可以从红移数据仓库定制和创建的标量函数。每个函数可以接受固定数量的参数来返回一个*单个*输出。UDF 可以像内置的红移函数一样执行，比如`replace`或`lower`。

## 命名规格

亚马逊推荐所有 UDF 名字都以`f_`开头。例如，用于计算两个日期之间的营业天数的 UDF 可以命名为`f_calculate_business_days`。这将防止 UDF 名字和新红移函数之间的冲突——`f_`前缀是专门为 UDF 保留的。

## UDF 语言

可以使用 SQL `select`语句或 Python 函数创建 UDF。用 SQL 编写的 UDF 性能更高，但是 Python UDFs 具有内置库的优势。除了 Python 标准库之外，Python UDFs 还支持 pandas、scipy、numpy 等函数。还有一个选项，通过从保存在 S3 的代码创建一个库来导入额外的 Python 库。

# 创造一个 UDF

所有的 UDF 都是使用`CREATE FUNCTION`命令创建的。或者，我们可以使用`CREATE OR REPLACE FUNCTION`来更新现有的 UDF。

任何 UDF 最多只能有 32 个参数。

# SQL UDF

这是 SQL UDF 的结构:

```
create function f_sql_udf ([arg1 data type], ...)   
returns [return data type] 
stable as $$ select ...$$ language sql;
```

UDF 体由*一个*选择语句组成。SQL UDF 中的参数不能命名。它们必须被引用(按照它们被定义的顺序)为$1、$2 等等。参数可以采用任何红移数据类型。

# 蟒蛇 UDF

这是 Python UDF 的结构:

```
create function f_python_udf (arg_name [arg1 data type], ...)   returns [return data type] 
stable as $$ [Python code here]$$ language plpythonu;
```

与 SQL UDFs 不同，Python UDFs 需要参数名。已安装的库也可以作为 Python 代码的一部分导入。参考[本页](https://docs.amazonaws.cn/en_us/redshift/latest/dg/udf-data-types.html)了解支持的 Python UDF 数据类型。

# UDF 和用法示例

考虑一个非常简单的 UDF 清洗字符串。我们希望这个 UDF 将输入转换成小写，并用空格替换下划线。虽然这些清理步骤可以内置到 SQL 查询本身中，但是我们也可以使用 UDF 来使查询更加一致和易读。

## 结构化查询语言

```
create function f_clean_text (varchar)   
returns varchar
stable as $$select replace(lower($1), '_', ' ')$$ language sql;
```

## 计算机编程语言

```
create function f_clean_text (text varchar)   
returns varchar
stable as $$return text.lower().replace('_', ' ')$$ language plpythonu;
```

## 使用

我们可以用这个语句来测试我们的新 UDF。

```
select f_clean_text('eRROR_BAD_input')
```

我们可以将这个 UDF 应用于一个名为`error_messages`的列

```
select f_clean_text(error_messages) from schema.table
```

# 多方面的

*   要查看用于创建现有 UDF 的代码，请查询`pg_proc`表
*   UDF 支持 Python 日志库，这允许我们[在 UDF 中记录警告和错误](https://docs.amazonaws.cn/en_us/redshift/latest/dg/udf-logging-messages.html)
*   数据库管理员可以使用 GRANT 和 REVOKE 来自定义 [UDF 特权](https://docs.amazonaws.cn/en_us/redshift/latest/dg/udf-security-and-privileges.html)
*   要删除 UDF，运行`drop function udf_name(arguments)`

这是一个练习和应用 SQL 的 Coursera 课程。

[](https://click.linksynergy.com/link?id=J2RDo*Rlzkk&offerid=759505.17916350380&type=2&murl=https%3A%2F%2Fwww.coursera.org%2Fspecializations%2Fdata-science-fundamentals-python-sql) [## Python 和 SQL 的数据科学基础

### IBM 是通过开放的混合云平台和人工智能进行业务转型的全球领导者，为客户提供…

click.linksynergy.com](https://click.linksynergy.com/link?id=J2RDo*Rlzkk&offerid=759505.17916350380&type=2&murl=https%3A%2F%2Fwww.coursera.org%2Fspecializations%2Fdata-science-fundamentals-python-sql) 

# 感谢您的阅读！

[![](img/c2fcf86bbf79d7f57582562b51819c0c.png)](https://ko-fi.com/mandygu#checkoutModal)

如果你喜欢这篇文章，可以考虑给我买杯咖啡——每一点小小的贡献都帮助我找到更多的时间在这个博客上工作。[通过 Medium](https://medium.com/@mandygu) 关注我的最新更新。😃

作为一个业余爱好项目，我还在[www.dscrashcourse.com](http://www.dscrashcourse.com/)建立了一套全面的**免费**数据科学课程和练习题。

再次感谢您的阅读！📕