# Python 2020 的新特性—第 1 部分

> 原文：<https://towardsdatascience.com/whats-new-in-python-2020-part-1-c101939c8800?source=collection_archive---------18----------------------->

## Python 过去(3.7)、现在(3.8)、未来(3.9)的幽灵正在 2020 年拜访你。

![](img/945cbac90edee3cddb7e51f19c0a4057.png)

克里斯里德在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

自从很久以前切换到 Python 3(*咳咳* —我*希望*那是很久以前的事了！)，语言层面的特性变化相对较小。然而，在每个版本中，从事 Python 工作的天才们都在不断添加我不能缺少的东西。

随着 Python 3.8 于 2019 年 10 月发布，我发现自己在使用该语言的一些功能，当我第一次读到它们时，让我说“随便吧”。

*   3.5-类型注释
*   3.6 — asyncio
*   3.7 —数据类
*   3.8 —赋值表达式又名海象运算符

在 3.9 中，字典联合操作符和泛型类型提示。尽量减少感叹号，但这是令人兴奋的事情！

以上所述，我一直在专业地使用代码库，并在我的项目中寻找乐趣。

> 快速演讲:如果您还在使用旧版本的 Python 工作或项目，不要害怕升级！您的旧代码仍然可以工作，而且您将获得 Python 新特性的好处！
> 
> 声明:如果你还在使用 Python 2.7，这是不正确的。但在这种情况下，你不是那种会接受好建议的人。😎

下面我将(快速)回顾一些我最喜欢的特性，希望你会发现它们每天都在你的编码中使用。

它们是:类型注释、数据类、字典联合操作符、walrus 操作符。

在这一部分:输入注释，walrus 操作符。

# 打字— 3.5 以上

从 Python 3 开始，打字就已经成为一项功能。因为我们是开发者，而不是历史学家，所以现在(2020 年)将会有类型注释和类型提示。

Python 不需要给变量赋值类型。这可能是我如此热爱这门语言的部分原因。清晰易读的语法。用 24 种不同的方法中的一种来编码解决方案并仍然得到相同结果的能力。

但是后来…应用程序增长了。或者你得看看你几个月或几年没碰过的代码。或者，最糟糕的是，你还得理解别人写的代码！**战栗**

然后你意识到输入变量对解释器没有好处。这是给你的。

键入有助于您在编写代码时以及以后理解代码。TypeScript 如此受欢迎是有原因的，即使 JavaScript 完全能够编译成没有类型的工作代码。

```
from typing import List
​
def print_cats(cats: List[str]) -> None:
    for cat in cats:
        print(f"{cat} has a name with {len(cat)} letters.")
​

class Cat(object):
    def __init__(self, name: str, age: int, **attrs):
        self.cattributes = {
            "name": name,
            "age": age,
            **attrs
        }
​
cats = "this still works w/o type annotation!"
cats: List[str] = ["Meowie", "Fluffy", "Deathspawn"]
# not a list of strings, but Python will not check
cats2: List[str] = [Cat("Meowie", 2), Cat("Deathspawn", 8)]
​
print_cats(cats) # succeeds
print_cats(cats2) # fails
```

这将返回:

```
Meowie has a name with 6 letters.
Fluffy has a name with 6 letters.
Deathspawn has a name with 10 letters.
--------------------------------------------
...
TypeError: object of type 'Cat' has no len()
```

在这里，类型注释并没有拯救我们，那么为什么要使用它们呢？因为当创建变量`cats`并用`List[str]`键入它时，显而易见，分配的数据应该与该结构匹配。所以当一个函数稍后消耗`cats`时，它变得(更加)明显，你传递的数据正是它所期望的。

我认为，对于具有复杂类型的可维护代码来说，这变得更加有用——必要。

```
from typing import List
​
​
class Cat(object):
    def __init__(self, name: str, age: int, **attrs):
        self.cattributes = {
            "name": name,
            "age": age,
            **attrs
        }
​
# creating a type variable
Cats: type = List[Cat]
​
​
def print_cats(cats: Cats) -> None:
    for cat in cats:
        name: str = cat.cattributes.get("name")
        print(f"{name} has a name with {len(name)} letters.")

cats = [Cat("Meowie", 2), Cat("Deathspawn", 8)]
​
print_cats(cats)
```

输出:

```
Meowie has a name with 6 letters.
Deathspawn has a name with 10 letters.
```

在函数/方法的定义中输入参数被称为*类型提示*。类型甚至不必是 Python 数据类型或来自`typing`模块。一个简单的，虽然有点尴尬的文本提示是完全合法的:

```
import pandas as pd
​
cols = ["name", "age", "gender"]
data = [["Meowie", 2, "female"],
       ["Fluffy", 5, "male"],
       ["Deathspawn", 8, "rather not say"]]
df: pd.DataFrame = pd.DataFrame() # not very descriptive
df: "name (string), age (integer), gender (string)" = \
    pd.DataFrame(data, columns=cols)
```

类似这样的东西在包含大量复杂类型变量的数据处理管道中可能会很有用，并且您的脑袋开始发晕，试图让它们保持直线。在变量 mouseover 上有类型提示的 ide 也会显示那个提示，而不是`pandas.DataFrame`，如果它有 Python 支持的话。

**奖励:**在 Python 4 中，前向引用将被允许开箱即用。这意味着您可以注释尚未定义的类型。我们现在仍然可以通过将`from __future__ import annotations`放在文件的顶部来利用这一优点，然后做如下事情:

```
from __future__ import annotations
​
class Food:
    """ Look at the type hint. Food is legal even without the 
    class defined yet.
    """
    def __init__(self, ingred_1: Food, ingred_2: Food) -> None:
        self.ingred_1 = ingred_1
        self.ingred_2 = ingred_2
```

# 原生类型注释— 3.9(很快将成为我的最爱)

这将是真正的快速，因为我把打字部分拖了出来。

[内置的泛型类型](https://www.python.org/dev/peps/pep-0585/)将是 3.9 中的东西，所以从`typing`导入来添加参数到泛型数据类型将不再是必要的。从 3.7 开始，`from __futures__ import annotations`就提供了这一功能，但这是因为它阻止了类型引用在运行时被求值。

这让我对从 3.8 升级感到兴奋。现在我将`typing`导入到每个模块中，或者从我保存在代码旁边的类型定义模块中导入。

示例(信用: [PEP 585](https://www.python.org/dev/peps/pep-0585/) ):

```
>>> l = list[str]()
[]
>>> list is list[str]
False
>>> list == list[str]
False
>>> list[str] == list[str]
True
>>> list[str] == list[int]
False
>>> isinstance([1, 2, 3], list[str])
TypeError: isinstance() arg 2 cannot be a parameterized generic
>>> issubclass(list, list[str])
TypeError: issubclass() arg 2 cannot be a parameterized generic
>>> isinstance(list[str], types.GenericAlias)
Truedef find(haystack: dict[str, list[int]]) -> int:
    ...
```

# 海象运营商——3.8(我的最爱)

海象有眼睛`:`然后是长牙`=`。

`:=`是一个*赋值表达式*，在 Python 3.8 中新增。

```
complicated = {
    "data": {
        "list": [1,2,3],
        "other": "stuff"
    }
}
​
if (nums := complicated.get('data').get('list')):
    print(nums)
```

结果:

```
1
2
3
```

如果没有海象，这将是更多的代码行。

```
...
​
nums = complicated.get('data').get('list')
if nums:
    print(nums)
```

这不是世界末日，但是因为控制流语句在编程中经常被使用，一旦你开始使用 walrus 操作符，你就不会停止。

来自 [PEP 572](https://www.python.org/dev/peps/pep-0572/#id9) :

> 这种命名表达式的值与合并表达式的值是相同的，但有一个额外的副作用，即目标被赋予该值

换句话说，用一个表达式杀死两个语句。

当我复制/粘贴 PEP 指南时，这里有几个我认为很好的例子。迫不及待地想在列表理解中尝试一下 walrus 运算符。

```
# Handle a matched regex
if (match := pattern.search(data)) is not None:
    # Do something with match
​
# A loop that can't be trivially rewritten using 2-arg iter()
while chunk := file.read(8192):
   process(chunk)
​
# Reuse a value that's expensive to compute
[y := f(x), y**2, y**3]
​
# Share a subexpression between a comprehension filter clause and its output
filtered_data = [y for x in data if (y := f(x)) is not None]
```

# 结论

最近对 Python 语言的补充提供了一些相当不错的特性供练习。我希望我对打字和海象操作符的看法对你有用。

在[第 2 部分](https://medium.com/@nball/whats-new-in-python-2020-part-2-1d9abb0f0e7c)中，我们将看看内置库的数据类，同时也看看需要考虑的一些原因`pydantic`。我们还将介绍 dictionary union 操作符，这是 Python 3.9 语法中的一项新内容。