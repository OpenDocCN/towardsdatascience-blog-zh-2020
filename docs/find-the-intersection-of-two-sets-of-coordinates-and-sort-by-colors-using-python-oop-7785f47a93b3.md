# 使用 Python OOP 找到两组坐标的交集并按颜色排序

> 原文：<https://towardsdatascience.com/find-the-intersection-of-two-sets-of-coordinates-and-sort-by-colors-using-python-oop-7785f47a93b3?source=collection_archive---------65----------------------->

## 解决两个编程问题，一个用排序算法，另一个用 Python 中的 OOP。

![](img/d6ebbea95666a3fb445471bb02e10531.png)

资料来源:马丁·w·柯斯特的《Unsplash》

这篇文章是关于一些编程练习的。如果你是一个初学者，正在学习 Python 中的数据结构和面向对象编程，这可能对你有帮助。我将解决两个问题，并尽可能多的解释。我假设您了解 Python 编程基础和 OOP 基础。我是从 Coursera 的课程《算法》第一部分得到这两个问题的想法的。

## 问题 1

开发一种算法，它采用两个坐标列表并返回它们的交集。我们需要找到两个列表中的共同坐标。

**解决方案**

解决这个问题有 4 个步骤

1.  连接两个列表，并从两个列表中生成一个列表。
2.  首先按 x 坐标，然后按 y 坐标对这个合并列表进行排序。因此，如果有任何共同的项目，他们将并排。
3.  然后返回重复的坐标。

下面是完整的代码。函数“concArray”将连接列表。函数“sortList”将对坐标进行排序。如果两个连续坐标相同，函数“clash”将返回。

```
class Intersection():
    def __init__ (self, sets):
        self.sets = setsdef concArrays(self):
        self.sets = self.sets[0] + self.sets[1]
        return self.setsdef sortList(self):
        self.sets = sorted(self.sets, key=lambda x: x[0])
        return sorted(self.sets, key=lambda x: x[1])

    def clash(self):
        return [self.sets[i] for i in range(0, len(self.sets)-1) if self.sets[i] == self.sets[i+1]]
```

让我们检查算法是否正常工作:

```
sets = [[(2,4),(5,3),(2,6),(6,2),(4,9)],[(4,9),(10,8),(9,3),(5,3),(1,7)]]
inter = Intersection(sets)
inter.concArrays()
inter.sortList()
print(inter.clash())
```

它返回[(4，9)，(5，3)]。如果你注意到我们的集合变量，这是两个公共坐标。由于我们的列表不太大，我们可以通过查看来检查。

## 问题 2

给定 n 个桶的列表，每个桶包含蓝色、白色或红色卵石。按照红、白、蓝的顺序按颜色排序。

**解决方案**

可能有不同的解决方法。我展示了两个解决方案。第一个是使用排序算法。这里我使用了插入排序。任何其他排序算法都将以同样的方式工作。

以下是步骤:

1.  制作一个字典，其中颜色是关键字，值是整数。
2.  在排序算法中，比较两种颜色时使用字典中的值。

以下是完整的代码:

```
def sortColor(a):
    color = {'red': 1, 'white': 2, 'blue': 3}
    for i in range(1, len(a)):
        value = a[i]
        hole = i
        while (hole > 0) and (color[a[hole -1]]>color[value]):
            a[hole] = a[hole -1]
            hole = hole -1
        a[hole] = value
    return a
```

使用以下颜色列表检查该算法:

```
print(sortColor(['red', 'white', 'red', 'blue', 'white', 'blue']))
```

输出是完美的。请尝试一下。

我还想展示一个两行代码的解决方案。如果你知道如何使用 lambda，这是给你的。

```
def sortColor1(a):
    color = {'red': 1, 'white': 2, 'blue': 3}
    return sorted(a, key=lambda x: a[color[x]], reverse=True)
```

我希望它有帮助。

附加阅读:

1.  [在 Python 中使用 Lambda、Map 和 Filter](https://regenerativetoday.com/lambda-map-filter-and-sorted-efficient-programming-with-python/)。