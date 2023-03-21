# 如何在 SQL 中克隆表

> 原文：<https://towardsdatascience.com/how-to-clone-tables-in-sql-dd29586ec89c?source=collection_archive---------0----------------------->

## 了解如何创建表的克隆

![](img/870dc3803d2425df31560bef77ad9d87.png)

照片由[卡斯帕·卡米尔·鲁宾](https://unsplash.com/@casparrubin?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在数据库操作中，有时您可能需要将一个现有的表克隆或复制到一个新表中，因为它们在列和属性上有相似性，或者是为了在不影响原始表的情况下执行测试，或者是出于其他个人原因。

我遇到了这种情况，我需要为我们正在集成的新特性创建一组新的表。这个表有相当多的列，并且非常类似于处理另一组特定特性的类似数据的现有表。

因为这个现有的表和新表非常相似，所以我的快速解决方案是克隆现有的表来创建新表。

在 SQL 中，这很容易做到，因为您可以轻松地运行几个命令来最大限度地满足您的克隆需求。

在本文中，我将向您展示如何在 SQL 中复制和克隆现有的表。

# 简单克隆

第一种方法称为简单克隆，顾名思义，它从另一个表创建一个表，而不考虑任何列属性和索引。

```
CREATE TABLE *new_table* SELECT * FROM *original_table*;
```

因此，如果我有一个名为`users`的表，我可以很容易地创建另一个名为`adminUsers`的表，而不用关心`users`表的列属性和索引。

下面的 SQL 命令创建了一个简单的`users`表副本。

```
CREATE TABLE *adminUsers* SELECT * FROM *users*;
```

使用它可以快速克隆任何只包含原始表的结构和数据的表。

# 浅层克隆

浅层克隆主要用于在不复制数据的情况下创建现有表数据结构和列属性的副本。这只会基于原始表的结构创建一个空表。

```
CREATE TABLE *new_table* LIKE *original_table*;
```

以下命令将基于原始表创建一个空表。

```
CREATE TABLE *adminUsers* LIKE *users*;
```

如果您只需要原始表的数据结构和列属性，请使用此选项

# 深度克隆

深度克隆与简单克隆有很大的不同，但与浅层克隆相似，只是数据不同，顾名思义，它创建原始表的深度副本。

这意味着新表将拥有现有表的每个列和索引的所有属性。如果您想维护现有表的索引和属性，这非常有用。

为此，我们必须根据原始表的结构和属性创建一个空表，然后从原始表中选择数据并插入到新表中。

```
CREATE TABLE *new_table* LIKE *original_table*;
INSERT INTO *new_table* SELECT * FROM *original_table*;
```

要轻松克隆我们的原件并复制其数据:

```
CREATE TABLE *adminUsers* LIKE *users*;
INSERT INTO *adminUsers* SELECT * FROM *adminUsers*;
```

现在另一件很酷的事情是，假设您不想要现有表中的所有数据，只想要一些数据，基于一些条件，然后微调您的`SELECT`查询是您的最佳选择。

例如，我们的`users`表中有带`userType="admin"`的用户，我们只想将这些用户复制到我们的新表中，您可以像下面这样轻松地完成:

```
INSERT INTO *adminUsers* SELECT * FROM *adminUsers where userType="admin"*;
```

酷吧？我知道。

如果您是 SQL 新手，下面是一些有用的资源:

[](https://www.w3schools.com/sql/default.asp) [## SQL 教程

### SQL 是一种在数据库中存储、操作和检索数据的标准语言。我们的 SQL 教程将教你…

www.w3schools.com](https://www.w3schools.com/sql/default.asp) [](https://siitgo.com/pages/lhtml5z-8/1472/html5-web-sql-database) [## HTML5 - Web SQL 数据库

### 实际上，Web SQL 数据库 API 并不是已发布的 HTML5 规范的一部分，它是一个规范…

siitgo.com](https://siitgo.com/pages/lhtml5z-8/1472/html5-web-sql-database)