# 自动化这 3 个(无聊！！)用 Python 做 Excel 任务！

> 原文：<https://towardsdatascience.com/automate-these-3-boring-excel-tasks-with-python-666b4ded101b?source=collection_archive---------3----------------------->

## 不用再打开数百个 Excel 文件

![](img/74bf3f6ad63c8a58a65fb213777f7ccd.png)

让我们来学习如何自动化一些枯燥的 Excel 任务！资料来源:Nik Piepenbreier

Excel 无处不在。无论好坏，它基本上是工作场所中数据分析的默认应用程序。在你的日常生活中，你可能需要完成许多无聊的任务，这让你思考，“一定有更好的方法”。Python 在那边！

所有这些例子在 VBA 都是可能的，但是 VBA 可能很乏味，嘿，我们喜欢 Python！

# 合并多个 Excel 文件

![](img/92a4e9bf6da76fefa253cd697d8cd663.png)

让我们将多个 Excel 文件合并成一个文件！资料来源:Nik Piepenbreier

您可能会发现自己有许多 Excel 工作簿(例如每月销售报告)。有一天，你被要求计算所有这些报表的总销售额。我们可以使用 Pandas 库轻松地将 Excel 工作簿与 Python 结合起来。

您可以使用 pip 或 conda 安装 Pandas:

```
pip install pandas
conda install pandas
```

对于本教程，让我们加载三个独立的 Excel 工作簿，它们链接到下面:

*   文件一:[https://github . com/datagy/medium data/raw/master/January . xlsx](https://github.com/datagy/mediumdata/raw/master/january.xlsx)
*   文件二:h[ttps://github . com/datagy/medium data/raw/master/junior . xlsx](https://github.com/datagy/mediumdata/raw/master/february.xlsx)
*   文件三:【https://github.com/datagy/mediumdata/raw/master/march.xlsx 

我们可以看到数据直到第 4 行才真正开始，所以我们需要 Pandas 从那一行开始导入工作表。在下面的代码中，我们将使用 read_excel 和 append 函数。

将 Excel 文件与 Python 相结合。资料来源:Nik Piepenbreier

让我们看看我们在这里做了什么:

1.  在第 1 部分中，我们导入了 pandas，创建了一个包含所有 URL 的列表，并生成了一个名为 *combined* 的空白数据帧。
2.  在第 2 节中，我们遍历了*文件*中的每个 URL，将每个文件读入一个数据帧(“df”)，跳过前三行，并将其附加到组合的*数据帧中。*
3.  在第 3 节中，我们编写生成一个名为 *combined.xlsx* 的新 Excel 文件，其中包含我们合并的 Excel 工作簿！

# 从各种工作簿中获取值

![](img/acb1883b2f8cc5b89b5d1b013358f943.png)

使用 Python，很容易从多个文件中获得任何值！资料来源:Nik Piepenbreier

我们再来看另一个例子！假设我们只需要从每个销售报告中获得多伦多的总数，并将它们收集到一个列表中。我们知道总数存储在每个工作簿的单元格 F5 中。如果您正在跟进，如果您将文件存储在本地，这个包就可以工作。用上面的链接下载文件并保存到你的机器上。

对于这个例子，我们将使用另一个名为 **openpyxl** 的库。您可以使用以下代码安装 pip 或 conda:

```
pip install openpyxl
conda install openpyxl
```

让我们开始编码吧！

用 Python 从多个工作簿中获取单元格。资料来源:Nik Piepenbreier

让我们一步一步地分解它:

![](img/897831b0dfe8feeb06125444af919372.png)

在 Windows 中复制文件路径。资料来源:Nik Piepenbreier

在**第 1 节**中，我们:

*   生成了一个包含所有文件链接的列表(“文件”)。在 Windows 中，我们可以按住 Shift 键并单击鼠标右键，然后使用“复制为路径”来获取它的路径。
*   您可能希望将字符串转换为原始字符串，方法是在它前面加上前缀“r”。
*   我们还生成了一个空列表来存储我们的值。

在**步骤** 2 中，我们

*   使用 openpyxl 遍历文件。
*   的。load_workbook()方法加载一个文件。
*   我们使用['Sheet1']和['F5']来引用工作簿和工作表对象中的工作表名称和单元格引用。
*   最后，我们使用。value 属性提取单元格的值，并将其追加到*值*列表中。

# 在工作簿中应用公式

![](img/01b2041ee9e7365e080ceb9cac605dff.png)

让我们在多个工作簿中应用相同的公式！资料来源:Nik Piepenbreier

让我们看看最后一个例子！在每个 Excel 工作簿中，我们都有跨行的汇总，但没有销售额的总计。同样，我们可以打开每个工作簿并添加一个公式，或者我们可以使用 Python 来完成这项工作！

我们将再次使用 openpyxl。如果您需要安装它，请查看上面的说明。上面也有下载文件的链接。

在工作簿中插入公式！资料来源:Nik Piepenbreier

在这段代码中，我们再次填充了一个文件列表。for 循环打开每个文件，并将“Sheet1”分配给一个变量 Sheet。

然后，我们将字符串' =SUM(F5:F8)'赋给单元格 F9，并使用。属性将货币样式直接分配给单元格。更多单元格样式可在[官方文档](https://openpyxl.readthedocs.io/en/stable/styles.html)中找到。

# 结论:用 Python 自动化 Excel

Python 使得使用 Excel 文件变得非常容易！在本文中，我们学习了如何组合各种 Excel 文件、获取特定值以及在工作簿中添加公式。

让我们使用 Python 创建可追踪的、可复制的代码，让我们复制我们的分析设计。虽然您可能一整天都在使用 Excel 工作簿，但是 Python 可以自动完成一些随之而来的繁琐任务。