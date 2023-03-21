# Python 列表方法

> 原文：<https://towardsdatascience.com/python-list-methods-fa7c53010300?source=collection_archive---------15----------------------->

## Python 必须提供的列表方法的总结

![](img/3c2993d3bb8147a3f64d624021ea42ea.png)

马库斯·温克勒在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# 什么是列表，什么是列表方法？

在 Python 中，列表是数据片段的集合。列表由方括号[ ]包围，每个项目由逗号(，)分隔，可以包含从零到无穷大的任何项目(或者您的计算机允许的任何数量)。字符串、数字、布尔值，甚至其他列表都可以是列表中的项目。列表是有序的和可变的(可改变的)，这意味着每个项目被分配到一个特定的索引，可以排序，并有能力被改变。一旦你有了一个列表，你就可以使用所谓的“方法”来操作这个列表。像字符串方法一样，要使用列表方法，只需编写列表，后跟。【方法】()。例如，要在列表['意大利香肠'，'香肠'，'蘑菇']上运行 *append()* 方法，您只需编写['意大利香肠'，'香肠'，'蘑菇']。追加('洋葱')；如果该列表被设置为一个变量，您应该使用 variable.append('onion ')。正如您在 *append()* 示例中看到的，一些方法接受所谓的“参数”，这些参数放在括号中，进一步定义了该方法将做什么。Python 语言有很多内置的方法，比如 *append()* ，允许你轻松地改变列表。

## append()和 extend()

**append()** 方法允许您将另一个项目添加到列表的末尾。该方法使用*一个必需的参数*，这是您希望添加到列表中的项目。

```
Syntax: *list*.append(*item*)**toppings = ['pepperoni', 'sausage', 'mushroom']**toppings.append('onion') 
--> ['pepperoni', 'sausage', 'mushroom', 'onion']
```

**extend()** 方法类似于 append()，因为它允许您添加到列表中；但是，extend()方法允许您添加另一个 iterable(列表、元组、集合等)中的所有项。)作为单独的项目而不是一个项目添加到列表的末尾。该方法采用一个必需的参数，即 iterable。

```
Syntax: *list*.extend(*iterable*)**toppings = ['pepperoni', 'sausage', 'mushroom']
more_toppings = ['onion', 'bacon']**toppings.**extend**(more_toppings)
--> ['pepperoni', 'sausage', 'mushroom', 'onion', 'bacon'] *To contrast...**toppings.****append****(more_toppings)
--> ['pepperoni', 'sausage', 'mushroom', ['onion', 'bacon']]*
```

如上所述，当 extend()方法用于 iterable 时，*iterable 中的每一项*都被添加到列表中，作为*不再有界的单独项*。相反，当 append()方法使用 iterable 作为参数时，*整个* iterable 作为*一项*添加到列表中。密切注意列表中的逗号和括号总是很重要的。

## pop()和 remove()

**pop()** 方法允许您从列表中移除指定索引值处的元素。该方法可以接受*一个可选参数*，即您希望删除的索引的整数值——默认情况下，pop()将删除列表中的最后一项，因为默认值为-1。

```
Syntax: *list*.pop(*index*)**toppings = ['pepperoni', 'sausage', 'mushroom']**toppings.pop(1)
--> ['pepperoni', 'mushroom']toppings.pop()
--> ['pepperoni', 'sausage'] *If you wanted to retrieve the removed item...*extra = toppings.pop(1)
extra --> 'sausage'
```

像 pop()方法一样， **remove()** 方法允许您从列表中删除一个项目。不过，remove()方法删除列表中第一个出现的指定值。该方法使用一个必需的参数，即您希望移除的项目。

```
Syntax: *list*.remove(*item*)**toppings = ['pepperoni', 'sausage', 'mushroom']**toppings.remove('sausage')
--> ['pepperoni', 'mushroom']
```

对于 pop()和 remove()，如果参数超出范围或不在列表中，您将分别得到一个错误。

## 排序()和反转()

方法按照一定的标准对列表进行排序。该方法可以带两个可选参数。第一个参数是设置 *reverse=True* 或 *reverse=False* 。默认情况下，该参数设置为 *reverse=False* ，如果列表仅包含字符串，则按字母顺序排列；如果列表仅包含数字，则按升序排列。第二个参数允许您将一个 *key=* 设置为一个函数，如果它比 sort()方法的默认排序更复杂，您可以使用该函数来指定您希望列表如何精确排序。这可能是一个内置的 Python 函数，一个你在程序中其他地方定义的函数，或者是你写的一个内嵌的λ函数。

```
Syntax: *list*.sort(reverse=True|False, key=function)**toppings = ['pepperoni', 'sausage', 'mushroom']**toppings.sort()
--> ['mushroom', 'pepperoni', 'sausage']toppings.sort(reverse=True)
--> ['sausage', 'pepperoni', 'mushroom']toppings.sort(reverse=True, key=lambda x: len(x))
--> ['pepperoni', 'mushroom', 'sausage']
** Sorted in reverse order by length of the topping name* **prices = [1.50, 2.00, 0.50]**prices.sort(reverse=False)
--> [0.50, 1.50, 2.00]prices.sort(reverse=True)
--> [2.00, 1.50, 0.50]  **pies = [['bacon', 'ranch'], ['sausage', 'peppers']]**pies.sort(reverse=True)
--> [['sausage', 'peppers'], ['bacon', 'ranch']]
** Sorts iterators by their first value*
```

方法只是颠倒了列表中条目的顺序。该方法不带*任何参数*。

```
Syntax: *list*.reverse()**toppings = ['pepperoni', 'sausage', 'mushroom']**toppings.reverse()
--> ['mushroom', 'sausage', 'pepperoni'] **prices = [1.50, 2.00, 0.50]**prices.reverse()
--> [0.50, 2.00, 1.50]
```

## 计数()

方法的作用是:返回一个列表中指定条目出现的次数。该方法接受一个必需的参数，这是您希望计算其计数的项目。如果您希望找出哪些项目在列表中出现了不止一次，这种方法会很有用。

```
Syntax: *list*.count(*item*)**toppings = ['pepperoni', 'sausage', 'mushroom', 'sausage']**toppings.count('sausage')
--> 2toppings.count('pepperoni')
--> 1toppings.count('bacon')
--> 0
```

## 索引()

**index()** 方法返回指定项目第一次出现的索引。该方法使用一个必需的参数，这是您希望查找其索引的项目。如果该项目不在列表中，您将得到一个错误。

```
Syntax: *list*.index(*item*)**toppings = ['pepperoni', 'sausage', 'mushroom', 'sausage']**toppings.index('mushroom')
--> 2toppings.index('pepperoni')
--> 0
```

## 插入()

方法的作用是:将一个指定的条目插入到一个指定索引的列表中。该方法采用两个必需的参数——您希望插入值的整数索引和您希望插入的项目。

```
Syntax: *list*.insert(*index*, index)**toppings = ['pepperoni', 'sausage', 'mushroom']**toppings.insert(1, 'onion')
--> ['pepperoni', 'onion', 'sausage', 'mushroom']
```

## 复制()

**copy()** 方法只是返回你的列表的一个副本。该方法不带*参数*。

```
Syntax: *list*.copy()**toppings = ['pepperoni', 'sausage', 'mushroom']**toppings**2** = toppings.copy()
toppings**2** --> ['pepperoni', 'sausage', 'mushroom']
```

## 清除()

**clear()** 方法只是从一个列表中删除所有的条目，留下一个空列表。该方法采用*无参数*。

```
Syntax: *list*.clear()**toppings = ['pepperoni', 'sausage', 'mushroom']**toppings.clear()
--> []
```

仅此而已。有了上述这些方法，您应该可以对 Python 列表执行所需的操作。更多信息和例子，请务必查看 [w3schools 列表](https://www.w3schools.com/python/python_ref_list.asp)。

祝你编码愉快，一定要看看我下面关于 Python 字符串方法的博客！

[](/python-string-methods-7ac76ed7590b) [## Python 字符串方法

### Python 中最常用的字符串方法的总结

towardsdatascience.com](/python-string-methods-7ac76ed7590b) 

## 参考资料:

[](https://www.w3schools.com/python/python_ref_list.asp) [## Python 列表/数组方法

### Python 有一组内置的方法，可以用在列表/数组上。注意:Python 没有对…的内置支持

www.w3schools.com](https://www.w3schools.com/python/python_ref_list.asp)