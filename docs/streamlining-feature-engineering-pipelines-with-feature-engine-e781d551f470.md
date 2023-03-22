# 用特征引擎简化特征工程管道

> 原文：<https://towardsdatascience.com/streamlining-feature-engineering-pipelines-with-feature-engine-e781d551f470?source=collection_archive---------25----------------------->

## [入门](https://towardsdatascience.com/tagged/getting-started)

## 找出在开发用于部署的机器学习管道时遇到的挑战，并了解开源软件如何提供帮助。

![](img/f005b601cec79c786b4ea7b85512afef.png)

简化特征工程管道—图片来自 Pixabay，无需注明出处

在许多组织中，我们创建机器学习模型来处理一组输入变量，以输出预测。例如，这些模型中的一些预测贷款偿还的可能性、申请欺诈的可能性、汽车在事故后是否应该修理或更换、客户是否会流失等等。

由组织收集和存储的原始数据几乎从不适合于训练新的机器学习模型，或者被现有模型消费来产生预测。相反，在变量可以被机器学习算法利用之前，我们执行大量的转换。变量转换的集合通常被称为特征工程。

在本文中，我们将描述在构建和部署特征工程和机器学习管道时遇到的最常见的挑战，以及如何利用开源软件和名为[特征引擎](https://feature-engine.readthedocs.io/en/latest/)的新 Python 库来帮助缓解这些挑战。

# **什么是特征工程？**

特征工程是使用数据的领域知识来创建使机器学习算法工作的特征的过程。原始数据中有几个方面需要解决。例如，数据经常丢失，或者变量值采用字符串而不是数字的形式。许多机器学习库，如 Scikit-learn，无法处理丢失的值或字符串，因此我们需要将它们转换成数值。此外，当变量显示某些特征时，如正态分布或类似的尺度，一些模型往往会工作得更好。因此，我们试图转换变量，使它们具有这些特征。

特征工程包括处理所有这些方面的数据转换程序，包括缺失数据的插补、分类变量的编码、数值变量的转换或离散化，以及在相似尺度中设置特征。此外，特征工程还涉及通过将现有特征组合成新变量，或通过从日期中提取信息，汇总交易数据，或从时间序列、文本甚至图像中导出特征来创建特征。

总之，特征工程包括数据转换和新数据创建的每个方面，以返回适合训练并被机器学习模型使用的数据集。我在别处广泛描述了不同的特征工程技术[。更多细节和代码实现可以在课程“](https://trainindata.medium.com/feature-engineering-for-machine-learning-a-comprehensive-overview-a7ad04c896f8)[机器学习的特征工程](https://www.courses.trainindata.com/p/feature-engineering-for-machine-learning)”和书籍“ [Python 特征工程食谱](https://packt.link/0ewSo)”中找到。

# **特征工程是重复且耗时的**

根据福布斯的[调查，数据科学家和机器学习工程师花费大约 **60%** 的时间清理和组织数据以供分析和机器学习，或者换句话说，用于数据转换和特征工程**。**](https://www.forbes.com/sites/gilpress/2016/03/23/data-preparation-most-time-consuming-least-enjoyable-data-science-task-survey-says/)

特征工程可能非常重复，数据科学家倾向于对各种数据源执行相同类型的转换，这些数据源将用于创建不同的机器学习模型。大多数数据源都显示出相同的挑战:缺乏信息或缺失数据，变量以字符串而不是数字的形式存在，分布对于我们打算建立的机器学习模型的性能不是最佳的，或者不同的规模。因此，我们发现自己一遍又一遍地做着相同类型的数据转换，一个模型接一个模型。如果我们每次都这样做，我们从头开始编码，整个过程会变得非常低效。

我们应该努力减少重复和重复代码，优化可靠性，以提高性能和效率，同时减少数据科学家在数据处理上花费的时间。如果数据科学家花更少的时间清理和预处理数据，他们将能够花更多的时间来创建创新的解决方案，应对更有趣的挑战，并在其他领域进行学习和发展。

# **团队工作时的特色工程**

特征工程是非常重复的，我们倾向于对几个数据源进行相同的转换。每次从头开始编码这些转换，或者将代码从一个笔记本复制粘贴到另一个笔记本是非常低效和糟糕的做法。

在这样的场景中，团队工作带来了额外的复杂性，我们可能会在研究和生产环境中以相同技术的不同代码实现而告终，每个代码实现都是由不同的团队成员开发的。这不仅效率低下，而且很难跟踪团队中做了什么。我们最终得到相同代码的多个来源或版本，几乎没有任何代码标准化、版本化和测试。

通常最好创建或使用可以在团队中共享的工具。公共工具有几个优点:首先，我们不需要在每个项目中从头开始重写我们的管道，而且，它们促进了知识共享。当所有成员都可以阅读和使用现有的图书馆时，他们可以互相学习，更快地发展自己和同事的技能。

同样值得考虑的是，随着数据科学家研究和开发机器学习模型，代码测试和单元测试经常被忽略或遗忘。在 Jupyter 笔记本中经常使用的程序编程期间，从以前的命令中获得的输入往往会一遍又一遍地重新设计和重新分配，然后用于训练机器学习模型。这个过程使得很难跟踪转换和代码依赖，这本身就可能包含错误，从而导致错误传播和调试困难。

# **功能工程的特别流程不可复制**

再现性是精确复制机器学习模型的能力，使得给定相同的原始数据作为输入，两个模型返回相同的输出。这是我们在研究环境中开发的模型和在生产环境中部署的模型之间的最终目标。

重构在研究环境中开发的特征工程管道以在生产环境中添加单元测试和集成测试是非常耗时的，这提供了引入错误或发现在模型开发期间引入的错误的新机会。更重要的是，重构代码达到相同的结果，但由不同的开发人员编写，是非常低效的。最终，我们希望在我们的研究和生产环境中使用相同的代码，以最小化部署时间并最大化可重复性。

# **面向特征工程的开源库的激增**

在过去几年中，越来越多的开源 Python 库开始支持作为机器学习管道一部分的特征工程。其中，库 [Featuretools](https://www.featuretools.com/) 支持一系列详尽的函数来处理事务数据和时间序列；库[分类编码器](https://pypi.org/project/category-encoders/)支持对分类变量编码方法的全面选择；库 [Scikit-learn](https://scikit-learn.org/stable/index.html) 和[特征引擎](https://feature-engine.readthedocs.io/en/latest/)支持广泛的转换，包括插补、分类编码、离散化和数学转换等。

# **开源项目简化了机器学习管道的开发**

使用开源软件，帮助数据科学家减少他们在功能转换上花费的时间，提高团队之间的代码共享标准，并允许使用版本化和测试良好的代码，最大限度地缩短部署时间，同时最大限度地提高可重复性。换句话说，开源允许我们在研究和开发环境中使用相同的代码，有清晰的版本，因此消除或最小化了需要重构才能投入生产的代码量。

为什么使用成熟的开源项目更有效？出于多种原因，第一个开源项目往往会被完整地文档化，所以每段代码想要达到的目的是很清楚的。第二，完善的项目已经被社区广泛采用和批准，这让我们放心，代码是高质量的，并且将在未来几年得到维护和改进。第三，开源包经过了广泛的测试，以防止引入错误并最大化可再现性。第四，包有明确的版本，所以我们可以导航到更现代的，或者以前的代码实现，以获得想要的结果。第五，开源包可以共享，促进知识的采用和传播。最后，利用开源包消除了我们手中的编码任务，极大地提高了团队绩效、可重复性和协作性。

关于我们为什么应该使用开源的广泛讨论可以在本文中找到。

# **特征引擎有助于简化我们的特征工程流程**

Feature-engine 是一个开源 Python 库，我创建它是为了简化和精简端到端功能工程管道的实现。Feature-engine 最初是为机器学习的课程[Feature Engineering](https://www.udemy.com/course/feature-engineering-for-machine-learning/?referralCode=A855148E05283015CF06)而设计的，但现在已经被社区采用，并且有越来越多的贡献者加入到代码库中。

Feature-engine 保留了 Scikit-learn 功能，使用 fit()和 transform()方法从数据中学习参数，然后转换数据。请记住，许多特征工程技术需要从数据中学习参数，如统计值或编码映射，以转换数据。Scikit-learn like 功能与 fit 和 transform 方法使特征引擎易于使用。

[特征引擎](https://feature-engine.readthedocs.io/en/latest/)包括多个转换器，用于估算缺失数据、编码分类变量、离散化或转换数值变量以及移除异常值，从而提供**最详尽的特征工程转换组合**

[特征引擎](https://feature-engine.readthedocs.io/en/latest/)转换器的一些关键特征是:I)它允许选择变量子集直接在转换器上进行转换，ii)它接收数据帧并返回数据帧，促进数据探索和模型部署，iii)它自动识别数字和分类变量，从而对正确的特征子集应用正确的预处理。

功能引擎转换器可以在 Scikit-learn 管道中组装，从而可以将整个机器学习管道存储到单个对象中，该对象可以在稍后阶段保存和检索以进行批量评分，或者放置在内存中以进行实时评分。

Feature-engine 正在积极开发中，欢迎用户的反馈和社区的贡献。关于如何使用功能引擎的更多细节可以在它的[文档](https://feature-engine.readthedocs.io/en/latest/)和本文中找到:

[](https://trainindata.medium.com/feature-engine-a-new-open-source-python-package-for-feature-engineering-29a0ab88ea7c) [## Feature-engine:一个新的用于特征工程的开源 Python 包

### Feature-engine 是一个开源 Python 库，提供了最全面的转换器来设计特性…

trainindata.medium.com](https://trainindata.medium.com/feature-engine-a-new-open-source-python-package-for-feature-engineering-29a0ab88ea7c) 

# 参考资料和进一步阅读

*   [机器学习的特征工程](https://www.courses.trainindata.com/p/feature-engineering-for-machine-learning) —在线课程
*   [Python 特性工程食谱](https://packt.link/0ewSo) —书
*   [特征引擎](https://feature-engine.readthedocs.io/en/latest/):用于特征工程的 Python 库
*   [Feature-engine:一个新的用于特性工程的开源 Python 包](https://trainindata.medium.com/feature-engine-a-new-open-source-python-package-for-feature-engineering-29a0ab88ea7c)
*   [用 Python 实现特征工程的实用代码](/practical-code-implementations-of-feature-engineering-for-machine-learning-with-python-f13b953d4bcd)
*   [机器学习的特征工程:综合概述](https://trainindata.medium.com/feature-engineering-for-machine-learning-a-comprehensive-overview-a7ad04c896f8)