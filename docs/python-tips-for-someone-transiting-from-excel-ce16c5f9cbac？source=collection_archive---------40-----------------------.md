# 从 Excel 过渡到 Python 的技巧

> 原文：<https://towardsdatascience.com/python-tips-for-someone-transiting-from-excel-ce16c5f9cbac?source=collection_archive---------40----------------------->

关于提高生产率的最佳做法的讨论

![](img/188e54a69d2943052f459671005b40e0.png)

米卡·鲍梅斯特在 [Unsplash](https://unsplash.com/s/photos/excel-automation?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

Excel 是最常用的数据分析应用程序之一。然而，由于其明显的局限性(即性能、行和列的限制)，它可能不足以应付现代的需求。Python 狂热者经常推荐使用 pandas 库作为 Excel 的替代品来自动化/加速数据分析。然而，如果不熟悉这个库和一些最佳实践，它不一定能节省时间。本文旨在强调使用 Excel 进行数据分析的一些挑战，以及使用 python(和 pandas 库，以及一些附加库)和采用最佳实践如何有助于提高整体效率。假设有一些 python 和熊猫的基础知识。

典型的数据分析师可能夹在业务职能部门和应用程序所有者之间。这种职责分离的结果是，可能没有一个人会理解数据的所有用例。热心的数据分析师有责任对数据进行切片和切块，形成基于输入的假设，并寻找反例来反驳假设。这种递归过程需要数据分析师手动重复过滤、排序、分组、连接、透视、vlookup、拆分 Excel 上的数据。这些步骤可能会导致人工错误，特别是当操作没有[往返](https://en.wikipedia.org/wiki/Commutative_property)时(即结果取决于操作的顺序)。解决这个问题的关键是从根本上改变方法:**不要机械地操纵数据来准备输出，而应该进行战略性的思考，并准备一个生成输出的程序/脚本。对我来说，这是使用 python 的最大动机，因为重复一系列动作的纯手工和令人麻木的任务不适合我，基于 Excel 的宏太慢了。**

这篇文档/文章涵盖了一些有用的想法，通过使用 python，特别是 pandas 库，人们也许可以利用这些想法来节省一些时间并潜在地避免重复工作。

## 一些警告

在以下情况下，使用 pandas 几乎没有什么价值:
(a)数据量相当小(少于 50000 行)，
(b)涉及的文件非常少(少于 10 个)，
(c)为将来使用而准备的输出数据不太可能频繁重复或手动重复。

在这些情况下，Excel 将是首选工具，这取决于您对 python 和 pandas 的熟悉程度。

Python 和 pandas 的图形用户界面非常有限，因此大多数人可能会发现，与 Excel 相比，它的使用不够直观。例如，重新排序列和查找值是否存在这样的琐碎操作在 Excel 中是微不足道的，但是这些操作需要 python 上的几行代码，并且对于第一次接触 python 和 pandas 的初学者来说不那么直观。

## 在 ide 上，文本编辑器…

虽然有一些不错的工具(PyCharm 和 Jupyter 笔记本)可用，但是在我参与的项目中，这些工具是不可用的或者不可行的(工具是可用的，但是没有足够的 RAM 来使用它)。因此，我经常只使用命令提示符或 Powershell 来运行代码。任何带语法高亮的文本编辑器都可以，我用的是 Notepad++。

## 1.一次读文件，泡菜，以后读泡菜。

虽然这听起来微不足道，但数据分析师的首要任务是打开并读取数据文件。pandas 将 Excel 文件作为数据帧读取所花费的时间比 Excel 打开相同的 Excel 文件所花费的时间要长。当文件很大(尤其是 Excel 文件)时，将数据文件作为数据帧读取可能需要几分钟时间。如果每次执行脚本时都需要读取数据集，那么执行和编辑脚本的重复过程将大大降低效率。克服瓶颈的方法是读取数据并将其作为 pickle 写入(即`.pkl`)。读泡菜快 100 倍左右。通过获取数据样本(例如大约 10，000 行)并将其作为 pickle 写入，可以进一步提高效率。还应注意，读取平面文件(`.csv`和带分隔符的文本文件)比读取 excel 文件快得多，因此，如果可能，应检索/请求此类平面文件格式的文件，假设这些数据文件不打算由用户在 Excel 中打开。

## 2.规划好功能和代码结构

每个开发人员在开始一个全新的项目时都会说:“这一次，我会把事情做好。”。

在每个项目的开始，自然倾向于编写硬编码脚本来快速生成必要的报告。在这个阶段，可能会有一个单独的`.py`文件，它有一个巨大的功能，可以创建一个单独的报告。抵制匆忙生成输出报告的冲动，因为长函数很难维护，而且技术债务积累得非常快。

因为可读性很重要，所以每个函数在逻辑上应该代表一个预期的操作。下面的示例显示了读取、清理、修改数据以及将两列相乘以在另一列中给出结果的推荐方法。

示例 1.1(推荐):

```
def read_trade_data():
   return pd.read_excel('data.xlsx')def clean_trade_data(df):
    # PRICE turns out to be text with comma as decimals point.
    df['PRICE'] = df['PRICE'].str.replace({",", "."})
                             .astype('float64')
    return dfdef add_col_total(df):
    df['TOTAL'] = df['QTY']*df['PRICE']
    return dfdef df_output():
    df = read_trade_data()
    df1 = clean_trade_data(df)
    df2 = add_col_total(df1)
    return df2
```

示例 1.2

```
def df_output():
    df = pd.read_excel('data.xlsx')
    df['PRICE'] = df['PRICE'].str.replace({",","."})
                             .astype('float64')
    df['TOTAL'] = df['QTY']*df['PRICE']
    return df
```

虽然示例 1.1 看起来更冗长，但有几个好处。由于这种分离，`df_output`在 1.1 中可读性更好，因为在读取修改数据的代码之前，不必通读所有代码来清理数据。此外，如果期望其他数据帧具有类似的十进制格式，可以重用 1.1 中的`clean_trade_data`函数。另一方面，在 1.2 中，人们可能会求助于复制和粘贴代码。此外，如果在`df_output`上发生错误，在 python shell 中调试 1.1 的小函数会更容易。

应使用可选参数和关键字参数为数据分析师提供灵活性；应该避免执行类似操作的小函数。

如果预期只有一个版本的`data.xlsx`，这就足够了:

```
def read_trade_data():
    return pd.read_excel('data.xlsx')
```

然而，如果有多个版本的`data.xlsx`可能需要单独分析，那么编写一个函数来从命令行快速读取这些文件而不是重新编译代码是有意义的。

例 2.1(反例):

```
def read_trade_data1():
    return pd.read_excel('data1.xlsx')
```

将会有许多这样的函数，每个映射到不同的文件(例如`data2.xlsx`、`data3.xlsx`、…)，随着时间的推移，它会变得非常混乱。

示例 2.2(推荐):

```
def read_trade_data(file=None):
    d_file = {1: 'data.xlsx',
              2: 'data1.xlsx'}
    file = file if file else d_file[max(d_files.keys())]
    return pd.read_excel(file)
```

例 2.2 有几个优点:
(a)所有文件都在同一个字典里(可读性很重要！)，
(b)如果没有任何东西作为文件参数传递，它读取最新的`dataN.xlsx`，
(c)如果文件没有被添加到`files`字典，它允许用户显式地传递文件的路径。

许多编码教程都包含类似于示例 2.1 的代码，因为教程的上下文可能不支持使用推荐的最佳实践。

这些功能中的一些可以被做得足够通用，以至于它可以被应用于其他项目。例如，输出数据摘要(即每列唯一值的数量)并打印一小份数据样本通常非常有用。这样的函数应该作为 dataframe 类的方法编写(这样它就可以作为方法 df.summarize()而不是 mymodule.summarize(df)被调用)。此外，如果这样的函数一般适用于任何数据集，那么应该将它重构到一个单独的 python 文件中，并导入到主项目文件的名称空间中。

下面是我的工具箱中的一个函数(`pypo.py`)。

有时在 Excel 中查看和执行分析会更快。但是导航到文件然后双击它会很麻烦。该功能通过在 Excel 文件被写入后打开该文件来升级`to_excel()`方法。

```
#pypo.pyfrom pandas.core.base import PandasObjectdef to_excelp(df, *arg, **kw):

    def xlopen(path):
        import win32com.client as w32
        from pywintypes import com_error
        import os #Opens the file (.csv, .xls, .xlsx) in Excel
        xl = w32.Dispatch('Excel.Application') try:
            wb = xl.Workbooks.Open(path)
        except com_error:
            print('Check if file exists in current working directory…', end='')
            if path in os.listdir():
                print('found!')
                path = '{}\{}'.format(os.getcwd(), path)
                wb = xl.Workbooks.Open(path)
            else:
                print('not found! Please check file path!')
        else:
            pass
        finally:
            xl.Visible = True
        return path, xl, wb

    df.to_excel(*arg, **kw)
    path, xl, wb = xlopen(arg[0])
    return path, xl, wbPandasObject.to_excelp = to_excelp
```

将它添加到主工作文件的名称空间中:

```
#main.pysys.path.insert(1, 'C:/Documents/pyprojects/pypo.py')
import pypo
```

在命令行界面中编写脚本时，可以简单地键入`df.to_excelp('test.xlsx')`来将数据帧写成 Excel 文件，该文件将在编写后打开。该函数还返回 Excel 应用程序`xl`和工作簿`wb`对象，可以在以后使用(也许是为了在 MsExcel 中自动格式化和创建表格？).

评估器可用于扩展 python，虽然这些可以简化`pypo.py`中的代码，但会导致主文件中的语法稍微冗长一些(例如`df.myfunc.to_excel()`)。回想一下“平的比嵌套的好”。

## 3.过滤和选择数据

直观的方法是编写在过滤和/或选择相关行或列后返回数据帧的函数。然而，追加或连接这些数据帧是缓慢的。目前发现的最佳实践是编写返回布尔掩码的函数。组合其他掩码可以使用二进制操作完成，获得行数将成为 simpler⁴，尽管不太直观。

示例 3.1:返回数据帧

```
# Returns a dataframe
def apples(df):
    return df[df['Fruits']=='Apples']df_apples = apples(df)
```

如果不需要进一步的转换(如合并、排序、过滤)，直观的方法会更快。

示例 3.2:返回布尔掩码

```
def select_apples(df)
    return df['Fruits']=='Apples'mask_apples = select_apples(df)
```

获取苹果的行数:`sum(mask_apples)`
获取包含苹果的数据帧:`df[mask_apples]`

这种方法避免了数据帧的设置成本，直到它真正需要时。使用布尔掩码的操作比数据帧上的操作快得多。

## 4.使最佳化

尽可能避免在数据帧的行或列中显式循环；这就是熊猫的全部意义。在大多数情况下，`np.where`、`np.select`、`np.cut`、`np.vectorize`和`df.apply`应该可以做到。其中一些方法本质上仍然是循环，但通常比显式循环快。

## 5.避免在 Excel 中使用 vlookups，而是合并表格

Excel 的 *vlookup* 函数从来不是用来合并两个表格的。Quoting [docs.microsoft](https://docs.microsoft.com/en-us/office/vba/api/excel.worksheetfunction.vlookup) ， *vlookup* “在**第一列**中搜索**值**，并从表数组中的另一列返回同一行中的值”。当表数组的第一列包含多个查找值时，将使用找到的第一个值的行号。与查找值匹配的后续值将被忽略。因此，使用`pandas.merge`来代替，并处理重复条目(如果有的话)。如果首选 *vlookup* ，确保查找值是唯一的。

Vlookup 看表的右边；“向左看”需要结合使用 *choose()* 和数组公式，如果不熟悉数组公式的行为，这很容易出错。有些人可能会通过插入一个辅助(重复)列来避免“向左看”,这会引入某种形式的冗余。

*这篇文章的动机是整合作者在各种项目中的学习，以便读者在工作中处理大量数据时不必经历类似的痛苦。在撰写本文时，互联网上有大量关于数据科学、大数据等方面的信息。然而，在实现用于数据分析的开源框架时，几乎没有关于最佳项目实践的指导。*

[1]在某种程度上，这不是一个公平的比较，因为 Excel 文件被设计为在 MsExcel 中打开，pandas 在幕后做了很多神奇的事情，将数据准备为 dataframe。

[2]从 12 个 excel 文件中读取 1 张需要 30 分钟。总共 170 万行。读取泡菜当量需要 15 秒。

[3]如果你对此一笑置之，是因为你理解或看到了这样的恐怖，那很好。如果你想知道这有什么问题，请继续阅读。

[4] `sum(mask)`，因为`True`值在 python 中被求值为 1。