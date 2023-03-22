# 4 个基本正则表达式操作符简化了自然语言处理

> 原文：<https://towardsdatascience.com/natural-language-processing-made-simpler-with-4-basic-regular-expression-operators-5002342cbac1?source=collection_archive---------56----------------------->

## 了解四种基本的常规操作，以清理几乎所有类型的可用数据

![](img/b346c2b7a2c350f199ac4f59bddfc806.png)

图为[苏珊尹](https://unsplash.com/@syinq?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

用机器学习和深度学习来构建自然语言处理项目总是很棒的。然而，在我们开始构建我们的模型之前，我们遇到了一个非常无聊但重要的问题。

在数据收集和探索性数据分析步骤之后，我们必须立即处理令人厌烦的预处理步骤。显然，我们可用的大多数数据并不清晰，我们必须对其进行处理以提高数据集的质量，从而从构建的模型中获得更有效的结果。

今天，我们将看看带有四个内置函数的正则表达式是如何成为最佳解决方案的。正则表达式是一个内置的标准库模块，可用于清理和预处理数据。下面的简单命令可以用来导入正则表达式模块。

```
import re
```

在开始之前，让我们先了解正则表达式模块和一些基础知识，以便更直观地理解进一步的概念。

# 正则表达式:

正则表达式是字符串，其语法允许它们处理其他形式的文本数据中的模式。re 模块用于解析文本数据，不需要额外安装。

GIF 来自 [GIPHY](https://giphy.com/gifs/letters-numbers-shurly-3o6Zt7yXxGpZSY6mgU/links)

正则表达式文档提供的常见模式列表非常庞大。我将提供清理数据所需的更简短的形式。这些是我唯一用过的。我会少用一些其他的。

因此，我发现与其他操作相比，用于匹配文本数据的一些常见模式是最有用的，如下所示:

1.  **\d:** 匹配任何 Unicode 十进制数字(即 Unicode 字符类别[Nd]中的任何字符)。这包括`[0-9]`，以及许多其他数字字符。如果使用了`[ASCII](https://docs.python.org/3/library/re.html#re.ASCII)`标志，则只有`[0-9]`匹配。
2.  **\D:** 匹配任何不是十进制数字的字符。这是`\d`的反义词。如果使用了`[ASCII](https://docs.python.org/3/library/re.html#re.ASCII)`标志，这相当于`[^0-9]`。
3.  **\s':** 匹配 Unicode 空白字符(包括`[ \t\n\r\f\v]`，以及许多其他字符，例如许多语言中排版规则要求的不间断空格)。如果使用了`[ASCII](https://docs.python.org/3/library/re.html#re.ASCII)`标志，则只有`[ \t\n\r\f\v]`匹配。
4.  **'\S':** 匹配任何不是空白字符的字符。这是`\s`的反义词。如果使用了`[ASCII](https://docs.python.org/3/library/re.html#re.ASCII)`标志，这相当于`[^ \t\n\r\f\v]`。
5.  **'\w':** 匹配 Unicode word 字符；这包括任何语言中单词的大部分字符，以及数字和下划线。如果使用了`[ASCII](https://docs.python.org/3/library/re.html#re.ASCII)`标志，则只有`[a-zA-Z0-9_]`匹配。
6.  **'\W':** 匹配任何不是单词字符的字符。这与`\w`相反。如果使用了`[ASCII](https://docs.python.org/3/library/re.html#re.ASCII)`标志，这相当于`[^a-zA-Z0-9_]`。如果使用了`[LOCALE](https://docs.python.org/3/library/re.html#re.LOCALE)`标志，则匹配当前语言环境中既不是字母数字也不是下划线的字符。
7.  **'+或*':** 执行贪婪匹配。
8.  **'**[**a-z**](https://www.pluralsight.com/guides/text-parsing)**':**匹配小写组。
9.  **'**[**A-Za-z**](https://www.pluralsight.com/guides/text-parsing)**':**匹配大小写英文字母。
10.  **'**[**0–9**](https://www.pluralsight.com/guides/text-parsing)**':**匹配从 0 到 9 的数字。

要了解更多关于这些常见图案的信息，以及如何搭配它们以发现更多图案和风格，请访问官方网站[此处](https://docs.python.org/3/library/re.html)。

# 正则表达式函数和方法:

在本节中，我们将讨论四种正则表达式方法，它们可以用来解决数据清理和数据预处理的大多数常见问题。正则表达式函数与正则表达式模式一起使用，对我们拥有的文本数据执行特定的操作。

![](img/022e9e01138cf52250b7b1875ca0e34e.png)

由[艾德·罗伯森](https://unsplash.com/@eddrobertson?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

现在，让我们通过下面的文本示例更详细地分析如何使用这个模块，以及如何准确地使用 re 模块来执行适当处理和解析文本数据所需的各种操作。我只是用一些随机的不规则句子组成了一个随机的文本样本。你可以用和我一样的句子，或者随便编一个句子，然后跟着学。

文本示例如下所示:

```
sentence = "Machine Learning is fun. Deep learning is awesome. Artificial Intelligence: is it the future?"
```

我们将用于数据预处理的函数是以下四个基本的正则表达式操作—

1.  re.findall()
2.  重新拆分()
3.  re.sub()
4.  重新搜索()

使用上述四个函数，几乎可以完成任何自然语言任务和文本数据的数据预处理。所以，事不宜迟，让我们开始分析这些功能，以及如何利用它们。

*   **re.findall()**

上面的方法返回所有匹配的列表。如果没有找到匹配，则返回一个空列表。

让我们试着找出所有以大写字母开头的单词。下面的代码块可用于以下过程—

```
capital = re.findall("[A-Z]\w+", sentence)
print(capital)
```

这应该会给我们以下输出['机器'，'学习'，'深度'，'人工'，'智能']。

如果你想知道文本数据中有多少个句号或句号，你可以使用两个命令中的任何一个

```
1\. len(re.findall("[.]", sentence))
2\. len(re.findall("\.", sentence))
```

上面的两个命令都应该给出结果 2，因为我们总共有两个周期。backlash '\ '命令用于一个分隔符，只查找句点，而不执行另一个正则表达式操作。

*   **re.split()**

这个函数可以用来相应地拆分文本，只要有匹配，就会返回一个数据列表。否则返回一个空列表。

让我们执行一个拆分操作，得到一串由句点分隔的句子。下面的命令将是这个操作。

```
re.split("\.", sentence)
```

该操作将返回以下句子列表。

```
['Machine Learning is fun',
 ' Deep learning is awesome',
 ' Artificial Intelligence: is it the future?']
```

如果你想用句点和问号来分割，那么请执行下面的命令。

```
re.split("[.?]", sentence)
```

*   **re()**

以下函数在找到匹配项时执行替换操作。如果没有找到匹配，则模式保持不变。

如果你想用解释代替所有的句号和问号，你可以使用下面的命令

```
re.sub("[.?]", '!', sentence)
```

函数中的第一个位置是要替换的项目。第二个位置是您指定用什么来替换选择的位置。最后和第三个位置是要执行替换操作的句子或文本数据。

执行上述操作后，您应该会看到下面的句子。

```
'Machine Learning is fun! Deep learning is awesome! Artificial Intelligence: is it the future!'
```

*   **re.search()**

该函数查找特定单词、标点符号或选定项目的第一个匹配项，并相应地返回操作。如果没有找到匹配，则返回 none 类型值。

如果我想找到单词“fun”的起始字符和结束字符的位置在文本中，然后我可以运行下面的命令。

```
x = re.search("fun.", sentence)
print(x.start())
print(x.end())
```

上述代码块将返回 20 和 24 的输出。这个结果告诉我们，f 的位置是 20，而。是 24。我强烈推荐您使用这个功能尝试更多的操作。

这样，我们就完成了常规行动的主要部分。继续尝试本模块，以了解与本主题相关的更复杂的细节。

![](img/8276b6db13593912be5beb01c37b3b13.png)

照片由 [Giammarco Boscaro](https://unsplash.com/@giamboscaro?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论:

在本文中，我们已经涵盖了机器学习的大多数数据集的预处理所需的大多数重要模式。我们还可以将从这篇文章中学到的知识用于网络抓取以及其他需要清理数据的活动。在 python 中使用正则表达式模块在较小程度上简化了这个枯燥的过程。

利用所有提供的模式和四个特定的函数通常足以执行几乎任何预处理操作，但是可以从正则表达式官方网站的[这里的](https://docs.python.org/3/library/re.html)查看更多这些操作。

看看我的其他一些文章，你可能也会喜欢读。

[](/5-best-python-project-ideas-with-full-code-snippets-and-useful-links-d9dc2846a0c5) [## 带有完整代码片段和有用链接的 5 个最佳 Python 项目创意！

### 为 Python 和机器学习创建一份令人敬畏的简历的 5 个最佳项目想法的代码片段和示例！

towardsdatascience.com](/5-best-python-project-ideas-with-full-code-snippets-and-useful-links-d9dc2846a0c5) [](/artificial-intelligence-is-the-key-to-crack-the-mysteries-of-the-universe-heres-why-56c208d35b62) [## 人工智能是破解宇宙奥秘的关键，下面是原因！

### 人工智能、数据科学和深度学习的工具是否先进到足以破解人类大脑的秘密

towardsdatascience.com](/artificial-intelligence-is-the-key-to-crack-the-mysteries-of-the-universe-heres-why-56c208d35b62) [](/opencv-complete-beginners-guide-to-master-the-basics-of-computer-vision-with-code-4a1cd0c687f9) [## OpenCV:用代码掌握计算机视觉基础的完全初学者指南！

### 包含代码的教程，用于掌握计算机视觉的所有重要概念，以及如何使用 OpenCV 实现它们

towardsdatascience.com](/opencv-complete-beginners-guide-to-master-the-basics-of-computer-vision-with-code-4a1cd0c687f9) [](/lost-in-a-dense-forest-intuition-on-sparsity-in-machine-learning-with-simple-code-2b44ea7b07b0) [## 迷失在密林中:用简单的代码对机器学习中稀疏性的直觉！

### 为什么 ML 需要稀疏性？理解稀疏性的核心概念。

towardsdatascience.com](/lost-in-a-dense-forest-intuition-on-sparsity-in-machine-learning-with-simple-code-2b44ea7b07b0) [](https://medium.com/illumination-curated/train-machine-learning-models-without-knowing-any-coding-for-free-on-teachable-machine-51d69657c6e5) [## 在不知道任何编码的情况下，在可教机器上免费训练机器学习模型！

### 不需要编码，不需要注册，没有额外的东西，只是你的浏览器。训练计算机识别您的…

medium.com](https://medium.com/illumination-curated/train-machine-learning-models-without-knowing-any-coding-for-free-on-teachable-machine-51d69657c6e5) 

谢谢你们坚持到最后。我希望你们都喜欢阅读这篇文章。祝大家度过美好的一天！