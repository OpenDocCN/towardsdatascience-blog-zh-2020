# GraphQL 最佳实践

> 原文：<https://towardsdatascience.com/graphql-best-practices-3fda586538c4?source=collection_archive---------9----------------------->

## 在使用 GraphQL 6 个月之后，我分享了我对创建 graph QL 服务器的良好实践的想法

![](img/7bbaac5b7419cafe22a7f367323c6589.png)

[威森·王](https://unsplash.com/@wesson?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

# 首先

GraphQL 是一种 API 查询语言，也是一种用现有数据完成这些查询的运行时语言。它为 API 中的数据提供了一个完整的、可理解的描述，并且让客户能够准确地要求他们所需要的，仅此而已。

它是由脸书开发的，作为他们移动应用的内部解决方案，后来向社区开源。

# 最佳实践

如果你看一下官方的 [GraphQL 最佳实践](https://graphql.org/learn/best-practices/)页面，你会注意到它只分享了一些更常见的最佳实践的简要描述，在我看来，这只是一些指南，并没有支持实现的细节。我将尝试分享一些使用 GraphQL 的后端应用程序的具体实现。

这绝对不是一个完整的指南；这只是关于如何避免经常出现的最大陷阱的最佳实践和说明的列表。

# 将精益数据模型用于简单的查询和变异

当您开始创建模式时，设计数据模型可能会很棘手。有多种方法可以做到这一点，尽管大多数实现都可以正常工作，但是只有当您尝试扩展实现时，问题才会浮出水面。

一般来说，看看已经在使用 GraphQL 的平台总是好的。Github API 是理解输入和输出对象如何建模以及查询和变异如何暴露的好地方。

作为一般的经验法则。让你的[突变](http://spec.graphql.org/June2018/#sec-Root-Operation-Types)尽可能小。保持输入精简，并精心命名。

突变`addComment`简单、简洁、清晰。它接受一个输入`AddCommentInput`并返回一个`Comment`。我们正在应用一个非空的修饰符(！)以确保输入有效负载不能为空。这里需要注意的一点是**我们发送回创建/更新的对象**。这允许客户端更新状态，以便用户知道是否有更新。

相当整洁！

如有任何问题或讨论，请随时联系我。我在推特上有空。

# 使用嵌套对象减少网络调用

上例中的类型`Comment`是一个只有一个字段的简单类型。假设我们想将`userId`作为`Comment`的一部分发送回去。一种可能的方法是这样的。

虽然这看起来是一种快速简单的方法，但是有一个小问题。对于前端来说，`userId`本身并不是很有用，它最终必须调用`userId`来获取用户。这确实很不方便，也没有最大限度地发挥 GraphQL 的威力。在 GraphQL 中，嵌套输出类型要好得多。这样，我们可以用一个请求调用所有的东西，还可以用[数据加载器](https://github.com/facebook/dataloader)执行缓存和批处理。

方法将是在`Comment`中发送`User`。它看起来会像这样。

然后查询看起来像这样。

# 启用正确的错误处理

这是几乎每个人在开始时都会忘记的事情之一，然后当应用程序变得庞大时，覆盖错误情况就变得非常麻烦。当涉及到 GraphQl 错误处理时可能会非常棘手，因为响应总是有一个 HTTP 状态`200 OK`。如果请求失败，JSON 有效负载响应将包含一个名为`errors`的根字段，其中包含失败的详细信息。

如果没有适当的错误处理，响应将如下所示:

这一点帮助都没有——回复并没有告诉你到底哪里出了问题。这就是为什么大多数应用程序失败了，却没有真正给用户提供正确的消息或后备选项。处理错误本身就是一个很大的话题，我为此写了一篇单独的博客，里面有一个完整的运行示例。可以在这里找到。

[](https://medium.com/better-programming/error-handling-with-graphql-spring-boot-and-kotlin-ed55f9da4221) [## 用 GraphQL、Spring Boot 和科特林处理错误

### 使用 Spring Boot 和 Kotlin 对 GraphQL 错误和异常建模

medium.com](https://medium.com/better-programming/error-handling-with-graphql-spring-boot-and-kotlin-ed55f9da4221) 

简而言之，实现后的输出如下所示:

# 使用接口和联合进行抽象

[接口](https://graphql.github.io/graphql-spec/June2018/#sec-Interfaces)作为父对象，其他对象可以继承。

> 接口类型扩展用于表示从某个原始接口扩展而来的接口。例如，这可能用于表示多种类型的常见本地数据，或者由 GraphQL 服务(它本身是另一个 GraphQL 服务的扩展)来表示。

并且[联合](https://graphql.github.io/graphql-spec/June2018/#sec-Unions)是一种表示许多对象的对象类型。

> GraphQL 联合表示可能是 GraphQL 对象类型列表中的一个对象，但是在这些类型之间不提供有保证的字段。它们与接口的不同之处还在于，对象类型声明它们实现什么接口，但不知道包含它们的联合是什么。

考虑下面的例子。这个概要文件是一个由`User`和`Company`实现的接口。很少有字段是必填的。可以根据不同的类型和要求添加附加字段。

当您的查询需要返回两种类型时，可以使用类似的联合。您的`search`查询可以返回`User`或`Company`。

关于更详细的解释和实现，你可以阅读这个。

[](https://medium.com/better-programming/using-graphql-with-spring-boot-interfaces-and-unions-a76f62d62867) [## 通过 Spring Boot 使用 GraphQL:接口和联合

### GraphQL 的接口和联合提供了一种在查询中处理多种字段类型的好方法

medium.com](https://medium.com/better-programming/using-graphql-with-spring-boot-interfaces-and-unions-a76f62d62867) 

# 使用片段重用已定义的类型

片段是 GraphQL 中可重用的东西。你可以假设像编程语言中的函数一样的片段。

使用扩展运算符(`...`)消耗片段。这样会减少很多冗余代码。

感谢阅读。我希望它能帮助您实现一个健壮的 GraphQL 服务器。这些方法屡试不爽。如果你有除此之外的建议，请随时回复。如有任何问题或讨论，请随时联系我。我在推特上有空。

如果你想知道 GraphQL 是否适合你，你可能会有兴趣阅读这篇文章。

[](https://levelup.gitconnected.com/6-months-of-using-graphql-faa0fb68b4af) [## 使用 GraphQL 个月

### 在后端使用 GraphQL 做了 6 个月的项目后，我衡量了这项技术是否适合…

levelup.gitconnected.com](https://levelup.gitconnected.com/6-months-of-using-graphql-faa0fb68b4af) 

参考资料:

*   [GraphQL 规格](http://spec.graphql.org/June2018/#sec-Unions)
*   [GraphQL 最佳实践](https://graphql.org/learn/best-practices/)
*   [GraphQL 接口和联合](https://medium.com/better-programming/using-graphql-with-spring-boot-interfaces-and-unions-a76f62d62867)
*   [GraphQL 错误处理](https://medium.com/better-programming/error-handling-with-graphql-spring-boot-and-kotlin-ed55f9da4221)