# 用这 10 个技巧写出更好的 Python 代码

> 原文：<https://towardsdatascience.com/write-better-python-code-with-these-10-tricks-30b7018e247a?source=collection_archive---------3----------------------->

## 提高您的 Python 技能

## 学习如何用 Pythonic 的方式编码

![](img/23febd9826a56966f909cdafd1fce3af.png)

由[克里斯托弗·高尔](https://unsplash.com/@cgower?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

编码很有趣，用 Python 编码更有趣，因为有许多不同的方法来完成相同的功能。然而，大多数时候，都有首选的实现，有些人称之为 Pythonic。这些 Pythonic 实现的一个共同特点是代码简洁明了。

用 Python 或任何编码语言编程都不是火箭科学，它主要是关于制作技能。如果您有意尝试 Pythonic 编码，这些技术将很快成为您的工具包的一部分，并且您会发现在您的项目中使用它们会越来越自然。因此，让我们来探索一些简单的技巧，希望对你有所帮助。

## 1.负索引

人们喜欢使用序列，因为我们知道元素的顺序，并且我们可以按顺序操作这些元素。在 Python 中，字符串、元组和列表是最常见的序列数据类型。我们可以使用索引来访问单个项目。像其他主流编程语言一样，Python 支持基于 0 的索引，其中我们使用一对方括号中的 0 来访问第一个元素。此外，我们还可以使用 slice 对象来检索序列中的特定元素，如下面的代码示例所示。

```
>>> # Positive Indexing
... numbers = [1, 2, 3, 4, 5, 6, 7, 8]
... print("First Number:", numbers[0])
... print("First Four Numbers:", numbers[:4])
... print("Odd Numbers:", numbers[::2])
... 
First Number: 1
First Four Numbers: [1, 2, 3, 4]
Odd Numbers: [1, 3, 5, 7]
```

然而，Python 更进了一步，支持负索引。具体来说，我们可以使用-1 来引用序列中的最后一个元素，并对项目进行反向计数。例如，倒数第二个元素的索引为-2，依此类推。重要的是，负索引也可以与切片对象中的正索引一起工作。

```
>>> # Negative Indexing
... data_shape = (100, 50, 4)
... names = ["John", "Aaron", "Mike", "Danny"]
... hello = "Hello World!"
... 
... print(data_shape[-1])
... print(names[-3:-1])
... print(hello[1:-1:2])
... 
4
['Aaron', 'Mike']
el ol
```

## 2.检查容器的空度

容器是指那些可以存储其他数据的容器数据类型。一些常用的内置容器是元组、列表、字典和集合。当我们处理这些容器时，我们经常需要在执行附加操作之前检查它们是否包含任何元素。事实上，我们可以检查这些容器的长度，它对应于存储项目的数量。当长度为零时，容器为空。下面给你看一个简单的例子。

```
if len(some_list) > 0:
    # do something here when the list is not empty
else:
    # do something else when the list is empty
```

然而，这并不是最有效的方法。相反，我们可以简单地检查容器本身，它将在包含元素时计算`True`。虽然下面的代码向您展示了主要的容器数据类型，但是这种用法也适用于字符串(即任何非空字符串都是`True`)。

```
>>> def check_container_empty(container):
...     if container:
...         print(f"{container} has elements.")
...     else:
...         print(f"{container} doesn't have elements.")
... 
... check_container_empty([1, 2, 3])
... check_container_empty(set())
... check_container_empty({"zero": 0, "one": 1})
... check_container_empty(tuple())
... 
[1, 2, 3] has elements.
set() doesn't have elements.
{'zero': 0, 'one': 1} has elements.
() doesn't have elements.
```

## 3.用 Split()创建字符串列表

我们经常使用字符串作为特定对象的标识符。例如，我们可以使用字符串作为字典中的关键字。在数据科学项目中，字符串通常是数据的列名。当我们选择多个列时，我们不可避免地需要创建一个字符串列表。事实上，我们可以在列表中使用文字创建字符串。然而，我们必须用引号将每个字符串括起来，这对于我们这些“懒人”来说有点乏味。因此，我更喜欢利用字符串的`split()`方法创建一个字符串列表，如下面的代码片段所示。

```
>>> # List of strings
... # The typical way
... columns = ['name', 'age', 'gender', 'address', 'account_type']
... print("* Literals:", columns)
... 
... # Do this instead
... columns = 'name age gender address account_type'.split()
... print("* Split with spaces:", columns)
... 
... # If the strings contain spaces, you can use commas instead
... columns = 'name, age, gender, address, account type'.split(', ')
... print("* Split with commas:", columns)
... 
* Literals: ['name', 'age', 'gender', 'address', 'account_type']
* Split with spaces: ['name', 'age', 'gender', 'address', 'account_type']
* Split with commas: ['name', 'age', 'gender', 'address', 'account type']
```

如上所示，`split()`方法默认使用空格作为分隔符，并从字符串创建一个字符串列表。值得注意的是，当您创建一个包含一些包含空格的元素的字符串列表时，您可以选择使用不同类型的分隔符(例如，逗号)。

这种用法受到一些内置功能的启发。例如，当你创建一个命名的元组类时，我们可以这样做:`Student = namedtuple(“Student”, [“name”, “gender”, “age”])`。字符串列表指定了元组的“属性”然而，通过这样定义该类，它本身也得到支持:`Student = namedtuple(“Student”, “name gender age”)`。再举一个例子，创建一个*枚举*类支持相同的替代解决方案。

## 4.三元表达式

在许多用例中，我们需要根据条件定义具有特定值的变量，我们可以简单地使用 *if…else* 语句来检查条件。然而，它需要几行代码。如果我们只处理一个变量的赋值，我们可能想要使用三元表达式，它只需要一行代码就可以检查条件并完成赋值。此外，它的形式更短，这使得代码更加简洁。考虑下面的例子。

```
# The typical way
if score > 90:
    reward = "1000 dollars"
else:
    reward = "500 dollars"# Do this instead
reward = "1000 dollars" if score > 90 else "500 dollars"
```

有时候，我们可以从一个已定义的函数中获取一些数据，我们可以利用这一点，写一个三元表达式的速记运算，如下所示。

```
# Another possible scenario
# You got a reward amount from somewhere else, but don't know if None/0 or not
reward = reward_known or "500 dollars"
# The above line of code is equivalent to below
reward = reward_known if reward_known else "500 dollars"
```

## 5.文件对象的 With 语句

我们经常需要从文件中读取数据，并向文件中写入数据。最常见的方法是简单地使用内置的`open()`函数打开一个文件，它创建了一个我们可以操作的文件对象。您以前遇到过以下问题吗？

```
>>> # Create a text file that has the text: Hello World!
... 
... # Open the file and append some new data
... text_file0 = open("hello_world.txt", "a")
... text_file0.write("Hello Python!")
... 
... # Open the file again for something else
... text_file1 = open("hello_world.txt")
... print(text_file1.read())
... 
Hello World!
```

在前面的代码片段中，我们从一个文本文件开始，该文件包含文本“Hello World！”然后，我们将一些新数据添加到文件中。然而，过了一会儿，我们又想在文件上工作了；当我们读取文本文件时，它仍然有旧的数据。换句话说，附加的文本不包括在文本文件中。为什么会这样？

这是因为我们没有首先关闭文件对象。如果不关闭文件，将无法保存更改。事实上，我们可以在 file 对象上显式调用`close()`方法。但是，我们可以使用“with”语句来实现这一点，它会自动为我们关闭 file 对象，如下所示。当我们完成对文件的操作时，我们可以通过访问 file 对象的`closed`属性来验证文件是否被关闭。

```
>>> with open("hello_world.txt", "a") as file:
...     file.write("Hello Python!")
... 
... with open("hello_world.txt") as file:
...     print(file.read())
... 
... print("Is file close?", file.closed)
... 
Hello World!Hello Python!Hello Python!
Is file close? True
```

更一般地说， *with* 语句是在 Python 中使用上下文管理器的语法。前面的例子涉及文件操作，因为文件是共享资源，我们负责释放这些资源。上下文管理器可以帮助我们完成工作。如前所示，文件操作结束后，通过使用带有语句的*自动关闭文件。你可以在我的[上一篇文章](https://medium.com/better-programming/context-managers-in-python-go-beyond-with-open-as-file-85a27e392114)中了解更多关于上下文管理的内容。*

## 6.评估多个条件

我们经常需要评估多种情况。有几种可能的情况。对于数值，我们可以对同一个变量进行多次比较。在这种情况下，我们可以将这些比较链接起来。

```
# Multiple Comparisons
# The typical way
if a < 4 and a > 1:
    # do something here# Do this instead
if 1 < a < 4:
    # do somerthing here
```

在其他一些场景中，我们可以进行多重等式比较，并且我们可以利用下面的技术，使用关键字中的*进行成员测试。*

```
# The typical way
if b == "Mon" or b == "Wed" or b == "Fri" or b == "Sun":
    # do something here# Do this instead, you can also specify a tuple ("Mon", "Wed", "Fri", "Sun")
if b in "Mon Wed Fri Sun".split():
    # do something here
```

另一种技术是使用内置的`all()`和`any()`函数来评估多个条件。具体来说，当 iterable 中的元素都是`True`时，`all()`函数的计算结果将是`True`，因此该函数适合代替一系列的 and 逻辑比较。另一方面，当 iterable 中的任何元素为`True`时，`any()`函数将计算为`True`，因此适合替换一系列 OR 逻辑运算。相关的例子如下。

```
# The typical ways
if a < 10 and b > 5 and c == 4:
    # do somethingif a < 10 or b > 5 or c == 4:
    # do something# Do these instead
if all([a < 10, b > 5, c == 4]):
    # do somethingif any([a < 10, b > 5, c == 4]):
    # do something
```

## 7.在函数声明中使用默认值

在几乎所有的 Python 项目中，大部分代码都涉及到创建和调用函数。换句话说，我们不断地处理函数声明和重构。在许多情况下，我们需要多次调用一个函数。根据不同的参数集，该函数的运行方式会略有不同。然而，有时一组参数可能比其他参数更常用，在这种情况下，我们应该考虑在声明函数时设置默认值。考虑下面这个微不足道的例子。

```
# The original form:
def generate_plot(data, image_name):
    """This function creates a scatter plot for the data"""
    # create the plot based on the data
    ...
    if image_name:
        # save the image
        ...# In many cases, we don't need to save the image
generate_plot(data, None)# The one with a default value
def generate_plot(data, image_name=None):
    pass# Now, we can omit the second parameter
generate_plot(data)
```

需要注意的一点是，如果在设置默认值时处理可变数据类型(例如，列表、集合)，请确保使用`None`而不是构造函数(例如，arg_name=[])。因为 Python 在定义函数对象的地方创建函数对象，所以提供空列表会被函数对象“卡住”。换句话说，函数对象不会在你调用它的时候被创建。相反，您将在内存中处理同一个函数对象，包括它最初创建的默认可变对象，这可能会导致意想不到的行为(更多讨论见[。](https://medium.com/better-programming/top-5-mistakes-you-make-when-declaring-functions-in-python-b7a0747711a4)

## 8.使用计数器进行元素计数

当我们在一个列表、元组或字符串中有多个项目时(例如，多个字符)，我们经常想要计算每个项目有多少个。要做到这一点，可以为该功能编写一些乏味的代码。

```
>>> words = ['an', 'boy', 'girl', 'an', 'boy', 'dog', 'cat', 'Dog', 'CAT', 'an','GIRL', 'AN', 'dog', 'cat', 'cat', 'bag', 'BAG', 'BOY', 'boy', 'an']
... unique_words = {x.lower() for x in set(words)}
... for word in unique_words:
...     print(f"* Count of {word}: {words.count(word)}")
... 
* Count of cat: 3
* Count of bag: 1
* Count of boy: 3
* Count of dog: 2
* Count of an: 5
* Count of girl: 1
```

如上所示，我们首先必须创建一个只包含唯一单词的集合。然后我们迭代单词集，并使用`count()`方法找出每个单词的出现次数。然而，有一个更好的方法——使用`Counter`类，它是为完成这个计数任务而设计的。

```
>>> from collections import Counter
... 
... word_counter = Counter(x.lower() for x in words)
... print("Word Counts:", word_counter)
... 
Word Counts: Counter({'an': 5, 'boy': 4, 'cat': 4, 'dog': 3, 'girl': 2, 'bag': 2})
```

`collections`模块中提供了*计数器*类。为了使用该类，我们简单地创建了一个生成器:`x.lower() for x in words`，并且每一项都会被计数。如你所见，*计数器*对象是一个类似 dict 的映射对象，每个键对应于单词列表的唯一项，而值是这些项的计数。很简洁，对吧？

此外，如果您对找出单词列表中最频繁出现的条目感兴趣，我们可以利用 *Counter* 对象的`most_common()`方法。下面的代码向您展示了这种用法。你只需要指定一个整数(N)，它会从列表中找出最频繁出现的 N 个条目。顺便提一下，`Counter`对象也可以处理其他序列数据，比如字符串和元组。

```
>>> # Find out the most common item
... print("Most Frequent:", word_counter.most_common(1))
Most Frequent: [('an', 5)]
>>> # Find out the most common 2 items
... print("Most Frequent:", word_counter.most_common(2))
Most Frequent: [('an', 5), ('boy', 4)]
```

## 9.不同订单要求的排序

在许多项目中，对列表中的项进行排序是一项普遍的任务。最基本的排序是基于数字或者字母顺序，我们可以使用内置的`sorted()`函数。默认情况下，`sorted()`函数将按升序对列表(实际上，它可以是任何可迭代的)进行排序。如果我们将`reverse`参数指定为`True`，我们可以得到降序排列的条目。下面显示了一些简单的用法。

```
>>> # A list of numbers and strings
... numbers = [1, 3, 7, 2, 5, 4]
... words = ['yay', 'bill', 'zen', 'del']
... # Sort them
... print(sorted(numbers))
... print(sorted(words))
... 
[1, 2, 3, 4, 5, 7]
['bill', 'del', 'yay', 'zen']
>>> # Sort them in descending order
... print(sorted(numbers, reverse=True))
... print(sorted(words, reverse=True))
... 
[7, 5, 4, 3, 2, 1]
['zen', 'yay', 'del', 'bill']
```

除了这些基本用法，我们还可以指定`key`参数，以便对复杂的项目进行排序，比如元组列表。考虑以下这种情况的例子。

```
>>> # Create a list of tuples
... grades = [('John', 95), ('Aaron', 99), ('Zack', 97), ('Don', 92), ('Jennifer', 100), ('Abby', 94), ('Zoe', 99), ('Dee', 93)]
>>> # Sort by the grades, descending
... sorted(grades, key=lambda x: x[1], reverse=True)
[('Jennifer', 100), ('Aaron', 99), ('Zoe', 99), ('Zack', 97), ('John', 95), ('Abby', 94), ('Dee', 93), ('Don', 92)]
>>> # Sort by the name's initial letter, ascending
... sorted(grades, key=lambda x: x[0][0])
[('Aaron', 99), ('Abby', 94), ('Don', 92), ('Dee', 93), ('John', 95), ('Jennifer', 100), ('Zack', 97), ('Zoe', 99)]
```

上面的代码通过利用 lambda 函数向您展示了两个高级排序示例，该函数被传递给`key`参数。第一个是使用降序对项目进行排序，而第二个是使用默认的升序。如果我们想把这两个要求结合起来呢？如果你考虑使用`reverse`参数，你可能找错了对象，因为如果你试图按多个标准排序，相反的参数将适用于所有标准。那有什么诀窍呢？请参见下面的代码片段。

```
>>> # Requirement: sort by name initial ascending, and by grades, descending
... # Both won't work
... sorted(grades, key=lambda x: (x[0][0], x[1]), reverse=True)
[('Zoe', 99), ('Zack', 97), ('Jennifer', 100), ('John', 95), ('Dee', 93), ('Don', 92), ('Aaron', 99), ('Abby', 94)]
>>> sorted(grades, key=lambda x: (x[0][0], x[1]), reverse=False)
[('Abby', 94), ('Aaron', 99), ('Don', 92), ('Dee', 93), ('John', 95), ('Jennifer', 100), ('Zack', 97), ('Zoe', 99)]
>>> # This will do the trick
... sorted(grades, key=lambda x: (x[0][0], -x[1]))
[('Aaron', 99), ('Abby', 94), ('Dee', 93), ('Don', 92), ('Jennifer', 100), ('John', 95), ('Zoe', 99), ('Zack', 97)]
```

如您所见，通过将`reverse`参数设置为`True`或`False`，两者都不起作用。相反，诀窍是否定等级，因此当您按默认升序排序时，分数将因为这些值的否定而反向排序。但是，对于这种方法有一个警告，因为求反只能处理数值，而不能处理字符串。

## 10.不要忘记 defaultdict

字典是一种有效的数据类型，它允许我们以键值对的形式存储数据。要求所有的键都是可散列的，这样在幕后，存储这些数据可能涉及到使用散列表。这种实现允许数据检索和插入的 O(1)效率。然而，应该注意的是，除了内置的 *dict* 类型之外，我们还可以使用其他的字典。其中，我想讨论一下 *defaultdict* 类型。与内置的 *dict* 类型不同， *defaultdict* 允许我们设置一个默认的工厂函数，当键不存在时创建一个元素。您可能对以下错误并不陌生。

```
>>> student = {'name': "John", 'age': 18}
... student['gender']
... 
Traceback (most recent call last):
  File "<input>", line 2, in <module>
KeyError: 'gender'
```

假设我们在处理单词，我们想将相同的字符组成一个列表，这些列表与作为键的字符相关联。这里有一个使用内置*字典*类型的简单实现。值得注意的是，检查 dict 对象是否有`letter`键至关重要，因为如果键不存在，调用`append()`方法会引发`KeyError`异常。

```
>>> letters = ["a", "a", "c", "d", "d", "c", "a", "b"]
... final_dict = {}
... for letter in letters:
...     if letter not in final_dict:
...         final_dict[letter] = []
...     final_dict[letter].append(letter)
... 
... print("Final Dict:", final_dict)
... 
Final Dict: {'a': ['a', 'a', 'a'], 'c': ['c', 'c'], 'd': ['d', 'd'], 'b': ['b']}
```

让我们看看如何使用 *defaultdict* 来编写更简洁的代码。虽然这个例子很简单，但它只是给了你一些关于 *defaultdict* 类的想法，这样我们就不用处理字典对象中不存在的键了。

```
>>> from collections import defaultdict
... 
... final_defaultdict = defaultdict(list)
... for letter in letters:
...     final_defaultdict[letter].append(letter)
... 
... print("Final Default Dict:", final_defaultdict)
... 
Final Default Dict: defaultdict(<class 'list'>, {'a': ['a', 'a', 'a'], 'c': ['c', 'c'], 'd': ['d', 'd'], 'b': ['b']})
```

## 结论

在阅读本文之前，您可能已经知道了其中的一些技巧，但是我希望您仍然能够很好地掌握这些技巧。在您的项目中实践这些习惯用法将使您的 Python 代码更具可读性和性能。

这件作品到此为止。感谢阅读。