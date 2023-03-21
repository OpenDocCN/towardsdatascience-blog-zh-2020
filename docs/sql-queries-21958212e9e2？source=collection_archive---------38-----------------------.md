# SQL 查询

> 原文：<https://towardsdatascience.com/sql-queries-21958212e9e2?source=collection_archive---------38----------------------->

## 像我这样无知的人的基础小抄。

![](img/a46c7a4005fb19a5c6f7ed57996b1f66.png)

[国家癌症研究所](https://unsplash.com/@nci?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

我不经常使用 SQL，每当我需要它的时候，我发现自己甚至在谷歌上搜索最基本操作的语法。为了帮助自己，我把有用的查询汇总到一个地方。我希望你也会觉得有用。

查询是 Postgres 格式的，但是模式可以转换成其他 SQL 格式。这些笔记基于优秀的 DataCamp 课程，如 SQL 简介、SQL 中的联接数据和 SQL 中的关系数据库简介，以及我自己的 StackOverflow 搜索。尽情享受吧！

## 符号的关键:

*   🟠, 🔵、🟢等。表示字段(即变量或列)
*   🗂️表示表名当一个查询中有多个表时，我称它们为🗂️_1、🗂️_2 等。或者 left_🗂️和 right_🗂️，哪个更方便。

## 目录:

1.  [创建、更改和删除表格，并用数据填充它们](https://medium.com/@oleszak.michal/sql-queries-21958212e9e2#e6fa)
2.  [概览表&列](https://medium.com/@oleszak.michal/sql-queries-21958212e9e2#d9fd)
3.  [选择列](https://medium.com/@oleszak.michal/sql-queries-21958212e9e2#95f4)
4.  [过滤行](https://medium.com/@oleszak.michal/sql-queries-21958212e9e2#807b)
5.  [聚合、算术、混叠、排序&分组](https://medium.com/@oleszak.michal/sql-queries-21958212e9e2#ec0e)
6.  [内部联接](https://medium.com/@oleszak.michal/sql-queries-21958212e9e2#f72f)
7.  [案例(长 if-else 语句)](https://medium.com/@oleszak.michal/sql-queries-21958212e9e2#3222)
8.  [左&右](https://medium.com/@oleszak.michal/sql-queries-21958212e9e2#5d11)
9.  [集合论条款](https://medium.com/@oleszak.michal/sql-queries-21958212e9e2#713c)
10.  [满&交叉连接](https://medium.com/@oleszak.michal/sql-queries-21958212e9e2#23ec)
11.  [半&反联接](https://medium.com/@oleszak.michal/sql-queries-21958212e9e2#3623)
12.  [子查询](https://medium.com/@oleszak.michal/sql-queries-21958212e9e2#c055)

![](img/4fa79d8e1efffb34c1c49ca6a5ee5eb5.png)

## 1.创建、更改和删除表，并向其中填充数据

![](img/4fa79d8e1efffb34c1c49ca6a5ee5eb5.png)

## 2.表格和列概述

![](img/4fa79d8e1efffb34c1c49ca6a5ee5eb5.png)

## 3.选择列

![](img/4fa79d8e1efffb34c1c49ca6a5ee5eb5.png)

## 4.筛选行

![](img/4fa79d8e1efffb34c1c49ca6a5ee5eb5.png)

## 5.聚合、算法、别名、排序和分组

![](img/4fa79d8e1efffb34c1c49ca6a5ee5eb5.png)

## 6.内部联接

![](img/4fa79d8e1efffb34c1c49ca6a5ee5eb5.png)

## 7.案例(长 if-else 语句)

![](img/4fa79d8e1efffb34c1c49ca6a5ee5eb5.png)

## 8.左右连接

![](img/4fa79d8e1efffb34c1c49ca6a5ee5eb5.png)

## 9.集合论子句

![](img/4fa79d8e1efffb34c1c49ca6a5ee5eb5.png)

## 10.完全连接和交叉连接

![](img/4fa79d8e1efffb34c1c49ca6a5ee5eb5.png)

## 11.半连接和反连接

![](img/4fa79d8e1efffb34c1c49ca6a5ee5eb5.png)

## 12.子查询

![](img/4fa79d8e1efffb34c1c49ca6a5ee5eb5.png)

感谢阅读！

如果你喜欢这篇文章，为什么不在我的新文章上 [**订阅电子邮件更新**](https://michaloleszak.medium.com/subscribe) ？通过 [**成为媒介会员**](https://michaloleszak.medium.com/membership) ，你可以支持我的写作，并无限制地访问其他作者和我自己的所有故事。

需要咨询？你可以问我任何事情，也可以在这里 预定我 1:1 [**。**](http://hiretheauthor.com/michal)

也可以试试 [**我的其他文章**](https://michaloleszak.github.io/blog/) 中的一篇。不能选择？从这些中选择一个:

[](/calibrating-classifiers-559abc30711a) [## 校准分类器

### 你确定你的模型返回概率吗？🎲

towardsdatascience.com](/calibrating-classifiers-559abc30711a) [](/boost-your-grasp-on-boosting-acf239694b1) [## 增强你对助推的把握

### 揭秘著名的竞赛获奖算法。

towardsdatascience.com](/boost-your-grasp-on-boosting-acf239694b1) [](/working-with-amazon-s3-buckets-with-boto3-785252ea22e0) [## 使用 Boto3 处理亚马逊 S3 桶。

### 完整的备忘单。

towardsdatascience.com](/working-with-amazon-s3-buckets-with-boto3-785252ea22e0)