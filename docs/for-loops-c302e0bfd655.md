# 对于循环

> 原文：<https://towardsdatascience.com/for-loops-c302e0bfd655?source=collection_archive---------64----------------------->

## 看看 Python 编码中最基本的概念之一

![](img/406fb5dbc5939714ae707ba01bad34fc.png)

For 循环允许我们迭代 Python 中的列表、元组、集合和字典等集合。虽然您可以选择在单独的代码行中操作每一项，但这将是非常低效的，尤其是当列表非常长的时候。

```
nums = [1, 2, 3, 4, 5]print (nums[0])
print (nums[1])
print (nums[2])
print (nums[3])
print (nums[4])1
2
3
4
5
```

For 循环使编码人员能够编写一行代码来根据需要修改任意多的项目。该方法逐个检查集合中的每一项，并执行 for 循环声明中的任何操作。for 循环的标准语法如下:

```
**for** number **in** [1, 2, 3, 4, 5]**:**
```

1.  **一词为**
2.  您选择的占位符变量名，它将*引用当前正在迭代的项目*
3.  中的**一词**
4.  您正在迭代的集合——可以是集合本身，如上面的示例，也可以是代表集合的变量名，如下面的示例
5.  冒号( **:** )
6.  要对集合中的每一项执行的缩进代码块

每个 for 循环都遵循这个基本结构。

![](img/5863a0ac6455086827b1c5245a7c3aa1.png)

由 [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/list?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

如果您想像在第一个代码块中一样打印列表中的每个数字，使用 for 循环的代码和结果将如下所示:

```
nums = [1, 2, 3, 4, 5]**for** num **in** nums**:
**    print (num)1
2
3
4
5
```

一旦缩进块中的代码完成，它就对集合中的下一项运行，直到没有其他项。之后，缩进块之外的下一行代码将运行。

```
nums = [1, 2, 3, 4, 5]**for** num **in** nums**:
**    print (num)
print ("That's all folks!")1
2
3
4
5
That's all folks!
```

虽然使用 for 循环迭代列表、元组和集合看起来非常相似，但使用 for 循环迭代字典看起来略有不同。

字典包含*键，值*对，而不是单个元素。出于这个原因，我们的占位符变量名*引用了当前正在迭代的项目*，现在将有两个名称，一个是键*的名称*，一个是值*的名称*，用逗号分隔。默认情况下，如果我们只使用*一个*变量名来遍历字典，那么只有键会被选中。如果我们希望获得键*和*值，我们必须首先获得字典的**条目**。

```
players = {'Eli': 10, 'Saquon': 26, 'Odell': 13} print (players**.items()**)**dict_items**([('Eli', 10), ('Saquon', 26), ('Odell', 13)])
```

这里我们看到 *dict_items* 看起来像一个列表，其中每个项目都有两部分，即*键*和*值*。

现在，鉴于这一点:

```
players = {'Eli': 10, 'Saquon': 26, 'Odell': 13}**for** name **in** players**:**
    print (name)Eli
Saquon
Odell **for** name, number **in** players**.items():
**    print ("This is the name: ", name)
    print ("This is the number: ", number)
    print ("")This is the name: Eli
This is the number: 10This is the name: Saquon
This is the number: 26This is the name: Odell
This is the number: 13
```

正如我们所看到的，for 循环允许我们只用一行代码就对许多项执行操作，后面是我们希望对每一项执行什么操作的指示。For 循环是 Python 代码的一个重要方面，许多其他语言的代码也是如此。作为一名编码人员，你可能会处理许多不同的数据，你希望对这些数据执行相同的操作，你会经常使用 for 循环。因此，了解它们的工作原理和内部情况非常重要。