# 新手通常误用 Python 的 5 种场景

> 原文：<https://towardsdatascience.com/5-scenarios-where-beginners-usually-misuse-python-98bac34e6978?source=collection_archive---------26----------------------->

## **更好地利用 Python**

![](img/e19851f0173c5635953dacf09fada5da.png)

约翰·马特丘克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Python 是当今许多初学者的首选编程语言。简单易学的语法、大量的库和丰富的社区是 Python 飞速发展的主要原因。

当我六年前满载着 Java 登陆 Python 时，我发现自己很多次都是在用 Java 编写 Python 代码。作为新手，我没有充分利用 Python 的优势，在某些情况下，我甚至误用了它。

回到现在，我仍然看到一些新的初学者在没有花时间阅读最佳实践和建议的情况下用 Python 跳跃和编码。为了对此有所帮助，我列出了以下五个 Python 被误用的场景以及更好地利用它的建议。

## #1 使用列表时

![](img/b4be4b3a7e776faa51bab3bdaf397aef.png)

列表和元组

List 允许存储任何数据类型的元素，没有大小限制。尽管这种灵活性使 list 成为首选的数据收集方式，但实际上，什么时候使用它，什么时候不使用它，都有一些最佳实践。

> *当存储相同性质(数据类型和含义)的元素时，应使用 List。*

*Python 并没有以编程方式对此进行限制。将单一性质的项目存储在一个列表中使开发人员的工作更容易。开发人员很容易就能预料到列表中未来会有哪些项目，并据此满怀信心地编写脚本。*

考虑下面的*列表。这个列表没有单一的项目。开发人员无法确定该列表是否包含房屋部件、尺寸或其他内容，因此他应该分别处理所有不同的项目。*

```
list_of_things = ['Door', 2, 'Window', True, [2.3, 1.4])]
```

考虑下面的*水果清单*和*分数清单*。根据前几项，您可以很容易地推断出第一个列表将总是包含水果名称，第二个列表将包含分值。

```
list_of_fruits = ['apple', 'orange', 'pear', 'cherry', 'banana']
list_of_scores = [80, 98, 50, 55, 100]
```

另一方面， *tuple* 更适合用于存储具有不同含义或数据类型的项目。Tuple 不提供在不创建新对象的情况下存储无限项的灵活性(因为 tuple 是不可变的)。

## #2 迭代连接字符串时

![](img/6aac6165c68237f3f1bb2ab5f127a54a.png)

串并置

你可能听说过在 Python 中一切都是一个*对象*，对象可能是*不可变的*和*可变的*。不可变对象需要在更新赋值时创建新的对象，而可变对象则不需要。

假设您想在一个字符串中生成整个字母表。由于 string 是一个不可变的对象，每当您使用“+”操作符连接字符串值时，您总是会生成新的对象。

```
one_line_alphabet = ''
for letter_index in range(ord('a'), ord('z')):
    one_line_alphabet += chr(letter_index)
```

连接字符串的首选方式是使用 *join* 函数。使用*连接*功能可将计算时间减少约 3 倍。在我执行的一个测试中，迭代连接一百万个字符串值花费了 *0.135s* ，而使用 *join()* 函数仅花费了 *0.044s* 。

```
small_letters = [chr(i) for i in range(ord('a'), ord('z')+1)]
single_line_alphabet = ''.join(small_letters)
```

所以**，**每当你需要连接一个字符串列表时，使用 *join* 函数。通过 *join* 函数连接几个字符串不会真正让您看到性能差异。要连接几个字符串值，使用*。格式*代替加号运算符。例如:

```
name = 'John'
surname = 'Doe'
full_name = '{name} {surname}'.format(name=name, surname=surname)
```

## #3 读写文件时

![](img/4f3fba60b84574be48207fdd5f1f2296.png)

答。txt 文件

要在 Python 中读写文件，你需要首先通过内置的 *open* 函数打开文件。你打开文件，读或写内容，然后关闭文件。当您这样做时，可能会出现一些问题。忘记关闭文件和不处理异常就是其中的一些。

完成作业后忘记关闭文件会导致问题。例如，如果您在写入文件后忘记关闭文件，写入的内容将不会出现在文件中，并且您将在文件仍处于打开状态时保留计算机中分配的资源。如果没有手动处理异常，并且在处理文件时出现错误，文件将保持打开状态。

```
f = open(file='file.txt', mode='r')
lines = f.readlines()
...
f.close()
```

每当你打开文件*时，建议使用带有*关键字的*。用*是一个[上下文管理器](https://book.pythontips.com/en/latest/context_managers.html)，它包装代码并确保为您自动处理异常。例如，当您读/写文件时，无论 *with-body* 中有什么可能失败，都会自动处理异常，并且文件总是为您关闭。

```
with open('file.txt') as f:
    read_data = f.read()
    ...
```

当***s***kipping*with*你应该自己处理一切。关闭文件和异常处理应该由您来明确处理。相反，让你的生活变得更轻松，让*和*一起管理局面。

## #4 跳过发电机时

![](img/6386487afd7df9a4d7556aacb3618f4b.png)

将所有值保存在一个列表中，而不是逐个生成它们

在许多情况下，您需要生成一个值列表，以便稍后在脚本中使用。比方说，您需要为前 100 个数字生成所有三个数字的组合。

```
combinations = []
value = 100
for i in range(value):
    for j in range(value):
        for k in range(value):
            combinations.append((i, j, k))
```

当执行完成时，*组合*列表将包含 1M 个元组，每个元组具有三个 *int* 值。这些值将驻留在内存中，直到被删除。使用 *sys* 模块中的 *getobjectsize* 函数检查对象大小，得到的大小为 8.29MB

您可以创建一个 [*生成器*](https://wiki.python.org/moin/Generators) 来代替使用列表来存储值并将它们保存在内存中，无论何时您调用它，它都会一次生成一个组合。这减少了内存消耗并保持了更快的执行速度。

```
def generate_combinations_of_three(value):
    for i in range(value):
        for j in range(value):
            for k in range(value):
                yield (i, j, k)gen = generate_combinations_of_three(100)next(gen) # yields (0, 0, 0)
next(gen) # yileds (0, 0, 1)
...
```

因此，尽可能使用发电机。请始终记住，内存容量是有限的，并尽可能优化内存使用。使用生成器，尤其是在开发可伸缩的解决方案时。发电机很重要，考虑一下吧！

## #5 使用理解时

![](img/c99073b7e45ec23eef982bdeda9e8a5c.png)

列出理解

Pythonista 描述了一个程序员，每当用 Python 编码时，他都遵循来自[Python 之禅](https://www.python.org/dev/peps/pep-0020/)的指导方针。如果你是 Python 的新手，你会倾向于夸大这个禅宗的某些观点，而轻描淡写其他观点。

当你开始了解理解的时候，你会注意到这一点——你倾向于翻译理解中的“每一个”循环。假设您有一个要展平的三维数字矩阵。

```
matrix = [[[ 1, 2, 3 ],
           [ 4, 5, 6 ],
           [ 7, 8, 9 ]],
          [[ 10, 20, 30 ],
           [ 40, 50, 60 ],
           [ 70, 80, 90 ]]]
```

使用列表理解，扁平化看起来像:

```
flatten_list = [x for sub_matrix in matrix for row in sub_matrix for     
                x in row]
```

使用 for 循环时，展平看起来像:

```
flatten_list = []
for sub_matrix in matrix:
    for row in sub_matrix:
        for x in row:
            flatten_list.append(x)
```

理解很酷，但可读的代码更酷。不要让你的意图总是使用理解。即使这可能需要编写更少的代码，也不要牺牲代码的可读性。

# 结论

无论何时开始学习一门新的编程语言，无论你是否有经验，都要花时间阅读最佳实践。每种语言都有一些使其特殊的成分，所以要确保在正确的地方使用它们。

Python 致力于更快更容易地完成工作，但是你不应该忽视那些可能对你的代码生命周期产生负面影响的小决定。尽可能寻找更好的优化解决方案。

你可以用勺子或叉子切奶酪，但刀子肯定会做得更好。

[](/10-pure-python-functions-for-ad-hoc-text-analysis-e23dd4b1508a) [## 用于即席文本分析的 10 个纯 Python 函数

### 不使用外部库压缩文本数据

towardsdatascience.com](/10-pure-python-functions-for-ad-hoc-text-analysis-e23dd4b1508a) [](https://medium.com/swlh/three-significant-things-to-elevate-your-software-development-career-83be99cedf28) [## 提升你的软件开发职业生涯的三件大事

### 从优秀到卓越

medium.com](https://medium.com/swlh/three-significant-things-to-elevate-your-software-development-career-83be99cedf28) [](https://medium.com/@dardanx/4-things-that-will-earn-you-respect-as-a-developer-eec54d1ac189) [## 作为一名开发人员，赢得尊重的 4 件事

### 迈向成功的事业

medium.com](https://medium.com/@dardanx/4-things-that-will-earn-you-respect-as-a-developer-eec54d1ac189)