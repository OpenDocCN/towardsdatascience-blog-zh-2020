# 探索 Python 列表的方法和技巧

> 原文：<https://towardsdatascience.com/exploring-methods-and-tips-for-python-lists-7bae558ccb74?source=collection_archive---------51----------------------->

## Python 列表非常强大💪🏻

![](img/469f32540ea2ce7328c69b58716476b8.png)

照片由[艾玛·马修斯在](https://unsplash.com/@emmamatthews?utm_source=medium&utm_medium=referral) [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上进行数字内容制作

上周，我开始复习 Python 的一些基础知识，并决定创建一个存储库，在那里为我修改的所有内容创建一个笔记本。这将使我能够在将来任何时候再次回顾某些话题时，都能够找到一个单独的地方。

上周我浏览了 Python 列表，这篇文章包括了笔记本中的一部分内容，突出了 Python 列表的关键功能。当我有机会浏览其他主题时，我会继续创作这些笔记本。

[](https://github.com/kb22/python-notebooks) [## kb22/python-笔记本

### 该库包括一系列 Python 笔记本，描述了 Python 中几种有用的方法和技巧。…

github.com](https://github.com/kb22/python-notebooks) 

# 反转列表— []

我们可以在 Python 列表中使用`[]`操作符的第三个参数来返回一个反向列表。当我们将第三个值设置为`-1`时，我们指示我们希望在从末尾走一步的同时遍历列表，因此是反转的列表。

```
long_list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]long_list[::-1]# Output
[10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
```

# 数字列表—范围()

我们使用`range()`方法来实现这一点。它需要三个参数，起始值、最终值和每个值之后的增量步长。例如，如果第一个值是 0，增量步长是 10，则列表中的下一个数字是 10，然后是 20，依此类推。默认情况下，`range()`不生成列表，所以我们使用`list()`方法生成完整的列表。

```
list(range(0, 100, 10))# Output
[0, 10, 20, 30, 40, 50, 60, 70, 80, 90]
```

# 具有相同值的列表— *

假设您想要为每个元素创建一个具有相同值的列表。您可以从单个元素列表开始，然后将列表乘以您希望该元素在新列表中出现的次数。

```
["element"]*5# Output
['element', 'element', 'element', 'element', 'element']
```

# 元素到字符串 join()

我们可以将列表中的各种元素组合成一个字符串。我们使用`join()`方法，并向它提供包含我们的元素的列表的名称。接下来是一个字符或一个字符列表，列表中的每个元素都要通过这个字符或列表进行连接。在下面的例子中，我用一个空格组合了各种元素。

```
separated_list = ["I", "am", "Karan"]
" ".join(separated_list)# Output
'I am Karan'
```

# 对列表进行切片— slice()

我们可以使用`slice()`方法来分割一个给定的列表，并从原始列表中检索某些元素。它需要起始索引、结束索引和增量步长。它也可以接受负的索引值。这个例子展示了我们如何检索一个 10 元素列表的最后 5 个元素。

```
long_list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]last_five_elements = slice(-5, 10, 1)
long_list[last_five_elements]# Output
[6, 7, 8, 9, 10]
```

# 排序—排序()

列表有一个内置的函数，允许我们对列表中的元素进行排序。我们也可以将`reverse`参数设置为 True 来反转列表。

```
long_list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]long_list.sort(reverse = **True**)# Output
[10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
```

# 检查列表中的存在性— any()与 all()

假设您有一个布尔列表，您想检查其中的`True`值。如果你想检查是否至少有一个`True`值存在，你可以使用`any()`方法，但是如果你想检查所有的值都是`True`，我们使用 all()。

```
arr = [**False**, **False**, **False**, **True**, **False**, **False**]print("At least one True - **{}**".format(any(arr)))
print("All True - **{}**".format(all(arr)))# Output
At least one True - True
All True - False
```

# 使用索引进行迭代—枚举()

`enumerate()`方法允许我们迭代 Python 列表，同时提取每一步的值以及元素的索引。

```
long_list = [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]**for** index, value **in** enumerate(long_list):
    print("At index **{}**, value is **{}**".format(index, value))# Output
At index 0, value is 10
At index 1, value is 9 
At index 2, value is 8 
At index 3, value is 7 
At index 4, value is 6 
At index 5, value is 5 
At index 6, value is 4 
At index 7, value is 3 
At index 8, value is 2 
At index 9, value is 1
```

# 检查列表中元素的存在性—在

我们使用`in`操作符来查看一个元素是否存在于列表中。我们也可以使用`not in`来检查一个元素是否不存在。

```
long_list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]1 **in** long_list
1 not **in** long_list# Output
True
False
```

# 空列表检查—不是

我们可以在列表前面使用`not`操作符，如果它返回 True，我们知道列表是空的。

```
empty_list = []**not** empty_list# Output
True
```

# 对每个元素的操作— map()

我们使用`map()`方法在列表的每个元素上应用一个给定的函数。这里，我定义了一个使输入值加倍的函数。

```
long_list = [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]**def** double(x):
   **return** 2*x

list(map(double, long_list))# Output
[20, 18, 16, 14, 12, 10, 8, 6, 4, 2]
```

# 迭代多个列表— zip

`zip()`方法允许我们遍历多个列表，其中返回的对象是一个元组列表，其第一个元素属于第一个列表，第二个元素属于第二个列表。

```
list_1 = [1, 2, 3, 4]
list_2 = [5, 6, 7, 8]**for** item1, item2 **in** zip(list_1, list_2):
    print("**{}** - **{}**".format(item1, item2))# Output
1 - 5
2 - 6
3 - 7
4 - 8
```

# 结论

Python 列表非常强大，即使没有其他像`numpy`这样的高级库也可以非常有效。

我希望你喜欢我的作品。如果您有任何问题、建议或意见，请告诉我。