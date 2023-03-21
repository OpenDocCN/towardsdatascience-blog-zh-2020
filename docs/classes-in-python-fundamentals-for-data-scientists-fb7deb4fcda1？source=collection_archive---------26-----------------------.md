# Python 中的类:数据科学家的基础

> 原文：<https://towardsdatascience.com/classes-in-python-fundamentals-for-data-scientists-fb7deb4fcda1?source=collection_archive---------26----------------------->

## 用一个具体的例子来理解基础！

![](img/42e914e8809f3ed4ed7ef2f77eb8fba8.png)

图片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Alexandru Acea](https://unsplash.com/@alexacea?utm_source=medium&utm_medium=referral) 拍摄

类通过将数据和函数封装在一个实体中来帮助我们整理代码。此外，它们增加了代码的可重用性和可伸缩性，简化了调试过程。

一旦我们定义了一个类，我们就可以使用这个类的实例来处理特定的任务。在这方面，它们是我们在代码中使用的对象的蓝图。

让我们用一个具体的例子来了解一下基础知识。

# **定义类别**

在我们的例子中，我将使用 [Pandas](https://pandas.pydata.org) ，这是一个流行的 Python 数据操作库，以及几个包含表格数据的 [CSV 文件](https://www.kaggle.com/yamaerenay/spotify-dataset-19212020-160k-tracks)。

在 Python 中，基本类定义具有以下格式:

```
classClassName():
 # Comments / Notes
 ...
 # Initializer / Instance Attributes
 ...
 # Methods
 ...
```

首先，让我们创建一个简单的类，帮助我们显示 CSV 文件中包含的表格数据的摘要信息。

```
*class* CSVGetInfo:
**""" This class displays the summary of the tabular data contained in a CSV file """***def* **__init__**(*self*, *path*, *file_name*):
 self.path = path
 self.file_name = file_name*def* **display_summary**(*self*):
 data = pd.read_csv(self.path + self.file_name)
 print(self.file_name)
 print(data.info())
```

在 Python 中，一个简单的类定义以下面的符号开始:

```
*class* ClassName:
```

在我们的例子中，我们的类名是**“CSV getinfo”**。请注意 Python 中的类名遵循 **UpperCaseCamelCase** 命名约定。虽然这不是强制性的，但是通常将类存储在一个单独的文件中，并在以后需要时导入它们。

增加一个评论区让其他人知道我们的类是做什么的，这是一个很好的做法。我们可以通过在三重双引号内提供注释来做到这一点。

```
*class* CSVGetInfo:
**""" This class displays the summary of the tabular data contained in a CSV file """**
```

# **初始化实例变量(属性)**

在这些最初的步骤之后，现在是时候定义我们的类所涉及的数据了。在我们的例子中，我们需要知道想要读取的 CSV 文件的路径和文件名信息。

所以，我们的类有**‘path’**和**‘file _ name’**变量。它们将为我们的 **CSVGetInfo** 类的每个实例初始化，它们被称为我们类的实例变量或属性。

```
**# Initializer / Instance Attributes** 
 *def* __init__(*self*, *path*, *file_name*):
  self.path = path
  self.file_name = file_name
```

**__init__** 方法指出当我们创建类的新实例时必须提供什么数据。

本地变量提供的数据存储在 ***self.path*** 和 ***self.file_name 中。*** 在这里， *self* 代表实例本身。

在创建实例时，变量初始化是一个强制性的过程，这个特殊的 **__init__** 方法保证了它总是发生。

![](img/00de90f453d84e748f79cce493481312.png)

由 [Tra Tran](https://unsplash.com/@tratran?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# **添加实例函数(方法)**

现在，我们可以定义使用这些数据来处理特定任务的函数。类结构中的函数称为方法或实例函数，因为它们与类的每个实例都有联系。我们将在我们的 **CSVGetInfo** 类结构中定义 **"display_summary"** 方法，它显示包含在 CSV 文件中的表格数据的摘要信息。

```
**# M****ethods***def* **display_summary**(*self*):
 data = pd.read_csv(self.path + self.file_name)
 print(self.file_name)
 print(data.info())
```

注意，我们在方法中再次使用了 *self* 作为参数。这是访问我们正在处理的实例中包含的数据所需要的。

为了访问 **"display_summary"** 方法中的实例变量，我们使用 self.path 和 self.file_name 符号。

# **创建类(对象)的实例**

由于类本身是对象的蓝图，我们需要创建类(对象)的实例，以使用类定义中可用的方法来处理任务。

在我们的类中，我们有**“display _ summary”**方法。

因此，让我们创建两个 **CSVGetInfo** 的实例，并使用**“display _ summary”**方法来查看 CSV 文件中的内容。

为了创建一个类的实例/对象，我们使用类名并将值传递给该类所需的参数。

```
data_by_artists = CSVGetInfo("/Users/erdemisbilen/Lessons/", "data_by_artists.csv")data_by_genres = CSVGetInfo("/Users/erdemisbilen/Lessons/", "data_by_genres.csv")
```

现在，变量 **data_by_artist** 保存了对我们的 **CSVGetInfo** 类的第一个实例的引用。它是 **CSVGetInfo** 类的对象/实例，将**“data _ by _ artists . CSV”**的值存储在 **file_name** (实例变量)中。其他变量**data _ by _ 流派**保存了对我们类的第二个实例的引用，该实例的**“data _ by _ 流派. CSV”**值存储在**文件名**中。尽管它们是用相同的类定义创建的，但是它们在实例变量(属性)中存储了不同的值。

```
print(data_by_artists)
print(data_by_genres)**#Output**
<__main__.CSVGetInfo instance at 0x10b84f248>
<__main__.CSVGetInfo instance at 0x114eb58c0>
```

当我们打印实例时，我们可以看到它们存储在内存的不同位置。

![](img/012e2531dc0a17a470d112f54b1ac561.png)

彼得·德·卢奇亚在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

让我们用**【展示 _ 总结】**的方法。

```
data_by_artists.display_summary()
data_by_genres.display_summary()**#Output
data_by_artists.csv**
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 27606 entries, 0 to 27605
Data columns (total 15 columns):
artists             27606 non-null object
acousticness        27606 non-null float64
danceability        27606 non-null float64
duration_ms         27606 non-null float64
energy              27606 non-null float64
instrumentalness    27606 non-null float64
liveness            27606 non-null float64
loudness            27606 non-null float64
speechiness         27606 non-null float64
tempo               27606 non-null float64
valence             27606 non-null float64
popularity          27606 non-null float64
key                 27606 non-null int64
mode                27606 non-null int64
count               27606 non-null int64
dtypes: float64(11), int64(3), object(1)
memory usage: 3.2+ MB
None**data_by_genres.csv**
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 2617 entries, 0 to 2616
Data columns (total 14 columns):
genres              2617 non-null object
acousticness        2617 non-null float64
danceability        2617 non-null float64
duration_ms         2617 non-null float64
energy              2617 non-null float64
instrumentalness    2617 non-null float64
liveness            2617 non-null float64
loudness            2617 non-null float64
speechiness         2617 non-null float64
tempo               2617 non-null float64
valence             2617 non-null float64
popularity          2617 non-null float64
key                 2617 non-null int64
mode                2617 non-null int64
dtypes: float64(11), int64(2), object(1)
memory usage: 286.3+ KB
None
```

# 结论

在这篇文章中，我解释了 Python 中类的基础知识。

这篇文章中的代码和使用的 CSV 文件可以在 [my GitHub repository](https://github.com/eisbilen/CSVGetInfo) 中找到。

我希望这篇文章对你有用。

感谢您的阅读！