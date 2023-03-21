# 面向数据科学家的 Linux 命令行工具

> 原文：<https://towardsdatascience.com/linux-command-line-utils-for-data-scientist-efff43a10f3c?source=collection_archive---------24----------------------->

## 数据科学提示

## 没有 Python 和花哨库的核心数据分析

![](img/7113aeec33f5be1cb22d9eafa2d6d903.png)

照片由[JESHOOTS.COM](https://unsplash.com/@jeshoots?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

数据科学总是与 R、Excel、SQL 联系在一起，最重要的是，与 Python 及其数量庞大的高级库(如 pandas、numpy 等)联系在一起，这并不奇怪。然而，如果我说，你可以提供非常完整和翔实的数据分析，没有这些东西。我想与您分享一组 Linux 命令行工具，它们提供了一个简单、快速和 Linux 风格的模拟您喜爱的数据工具的工具。因此，我想展示的是，数据科学家不受特定环境的约束，也不受他的武器库的限制。

# 1.获取数据

幸运的是，Linux 有各种显示文件内容的工具。不过，为了公平地搜索 Python 类似物，让我们排除所有文本编辑器，因为它们需要手动完成所有工作。我们正在寻找可能以熊猫方式表演的剧本。

## 头/尾

这些是从文件中获取明确行的简单实用程序:

`head -n 5 example_data.csv` —获取前 5 行。

`head -n -15 example_data.csv` —获取除最后 15 行之外的所有行。

`tail -n 15 example_data.csv` —获取文件中的最后 15 行。

`tail -n +15 example_data.csv` —获取从 15 号开始的所有行。

## 圆柱

如果我们想看到格式化的表格而不是原始数据，我们有另一个命令。

```
column -s"," -t example_data.csv
```

`-s`键提供文件中的列分隔符，而`-t`只提供格式。它将产生如下输出:

```
col1  col2  col3
0     1     D
1     0     C
2     0     C
3     1     C
```

但是这个命令将生成整个文件的输出，所以最好与前面的命令一起使用:

```
head -n 5 example_data.csv | column -s"," -t
```

## 切口

前面的命令同时处理所有列。但是分栏怎么办呢？让我们检查另一个命令:

```
cut -d"," -f2,5 example_data.csv
```

它将从逗号分隔(由`-d`键给出)的文件中提取第 2 和第 5 列(由`-f`键给出)。您可以选择单个列或几个列，然后您可以重新定向输出以对它们进行一些分析。

您也可以从 [csvkit 套件](https://csvkit.readthedocs.io/en/0.9.1/index.html)中通过 [csvcut](https://csvkit.readthedocs.io/en/0.9.1/scripts/csvcut.html) 命令获取列。

`csvcut -n example_data.csv` —返回列的列表。

`csvcut -c -1 example_data.csv` —返回第一列的所有值。

## csvlook

如果你想要更漂亮的预览，这个命令是给你的。它是 [csvkit 套装](https://csvkit.readthedocs.io/en/0.9.1/index.html)的一部分，因此您可以在 [csvlook 页面](https://csvkit.readthedocs.io/en/0.9.1/scripts/csvlook.html)上找到更多详细信息。命令:

`head -4 example_data.csv | csvlook`

将产生如下输出

```
|-------+-------+-------|
|  COL1 |  COL2 |  COL3 |
|-------+-------+-------|
|  0    |  1    |  'D'  |
|  1    |  0    |  'C'  |
|  2    |  0    |  'C'  |
|  3    |  1    |  'C'  |
|-------+-------+-------|
```

# 2.数据清理

但是，如果我们想要删除不需要的列、删除重复的内容或删除一些不必要的行，该怎么办呢？嗯，它是 Linux，所以我们对所有这些活动都有特殊的实用程序。

## 删除列

您应该反过来操作:不要选择您想要删除的列，而是选择您想要保留的命令。最简单的方法是使用`cut`命令将您需要的列提取到新的数据集中:

```
cut -d"," -f2,5 example_data.csv > new_example_data.csv
```

## 使用 grep 过滤

`grep`是一个带来 regex 所有力量的命令。对我来说，它是解析数据最方便的工具。例如:

```
grep -n ‘C’ example_data.csv >filtered_data.csv
```

该命令将找到所有带有“C”符号的行，提取它们，添加行数并写入新文件:

```
3:1,0,C
4:2,0,C
5:3,1,C
```

您可以添加`-v`键进行反向搜索:所有不包含“C”符号的行。或者您可以添加`-i`键，在搜索时忽略字母大小写。因此，您可以结合使用`grep`和`cut`管道来检查一些列，获取被过滤行的索引，并通过`sed`或`awk`命令选择这些行。

## awk 和 sed

这两个包太强大了，可以携带几乎所有可以想象的处理数据集的命令。有时它们可以被视为控制台内的另一种内置语言和解释器。[查看](https://www.gnu.org/software/gawk/manual/gawk.html) `[awk](https://www.gnu.org/software/gawk/manual/gawk.html)`和`[sed](https://www.gnu.org/software/sed/manual/sed.html)` [文档](https://www.gnu.org/software/sed/manual/sed.html)获取更多示例，因为它们对于本文来说太大了。

## 整理

没有更多的话，但一个单一的命令:

```
tail -n +1 example_data.csv | sort -t"," -k1,1g -k2,2gr -k2,2
```

这里发生了什么事？我们获取没有标题的数据，注意，列由逗号(`-t`键)分隔，并按前三列排序:第一列按数字(`-g`键)顺序，第二列按数字和相反(`-r`键)顺序，最后一列按默认的词典顺序。排序键(`-k` command 键)的列数可以变化或由一系列列表示。

# 3.基本分析

干净的数据看起来很漂亮，但如果没有进一步的调查和分析，它是没有用的。那么，如果没有熊猫的所有特征，我们能做什么呢？

## 单词/符号计数

简单的命令`wc`允许得到这些值:

```
wc example_data.csv
```

我们将获得文件中的行数、字数和字节数。字节数也可以视为 ASCII 编码文件中的符号数。该命令也有`-m`键来使用控制台编码。

## 独特元素

一如既往，Linux 准备了 util:

```
uniq -u example_data.csv 
```

它将显示没有重复的唯一行。但是，当应用于列时，它会更有用:

```
cut -d"," -f2 example_data.csv | uniq -u 
```

这将返回文件第二列中的所有唯一值。

## 统计值

有一个特殊的工具可以得到所选列的常见计算结果:最小值、最大值、总和、中值、平均值、标准值。它是 [csvkit 套装](https://csvkit.readthedocs.io/en/0.9.1/index.html)的一部分。

```
csvcut -c 1 example_data.csv | **csvstat**
```

[csvstat](https://csvkit.readthedocs.io/en/0.9.1/scripts/csvstat.html) 也处理分类数据，并显示缺失值的数量和唯一值的列表。所以，这对熊猫用户来说很方便。

# 4.抽样

让我们想象一下，我们的数据被清理，并为统计分析或模型构建做好准备。下一步是什么？在大多数情况下，它是数据的抽样或测试/训练分割。在 Linux 控制台中，我们可以很容易地用`shuf`命令来完成(对于 *shuffle* )。

`shuf -n 4 example_data.csv` —从文件中随机产生 4 行。

要从采样中排除标头，我们应该使用下一个命令:

`tail -n +1 example_data.csv | shuf -n 4`

同样，您可以自由地使用重定向将命令的输出写入文件。

我相信，代表你作为数据科学家的主要东西，不是一套工具和仪器，而是你可以应用于任务的知识和你可以用你的知识实现的方法。你可以使用不同的编程语言，不同的支持程序，不同的脚本或软件包，但是你的知识和技能应该独立于工具集，这样你就可以在任何环境下工作。

但是测试一下自己就好了，看看，在寒冷黑暗的 Linux 命令行中，没有你热情而熟悉的 Python 库，你怎么能完成一个分析呢，不是吗？此外，还有更有用的 Linux 实用程序，因此它们不可能在一篇文章中涵盖。因此，您可以自由地分享更多如何在终端中处理数据的示例。