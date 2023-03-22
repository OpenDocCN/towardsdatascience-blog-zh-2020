# 基于 SQL 顺序的计算

> 原文：<https://towardsdatascience.com/sql-order-based-calculations-b924e79401a0?source=collection_archive---------48----------------------->

## SQL 如何解决这种困难但常见的计算

![](img/7452b68d8007004a5e19bb286c878cba.png)

*作者图片*

SQL 中字段值的联合很常见，比如 firstname+lastname 和 year(生日)。无论一个表达式包含多少个字段，它们都来自同一行。我们称之为**行内计算**。

相应的，还有**行间计算**。例子包括获得冠军和亚军的结果之间的差异，以及计算从 1 月到当月的累计销售额。为了确定冠军和亚军，需要根据结果对数据进行排序。从某一点到另一点的累加和也要求数据是有序的。所以我们称它们为**基于顺序的计算**。行内计算处理单个记录中的值，而行间计算处理有序记录之间的差异。

# 引用上一条/下一条记录中的值

最简单也是最常见的基于顺序的计算是，当记录已经按一定顺序排序时，引用上一条或下一条记录中的值。下面是三种情况:

**1。计算一只股票每天的增长率(链接相对比率)**

按日期排序记录，并参考前一天的收盘价。

**2。计算一只股票三天内的平均价格，这三天分别是前一天、当天和第二天(移动平均线)**

按日期排序记录，并参考前一天和第二天的收盘价。

**3。有多只股票。计算每个交易日每只股票的涨幅(集团内环比)**

按股票分组记录，按日期对每组排序，参考前一天的收盘价。

现在让我们看看 SQL 如何处理这种基于订单的计算。

# 早期的 SQL 解决方案

早期的 SQL 没有窗口函数。为了引用相邻记录中的值，该语言将两个记录合并成一个记录。

下面是处理任务 1 的程序:

```
SELECT day, curr.price/pre.price rate
FROM (
  SELECT day, price, rownum row1
  FROM tbl ORDER BY day ASC) curr
LEFT JOIN (
  SELECT day, price, rownum row2
  FROM tbl ORDER BY day ASC) pre
ON curr.row1=pre.row2+1
```

通过当天和前一天对表执行自联接，将前一天的收盘价和当天的收盘价放入一条记录中，然后执行行内计算以获得增长率。您可以看到子查询用于一个简单的任务。

SQL 也使用 JOIN 计算任务 2 中的移动平均值:

```
SELECT day, (curr.price+pre.price+after.price)/3 movingAvg
  FROM (
    SELECT day, price, rownum row1
    FROM tbl ORDER BY day ASC) curr
  LEFT JOIN (
    SELECT day, price, rownum row2
    FROM tbl ORDER BY day ASC) pre
  ON curr.row1=pre.row2+1
  LEFT JOIN (
    SELECT day, price, rownum row3
    FROM tbl ORDER BY day ASC) after
  ON curr.row1=after.row3-1
```

又一个子查询将被连接一天。想象一下获取过去 10 天和未来 10 天的移动平均值的程序。写 20 个 JOINs 肯定会烦死你。

任务 3 更复杂。由于有多只股票，SQL 添加了一个代码列来区分不同的股票。因此，增长率是在一组库存记录中计算的:

```
SELECT code, day ,currPrice/prePrice rate
  FROM(
    SELECT code, day, curr.price currPrice, pre.price prePrice
    FROM (
      SELECT code, day, price, rownum row1
      FROM tbl ORDER BY code, day ASC) curr
    LEFT JOIN (
      SELECT code, day, price, rownum row2
      FROM tbl ORDER BY code, day ASC) pre
    ON curr.row1=pre.row2+1 AND curr.code=pre.code
  )
```

有两点我想说一下。您必须使用“code，day”对表进行复合排序。代码走在前面，因为您需要首先将相同股票的记录放在一起，然后对它们进行排序。您还需要在连接条件中加入代码匹配，因为如果您不这样做，增长率将在不同股票的相邻记录之间进行计算。那会产生无用的脏数据。

# 具有窗口功能的解决方案

SQL 2003 引入了窗口函数来表达顺序的概念。在 SQL 中实现基于顺序的计算更容易。以上三项任务可以通过更简单的方式实现:

以下程序计算任务 1 中的链路相对比率。为了更容易理解，我把窗口函数写成几个缩进:

```
SELECT day, price /
    LAG(price,1)
      OVER (
        ORDER BY day ASC
      ) rate
 FROM tbl
```

LAG 函数实现对前一条记录的引用。它的两个参数找到了直接在它之前的记录的价格。OVER 是 LAG 函数中的 substatement。每个窗口函数都有一个 OVER 语句。它的作用是定义一个待分析的有序集。在这个例子中，要分析的数据集已经按日期排序。

下面的程序在任务 2 中计算移动平均值。一种方法是用 LAG 函数获取前一个值，然后用 LEAD 函数获取下一个值。另一种方法是这个程序使用的 AVG 函数，这是更可取的。AVG 函数可以一次得到指定范围内的平均值，例如覆盖前 10 条记录和后 10 条记录的平均值，而滞后/超前函数一次只能得到一个值。

```
SELECT price,
    AVG(price) OVER (
      ORDER BY day ASC
      RANGE BETWEEN 1 PRECEDING AND 1 FOLLOWING
    ) movingAvg
FROM tbl;
```

这样，通过覆盖前面的 n 条记录和后面的 n 条记录，更容易得到一个移动平均值。你只需要在 t 之间改变 range 定义的 RANGE 参数。

该程序执行任务 3 中要求的基于组内订单的计算。将同一只股票的所有收盘价记录放在一个组中，这可以通过 window 函数实现。

```
SELECT code, day, price /
    LAG(price,1)
      OVER (
        PARTITION BY code
        ORDER BY day ASC
      ) rate
FROM tbl
```

OVER 函数中的 PARTITION BY substatement 定义了记录分组的方式，并限制了组中的每个 LAG 操作。比之前的 JOIN 方法更直观。JOIN 方法按多个字段对记录进行排序，这相当于 PARTITION BY，但很难理解。

# 按序列号排列的位置

它是一个相对位置，用来获取有序集合中相邻记录的值。有时，我们需要找到记录的绝对位置，因为计算每天的发行价和收盘价之间的差异需要:

```
SELECT day, price-FIRST_VALUE(price) OVER (ORDER BY day ASC) FROM tbl
```

或者计算第 10 个交易日的最高收盘价与每天收盘价之间的差额，需要:

```
SELECT day, price-NTH_VALUE(price,10)OVER (ORDER BY day ASC) FROM tbl
```

还有更复杂的情况，其中用于定位记录的序列号是未知的，并且需要从现有值中生成:

**4。有按收盘价排序的股票记录，我们想找到中间位置的价格(中位数)**

让我们从简单的一只股票开始。记录按价格排序后，我们还是不知道中间的地方在哪里。所以我们需要根据记录的数量来计算中间位置的序号:

```
SELECT *
  FROM
    SELECT day, price, ROW_NUMBER()OVER (ORDER BY day ASC) seq FROM tbl
  WHERE seq=(
    SELECT TRUNC((COUNT(*)+1)/2) middleSeq FROM tbl)
```

FROM 语句中的子查询使用 ROW_NUMBER()为每一行生成一个序列号。WHERE 语句中的另一个子查询获取中间位置的序列号。这个 SQL 查询中有两点值得您注意。一个是不能直接在第一个子查询上执行筛选，因为 WHERE 子句不能使用同级 SELECT 子句的计算字段。这是由 SQL 执行顺序决定的。另一个是 WHERE 子句中的子查询的结果是一个一列一行的表，表中有一个值。它可以被视为要与 Seq 进行比较的单个值。

下面是获取多只股票记录的中位数的 SQL 程序:

```
SELECT *
  FROM
    (SELECT code, day, price,
      ROW_NUMBER() OVER (PARTITION BY code ORDER BY day ASC)seq  FROM tbl) t1
    WHERE seq=(
      SELECT TRUNC((COUNT(*)+1)/2) middleSeq
      FROM tbl t2
      WHERE t1.code=t2.code)
```

除了在窗口函数中用嵌入**分区外，在计算中间位置的序号时，要确保查询条件是为一只股票设置的。**

**5。计算最高收盘价当天与前一天相比的涨幅**

需要两个排序操作来定位收盘价最高的记录。让我们从一只股票开始:

```
SELECT day, price, seq, rate
  FROM (
    SELECT day, price, seq,
      price/LAG(price,1) OVER (ORDER BY day ASC) rate
    FROM (
      SELECT day, price,
        ROW_NUMBER ()OVER (ORDER BY price DESC) seq
      FROM tbl)
    )
  WHERE seq=1
```

连续的两级子查询将有用的新数据添加到原始表中。ROW_NUMBER 按升序排列收盘价。滞后函数计算每天的增长率。最后，我们通过过滤操作得到收盘价最高的那一天(seq=1)。

过滤操作应该在计算增长率之后进行，因为如果过滤掉收盘价最高的前一天，就无法计算增长率。

# 基于订单的分组

该顺序在执行分组时也很有用。这里有一个例子:

**6。求一只股票连续上涨的最大交易日数。**

有点复杂。逻辑是这样的:通过将连续上升的股票记录放入同一组，将按日期排序的股票记录分成若干组。也就是说，如果一个记录的收盘价高于前一个记录的收盘价，那么就把它们归入同一组；如果一个记录的收盘价比前一个低，那么把它放入一个新的组。所有记录分组后，对每组中的记录进行计数，得到最大计数，这就是我们想要的结果。

这种类型的分组是根据记录的顺序执行的。因为 SQL 只支持等分组，所以它需要将基于顺序的分组转换为等分组。它是这样做的:

1)按日期对记录进行排序，并为每个日期获取前一个日期的收盘价；

下面是完成这项工作的完整 SQL 程序:

```
SELECT MAX(ContinuousDays)
  FROM (
    SELECT COUNT(*) ContinuousDays
    FROM (
      SELECT SUM(RisingFlag) OVER (ORDER BY day) NoRisingDays
      FROM (
        SELECT day, CASE WHEN price>
           LAG(price) OVER (ORDER BY day) THEN 0 ELSE 1 END RisingFlag
      FROM tbl
      )
    ) GROUP BY NoRisingDays
  )
```

这个 SQL 解决方案包括 4 级子查询。与 Java 和 C 语言不同，SQL 是基于集合的。它提供了直接在集合上工作的方法，而不需要显式可控的循环操作和临时中间变量。与直观的思维方式不同，SQL 实现采用迂回的方式，使用标准的集合操作来获得结果。然而，Java 或 C 语言通过循环处理每个记录更接近我们的自然思维方式。生成新组或向现有组追加数据是很直观的。但是它们不支持集合运算。从这个角度来看，SQL 和 Java/C 各有利弊。

真实世界的计算场景可能比您想象的更复杂:

7 .**。找出连续上涨 3 天的股票。**

这个场景需要基于顺序的分组、对分组后子集的操作、标准分组和 HAVING 子句。首先，我们使用前面查询任务中的实现方法获得每只股票的所有上涨组，用一个分组操作将其括起来，以计算连续上涨的最大天数，然后通过 HAVING 子句找到连续上涨 3 天的股票:

```
SELECT code, MAX(ContinuousDays)
  FROM (
    SELECT code, NoRisingDays, COUNT(*) ContinuousDays
    FROM (
      SELECT code,
      SUM(RisingFlag) OVER (PARTITION BY code ORDER BY day) NoRisingDays
      FROM (
        SELECT code, day,
          CASE WHEN price>
            LAG(price) OVER (PARTITION BY code ORDER BY day)
          THEN 0 ELSE 1 END RisingFlag
        FROM tbl
      )
    ) GROUP BY NoRisingDays
  )
  GROUP BY code
  HAVING MAX(ContinuousDays)>=3
```

SQL 程序几乎难以理解。

在引入窗口函数之前，SQL 在处理基于顺序的计算时非常笨拙(即使现在一些数据库仍然不支持窗口函数)。理论上，它可以管理所有的场景，但实际上所有的都不算什么，因为实现太复杂了。窗口函数极大地改善了 SQL 的困境，尽管它在处理复杂场景时仍然迂回曲折。

SQL 的问题根源于它的理论基础——基于无序集的关系代数。窗口函数是有用的，但不能解决根本问题。

实际上，计算语言中的数组(集合)是自然有序的(它们有自然的序列号)。用 Java 和 C++这样的高级语言很容易理解和实现这个特性。问题是他们处理集合运算的能力很弱。这意味着它们也会产生冗长的程序来处理基于订单的计算(尽管逻辑并不复杂)。

埃斯普罗克·SPL 就是这样一位出色的选手。esProc 是一个专业的数据计算引擎。它以有序集合为基础，为执行集合运算提供全面的功能。它继承了 Java 和 SQL 的优点。在 SPL 进行基于订单的计算非常容易，例如，SPL 有自己简单的解决方案:

```
1． T.sort(day).derive(price/price[-1]:rate)2． T.sort(day).derive(avg(price[-1:1]):movingAvg)3． T.sort(day).group(code).(~.derive(price/price[-1]:rate)).conj()4． T.sort(price).group(code).(~((~.len()+1)\2))5． T.sort(day).group(code).((p=~.pmax(price),~.calc(p,price/price[-1])))6． T.sort(day).group@o(price >price[-1]).max(~.len()))7． T.sort(day).group(code).select(~.group@o(price>price[-1]).max(~.len())>3).(code)
```

SPL 提供了实现跨行引用的语法，并为上述所有计算提供了坚实的支持。它使程序员能够以直观、简单和优雅的方式表达逻辑。