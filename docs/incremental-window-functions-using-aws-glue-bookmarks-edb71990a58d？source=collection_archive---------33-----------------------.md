# 使用 AWS 粘合书签的增量窗口函数

> 原文：<https://towardsdatascience.com/incremental-window-functions-using-aws-glue-bookmarks-edb71990a58d?source=collection_archive---------33----------------------->

## 无序数据着陆问题

# 无序数据着陆问题

如果数据无序到达(相对于应用窗口函数的维度)，对数据应用窗口函数是很重要的。为了清楚起见，让我们把这个例子中的时间序列数据作为我们的窗口维度。如果时间序列数据从一周的星期二到星期四到达，那么该周的星期一的数据在较晚的时间到达，则数据到达是无序的。

![](img/c9a2fe199ead7f7624550b86de43ae73.png)

里卡多·戈麦斯·安吉尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

由于窗函数输出在时间空间上对其周围环境敏感，窗函数的结果将被到达的新的无序数据改变。所有受影响的数据都需要重新处理。

当数据无序到达时，您可以重新处理所有数据。但是，当数据量很大时，重新处理整个数据集变得不切实际。本文讨论了一种有效的方法，使用我在以前的[文章](/incremental-join-using-aws-glue-bookmarks-ad8fb2b05505)中描述的构建 AWS 粘合谓词下推的方法。这种方法仅重新处理受已到达的无序数据影响的数据。

# 解决办法

# 粘附 ETL 作业环境设置

# 检查新数据

使用 AWS 粘合书签只将新数据输入粘合 ETL 作业。

```
new = glueContext.create_dynamic_frame.from_catalog(database="db", table_name="table", transformation_ctx='new')
```

为新数据涉及的每个分区找到最早的时间戳分区。

注意:在下面的示例中，数据被分区为`partition1` > `timestamp_partition`，其中`timestamp_partition`是唯一的时间序列分区。

从需要处理/重新处理的整个数据集中构建数据的下推谓词字符串。对于没有无序数据的分区，手动定义一个数据窗口来解锁过去的数据，以确保正确处理新的有序数据。

注意:在下面的例子中，我们按照日期在`timestamp_partition`中进行分区。

# 对所有需要的数据应用窗口函数

现在，我们可以使用刚刚构建的谓词字符串加载所有需要处理/重新处理的数据。

然后，我们可以在加载的所有数据上定义我们的窗口。

在你的窗口上应用你的函数，这里我们以`last`函数为例。

为了确保在重新处理过程中被更改的任何旧数据被覆盖，请使用 PySpark API `overwrite`模式直接访问 S3。

# 结论

根据从 AWS 粘合书签传递的新数据构建下推谓词字符串仅允许处理/重新处理数据集的所需分区，即使在使用窗口函数时也是如此，窗口函数对其在数据集中的周围环境固有地敏感。

*原载于*[*https://data munch . tech .*](https://datamunch.tech.)