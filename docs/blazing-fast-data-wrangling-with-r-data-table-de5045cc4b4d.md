# 与 R data.table 进行极快的数据争论

> 原文：<https://towardsdatascience.com/blazing-fast-data-wrangling-with-r-data-table-de5045cc4b4d?source=collection_archive---------9----------------------->

## 谁有时间用慢代码做数据科学？

![](img/9997ec6bc45120a82d7f11fc596ab8c5.png)

照片由 [Fotis Fotopoulos](https://unsplash.com/@ffstop?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

***更新 2020 年 3 月:*** 在这里你可以找到一个很有意思的`data.table`和`dplyr` [的对比。](https://atrebas.github.io/post/2019-03-03-datatable-dplyr/)

# 介绍

我最近注意到我写的每个 R 脚本都以`library(data.table)`开头。这似乎是我写这篇文章的一个非常有说服力的理由。

你可能已经看到，随着机器学习社区的发展，Python 获得了巨大的人气，因此，熊猫图书馆自动成为许多人的主食。

然而，如果我有选择的话，我肯定更喜欢使用 R [data.table](https://cran.r-project.org/web/packages/data.table/vignettes/datatable-intro.html) 进行相对较大的数据集数据操作、深入探索和特别分析。为什么？因为它太快了，优雅而美丽(抱歉我太热情了！).但是，嘿，处理数据意味着你必须把代码颠倒过来，塑造它，清理它，也许…嗯…一千次，然后你才能真正建立一个机器学习模型。事实上，数据预处理通常占用数据科学项目 80–90%的时间。这就是为什么我不能承受缓慢而低效的代码(作为一名数据专家，我认为任何人都不应该这样)。

所以让我们深入了解什么是`data.table`以及为什么[许多人](https://www.reddit.com/r/rstats/comments/ca90nk/why_i_love_datatable/)成为它的忠实粉丝。

# 1.那么 data.table 到底是什么？

`data.table` [包](https://cran.r-project.org/web/packages/data.table/data.table.pdf)是 r 中`data.frame`包的扩展，广泛用于大型数据集的快速聚合、低延迟的列添加/更新/删除、更快的有序连接以及快速的文件读取器。

听起来不错，对吧？你可能觉得很难接，但实际上 a `data.table`也是`data.frame`的一种。所以请放心，您为`data.frame`使用的任何代码也将同样适用于`data.table`。但是相信我，一旦你使用了`data.table`，你就再也不想使用 base R `data.frame`语法了。想知道为什么就继续读下去。

# 2.Data.table 非常快

从我自己的经验来看，使用快速代码确实可以提高数据分析过程中的思维流程。速度在数据科学项目中也非常重要，在这个项目中，你通常必须快速地将一个想法原型化。

说到速度，`data.table`让 Python 和许多其他语言的所有其他包都相形见绌。这个[基准](https://h2oai.github.io/db-benchmark/)显示了这一点，它比较了来自 R、Python 和 Julia 的工具。在一个 50GB 的数据集上进行五次数据操作，`data.table`平均只用了 123 秒，而 Spark 用了 381 秒，(py)datatable 用了 712 秒，`pandas`由于内存不足无法完成任务。

data.table 包中最强大的函数之一是`fread()`，它与`read.csv()`类似地导入数据。但它已经过优化，速度快得多。让我们看看这个例子:

```
require("microbenchmark")
res <- microbenchmark(
  read.csv = read.csv(url("[https://archive.ics.uci.edu/ml/machine-learning-databases/adult/adult.data](https://archive.ics.uci.edu/ml/machine-learning-databases/adult/adult.data)"), header=FALSE),
  fread = data.table::fread("[https://archive.ics.uci.edu/ml/machine-learning-databases/adult/adult.data](https://archive.ics.uci.edu/ml/machine-learning-databases/adult/adult.data)", header=FALSE),
  times = 10)
res
```

结果将显示，平均而言，`fread()`将比`read.csv()`函数快 3-4 倍。

# 3.它直观而优雅

几乎所有使用`data.table`的数据操作都是这样的:

![](img/f444a9fecc52fd6d059fbb8f1e853ced.png)

来源:https://github.com/Rdatatable/data.table/wiki

因此，您编写的代码将非常一致且易于阅读。让我们以美国人口普查收入数据集为例进行说明:

```
dt <- fread("[https://archive.ics.uci.edu/ml/machine-learning-databases/adult/adult.data](https://archive.ics.uci.edu/ml/machine-learning-databases/adult/adult.data)", header=FALSE)
names(dt) <- c("age", "workclass", "fnlwgt", "education", "education_num", "marital_status", "occupation", "relationship", "race", "sex", "capital_gain", "capital_loss","hours_per_week", "native_country", "label")
```

在下面的例子中，我将比较 base R、Python 和 data.table 中的代码，以便您可以轻松地比较它们:

1.  计算所有“技术支持”人员的平均*`age`*:**

***>** 在 **base R** 中你大概会写出这样的东西:*

```
*mean(dt[dt$occupation == 'Tech-support', 'age'])*
```

*>在 **Python** 中:*

```
*np.mean(dt[dt.occupation == 'Tech-support'].age)*
```

***数据表**中 **>与**:*

```
*dt[occupation == 'Tech-support', mean(age)]*
```

*正如你在这个简单的例子中看到的，与 Python 和 base R 相比，`data.table`消除了所有重复`dt`的冗余。这反过来减少了犯打字错误的机会(记住编码原则 DRY —不要重复自己！)*

*2.到*所有男性工人按职业分类的总年龄*:*

***>** 在**基地 R** 你大概会写:*

```
*aggregate(age ~ occupation, dt[dt$sex == 'Male', ], sum)*
```

*>在 **Python** 中:*

```
*dt[dt.sex == 'Male'].groupby('occupation')['age'].sum()*
```

***数据表**中**与>对比**:*

```
*dt[sex == 'Male', .(sum(age)), by = occupation]*
```

*`by =`术语定义了您希望在哪个(哪些)列上聚合数据。这种`data.table`语法一开始可能看起来有点吓人，但是一旦你习惯了，你就再也不会费心输入“groupby(…)”了。*

*3.为了*有条件地修改列中的*值，例如将所有技术支持人员的`age`增加`5`(我知道这是一个愚蠢的例子，但只是为了说明:)。*

*>在 **base R** 中，你可能不得不写下这可怕的一行(谁有时间写这么多次`dt$`！):*

```
*dt$age[dt$occupation == 'Tech-support'] <- dt$age[dt$occupation == 'Tech-support'] + 5*
```

*>在 **Python** 中(有几个备选方案同样长):*

```
*mask = dt['occupation'] == 'Tech-support'
dt.loc[mask, 'age'] = dt.loc[mask, 'age'] + 5*
```

*或者使用`np.where`:*

```
*dt['age'] = np.where(dt['occupation'] == 'Tech-support', dt.age + 5, dt.age]*
```

*>与**数据表**中的比较:*

```
*dt[occupation == 'Tech-support', age := age + 5]# and the ifelse function does just as nicely:
dt[, age := ifelse(occupation == 'Tech-support', age + 5, age)]*
```

*这几乎就像一个魔术，不是吗？没有更多繁琐的重复代码，它让你保持干爽。您可能已经注意到了`data.table`语法中奇怪的操作符`:=`。该操作符用于向现有列分配新值，就像在 Python 中使用参数`inplace=True`一样。*

*4.*重命名列*在 data.table 中轻而易举。如果我想将`occupation` 列重命名为`job`:*

*>在 **base R** 中，您可以写:*

```
*colnames(dt)[which(names(dt) == "occupation")] <- "job"*
```

*>而在 **Python** 中:*

```
*dt = dt.rename(columns={'occupation': 'job'})*
```

*>相对于**数据表**:*

```
*setnames(dt, 'occupation', 'job')*
```

*5.如何将函数应用到几列？假设您想将`capital_gain`和`capital_loss`乘以 1000:*

*>在**底座 R:***

```
*apply(dt[,c('capital_gain', 'capital_loss')], 2, function(x) x*1000)*
```

*>在 **Python** 中:*

```
*dt[['capital_gain', 'capital_loss']].apply(lambda x : x*1000)*
```

*>相对于**数据表**:*

```
*dt[, lapply(.SD, function(x) x*1000), .SDcols = c("capital_gain", "capital_loss")]*
```

*您可能会发现 data.table 中的语法有点古怪，但是`.SDcols`参数在许多情况下非常方便，data.table 语法的一般形式就是这样保留下来的。*

# *结论:*

*从上面几个简单的例子可以看出，R `data.table`中的代码在很多情况下比 base R 和 Python 中的代码更快、更干净、更高效。`data.table`代码的形式非常一致。你只需要记住:*

```
*DT[i, j, by]*
```

*作为补充说明，通过*键入*和`data.table`使用`i`设置数据子集甚至允许更快的子集、连接和排序，你可以在这个[文档](https://cran.r-project.org/web/packages/data.table/vignettes/datatable-intro.html)或这个非常有用的[备忘单](https://www.beoptimized.be/pdf/R_Data_Transformation.pdf)中了解更多信息。*

*感谢您的阅读。如果你喜欢这篇文章，我会在以后的文章中写更多关于如何与 R 和 Python 进行高级数据辩论的内容。*