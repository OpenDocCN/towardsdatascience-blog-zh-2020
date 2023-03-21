# SQLAlchemy 中的自动反映表和列

> 原文：<https://towardsdatascience.com/auto-reflecting-tables-in-sqlalchemy-f859ff3b6aab?source=collection_archive---------39----------------------->

## 或者几行代码让我想在街上翻跟斗

![](img/3fd05accf011043f37078d945718b9db.png)

大卫·克洛德的《向上冲刺》

呻吟，不是另一篇 SQLAlchemy 文章！

相信我，这个不一样…

在对 SQAlchemy [docs](https://docs.sqlalchemy.org/en/13/) 进行了大量的自我反省和挖掘之后，我有了一个重要的发现:您不需要使用基于类的属性来利用基于会话的查询的强大功能。

本文中的代码是根据 Apache 2.0 软件许可证授权的。下面是将要讨论的 Python 代码的一部分:

[https://gist . github . com/steadynotion/129 c 7 b 58 ff 0 a 146d 45083527888 c 5749](https://gist.github.com/steadynotion/129c7b58ff0a146d45083527888c5749)

SQLAlchemy 中自动反映表的扰流板代码

简单吧？

嗯，算是吧:请允许我解释一下。

我已经创作了几个使用 Pandas 和 SQLite 的 Python 脚本。对于那些还没有享受过与熊猫一起工作的乐趣的人来说，这是一个惊人的包，在数据科学和许多其他领域有大量的应用。我惊叹于为各种 Pandas 函数指定各种关键字参数以获得惊人结果的简单性。我的脚本使用 SQLite 数据库来存储传入的、“中游”和结果数据集。虽然 SQLite 非常适合原型开发，但我现在需要更多。

比如认证。

我*可以*移植我现有的代码来指向一个不同的数据库，但是我**真正想要的是与方言无关的东西。进入 SQLAlchemy 及其强大的对象关系映射器(ORM)。它允许开发人员作为 Python 友好的类与他们的数据库进行交互。非常酷！**

SQLAlchemy 的巨大优势之一是基于会话的查询，它采用 Python 面向对象的语法，而不是 SQL 语句。对于那些使用过两种或两种以上数据库技术的人来说，我有点不好意思解释为什么这是有益的。对于那些没有享受过这种乐趣的人来说，这就像是在一次环球电话会议上，与每个外向、健谈的人交谈时雇佣一名翻译。

SQLAlchemy 的 ORM 是你的翻译器。标题中有“物体”意味着它热衷于…物体。有几个扩展 SQLAlchemy 的包，如 [Elixir](https://pypi.org/project/Elixir/) 和[SQLSoup](https://pypi.org/project/sqlsoup/)；他们也使用面向对象的方法。如果您的代码使用( *drum roll please…)* 对象，这是没有问题的。

一个小问题是，我的代码中的一些*咳咳* *大部分*使用 JavaScript 对象符号(JSON)在运行时配置参数。我有一个装满 JSON 趣闻的曲棍球球袜:这些是另一篇文章的主题。可以说，我的许多 Python 脚本使用从 JSON 获得的基于`string`的参数进行配置。

在查看了 SQLAlchemy 文档并通过 Eclipse 调试器进行了一些检查之后，我拼凑了一个小而庞大的代码片段，它使我能够通过名称引用给定数据库中的每一个表和列，而不必将它们指定为对象和属性。

当我第一次看到一个没有错误的控制台时，我想在街上翻跟斗。

有些人可能会问‘那又怎样，我为什么要在乎？’也许一个更充分的例子是合适的。在深入下面的代码之前，如果您想知道如何使用 SQLAlchemy 配置数据库，我建议您查看以下有用的文章: [SQLAlchemy — Python 教程](https://medium.com/hacking-datascience/sqlalchemy-python-tutorial-abcc2ec77b57)作者 [Vinay Kudari](https://medium.com/u/3fbe4f6ebe31?source=post_page-----f859ff3b6aab--------------------------------) 和[如何使用 Python SQLite3 使用 SQLAlchemy](https://medium.com/@mahmudahsan/how-to-use-python-sqlite3-using-sqlalchemy-158f9c54eb32) 作者 [Mahmud Ahsan](https://medium.com/u/4ba89fe45bd8?source=post_page-----f859ff3b6aab--------------------------------) 。

一旦配置好 Python 环境和测试数据库，就可以将下面的代码片段加载到您选择的 IDE 中:

[https://gist . github . com/steadynotion/8d 81 b5 bcdd 74 Fe 75 C3 a3 b 694516d 6528](https://gist.github.com/steadynotion/8d81b5bcdd74fe75c3a3b694516d6528)

一个更充分的例子

SQLAlchemy 中自动反映表的更完整的例子

配置了 SQLAlchemy `engine`之后，您可以将它与测试数据库中的表和列的名称一起提供给`get_data`函数。假设我们的测试数据库包含一个名为“ThisIsATable”的表和一个名为“ThisIsAColumn”的列。在“ThisIsAColumn”中，我们有几个基于文本的行，其中至少有一行包含“请尝试找到我”。

如果一切顺利，您应该有一个 Pandas `DataFrame`包含来自测试数据库的表的子集。多亏了几行代码，我们现在可以自由地编写与数据库无关的查询了。通过这些查询，我们可以获得供熊猫咀嚼的数据，这要归功于简单的基于`string`的参数。

从“易于维护”的角度来看，代码利用了公共函数和属性。调用私有属性通常是不明智的，因为这些属性在未来的版本中可能会改变；我倾向于尽可能回避私人属性。值得一提的是，上面代码中的`Session`是在函数`get_data`中调用的。这不是推荐的做法。有关创建和使用会话的更多信息，请参见这篇由 [A Gordon](https://medium.com/u/a353b021023b?source=post_page-----f859ff3b6aab--------------------------------) 撰写的[文章](https://medium.com/dataexplorations/sqlalchemy-orm-a-more-pythonic-way-of-interacting-with-your-database-935b57fd2d4d)。SQLAlchemy [文档](https://docs.sqlalchemy.org/en/13/orm/session_basics.html)还包含关于创建和使用会话的信息，特别是标题为“我何时构造一个`[Session](https://docs.sqlalchemy.org/en/13/orm/session_api.html#sqlalchemy.orm.session.Session)`、何时提交它以及何时关闭它”的部分。

有了上面的代码，您和您的概念验证 Python 脚本现在能够通过方便的字符串指定最少的信息来自动加载和反映数据库元数据。

弦乐！如此美妙的事情。

跟我一起说:琴弦是很奇妙的东西！

在我看来，这代表了通过 SQLAlchemy 的数据库无关性，以及强大的 Pandas 库和基于`string`的关键字参数进行高级查询的开端！我认为另一个蛇的形象是合适的:它总结了 SQLAlchemy，熊猫和基于文本的互动之间的关系。它们共同组成了三驾马车，为自然语言处理和许多其他用例的无限扩展做好了准备。

![](img/b1fecb218cf748a5ba1c9d89e9e1c2ec.png)

蛇的图像由 Laura Barry 在 Upslash 上提供

祝你玩得开心，在外面注意安全！

E