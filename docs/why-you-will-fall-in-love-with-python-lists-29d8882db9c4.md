# 为什么你会爱上 Python 列表

> 原文：<https://towardsdatascience.com/why-you-will-fall-in-love-with-python-lists-29d8882db9c4?source=collection_archive---------36----------------------->

## 列表是有序的和可变的美味

![](img/21a17d79d3ddaeb7796400d47b784ae5.png)

照片由[元素 5 数码](https://www.pexels.com/@element5?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从[像素](https://www.pexels.com/photo/file-cabinets-1370294/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

当你开始着手你的项目时，你需要选择如何解决你的问题。在 Python 中你会经常用到列表，所以你可能已经习惯了。列表是很好的工作方式。在本文中，我想向您展示列表的各种用例。

# 创建列表

列表的语法是`[]`。如果你想创建一个列表，有几种方法可以做到这一点。第一种方法是手动为其赋值:

```
my_list = ['Hello' , 'World']
```

逗号分隔列表中的元素。

您可以使用方法`list()`基于其他值或直接创建一个列表。

```
word = 'listme'
word_list = list(word)
print(word_list)*>>> ['l', 'i', 's', 't', 'm', 'e']*new_list = list('super great')
print(new_list)*>>> ['s', 'u', 'p', 'e', 'r', ' ', 'g', 'r', 'e', 'a', 't']*
```

列表可以包含多种类型。这是一个有效列表:

```
sick_list = ['a' , 1 , ('x','y','z') , ['eggs' , 'milk'] , {'eggs' : 4 , 'milk' : 3}]
```

您可以像这样检查您的列表中有哪些类型:

```
for element in sick_list:
    print(f'element {element} is type: {type(element)}')*>>>
element a is type: <class 'str'>
element 1 is type: <class 'int'>
element ('x', 'y', 'z') is type: <class 'tuple'>
element ['eggs', 'milk'] is type: <class 'list'>
element {'eggs': 4, 'milk': 3} is type: <class 'dict'>*
```

有时，您可能需要基于输入、数学或其他随机值创建列表。你可能会遇到这些:

```
my_list = ['a']*10
print(my_list)*>>> ['a', 'a', 'a', 'a', 'a', 'a', 'a', 'a', 'a', 'a']*my_other_list = list(range(0,13))
print(my_other_list)*>>> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]*
```

# 访问元素

有几种方法可以访问列表中的元素。根据你想做的事情，如果你寻找的话，这些元素应该在那里。

## 环

如果希望遍历 python 列表中的元素，可以使用`for-loop`。

如果您只需要值，您可以像这样简单地遍历它们:

```
my_list = [1,2,3,'a','b','c']
for element in my_list:
    print(element)
```

有时你可能也需要索引。如果你来自另一种语言，像这样使用`range(len())`可能很自然:

```
my_list = [1,2,3,'a','b','c']
for element in range(len(my_list)):
    print(f'index: {element} , value: {my_list[element]}')*>>>
index: 0 , value: 1
index: 1 , value: 2
index: 2 , value: 3
index: 3 , value: a
index: 4 , value: b
index: 5 , value: c*
```

虽然它打印的是您想要的东西，但它很难看，而且不是 pythonic 式的访问元素的方式。如果你需要索引而不仅仅是值，使用`enumerate()`

```
for index,value in enumerate(my_list):
    print(f'index: {index} , value: {value}')
```

## 从索引中返回值

如果你知道你的元素的索引，你可以简单地要求程序给你那个索引的值。

```
my_list = [1,2,3,'a','b','c']
#index:....0,1,2, 3 , 4 , 5print(f'I love the first number: {my_list[0]}')
print(f'A is my favorite letter: {my_list[3]}')*>>> I love the first number: 1
>>> A is my favorite letter: a*
```

## 元素在里面吗？

您可以通过使用 if 语句询问某个元素来检查它是否在您的列表中:

```
my_list = [1,2,3,'a','b','c']
num = 82
print(f'is the number {num} in my list?')
if num in my_list:
    print(f'{num} sure is in your list!')
else:
    print(f'sory, can\'t find {num} in your list')*>>> sory, can't find 82 in your list*
```

试着把`num`改成`1`，看看能不能在列表里找到。

## 元素在哪里？

如果你想知道 b 在你的列表中的什么位置，你可以用。index()，它将返回可以找到 b 的索引。

```
my_list = [1,2,3,'a','b','c','b','b']
print(my_list.index('b'))*>>> 4*
```

`.index()`返回 b 第一次出现的索引，并在此结束。你可以看到 b 在列表中出现的次数更多了。它们位于索引`4,6`和 `7`处。

有几种方法可以访问 b 的所有索引。如果您熟悉列表理解，您可以这样做:

```
b_occurances = [index for index,element in enumerate(my_list) if element == 'b']
print(b_occurances)*>>> [4, 6, 7]*
```

# 限幅

![](img/137d3fcca36c000b9ec70c01e617f5f6.png)

照片由[卢卡斯](https://www.pexels.com/@goumbik?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从[派克斯](https://www.pexels.com/photo/variety-of-sliced-fruits-1414131/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

与字符串一样，您可以对列表使用切片。语法是:

```
[start:stop:step]
```

如果你有一个字符串:`‘Hello World’`你可以通过切片来访问字符串的一部分。让我们看一个例子。

```
sentence = 'Hello World'print(sentence[:5]) #Hello
print(sentence[6:]) #World
```

在第一个打印行，我们从开始处打印(start 没有设置任何值，所以它从开始处开始)，并在索引 5 处停止。幕后发生的事情是这样的:

```
sentence = 'Hello World'print(sentence[0:5:1]) #Hello
print(sentence[6:11:1]) #World
```

也可以从负指数开始。这意味着你走到字符串的末尾，从那里开始计数。

```
print(sentence[-5:]) #World
```

这也是为什么您可以像这样反转字符串:

```
print(sentence[::-1])
```

它看起来像一个黑客，但是你告诉代码从开始开始，在结尾结束，但是每一步后退一次迭代。

让我们把我们的句子转换成一个列表，然后处理它:

```
sentence_list = list(sentence)
print(sentence_list)*>>> ['H', 'e', 'l', 'l', 'o', ' ', 'W', 'o', 'r', 'l', 'd']*
```

如果我们想抓住地狱的元素，我们可以使用与字符串相同的切片。

```
print(sentence_list[:4]) *>>> ['H', 'e', 'l', 'l']*
```

# 向列表中添加元素

![](img/9b34b089628bd55c6f527e1af6593c10.png)

照片由 [Pexels](https://www.pexels.com/photo/child-playing-wooden-blocks-3662635/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 [cottonbro](https://www.pexels.com/@cottonbro?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 拍摄

## 。追加()

有几种方法可以向列表中添加元素。一种方法是使用`.append()`方法

```
groceries = ['milk' , 'eggs' , 'cheese']
groceries.append('ham')
print(f'\nplease purchase:')
for element in groceries:
    print(f'*{element}')*>>>
please purchase:
*milk
*eggs
*cheese
*ham*
```

当你使用 append 时，你将把你的元素添加到列表的末尾。请注意，如果您试图将一个列表附加到另一个列表，您将简单地将整个列表作为一个新元素添加。如果您想将列表添加到列表中，您需要使用 extend。这段代码将简单地添加几个列表作为元素。

```
receipe = []
dry_ingredients = ['flour' , 'sugar']
wet_ingredients = ['milk' , 'eggs' , 'butter_melted']
receipe.append(dry_ingredients)
receipe.append(wet_ingredients)print(f'receipe: {receipe}')*>>> receipe: [['flour', 'sugar'], ['milk', 'eggs', 'butter_melted']]*
```

## 。扩展()

Extend 是向列表中添加元素的另一种方法。我们不需要将一个列表附加到另一个列表上，而是可以扩展它并得到以下结果:

```
ingredients = ['flour' , 'sugar']
more_ingredients = ['milk' , 'eggs' , 'butter_melted']ingredients.extend(more_ingredients)
print(f'receipe: {ingredients}')*>>> receipe: ['flour', 'sugar', 'milk', 'eggs', 'butter_melted']*
```

你可能会注意到它看起来与`+=`相似，的确如此。`+=`可以解决类似的问题，但是你也要考虑范围。尝试此方法并修复错误:

```
#variations of receipes with milk:
receipe_list = ['milk']def extend_receipe(add_list):
    receipe_list.extend(add_list)def plus_equals_receipe(add_list):
    receipe_list **+=** add_listcake = ['egg' , 'flour', 'butter']
bread = ['oat' , 'flour']extend_receipe(cake)
print(receipe_list)
**plus_equals_receipe(bread)**
print(receipe_list)*>>> UnboundLocalError: local variable ‘receipe_list’ referenced before assignment*
```

## 。插入()

您可以通过指向其索引来更改列表中的现有值，并替换该值，如下所示:

```
my_list = ['a','b','c']
my_list[1] = 'B'print(my_list)*>>> ['a','B','c']*
```

您甚至可以使用切片来编辑范围

```
my_list = ['a','b','c']
my_list[0:3] = ['A','B','C']
print(my_list)*>>> ['A', 'B', 'C']*
```

通过指向列表末尾后的下一个索引来尝试添加一个值可能很有诱惑力，如下所示:

```
my_list = ['a','b','c']
my_list[3] = 'd'print(my_list)
```

这是行不通的，因为你试图先访问一个在列表之外的索引，然后再把值加进去。现在还不存在。如果`d`在列表的末尾，您通常会使用 append 来添加它，但是使用`.insert()`您也可以选择想要添加元素的位置。`.insert()`的语法是:

```
list.insert(index,element)
```

这里我们用它在`a`和`c`之间添加`b`

```
my_list = ['a','c']
my_list.insert(1,'b')print(my_list)*>>> ['a', 'b', 'c']*
```

# 从列表中删除元素

![](img/21924aab7b48c9dbc628d0d9afc40a4d.png)

照片由 [cottonbro](https://www.pexels.com/@cottonbro?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 从 [Pexels](https://www.pexels.com/photo/green-and-white-labeled-box-4107100/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 拍摄

有几种方法可以从列表中删除元素。它们有不同的功能，你需要考虑你的项目需要什么来决定如何移除元素。

## 。移除()

顾名思义，从列表中删除一个元素。

```
my_list = [1,2,3,'a','b','c']
my_list.remove('b')
print(my_list)*>>> [1, 2, 3, 'a', 'c', 'b']*
```

像使用`index()`一样，remove 只删除第一次出现的作为参数传递的元素。在我们的例子中`b`被删除了，但是只有第一个`b`。

如果你想删除所有出现的`b`怎么办？你能像这样循环吗？

```
my_list = ['b',1,'b',2,3,'a','b','c','b','b','b','b']
for element in my_list:
    if element == 'b':
        my_list.remove(element)
print(my_list)*>>> [1, 2, 3, 'a', 'c', 'b', 'b']*
```

不，这样做的原因是删除会使列表更短。如果你把它分解，你就会明白为什么。这段代码将使您更容易:

```
my_list = ['b',1,'a','b','c','b','b','b','b']
print (f'list lenght is: {len(my_list)}')
for index,element in enumerate(my_list):
    print(f'working on index: {index} , value: {element}')
    if element == 'b':
        my_list.remove(element)
        print(f'removed value \"{element}\" from index {index}')
        print(f'new list looks like this: {my_list}')
print(my_list)
```

让我们看看输出:

```
list lenght is: 9
working on index: **0** , value: **b**
**removed value "b" from index 0**
new list looks like this: [1, 'a', 'b', 'c', 'b', 'b', 'b', 'b']
working on index: **1** , value: **a**
working on index: 2 , value: b
removed value "b" from index 2
new list looks like this: [1, 'a', 'c', 'b', 'b', 'b', 'b']
working on index: 3 , value: b
removed value "b" from index 3
new list looks like this: [1, 'a', 'c', 'b', 'b', 'b']
working on index: 4 , value: b
removed value "b" from index 4
new list looks like this: [1, 'a', 'c', 'b', 'b']
[1, 'a', 'c', 'b', 'b']
```

如果我们仔细看看这部分:

```
working on index: **0** , value: **b**
**removed value "b" from index 0**
new list looks like this: [1, 'a', 'b', 'c', 'b', 'b', 'b', 'b']
working on index: **1** , value: **a**
```

我们可以看到，当程序在索引 0 上工作时，它找到了一个 b，它按照你告诉它的去做，并删除了那个元素。新列表现在缺少第一个 b。

下一个要查看的索引是索引 1。记住，我们仍在循环中。我们完成了索引 0，现在我们跳到索引 1。问题是索引 1 现在是`a`，索引 0 是`1`。新旧列表之间没有链接，所以我们实际上缺少了一个元素— `1`。`1`不会发生任何事情

这也是接下来会发生的事情，我们会剩下一个不起作用的程序，在这个程序中，b 会出现几次。

一种解决方案是对这个操作也使用列表理解

```
my_list[:] = [element for element in my_list if element !='b']
print(my_list)
```

注意[:]正在修改列表，而不是创建一个新的变量。如果您这样做:

```
my_list = [element for element in my_list if element !='b']
print(my_list)
```

`my_list`现在有了新的 ID 而不是被修改。

```
my_list = ['b',1,'a','b','c','b','b','b','b']
print(id(my_list))my_list[:] = [element for element in my_list if element !='b']
print(id(my_list))my_list = [element for element in my_list if element !='b']
print(id(my_list))*>>>
140559114976896
140559114976896
14055911497****6768***
```

你可以用`print(id(my_list))`来检查这个

或者，您只需创建一个新列表

```
new_list = [element for element in my_list if element !='b']
print(my_list)
```

`append()`和`extend()`是其他方法的例子，它们在适当的位置工作，并且不会改变列表的 ID。

```
short_list = ['x','y']
print(id(short_list))
short_list.append('z')
print(id(short_list))*>>>
140559114976896
140559114976896*
```

## 。流行()

`.pop()`从列表中删除一个元素。`remove`和`pop`之间有一个小小的区别，那就是`pop`返回你移除的值。您还可以告诉 pop 您想要删除什么索引。默认位于列表的末尾。

```
my_list = ['b',1,'a']
#remove last item:
my_list.pop()
print(my_list)#remove 'b' at index 0
my_list.pop(0)
print(my_list)*>>>
['b', 1]
[1]*
```

由于 pop 返回值，您可以将它存储在变量中，也可以直接使用它。

```
my_list = ['b',1,'a']
new_list = []new_list.append(my_list.pop(1))
print(new_list)*>>> [1]*
```

## 。清除()

如果您想完全清除列表，可以使用。清除()。它完全符合您的预期。

```
clear_list = ['a',2,3]
clear_list.clear()print(clear_list)*>>> []*
```

# 其他有用的方法

当然，除了在列表中添加和删除元素之外，您还可以做更多的事情。让我们看一看。

## 。计数()

与返回列表长度的`len(my_list)`不同，`.count()`可以让你知道一个元素在列表中出现了多少次。

```
my_list = [1,1,1,2,3,3]
print(len(my_list))
print(my_list.count(1))*>>>
6
3*
```

## 。排序()

这些方法的名字非常具有描述性，并且很明显这也是做什么的。如果你使用`.sort()`，它会对列表进行排序。因为它就地工作并修改列表，所以您不能这样做:

```
my_list = [1,8,4,2,3,3]
print(my_list.sort())*>>> None*
```

但是，如果您进行排序，然后再次打印列表，您将打印排序后的列表:

```
my_list = [1,8,4,2,3,3]
my_list.sort()
print(my_list)*>>> [1, 2, 3, 3, 4, 8]*
```

您可能已经知道 sorted()函数。如果你尝试上面的列表，你可以这样做:

```
my_list = [1,8,4,2,3,3]
sorted_list = sorted(my_list)print(my_list)
print(sorted_list)
print(sorted(my_list))*>>>
[1, 8, 4, 2, 3, 3]
[1, 2, 3, 3, 4, 8]
[1, 2, 3, 3, 4, 8]*
```

## 。反向()

我们已经讨论了反转列表的方法。像 sort 一样，reverse 将就地编辑列表。

```
my_list = [1,8,4,2,3,3]
print(my_list)
my_list.reverse()
print(my_list)*>>>
[1, 8, 4, 2, 3, 3]
[3, 3, 2, 4, 8, 1]*
```

你也可以在列表上使用 python 的内置`reversed()`。

```
print('reverse...')
my_list = [1,8,4,2,3,3]reversed_list = reversed(my_list)
print(reversed_list)
print(list(reversed(my_list)))*>>>
<list_reverseiterator object at 0x7f9e601529a0>
[3, 3, 2, 4, 8, 1]*
```

因为`reversed()`返回了一个迭代器，我们用这个迭代器创建了一个列表，打印出了一个漂亮的反向列表。

我们已经用切片反转了列表。

```
my_list = [1,2,3]
print(my_list[::-1])
```

## 。复制()

需要复制一个列表吗？

```
copy_from = [3,3,4,4,4]
copy_to = copy_from.copy()
```

## 拆包列表

您可以通过给元素分配变量来解包列表。假设列表中的前两个元素需要变量，剩下的就不用了。然后你还需要分配最后一个元素。

```
my_list = ['FirstName' , 'LastName' , '.txt' ,'.jpg' ,'.png','.docx', True]
first_name,last_name,*ext,approved = my_list
print(first_name)
print(last_name)
print(ext)
print(approved)*>>>
FirstName
LastName
['.txt', '.jpg', '.png', '.docx']
True*
```

## 。加入()

要将列表元素连接成一个字符串，可以使用。join()方法。它适用于字符串类型的元素。

```
my_list = ['FirstName' , 'LastName' , '.txt' ,'.jpg' ,'.png','.docx']
sentence = ' '.join(my_list)
print(sentence)*>>> FirstName LastName .txt .jpg .png .docx*
```

之前。join()你可以看到我们创建了一个空间。这是分离器。如果您愿意，您可以使用其他分隔符。

```
sentence = '-'.join(my_list)*>>> FirstName-LastName-.txt-.jpg-.png-.docx*
```

## 。zip()

如果你想压缩两个列表的元素，你可以使用 zip 方法。有了两个列表，你可以创建相应索引的元组对。

```
coordinates = ['x','y','z']
coor_values = [10,33,90]zipped_coordinates = zip(coordinates,coor_values)
print (zipped_coordinates)
print(list(zipped_coordinates))*>>>
<zip object at 0x7fd9f0076480>
[('x', 10), ('y', 33), ('z', 90)]*
```

和 reversed 一样，zip 返回一个迭代器。如果我们创建一个迭代器列表，我们得到元组对。如果列表长度不等，它将只考虑你传递的第一个列表。

```
coordinates = ['x','y']
coor_values = [10,33,90]
print(list(zip(coordinates,coor_values)))*>>> [('x', 10), ('y', 33)]*
```

# 最后

Python 列表太棒了！当你选择列表的正确用法时，你必须单独看所有的项目。也许你的项目会受益于字典。如果你想要不可变的对象，也许你正在寻找元组。

列表是超级通用的，所以如果你最终使用它们，你会有一个很大的用例库。

感谢阅读

-M