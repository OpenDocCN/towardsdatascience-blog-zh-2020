# MySQL 中的数据分析——关系数据库中的运算符、连接等

> 原文：<https://towardsdatascience.com/data-analysis-in-mysql-operators-joins-and-more-in-relational-databases-26c0a968e61e?source=collection_archive---------5----------------------->

## 【Craig Dickson 的 SQL 教程

## 学习使用 SQL 和免费开源软件创建、更新和查询您自己的全功能关系数据库——第 3 部分

![](img/51fcc0a6d28847cc4fe7bba46c351db0.png)

我们的国际语言学校数据库的实体关系图(ERD)

*这是一个由 3 部分组成的系列文章的第 3 部分，从零开始，带您了解使用 MySQL 设计、编码、实现和查询关系数据库的过程。参见第一部分(* [*设计关系数据库并创建实体关系图*](https://medium.com/@thecraigdickson/designing-a-relational-database-and-creating-an-entity-relationship-diagram-89c1c19320b2) *)* [*此处*](https://medium.com/@thecraigdickson/designing-a-relational-database-and-creating-an-entity-relationship-diagram-89c1c19320b2) *)和第二部分(* [*使用 MySQL*](https://medium.com/@thecraigdickson/coding-and-implementing-a-relational-database-using-mysql-d9bc69be90f5)*)*[*此处*](https://medium.com/@thecraigdickson/coding-and-implementing-a-relational-database-using-mysql-d9bc69be90f5) *)。*

*本教程的所有代码和信息都可以在相关的* [*GitHub 资源库*](https://github.com/thecraigd/SQL_School_Tutorial) *中找到。我使用了*[*lucid chart*](https://www.lucidchart.com/pages/)*来制作文章中显示的图表。*

在本系列的第 [1](https://medium.com/@thecraigdickson/designing-a-relational-database-and-creating-an-entity-relationship-diagram-89c1c19320b2) 和 [2](https://medium.com/@thecraigdickson/coding-and-implementing-a-relational-database-using-mysql-d9bc69be90f5) 部分中，我们已经经历了设计数据库、在 MySQL 中实现它以及用我们的数据填充它的步骤。在进入本文之前阅读这些是一个好主意，但是如果你想跳到这个分析部分，让你跟上进度的 SQL 代码都包含在相关的 [GitHub 库](https://github.com/thecraigd/SQL_School_Tutorial)中。

# 查询关系数据库

我们从头开始设计并实现了一个全功能的关系数据库。很棒的东西！但是现在呢？

如果我们从事数据分析师类型的工作，我们会希望开始分析这些数据。但是首先，我们需要知道如何到达它。本文将描述如何用 SQL(特别是 [MySQL](https://www.mysql.com/) )编写查询，这将允许我们访问锁定在我们的 [RDBMS](https://techterms.com/definition/rdbms) 中的数据和关系。

## 正在设置

我们在本系列的第 2 部分的[中介绍了安装](https://medium.com/@thecraigdickson/coding-and-implementing-a-relational-database-using-mysql-d9bc69be90f5) [MySQL 社区服务器](https://dev.mysql.com/downloads/mysql/)，所以我们应该有一个运行在我们系统上的实例。我们还将再次使用 [PopSQL](https://popsql.com/) 的免费计划来运行我们的查询。

## 查询数据库

SQL 查询语句的主要形式如下:

```
SELECT *columns* FROM *table* WHERE *condition;*
```

例如，要从我们的教师表中选择所有的列，我们使用语句:

```
SELECT *
FROM teacher;
```

在这种情况下，星号(*)表示“所有列”。我们正在选择教师表，并且没有附加条件，这意味着我们将从该表中选择所有列和所有记录(即所有数据)。

![](img/b4515f3c9df7802085b056fb5ecd355d.png)

不错！

如果我们只需要表中的特定属性，我们可以命名这些属性，而不是使用星号。

```
SELECT last_name, dob
FROM teacher;
```

![](img/08a17b1d04d9dbecf8fc6e545a547ffa.png)

## 使用条件

在大多数情况下，我们不想选择一个表中的所有记录，而是寻找满足某些条件的特定记录。在这些情况下，我们将需要使用一个 [WHERE](https://www.mysqltutorial.org/mysql-where/) 子句。

请注意，在本文中，我们将在 SELECT 语句中使用 WHERE 子句，但是完全相同的语法和操作符也适用于 UPDATE、INSERT 或 DELETE 语句。WHERE 子句是所有需要掌握的 SQL[风格](/the-many-flavours-of-sql-7b7da5d56c1e)的重要方面。

WHERE 子句允许我们设置一个条件，例如:

```
SELECT *
FROM course
WHERE language = 'ENG';
```

这将从课程表中选择语言为英语的课程的所有详细信息。

![](img/603d4bbb4b211ef0366626c6e93d1e70.png)

果不其然，确实如此

也许我们想对结果进行不同的排序，那么我们可以使用 [ORDER BY](https://www.w3schools.com/sql/sql_orderby.asp) 来做到这一点。

```
SELECT *
FROM course
WHERE language = 'ENG'
ORDER BY start_date;
```

![](img/256118b9608e86bf37c9a027c7bd22db.png)

默认情况下，MySQL 按升序排序。如果我们想让结果按降序排列，我们可以像这样添加 DESC:

```
SELECT *
FROM course
WHERE language = 'ENG'
ORDER BY start_date DESC;
```

![](img/e764b20921b645ea353572c7da7ecc47.png)

我们也可以按顺序使用 ORDER BY，因此我们可以:

```
SELECT *
FROM course
WHERE language = 'ENG'
ORDER BY start_date DESC, client, teacher;
```

这将按照属性的层次结构，按照它们被指定的顺序，给出我们的结果。

## AND、OR 和 NOT 运算符

我们也可以在 WHERE 子句中使用[和](https://www.w3schools.com/sql/sql_and_or.asp)运算符。如果你熟悉任何其他编程语言，你很可能以前就遇到过这些逻辑操作符。在 SQL 中，它们完全按照您的预期工作。

```
SELECT *
FROM course
WHERE language = 'ENG' AND level = 'C1';
```

![](img/445b1c12688bd998fa98ef6021386a96.png)

使用和给我们所有的课程，语言是英语和水平是 C1

使用 AND 运算符排除了所有不符合这两个条件的结果，因此只显示 C1 英语水平的课程——在国际语言学校数据库中，这给了我们一个结果。如果我们在同一个语句中使用 OR，我们会得到一些不同的结果。

```
SELECT *
FROM course
WHERE language = 'ENG' OR level = 'C1';
```

![](img/08b5c5398b675550b4771e4464f01cbf.png)

使用 OR 为我们提供所有英语课程，以及所有其他 C1 水平的课程

使用 OR 运算符包括满足任一条件的所有记录，因此现在我们有所有英语课程(因为这些课程满足 language = 'Eng '条件)加上一门 C1 级别的课程，高级俄语。

我们还可以在条件前使用 NOT 运算符，以排除符合该条件的所有记录，并包括所有其他记录。

```
SELECT *
FROM course
WHERE NOT language = 'ENG' OR level = 'C1';
```

![](img/b91d59390f5291712e07931330590d9a.png)

使用 NOT 会给出表中不满足第一个条件的所有记录。此处包含课程 15，因为它满足第二个条件(级别= 'C1 ')。

当我们想要排除某些记录时，这非常有用。

## 比较运算符

还可以在 WHERE 子句中使用其他的[比较](https://www.techonthenet.com/mysql/comparison_operators.php)操作符。这是我们可以在 MySQL 中使用的比较运算符的完整列表:

*   等于:`=`
*   不等于:`<>`或`!=`
*   小于:`<`
*   小于或等于:`<=`
*   大于:`>`
*   大于或等于:`>=`

这给我们带来了许多可能性。一个非常简单的例子是识别 1990 年以前出生的所有教师的姓名和联系信息:

```
SELECT first_name, last_name, phone_no
FROM teacher
WHERE dob < '1990-01-01';
```

![](img/779f40e6dad2bc6831f2b778bf185a80.png)

这些是我们的，我们可以说，更有经验的老师

## BETWEEN 运算符

运算符之间的[允许我们选择值在某个范围内的记录。我们可以使用以下代码来识别 2020 年 1 月开始的所有课程:](https://www.w3schools.com/sql/sql_between.asp)

```
SELECT *
FROM course
WHERE start_date BETWEEN '2020-01-01' AND '2020-01-31';
```

![](img/5195a070a54f1b20dcbb8ab862bd520e.png)

只有 2020 年 1 月的一门新课程

在我们的示例中，我们比较的是日期，但是 BETWEEN 也可以用于查找特定范围内的任何值(销售额、订单数量等)。非常有用！

## 我们喜欢这样

[LIKE](https://www.w3schools.com/sql/sql_like.asp) 操作符允许我们搜索与指定模式匹配的值。我们可以使用两个[通配符](https://www.w3schools.com/sql/sql_wildcards.asp)来构建我们的模式:

*   此通配符代表零个、一个或多个字符
*   `_`该通配符仅代表一个字符

如果你熟悉[正则表达式](https://www.regular-expressions.info/)，那么这似乎对你来说很熟悉，因为 SQL 基本上实现了它们的简化版本。这允许我们构建模式，并让 RDBMS 找到符合这些模式的记录。

让我们看一个例子:

```
SELECT course_name
FROM course
WHERE course_name LIKE '%interm%';
```

![](img/d2baa801bf78af99f60432ecdb100346.png)

我们找到了三个名字符合我们模式的课程

通过在模式的开头和结尾使用`%`通配符，我们指示 MySQL 识别出现在 course_name 属性中的字符序列‘interm’。这是一个非常有用的特性。

## 进入最佳状态

我们的下一个操作符是中的[——这是许多 or 条件的有效简写，允许我们给 MySQL 一个我们想要使用的值的列表。](https://www.w3schools.com/sql/sql_in.asp)

```
SELECT first_name, last_name
FROM participant
WHERE last_name IN ('Garcia', 'Nowak', 'Mustermann');
```

![](img/cf4149e52db0cc805e07de0d591155b4.png)

这里的输出与我们从查询中得到的输出相同:

```
SELECT first_name, last_name
FROM participant
WHERE last_name = 'Garcia' OR last_name = 'Nowak' OR last_name = 'Mustermann';
```

好处是我们不用重复写出列名。当我们的查询中包含很多值时，这是非常有益的。

## 为空

我们将在本节中讨论的最后一个运算符是[为空，](https://www.techonthenet.com/mysql/is_null.php)及其负表亲不为空。这允许我们识别特定属性(或一组属性)为空的记录。这对于用其他值更新它们、从数据集中排除它们或者出于各种其他原因可能是有用的。

在我们的数据库中，我们可以用这个来识别“只”用一种语言教学的教师:

```
SELECT * 
FROM teacher
WHERE language_2 IS NULL;
```

![](img/3e41e7d5c8381fad691c3b81dcef8bff.png)

詹姆斯和斯蒂芬妮，这仍然令人印象深刻。继续努力吧！

或者，我们可以使用 IS NOT NULL 来查找用两种语言教学的教师:

```
SELECT * 
FROM teacher
WHERE language_2 IS NOT NULL;
```

![](img/f4a6122582526d418d289d72acceac3e.png)

然而，这更令人印象深刻

## 错认假频伪信号

有时，我们数据库中的数据以属性名称存储，这可能会使我们报告的受众感到困惑，或者只是以一种不吸引人的方式格式化。我们可以通过使用[别名](https://www.w3schools.com/sql/sql_alias.asp)在查询阶段解决这个问题。

这里我们使用 AS 关键字来指示 MySQL 显示带有临时名称的特定列。例如:

```
SELECT course_name AS 'Course Name', course_length_weeks AS 'Length of Course (Weeks)'
FROM course
WHERE language = 'DEU';
```

![](img/e4d93db436c854bffc0ff5a881d0c53c.png)

哦，太棒了！

当我们为其他利益相关者准备报告并想让事情看起来更漂亮时，或者当一个列的名称在我们的特定查询的上下文中可能会引起混淆时，这可能是有用的。

## 汇总数据

在执行数据分析时，能够汇总我们的数据也很重要。MySQL 提供了许多方法来实现这一点，我们不会在这里一一介绍。这里有一个全面的列表[，](https://www.w3resource.com/mysql/aggregate-functions-and-grouping/aggregate-functions-and-grouping-in-mysql.php)供那些想深入了解的人使用。

我们想要执行的最常见的聚合之一是求平均值。在 MySQL 中，我们用 [AVG()](https://www.w3schools.com/sql/func_mysql_avg.asp) 聚合器来做这件事。如果我们想知道一门课程的平均长度，我们可以使用以下查询:

```
SELECT AVG(course_length_weeks)
FROM course;
```

![](img/5b86c216fc23ace022913c1876aa3d23.png)

平均疗程为 20.5556 周。有意思！

在使用聚合器时，通常会将它们与由语句组成的[组合使用。这可以帮助我们超越基础，在数据库中找到真正有用的信息。](https://www.w3schools.com/sql/sql_groupby.asp)

```
SELECT client, AVG(course_length_weeks)
FROM course
GROUP BY client;
```

![](img/50d22c12adcad4ebf16026678e1a81c3.png)

这就更有趣了！

您可以看到我们如何使用 GROUP BY 和一些聚合器在我们的数据库中找到真正的见解。在这里，我们可能希望了解为什么客户 101 预订的课程平均比其他客户长得多，并看看我们是否可以在那里学到一些东西，让其他客户效仿。

另一个需要了解的重要聚合器是[计数](https://www.w3resource.com/mysql/aggregate-functions-and-grouping/count.php)。这让我们可以计算表中特定值的实例数。

```
SELECT COUNT(*)
FROM course;
```

![](img/81dafefde8c28d8a6b86c2cd08ff34b8.png)

好的，很高兴知道

让我们说得更具体些。

```
SELECT COUNT(*)
FROM course
WHERE language = 'Eng';
```

![](img/8a5cf04e7ecac7ac4842e9352f194e85.png)

哦，不错

但是我们可以使用 GROUP BY 使它更加有用。

```
SELECT language, COUNT(language)
FROM course
GROUP BY language;
```

![](img/3e53e1a677958e35e5ac84cb8037616b.png)

非常好！

在 MySQL 中，我们还可以做更多的聚合数据的工作，但这已经足够让我们开始了。

# 嵌套查询

当我们开始在其他查询中嵌套查询时，事情会变得更加令人兴奋。这就是 SQL 真正展示其威力的地方。我们将通过一个简单的例子来演示这些原理，但是通过在查询中嵌套查询，编写真正复杂和强大的查询的可能性几乎是无限的。

让我们看看我们的教师表。

![](img/400aad982f0aa59ec44380669025d535.png)

如果我们想要识别比我们老师的平均年龄更年轻的老师(一系列日期的平均年龄到底是多少是一个复杂的问题，但我们现在会忽略它)。我们可以用如下的嵌套查询来回答这个问题:

```
SELECT *
FROM teacher
WHERE dob > 
    (SELECT AVG(dob)
    FROM teacher);
```

![](img/c6f0e0b6df3435c66156e1618f186325.png)

这里发生的事情是，在我们的 WHERE 子句中，我们放置了另一个完整的 SQL 查询(在括号中)。这就是嵌套查询，一个完整的查询在另一个查询中使用。

这是一个基本的例子，但是希望这种查询的能力和潜力是显而易见的。这不会是我们的最后一个嵌套查询，所以我们以后会有更多的机会看到它是如何工作的。

# 请加入我们

关系数据库的全部好处是实体/表相互关联的方式，允许我们探索和分析各种实体之间的关系。到目前为止，我们所有的查询都是针对单个表的。这很好，但是我们错过了关系数据库的一些主要优势。让我们通过观察[加入](https://dev.mysql.com/doc/refman/8.0/en/join.html)的奇妙世界来补救这一点。

如果您熟悉其他流行的数据分析工具，如针对 [Python](https://www.python.org/) 的[熊猫](https://pandas.pydata.org/)或针对 [R、](https://www.r-project.org/)的 [dplyr](https://dplyr.tidyverse.org/) ，那么您可能已经熟悉了连接。MySQL 中的概念完全相同，但是语法当然有一点不同(首先是更多的大写字母)。

要将任何表连接到另一个表，我们需要在两个表中都有一个包含相同数据的列。在关系数据库中，这是外键的功能(或者对于 N 到 M 关系，是存储这些关系的表)，这些连接使得关系数据库成为存储大量数据的强大而有效的方式。

SQL 中有三种类型的连接——一个[内部连接](https://www.w3schools.com/sql/sql_join_inner.asp)(在 MySQL 中，当只使用连接时，这是默认的)和两种类型的外部连接——左连接[和右连接](https://www.w3schools.com/sql/sql_join_left.asp)和。我们将从最常见、也最容易理解的内部连接开始，在 SQL 中也称为普通连接。

## 内部联接

内部联接采用两个表，并基于它们共有的列将它们联接起来。这允许我们利用表之间存在的关系。让我们先看一个例子:

```
SELECT course.course_id, course.course_name, course.language, client.client_name, client.address
FROM course
JOIN client
ON course.client = client.client_id
WHERE course.in_school = FALSE;
```

![](img/bbf58781f4ba2cbf3ce452cfd83d7362.png)

我们第一次加入的产出！

这里我们从两个不同的表中选择了列。在 MySQL 中，当我们处理多个表时，有必要一起标识列和表。一般来说，这样做也是一种很好的做法，因为它可以让任何阅读您代码的人清楚地知道引用了哪些列。

这是使用上面看到的 *table.column* 语法完成的。在我们的查询中，我们从 course 表中选择 course_id、course_name 和 language 列，从 client 表中选择 client_name 和 address 列。

然后，像往常一样，我们使用 FROM 语句来标识我们的第一个表(在本例中，当然)。到目前为止，一切顺利。

接下来的几行变得有趣了。首先，我们通过使用 JOIN 关键字让 MySQL 知道我们想要做一个连接，然后是第二个表(Client)。我们必须指示 MySQL 哪个表包含共有的数据——不可能将两个没有共享数据的表连接在一起。

为此，我们使用 ON 关键字，后跟*table 1 . column = table 2 . column*，其中标识的列是包含公共数据的列(通常是一个表的主键和另一个表的外键)。

在我们的示例中，我们还使用了 WHERE 关键字来添加条件。我们只是在寻找不在国际语言学校进行的课程的详细信息和公司地址。这对我们的老师来说是有用的信息，这样他们就知道他们需要去哪里旅行！

也许我们想为我们的一位老师——在我们的例子中，让我们选择斯蒂芬妮·马丁——提供她必须在校外授课的班级的地址。使用连接，这很简单:

```
SELECT course.course_id, course.course_name, course.language, client.client_name, client.address
FROM course
JOIN client
ON course.client = client.client_id
WHERE course.in_school = FALSE AND course.teacher = 2;
```

![](img/cf7002ebac02dd832bacd157660d2e0b.png)

斯蒂芬妮只有一门课是在客户的办公室教的

## 复杂的事情——嵌套查询的连接

通过将连接与嵌套查询结合起来，我们可以进一步提高强度，从而从数据库中获得更具体的见解。如果我们想知道所有参加由 [Niamh](https://en.wikipedia.org/wiki/Niamh) Murphy 教授的课程的人的名字会怎么样？

使用两个连接和一个嵌套查询，我们可以立即从数据库中获得这些信息:

```
SELECT participant.first_name, participant.last_name
FROM participant
JOIN takes_course ON takes_course.participant_id = participant.participant_id 
JOIN course ON takes_course.course_id = course.course_id
WHERE takes_course.course_id = 
    (SELECT takes_course.course_id 
    WHERE course.teacher = 6);
```

![](img/b0c7338cd14f6e58c2a4f0b0ce2c2969.png)

瞧啊。

现在事情严重了！为了解构这一点，并了解更复杂的 SQL 查询中一般会发生什么，向后工作会非常有帮助(在构造自己的更复杂的查询时，这也是一个有用的技巧)。如果我们在这里这样做，我们可以看到我们有一个标识 course_id 的子查询，其中 teacher_id = 6 — Niamh 的 id 号。

```
(SELECT takes_course.course_idWHERE course.teacher = 6)
```

其结果将是 Niamh 教授的所有课程的 course_id 值，用于我们查询的 WHERE 条件中。因此，该条件可以用简单的英语表达为“该课程由 Niamh Murphy 教授”。

因为我们正在寻找的输出是参与者的名字，所以我们的最终目标是将课程表连接到参与者表。然而，由于这些表之间的关系是 N 对 M 的关系(一个参与者可以选修多门课程，一门课程可以由多个参与者选修)，我们不能简单地使用典型的 1 对 N 关系的外键关系。

相反，我们需要连接*到*我们创建的额外的 Takes_Course 表，以存储 Course 和 Participant 表之间的 N 对 M 关系。唷！

```
SELECT participant.first_name, participant.last_name
FROM participantJOIN takes_course 
ON takes_course.participant_id = participant.participant_idJOIN course 
ON takes_course.course_id = course.course_id
```

我们使用键 participant_id 将 Participant 表连接到 Takes_Course 表，然后立即使用 course_id 将其连接到 Course 表。这就是我们如何从存储在数据库中的这些更复杂的关系中提取信息。

![](img/b35d5a49fed2a7fc4a236daf2e0c1923.png)

起初，这似乎很复杂，有点令人生畏，但是如果我们花点时间思考一下，再看一下我们的 ERD，就会明白这就是我们如何解锁存储在这个结构中的数据的方法。整个过程完全符合逻辑，完全由我们放入查询中的代码驱动。

理解这一点的一个好方法是尝试编写我们自己的查询。看着这些数据，思考“我们怎样才能把这种联系从数据库中提取出来”，并尝试不同的组合。这总是习惯一项新技术的最好方法，尝试一下，看看会发生什么。另外，这是一个有趣的时间！

# 其他类型的连接

现在我们已经掌握了内部连接，让我们看看 MySQL 中不同种类的外部连接。当我们有一些信息没有包含在所有表中时，连接类型之间的差异就变得更加明显了。ILS 数据库被很好地组合在一起，但大部分包含完整的数据。遗憾的是，在现实生活中情况并非如此，我们需要知道如何应对。

这意味着是时候向我们的数据库添加一个新表了！激动人心的时刻！

国际语言学校的管理层正在进行一些市场调查。他们想知道新课程以哪些行业为目标，并咨询了商业出版社，整理了一份汇总表，展示了一些不同行业的预测前景。

![](img/929315d5865a837598b18a6b08a9fa92.png)

显然，现在不是物流的好时机

让我们将它与我们的客户端表进行比较。

![](img/5219b6fdf01df9abd3b36fdd9f4e6f8d.png)

当我们比较每个表中的行业列时，我们注意到有些行业同时出现在两个表中(零售、物流)，而有些则没有。

在我们继续之前，让我们将行业前景表添加到我们的数据库中。

```
CREATE TABLE industry_prospects (
  industry VARCHAR(20) PRIMARY KEY,
  outlook VARCHAR(20)
);INSERT INTO industry_prospects VALUES
('Retail', 'Good'),
('Hospitality', 'Poor'),
('Logistics', 'Terrible'),
('Tourism', 'Great'),
('Events', 'Good');
```

![](img/21b325284f7a6c65fe0f86014745302f.png)

成功！

## 内部联接

到目前为止，我们一直在处理内部连接。这种类型的连接遵循下面的文氏图。

![](img/6e8219a979d59903cbd1fc94f6baff20.png)

内部联接将只返回两个表都匹配的值，而不会返回只在一个表中正在执行联接的列中有值的任何记录。

因此，使用下面的代码对我们的客户和行业前景表执行内部连接，将得到两个表都有匹配条目的条目。

```
SELECT client.client_name, client.industry, industry_prospects.outlook
FROM client
JOIN industry_prospects
ON client.industry = industry_prospects.industry;
```

![](img/5750b8fd5f20f4e0bd81df043e7e332a.png)

因为零售和物流都包含在这两个表中，所以我们只得到匹配的条目，没有空值。

## 左连接

一个[左连接](https://www.w3schools.com/sql/sql_join_left.asp)将给出左表中的所有值，以及右表中的匹配值。如果右边的表中没有匹配的值，我们将在输出中得到空值。

![](img/e086bf4acae48137bfca631ed57bcb3d.png)

左连接的代码与我们的内连接代码相同，只是我们用左连接替换了连接，如下所示:

```
SELECT client.client_name, client.industry, industry_prospects.outlook
FROM client
LEFT JOIN industry_prospects
ON client.industry = industry_prospects.industry;
```

![](img/c31b6df20d578adfa41f97cb136dc0b3.png)

我们的输出现在包括来自客户端表的每个条目(它出现在我们的代码中的第一个位置，所以它是“左边”的表，但这纯粹是因为我们选择了对表进行排序的方式)。如果行业前景表中没有相应的条目，则返回空值。

当我们想从另一个表中引入数据，但又不想丢失主表中的任何数据时，左连接是很好的。这可能发生在大量的现实场景中，左连接是我们数据分析工具箱中的一个主要部分。

## 右连接

[右连接](https://www.w3schools.com/sql/sql_join_right.asp)是左连接的镜像，不常用。因为哪个表是左表，哪个是右表完全取决于编写查询的人，所以数据分析人员和查询编写人员倾向于将我们想要保存所有数据的表放在左边。但是，仍然有一些情况需要正确的连接，特别是在连接多个表的更复杂的查询中。

![](img/c7c5b4af973bffb42338328f780a442e.png)

为了完整起见，让我们在表上执行一个右连接。

```
SELECT client.client_name, client.industry, industry_prospects.outlook
FROM client
RIGHT JOIN industry_prospects
ON client.industry = industry_prospects.industry;
```

![](img/e6fba9e075583b86a4d6462f6d70ea07.png)

我们的输出现在包括了来自行业前景表的所有条目，正如所预期的那样，空值现在出现在来自客户表的数据中。

这只是左连接的镜像，我们可以简单地通过交换代码中`client`和`industry_prospects`的顺序来获得相同的输出。

还有另一种类型的连接，在其他 SQL 风格中也支持，即[完全外部连接](https://www.w3schools.com/sql/sql_join_full.asp)。这将合并左连接和右连接，并返回两个表中的所有条目，如果不匹配，则返回空值。

MySQL 中不支持这个，这里就不看了。它可以在 MySQL [中相对容易地被模拟](https://www.w3schools.com/sql/sql_join_full.asp)，这就是(人们推测)为什么开发者没有选择包含它的原因。如果我们最终使用另一种风格的 SQL，了解这一点对我们很有好处。所以现在我们知道了！

哇！在本文中，我们已经讨论了很多基础知识，从学习 SQL 查询的基础知识到构建具有多个连接的复杂嵌套查询。希望 SQL 在数据库中创建、检查、更新和删除数据的能力比开始时更加清晰。

关系数据库和 SQL 是处理大型数据集的一些最广泛使用的技术，尽管它们可能不像 R 或 Python 中的最新库那样流行，但对于工作数据分析师来说，它们是一个必不可少的组件，理解起来非常有用。

我们在这里看到的查询已经从相当基本的到仍然相当基本的，但是希望我们可以看到如何组合不同的条件和操作符可以让我们产生真正令人敬畏的结果。勇往直前，负责任地使用这种新的力量！

这是我的三篇系列文章中的最后一篇，介绍如何使用 SQL 和免费开源软件创建、更新和查询自己的全功能关系数据库。我们一起讨论了很多问题，我希望这对您有所帮助。

我希望听到您的反馈，如果您有任何意见或问题，请通过我的网站[联系我。如果你想了解我的项目，你可以在 Medium 上关注我，在](https://www.craigdoesdata.de/contact.html) [GitHub](https://github.com/thecraigd) 上关注我，或者访问我的[网站](https://www.craigdoesdata.de/)。

谢谢你花时间和我在一起，下次再见！

![](img/13abfd4d4dc63e09002660eca725f68a.png)

更像这样？访问[craigdoedata . de](https://www.craigdoesdata.de/)