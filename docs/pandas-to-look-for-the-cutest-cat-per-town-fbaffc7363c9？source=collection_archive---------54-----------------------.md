# 熊猫在每个城镇寻找最可爱的猫

> 原文：<https://towardsdatascience.com/pandas-to-look-for-the-cutest-cat-per-town-fbaffc7363c9?source=collection_archive---------54----------------------->

![](img/6cb361bbfa73155acd90a982a377848b.png)

[蔡文许](https://unsplash.com/@tsaiwen_hsu?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的原图，本人编辑。

## 熊猫的有趣介绍——Python 数据分析库

嗨，在[的上一篇文章](https://medium.com/activeai/finding-meow-the-cutest-cat-from-each-town-using-elastic-search-29a9417bc24d)中，我们看到了使用 SQL 和 Elasticsearch 解决典型的*每组最大数量*的问题。本文将探讨如何使用强大的 Python 数据分析库 [*Pandas*](https://pandas.pydata.org/) 来解决这个问题。🐼

熊猫得到了两个重要的数据结构[系列](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.html)和[数据帧](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html)。我们将探索 Pandas DataFrame 数据结构和它的一些功能来解决我们的问题。说问题，没看过上一篇的我来解释一下。在一个城镇里，会有许多可爱的猫。我们要从每个镇只挑一只最可爱的猫，它的名字叫喵(或者类似喵，以后会看到)。

如果你想跟进，我建议你参考[熊猫官方网站](https://pandas.pydata.org/pandas-docs/stable/getting_started/install.html)进行本地设置，或者你可以在 [Kaggle](https://www.kaggle.com/sharmasha2nk/finding-meow) 上创建一个新的笔记本。甚至提交你的 [Kaggle 找喵任务](https://www.kaggle.com/sharmasha2nk/finding-meow/tasks?taskId=1056)的解决方案。

让我们用一个小的猫的数据集，列 id，猫的名字，城镇，可爱程度从 1 到 10，10 是最可爱的。

资料组

该数据集的预期输出为:

预期结果

首先，我们将 pandas 库作为`pd`导入，并用样本数据集创建一个数据帧`df`。有许多方法可以创建数据帧。下面我提供了三个选项。选项 1 使用字典；选项 2，通过加载本地可用的 CSV 文件；选项 3 从 URL 读取 CSV 文件。

一旦创建了数据帧，就可以使用`df.head()`方法从数据帧中获取前 5 行。可选地，您可以传递要检索的行数，比如`df.head(10)`，检索前 10 行。

创建数据框架

类似于`df.head(n)`方法，有`df.tail(n)`返回最后 n 条记录。

df.tail()

`df.columns`将返回所有的列名。
`df.shape`返回表示数据帧维度的元组。比如我们的 DataFrame，(10，4)，这意味着 10 行 4 列。
`df.size`返回一个表示该对象中元素个数的 int。比如我们的数据帧，40 (10*4)

数据帧信息

为了获得数据帧的简明摘要，我们可以使用`df.info()`。我们的数据有 4 个非空列，每个列有 10 个条目。`id`和`cuteness`为表示数字的`int64`数据类型。`name`和`town`作为一个`object`。如果您正在处理一些未知的数据，这可能是方便的信息。

其中`df.info`提供关于数据类型、内存使用等的信息。，我们可以使用`df.describe()`来获得统计数据。你可以参考官方的[文件](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.describe.html#pandas.DataFrame.describe)来理解这些数字的含义。

数据帧描述

要引用特定的列数据，您可以像`df['town']`一样访问并对其执行查询。想要获得列 town 的唯一值，我们可以运行`df['town'].unique()`。或者获取每个城镇的值计数，`df['town'].value_counts()`。根据我们的数据，我们在德里有 4 只猫，在孟买和班加罗尔各有 2 只猫& Meerut，我们必须在每个城镇中找出 1 只最可爱的猫。

唯一计数

要查看每个城镇每列的最大值，我们可以按城镇对数据进行分组，并使用 max 函数。如果你看下面的结果，乍一看，它可能似乎是我们问题的解决方案(忽略名称条件)，但它肯定不是。像我前面说的，它分别返回每一列的最大值**。如您所见，我们没有 id 和姓名为 5、tom 可爱度为 10 的记录。尽管我们在班加罗尔的最大 id 是 8。同样，在密鲁特和孟买，9 是最大的可爱。**

**最大**

**要找到名为喵的猫，我们必须添加过滤器。我们可以通过多种方式查询数据。比如`df[df['name']=='meow']`或者使用`df.query()`执行字符串查询。本文将使用前一个。但是，在某些情况下，后一种方法可能更有用，比如您从一些外部来源获取这些查询。**

**询问**

**根据我们的任务，我们不仅要寻找猫名"喵",还要寻找听起来相似的猫名。所以将修改我们的搜索查询来查找包含喵的名字。我们将如何使用`.str`将列转换为字符串，并使用字符串的 contains 方法。看看下面的搜索结果，现在我们有了猫的子集，它们的名字叫喵或者听起来像喵。**

**包含查询**

**对于猫的子集，让我们按城镇将它们分组，并找出每个城镇的最大可爱程度。这给了我们每个城镇最大的可爱度，但是我们还没有从这个结果中识别出猫。我们丢失了一些列，如 id 和 name。**

**分组依据**

**因此，不是取出可爱列并对其应用一个`max`函数，而是对整个`DataFrameGroupBy`对象应用一个 lambda 函数，返回一个以城镇为索引的数据帧。如果你检查下面的结果，现在我们有城镇作为一个列和一个索引。**

**分类**

**现在有了这个数据框架，我们可以再次按城镇分组，并从每个组中获得第一个条目，这就是我们的解决方案。但是如果我们这样做，我们会得到一个异常，因为`town`既是一个索引级别又是一个列标签，这是不明确的。**

**错误**

**我们能做的是在做另一个 group by 之前重置索引。我们可以在`df.reset_index(drop**=**True)`前完成。`drop=True`选项将避免索引作为一列添加到结果数据帧中。或者更好的是，我们可以使用`groupby`方法的现有选项`as_index=False`，这将产生类似的结果。**

**看看下面的解决方案，我们可以在每个城镇找到名字为喵或与之相似的最可爱的猫。**

**但是它与我们预期的解决方案不匹配😳。对于德里，我们期待 id 为 5 的猫，而不是 3。我们遗漏了任务的一部分，我们必须优先选择名字叫喵的猫，而不是名字听起来像喵的猫。**

**解决办法**

**为此，一种方法是我们添加另一个得分字段。假设猫的名字不能超过 100 个单词，我们可以使用公式`(cuteness of cat * 100) — length of the name`给每只猫打分。所以即使两只猫的可爱度都是 10。喵喵猫会得分`10*100-4 = 996`，卡米诺会得分`10*100-6 = 994`。有趣的是，我们的分数不会因为可爱程度不同而重叠。比如，如果可爱度是 10，分数将在范围`10*100-100 = 900`到`10*100-4 = 996`之间。如果可爱度是 9，分数将在`9*100-100 = 800`到`9*100-4 = 896`的范围内。所以我们可以安全地使用这个公式。**

**要向我们的 DataFrame 添加一个新列，我们可以简单地做如下的事情。**

**新列**

**所以现在对于我们的最终解决方案，我们可以用可爱度排序代替 total _ score，我们得到了正确的结果。🤗**

**最终解决方案**

**感谢阅读！如果你有任何疑问或者你觉得你知道一个更好的方法来“找到喵”，请发表评论**

**页（page 的缩写）准备好迎接每镇最可爱的猫挑战了吗？在推特上发布你对熊猫的最佳解决方案，标签为# *每镇最可爱的猫，#findingmeow* 👨‍💻或者下面评论。如果我们发现您的解决方案比我们的更好，我们将附上您的姓名和您提交的内容。**