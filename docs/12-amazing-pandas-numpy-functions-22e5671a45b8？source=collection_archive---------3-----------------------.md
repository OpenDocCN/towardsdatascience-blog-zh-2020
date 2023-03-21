# 12 只神奇的熊猫和数字功能

> 原文：<https://towardsdatascience.com/12-amazing-pandas-numpy-functions-22e5671a45b8?source=collection_archive---------3----------------------->

## 通过在您的分析中使用这些函数，让您的日常生活更加轻松

![](img/7b1305e7604ec5f48e973c107b1ce382.png)

礼貌:[https://pixabay.com/photos/code-programming-python-1084923/](https://pixabay.com/photos/code-programming-python-1084923/)

我们都知道熊猫和熊猫很神奇，它们在我们的日常分析中起着至关重要的作用。如果没有熊猫和 NumPy，我们将被遗弃在这个巨大的数据分析和科学世界中。今天，我将分享 12 个令人惊奇的熊猫和 NumPy 函数，它们将使你的生活和分析比以前容易得多。*最后，您可以在 Jupyter 笔记本上找到本文中使用的代码。*

**让我们从 NumPy 开始:**

NumPy 是使用 Python 进行科学计算的基础包。除其他外，它包含:

*   一个强大的 N 维数组对象
*   复杂的(广播)功能
*   集成 C/C++和 Fortran 代码的工具
*   有用的线性代数、傅立叶变换和随机数功能

除了其明显的科学用途，NumPy 还可以用作通用数据的高效多维容器。可以定义任意的数据类型。这使得 NumPy 可以无缝、快速地与各种数据库集成。

1.  **argpartition()**

NumPy 有这个神奇的功能，可以找到 N 个最大值索引。输出将是 N 个最大值的索引，然后我们可以根据需要对这些值进行排序。

```
x = np.array([12, 10, 12, 0, 6, 8, 9, 1, 16, 4, 6, 0])index_val = np.argpartition(x, -4)[-4:]
index_val
array([1, 8, 2, 0], dtype=int64)np.sort(x[index_val])
array([10, 12, 12, 16])
```

2. **allclose()**

Allclose()用于匹配两个数组并获得布尔值形式的输出。如果两个数组中的项在容差范围内不相等，它将返回 False。这是检查两个数组是否相似的好方法，而这实际上很难手工实现。

```
array1 = np.array([0.12,0.17,0.24,0.29])
array2 = np.array([0.13,0.19,0.26,0.31])# with a tolerance of 0.1, it should return False:
np.allclose(array1,array2,0.1)
False# with a tolerance of 0.2, it should return True:
np.allclose(array1,array2,0.2)
True
```

3.**剪辑()**

Clip()用于将数组中的值保持在一个区间内。有时，我们需要将值保持在一个上限和下限内。出于上述目的，我们可以利用 NumPy 的 clip()。给定一个区间，区间外的值被剪切到区间边缘。

```
x = np.array([3, 17, 14, 23, 2, 2, 6, 8, 1, 2, 16, 0])np.clip(x,2,5)
array([3, 5, 5, 5, 2, 2, 5, 5, 2, 2, 5, 2])
```

4.**提取()**

Extract()顾名思义，用于根据特定的条件从数组中提取特定的元素。使用 extract()，我们还可以使用类似于**和**和**或**的条件。

```
# Random integers
array = np.random.randint(20, size=12)
array
array([ 0,  1,  8, 19, 16, 18, 10, 11,  2, 13, 14,  3])#  Divide by 2 and check if remainder is 1
cond = np.mod(array, 2)==1
cond
array([False,  True, False,  True, False, False, False,  True, False, True, False,  True])# Use extract to get the values
np.extract(cond, array)
array([ 1, 19, 11, 13,  3])# Apply condition on extract directly
np.extract(((array < 3) | (array > 15)), array)
array([ 0,  1, 19, 16, 18,  2])
```

5. **where()**

其中()用于从满足特定条件的数组中返回元素。它返回符合特定条件的值的索引位置。这几乎类似于我们在 SQL 中使用的 where 条件，我将在下面的例子中演示它。

```
y = np.array([1,5,6,8,1,7,3,6,9])# Where y is greater than 5, returns index position
np.where(y>5)
array([2, 3, 5, 7, 8], dtype=int64),)# First will replace the values that match the condition, 
# second will replace the values that does not
np.where(y>5, "Hit", "Miss")
array(['Miss', 'Miss', 'Hit', 'Hit', 'Miss', 'Hit', 'Miss', 'Hit', 'Hit'],dtype='<U4')
```

6.**百分位()**

Percentile()用于计算沿指定轴的数组元素的第 n 个百分位数。

```
a = np.array([1,5,6,8,1,7,3,6,9])print("50th Percentile of a, axis = 0 : ",  
      np.percentile(a, 50, axis =0))
50th Percentile of a, axis = 0 :  6.0b = np.array([[10, 7, 4], [3, 2, 1]])print("30th Percentile of b, axis = 0 : ",  
      np.percentile(b, 30, axis =0))
30th Percentile of b, axis = 0 :  [5.1 3.5 1.9]
```

让我知道你以前是否用过它们，它对你有多大帮助。让我们继续看神奇的熊猫。

**熊猫:**

pandas 是一个 Python 包，提供了快速、灵活、富于表现力的数据结构，旨在使结构化(表格、多维、潜在异构)和时间序列数据的处理变得既简单又直观。

pandas 非常适合许多不同类型的数据:

*   具有不同类型列的表格数据，如在 SQL 表或 Excel 电子表格中
*   有序和无序(不一定是固定频率)时间序列数据。
*   带有行和列标签的任意矩阵数据(同类或异类)
*   任何其他形式的观察/统计数据集。这些数据实际上根本不需要标记就可以放入 pandas 数据结构中。

以下是熊猫擅长的几件事:

*   轻松处理浮点和非浮点数据中的缺失数据(表示为 NaN)
*   大小可变性:可以在数据帧和高维对象中插入和删除列
*   自动和明确的数据对齐:对象可以明确地与一组标签对齐，或者用户可以简单地忽略标签，让系列、数据框等。在计算中自动调整数据
*   强大、灵活的分组功能，可对数据集执行拆分-应用-组合操作，用于聚合和转换数据
*   使将其他 Python 和 NumPy 数据结构中粗糙的、不同索引的数据转换成 DataFrame 对象变得容易
*   大型数据集的智能基于标签的切片、花式索引和子集化
*   直观的合并和连接数据集
*   数据集的灵活整形和旋转
*   轴的分层标签(每个刻度可能有多个标签)
*   强大的 IO 工具，用于从平面文件(CSV 和带分隔符文件)、Excel 文件、数据库加载数据，以及从超快速 HDF5 格式保存/加载数据
*   特定于时间序列的功能:日期范围生成和频率转换、移动窗口统计、日期移动和滞后。

1.  **read_csv(nrows=n)**

您可能已经知道 read_csv 函数的用法。但是，我们中的大多数人仍然会犯一个错误。csv 文件，即使它不是必需的。让我们考虑这样一种情况，我们不知道一个 10gb 的. csv 文件中存在的列和数据，整个读取。csv 文件在这里不会是一个聪明的决定，因为它会不必要的使用我们的内存，并会花费大量的时间。我们可以只从。csv 文件，然后根据我们的需要进一步进行。

```
import io
import requests# I am using this online data set just to make things easier for you guys
url = "[https://raw.github.com/vincentarelbundock/Rdatasets/master/csv/datasets/AirPassengers.csv](https://raw.github.com/vincentarelbundock/Rdatasets/master/csv/datasets/AirPassengers.csv)"
s = requests.get(url).content# read only first 10 rows
df = pd.read_csv(io.StringIO(s.decode('utf-8')),nrows=10 , index_col=0)
```

2.**地图()**

map()函数用于根据输入的对应关系映射系列的值。用于将一个数列中的每一个值替换为另一个值，该值可以是从一个函数、一个字典或一个数列中导出的。

```
# create a dataframe
dframe = pd.DataFrame(np.random.randn(4, 3), columns=list('bde'), index=['India', 'USA', 'China', 'Russia'])#compute a formatted string from each floating point value in frame
changefn = lambda x: '%.2f' % x# Make changes element-wise
dframe['d'].map(changefn)
```

3. **apply()**

apply()允许用户传递一个函数，并将其应用于 Pandas 系列的每个值。

```
# max minus mix lambda fn
fn = lambda x: x.max() - x.min()# Apply this on dframe that we've just created above
dframe.apply(fn)
```

4. **isin()**

isin()用于过滤数据帧。isin()有助于选择在特定列中具有特定(或多个)值的行。这是我遇到的最有用的功能。

```
# Using the dataframe we created for read_csv
filter1 = df["value"].isin([112]) 
filter2 = df["time"].isin([1949.000000])df [filter1 & filter2]
```

5.**复制()**

*copy()*用于创建一个熊猫对象的副本。当您将一个数据框分配给另一个数据框时，当您在另一个数据框中进行更改时，其值也会发生变化。为了防止上述问题，我们可以使用 copy()。

```
# creating sample series 
data = pd.Series(['India', 'Pakistan', 'China', 'Mongolia'])# Assigning issue that we face
data1= data
# Change a value
data1[0]='USA'
# Also changes value in old dataframe
data# To prevent that, we use
# creating copy of series 
new = data.copy()# assigning new values 
new[1]='Changed value'# printing data 
print(new) 
print(data)
```

6. **select_dtypes()**

select_dtypes()函数根据列 dtypes 返回数据框列的子集。此函数的参数可以设置为包括所有具有某种特定数据类型的列，也可以设置为排除所有具有某种特定数据类型的列。

```
# We'll use the same dataframe that we used for read_csv
framex =  df.select_dtypes(include="float64")# Returns only time column
```

**奖励:**

**pivot_table()**

熊猫最神奇最有用的功能就是 pivot_table。如果您对使用 groupby 犹豫不决，并且希望扩展它的功能，那么您可以使用 pivot_table。如果您知道 excel 中的数据透视表是如何工作的，那么这对您来说可能是小菜一碟。数据透视表中的级别将存储在结果数据帧的索引和列上的多索引对象(分层索引)中。

```
# Create a sample dataframe
school = pd.DataFrame({'A': ['Jay', 'Usher', 'Nicky', 'Romero', 'Will'], 
      'B': ['Masters', 'Graduate', 'Graduate', 'Masters', 'Graduate'], 
      'C': [26, 22, 20, 23, 24]}) # Lets create a pivot table to segregate students based on age and course
table = pd.pivot_table(school, values ='A', index =['B', 'C'], 
                         columns =['B'], aggfunc = np.sum, fill_value="Not Available") 

table
```

如果你们遇到或使用过其他神奇的功能，请在下面的评论中告诉我。我很想知道更多关于他们的事情。

> **Jupyter 笔记本(使用代码):**[https://github . com/kunaldhariwal/Medium-12-Amazing-Pandas-NumPy-Functions](https://github.com/kunaldhariwal/Medium-12-Amazing-Pandas-NumPy-Functions)
> 
> ***LinkedIn****:*[*https://bit.ly/2u4YPoF*](https://bit.ly/2u4YPoF)

我希望这有助于增强你的知识基础:)

更多信息请关注我！

感谢您的阅读和宝贵时间！