# 掌握数据科学的 SQL 技能所需的一切

> 原文：<https://towardsdatascience.com/everything-that-needs-to-master-sql-skills-for-data-science-dca851995b8d?source=collection_archive---------5----------------------->

如果你谷歌“如何学习 SQL？”，你得到很多结果，这是压倒性的。让我们来看看一些最好的资源。

![](img/1223d912ba465be40c53ab2fe39229d6.png)

[杰夫·W 在 Unsplash 上拍摄的照片](https://unsplash.com/photos/X-o-Pd_FRU4)

我写这篇文章是通过分析业内一些最好的数据科学家写的各种 LinkedIn 帖子以及我在学习 SQL 时经历的事情。我认为这是 SQL 的一些好资源的列表。我用过其中的一些，但不是全部。

本文包括 SQL 的重要性、必须涵盖的重要主题、课程、实践平台、面试准备资源，以及 SQL 注释。

# SQL 的重要性？

在脸书最近发布的 25 个数据科学家职位中，每个职位都列出了 SQL 技能。在 2020 年 LinkedIn 印度十大创业公司名单中，有 7 家将 SQL 作为他们**最常用的技能之一**。这种经常被低估的语言不仅在印度，而且在全世界都是需要的顶级技能之一。只要数据科学中有‘数据’，SQL 就仍然是它的重要组成部分。虽然 SQL 已经有 40 多年的历史，但它在 21 世纪仍然适用，这是因为它提供了许多优于替代产品的关键优势。

# SQL 在什么情况下出现？

数据科学是对数据的研究和分析。为了分析数据，我们需要从数据库中提取数据。这就是 SQL 的用武之地。

许多数据库平台都是模仿 SQL 的。这是因为它已经成为许多数据库系统的标准。事实上，像 Hadoop、Spark 这样的现代大数据系统利用 SQL 来维护关系数据库系统和处理结构化数据。

识别正确的数据源、获取数据和预处理数据是任何描述性或预测性分析工作的基本步骤。由于这些数据主要存储在关系数据库中，因此，为了查询这些数据库，数据科学家必须具备良好的 SQL 知识。而 SQL 在这些步骤中扮演着最关键的角色。

为了通过创建测试环境来试验数据，数据科学家使用 SQL 作为他们的标准工具，并使用存储在 Oracle、Microsoft SQL、MySQL 等关系数据库中的数据进行数据分析，我们需要 SQL。

# 数据科学 SQL 中的重要主题

在进入参考资料之前，让我们看看哪些是重要的主题。确保你涵盖了以下话题，但不限于这些。

**1】Group By 子句:**SQL Group By 子句与 SELECT 语句配合使用，将相同的数据分组。大多数情况下，我们将聚合函数与 group by 子句一起使用，也使用 *Having 子句*和 group by 子句*来应用条件。*

**2】聚合函数**:聚合函数对一组值执行计算，并返回单个值。《出埃及记》计数、平均值、最小值、最大值等。

**3】字符串函数和操作:**为了执行各种操作如将字符串转换成大写，匹配一个正则表达式等。

《出埃及记》1]查找名称以“A”开头的学生 id。2]从地址栏中获取 pin 码。

**4】日期&时间操作:**当值包含唯一的日期时，很容易处理，但是当还包含时间部分时，事情就有点复杂了。所以一定要多练习。

**5】输出控制语句:**按要求得到结果。例如:order by 子句，limit 函数获取有限的行。

**6】各种运算符:**主要有三种运算符，分别是*算术运算符、逻辑运算符和比较运算符*。

**7]联接:**这是一个重要的主题，用于联接多个表以获得期望的输出。确保您了解所有的概念，如连接类型、主键、外键、组合键等。

**8】嵌套查询:**子查询/嵌套用于返回数据，当该数据将在主查询中用作条件以进一步限制要检索的数据时。

*(嵌套查询可用于返回标量(单个)值或行集；然而，联接用于返回行。如果您可以用两种方式执行操作，那么优化的方式是使用连接。)*

**9】视图&索引:**索引是数据库搜索引擎可以用来加速数据检索的特殊查找表。简单地说，数据库中的索引类似于一本书的索引。

临时表:这是一个很棒的特性，它允许您使用相同的选择、更新和连接功能来存储和处理中间结果。

**11】窗口函数:**窗口函数对一组行进行操作，并为基础查询中的每一行返回一个值。它们降低了分析数据集分区(窗口)的查询的复杂性。

**12】查询优化:**当我们处理较大的数据集时，使用最有效的方法让 SQL 语句访问所请求的数据是很重要的。

最后是 **13】常用表表达式。**

*如果您在以下课程中没有找到****【CTE】****，那么这里有 5 个有用的资源:* [*资源 1*](https://www.essentialsql.com/introduction-common-table-expressions-ctes/) *，* [*资源 2*](https://www.sqlservertutorial.net/sql-server-basics/sql-server-cte/) *，* [*资源 3*](https://www.geeksforgeeks.org/cte-in-sql/)*[*资源 4*](https://chartio.com/resources/tutorials/using-common-table-expressions/)*

# *学习和实践的资源:*

# *课程:*

***1】uda city 用于数据分析的 SQL:***

*[](https://www.udacity.com/course/sql-for-data-analysis--ud198) [## 数据分析| Udacity

### 在本课程中，您将学习使用结构化查询语言(SQL)来提取和分析存储在数据库中的数据…

www.udacity.com](https://www.udacity.com/course/sql-for-data-analysis--ud198) 

这是最好的 ***免费*** 课程之一，涵盖上述所有主题，每个主题后都有清晰的解释和练习测验。实践测验的质量使本课程非常有效。总的来说，这是一门很棒的课程。

**2】Excel 用户 SQL 编程入门:**

如果你是一个 Excel 用户，并想学习 SQL，那么这将是一个很好的 YouTube 播放列表。这涵盖了以上列表中的主要主题。

**3】Udemy 的数据科学主 SQL:**

[](https://www.udemy.com/course/master-sql-for-data-science/) [## 面向数据科学的 SQL:通过交互式练习学习 SQL

### 本课程将使您成为 SQL 查询向导。你将学到从…中提取关键洞察力所需的技能

www.udemy.com](https://www.udemy.com/course/master-sql-for-data-science/) 

在本课程中，您将学习从数据库中的数据中提取关键洞察力所需的技能。有超过 **100 个谜题**散布在整个课程中，有深入的解决方案为你提供大量练习的机会。

**4】可汗学院:**

[](https://www.khanacademy.org/computing/computer-programming/sql) [## SQL 简介:查询和管理数据|可汗学院

### 了解如何使用 SQL 来存储、查询和操作数据。SQL 是一种专用编程语言，专为…

www.khanacademy.org](https://www.khanacademy.org/computing/computer-programming/sql) 

这是一个非营利教育组织，目标是创建一套帮助教育学生的在线工具。这是一个很棒的平台，提供免费的优质课程，并有详细的解释。你也可以尝试其他课程[这里](https://www.khanacademy.org/)。尤其是我爱统计一。

整个课程包含 5 个部分，从基础开始，一直到更高级的课程。在本课程中，每个主题的末尾都有挑战，然后是视频教程和一个小项目。

**5】200+SQL 面试问题:**

[](https://www.udemy.com/course/sql-interview-questions/?LSNPUBID=JVFxdTr9V80&ranEAID=JVFxdTr9V80&ranMID=39197&ranSiteID=JVFxdTr9V80-u9gxl6DftoBTfGmhkAhkog&utm_medium=udemyads&utm_source=aff-campaign) [## 面向开发人员的 200 多个 SQL 面试问题和答案

### 您准备好参加 SQL Developer 面试了吗？我在数据库领域有大约 15 年以上的经验，并且…

www.udemy.com](https://www.udemy.com/course/sql-interview-questions/?LSNPUBID=JVFxdTr9V80&ranEAID=JVFxdTr9V80&ranMID=39197&ranSiteID=JVFxdTr9V80-u9gxl6DftoBTfGmhkAhkog&utm_medium=udemyads&utm_source=aff-campaign) 

如果你正在为面试做准备，本课程将通过解决复杂的问题来帮助你。在本课程中，您将找到 200 多个真实世界的 SQL 问题和实用答案。

**6】LinkedIn Master SQL for Data Science:**

[](https://www.linkedin.com/learning/topics/sql) [## SQL 在线培训课程| LinkedIn Learning，原名 Lynda.com

### 从选择编程语言到理解存储过程，通过观看我们的…

www.linkedin.com](https://www.linkedin.com/learning/topics/sql) 

数据科学主 LinkedIn

这总共包含 6 个项目。这套课程涵盖了数据科学所需的各个方面。

如果你对 SQL 的**历史感兴趣，那就去看看 [*这段视频*](https://www.youtube.com/watch?v=vyVGm_2iFwU&list=PLSE8ODhjZXja3hgmuwhf89qboV1kOxMx7&index=1&ab_channel=CMUDatabaseGroup) (仅第 1 部分)。**

*如果你是一名* ***初学者*** *，选修以上任何一门课程或你认为好的任何其他课程，并完成以上列表中的所有主题。*

*如果你* ***知道了基础知识*** *那么对复习所有题目* [*这段视频*](https://www.youtube.com/watch?v=80atcA6gBU8&list=PLSE8ODhjZXja3hgmuwhf89qboV1kOxMx7&index=2&ab_channel=CMUDatabaseGroup) *会很有帮助。*

> 实践是成功的关键。

一旦你知道了所有的主题，就该练习了。没有实践，你无法掌握任何技能。所以让我们来看看一些好的练习平台……

# SQL 查询实践平台:

**1】leet code:**

[](https://leetcode.com/problemset/database/) [## 问题- LeetCode

### 提高你的编码技能，迅速找到工作。这是扩展你的知识和做好准备的最好地方…

leetcode.com](https://leetcode.com/problemset/database/) 

这是一个最好的实践平台，有各种各样的问题。下面是一些好问题有[第二高工资问题](https://leetcode.com/problems/second-highest-salary/)、[重复邮件](https://leetcode.com/problems/duplicate-emails/)、[班级 5 人以上](https://leetcode.com/problems/classes-more-than-5-students/)、[升温](https://leetcode.com/problems/rising-temperature/)、[班级 5 人以上](https://leetcode.com/problems/classes-more-than-5-students/)。

**2】SQL 动物园:**

 [## SQLZOO

### 跳转到导航跳转到搜索新教程:新冠肺炎国际数据。这个服务器是由爱丁堡纳皮尔托管的…

sqlzoo.net](https://sqlzoo.net/wiki/SQL_Tutorial) 

SQLZoo 是一个完善的在线平台(自 1999 年以来)，用于编写和运行针对实时数据库的 SQL 查询。您可以看到查询的实际结果，而不必仔细检查您的查询是否与解决方案匹配，因为解决一个问题可能有多种方法。*评估*部分包含更多复杂的示例，允许您以不同的难度深入数据库

**3】黑客排名:**

[](https://www.hackerrank.com/domains/sql) [## 解决 SQL 代码挑战

### 加入超过 1100 万名开发人员的行列，在 HackerRank 上解决代码挑战，这是为…

www.hackerrank.com](https://www.hackerrank.com/domains/sql) 

这是一个很好的练习平台。这里的问题分为三个部分，容易，中等，困难。

**4】SQL Bolt:**

 [## SQLBolt -学习 SQL-SQL 介绍

### 欢迎使用 SQLBolt，这是一系列交互式课程和练习，旨在帮助您在工作中快速学习 SQL

sqlbolt.com](https://sqlbolt.com/lesson/introduction) 

本质上，它是一系列互动的课程和练习，旨在帮助用户轻松学习 SQL(T21)。本网站上的课程和主题非常全面，涵盖了使用 SQL 的所有重要细节。

**5]选择* SQL:**

 [## 选择星形 SQL

### 这是一本交互式的书，旨在成为互联网上学习 SQL 的最佳场所。这是免费的…

selectstarsql.com](https://selectstarsql.com/) 

这是一本**交互式书籍**，旨在成为互联网上学习 SQL 的最佳场所。它是免费的，没有广告，不需要注册或下载。它通过对现实世界的数据集运行查询来帮助您完成重要的项目。

**6】模式:**

[](https://mode.com/sql-tutorial/) [## 模式 SQL 教程

### 学会用 SQL 用数据回答问题。不需要编码经验。

mode.com](https://mode.com/sql-tutorial/) 

在这里，您可以按主题练习*和*，有四个主要部分，分别是基础、中级、高级和 SQL 分析培训。在这里，您可以阅读理论并练习 SQL 查询。

**7】斯坦福大学:**

[](https://online.stanford.edu/lagunita-learning-platform) [## LAGUNITA 学习平台上的斯坦福课程

### 斯坦福在 2013 年 6 月发布了 edX 平台的第一个开源版本 Open edX。我们将我们的实例命名为…

online.stanford.edu](https://online.stanford.edu/lagunita-learning-platform) 

[在这里](https://pgexercises.com/questions/basic/)你可以练习题型智慧题。用户界面不是很好，但你会得到高质量的材料。在这里，您可以解决主题式查询。涵盖的主要主题是基础知识、连接、子查询、修改数据、聚合、日期、字符串，最后是递归。

# 面试准备资源

如果你正在准备面试，下面的资源可以帮助你，

**1】Zachary Thomas 的数据分析师 SQL 面试问题:**

 [## 最佳中硬数据分析师 SQL 面试问题

### 扎克里·托马斯·(zthomas.nc@gmail.com，推特，LinkedIn)

quip.com](https://quip.com/2gwZArKuWk7W) 

SQL 的前 70%相当简单，剩下的 30%可能相当复杂。科技公司数据分析师和数据科学家的面试问题通常来自这 30%。在这里，他专注于那些中等难度的问题。如果你练习了以上任何一个平台的所有重要话题(我会更喜欢 Leetcode。)那么这就足够面试准备了。

**2】数据和尚:**

[](http://thedatamonk.com/sql-data-science-interview-questions/) [## 150 多个 SQL 数据科学面试问题

### SQL 数据科学面试问题这里我们有 150 多个 SQL 面试问题，大部分数据都会问这些问题…

thedatamonk.com](http://thedatamonk.com/sql-data-science-interview-questions/) 

在这里你可以找到公司的所有主题(SQL、python、统计学、案例研究等。)面试问题破解数据科学与数据分析师面试。

# 笔记

1]如果你想在短时间内复习所有概念，那么 [**这里是 GoalKicer 提供的 SQL 100+ pages**](https://drive.google.com/file/d/12jParR0hyGNkp2hUQtFaJ5mVt9g1Bn_f/view?usp=sharing) 笔记。

**2】**[**这是 stack overflow 投稿人创作的电子书。**](https://drive.google.com/file/d/1eXnsvPD-tqcgtGYszzhReJ8ldsnD0ipj/view?usp=sharing) 这本书涵盖了所有的概念。

***注:很可能还有其他资源。太好了。我只是还没有全部做完！***

如果你发现任何可以帮助别人的东西，请写在评论里，我会把这些资源添加到博客里。

# 结论

在本文中，我们回顾了所有有助于规划您自己的 SQL 跟踪的资源。本文涵盖了从基础知识到面试准备的所有资源。如果你还在困惑“如何开始？”，只需使用每个标题下的第一个资源。

**您可能想阅读的其他文章**

[](/the-complete-guide-to-linear-regression-analysis-38a421a89dc2) [## 线性回归分析完全指南

### 这篇文章是关于用统计术语理解线性回归的。

towardsdatascience.com](/the-complete-guide-to-linear-regression-analysis-38a421a89dc2)*