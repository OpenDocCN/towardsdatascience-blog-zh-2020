# 保存日期

> 原文：<https://towardsdatascience.com/save-the-date-ad964b2397d0?source=collection_archive---------22----------------------->

## 如何在 SQL 查询中生成缺失日期

## 在本文中，我们将讨论:

1.  如何使用 Presto SQL 生成一个包含日期范围的表
2.  连接应该从另一个表中补齐缺失数据的表的经验法则。

在对数据进行计算之前，数据完整性是我们需要解决的最重要的事情之一。即使有正确的意图，我们有时也会忽略数据中的错误。当错误不在我们拥有的数据中，而是在我们没有的数据中时，这将变得非常困难。

当执行考虑数据中样本数量的计算(计算平均值或中值)时，我们需要处理值为 NULL 或零的行。

让我们假设我们经营一家网上商店，想要查看一个客户一个月内的平均日购买量。在客户没有购买的日期，我们的数据中不会有这种迹象。如果我们忽略这个问题，计算每个客户的平均购买量，我们会得到一个过高的估计。

```
 customer_id    | order_date | purchase_amount |
10000100005411274 | 2020-04-11 |        1        |
10000100005411274 | 2020-04-16 |        1        |
10000100005411274 | 2020-04-18 |        2        |
10000100005411274 | 2020-04-21 |        2        |
10000100005411274 | 2020-04-24 |        1        |
```

*如果我们在不看我们的原始数据的情况下计算客户的日均购买量，我们会认为他的平均购买量是 1.4(真是个客户！).*

为了解决这个问题，我们必须为所有客户生成并匹配所有日期。通过 Presto SQL，我们可以在一个简单的查询中做到这一点:

```
SELECT
     *CAST*(date_column AS DATE) date_column
 FROM
     (VALUES
         (SEQUENCE(*date*('2020-04-01'),
                   *date*('2020-04-30'),
                   INTERVAL '1' DAY)
         )
     ) AS t1(date_array)
 CROSS JOIN
     UNNEST(date_array) AS t2(date_column)
```

使用 SEQUENCE，我们将创建一个日期在我们范围内的数组，并在数组中的每个元素与数组本身之间执行一个交叉连接。结果是每个不同日期对应一列。

一种快速的替代方法是从我们的初始数据中提取所有不同的日期，而不考虑客户，并将其存储为 WITH AS 语句。

接下来，我们将执行另一个交叉连接，以匹配我们的客户和不同的日期，从而填充缺失的日期:

```
with all_dates as (
SELECT
     *CAST*(date_column AS DATE) date_column
 FROM
     (VALUES
         (SEQUENCE(*date*('2020-04-01'),
                   *date*('2020-04-30'),
                   INTERVAL '1' DAY)
         )
     ) AS t1(date_array)
 CROSS JOIN
     UNNEST(date_array) AS t2(date_column)
)
select distinct customer_id
               ,date_column as order_date
from customer_purchases
cross join all_dates
```

最后，我们将把客户和日期之间的新匹配加入到包含我们数据的初始表中。

***重要通知***

**强**表应该是我们用客户和日期创建的新表，而*左*连接表应该是我们的初始数据。如果我们执行内部连接，我们将丢失所有没有购买的日期。此外，我们需要确保不要在 where 子句中放置处理左连接表中的列的条件，这将把左连接变成内连接。

```
with all_dates as (
SELECT
     *CAST*(date_column AS DATE) date_column
 FROM
     (VALUES
         (SEQUENCE(*date*('2020-04-01'),
                   *date*('2020-04-30'),
                   INTERVAL '1' DAY)
         )
     ) AS t1(date_array)
 CROSS JOIN
     UNNEST(date_array) AS t2(date_column)
),customers_dates as (
select distinct customer_id
               ,date_column as order_date
from customer_purchases
cross join all_dates)

select u.customer_id
     , u.order_date
     , *coalesce*(p.purchase_amount,0) purchase_amount
from customers_dates u
left join customer_purchases p
on u.customer_id = p.customer_id
and u.order_date = p.order_date
order by customer_id asc-------------------------------------------------------------------- customer_id    | order_date | purchase_amount|
................. | .......... | .............  |
10000100005411274 | 2020-04-13 |       0        |
10000100005411274 | 2020-04-14 |       0        |
10000100005411274 | 2020-04-15 |       0        |
10000100005411274 | 2020-04-16 |       1        |
10000100005411274 | 2020-04-17 |       0        |
10000100005411274 | 2020-04-18 |       2        |
```

*现在我们可以用正确的方法计算平均每日购买量。*

## 总结(TL；博士):

1.使用 SEQUENCE，我们可以创建一个包含一系列日期的数组，并将它们转换成表格。这种方法对于填充数据中缺失的日期非常有效，可以确保*没有发生*的日期仍然会出现。

2.当在两个表之间执行左连接时，如果 where 子句中的条件寻址来自*左*表的列，则将左连接转换为**内连接**。人们很容易忽略这一点，当在同一个查询中使用表间连接应用条件时，应该三思而行。