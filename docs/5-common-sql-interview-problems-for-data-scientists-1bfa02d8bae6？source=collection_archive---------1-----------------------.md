# 数据科学家的 5 个常见 SQL 面试问题

> 原文：<https://towardsdatascience.com/5-common-sql-interview-problems-for-data-scientists-1bfa02d8bae6?source=collection_archive---------1----------------------->

## 帮助您发展 SQL 技能，在任何面试中胜出

![](img/51c97f8b7147a27f466039b578478308.png)

由[马库斯·斯皮斯克](https://unsplash.com/@markusspiske?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

虽然这并不是这份工作中最吸引人的部分，但对 SQL 有深刻的理解是在任何以数据为中心的工作中取得成功的关键。事实是，除了 SELECT FROM WHERE GROUP BY ORDER BY 之外，SQL 还有很多其他方式。你知道的函数越多，你就越容易操作和查询任何你想要的东西。

本文有两点希望学习和交流:

1.  学习和教授基本基础之外的 SQL 函数
2.  经历一些 SQL 面试练习问题

*这些问题不是别人，正是取自 Leetcode！如果你还没有，就去看看吧！*

# 目录

*   [问题 1:第二高的薪水](#2c3f)
*   [问题#2:重复的电子邮件](#4d7a)
*   [问题#3:温度上升](#ba30)
*   [问题#4:部门最高工资](#9987)
*   [问题#5:交换座位](#4995)

# 问题 1:第二高的薪水

*编写一个 SQL 查询，从* `*Employee*` *表中获取第二高的薪水。例如，给定下面的雇员表，查询应该返回* `*200*` *作为第二高的薪水。如果没有第二高的薪水，那么查询应该返回* `*null*` *。*

```
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```

## **解决方案 A:使用 IFNULL，OFFSET**

*   **IFNULL( *表达式，alt* )** : ifNULL()如果为 null 则返回指定值，否则返回预期值。如果没有第二高的薪水，我们将用它返回 null。
*   **OFFSET :** offset 与 ORDER BY 子句一起使用，忽略您指定的前 n 行。这将是有用的，因为你想得到第二排(第二高的薪水)

```
SELECT
    IFNULL(
        (SELECT DISTINCT Salary
        FROM Employee
        ORDER BY Salary DESC
        LIMIT 1 OFFSET 1
        ), null) as SecondHighestSalary
FROM Employee
LIMIT 1
```

## **解决方案 B:使用 MAX()**

这个查询说选择不等于最高工资的最高工资，相当于说选择第二高的工资！

```
SELECT MAX(salary) AS SecondHighestSalary
FROM Employee
WHERE salary != (SELECT MAX(salary) FROM Employee)
```

# 问题 2:重复的邮件

*编写一个 SQL 查询，在一个名为* `*Person*` *的表中查找所有重复的电子邮件。*

```
+----+---------+
| Id | Email   |
+----+---------+
| 1  | a@b.com |
| 2  | c@d.com |
| 3  | a@b.com |
+----+---------+
```

## **解决方案 A:子查询中的 COUNT()**

首先，创建一个子查询来显示每封电子邮件的频率计数。然后在计数大于 1 时过滤子查询。

```
SELECT Email
FROM (
    SELECT Email, count(Email) AS count
    FROM Person
    GROUP BY Email
) as email_count
WHERE count > 1
```

## 解决方案 B: HAVING 子句

*   **HAVING** 是一个子句，本质上允许您将 WHERE 语句与 aggregates (GROUP BY)结合使用。

```
SELECT Email
FROM Person
GROUP BY Email
HAVING count(Email) > 1
```

# 问题#3:温度上升

*给定一个* `*Weather*` *表，编写一个 SQL 查询来查找与前一个(昨天的)日期相比温度更高的所有日期的 id。*

```
+---------+------------------+------------------+
| Id(INT) | RecordDate(DATE) | Temperature(INT) |
+---------+------------------+------------------+
|       1 |       2015-01-01 |               10 |
|       2 |       2015-01-02 |               25 |
|       3 |       2015-01-03 |               20 |
|       4 |       2015-01-04 |               30 |
+---------+------------------+------------------+
```

## 解决方案:DATEDIFF()

*   **DATEDIFF** 计算两个日期之间的差异，并用于确保我们将今天的温度与昨天的温度进行比较。

简单地说，该查询是说，选择某一天的温度高于昨天温度的 id。

```
SELECT DISTINCT [a.Id](http://a.id/)
FROM Weather a, Weather b
WHERE a.Temperature > b.Temperature
AND DATEDIFF(a.Recorddate, b.Recorddate) = 1
```

# 问题 4:部门最高工资

*`*Employee*`*表包含所有员工。每个雇员都有一个 Id，一份薪水，还有一个部门 Id 列。**

```
*+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 70000  | 1            |
| 2  | Jim   | 90000  | 1            |
| 3  | Henry | 80000  | 2            |
| 4  | Sam   | 60000  | 2            |
| 5  | Max   | 90000  | 1            |
+----+-------+--------+--------------+*
```

**`*Department*`*表包含公司的所有部门。***

```
**+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+**
```

**编写一个 SQL 查询来查找每个部门中工资最高的雇员。对于上面的表，您的 SQL 查询应该返回下面的行(行的顺序无关紧要)。**

```
**+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| IT         | Jim      | 90000  |
| Sales      | Henry    | 80000  |
+------------+----------+--------+**
```

## ****解决方案:在条款**中**

*   **子句中的**允许您在 WHERE 语句中使用多个 OR 子句。例如，country = 'Canada '或 country = 'USA '与 WHERE country IN ('Canada '，' USA ')相同。****
*   **在这种情况下，我们希望筛选 Department 表，只显示每个部门的最高工资(即 DepartmentId)。然后我们可以连接两个表，其中 DepartmentId 和 Salary 位于筛选的 Department 表中。**

```
**SELECT
    Department.name AS 'Department',
    Employee.name AS 'Employee',
    Salary
FROM Employee
INNER JOIN Department ON Employee.DepartmentId = Department.Id
WHERE (DepartmentId , Salary) 
    IN
    (   SELECT
            DepartmentId, MAX(Salary)
        FROM
            Employee
        GROUP BY DepartmentId
 )**
```

# **问题 5:交换座位**

**玛丽是一所中学的老师，她有一张表 `*seat*` *存储着学生的名字和他们相应的座位号。列* ***id*** *为连续增量。玛丽想给相邻的学生换座位。***

**你能写一个 SQL 查询来为 Mary 输出结果吗？**

```
**+---------+---------+
|    id   | student |
+---------+---------+
|    1    | Abbot   |
|    2    | Doris   |
|    3    | Emerson |
|    4    | Green   |
|    5    | Jeames  |
+---------+---------+**
```

***对于样本输入，输出为:***

```
**+---------+---------+
|    id   | student |
+---------+---------+
|    1    | Doris   |
|    2    | Abbot   |
|    3    | Green   |
|    4    | Emerson |
|    5    | Jeames  |
+---------+---------+**
```

*****注:*** *如果学生人数为奇数，则无需换最后一个座位。***

## **解决方案:CASE WHEN**

*   **想一个例子，当 THEN 语句类似于代码中的 IF 语句时。**
*   **第一个 WHEN 语句检查是否有奇数行，如果有，确保 id 号不变。**
*   **第二个 WHEN 语句为每个 id 加 1(例如 1，3，5 变成 2，4，6)**
*   **类似地，第三个 WHEN 语句将每个 id 减 1(2，4，6 变成 1，3，5)**

```
**SELECT 
    CASE 
        WHEN((SELECT MAX(id) FROM seat)%2 = 1) AND id = (SELECT MAX(id) FROM seat) THEN id
        WHEN id%2 = 1 THEN id + 1
        ELSE id - 1
    END AS id, student
FROM seat
ORDER BY id**
```

**就是这样！如果有任何不清楚的地方，请在下面评论，我将尽我所能澄清任何事情——谢谢！**

# **感谢阅读！**

**如果你喜欢我的工作，想支持我…**

1.  **支持我的最好方式就是在**媒体**T2 上关注我。**
2.  **在**推特** [这里](https://twitter.com/terence_shin)成为第一批关注我的人之一。我会在这里发布很多更新和有趣的东西！**
3.  **此外，成为第一批订阅我的新 **YouTube 频道** [这里](https://www.youtube.com/channel/UCmy1ox7bo7zsLlDo8pOEEhA?view_as=subscriber)！**
4.  **在 **LinkedIn** [这里](https://www.linkedin.com/in/terenceshin/)关注我。**
5.  **在我的**邮箱列表** [这里](https://forms.gle/UGdTom9G6aFGHzPD9)报名。**
6.  **查看我的网站，[**terenceshin.com**](https://terenceshin.com/)。**

# **更多相关文章**

**[](/40-statistics-interview-problems-and-answers-for-data-scientists-6971a02b7eee) [## 数据科学家的 40 个统计面试问题和答案

### 为你的面试复习统计知识的资源！

towardsdatascience.com](/40-statistics-interview-problems-and-answers-for-data-scientists-6971a02b7eee) [](/amazon-data-scientist-interview-practice-problems-15b9b86e86c6) [## 亚马逊的数据科学家面试实践问题

### 一些亚马逊面试问题的演练！

towardsdatascience.com](/amazon-data-scientist-interview-practice-problems-15b9b86e86c6) [](/microsoft-data-science-interview-questions-and-answers-69ccac16bd9b) [## 微软数据科学面试问答！

### 微软面试中一些数据科学问题的演练

towardsdatascience.com](/microsoft-data-science-interview-questions-and-answers-69ccac16bd9b) [](/more-microsoft-data-science-interview-questions-and-answers-f9ee8337072c) [## 更多微软数据科学面试问题和答案

### 微软面试中一些数据科学问题的另一个演练

towardsdatascience.com](/more-microsoft-data-science-interview-questions-and-answers-f9ee8337072c) [](/googles-data-science-interview-brain-teasers-7f3c1dc4ea7f) [## 谷歌的数据科学面试脑筋急转弯

### 作为谷歌数据科学面试的一部分，他们喜欢问一些他们称为“解决问题”的问题…

towardsdatascience.com](/googles-data-science-interview-brain-teasers-7f3c1dc4ea7f)**