# 你能教计算机像斯蒂芬·金那样写作吗？

> 原文：<https://towardsdatascience.com/can-you-teach-a-computer-to-write-like-stephen-king-3bdec697723?source=collection_archive---------36----------------------->

## 在 NLP 的深度学习的帮助下

![](img/5bb5a69124d18c9fedf95076199a745a.png)

照片由[思想目录](https://unsplash.com/@thoughtcatalog?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

你是否曾经想写一个引人入胜的故事，但缺乏合适的技巧？也许让计算机为你做艰苦的工作。

在这篇文章中，我将在深度学习的帮助下教你如何教计算机像斯蒂芬·金一样写故事。数据集由斯蒂芬·金的五篇短篇小说组成。

*   *草莓春天*
*   *大夜班*
*   *房间里的女人*
*   *我是门道*
*   *战场*

我将使用专门用于自然语言处理的递归神经网络，并将 Python 作为编程语言。

# 1.导入所有必需的库

# 2.导入数据

文本文件包含上面提到的五个故事。另外，请注意，我降低了所有单词的大小写，以减少词汇量。

# 3.创造词汇

这里的第一行将创建所有独特字符的词汇表。由于神经网络只理解数值，我们必须将每个字符映射到一个整数。第二行和第三行创建一个字典，将每个字符映射到一个整数。

# 4.创建序列和目标

这里我们将创建 100 个字符长度的序列，预测下一个 101 个字符。

# 5.编码序列和目标

如前所述，神经网络只能理解数值，我们必须对序列和目标进行编码。

# 6.构建模型

有人可能认为 RNN 的实现非常复杂，但是在 Keras 的帮助下，实现起来非常简单。该模型基本上由以下 4 层组成:

**1。嵌入:**嵌入层将高维空间的向量变换到低维空间。在某种程度上，意思相似的词也有相似的价值。

**2。LSTM:** 长短期记忆(LSTM)是一种递归神经网络。它们能够学习序列预测问题中的顺序依赖性。

**3。辍学:**辍学有助于防止过度适应。

**4。密集:**这是发生预测的输出层。Softmax 函数用于输出每个字符的概率值。

# 7.训练网络

使用拟合函数，我们可以训练我们的网络。我把批量大小定为 64，训练了 40 个纪元。

# 8.预测

函数`sample()` 将从输出概率数组中抽取一个索引。参数`temperature` 定义了函数在创建文本时的自由度。

种子句子用于预测下一个字符。然后，我们可以简单地用预测的字符更新种子句子，并修剪第一个字符以保持序列长度不变。

# 9.结果

生成的段落:

```
Input: people clustered in small groups that had a tendency to break up and re-form with surprising speed.Output: people clustered in small groups that had a tendency to break up and re-form with surprising speed. looking out to be an undergrad named donald morris work and i passed all the look and looked at him. he was supposed to stay wiping across the water,
but they were working and shittered back of its an
appla night. the corpse in the grinder, expecting the crap outs.the counted in the second. jagged sallow sound over the way back on the way — at least aloud.'i suspect there,’ hall said softly. he was supposed to be our life counting fingers.
```

生成的文本看起来可读性很强。虽然它没有意义，有些句子语法不正确，有些单词拼写错误，但它使用了正确的标点符号，如每句话后的句号和引用一个人时的引号。

# 结论

我承认生成的文本与斯蒂芬·金的写法相去甚远。但是看到最近在深度学习和自然语言处理方面的进展，我满怀希望。也许几年后，电脑会成为比人更好的作家。

此外，如果你想了解更多关于循环神经网络的内容，安德烈·卡帕西有一个很棒的[博客](http://karpathy.github.io/2015/05/21/rnn-effectiveness/)。

我们的模型的性能可以通过使用更大的训练数据和修补超参数来提高。

请随意使用代码。完整的项目可以在 [Github](https://github.com/himanshuagarwal190/Stephen-King-Text-Generation) 中找到。

# 谢谢大家！

感谢您的阅读，祝您有美好的一天。:)