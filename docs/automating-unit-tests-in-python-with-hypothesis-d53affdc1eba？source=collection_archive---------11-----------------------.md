# 使用假设在 Python 中自动化单元测试

> 原文：<https://towardsdatascience.com/automating-unit-tests-in-python-with-hypothesis-d53affdc1eba?source=collection_archive---------11----------------------->

![](img/b3de7a8c44e535aed22ecb5384de1de6.png)

凯文·Ku 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## 单元测试是产生高质量代码的关键。下面是如何实现自动化。

单元测试是开发高质量代码的关键。有许多可用的库和服务[，你可以用它们来完善你的 Python 代码](https://realpython.com/python-testing/)的测试。然而，“传统的”单元测试是时间密集型的，不太可能涵盖你的代码应该能够处理的所有情况。在这篇文章中，我将向你展示如何使用基于属性的测试和[假设](https://hypothesis.readthedocs.io/en/latest/)来自动测试你的 Python 代码。我还讨论了使用基于属性的测试框架的一些优点。

# 基于属性的自动化测试

单元测试包括测试代码的单个组件。典型的单元测试获取输入数据，通过一段代码运行它，并根据一些预定义的预期结果检查结果。

[假设](https://hypothesis.readthedocs.io/en/latest/)做了一些不同的事情。它是一个基于属性的(或:“生成的”)测试框架。基于属性的测试包括定义对代码的一般期望，而不是具体的例子。例如，如果您有一些代码来计算一些交易的总增值税，您可以定义一组假设的数字及其相应的增值税金额(100 美元交易→ $xx.xx 税),并对其进行测试。然而，如果你知道增值税是，比方说，20%，一个基于财产的测试将验证总增值税始终是总额的 20%。

假设是建立在这些原则之上的。它*根据某个规范*生成任意输入数据，并随后对该数据进行测试。更重要的是，当 Hypothesis 发现一个导致断言失败的例子时，它会试图简化这个例子，并找到最小的失败案例——这个过程称为“[收缩](https://hypothesis.readthedocs.io/en/latest/data.html#shrinking)”。假设实际上会试图“破坏”你的代码。因此，您的测试将用相同数量的代码覆盖更大的领域空间。而且，你一定会发现你甚至没有想到的边缘情况。

# 假设入门

让我们看看假设在实践中是如何工作的。假设有三个关键部分:你正在测试的代码，定义你的测试数据的策略，和一个使用策略测试你的代码的函数。

让我们假设我们有一段简单的(无意义的)Python 代码，它将浮点值转换为整数:

这段代码有一个明确的属性:结果应该总是整数类型。

## **策略**

为了测试这段代码，我们将首先定义一个“策略”。[策略定义了假设为测试](https://hypothesis.readthedocs.io/en/latest/data.html#)生成的数据，以及如何“简化”示例。在我们的代码中，我们只定义数据的参数；简化(或:“缩小”)是假说的内在要求。

我们将从生成 0.0 到 10.0(包括 0.0 和 10.0)之间的浮点值的策略开始。我们在一个名为`data_strategies.py`的单独文件中对此进行了定义。为此使用一个[数据类](https://realpython.com/python-data-classes/)可能看起来有点过头，但是当您处理带有一堆不同参数的更复杂的代码时，这是很有用的。

使用数据类和假设来定义数据策略

很多时间可以用来定义策略，事实上也应该如此。使用假设进行基于属性的测试的全部要点在于，您定义了生成数据的参数，以便您随后允许您的自动化测试发挥其魔力。你在这上面花的时间越多，你的测试必然越好(想想:“高投入；高奖励”)。

## 将代码和策略结合在一起:用假设运行测试

在我们定义了我们的策略之后，我们添加了一小段代码来将假设生成的示例传递给我们的函数，并断言我们想要测试的代码的所需结果(“属性”)。下面的代码从我们在上面的`data_strategies.py`文件中定义的`generated_data` dataclass 对象中提取一个浮点值，通过我们的`convert_to_integer`函数传递该值，最后断言期望的属性保持不变。

使用假设定义您的测试模块

## 配置假设:有用的设置

在我们运行上面开发的测试模块之前，让我们回顾一下一些配置，我们可以使用这些配置来为我们的用例定制假设。假设自带[一堆设定](https://hypothesis.readthedocs.io/en/latest/settings.html)。这些设置可以使用`settings()` [装饰器](https://www.datacamp.com/community/tutorials/decorators-python)传递到您的测试函数，或者通过在一个概要文件中注册设置，并使用装饰器传递概要文件(参见下面的示例代码)。一些有用的设置包括:

*   `max_examples`:控制测试结束前需要多少个通过的例子。如果您对一段新代码通过评审所需的测试量有一些内部指导方针，这是很有用的。一般来说:你的代码越复杂，你想要运行的例子就越多(假设的作者[注意到他们在测试 SymPy](https://hypothesis.readthedocs.io/en/latest/settings.html) 的时候，在几百万个例子之后找到了新的 bugs
*   `deadline`:指定单个例子允许用多长时间。如果您有非常复杂的代码，其中一个示例运行的时间可能会超过默认时间，您会希望增加这个值；
*   `suppress_health_check`:允许您指定忽略哪些“健康检查”。当您处理大型数据集(`HealthCheck.data_too_large`)或需要很长时间才能生成的数据(`HealthCheck.too_slow`)时非常有用。

让我们在测试模块中使用这些设置。有了这些简单的代码行，我们现在可以继续前进，在我们的函数中抛出 1000 个例子来验证它是否按预期工作。您可以从终端(`python -m pytest test_my_function.py`)运行测试，或者如果您使用像 [Pycharm](http://Pycharm) 这样的 IDE，通过 s [为您的代码](https://www.jetbrains.com/help/pycharm/pytest.html#run-pytest-test)指定适当的 pytest 配置。

使用假设设置对 Python 代码进行基于属性的测试

# 升级你的游戏:使用复合策略

到目前为止，我使用的例子都很简单。Hypothesis 可以使用复合策略处理复杂得多的测试用例，顾名思义，复合策略允许您组合策略来生成测试用例。所以，让我们开始游戏，在更复杂的环境中使用假设。

假设您已经开发了一段计算数组百分比值的代码。有很多 Python 库可以为您完成这项工作，但是假设您对百分位特别感兴趣，并且只是想要一个自己的实现。在这个例子中，我们的目标是根据现有的实现对我们的解决方案进行基准测试。考虑到这一点，我们可以定义这段代码的两个简单属性来进行测试:

1.  我们用来计算百分位数的数值数组的顺序对其结果并不重要；
2.  该函数的输出需要与用另一个普遍接受的库计算的值一致(注意，将它定义为“属性”有点不典型——在本文的最后一节会有更多的介绍)。

让我们从百分位数函数开始。这里，我们实现了一个使用中点插值的版本。该函数接受整数值或浮点值(`arr`)的数组，对其进行排序，根据指定的百分位(`perc`)确定下限和上限，最后取两个值的中点。

简单的百分位数函数

现在，让我们继续我们的策略，也就是，我们为测试生成的数据的定义。这里，我们定义了一个名为`generate_scenario`的函数，它基于随机选择的分布(`dist`)产生一个随机长度的浮点值数组(`n`)。我们还生成我们想要计算的百分比值(`perc`)。我们将值和百分位数作为字典返回，因此我们可以轻松地访问测试所需的值。注意`@st.composite`装饰器的使用，它将"[一个返回一个例子的函数转换成一个返回产生这样的例子的策略的函数](https://hypothesis.readthedocs.io/en/latest/data.html#composite-strategies)。

在假设中定义复合策略

最后，我们可以在测试模块中使用我们的复合策略。在这个模块中，我们指定我们的策略设置(`SETTINGS`)，并指定一个运行我们自己代码的函数。`test_calc_percentile`函数测试反转数组顺序不会影响我们的 percentile 函数的输出，并将结果与 NumPy 实现进行比较。使用我们在这里设置的配置文件，我们运行 10，000 个示例，在我五年前的笔记本电脑上大约需要 30 秒来完成。

使用复合策略的测试模块示例

# 在你开始测试之前:关于你什么时候应该和不应该使用假设的说明

在你开始开发你自己的单元测试之前，有一些事情你需要知道。基于属性的测试在特定的环境下非常有效。当一段代码的属性被很好且容易地定义时，它是一个强大的框架，也就是说，“我的代码的 x 部分需要产生一个具有属性 y 的结果”。当定义“assert”语句的条件与您试图测试的代码一样复杂时，您将最终重新实现您现有的代码。

在某些情况下，拥有两个并行的实现可能是合适的，例如，当您创建了一段新的代码，或者用一个新的框架重新实现了一些东西，您需要针对现有的实现进行基准测试，如上面的示例所示。然而，在大多数情况下，事实并非如此:您将浪费时间和资源，最终不得不维护两段做完全相同事情的代码。

即使在自动化测试时，在“传统的”单元测试和基于属性的测试之间走一条中间路线也是明智的。您将希望避免让您的测试成为太多的“黑盒”,并确保您覆盖了明显的情况，尤其是在开发的早期阶段。总是建议至少创建几个很好理解的场景。并且，将你自己的“手动”测试用例与自动化测试结合起来，将会对提高你的测试覆盖率大有帮助，并且可以极大地改进你的代码。

感谢阅读！你喜欢用哪些 Python 工具来提高代码质量？请在评论中留下你的建议！

***支持我的工作:*** *如果你喜欢这篇文章并愿意支持我的工作，请考虑通过我的推荐页面* *成为付费媒介会员。如果你通过我的推荐页面* *注册* [*，订阅的价格是一样的，但是我会收取你每月的部分会员费。*](https://medium.com/@ndgoet/membership)

**如果你喜欢这篇文章，这里还有一些你可能喜欢的文章:**

[](https://medium.com/python-in-plain-english/how-to-improve-your-python-code-style-with-pre-commit-hooks-e7fe3fd43bfa) [## 如何用预提交钩子改进 Python 代码风格

### 实施代码风格是简化开发过程的关键，下面是如何实现自动化。

medium.com](https://medium.com/python-in-plain-english/how-to-improve-your-python-code-style-with-pre-commit-hooks-e7fe3fd43bfa) [](/automating-version-tags-and-changelogs-for-your-python-projects-6c46b68c7139) [## 为您的 Python 项目自动化版本标签和变更日志

### 基于项目的提交历史，使用 commitizen 实现版本标签和变更日志自动化的实践指南

towardsdatascience.com](/automating-version-tags-and-changelogs-for-your-python-projects-6c46b68c7139) [](/simplify-your-python-code-automating-code-complexity-analysis-with-wily-5c1e90c9a485) [## 简化您的 Python 代码:用 Wily 自动化代码复杂性分析

### 以下是如何让评估代码复杂性成为 Python 开发例程的一部分

towardsdatascience.com](/simplify-your-python-code-automating-code-complexity-analysis-with-wily-5c1e90c9a485) 

*请仔细阅读* [*本免责声明*](https://medium.com/@ndgoet/disclaimer-5ad928afc841) *中的任何内容后再依托* [*我的 Medium.com 文章*](/@ndgoet) *。*