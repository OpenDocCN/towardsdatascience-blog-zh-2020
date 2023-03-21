# data.table: R 的最佳数据对象

> 原文：<https://towardsdatascience.com/data-table-rs-best-data-object-c95b7d5f0104?source=collection_archive---------57----------------------->

## data.frame 太 2016 了

data.table 是我更喜欢用 R 而不是 Python 工作的主要原因。尽管 R 是一种众所周知的慢语言，但 data.table 通常比 Python 的 pandas ( [benchmark](https://h2oai.github.io/db-benchmark/) )运行得更快，只要 R 能够处理如此大规模的数据，它甚至可以与 Spark 一较高下。更好的是，data.table 非常简洁，只需要一点点输入就可以支持像窗口函数这样的复杂操作。

我相信 **data.table 是最重要的 R 包**。任何形式的争论最好使用 data.table。它是我们需要的数据超级英雄。

![](img/05bb4dfb3e38e74970c53df30cfca240.png)

[来源](https://www.needpix.com/photo/download/271646/superhero-girl-speed-runner-running-lights-space-cyber-suit)

除了函数不接受 data.table .我个人不喜欢 tidyverse(抱歉！)因为它使用 data.frame 和 tibble，因此运行起来比较慢——需要两倍的打字量。(最近 Wickham 发布了 [dtplyr](https://www.tidyverse.org/blog/2019/11/dtplyr-1-0-0/) ，它将 dplyr 代码翻译成 data.table 操作。你得到 dplyr 语法和数据表速度，如果那是你的事情。)

本文将指导您如何使用 data.table，以及让您更快更轻松地处理数据的提示和技巧。

# 构建数据表

除了使用 read.csv()之类的方法，您还可以使用 fread()将数据直接加载到 data.table 中。一旦有了大小适中的数据，就可以看到 fread()比 read.csv()快多少。

您也可以使用 data.table()函数构造一个，就像您对 data.frame()所做的那样。请注意，字符串不再自动转换为因子。

如果您已经有一个 data.frame，data.table()函数将转换它。

如果您的数据被分割成多个文件，您可以将每个文件加载到它自己的 data.table 中，存储到一个列表中，然后使用 rbindlist()。这类似于 do.call("rbind "，……)，但效率更高。

```
library(data.table)dt <- fread('mydata.csv')
dt <- data.table(x = 1:10, y = 1:10)
dt <- data.table(mydataframe)templist <- list()
for(i in 1:length(filenames)){
  templist[[i]] <- fread(filenames[i])
}
dt <- rbindlist(templist)
```

# 类似 SQL 的语法

SQL 是事实上的数据语言(还记得 NoSQL 被说成“不仅仅是 SQL”吗？)并且应该为大多数数据专业人员所熟悉。如果能写 SQL，就能写数据，表语法:

```
FROM[WHERE, SELECT, GROUP BY][ORDER BY]
```

假设您想要运行以下查询:

```
SELECT 
  col1, 
  count(1) as num_obs, 
  sum(col2) AS group_sum
FROM mydt
WHERE col3 > 0
GROUP BY col1
ORDER BY 2 DESC
```

在 data.table 中，这变成了

```
mydt[
  col3 > 0, 
  .(num_obs = .N,
    group_sum = sum(col2)), 
  by = col1
][
  order(-num_obs)
]
```

就是这样！最少的输入，不需要管道传输大量的函数。这确实需要一些时间来适应，但一旦你掌握了窍门，这就很有意义了。

的。()是 list()的简称。在 SELECT 语句中使用它将返回一个 data.table(将在下一节中解释)。

有了 data.table，您在引用字段时不再需要重复键入表名。使用 data.frame 您需要键入

```
mydt[mydt$col3 > 0]
```

但是 data.table 知道方括号内的内容可能是指它的列，所以您可以输入

```
mydt[col3 > 0]
```

这看起来并不多，但当您键入数百行代码时，这是生活质量的巨大提高。

# 快速转化和变异

SELECT 语句采用两种形式之一:

*   。()返回一个新的数据表
*   :=就地修改数据表

来自的输出。除非创建新的对象，否则()不会存储在任何位置。对于示例任务，data.table X 有两列:grp(分组变量)和 val(数值)。

任务 1:创建所有值的平均值的标量。

```
X[,mean(val)]
```

任务 2:创建一个包含组均值的新 data.table X_agg。

```
X_agg <- X[,.(avg = mean(val)), by = grp]
```

任务 3:在 X 中创建一个包含组平均值的新列。

```
X[,avg := mean(val), by = grp]
```

任务 4:在 X 中创建新列，说明观察值是否大于组平均值。

```
X[,is_greater := val > mean(val), by = grp]
```

任务 5:在 X 中为组 avg 和 stdev 创建两个新列。

```
X[,
  ':='(avg = mean(val), stdev = sd(val)),
  by = grp
]
```

有时，您有许多列需要按两个字段分组，grp1 和 grp2。您可以:

```
X_agg <- X[,lapply(.SD, mean), by = .(grp1, grp2)]
```

的。SD 是除用于分组的列之外的所有列的简写。注意所有这些任务的代码有多短！

# 快速搜索和加入

与 data.frame 不同，data.table 支持使用 setkey()函数进行索引:

```
X <- data.table(
  grp = sample(1:10),
  xval = rnorm(10)
)
Y <- data.table(
  grp = sample(rep(1:10, 2)),
  yval = rnorm(20)
)
setkey(X, grp)
setkey(Y, grp)
X[Y]
```

最后一行 X[Y]使用指定的索引合并两个表，比使用 base merge()要快得多。当您希望查找特定值时，索引还使 data.table 能够使用二分搜索法。

# 窗口功能

在 SQL 中，您可以使用一些有趣的累积和，例如

```
SELECT 
  grp, 
  obs_date, 
  sum(val) over (
    partition by grp 
    order by obs_date 
    rows unbounded preceding
  ) as cumulative
FROM mydt
```

您可以在 data.table 中快速完成同样的操作:

```
mydt[
  order(obs_date),
  .(obs_date,
    cumulative = cumsum(val)),
  by = grp
][
  order(obs_date)
]
```

data.table 甚至自带了 shift()和 frank()等快速窗口函数——后者用于快速排序。

# 最后

如果您还没有在 R 工作中使用过 data.table，我强烈建议您尝试并学习它。你会回过头来想，如果没有它，你以前是怎么工作的。它速度更快，内存效率更高，而且比 r 中的其他任何东西都更简洁。

与纯粹使用 SQL 相比，一个主要的优势是您可以保持中间表组织良好，而不是创建视图。在 Rstudio 中，可以使用环境窗口来跟踪所有对象。