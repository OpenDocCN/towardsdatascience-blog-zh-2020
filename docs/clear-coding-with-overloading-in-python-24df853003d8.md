# 使用 Python 中的模块和包进行清晰的编码

> 原文：<https://towardsdatascience.com/clear-coding-with-overloading-in-python-24df853003d8?source=collection_archive---------29----------------------->

## 用通俗易懂的例子！

![](img/d956d209746746087cefd02b61f6430d.png)

扎克·卡道夫在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

当您的 Python 代码变大时，随着时间的推移，它很可能变得杂乱无章。随着代码的增长，将代码保存在同一个文件中会使代码难以维护。此时，Python **模块**和**包**帮助你通过使用文件和文件夹来组织和分组你的内容。

*   **模块**是带有**的文件。py"** 包含 Python 代码的扩展。它们有助于在同一个文件中组织相关的函数、类或任何代码块。
*   将大型 Python 代码块分割成包含多达 300–400 行代码的**模块**被认为是最佳实践。
*   **包**将相似的模块放在一个单独的目录中。它们是包含相关模块和一个 **__init__ 的文件夹。py** 文件，用于可选的包级初始化。
*   根据您的 Python 应用程序，您可以考虑将您的模块分组到子包中，例如 **doc、core、utils、data、examples、test。**

让我们编写一个示例 Python3 代码来进一步理解模块和包:

```
**""" cvs_get_module.py
This module displays the summary of the tabular data contained in a CSV file 
"""**import pandas as pdprint("cvs_get_module is loaded")*def* **display_file_location(*path, file_name*)**:
 print("File Location: {}".format(path+filename))*class* **CSVGetInfo:**
 *def* __init__(*self*, *path*, *file_name*):
  self.path = path
  self.file_name = file_name

 *def* **display_summary(*self*):**
  data = pd.read_csv(self.path + self.file_name)
  print(self.file_name)
  print(data.info())
```

![](img/1bcfed4d3e40f9fef8e8ec0cd6db78a7.png)

丹尼尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 导入模块

为了在外部 Python 代码块中使用一个模块，我们需要将那个特定的模块导入到我们的代码结构中。为此，使用带有"**import<*module _ name*>**"语法的 **import** 语句。这里的模块名是指不带**的 Python 文件名。py"** 扩展名。一旦我们导入模块，我们使用点符号，**.”，**访问模块内部的元素。

```
# **ModulesExample.py
# Importing 'csv_get_module' and accessing its elements**import **csv_get_module****data_by_genres** = **csv_get_module.CSVGetInfo**("/Users/erdemisbilen/
Lessons/", "data_by_genres.csv")**csv_get_module.display_file_location**(data_by_genres.**path**, data_by_genres.**file_name**)data_by_genres.**display_summary()****Output:** cvs_get_module is loaded
File Location: /Users/erdemisbilen/Lessons/data_by_genres.csv
data_by_genres.csv
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 2617 entries, 0 to 2616
Data columns (total 14 columns):
```

# 通过重命名导入模块

我们可以用**‘导入< *模块 _ 名称* >作为< *替代 _ 名称*>’**语法在导入的同时重命名模块。这可能有助于缩短长模块名。

```
# **ModulesExample.py
# Importing 'csv_get_module' and accessing its elements**import **csv_get_module** as **cg****data_by_genres** = **cg.CSVGetInfo**("/Users/erdemisbilen/Lessons/", "data_by_genres.csv")**cg.display_file_location**(data_by_genres.path, data_by_genres.file_name)data_by_genres.display_summary()**Output:** cvs_get_module is loaded
File Location: /Users/erdemisbilen/Lessons/data_by_genres.csv
data_by_genres.csv
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 2617 entries, 0 to 2616
Data columns (total 14 columns):
```

![](img/519e8c934ae4cf096ff09f9ca68bdba4.png)

照片由[亚历克斯·布洛克](https://unsplash.com/@alexblock?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 从模块导入特定名称

我们可以导入模块的特定名称，而不是加载模块中包含的所有元素。此外，我们可以通过用逗号分隔名称来导入多个元素。

注意，这里我们不需要使用点符号，因为我们直接从<*module _ name*>import<*element _ name*>’语法中导入带有**的名称。**

```
from **csv_get_module** import **display_file_location** as **dfl****dfl**("/User/Python/","ModulesExample.py")**Output:** File Location: /UserModulesExample.py
```

# 导入模块内的所有名称

我们也可以使用星号(*)直接导入模块的所有名称，尽管这不是一个好的做法。如果导入多个包含同名元素的模块，这可能会导致名称冲突。

```
from **csv_get_module** import *****
```

# 隐藏模块的元素

如果我们想要隐藏模块的一些元素，我们可以用下划线 **"_"** 开始命名元素。这种元素不能导入到外部文件中，因为这种命名约定使该元素成为模块本身的私有元素。

# 区分独立脚本运行和模块加载

包含 Python 代码的文件可以作为独立脚本运行，也可以作为模块加载到另一个代码结构中。

有时，考虑到这些用途，需要将 Python 文件中的代码分开。Python 有内置的 **__name__** 属性，当文件作为模块加载时，该属性为我们提供了**模块名称**。当文件作为独立脚本运行时，这次它返回 **__main__** 字符串。

```
**""" cvs_get_module.py
This module displays the summary of the tabular data contained in a CSV file 
"""**import pandas as pdprint("cvs_get_module is loaded")*def* **display_file_location(*path, file_name*)**:
 print("File Location: {}".format(path+filename))*class* **CSVGetInfo:**
 *def* __init__(*self*, *path*, *file_name*):
  self.path = path
  self.file_name = file_name

 *def* **display_summary(*self*):**
  data = pd.read_csv(self.path + self.file_name)
  print(self.file_name)
  print(data.info())if **__name__ == '__main__'**:**data_by_genres** = **CSVGetInfo**("/Users/erdemisbilen/Lessons/", "data_by_genres.csv")**display_file_location**(data_by_genres.path, data_by_genres.file_name)
data_by_genres.display_summary()
```

只有当脚本作为独立脚本运行时，上述 if 语句中的代码才会运行。

# Python 中的包

**包**将相似的模块放在一个单独的目录中。它们是包含相关模块和一个 **__init__ 的文件夹。py** 文件，用于可选的包级初始化。

**__init__。py** 在引用包内模块时执行一次。这个文件可以保留为空，或者可以选择实现包级初始化代码。

根据您的 Python 应用程序，您可以考虑将您的模块分组到子包中，例如 **doc、core、utils、data、examples、test。由于相似的模块保存在不同的文件夹中，这使得你的整体结构得到很好的组织和维护。**

![](img/967d24c8cfac674cbd4321832dc761e4.png)

Lynn Kintziger 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 从包中导入模块

我们可以使用包名(文件夹名)和点“.”来导入包内的模块接线员。

```
from **utils.csv_get_module** import **display_file_location** as **dfl****dfl**("/User/Python/","ModulesExample.py")**Output:** File Location: /UserModulesExample.py
```

# 关键要点

*   在 Python 中，组织大型代码块由**模块**和**包管理。**
*   这简化了代码结构，增加了代码的可重用性，并简化了维护和测试工作。

# 结论

在这篇文章中，我解释了 Python 中**模块和包**的基础知识。

这篇文章中的代码可以在我的 GitHub 库中找到。

我希望这篇文章对你有用。

感谢您的阅读！