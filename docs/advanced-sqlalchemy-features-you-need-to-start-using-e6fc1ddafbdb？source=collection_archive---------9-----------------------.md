# 您需要开始使用的高级 SQLAlchemy 功能

> 原文：<https://towardsdatascience.com/advanced-sqlalchemy-features-you-need-to-start-using-e6fc1ddafbdb?source=collection_archive---------9----------------------->

## 通过 SQLAlchemy 及其混合属性、嵌套查询、表元数据、方言等等，在 Python 中使用 SQL 变得很容易！

如果您是 Python 开发人员，并且使用 SQL 数据库，那么 SQLAlchemy 很可能是您熟悉的库。这是一个强大而灵活的工具包，用于在 Python 中使用 SQL，具有很多特性。像 ORM 和基本查询这样的一些特性是众所周知的，但是有相当多的特性你可能不知道，并且绝对应该加以利用。因此，让我们看看如何利用混合属性、嵌套查询、表元数据、方言等等！

![](img/45a7fe192b528a25d77fef0331756db1.png)

西蒙·维亚尼在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的原始照片

# 列属性

让我们从简单的开始。我认为您可能希望基于其他列创建映射属性是很常见的——本质上是创建计算列。最简单的例子是字符串串联:

这很好，但是当我们使用 SQL 表达式来创建这样的属性时，它更有用:

对于上面的例子，我们增加了一些代码。我们创建了与`User`有*多对一*关系的`CreditCard`类。这个用户——在第一个例子的列和属性之上——还有一个名为`has_credit_card`的列属性，它是通过检查带有用户 ID 的信用卡是否存在来计算的。

使用该特性时，有一点需要注意，即在提交会话之前不会填充列属性，这在处理新创建的记录时可能是意外的:

# 混合属性

继续上一个技巧，让我也向您展示一下*混合属性*。它们类似于列属性，因为它们产生*计算属性*。然而，混合属性从实例级的 Python 表达式和类级的 SQL 表达式中产生值。有点困惑？好吧，让我们看一个例子:

为了展示`hybrid_property`的能力，我们实现了`User`和`Order`之间的简单关系，其中每个用户都有一个包含`.state`的订单列表——在这种情况下，要么是*待定*要么是*完成*。现在，如果我们想知道用户是否有任何*未决的*订单，我们需要考虑两种情况——如果我们正在处理已经加载到 Python 对象中的行，那么我们可以只使用 Python 表达式并产生 Python 值(`has_pending_orders(self)`)。另一方面，如果我们直接从数据库中查询这些信息，我们不能使用 Python 表达式，因为数据库引擎不能理解它。因此，对于这种情况(`has_pending_orders(cls)`)，我们编写 SQL 表达式，可以针对数据库运行。

顺便提一下——如果 Python 和 SQL 评估的表达式相同，那么可以省略用`.expression`修饰的第二个函数，SQLAlchemy 将在两种情况下使用第一个函数。

# 混合蛋白

我最喜欢的特性之一是混合类。Mixins 不仅仅是 SQLAlchemy 特有的东西，但是它们在与 ORM 模型结合时特别有用。您经常会遇到这样的情况，您有多个类(模型)需要相同的属性或相同的`classmethod`。下面的`User`模型就是这样一个例子:

在这个例子中，我们有 2 个 *Mixin* 类，它们是`User`模型继承的。首先- `MixinAsDict`提供了方法`as_dict(self)`，可以用来获得模型的`dict`表示。另一个`MixinGetByUsername`既提供了`username`列，也提供了通过用户名查询用户的静态方法。

将这些函数定义为 *Mixins* 允许我们重用它们，并将它们添加到其他模型中，而无需到处复制粘贴相同的代码。

如果你不想自己写所有的*mixin*，那么你可以看看[https://github.com/absent1706/sqlalchemy-mixins](https://github.com/absent1706/sqlalchemy-mixins)，它是一个常见 SQLAlchemy*mixin*的集合。

# 使用元数据

有时，您可能需要访问表的列名，检查表上的约束，或者检查列是否可为空。所有这些都可以用`MetaData()`类来完成:

这里重要的部分是代码片段底部的`print`语句。它们中的每一个都演示了一些可以通过元数据对象访问的东西。这包括表名、列名、列类型、外键和主键以及其他约束。

# 配置表格

一些数据库表可能需要更多的初始设置。例如，您可能希望包含一些检查约束、索引或指定不同的模式:

所有这些都可以使用`__table_args__` class 属性进行配置。这里，我们为 ID 列和外键约束设置了 2 个*检查约束*，1 个索引。我们还打开了自动表扩展，这意味着如果我们在这个表创建之后向它添加列，那么它将被自动添加。最后，我们还指定这个表属于哪个模式。

# 使用习惯方言

每个数据库引擎都有一些您可能想要利用的自定义功能。对我来说——作为一个 PostgreSQL 用户——我想使用 PostgreSQL 的一些自定义列类型。那么如何将它们与 SQLAlchemy 结合使用呢？

上面的代码显示了一个具有 PostgreSQL `UUID`、`INT4RANGE`、`NUMRANGE`、`JSON`和`ARRAY`列的`Example`表。所有这些和[更多的](https://docs.sqlalchemy.org/en/13/dialects/postgresql.html)都可以从`sqlalchemy.dialects.postgresql`导入。

创建包含这些类型值的行是不言自明的。但是在查询它们时，您需要使用方言和类型特定的比较器，如上面 PostgreSQL `ARRAY`类型和`.contains`比较器所示。

对于像`JSON`这样的其他类型，你也许可以将它们作为文本进行比较(使用`.astext`)。

为了让您在创建这些查询时更轻松，我建议在创建引擎时设置`echo=True`，这将使 SQLAchemy 将所有 SQL 查询打印到控制台中，以便您可以检查您的代码是否实际生成了正确的查询。

所有的方言、它们的类型和比较者都记录在[https://docs.sqlalchemy.org/en/13/dialects/](https://docs.sqlalchemy.org/en/13/dialects/)中。

# 使用 PostgreSQL 进行全文搜索

当谈到 PostgreSQL 特性时。用`tsqeury`和`tsvector`进行全文搜索呢？我们也可以用 SQLAchemy 做到这一点:

我们再次为全文搜索创建了 *Mixin* 类，因为这是很多模型都可以使用的。这个 *Mixin* 有单一的静态方法，它采用搜索字符串和列在(`field`)中进行搜索。为了进行实际的搜索，我们使用了`func.to_tsvector`，我们将语言和对表列的引用传递给它。在这一点上，我们链接对`.match`函数的调用，它实际上是对 PostgreSQL 中的`to_tsquery`的调用，我们给它搜索字符串和搜索配置作为参数。

从生成的 SQL 中，我们可以看到 Python 代码确实生成了正确的 SQL 查询。

# 跟踪行的上次更新

创建`created_at`或`updated_at`列是很常见的做法。这可以通过 SQLAlchemy 非常简单地完成:

对于`updated_at`,您只需要将`onupdate`设置为`func.now()`,这将使得每次更新行时，该列将被设置为当前时间戳。对于`created_at`列，您可以省略`onupdate`参数，而使用`server_default`来设置创建行时调用的函数。

# 自引用表

在数据库中有递归/自引用关系并不罕见——无论是*经理- >员工*关系、树结构还是一些物化路径。这篇技巧展示了如何使用 SQLAlchemy 建立这种关系:

对于本例，我们使用使用`Node`记录创建的树形结构。每个节点都有一些`data`，引用它的父节点和它的子节点列表。作为一种方便的方法，我们也包括了`__str__`和`__repr__`来帮助我们更好的可视化树。

如果你对普通的*一对多*关系没有意见，那么你可以用同样的方式处理任何非自指关系。然而，为了使其适用于*双向*关系，您还需要包括如上所示的带有`remote_side=[id]`的`backref`。

# 用 Flask 绑定多个数据库

最后一个是给所有*烧瓶*用户的。如果您需要连接到多个数据库—例如，由于多个地理位置或多个数据源—那么您可以使用`SQLALCHEMY_BINDS`来指定额外的数据库绑定:

在上面的代码片段中，我们通过设置`SQLALCHEMY_DATABASE_URI`和`SQLALCHEMY_BINDS`中的替代绑定来配置默认数据库。有了这个配置，我们就可以使用上述所有数据库。接下来，我们设置一个表的`__bind_key__`来引用其中一个绑定，这样每当我们与这个特定的表交互时，SQLAlchemy 就会知道要连接到哪个数据库。

但是，如果您需要连接到具有相同表/模式的多个数据库，您可以使用多个引擎和会话——每个数据库一个，并根据需要在它们之间切换，如下所示:

# 结论

希望这里显示的这些提示和技巧中至少有一些对您有用，并且在您下次需要使用 SQLAlchemy 时会让您的生活稍微轻松一点。这篇文章绝对不是你可以用 SQLAlchemy 做的所有酷事情的详尽列表，你可以通过滚动 [SQLAlchemy API 参考](https://docs.sqlalchemy.org/en/13/core/index.html)找到一堆有用的东西。

*本文最初发布于*[*martinheinz . dev*](https://martinheinz.dev/blog/28?utm_source=tds&utm_medium=referral&utm_campaign=blog_post_28)

[](/all-the-things-you-can-do-with-github-api-and-python-f01790fca131) [## 你可以用 GitHub API 和 Python 做的所有事情

### GitHub REST API 允许您管理问题、分支、回购、提交等等，所以让我们看看您如何使用…

towardsdatascience.com](/all-the-things-you-can-do-with-github-api-and-python-f01790fca131) [](/ultimate-guide-to-python-debugging-854dea731e1b) [## Python 调试终极指南

### 让我们探索使用 Python 日志记录、回溯、装饰器等等进行调试的艺术…

towardsdatascience.com](/ultimate-guide-to-python-debugging-854dea731e1b) [](/automating-every-aspect-of-your-python-project-6517336af9da) [## 自动化 Python 项目的各个方面

### 每个 Python 项目都可以从使用 Makefile、优化的 Docker 映像、配置良好的 CI/CD、代码…

towardsdatascience.com](/automating-every-aspect-of-your-python-project-6517336af9da)