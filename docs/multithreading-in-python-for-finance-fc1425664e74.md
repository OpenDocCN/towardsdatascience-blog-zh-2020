# 面向金融的 Python 多线程技术

> 原文：<https://towardsdatascience.com/multithreading-in-python-for-finance-fc1425664e74?source=collection_archive---------14----------------------->

## 并行计算及其在金融中的应用快速指南

![](img/addb3253d215f8513474e3ad7c40bcee.png)

[照片致谢](https://www.freepik.com/free-photos-vectors/design)

# 并行计算

想象一下，一台计算机试图画四面墙，每面墙要花 30 分钟。如果计算机按顺序绘制所有四面墙，按照正常执行代码的方式，绘制所有四面墙需要(30 米)*(4 面墙)或 120 米。下面的代码用 Python 演示了这一点。

如果我们能同时粉刷所有四面墙会怎么样？这种想法被称为并行计算，在各自的线程上同时异步运行代码。如果计算机是多线程的(在它自己的线程上同时运行每个 paint_wall 函数),这个过程只需要 30 分钟就可以完成四面墙的绘制。下面的代码说明了用 Python 多线程处理这个过程。

这在金融领域对我们有什么帮助？算法交易系统负责在不同的时间框架内执行各种资产类别的交易。交易系统可以高频交易订单簿不平衡策略，同时分析指数中的头寸大小以对冲该策略。在这个系统中，两个过程同时发生，但需要不同时间范围的数据。在本文中，我的目标是给出一个简单的面向对象的方法来支持财务分析系统中的多线程。

# 线程化抽象类

考虑一个抽象类交易系统。抽象类维护了作为子类创建的所有交易系统的基本要求。也就是说，它包含一个驻留在定时无限循环中的函数。这是一个抽象交易系统的简化版本，用来说明多线程，但是你可以期待一个更实用的抽象类，包括交易数量、利润/损失和未结/已结/已结订单。

在这种情况下，抽象类接受两个参数，一个 ticker 和一个 timeframe。TradingSystem 类的每个实例将为自己分配 ticker 和时间框架，并创建一个线程无限循环，在时间框架期间休眠。这实际上允许我们管理每个子类中的*什么*和*什么时候*。

# 抽象类实现

下面是一个 NVIDIA 的交易系统，作为抽象 trading system 类的实现而创建。将 TradingSystem 类构造为一个超类使我们能够灵活地跨不同的时间框架开发特定的交易规则集。NVIDIA 交易系统每十秒钟从 yahoo finance 的 API 请求一个期权链(即 *what* )(即 *when* )。

# 量化发展和交易

如果你想在真实的交易环境中实现这一点，你可以看看我其他的关于用 Java 和 Python 开发算法交易系统和策略的文章。

*   [Java 算法交易系统开发](https://medium.com/swlh/algorithmic-trading-system-development-1a5a200af260)
*   [用 Python 免费搭建一个 AI 炒股机器人](https://medium.com/swlh/build-an-ai-stock-trading-bot-for-free-4a46bec2a18)
*   [如何建立有利可图的交易策略](https://medium.com/swlh/build-a-profitable-stock-trading-bot-6ba376cba955)