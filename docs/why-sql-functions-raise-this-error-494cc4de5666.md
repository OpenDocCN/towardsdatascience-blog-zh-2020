# 如何在 SQL 中创建带参数或不带参数的函数

> 原文：<https://towardsdatascience.com/why-sql-functions-raise-this-error-494cc4de5666?source=collection_archive---------37----------------------->

## 调试这个简单的错误可能会浪费您几个小时的时间。

![](img/3802cc78a2aa82a6600dea1812b84af1.png)

在 [Unsplash](https://unsplash.com/s/photos/wrong?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上 [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

你不应该经历我在学习用 SQL 编写函数查询时的痛苦经历。在我最终发现括号花费了我很多时间之前，我已经花了很长时间仔细研究了我写的查询。

在本文中，通过查询示例，您将学习如何用 SQL 编写带参数或不带参数的函数。我找出了浪费我时间的简单错误。

下面的查询是一个没有参数的函数，它对 sales 列中的销售额进行求和。我使用了 [AdvertureWorks 2012](https://docs.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver15&tabs=ssms) 数据库——销售。SalesTerritory 表—在本教程中。

注意:您可以使用任何其他数据库来遵循本教程。

# 如何在 SQL 中创建不带参数的函数

第一行创建了一个函数，然后将其命名为“YTDSALES()”。你可以给你的函数取任何名字。当没有参数时，记得在函数名中添加括号。

在下一行中，写下单词“RETURNS ”,后跟您想要的任何名称。我在这里用了" MONEY ",因为我想得到钱的总数。接下来写下关键字“AS”和“BEGIN”。

“将@YTDSALES 声明为 MONEY”出现在第 5 行。您可以将“@”与您选择的任何单词一起使用，而不一定是“@YTDSALES”。

以“SELECT @YTDSALES =”开始第六行，后跟列(SalesYTD)，您希望在括号前加上“sum”。你应该包括表(销售。SalesTerritory)，它包含列(SalesYTD)。

然后写“RETURN @YTDSALES”和“END”完成函数。运行您的查询。您将看到“命令成功完成”在你的屏幕上。

您刚刚创建了一个函数。要运行您的函数，请运行以下查询:

在上面的第一行，您可以给“@RESULT”起任何名字。

记得包括“dbo”你的函数名。在这种情况下，它是 dbo。YTDSALES()。创建函数时检查第一行，以了解函数的名称。这里，YTDSALES()是函数的名称。

将“PRINT @RESULT”作为查询的最后一行。

运行查询，您将获得指定列中数字的总和。

# 如何在 SQL 中创建带参数的函数

带参数的函数可以直接提取准确的信息。编写带参数的函数和不带参数的函数几乎是一样的。下面指出两者的区别。

首先运行上述查询来创建一个函数。

运行以下查询查看结果:

带参数的 SQL 函数和不带参数的 SQL 函数的区别之一是括号—()。注意上面查询的第一行，与带括号的查询函数不同，它没有括号！如果您错误地添加了一个括号，您将得到以下错误消息:

要亲身体验这个错误，请在上面创建的函数的第一行添加括号。

其他的区别是这里使用的“组”和“太平洋”这两个词。为了澄清起见，“GROUP”是列的名称，“Pacific”是“GROUP”列中要从中获取数据的地点名称之一。

当查询是有序的(没有括号)时，创建的带有参数的函数给出“Pacific”区域的总数。

# 结论

带参数的函数和不带参数的函数之间的差别很小。没有参数的函数的第一行以空括号结束。带参数的函数的第一行没有括号。请注意这一差异以及本文中提到的其他差异，以避免花费大量时间调试函数查询。