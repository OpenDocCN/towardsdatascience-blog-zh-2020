# 在 Azure 函数中使用 Azure Cosmos DB

> 原文：<https://towardsdatascience.com/working-with-azure-cosmos-db-in-your-azure-functions-cc4f0f98a44d?source=collection_archive---------1----------------------->

## 如何使用 Azure 函数和 Cosmos DB 实现无服务器数据库计算

![](img/71e9bef142cf31ea7021844508b94e36.png)

开发使用 [Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction) 作为数据存储的 [Azure 函数](https://azure.microsoft.com/en-us/blog/introducing-azure-functions/)很容易实现。我们可以使用 CosmosDB 触发器调用我们的 Azure 函数，我们可以使用输入和输出绑定从我们的 Cosmos DB 集合中获取数据，或者我们可以使用 Azure 函数支持依赖注入到我们的 Cosmos DB 客户端的单例实例中，以供我们的函数使用。

在本文中，我将讨论在 Azure Function 应用程序中使用 Cosmos DB 的各种不同方式，每种方式的优缺点，并展示一些过程中的代码。出于本文的目的，我将用 C#编写代码示例。

要跟随代码，请在 GitHub 上查看[这个回购](https://github.com/willvelida/global-integration-camp-2020)。

[**使用 CosmosDB 触发器**](https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-cosmosdb-v2?tabs=csharp#trigger)

我们可以使用 CosmosDB 触发器来创建事件驱动的函数，这些函数使用 [Cosmos DB Change Feed](https://docs.microsoft.com/en-us/azure/cosmos-db/change-feed) 功能来监视我们的 Cosmos DB 数据库中容器的变化。

![](img/606a33146ecb5753bcd520e81667be6e.png)

来源:[https://docs . Microsoft . com/en-us/azure/cosmos-db/server less-computing-database # why-choose-azure-functions-integration-for-server less-computing](https://docs.microsoft.com/en-us/azure/cosmos-db/serverless-computing-database#why-choose-azure-functions-integration-for-serverless-computing)

使用 Cosmos DB 触发器，我们可以使用变更提要对容器中的项目执行操作，并将这些操作的结果存储在另一个容器中。我们还可以使用触发器来实现数据的归档策略。例如，我们可以在 Cosmos DB 中存储热数据，使用 change feed 来监听进入该容器的新项目并将其保存在 Azure 存储中，然后删除 Cosmos DB 中的热数据。

让我们来看一个例子:

这是在函数中使用 CosmosDB 触发器的最简单的例子。我们正在监听数据库中的一个集合，通过为它指定一个集合并在我们的绑定中不存在该集合时创建它来管理变更提要的租约。

然后，当变更提要检测到受监控容器中的变更时，我们只记录该容器中修改了多少个文档，以及第一个文档的 id 是什么。默认情况下，更改提要每 5 秒钟轮询一次指定的容器，但是如果需要更改该值，可以在绑定中指定不同的值。

如果你想进一步了解你可以在 Azure Functions 中使用变更提要，我在这里写了一个更详细的帖子。

[**使用输入绑定**](https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-cosmosdb-v2?tabs=csharp#input)

我们可以将 Azure 函数绑定到 Cosmos DB 中的容器，这将允许函数在执行时从容器中读取数据。

![](img/9b95919fcaa273f82a66d0661f44d91c.png)

来源:[https://docs . Microsoft . com/en-us/azure/cosmos-db/server less-computing-database # why-choose-azure-functions-integration-for-server less-computing](https://docs.microsoft.com/en-us/azure/cosmos-db/serverless-computing-database#why-choose-azure-functions-integration-for-serverless-computing)

假设我们有一个使用 HTTP 触发器调用我们的函数的无服务器 API，我们可以使用输入绑定获得一个存储在 Azure Cosmos DB 中的文档。

我们可以这样做:

我们可以指定希望在 Cosmos DB 输入绑定中运行的 SQL 查询，并将结果作为产品类的 IEnumerable 返回。如果我们没有得到结果，我们可以返回 Not Found，否则，在 16 行代码中，我们可以将产品项返回给我们的客户端。

[**使用输出绑定**](https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-cosmosdb-v2?tabs=csharp#output)

通过输出绑定，我们可以连接到 Cosmos 中的容器并将数据写入这些容器。

![](img/2df4c17c7eb5c75e8373c2db7df28fdc.png)

来源:[https://docs . Microsoft . com/en-us/azure/cosmos-db/server less-computing-database # why-choose-azure-functions-integration-for-server less-computing](https://docs.microsoft.com/en-us/azure/cosmos-db/serverless-computing-database#why-choose-azure-functions-integration-for-serverless-computing)

以我们的无服务器 API 为例，这次我们可以使用一个 Cosmos DB 输出绑定来连接到我们的容器，以便将我们的新产品项目插入其中。

这里有一个例子:

我们在输出绑定中所做的就是指定我们想要将项目插入哪个容器，它位于哪个数据库中，以及到我们的 Cosmos DB 帐户的连接字符串。

如您所见，使用 Cosmos DB 绑定构建一些简单的应用程序非常容易，但这是有代价的。

**绑定的缺点**

在我写这篇文章的时候， **Azure 函数触发器和绑定只支持 SQL API** 。如果我们想在我们的 Azure 函数中使用另一个 Cosmos DB API，我们必须创建一个静态客户端，或者像我们接下来要做的那样，为我们正在使用的 API 创建一个客户端的单独实例。

默认情况下，Cosmos DB 绑定使用版本 2 的。NET SDK。这意味着如果您想在函数中使用新的 [V3 特性，比如事务批处理](https://github.com/Azure/azure-cosmos-dotnet-v3)，那么您必须使用依赖注入来创建一个兼容 V3 的客户端。

**使用依赖注入**

Azure Functions 的 v2 提供了对依赖注入的支持。它建立在。NET 核心依赖注入特性。函数中的依赖注入提供了三种服务生存期:

*   **瞬态**:这些是为服务的每个请求创建的。
*   **作用域**:这个作用域匹配一个函数的执行生存期。每次执行都会创建一次。
*   **Singleton** :它们的生命周期与主机的生命周期相匹配，并在该实例上的所有函数执行中被重用。因此，我们可以在函数应用程序中为每个函数提供共享服务，而不是让客户端连接到每个绑定。这是我们将用于我们的 **CosmosClient** 的范围。

为了在我们的函数中实现依赖注入，我们需要在 Startup.cs 文件中注册我们的 CosmosClient 服务:

这里，我添加了一个 **CosmosClientBuilder** 的单例实例，通过我的 CosmosDB 帐户的连接设置。然后，我使用 fluent API 添加属性，比如设置应用程序区域和支持批量执行。

现在我有了我的 Singleton 实例，我可以将它注入到我们的函数中:

在这里，我使用构造函数注入来使我的依赖项对我的函数可用。注意这里我不再为我的函数使用静态类，因为这是构造函数注入所需要的。

如果你想了解依赖注入在 Azure 函数中是如何工作的，这份[文档](https://docs.microsoft.com/en-us/azure/azure-functions/functions-dotnet-dependency-injection)有一个非常简单的指南。

**结论**

我希望读完这篇文章后，您对在函数中使用绑定和依赖注入来处理 Cosmos DB 的含义有一个很好的了解。

我的偏好是使用单例实例而不是绑定，但是就像我前面说的，这取决于您的用例。

我希望你喜欢这篇文章，并发现它很有用。一如既往，如果你有任何问题，请在评论中告诉我！