# 35 个问题来测试你的 Python 集知识

> 原文：<https://towardsdatascience.com/35-questions-to-test-your-knowledge-of-python-sets-ad95267c43d9?source=collection_archive---------22----------------------->

## 如何通过掌握集合基础来碾压算法题

![](img/d9182ee32f111895965fe9377b51124b.png)

来自 [Pexels](https://www.pexels.com/photo/woman-in-pink-sweater-using-laptop-3764402/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 [Andrea Piacquadio](https://www.pexels.com/@olly?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的照片

在我追求精通采访算法的过程中，我发现钻研 Python 的基本数据结构很有用。

这是我写的个人问题列表，用来评估我对 Python 集合的了解。

你对 python 集合了解多少？

# 1.将一个对象转换成一个集合会保持对象的顺序吗？

不。集合不是有序的数据结构，所以不保持顺序。

看看当我们将列表`[3,2,1]`转换为集合时会发生什么。就变成了`{1,2,3}`。

```
a = set([3,2,1])
a 
#=> {1, 2, 3}
```

# 2.检查一个集合是否是另一个集合的子集

这可以用`issubset()`方法来完成。

```
a = {4,5}
b = {1,2,3,4,5}a.issubset(b) 
#=> Trueb.issubset(a)
#=> False
```

# 3.使用比较运算符检查集合是否是子集

如果`s1`的所有元素都在`s2`中，那么集合`s1`就是`s2`的子集。

如果第一个集合中的所有元素都存在于第二个集合中，那么操作符`<=`将返回`True`。是子集)。

```
a = {'a','b'}
b = {'a','b','c'}a <= b
#=> True
b <= a 
#=> False
```

# 4.集合是自身的子集吗？

是的。

因为集合包含了自身的所有元素，所以它确实是自身的子集。当我们稍后对比“子集”和“真子集”时，理解这一点很重要。

```
a = {10,20}a.issubset(a) 
#=> True
```

# 5.检查集合中是否存在特定值

像其他类型的 iterables 一样，我们可以用`in`操作符检查一个值是否存在于一个集合中。

```
s = {5,7,9}5 in s
#=> True6 in s
#=> False
```

# 6.检查值是否不在集合中

我们可以再次使用`in`操作符，但这次是以`not`开头。

```
s = {'x','y''z'}'w' not in s
#=> True'x' not in s
#=> False
```

# 7.什么是集合？

集合是唯一对象的无序集合。

当任何对象转换为集合时，重复值将被删除。

```
set([1,1,1,3,5])
#=> {1, 3, 5}
```

以我个人的经验来看，我使用集合是因为它们使得寻找交集、并集和差这样的运算变得容易。

# 8.子集和真子集有什么区别？

真子集是集合的子集，不等于自身。

## **比如:**

`{1,2,3}`是`{1,2,3,4}`的真子集。

`{1,2,3}`不是`{1,2,3}`的真子集，但它是子集。

# 9.检查一个集合是否是真子集

我们可以使用`<`操作符检查一个集合是否是另一个集合的真子集。

```
{1,2,3} < {1,2,3,4}
#=> True{1,2,3} < {1,2,3}
#=> False
```

# 10.向集合中添加元素

与列表不同，我们不能使用`+`操作符向集合中添加元素。

```
{1,2,3} + {4}
#=> TypeError: unsupported operand type(s) for +: 'set' and 'set'
```

使用`add`方法添加元素。

```
s = {'a','b','c'}
s.add('d')
s
#=> {'a', 'b', 'c', 'd'}
```

![](img/b50e45f0b9c809e3e264a2a7ac1e44ec.png)

照片由[杰森·维拉纽瓦](https://www.pexels.com/@jayoke?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从[佩克斯](https://www.pexels.com/photo/close-up-photography-of-cup-of-coffee-851555/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

# 11.复制一套

`copy()`方法对集合进行浅层复制。

```
s1 = {'a','b','c'}s2 = s1.copy()
s2 
#=> {'c', 'b', 'a'}
```

# 12.检查一个集合是否是另一个集合的超集

一个集合`s1`是另一个集合`s2`的超集，如果`s2`中的所有值都能在`s1`中找到。

您可以使用`issuperset()`方法检查一个集合是否是另一个集合的超集。

```
a = {10,20,30}
b = {10,20}a.issuperset(b) #=> True
b.issuperset(a) #=> False
```

# 13.用比较运算符检查一个集合是否是超集

除了`issuperset()`，我们还可以使用`>=`比较操作符来检查一个集合是否是一个超集。

```
a = {10,20,30}
b = {10,20}a >= b #=> True
b >= a #=> False
```

# 14.集合是自身的超集吗？

因为集合`s1`中的所有值都在`s1`中，所以它是自身的超集。尽管它不是一个合适的超集。

```
a = {10,20,30}a.issuperset(a) 
#=> True
```

# 15.检查一个集合是否是另一个集合的适当超集

如果`s2`中的所有值都在`s1`和`s1 != s2`中，则集合`s1`是另一个集合`s2`的真超集。

这可以用`>`操作器进行检查。

```
a = {10,20,30}
b = {10,20}
c = {10,20}a > b #=> True
b > c #=> False
```

# 16.将集合转换为列表

在集合上调用列表构造函数`list()`，将集合转换为列表。但是注意，不保证顺序。

```
a = {4,2,3}
list(a)
#=> [2, 3, 4]
```

# 17.如何迭代集合中的值？

一个集合可以像其他迭代器一样通过循环进行迭代。但是再次注意，顺序不保证。

```
s = {'a','b','c','d','e'}for i in s:
    print(i)

#=> b
#=> c
#=> e
#=> d
#=> a
```

# 18.返回集合的长度

集合中元素的数量可以通过`len()`函数返回。

```
s = {'a','b','c','d','e'}len(s)
#=> 5
```

# 19.创建集合

可以使用带花括号的集合符号创建集合，例如，`{…}`，示例:

```
{1,2,3}
#=> {1, 2, 3}
```

或使用 set 构造函数，示例:

```
# set(1,2,3)
=> TypeError: set expected at most 1 arguments, got 3set([1,2,3])
#=> {1, 2, 3}
```

但是请注意，后者需要传入另一个 iterable 对象，比如 list。

# 20.求两个集合的并集。

使用`union()`方法可以找到两个集合的并集。

```
s1 = {1,2,3,4,5}
s2 = {4,5,6,7,8}s1.union(s2)
#=> {1, 2, 3, 4, 5, 6, 7, 8}
```

也可以用`|`操作符找到。

```
s1 = {1,2,3,4,5}
s2 = {4,5,6,7,8}s1 | s2
#=> {1, 2, 3, 4, 5, 6, 7, 8}
```

![](img/3a791edcb213e7a435e7baf7849f7329.png)

照片由[迪贝拉咖啡](https://www.pexels.com/@di-bella-coffee-509008?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从 [Pexels](https://www.pexels.com/photo/selective-focus-photography-of-a-cup-of-black-coffee-1233528/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 拍摄

# 21.求两个集合的交集

两个集合的交集可以用`intersection()`方法获得。

```
s1 = {1,2,3,4,5}
s2 = {4,5,6,7,8}s1.intersection(s2)
# => {4, 5}
```

也可以用`&`操作器拍摄。

```
s1 = {1,2,3,4,5}
s2 = {4,5,6,7,8}s1 & s2
# => {4, 5}
```

# 22.找出 s1 中不在 s2 中的元素

这可以用`difference()`方法找到。

```
s1 = {1,2,3,4,5}
s2 = {4,5,6,7,8}s1.difference(s2)
{1, 2, 3}
```

也可以用`-`操作符找到它。

```
s1 = {1,2,3,4,5}
s2 = {4,5,6,7,8}s1 - s2
{1, 2, 3}
```

# 23.从集合中移除元素

`remove()`按值从集合中删除元素。

```
s = {'x','y','z'}
s.remove('x')
s
#=> {'y', 'z'}
```

# 24.从集合中移除并返回未指定的元素

从集合中移除并返回一个元素，将集合视为一个无序队列。

```
s = {'z','y','x'}
s.pop()
s
#=> {'y', 'z'}
```

# 25.检查 2 个集合是否不相交

如果集合、`s1`和`s2`没有公共元素，则它们是不相交的。

```
a = {1,2,3}
b = {4,5,6}
c = {3}a.isdisjoint(b) #=> True
a.isdisjoint(c) #=> False
```

# 26.将另一个集合中的所有元素添加到现有集合中

方法从另一个集合中添加元素。

```
a = {1,2,3}
b = {3,4,5}a.update(b)
a
#=> {1, 2, 3, 4, 5}
```

这也可以通过`|=`操作器来完成。

```
a = {1,2,3}
b = {3,4,5}a |= b
a
#=> {1, 2, 3, 4, 5}
```

注意这与`union`不同。`union()`返回新集合，而不是更新现有集合。

# 27.从集合中移除所有元素

`clear()`从集合中删除所有元素。然后，该集合可用于将来的操作并存储其他值。

```
a = {1,2,3}
a.clear()
a
#=> set()
```

# 28.从集合中移除元素(如果存在)

`discard()`如果某个元素存在，则删除该元素，否则不执行任何操作。

```
a = {1,2,3}
a.discard(1)
a
#=> {2, 3}a = {1,2,3}
a.discard(5)
a
#=> {1, 2, 3}
```

与此形成对比的是`remove()`，如果您试图删除一个不存在的元素，它会抛出一个错误。

```
a = {1,2,3}
a.remove(5)
a
#=> KeyError: 5
```

# 29.将字典传递给 set 构造函数的结果是什么？

只有字典的键会存在于返回的集合中。

```
d = {'dog': 1, 'cat':2, 'fish':3}
set(d)
#=> {'cat', 'dog', 'fish'}
```

# 30.你能把两套拉链拉上吗？

是的。但是每个集合中的值可能不会按顺序连接。

请注意整数集中的第一个值是如何与字母集中的第三个值组合在一起的，`(1, 'c')`。

```
z = zip(
  {1,2,3},
  {'a','b','c'}
)
list(z)
#=> [(1, 'c'), (2, 'b'), (3, 'a')]
```

![](img/6cc970d8c0b650c8a880d883ded4ddc8.png)

照片由[梅莉·迪·罗科](https://www.pexels.com/@meli-di-rocco-303801?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从[派克斯](https://www.pexels.com/photo/selective-focus-photo-of-three-macaroons-940872/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

# 31.集合可以通过索引访问吗？

不可以。试图通过索引访问集合将会引发错误。

```
s = {1,2,3}
s[0]
#=> TypeError: 'set' object is not subscriptable
```

# 32.集合和元组有什么区别？

元组是不可变的。集合是可变的。

元组中的值可以通过索引来访问。集合中的值只能通过值来访问。

元组有顺序。集合没有顺序。

集合实现了集合论，所以它们有很多有趣的功能，如并、交、差等。

# 33.set 和 frozenset 有什么区别？

冷冻集的行为就像集合一样，只是它们是不可变的。

```
s = set([1,2,3])
fs = frozenset([1,2,3])s #=> {1, 2, 3}
fs #=> frozenset({1, 2, 3})s.add(4)
s #=> {1, 2, 3, 4}fs.add(4)
fs #=> AttributeError: 'frozenset' object has no attribute 'add'
```

# 34.更新一个集合使其等于另一个集合的交集

`intersection_update()`将第一个集合更新为等于交集。

```
s1 = {1,2,3,4,5}
s2 = {4,5,6,7,8}s1.intersection_update(s2)
s1
#=> {4, 5}
```

这也可以用`&=`操作符来完成。

```
s1 = {1,2,3,4,5}
s2 = {4,5,6,7,8}s1 &= s2
s1
#=> {4, 5}
```

# 35.从第一个集合中移除第二个集合的交集

`difference_update()`从第一个集合中删除交集。

```
s1 = {1,2,3,4,5}
s2 = {4,5,6,7,8}s1.difference_update(s2)
s1
#=> {1, 2, 3}
```

操作员`-=`也工作。

```
s1 = {1,2,3,4,5}
s2 = {4,5,6,7,8}s1 -= s2
s1
#=> {1, 2, 3}
```

# 结论

你做得怎么样？

老实说，我在这里做得比我的[字符串问题](/41-questions-to-test-your-knowledge-of-python-strings-9eb473aa8fe8)和[列表问题](/60-questions-to-test-your-knowledge-of-python-lists-cca0ebfa0582)差。

我们在日常编程中不经常使用集合，我们大多数人在编写 SQL 连接时只接触过集合论。也就是说，熟悉基础知识并没有坏处。

如果你觉得这很有趣，你可能也会喜欢我的 [python 面试问题](/53-python-interview-questions-and-answers-91fa311eec3f)。

向前向上。