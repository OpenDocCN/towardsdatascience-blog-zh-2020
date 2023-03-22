# Python 2020 的新特性—第 2 部分

> 原文：<https://towardsdatascience.com/whats-new-in-python-2020-part-2-1d9abb0f0e7c?source=collection_archive---------39----------------------->

## 更多隐藏在 Python 3.9 袖子里的技巧。

![](img/1a01dfedb645c1d18ecf3d522e50933c.png)

照片由 [Fotis Fotopoulos](https://unsplash.com/@ffstop?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

我写给 Python 3 新特性的第一封情书有点长。这里是第一篇文章，介绍了编写 Python 代码时添加类型，以及用`:=`赋值表达式，也就是“walrus 操作符”。

本文将简要介绍即将发布的 Python 3.9 中的一个新特性。然后我们将深入研究数据类(`dataclasses`)。看看它们是什么，想知道它们在我们的编程生涯中去了哪里。

# 字典联合运算符— 3.9

这将是一个快速的。

从 Python 3.8 及以下版本开始，当我们想要组合两个或更多字典时，我们习惯于使用语法体操来完成工作:

```
d1: dict = dict(apple=2, banana=3, cactus=3)
d2: dict = dict(cactus="overwrite!", durian=4, elderberry=5)
​
# method 1
d1.update(d2)
​
# method 2
d1 = dict(d1, **d2)
​
# method 3(this is what I use)
d1 = {**d1, **d2}
​
# method 4 (for psychopaths)
for k, v in d2.items():
     d1[k] = v
```

我喜欢目前用双星号`**`打开字典的方法。这很像 JavaScript 中的`...` spread 运算符。

尽管如此，新的语法是一种改进。

```
# Python 3.9+ union operator
d1 = d1 | d2
​
# or ...
d1 |= d2
```

所有这些语句返回:

```
{'apple': 2, 'banana': 3, 'cactus': 'overwrite!', 'durian': 4, 'elderberry': 5}
```

很酷，对吧？

“新的”union 操作符实际上借鉴了在`set`集合类型中使用的现有 Python 语法。如果您没有使用集合来比较集合，请现在开始。他们太棒了。

# 数据类— 3.7 以上

`dataclasses`内置库已经有一个时代了(从 3.7 开始)。然而，我没有看到它得到太多的爱。应该的。创建数据类对于编写有效的 Python 代码是不必要的，Python 是一种动态*类型化的语言(类型与*血型*相同，而不是以每分钟 75 个单词*的速度*)。这意味着 Python 解释器在运行时运行类型检查，而不是在编译时，所以代码中不需要类型检查(这是推断出来的)。*

Python 中的 Dataclasses 很像 TypeScript 与 JavaScript 的关系。JavaScript 程序在 Node.js 或浏览器中运行并不需要输入。然而，有很好的理由在代码中使用类型。除非有回报，否则开发人员不会费尽周折去学习另一种工具。

回报就是理解通过你的程序的数据。验证输入。在声明时看到变量的类型，而不是在堆栈跟踪中找到。

例如变量`people`。那是...人数？一份名单？什么清单？一本字典？我们可以做得更好。

```
from dataclasses import dataclass, field
from typing import List
​
@dataclass
class Person:
    first: str
    last: str
    full_name: str = field(init=False)
    age: int

    def __post_init__(self):
        self.full_name = f"{self.first} {self.last}"
        if self.last == '' or not self.last:
            self.full_name = self.first
​
@dataclass 
class People:
    people: List[Person] = field(default_factory=list)

OO7 = Person('James', 'Bond', 40)
tech_guy = Person('Q', None, 80)
​
M16: People = People([OO7, tech_guy])

print(OO7)
print(tech_guy)
print(M16)
```

这将返回:

```
Person(first='James', last='Bond', full_name='James Bond', age=40)
Person(first='Q', last=None, full_name='Q', age=80)
People(people=[
    Person(first='James', last='Bond', full_name='James Bond', age=40),     
    Person(first='Q', last=None, full_name='Q', age=80)])
```

清晰多了！

当您开始输入 Python 代码时，内置的`dataclasses`库可以带您走得更远。尽管如此，你最终还是会漫游到美好的`pydantic` [图书馆](https://pydantic-docs.helpmanual.io/)的绿色牧场。Pydantic 本身对构建可伸缩、可维护的 Python 代码有很大帮助。库 [FastAPI](https://fastapi.tiangolo.com/) 可以做好一切，开箱即用。这两个图书馆都应该有自己的文章或课程，所以我将把它留在这里。

# 结论

Python 3.9 充满了有用的语言特性。全新的版本，并结合了所有在以前版本中添加的内容。如果您还没有阅读 Python 2020 年的新特性第一部分[的话，请阅读该部分。](https://medium.com/@nball/whats-new-in-python-2020-part-1-c101939c8800)

请将您的安装升级到 3.9！你没什么可失去的！