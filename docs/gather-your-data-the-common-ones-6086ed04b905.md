# 收集你的数据:常见的！

> 原文：<https://towardsdatascience.com/gather-your-data-the-common-ones-6086ed04b905?source=collection_archive---------81----------------------->

![](img/a7689b0ec01ad4a3031c4e9aec917a60.png)

维克多·塔拉舒克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 使用 Python 从 zip、csv、tsv、xlsx、txt

你知道哪个是最受欢迎的数据科学家模因吗？ ***“收集数据是我的心脏”。*** 没错，的确如此！与数据科学领域的初学者所想的不同，数据很少以干净整洁的表格格式提供给你。

在现实世界中，数据需要从多个来源收集，如平面文件、zip 文件、数据库、API、网站等。不仅仅是源代码，它们可以是不同的格式、文件结构，如。csv，。tsv、json 等，并由不同的分隔符分隔。如何从这个烂摊子中理出头绪？

![](img/6903b8f0f3219962a9d6f4d37fde088d.png)

作者图片

要执行分析并从我们的数据中获得准确的结果，首先，您应该知道不同的格式，其次，知道如何将它们导入 Python。在这篇博客中，我将涉及一些您可能熟悉的基本文件结构，并提到用于导入数据的不同但通用的 python 库。

掌握这些文件格式对你在数据领域的成功至关重要。那么每个人都从哪里开始呢？哦，是的，无处不在的 csv 文件。

## 用 python 读取 CSV 文件

用于存储数据的最常见的平面文件结构是逗号分隔值(CSV)文件。它包含由逗号(，)分隔符分隔的纯文本格式的表格数据。要识别文件格式，通常可以查看文件扩展名。例如，以“CSV”格式保存的名为“datafile”的文件将显示为“datafile.csv”。

Pandas 库用于从 python 中的 csv 文件读取数据。Pandas 库中的 [read_csv()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html) 函数用于加载数据并将其存储在数据帧中。

## 在 python 中读取其他平面文件

平面文件是包含表格行列格式记录的数据文件，没有任何编号结构。CSV 是最常见的平面文件。但是也有其他平面文件格式，其中包含带有用户指定分隔符的数据，如制表符、空格、冒号、分号等。

制表符分隔值(TSV)文件是第二常见的平面文件结构。来自 Pandas 库中的相同 read_csv()函数用于读取这些文件，您只需要为 read_csv()函数的参数' sep '指定正确的分隔符。

## 用 python 读取 XLSX 文件

一个带有。xlsx 文件扩展名是由 Microsoft Excel 创建的 Microsoft Excel Open XML 电子表格(XLSX)文件。这些文件是在 Microsoft Excel 中使用的文件，Microsoft Excel 是一种电子表格应用程序，使用表格来组织、分析和存储数据。

电子表格的每个单元格由行和列定位，可以包含文本或数字数据，包括合并数学公式。在早期版本的 Excel 中制作的电子表格文件以 XLS 格式保存。

pandas 库中的 [read_excel()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_excel.html) 函数用于将 excel 文件数据加载到 python 中。

XLSX 文件将数据组织到存储在工作表中的单元格中，这些单元格又存储在工作簿(包含多个工作表的文件)中。为了读取所有的工作表，我们使用 pandas [ExcelFile()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.ExcelFile.parse.html) 函数。现在，我们只需在 read_excel()函数的参数 sheet_name 中指定工作表的名称，就可以从工作簿中读取特定的工作表。

## 在 python 中提取 ZIP 文件

扩展名为 ZIP 的文件是 ZIP 压缩文件，是您将遇到的最广泛使用的归档格式。

与其他归档文件格式一样，ZIP 文件只是一个或多个文件和/或文件夹的集合，但为了便于传输和压缩，它被压缩成一个文件。

我们经常获得压缩格式的 csv 或 excel 文件，因为它节省了服务器上的存储空间，减少了您将文件下载到计算机的时间。python 中的 [zipfile](https://docs.python.org/3/library/zipfile.html) 模块用于提取。压缩文件。

## 使用 python 读取文本文件

纯文本格式的基本概念结构是数据按行排列，每行存储几个值。纯文本文件(也称为扩展名 TXT)由按特定字符编码顺序编码的字符组成。

Python 提供了各种函数来读取文本文件中的数据。每行文本都以一个称为 EOL(行尾)的特殊字符结束，这是 python 中默认的换行符(' \n ')。

[Python 文件操作](https://www.programiz.com/python-programming/file-operation)，具体来说就是打开文件、读取文件、关闭文件和其他各种文件方法，这些都是你在访问文本文件时应该知道的。内置 Python 函数 open()有模式、编码等参数。用于在 python 中正确打开文本文件。

Python 为我们提供了三种从文本文件中读取数据的内置方法，read([n])、readline([n])和 readlines()。这里的“n”是要读取的字节数。如果没有东西传递给 n，那么整个文件被认为是被读取的。

如果参数中没有给出字节数(n ),则 **read()** 方法只输出整个文件。

**readline(n)** 最多输出文件单行的 n 个字节。它只能读一行。

**readlines()** 方法维护了文件中每一行的列表，可以使用 for 循环对其进行迭代。

以上给出了用 python 读取文本文件的基本思路。但是，数据从来不会以一种易于访问的格式呈现在我们面前。假设你想根据你附近的评论找到 10 家最受欢迎的餐馆。

假设有 1000 家餐馆，其数据存在于 1000 个单独的文本文件中，您需要这些文件中存在的餐馆名称、评论 url 和评论文本。为了将多个文件中的数据读入一个数据帧，使用了 Python 的 glob 库。

Python 内置的 [glob 模块](https://docs.python.org/2/library/glob.html)用于检索与指定模式匹配的文件/路径名。glob 的模式规则遵循标准的 Unix 路径扩展规则。

/*是 glob 语句中的通配符。星号(*)匹配一个名称段中的零个或多个字符。您需要始终在 open()函数中指定编码，以便正确读取文件。

这里，文本文件的第一行是餐馆名称，第二行是餐馆评论 url，其余行是评论文本。相应地使用 readline()和 read()函数来提取这些数据。

在这篇博客中，我介绍了数据行业中人们使用的最常见和最流行的文件结构。这应该为你在处理数据收集和混乱的文件结构时的下一次挫折做好准备。还有许多其他的，我打算在我接下来的博客中讨论。

如果你喜欢这篇博文，请在下面留下评论并与朋友分享！