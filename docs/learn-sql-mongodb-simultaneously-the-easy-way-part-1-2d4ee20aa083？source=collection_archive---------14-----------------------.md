# 同时学习 SQL 和 MongoDB 简单的方法(第 1 部分)

> 原文：<https://towardsdatascience.com/learn-sql-mongodb-simultaneously-the-easy-way-part-1-2d4ee20aa083?source=collection_archive---------14----------------------->

## 学习一切，简单的方法。

![](img/6402c686a7068bfc7272d97c2fc941a4.png)

礼貌: [Pixabay](https://pixabay.com/photos/server-space-the-server-room-dark-2160321/)

读了这个标题，你可能会想，究竟谁会在学习 SQL 的同时学习 MongoDB 呢？让我给你一个小蛋糕，同时学习一门关系和非关系数据库管理语言超级容易。你也可以称它们为脚本语言。

哦，你是在告诉我们它们在一起学的时候超级容易学吗？是的，它是。只要在整个系列中跟随我，你就会意识到我没有错。

但是，你为什么想要学习这些呢？嗯，知道一些可以帮助你在数据库上交互和执行操作的语言总是很棒的。另一个大问题是，如果你对数据科学、数据分析、业务分析、web 开发、应用程序开发和开发运营感兴趣，那么你必须学习这些。偶尔你将不得不接触和使用这样的数据库，如果事先没有这方面的知识，我担心你会因为没有学习它而感到内疚。

## 基本概述:

***SQL*** : SQL 是一种数据库计算机语言，设计用于关系数据库中数据的检索和管理。SQL 代表结构化查询语言。本教程将让您快速入门 SQL。它涵盖了基本理解 SQL 所需的大部分主题。SQL 数据库包含:

*   数据存储在称为**表的数据库对象中。**
*   每个表都被分解成更小的实体，称为**字段。**
*   一条**记录**也称为表中每个条目中数据的**行**。
*   **列**是表格中的垂直实体，包含与表格中特定字段相关的所有信息。

***MongoDB*** :使用 **MongoDB 查询语言** (MQL)查询 MongoDB 数据库。作为一个 NoSQL 数据库，MongoDB 确实没有使用 SQL 作为它的查询语言。相反，MongoDB 依赖于几个驱动程序，这些驱动程序允许其引擎与多种语言进行交互。非 SQL 数据库包含:

*   数据存储在被称为**集合的数据库对象中。**
*   一个**文档**是一组键值对。文档具有动态模式。这可以称为**行。**
*   **_id** :这是每个 MongoDB 文档都需要的字段。
*   **字段**是文档中的名称-值对。一个文档有零个或多个字段。这可以称为**列。**

**数据类型概述:**

SQL:

*   整数
*   Smallint
*   Numeric(p，s)——其中 ***p*** 是精度值； ***s*** 是一个刻度值。例如，numeric(6，2)是一个小数点前有 4 位、小数点后有 2 位的数字。
*   Decimal(p，s)其中 ***p*** 为精度值； ***s*** 是一个刻度值。
*   实数单精度浮点数。
*   双精度——双精度浮点数。
*   Float(p)其中 ***p*** 为精度值。
*   Char(x)其中 ***x*** 是要存储的字符数。它用空格填充，以填充指定的字符数。
*   Varchar(x)其中 ***x*** 是要存储的字符数。它没有空间垫。
*   Bit(x)其中 ***x*** 是要存储的位数。
*   位变化(x)其中 ***x*** 是要存储的位数。长度最大为 x。
*   日期-存储年、月和日的值。
*   时间-存储小时、分钟和秒的值。
*   时间戳-存储年、月、日、小时、分钟和秒的值。
*   带时区的时间-与 Time 相同，但也存储指定时间相对于 UTC 的偏移量。
*   带时区的时间戳——与时间戳相同，但也存储指定时间相对于 UTC 的偏移量。
*   年-月间隔-包含年值、月值或两者。
*   日-时间间隔-包含日值、小时值、分钟值和/或秒值。

MongoDB:

*   字符串——这是存储数据最常用的数据类型。MongoDB 中的字符串必须是 UTF-8 有效。
*   整数——用于存储数值。整数可以是 32 位或 64 位，具体取决于您的服务器。
*   boolean——用于存储布尔值(真/假)。
*   double——用于存储浮点值。
*   最小/最大键——用于将数值与最低和最高 BSON 元素进行比较。
*   arrays——它用于将数组或列表或多个值存储到一个键中。
*   时间戳 ctimestamp。当文档被修改或添加时，这对于记录非常方便。
*   对象-它用于嵌入的文档。
*   Null——用于存储空值。
*   符号——它的用法与字符串相同；然而，它通常是为使用特定符号类型的语言保留的。
*   日期**—用于以 UNIX 时间格式存储当前日期或时间。您可以通过创建日期对象并向其中传递日、月、年来指定您自己的日期时间。**
*   **对象 ID——它用于存储文档的 ID。**
*   **二进制数据——用于存储二进制数据。**
*   **code——它用于将 JavaScript 代码存储到文档中。**
*   **正则表达式——它用于存储正则表达式。**

**我不打算在一个部分中用所有的信息轰炸你，因为即使对我来说处理这么多信息是困难的，我知道那是什么感觉。我们将一步一步、非常流畅地学习 SQL & MongoDB。我将在第 1 部分介绍大约 4 个州。**

****创建表格/集合并插入记录:****

**表/集合的创建非常简单，如果您更专注于提取和分析数据，就不需要经常这样做。顺便说一下，SQL 是区分大小写的。**

**SQL:**

```
# Syntax to create a table
> **create** **table** "tablename" ("columnName" "datatype", "columnName" "datatype");# Create a table:
> **create** **table** winner ( Id INT, name VARCHAR(15))# Insert a row:
> **insert** **into** winner (Id) **values** (1)
(or)
> **insert** **into** winner **values** (1)
```

**MongoDB:**

```
# Syntax to create a collection
> db.createCollection(name, options)# Create a collection:
> db.createCollection("winner")# Syntax to insert a record> db.collectionName.insert({"key":"value"})# Inserting a record:
> db.winner.insert({"name" : "Kunal"})
```

****查询数据库:****

**查询数据库的一般定义是从集合或表中获取数据。查询数据库的基本过程非常简单。在 **SQL** 中，通过使用 **select** 来完成，在 **MongoDB** 中，通过使用 **find** 或 **aggregate** 来完成。以下是从数据库中获取数据的方法:**

**SQL:**

```
# Syntax to query a table
> **select** column **from** tables# Query all the records
> **select** * **from** winner# Query the DB, select all rows from the Id column
> **select** Id **from** winner
```

**MongoDB:**

```
# Syntax to query a collection, to select all the documents
> db.COLLECTION_NAME.find({})# Get all the records using **find**
> db.winner.find({})# Fetch data using **aggregate** > db.winner.aggregate(
[{
        $group: { 
            '_id':null,
            'max':{'$first':"$$ROOT"}
        }
    }])
```

**对于初学者，我建议您熟悉 **find** 函数，因为直接切换到 **aggregate** 函数会有点困难。原因是在后面的阶段你必须学习聚合，**发现**函数有各种限制，为了实现这一点，你必须使用**聚合**函数。**

****对查询数据进行排序:****

**当您希望数据按特定顺序排列时，排序非常重要。您之前可能运行过的查询会以随机顺序给出结果。排序在 **SQL** 中称为**订单**，在 **MongoDB** 中称为**排序**。我们可以根据列、升序、降序和限制对数据进行排序。让我告诉你怎么做。**

**SQL:**

```
# Syntax for ordering the data
> select * from tablename order by asc/desc/column/limit# Fetch all records and sort by ascending
> **select** * **from** winners **order** **by** asc# Fetch all records and sort by descending
> **select** * **from** winners **order** **by** desc# Fetch all records and sort by column
> **select** * **from** winners **order** **by** Id# Fetch all records and sort by multiple columns
> **select** * **from** winners **order** **by** Id, name
```

**MongoDB:**

```
# Syntax to sort the data - find()
> db.collectionName.find({}).sort({**KEY**:1})# Sort the data in descending order using find()
> db.winners.find({}).sort({"_id":-1})# Sort the data in ascending order using find()
> db.winners.find({}).sort({"_id":1})# Sort the data in ascending order using aggregate()
> db.users.aggregate(
   [
     { **$sort** : { name: 1 } }
   ]
)# Sort the data in descending order using aggregate()
> db.users.aggregate(
   [
     { **$sort** : { name: -1 } }
   ]
)
```

****_id** 是为插入集合中的每条记录捕获的 objectID，它是必需的。如果您解码 ObjectID，您将从中获得一个时间戳。对于聚合管道中的函数，必须加上前缀 **$。****

****基于条件查询数据库:****

**一次获取所有的记录总是很简单的，但有时可能会耗费你大量的时间，有时你甚至不需要查看全部数据。您还可以根据特定条件过滤数据。过滤的作用是，它从你的显示中去掉其余的数据，你只能看到需要的数据。在 **SQL** 中使用 **where** 完成，在 **MongoDB** 中使用$ **match** 在 **aggregate** 中完成，只是一些简单的**赋值**在 **find** 中完成。让我告诉你怎么做。**

**SQL:**

```
# Syntax to query data based on condition
> **select** * **from** table **where** *condition*# Fetch data based on a condition
> **select** Id **from** winners **where** name="Kunal" **and** Id>4# Another condition
> **select** name **from** winners **where** Id **between** 10 **and** 12
```

**Between 条件将过滤掉符合上述条件的结果。**

**MongoDB:**

```
# Syntax to query based on a condition
> db.winners.find({condition})# Fetch data based on a condition, Id greater than 2
> db.winners.find({"Id":{"$gt":2}})# Using aggregate, match is just like where in SQL
> db.winners.aggregate(
[{
"**$match**":{"Id":{"$gt":2}}
}])
```

**我们在**查找**和**聚合**中使用了相同的条件，但是你不认为**查找**更容易实现吗？**发现**有它的局限性，这就是为什么你必须在稍后阶段切换到**聚合**。我建议你习惯使用**聚合**，这将为你节省几个小时使用 find 的麻烦。**

****主题演讲:****

1.  **SQL 区分大小写。**
2.  **SQL 有表，MongoDB 有集合。**
3.  ****分号**是分隔每个 **SQL** 语句的标准方式。**
4.  **MongoDB 管道函数需要 **$** 作为前缀。**
5.  **在 MongoDB 中使用""，提高代码可读性。**
6.  **MongoDB 有 **find** 和 **aggregate** 来查询 DB。**
7.  **在 MongoDB 中， **_id** 是一个包含时间戳的 ObjectID。**

**如果你能走到这一步，你就太棒了。至此，我们结束了**系列的第一部分。****

> ***如果您遇到任何错误或需要任何帮助，您可以随时在 LinkedIn 上发表评论或 ping 我。***
> 
> *****LinkedIn:****[*https://bit.ly/2u4YPoF*](https://bit.ly/2u4YPoF)***
> 
> ****在 Github 上关注我，了解更多令人惊叹的项目和教程。****
> 
> ******Github:***[*https://bit.ly/2SQV7ss*](https://bit.ly/2SQV7ss)***

***我希望这有助于你增强你的知识基础:)***

*****更多关注我！*****

***感谢您的阅读和宝贵时间！***

> ***页（page 的缩写）这个系列的第二部分很快就要出来了。永远保持对知识的渴望。***