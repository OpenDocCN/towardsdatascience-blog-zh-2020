# 让您的查询更漂亮、更易读的 5 个 SQL 技巧

> 原文：<https://towardsdatascience.com/5-sql-tips-to-make-your-queries-prettier-and-easier-to-read-d9e3a543514f?source=collection_archive---------9----------------------->

## 适当的查询可以让你省心。

![](img/de6c0161ba6cc84096268a75f3aeffc3.png)

照片由[阿里·耶尔马兹](https://unsplash.com/@zamansizturk?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# **如果我的查询已经可以运行了，为什么我还需要把它变得漂亮呢？**

嗯，SQL 查询可以被认为是普通的代码。即使它可以工作，但这可能还不够，肯定可以通过应用一些格式标准来改进。更漂亮的代码易于分析和维护。同样的原则也适用于 SQL 查询。

## 定时和误差

您公司的同事必须检查您的工作，他将很难理解您试图用一个格式错误的查询做什么。

使用错误的或没有样式的规则将会导致同行评审的时间变慢，进入生产的时间变慢，更多的错误漏过，并且普遍缺乏评审你的工作的动力。当客户或同事要求您更改几个月前的查询时，糟糕的格式和样式也会产生影响。你完全忘记了那个巨大的查询是关于什么的。

![](img/7bfbce32bc985f14b7c3f3524c11bff8.png)

照片由 [Tim Gouw](https://unsplash.com/@punttim?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

我和我的同事遇到的几乎所有奇怪的**语法错误**都主要是由于糟糕的格式造成的**，因为很难发现错误放置的逗号或丢失的括号——这些是最糟糕的。**

开始吧！

## **与套管一致**

查询的不同部分可以有不同的大小写，但是要一致。看看这个:

```
SELECT firstName, count(*) from Users WHERE last_name = ‘smith’ Group By firstName
```

这个查询是**绝对讨厌的**😬。如果我是解析器，我肯定会抛出某种语法错误来修改这个查询。

让我们在这里标记一些不好的事情:

1.  一些 SQL 关键字是大写的。有些是小写的。分组依据在标题中。关键字的最佳格式规则是**对所有 SQL 关键字使用相同的大小写**。小写和大写选项都是最常用的选项。
2.  不容易区分小写关键字实际上是关键字还是列，因为大小写是混合的。除此之外，看书也很烦。
3.  SELECT 中的第一列在 camelCase 中，WHERE 子句中使用的列在 snake_case 中，表名在 PascalCase 中。如果您在表格中遇到这种不一致的情况，一定要敲响警钟，并与您的团队一起决定您应该坚持哪种大小写。

经过一些清理后，查询应该如下所示:

```
SELECT first_name, COUNT(*) FROM users WHERE last_name = ‘smith’ GROUP BY first_name
```

## **使用缩进**

试着阅读下面的查询，快速浏览一下，区分每个部分:

```
SELECT g.id, COUNT(u.id) FROM users u JOIN groups g on u.group_id = g.id WHERE u.name = 'John' GROUP BY g.id ORDER BY COUNT(u.id) desc
```

现在，用下面的一个来尝试同样的练习:

```
SELECT g.id, COUNT(u.id)
FROM users u JOIN groups g on u.group_id = g.id
WHERE u.name = 'John'
GROUP BY g.id
ORDER BY COUNT(u.id) desc
```

你能看出区分每个部分有多容易吗？

这可能是这个查询的一个小变化，但是想象一下必须处理那些有子查询、多个连接甚至窗口函数的查询。这是不一样的，不是吗？

让我们尝试添加另一个级别的缩进，看看它如何变得更容易:

```
SELECT
    g.id
  , COUNT(u.id)
FROM users u
    JOIN groups g on u.group_id = g.id
WHERE u.name = ‘John’
GROUP BY
    g.id
ORDER BY
    COUNT(u.id) desc
```

> 专业提示:我开始在 SELECT 子句中的每个字段前使用逗号。我确保从第二列开始的每一行都以逗号开头，而不是在每一行后面都有一个逗号。在我看来，它更容易阅读，而且你不会有在 FROM 子句前留下逗号的问题，这有时会给你带来难以理解的语法错误，这取决于你的 RDBMS。

对于不同级别的查询，使用 2 个空格还是 4 个空格可能会有一点偏好，但你不能说它看起来不会比一大行 SQL 更干净。或者更糟:同一缩进层次上的一大块 SQL。

相信我，你的同事会感激你的。😄

## **在 group by 和 order by 子句中使用数字**

同样，这可能是一种偏好，你可以随意接受或不接受。但是在我的职业生涯中，我遇到了许多新手数据分析师，他们甚至不知道这个东西的存在。

而不是:

```
SELECT
    first_name
    , last_name
    , COUNT(*)
FROM users
GROUP BY
    first_name
    , last_name
ORDER BY
    COUNT(*) desc
```

您可以:

```
SELECT
    first_name
    , last_name
    , COUNT(*)
FROM users
GROUP BY 1, 2
ORDER BY 3 desc
```

这带来了一系列简单的好处:

*   **您节省了行**:按大量字段分组不仅会在 SELECT 子句中添加更多的行，还会在 GROUP BY 和 ORDER BY 子句中添加更多的行，甚至可能会使查询中的行数翻倍。
*   **可维护性**:如果您想将列更改为 group by，只需在 select 子句中这样做，不需要记住在其他子句中也要这样做。
*   **更快:**如果你有把分组字段放在第一位的好习惯，你唯一需要做的就是按 1，2，3，…，n 分组，n 为分组列的数量。
*   **兼容:**如果你使用类似 [dbt](https://www.getdbt.com) 的工具，你甚至可以使用 Jinja 来调用{{group_by(n)}}，它会翻译成适当的 group by 子句。

## **使用常用表表达式**

所谓的 cte 是用来简化复杂查询的工具。它们可以被定义为临时视图，因为它们只存在于整个查询的执行过程中。cte 使您能够创建将用于最终目的的子查询。

让我们看一个简单的例子:

```
WITH employee_by_title_count AS (
    SELECT
        t.name as job_title
        , COUNT(e.id) as amount_of_employees
    FROM employees e
        JOIN job_titles t on e.job_title_id = t.id
    GROUP BY 1
),
salaries_by_title AS (
     SELECT
         name as job_title
         , salary
     FROM job_titles
)
SELECT *
FROM employee_by_title_count e
    JOIN salaries_by_title s ON s.job_title = e.job_title
```

这里你可以看到我们在底层查询中使用了`employee_by_title`和`salaries_by_title`来产生最终结果。这比在 SELECT 子句中执行子查询或直接在 FROM 子句中执行子查询更具可读性和可维护性。特别是，如果您打算使用多个 cte，那么创建简单的 cte 要比创建一大块包含不同级别查询的 FROM 子句容易得多。

不过，你最好小心点:确保你不会因为做了比应该做的更复杂的事情而显著降低性能。要检查查询性能，如果使用 PostgreSQL，可以使用 EXPLAIN ANALYZE。

## 使用别名，有描述性

每个专栏的恰当标题至关重要。这是人们在电子表格中看到的第一件事。" W *我的帽子列有*？"想象一下，如果这个人有一个 csv，其中 3 列名为`id`，4 列名为`count`，其他列名为`avg`、`min`等等。他可能会发疯的！

让我们看看下面的查询:

```
SELECT
    u.id
    , u.name
    , t.id
    , t.name
    , (SELECT COUNT(*) FROM job_titles where name = t.name)
FROM users u
    JOIN job_title t on u.job_title_id = t.id
```

该查询的结果可能如下:

```
id, name, id, name, count
1, John Wick, 4, hitman, 5
```

你大概可以看出*疾速追杀*不是一个职位，所以第二列肯定是用户名。对于职业杀手来说也是如此，你可以肯定地说那是一个头衔。但是什么是`count`？😅谁也猜不到。

![](img/0797e7ad21c5e7b8799384e91a6c983e.png)

[布鲁斯·马尔斯](https://unsplash.com/@brucemars?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

现在让我们看看这个:

```
SELECT
    u.id as user_id
    , u.name as user_name
    , t.id as job_title_id
    , t.name as job_title_name
    , (SELECT COUNT(*) FROM job_titles where name = t.name) as count_users_with_job
FROM users u
    JOIN job_title t on u.job_title_id = t.id
```

这个查询将产生可读性更好、更容易理解的列。收到这份报告的人一眼就能发现有价值的信息，并且会比收到前一份报告的人快乐得多。

# 结论

我的目的是通过这篇文章让你的生活更轻松。无论您是开发人员、数据分析师、数据科学家，还是您公司中的 SQL 人员，这都会派上用场，减少您工作中的许多麻烦。

记住:**对于 SQL** 格式来说，没有单一的最佳标准或布局，但是肯定会有一个你最喜欢的**。现在的挑战是找到那个特定的，并确保你和它保持一致。与你的同事讨论他们喜欢什么样的格式规则，并与团队达成**共识**，以确保你们都同意在公司的所有查询中使用相同的规则。**

**我希望你觉得这很有用。下次见！😄**