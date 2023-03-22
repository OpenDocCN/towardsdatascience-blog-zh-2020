# 数据科学家 Pytest

> 原文：<https://towardsdatascience.com/pytest-for-data-scientists-2990319e55e6?source=collection_archive---------6----------------------->

## 适用于您的数据科学项目的 Pytest 综合指南

![](img/eb1ef6e79cdedf92311854f85ab5dc88.png)

[拍摄的照片](https://www.pexels.com/@startup-stock-photos?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)来自 [Pexels](https://www.pexels.com/photo/man-wearing-black-and-white-stripe-shirt-looking-at-white-printer-papers-on-the-wall-212286/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

# 动机

应用不同的 python 代码来处理笔记本中的数据是很有趣的，但是为了使代码可复制，您需要将它们放入函数和类中。当你把你的代码放到脚本中时，代码可能会因为一些函数而中断。即使你的代码没有崩溃，你怎么知道你的函数是否会像你期望的那样工作？

例如，我们用 [TextBlob](https://textblob.readthedocs.io/en/dev/) 创建一个函数来提取文本的情感，这是一个用于处理文本数据的 Python 库。我们希望确保它像我们预期的那样工作:如果测试结果是肯定的，函数将返回一个大于 0 的值，如果文本是否定的，函数将返回一个小于 0 的值。

要确定函数是否每次都会返回正确的值，最好的方法是将函数应用于不同的示例，看看它是否会产生我们想要的结果。这就是测试变得重要的时候。

一般来说，您应该对您的数据科学项目使用测试，因为它允许您:

*   确保代码按预期运行
*   检测边缘情况
*   放心地用改进的代码替换现有的代码，而不用担心破坏整个管道
*   你的队友可以通过查看你的测试来理解你的功能

有许多 Python 工具可用于测试，但最简单的工具是 Pytest。

# Pytest 入门

Pytest 是一个框架，它使得用 Python 写小测试变得很容易。我喜欢 pytest，因为它帮助我用最少的代码编写测试。如果您不熟悉测试，pytest 是一个很好的入门工具。

要安装 pytest，请运行

```
pip install -U pytest
```

为了测试上面显示的函数，我们可以简单地创建一个以`test_`开头的函数，后面是我们想要测试的函数的名称，即`extract_sentiment`

在测试函数中，我们将函数`extract_sentiment`应用于一个示例文本:“我认为今天将是伟大的一天”。我们使用`assert sentiment > 0`来确保情绪是积极的。

就是这样！现在我们准备运行测试。

如果我们脚本的名字是`sentiment.py`，我们可以运行

```
pytest sentiment.py
```

Pytest 将遍历我们的脚本，并运行以`test`开头的函数。上面测试的输出将如下所示

```
========================================= test session starts ==========================================
platform linux -- Python 3.8.3, pytest-5.4.2, py-1.8.1, pluggy-0.13.1collected 1 itemprocess.py .                                                                                     [100%]========================================== 1 passed in 0.68s ===========================================
```

相当酷！我们不需要指定测试哪个函数。只要函数名以`test,`开头，pytest 就会检测并执行那个函数！为了运行 pytest，我们甚至不需要导入 pytest

如果测试失败，pytest 会产生什么输出？

```
>>> pytest sentiment.py========================================= test session starts ==========================================
platform linux -- Python 3.8.3, pytest-5.4.2, py-1.8.1, pluggy-0.13.1
collected 1 itemprocess.py F                                                                                     [100%]=============================================== FAILURES ===============================================
________________________________________ test_extract_sentiment ________________________________________def test_extract_sentiment():

        text = "I think today will be a great day"

        sentiment = extract_sentiment(text)

>       assert sentiment < 0
E       assert 0.8 < 0process.py:17: AssertionError
======================================= short test summary info ========================================
FAILED process.py::test_extract_sentiment - assert 0.8 < 0
========================================== 1 failed in 0.84s ===========================================
```

从输出可以看出，测试失败是因为函数的情绪是 0.8，而且不小于 0！我们不仅能够知道我们的功能是否像预期的那样工作，还能知道它为什么不工作。从这种洞察力中，我们知道在哪里修复我们的函数，使其按照我们想要的那样工作。

# 同一功能的多次测试

我们可能想用其他例子来测试我们的功能。新测试函数的名称是什么？

如果我们想在带有负面情绪的文本上测试我们的函数，第二个函数的名字可以是类似于`test_extract_sentiment_2`或`test_extract_sentiment_negative`的名字。任何函数名只要以`test`开头都可以

```
>>> pytest sentiment.py========================================= test session starts ==========================================
platform linux -- Python 3.8.3, pytest-5.4.2, py-1.8.1, pluggy-0.13.1
collected 2 itemsprocess.py .F                                                                                    [100%]=============================================== FAILURES ===============================================
___________________________________ test_extract_sentiment_negative ____________________________________def test_extract_sentiment_negative():

        text = "I do not think this will turn out well"

        sentiment = extract_sentiment(text)

>       assert sentiment < 0
E       assert 0.0 < 0process.py:25: AssertionError
======================================= short test summary info ========================================
FAILED process.py::test_extract_sentiment_negative - assert 0.0 < 0
===================================== 1 failed, 1 passed in 0.80s ======================================
```

从输出中，我们知道一个测试通过了，一个测试失败了，以及测试失败的原因。我们期望句子‘我不认为这会变好’是否定的，但是结果是 0。

这有助于我们理解这个函数可能不是 100%准确的；因此，在使用该函数提取文本情感时，我们应该谨慎。

# 参数化:组合测试

上面的两个测试函数用于测试相同的函数。有没有什么方法可以把两个例子合并成一个测试函数？这就是参数化派上用场的时候

## 用样本列表进行参数化

使用`pytest.mark.parametrize()`，我们可以通过在参数中提供一个例子列表来执行不同例子的测试。

在上面的代码中，我们将变量`sample`分配给一个样本列表，然后将该变量添加到测试函数的参数中。现在每个例子将被一次测试一次。

```
========================== test session starts ===========================
platform linux -- Python 3.8.3, pytest-5.4.2, py-1.8.1, pluggy-0.13.1
collected 2 itemssentiment.py .F                                                    [100%]================================ FAILURES ================================
_____ test_extract_sentiment[I do not think this will turn out well] _____sample = 'I do not think this will turn out well'[@pytest](http://twitter.com/pytest).mark.parametrize('sample', testdata)
    def test_extract_sentiment(sample):

        sentiment = extract_sentiment(sample)

>       assert sentiment > 0
E       assert 0.0 > 0sentiment.py:19: AssertionError
======================== short test summary info =========================
FAILED sentiment.py::test_extract_sentiment[I do not think this will turn out well]
====================== 1 failed, 1 passed in 0.80s ===================
```

使用`parametrize()`，我们能够在一次函数中测试 2 个不同的例子！

## 用一系列例子和预期输出来参数化

如果我们期望**不同的例子**有**不同的输出**会怎样？Pytest 还允许我们向测试函数的参数中添加示例和预期输出！

例如，下面的函数检查文本是否包含特定的单词。

如果文本包含单词，它将返回`True`。

如果单词是“duck ”,文本是“在这个文本中有一只鸭子”,我们期望这个句子返回`True.`

如果单词是“duck ”,文本是“There nothing here ”,我们希望句子返回`False.`

我们将使用`parametrize()`,但是使用元组列表。

我们函数的参数结构是`parametrize(‘sample, expected_out’, testdata)`和`testdata=[(<sample1>, <output1>), (<sample2>, <output2>)`

```
>>> pytest process.py========================================= test session starts ==========================================
platform linux -- Python 3.8.3, pytest-5.4.2, py-1.8.1, pluggy-0.13.1
plugins: hydra-core-1.0.0, Faker-4.1.1
collected 2 itemsprocess.py ..                                                                                    [100%]========================================== 2 passed in 0.04s ===========================================
```

厉害！我们两个测试都通过了！

# 一次测试一个功能

当脚本中测试函数的数量增加时，您可能希望一次测试一个函数，而不是多个函数。使用`pytest file.py::function_name`可以轻松做到这一点

例如，如果您只想运行`test_text_contain_word`，运行

```
pytest process.py::test_text_contain_word
```

pytest 将只执行我们指定的一个测试！

# 夹具:使用相同的数据测试不同的功能

如果我们想用相同的数据测试不同的功能呢？例如，我们想测试句子“今天我找到了一只鸭子，我很高兴”是否包含单词“鸭子”**和**它的情绪是积极的。我们想对相同的数据应用两个函数:“今天我发现了一只鸭子，我很高兴”。这时候`fixture`就派上用场了。

`pytest`夹具是向不同测试功能提供数据的一种方式

在上面的例子中，我们用函数`example_data.`上面的装饰符`@pytest.fixture`创建了一个示例数据，这将把`example_data`变成一个值为“今天我找到了一只鸭子，我很高兴”的变量

现在，我们可以使用`example_data`作为任何测试的参数！

# 构建您的项目

最后但同样重要的是，当我们的代码变大时，我们可能希望将数据科学函数和测试函数放在两个不同的文件夹中。这将使我们更容易找到每个功能的位置。

用`test_<name>.py`或`<name>_test.py`来命名我们的测试函数。Pytest 将搜索名称以“test”结尾或开头的文件，并在该文件中执行名称以“test”开头的函数。多方便啊！

有不同的方法来组织你的文件。您可以将我们的数据科学文件和测试文件组织在同一个目录中，或者组织在两个不同的目录中，一个用于源代码，一个用于测试

方法 1:

```
test_structure_example/
├── process.py
└── test_process.py
```

方法二:

```
test_structure_example/
├── src
│   └── process.py
└── tests
    └── test_process.py
```

由于您很可能有多个数据科学函数文件和多个测试函数文件，您可能希望将它们放在不同的目录中，就像方法 2 一样。

这是两个文件的样子

只需添加`sys.path.append(os.path.abspath(os.path.join(os.path.dirname(__file__), os.path.pardir)))`即可从父目录导入函数。

在根目录(`test_structure_example/`下，运行`pytest tests/test_process.py`或者运行`test_structure_example/tests`目录下的`pytest test_process.py`。

```
========================== test session starts ===========================
platform linux -- Python 3.8.3, pytest-5.4.2, py-1.8.1, pluggy-0.13.1
collected 1 itemtests/test_process.py .                                            [100%]=========================== 1 passed in 0.69s ============================
```

相当酷！

# 结论

恭喜你！您刚刚了解了 pytest。我希望这篇文章能够让您很好地了解为什么测试很重要，以及如何使用 pytest 将测试融入到您的数据科学项目中。通过测试，您不仅能够知道您的功能是否如预期的那样工作，而且能够自信地用不同的工具或不同的代码结构切换现有的代码。

本文的源代码可以在这里找到:

 [## khuyentran 1401/数据科学

### 有用的数据科学主题以及代码和文章的集合— khuyentran1401/Data-science

github.com](https://github.com/khuyentran1401/Data-science/tree/master/data_science_tools/pytest) 

我喜欢写一些基本的数据科学概念，并尝试不同的算法和数据科学工具。你可以在 [LinkedIn](https://www.linkedin.com/in/khuyen-tran-1ab926151/) 和 [Twitter](https://twitter.com/KhuyenTran16) 上联系我。

如果你想查看我写的所有文章的代码，请点击这里。在 Medium 上关注我，了解我的最新数据科学文章，例如:

[](/sentiment-analysis-of-linkedin-messages-3bb152307f84) [## 使用 Python 和情感分析探索和可视化您的 LinkedIn 网络

### 希望优化您的 LinkedIn 个人资料？为什么不让数据为你服务呢？

towardsdatascience.com](/sentiment-analysis-of-linkedin-messages-3bb152307f84) [](/i-scraped-more-than-1k-top-machine-learning-github-profiles-and-this-is-what-i-found-1ab4fb0c0474) [## 我收集了超过 1k 的顶级机器学习 Github 配置文件，这就是我的发现

### 从 Github 上的顶级机器学习档案中获得见解

towardsdatascience.com](/i-scraped-more-than-1k-top-machine-learning-github-profiles-and-this-is-what-i-found-1ab4fb0c0474) [](https://medium.com/swlh/get-the-most-out-of-your-array-with-these-four-numpy-methods-2fc4a6b04736) [## 使用这四种 Numpy 方法充分利用您的阵列

### 如何垂直拆分 Numpy 数组或查找特定范围内的元素。

medium.com](https://medium.com/swlh/get-the-most-out-of-your-array-with-these-four-numpy-methods-2fc4a6b04736) [](/how-to-fine-tune-your-machine-learning-models-with-ease-8ca62d1217b1) [## 如何有效地微调你的机器学习模型

### 发现为您的 ML 模型寻找最佳参数非常耗时？用这三招

towardsdatascience.com](/how-to-fine-tune-your-machine-learning-models-with-ease-8ca62d1217b1) [](/how-to-create-reusable-command-line-f9a2bb356bc9) [## 如何创建可重用的命令行

### 你能把你的多个有用的命令行打包成一个文件以便快速执行吗？

towardsdatascience.com](/how-to-create-reusable-command-line-f9a2bb356bc9)