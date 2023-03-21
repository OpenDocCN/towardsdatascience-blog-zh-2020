# 为没有时间测试的人准备的单元测试

> 原文：<https://towardsdatascience.com/unit-tests-for-people-with-no-time-for-tests-f1416e43087f?source=collection_archive---------34----------------------->

## 如何提高代码质量和安心的快速指南

![](img/f0f68e9cb2ec4dd613a335bfd9efe5e8.png)

照片由[帕特里克·福尔](https://unsplash.com/@patrickian4?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/balance?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

测试可能是一个大傻瓜，但是可靠的测试覆盖是成熟工程团队的标志。如果你能选择一件事来解决代码质量问题，单元测试是你能做的最好的投资。

## 什么是单元测试？

这是首先需要澄清的。测试有很多种类型:单元、回归、集成、端到端等。除了单元测试之外，大多数其他测试都处理组件之间的集成。这些组件可以是类、微服务或包。因为应用程序有不同的大小和复杂性，所以很难将测试归入其中的一个类别。但是单元测试是很容易定义的:*单元测试应该测试项目中最小的部分。*大多数时候，这意味着功能。

为了说明各种情况，我们将使用以下函数:

## 怎么写测试？

Python 有一个强大的单元测试包:“unittest”。还有其他几个有很多优点和缺点需要评估。但是请记住，我们是没有时间进行测试的人。使用 unittest 永远不会出错，所以我们将在示例中使用它。

python 中的单元测试是这样的:

这里唯一强制的事情是，您的测试需要是继承 TestCase 的类中的方法。您的所有测试都必须是名称以“test_”开头的方法。其他的都将被跳过。

从命令行，这就是你运行所有单元测试文件的方式:

```
python -m unittest discover
```

这些高级选项可以让您对正在执行的内容进行额外的控制:

```
#specify what modules to be executed
python -m unittest test_module1 test_module2#execute all tests inside a specific class
python -m unittest test_module.TestClass#cherry pick a test to be executed
python -m unittest test_module.TestClass.test_method
```

# 移除所有依赖关系

第一步是识别函数的所有依赖项。在我们的豚鼠函数中，我们可以确定 2 个外部端点。第一个是获取数据进行处理的地方，另一个是将结果发送到的地方。同样的逻辑也适用于正在读写的磁盘文件或数据库查询。这里不明显的依赖是“calculate_avg”。因为我们只是在测试我们的“单元”，所以我们也应该把其他的函数调用算作一个依赖

一旦确定了依赖关系，我们就应该模仿它们。主要原因是将我们的测试与我们无法控制的环境设置和外部服务隔离开来。这里有几个原因:

*   当这个测试的自动执行运行时，它们可以是离线的
*   与这种依赖关系的交互可能会导致整个系统发生不希望的变化(比如改变数据库中的数据)
*   您想要测试不同的场景，并且可能很难强迫这个依赖“失败”或者用不常见的数据测试执行
*   calculate_avg 可能改变了它的实现(可能现在已经坏了),这可能会导致我们的测试失败。
*   因为如果你测试两个组件之间的集成，这将是一个集成测试而不是一个单元测试

## 怎么嘲讽？

Python 有一个强大的模仿库，可以帮助你模仿所有类型的依赖。我将在我所有的例子中使用 decorators，因为我发现这样更容易阅读测试。我发现模仿是一种更好的学习方式，所以我编写了一份常见模仿场景的备忘单。记得参考我们的豚鼠功能

**修补依赖函数**

**在同一测试中修补多个依赖项。**

这里的问题是，装饰器离方法声明越近，它就越早在参数列表中被传递。

```
@mock.patch("guinea_pig_module.requests.post")
@mock.patch("guinea_pig_module.requests.get")
@mock.patch("guinea_pig_module.calculate_avg")
def test_get_sum(self, mocked_avg, mocked_get, mocked_post):
    pass
```

**改写怪异**

另一个常见的错误是在错误的地方模仿正确的函数。请注意“请求”中的“get”函数。重写“get”的正确方法是传递几内亚猪路径，然后传递“requests.get ”,就像它是在几内亚猪内部声明的一样。这不行:

```
@mock.patch("requests.get")
```

**删除样板代码**

到处添加这些装饰者会变得重复，事情也会变得混乱。一种选择是将装饰器一直添加到类声明的顶部，而不是方法声明。这样做的好处是，在类级别上进行的模拟会自动渗透到该类中的所有方法。所以要明智地使用它。

您还需要考虑方法签名中的这些新模仿。第一个参数总是在方法级和接下来的类级中声明的模拟。

**用护栏修补**

当您模仿一个函数时，默认情况下它返回一个 mock()对象。这个对象允许你做任何事情。这将起作用:

```
x = mocked_method()
x.fake_method()
x.not_real_method()
x.fooooo()
print(x.anything)
```

当它有意义的时候，你可能想要确保你以正确的方式使用被嘲笑的回答。

```
#this returns a mock object, but only with pre-existent methods
#and attributes from the class being specced@mock.patch("your_class_here", autospec=True)
```

试图访问原始类中不存在的元素将引发异常。

另一个用例可能是在对象内部修补一个特定的方法，而不模仿其他任何东西。这可以这样实现:

```
@mock.patch.object("your_class_here", "target_method_here")
```

**副作用调用副作用**

side_effect 的最后一个用例是引发异常。记住“return_value”有一个静态响应。因此，如果您想引发一个异常，您必须像这样做:

```
x.side_effect = RuntimeError("Bad")#this will raise an exception
x()#this will raise an exception in the 3rd call
x.side_effect = [1, 2, RuntimeError("Bad")]
```

# 覆盖面很重要

下一步是关注覆盖率。争取 100%的覆盖率。为了实现这一点，您可能需要为同一个函数编写几个测试。您将使用模拟技能来确保每个 if/else 和异常处理程序块都得到执行。回到我们的豚鼠功能，我们需要一些东西来达到 100%:

*   返回年龄小于 0 的记录
*   resp.json()应返回非空响应。您需要循环来执行
*   抛出异常

“覆盖率”是一个非常有用的软件包，它可以帮助你识别仍然缺少测试的行。要使用它，您需要对前面的单元测试命令做一个小小的修改:

```
python -m unittest discover#becomes
coverage run -m unittest discover#to see the reports from the run
coverage report -m
```

这将会正常运行您的测试，但是也会跟踪您的项目中正在执行的每一行。最后，您将看到这样一个漂亮的报告:

```
Name                      Stmts   Miss  Cover   Missing
-------------------------------------------------------
module1.py                   20      4    80%   33-35, 39
-------------------------------------------------------
TOTAL                        20      4    80%
```

利用这一点来跟踪任何被忽略的线。回到我们的豚鼠函数，我们的测试看起来像这样:

仅仅通过达到 100%的覆盖率，这些测试就发现了代码中的两个错误。

*   Try 子句未捕获到请求超时异常。这导致了功能崩溃
*   第二个是在创建大型 Try 块时常见的错误。如果“resp.json()”抛出异常，“idx”永远不会初始化。这将导致在打印前一个异常的错误信息时发生异常。不好了。

一旦这些错误被修复，新函数看起来就像这样:

# 输入和输出

这个函数只返回一个布尔值，但是在某些情况下，您可能需要验证一个需要符合特定规范的对象。所以你的测试需要验证这一点。输入更棘手，从常见的疑点开始:

*   列表:空列表，1 个元素，少量元素，大列表
*   文件:空文件、无文件、小文件、大文件
*   整数和浮点数:负数？、零、少数正数、大数
*   字符串:空字符串，小字符串，大字符串

您需要考虑什么对您正在测试的功能有意义。对于豚鼠的功能来说，输入被传递给被模仿的依赖者。所以值是多少并不重要。如果你不确定，就按照通常嫌疑人名单。不要想太多，思考比写那些测试要花更多的时间。偏向“测试越多越好”的一边

# 结论

记住，单元测试就像一个房子警报系统。这是为了让你安心，它本身不应该是一个项目。你只是在确保从现在起 6 个月后所做的改变不会破坏你的功能行为。所以，花更多的时间确保你涵盖了一些广泛的领域，而不是非常具体的案例。利用模拟包中的“规格”。你刚刚写了你的函数，你知道如何使用它。设置好闹钟，去做你最喜欢做的事情。造东西！