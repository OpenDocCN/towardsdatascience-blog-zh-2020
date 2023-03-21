# 测试您对 Python 列表的了解的 60 个问题

> 原文：<https://towardsdatascience.com/60-questions-to-test-your-knowledge-of-python-lists-cca0ebfa0582?source=collection_archive---------0----------------------->

## 通过掌握列表基础知识粉碎算法问题

![](img/58264dcfbaec1e6df95b9075532eace2.png)

照片由[安德鲁·尼尔](https://www.pexels.com/@andrew?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从 [Pexels](https://www.pexels.com/photo/woman-in-gray-sweater-sitting-on-couch-using-macbook-4134786/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 拍摄

我最近做了很多算法问题，发现我并没有像应该的那样理解列表

这是我为评估自己的知识而写的 60 个问题的汇编。

我希望你会发现它对我和写作一样有用。

# 问题 1–10:

## 1.检查列表是否包含元素

如果列表中有特定的元素，那么`in`操作符将返回 True。

```
li = [1,2,3,'a','b','c']'a' in li 
#=> True
```

## 2.如何同时遍历 2+个列表

您可以使用`zip()`列表，然后遍历`zip`对象。`zip`对象是元组的迭代器。

下面我们同时迭代 3 个列表，并将值插入到一个字符串中。

```
name = ['Snowball', 'Chewy', 'Bubbles', 'Gruff']
animal = ['Cat', 'Dog', 'Fish', 'Goat']
age = [1, 2, 2, 6]z = zip(name, animal, age)
z #=> <zip at 0x111081e48>for name,animal,age in z:
    print("%s the %s is %s" % (name, animal, age))

#=> Snowball the Cat is 1
#=> Chewy the Dog is 2
#=> Bubbles the Fish is 2
#=> Gruff the Goat is 6
```

## 3.什么时候你会使用列表而不是字典？

列表和字典通常有稍微不同的用例，但是有一些重叠。

我得出的算法问题的一般规则是，如果你可以两者都用，那就用字典，因为查找会更快。

## **列表**

如果你需要存储一些东西的顺序，使用一个**列表**。

即:数据库记录的 id，按照它们将被显示的顺序排列。

```
ids = [23,1,7,9]
```

从 python 3.7 开始，列表和字典都是有序的，列表允许重复值，而字典不允许重复键。

## **字典**

如果你想计算某事的发生次数，就用字典。比如家里宠物的数量。

```
pets = {'dogs':2,'cats':1,'fish':5}
```

每个键在字典中只能存在一次。注意，键也可以是其他不可变的数据结构，如元组。即:`{('a',1):1, ('b',2):1}`。

## 4.列表是可变的吗？

是的。请注意，在下面的代码中，与内存中相同标识符关联的值没有发生变化。

```
x = [1]
print(id(x),':',x) #=> 4501046920 : [1]x.append(5)
x.extend([6,7])
print(id(x),':',x) #=> 4501046920 : [1, 5, 6, 7]
```

## 5.一个列表需要同质吗？

不可以。不同类型的对象可以混合在一个列表中。

```
a = [1,'a',1.0,[]]
a #=> [1, 'a', 1.0, []]
```

## 6.追加和扩展有什么区别？

`.append()`在列表末尾添加一个对象。

```
a = [1,2,3]
a.append(4)
a #=> [1, 2, 3, 4]
```

这也意味着追加列表会将整个列表作为单个元素添加，而不是追加它的每个值。

```
a.append([5,6])
a #=> [1, 2, 3, 4, [5, 6]]
```

`.extend()`将第二个列表中的每个值作为自己的元素相加。所以用另一个列表扩展一个列表会合并它们的值。

```
b = [1,2,3]
b.extend([5,6])
b #=> [1, 2, 3, 5, 6]
```

## 7.python 列表是存储值还是指针？

Python 列表本身并不存储值。它们存储指向存储在内存中其他地方的值的指针。这使得列表是可变的。

这里我们初始化值`1`和`2`，然后创建一个包含值`1`和`2`的列表。

```
print( id(1) ) #=> 4438537632
print( id(2) ) #=> 4438537664a = [1,2,3]
print( id(a) ) #=> 4579953480print( id(a[0]) ) #=> 4438537632
print( id(a[1]) ) #=> 4438537664
```

注意链表是如何拥有自己的内存地址的。但是列表中的`1`和`2`与我们之前定义的`1`和`2`指向内存中相同的位置。

## 8.“del”是做什么的？

`del`从给定索引的列表中删除一个项目。

这里我们将删除索引 1 处的值。

```
a = ['w', 'x', 'y', 'z']
a #=> ['w', 'x', 'y', 'z']del a[1]a #=> ['w', 'y', 'z']
```

注意`del`没有返回被移除的元素。

## 9.“移除”和“弹出”有什么区别？

`.remove()`删除匹配对象的第一个实例。下面我们去掉第一个`b`。

```
a = ['a', 'a', 'b', 'b', 'c', 'c']
a.remove('b')
a #=> ['a', 'a', 'b', 'c', 'c']
```

`.pop()`根据索引删除对象。

`pop`和`del`的区别在于`pop`返回弹出的元素。这允许使用类似堆栈的列表。

```
a = ['a', 'a', 'b', 'b', 'c', 'c']
a.pop(4) #=> 'c'
a #=> ['a', 'a', 'b', 'b', 'c']
```

默认情况下，如果没有指定索引，`pop`会删除列表中的最后一个元素。

## 10.从列表中删除重复项

如果你不关心维护一个列表的顺序，那么转换成一个集合再转换回一个列表就可以实现这个目的。

```
li = [3, 2, 2, 1, 1, 1]
list(set(li)) #=> [1, 2, 3]
```

![](img/1d30c13f33d6ade258f1b287c382b4da.png)

照片由[切瓦农摄影](https://www.pexels.com/@chevanon?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从 [Pexels](https://www.pexels.com/photo/art-blur-cappuccino-close-up-302899/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 拍摄

# 问题 11–20:

## 11.查找第一个匹配元素的索引

例如，您想在水果列表中找到第一个“苹果”。使用`.index()`方法。

```
fruit = ['pear', 'orange', 'apple', 'grapefruit', 'apple', 'pear']
fruit.index('apple') #=> 2
fruit.index('pear') #=> 0
```

## 12.从列表中删除所有元素

我们可以用`.clear()`从现有列表中清除元素，而不是创建一个新的空列表。

```
fruit = ['pear', 'orange', 'apple']print( fruit )     #=> ['pear', 'orange', 'apple']
print( id(fruit) ) #=> 4581174216fruit.clear()print( fruit )     #=> []
print( id(fruit) ) #=> 4581174216
```

或者用`del`。

```
fruit = ['pear', 'orange', 'apple']print( fruit )     #=> ['pear', 'orange', 'apple']
print( id(fruit) ) #=> 4581166792del fruit[:]print( fruit )     #=> []
print( id(fruit) ) #=> 4581166792
```

## 13.遍历列表中的值及其索引

将计数器添加到作为参数传递的列表中。

下面我们遍历列表，将值和索引传递给字符串插值。

```
grocery_list = ['flour','cheese','carrots']for idx,val in enumerate(grocery_list):
    print("%s: %s" % (idx, val))

#=> 0: flour
#=> 1: cheese
#=> 2: carrots
```

## 14.如何连接两个列表

`+`操作符将连接两个列表。

```
one = ['a', 'b', 'c']
two = [1, 2, 3]one + two #=> ['a', 'b', 'c', 1, 2, 3]
```

## 15.如何用列表理解操作列表中的每个元素

下面我们返回一个新的列表，每个元素加 1。

```
li = [0,25,50,100][i+1 for i in li] 
#=> [1, 26, 51, 101]
```

## 16.统计列表中特定对象的出现次数

`count()`方法返回一个特定对象出现的次数。下面我们返回字符串的次数，`“fish”`存在于一个名为`pets`的列表中。

```
pets = ['dog','cat','fish','fish','cat']
pets.count('fish')
#=> 2
```

## 17.如何浅层复制一个列表？

`.copy()`可以用来浅层复制一个列表。

下面我们创建一个`round1`的浅层副本，给它分配一个新名字`round2`，然后移除字符串`sonny chiba`。

```
round1 = ['chuck norris', 'bruce lee', 'sonny chiba']**round2 = round1.copy()**
round2.remove('sonny chiba')print(round1) #=> ['chuck norris', 'bruce lee', 'sonny chiba']
print(round2) #=> ['chuck norris', 'bruce lee']
```

## 18.为什么要创建一个列表的浅层副本？

基于前面的例子，如果我们不创建浅层拷贝，修改`round2`将会修改`round1`。

```
round1 = ['chuck norris', 'bruce lee', 'sonny chiba']**round2 = round1**
round2.remove('sonny chiba')print(round1) #=> ['chuck norris', 'bruce lee']
print(round2) #=> ['chuck norris', 'bruce lee']
```

没有浅拷贝，`round1`和`round2`只是内存中指向同一个链表的名字。这就是为什么看起来改变一个的值会改变另一个的值。

## 19.如何深度复制一个列表？

为此我们需要导入`copy`模块，然后调用`copy.deepcopy()`。

下面我们创建一个名为`round2`的列表的深层副本`round1`，更新`round2`中的值，然后打印两者。在这种情况下，`round1`不受影响。

```
round1 = [
    ['Arnold', 'Sylvester', 'Jean Claude'],
    ['Buttercup', 'Bubbles', 'Blossom']
]import copy
**round2 = copy.deepcopy(round1)**round2[0][0] = 'Jet Lee'print(round1)
#=> [['Arnold', 'Sylvester', 'Jean Claude'], ['Buttercup', 'Bubbles', 'Blossom']]print(round2)
#=> [['Jet Lee', 'Sylvester', 'Jean Claude'], ['Buttercup', 'Bubbles', 'Blossom']]
```

上面我们可以看到，改变`round2`中的嵌套数组并没有更新`round1`。

## 20.深抄和浅抄有什么区别？

在前一个例子的基础上，创建一个浅层副本，然后修改它，会影响原始列表..

```
round1 = [
    ['Arnold', 'Sylvester', 'Jean Claude'],
    ['Buttercup', 'Bubbles', 'Blossom']
]import copy
**round2 = round1.copy()**round2[0][0] = 'Jet Lee'print(round1)
#=> [['Jet Lee', 'Sylvester', 'Jean Claude'], ['Buttercup', 'Bubbles', 'Blossom']]print(round2)
#=> [['Jet Lee', 'Sylvester', 'Jean Claude'], ['Buttercup', 'Bubbles', 'Blossom']]
```

为什么会这样？

创建一个浅层副本确实会在内存中创建一个新的对象，但是它会填充上一个列表中已有对象的引用。

创建深层副本会创建原始对象的副本，并指向这些新版本。因此新列表完全不受旧列表更改的影响，反之亦然。

![](img/96d08f73ae4399100882ff9453d443d0.png)

照片由[瓦莱里娅·博尔特涅娃](https://www.pexels.com/@valeriya?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从[派克斯](https://www.pexels.com/photo/croissant-with-butter-1510684/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

# 问题 21-30:

## 21.列表和元组的区别是什么？

元组在创建后无法更新。添加/移除/更新现有元组需要创建新元组。

列表创建后可以修改。

元组通常表示一个对象，如从数据库中加载的记录，其中元素具有不同的数据类型。

列表通常用于存储特定类型对象的有序序列(但不总是如此)。

两者都是序列，并且允许重复值。

## 22.返回列表的长度

`len()`可以返回一个列表的长度。

```
li = ['a', 'b', 'c', 'd', 'e']
len(li)
#=> 5
```

但是请注意，它对顶级对象进行计数，因此几个整数的嵌套列表将只被计为单个对象。下面，`li`的长度是 2，不是 5。

```
li = [[1,2],[3,4,5]]
len(li)
#=> 2
```

## 23.列表和集合的区别是什么？

列表是有序的，而集合不是。这就是为什么使用 set 在一个列表中寻找唯一值，像`list( set([3, 3, 2, 1]) )`失去了顺序。

列表通常用于跟踪顺序，而集合通常用于跟踪存在。

列表允许重复，但是根据定义，集合中的所有值都是唯一的。

## 24.如何检查一个元素是否不在列表中？

为此，我们使用了`in`操作符，但是在它前面加上了`not`。

```
li = [1,2,3,4]
5 not in li #=> True
4 not in li #=> False
```

## 25.用 map 函数将列表中的每个元素乘以 5

`.map()`允许迭代一个序列，并用另一个函数更新每个值。

返回一个地图对象，但是我已经用一个列表理解包装了它，所以我们可以看到更新的值。

```
def multiply_5(val):
    return val * 5a = [10,20,30,40,50][val for val in map(multiply_5, a)] 
#=> [50, 100, 150, 200, 250]
```

## 26.用 zip 函数将两个列表合并成一个元组列表

`zip()`将多个序列组合成一个元组迭代器，其中相同序列索引处的值组合在同一个元组中。

```
alphabet = ['a', 'b', 'c']
integers = [1, 2, 3]list(zip(alphabet, integers))
```

## 27.在现有列表中的特定索引处插入值

`insert()`方法接受一个要插入的对象和索引。

```
li = ['a','b','c','d','e']
li.insert(2, 'HERE')li #=> ['a', 'b', 'HERE', 'c', 'd', 'e']
```

请注意，先前位于指定索引处的元素被向右移动，而不是被覆盖。

## 28.用 reduce 函数从第一个元素中减去列表中的值

`reduce()`需要从`functools`导入。

给定一个函数，`reduce`遍历一个序列，并对每个元素调用该函数。当前一个元素调用函数时，前一个元素的输出作为参数传递。

```
from functools import reducedef subtract(a,b):
    return a - bnumbers = [100,10,5,1,2,7,5]reduce(subtract, numbers) #=> 70
```

上面我们从 100 减去了 10，5，1，2，7 和 5。

## 29.用 filter 函数从列表中删除负值

给定一个函数，`filter()`将从序列中删除该函数不返回`True`的任何元素。

下面我们去掉小于零的元素。

```
def remove_negatives(x):
    return True if x >= 0 else False

a = [-10, 27, 1000, -1, 0, -30][x for x in filter(remove_negatives, a)] 
#=> [27, 1000, 0]
```

## 30.将列表转换成字典，其中列表元素是键

为此我们可以用字典理解。

```
li = ['The', 'quick', 'brown', 'fox', 'was', 'quick']
d = {k:1 for k in li}
d #=> {'The': 1, 'quick': 1, 'brown': 1, 'fox': 1, 'was': 1}
```

![](img/2639b593844099566b683cfe0c5bc9b8.png)

凯文·梅纳江摄于 [Pexels](https://www.pexels.com/photo/shallow-focus-photography-of-cafe-late-982612/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

# 问题 31-40:

## 31.用 lambda 函数修改现有列表

让我们将之前编写的`map`函数转换成带有`lambda`的一行程序。

```
a = [10,20,30,40,50]list(map(lambda val:val*5, a))
#=> [50, 100, 150, 200, 250]
```

*我可以把它作为一个 map 对象，直到我需要迭代它，但是我把它转换成一个列表来显示里面的元素。*

## 32.移除列表中特定索引后的元素

使用 slice 语法，我们可以返回一个新的列表，其中只包含特定索引之前的元素。

```
li = [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,10]li[:10]
#=> [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

## 33.移除列表中特定索引前的元素

slice 语法还可以返回一个新列表，其中包含指定索引之后的值。

```
li = [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,10]li[15:]
#=> [16, 17, 18, 19, 10]
```

## 34.移除列表中两个索引之间的元素

或者在两个指数之间。

```
li = [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,10]li[12:17]
#=> [13, 14, 15, 16, 17]
```

## 35.返回列表中两个索引之间的第二个元素

或者在特定间隔的索引之前/之后/之间。

这里，我们使用 slice 语法返回索引 10 和 16 之间的每第二个值。

```
li = [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,10]li[10:16:2]
#=> [11, 13, 15]
```

## 36.按升序对整数列表进行排序

`sort()`方法将一个列表改变为升序。

```
li = [10,1,9,2,8,3,7,4,6,5]li.sort()
li #=> [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

## 37.按降序对整数列表进行排序

通过添加参数`reverse=True`，也可以用`sort()`进行降序排序。

```
li = [10,1,9,2,8,3,7,4,6,5]li.sort(reverse=True)
li #=> [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
```

## 38.用列表理解从列表中过滤出偶数值

您可以在列表理解中添加条件逻辑，以按照给定的模式过滤出值。

这里我们过滤掉能被 2 整除的值。

```
li = [1,2,3,4,5,6,7,8,9,10][i for i in li if i % 2 != 0]
#=> [1, 3, 5, 7, 9]
```

## 39.统计列表中每个值的出现次数

一种选择是遍历一个列表并将计数添加到一个字典中。但是最简单的选择是从`collections`导入`Counter`类，并将列表传递给它。

```
from collections import Counterli = ['blue', 'pink', 'green', 'green', 'yellow', 'pink', 'orange']Counter(li)
#=> Counter({'blue': 1, 'pink': 2, 'green': 2, 'yellow': 1, 'orange': 1})
```

## 40.获取列表中每个嵌套列表的第一个元素

列表理解非常适合迭代其他对象的列表，并从每个嵌套对象中获取一个元素。

```
li = [[1,2,3],[4,5,6],[7,8,9],[10,11,12],[13,14,15]][i[0] for i in li]
#=> [1, 4, 7, 10, 13]
```

![](img/6e3a8abc67714bd3115b28e1c4196d69.png)

来自 [Pexels](https://www.pexels.com/photo/macro-photography-of-pile-of-3-cookie-230325/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 [Lisa Fotios](https://www.pexels.com/@fotios-photos?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的照片

# 问题 41–50:

## 41.对于一个列表，插入、查找、删除的时间复杂度是多少？

**插入的是 O(n)** 。如果在开头插入一个元素，所有其他元素都必须右移。

**通过索引查找是 O(1)** 。但是 find by value 是 O(n ),因为需要迭代元素直到找到值。

**删除是 O(n)** 。如果一开始就删除了一个元素，那么所有其他元素都必须左移。

## 42.将列表中的元素组合成一个字符串。

这可以通过`join()`功能完成。

```
li = ['The','quick','brown', 'fox', 'jumped', 'over', 'the', 'lazy', 'dog']
' '.join(li)
#=> 'The quick brown fox jumped over the lazy dog'
```

## 43.一个列表乘以一个整数会有什么影响？

将一个列表乘以一个整数称为多重串联，其效果与将一个列表串联 n 次相同。

下面我们将列表乘以 5。

```
['a','b'] * 5
#=> ['a', 'b', 'a', 'b', 'a', 'b', 'a', 'b', 'a', 'b']
```

这与相同。

```
['a','b'] + ['a','b'] + ['a','b'] + ['a','b'] + ['a','b']
#=> ['a', 'b', 'a', 'b', 'a', 'b', 'a', 'b', 'a', 'b']
```

## 44.如果列表中的任何值可以被 2 整除，则使用“any”函数返回 True

如果返回的列表中有任何值的计算结果为`True`，我们可以将`any()`与列表理解结合起来返回`True`。

在第一个列表下面，理解返回`True`，因为列表中有一个`2`，它可以被`2`整除。

```
li1 = [1,2,3]
li2 = [1,3]any(i % 2 == 0 for i in li1) #=> True
any(i % 2 == 0 for i in li2) #=> False
```

## 45.如果列表中的所有值都是负数，则使用“all”函数返回 True

与`any()`函数类似，`all()`也可以与列表理解一起使用，仅当返回列表中的所有值都是`True`时才返回`True`。

```
li1 = [2,3,4]
li2 = [2,4]all(i % 2 == 0 for i in li1) #=> False
all(i % 2 == 0 for i in li2) #=> True
```

## 46.你能对一个包含“无”的列表进行排序吗？

您不能对包含`None`的列表进行排序，因为比较运算符(由`sort()`使用)不能将整数与`None`进行比较。

```
li = [10,1,9,2,8,3,7,4,6,None]li.sort()
li #=> TypeError: '<' not supported between instances of 'NoneType' and 'int'
```

## 47.列表构造函数将从现有列表中创建什么样的副本？

list 构造函数创建一个传入列表的浅表副本。也就是说，这比使用`.copy()`要简单得多。

```
li1 = ['a','b']
li2 = list(li1)
li2.append('c')print(li1) #=> ['a', 'b']
print(li2) #=> ['a', 'b', 'c']
```

## 48.颠倒列表的顺序

可以用`reverse()`方法将一个列表改变成相反的顺序。

```
li = [1,2,3,4,5,6,7,8,9,10]
li.reverse()
li #=> [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
```

请注意，这将改变对象，而不是返回一个新的对象。

## 49.反和反的区别是什么？

`reverse()`原地反转列表。`reversed()`以逆序返回列表的 iterable。

```
li = [1,2,3,4,5,6,7,8,9,10]
list(reversed(li))
#=> [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
```

## 50.sort 和 sorted 有什么区别？

`sort()`就地修改列表。`sorted()`返回一个逆序的新列表。

```
li = [10,1,9,2,8,3,7,4,6,5]
li.sort()
li #=> [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]li = [10,1,9,2,8,3,7,4,6,5]
sorted(li) #=> [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

![](img/1ec595d5e89558a1c8fad1f9941f7411.png)

来自[像素](https://www.pexels.com/photo/coffee-latte-1477486/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的 [TL 人像](https://www.pexels.com/@tl-portrait-614151?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)照片

# 问题 51–60:

## 51.返回列表中的最小值

`min()`函数返回列表中的最小值。

```
li = [10,1,9,2,8,3,7,4,6,5]
min(li)
#=> 1
```

## 52.返回列表中的最大值

`max()`函数返回列表中的最大值。

```
li = [10,1,9,2,8,3,7,4,6,5]
max(li)
#=> 10
```

## 53.返回列表中数值的总和

`sum()`函数返回列表中所有值的总和。

```
li = [10,1,9,2,8,3,7,4,6,5]
sum(li)
#=> 55
```

## 54.将列表用作堆栈

你可以使用`append()`和`pop()`把一个列表当作一个堆栈。堆栈按照后进先出法运行。

```
stack = []stack.append('Jess')
stack.append('Todd')
stack.append('Yuan')print(stack) #=> ['Jess', 'Todd', 'Yuan']print(stack.pop()) #=> Yuanprint(stack) #=> ['Jess', 'Todd']
```

堆栈的一个好处是可以在 O(1)时间内添加和删除元素，因为列表不需要迭代。

## 55.找到两个列表的交集

我们可以通过使用带“与”符号的`set()`来做到这一点。

```
li1 = [1,2,3]
li2 = [2,3,4]set(li1) & set(li2)
#=> {2, 3}
```

## 56.找出一个集合和另一个集合的区别

我们不能减去列表，但我们可以减去集合。

```
li1 = [1,2,3]
li2 = [2,3,4]set(li1) - set(li2)
#=> {1}set(li2) - set(li1)
#=> {4}
```

## 57.用列表理解展平列表列表

与 Ruby 不同，Python3 没有显式的 flatten 函数。但是我们可以使用列表理解来简化列表列表。

```
li = [[1,2,3],[4,5,6]][i for x in li for i in x]
#=> [1, 2, 3, 4, 5, 6]
```

## 58.生成两个值之间所有整数的列表

我们可以创建一个介于两个值之间的范围，然后将其转换为一个列表。

```
list(range(5,10))
#=> [5, 6, 7, 8, 9]
```

## 59.将两个列表合并成一个字典

使用`zip()`和`list()`构造函数，我们可以将两个列表合并成一个字典，其中一个列表成为键，另一个列表成为值。

```
name = ['Snowball', 'Chewy', 'Bubbles', 'Gruff']
animal = ['Cat', 'Dog', 'Fish', 'Goat']dict(zip(name, animal))
#=> {'Snowball': 'Cat', 'Chewy': 'Dog', 'Bubbles': 'Fish', 'Gruff': 'Goat'}
```

## 60.使用 slice 语法反转列表的顺序

虽然我们可以用`reverse()`和`reversed()`反转一个列表，但也可以用 slice 语法来完成。

这将通过从头到尾遍历列表返回一个新列表。

```
li = ['a','b',3,4]
li[::-1]
#=> [4, 3, 'b', 'a']
```

# 结论

做完这些后，我觉得更有准备去解决算法问题，而不用去猜测具体的列表方法。

去除持续的谷歌搜索可以为更高层次的思考释放能量，比如让一个功能做它应该做的事情，以及降低功能的复杂性。

同样，我希望您发现这也很有用。

你可能对我之前发表的关于[字符串问题](/41-questions-to-test-your-knowledge-of-python-strings-9eb473aa8fe8)的文章感兴趣，或者对我已经开始发表的关于算法问题的[每周系列文章感兴趣。](/3-algorithm-interview-questions-for-data-science-and-software-engineering-in-python-29fc86a07a6f)