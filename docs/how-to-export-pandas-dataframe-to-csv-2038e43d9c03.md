# 如何将熊猫数据帧导出为 CSV 格式

> 原文：<https://towardsdatascience.com/how-to-export-pandas-dataframe-to-csv-2038e43d9c03?source=collection_archive---------0----------------------->

## 在这篇文章中，我们将讨论如何将数据帧写入 CSV 文件。

![](img/f41187087aff7a8031e751ea7004008f.png)

由[绝对视觉](https://unsplash.com/@freegraphictoday?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 简短回答

最简单的方法是:

```
df.to_csv('file_name.csv')
```

如果想不带索引导出，只需添加`index=False`；

```
df.to_csv('file_name.csv', index=False)
```

如果得到`UnicodeEncodeError`，简单加`encoding='utf-8'`；

```
df.to_csv('file_name.csv', encoding='utf-8')
```

# 概述熊猫数据框架

熊猫数据框架创建了一个带有标签轴(行和列)的 excel 数据结构。要定义一个数据帧，至少需要数据行和列名(标题)。

这里有一个熊猫数据帧的例子:

熊猫数据框架是类似 Excel 的数据

生成数据帧的代码:

# 将数据帧导出到 CSV 文件中

Pandas DataFrame `to_csv()`函数将数据帧导出为 CSV 格式。如果提供了文件参数，输出将是 CSV 文件。否则，返回值是类似字符串的 CSV 格式。

以下是一些选项:

**path_or_buf** :文件或字符串的字符串路径

```
dt.to_csv('file_name.csv’) # relative position
dt.to_csv('C:/Users/abc/Desktop/file_name.csv')
```

**sep** :为 CSV 输出指定一个自定义分隔符，默认为逗号。

```
dt.to_csv('file_name.csv',sep='\t') # Use Tab to seperate data
```

**na_rep** :类似 NaN 的缺失值的字符串表示。默认值为“”。

```
dt.to_csv('file_name.csv',na_rep='Unkown') # missing value save as Unknown
```

**float_format:** 浮点数的格式字符串。

```
dt.to_csv('file_name.csv',float_format='%.2f') # rounded to two decimals
```

**表头:**是否导出列名。默认值为 True。

```
dt.to_csv('file_name.csv',header=False)
```

**列**:要写入的列。默认值为 None，每一列都将导出为 CSV 格式。如果设置，只有*列*会被导出。

```
dt.to_csv('file_name.csv',columns=['name'])
```

**索引**:是否写行索引号。默认值为 True。

```
dt.to_csv('file_name.csv',index=False)
```

# 附录

写入 CSV 文件的常见场景

DataFrame to_csv()的语法是:

```
DataFrame.to_csv(
  self, 
  path_or_buf, 
  sep: str = ',', 
  na_rep: str = '', 
  float_format: Union[str, NoneType] = None, 
  columns: Union[Sequence[Union[Hashable, NoneType]], NoneType] =    None, 
  header: Union[bool, List[str]] = True, 
  index: bool = True, 
  index_label: Union[bool, str, 
  Sequence[Union[Hashable, NoneType]], NoneType] = None, 
  mode: str = 'w', 
  encoding: Union[str, NoneType] = None, 
  compression: Union[str, Mapping[str, str], NoneType] = 'infer',  
  quoting: Union[int, NoneType] = None, 
  quotechar: str = '"', 
  line_terminator: Union[str, NoneType] = None, 
  chunksize: Union[int, NoneType] = None, 
  date_format: Union[str, NoneType] = None, 
  doublequote: bool = True, 
  escapechar: Union[str, NoneType] = None, 
  decimal: Union[str, NoneType] = '.')
```

使用 to_csv 的常见场景