# 加快 PySpark 和 Pandas 数据帧之间的转换

> 原文：<https://towardsdatascience.com/how-to-efficiently-convert-a-pyspark-dataframe-to-pandas-8bda2c3875c3?source=collection_archive---------8----------------------->

## 将大型 Spark 数据帧转换为熊猫时节省时间

![](img/bf458e4414027c0ab3262d120a8fcbef.png)

诺亚·博加德在[unsplash.com](https://unsplash.com/photos/mJFtH4FFl7o)拍摄的照片

由于使用了`toPandas()`方法，将 PySpark 数据帧转换成 Pandas 非常简单。然而，这可能是成本最高的操作之一，必须谨慎使用，尤其是在处理大量数据时。

# 为什么这么贵？

Pandas 数据帧存储在内存中，这意味着对它们的操作执行起来更快，然而，它们的大小受到单台机器内存的限制。

另一方面，Spark 数据帧分布在由至少一台机器组成的 Spark 集群的节点上，因此数据帧的大小受到集群大小的限制。当需要存储更多数据时，只需通过添加更多节点(和/或向节点添加更多内存)来扩展集群。

重要的是要理解，当在 Spark 数据帧上执行`toPandas()` 方法时，分布在集群**的节点上的所有行将被收集到驱动程序**中，该驱动程序需要有足够的内存来容纳数据。

# 用 PyArrow 加速转换

[Apache Arrow](https://arrow.apache.org/) 是一种独立于语言的内存列格式，当使用`toPandas()`或`createDataFrame()`时，可用于优化 Spark 和 Pandas 数据帧之间的转换。

首先，我们需要确保安装兼容的`PyArrow`和`pandas`版本。前者为 **0.15.1** ，后者为 **0.24.2** 。最近的版本也可能兼容，但目前 Spark 不提供任何保证，所以这很大程度上取决于用户测试和验证兼容性。有关所需版本和兼容性的更多详细信息，请参考 [Spark 的官方文档](https://spark.apache.org/docs/latest/sql-pyspark-pandas-with-arrow.html#recommended-pandas-and-pyarrow-versions)。

默认情况下，基于箭头的列数据传输是禁用的，因此我们必须稍微修改我们的配置，以便利用这种优化。为此，应启用`spark.sql.execution.arrow.pyspark.enabled` 。

```
import numpy as np
import pandas as pd

# Enable Arrow-based columnar data spark.conf.set("spark.sql.execution.arrow.pyspark.enabled", "true")# Create a dummy Spark DataFrame
test_sdf = spark.range(0, 1000000)# Create a pandas DataFrame from the Spark DataFrame using Arrow
pdf = test_sdf.toPandas()# Convert the pandas DataFrame back to Spark DF using Arrow
sdf = spark.createDataFrame(pdf)
```

如果在实际计算之前出现错误，PyArrow 优化将被禁用。如果我们想避免潜在的非箭头优化，我们还需要启用以下配置:

```
spark.conf.set(
    "spark.sql.execution.arrow.pyspark.fallback.enabled", "true"
)
```

# PyArrow 的局限性

目前，基于箭头的优化转换不支持某些 Spark SQL 数据类型。这些类型是:

*   `MapType`
*   `TimestampType`中的`ArrayType`
*   嵌套`StructType`

# 结论

正如我们已经讨论过的，`toPandas()`是一个开销很大的操作，应该小心使用，以尽量减少对 Spark 应用程序的性能影响。在需要这样做的情况下，尤其是当数据帧相当大时，在将 Spark 转换为 Pandas 数据帧时，需要考虑 PyArrow 优化(反之亦然)。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读介质上的每一个故事。你的会员费直接支持我和你看的其他作家。**