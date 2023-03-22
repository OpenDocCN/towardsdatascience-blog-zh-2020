# Python 中的朝鲜语自然语言处理

> 原文：<https://towardsdatascience.com/korean-natural-language-processing-in-python-cc3109cbbed8?source=collection_archive---------42----------------------->

## 利用“KoNLPy”模块进行词法分析和词性标注

![](img/c4f49f66aaa770584f7e98e59733beee.png)

照片由[鹤原](https://unsplash.com/@tsuyuri104?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/korean?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

通过阅读这篇文章，您将学会使用 Python 为朝鲜语执行简单的自然语言处理任务，如词法分析和词性标注。我们将使用一个名为`KoNLPy`的 Python 模块。基于[官方文档](https://konlpy.org/en/latest/)，KoNLPy(读作*为* `*ko en el PIE*` *):*

> “…是一个用于朝鲜语自然语言处理(NLP)的 Python 包。”

该模块基于以下理念:

*   保持简单。
*   让它变得简单。对人类来说。
*   网络民主让每个人都可以贡献自己的想法和建议，

本教程有 3 个部分:

1.  设置
2.  履行
3.  结论

让我们继续下一部分，开始安装必要的模块。

# 1.设置

## Windows 操作系统

安装有点复杂，因为您需要确保您已经安装了 Java 版本 1.7。进入以下[链接](https://www.oracle.com/java/technologies/javase-downloads.html)为您的电脑下载必要的 JDK。

从 Python 访问 Java 类库需要`JPype`。您需要根据您的操作系统和 Python 版本安装正确的`JPype`。用下面的代码检查它

```
python --version
```

前往下面的[链接](https://www.lfd.uci.edu/~gohlke/pythonlibs/#jpype)并下载相应的文件。我将使用下面的文件，因为我在 64 位 Windows 机器上运行 Python3.7.4。

```
JPype1-0.7.2-cp37-cp37m-win_amd64.whl
```

将文件放在一个目录中，然后打开一个终端。强烈建议您事先设置一个虚拟环境。将目录更改为 wheel 文件所在的根文件夹。运行以下命令来安装它。根据您下载的内容相应地修改名称。

```
pip install JPype1-0.5.7-cp27-none-win_amd64.whl
```

通过`pip install`命令继续安装`koNLPy`。

```
pip install konlpy
```

## 其他操作系统

如果你正在使用其他种类的操作系统，请查看官方的[文档](https://konlpy.org/en/latest/install/)，因为我还没有亲自测试过。它支持 Linux 和 MacOS，并且安装更加简单。

让我们继续下一节，开始写一些 Python 代码。

# 2.履行

这个模块为我们提供了几个不同的类来执行词法分析和词性标注。它们中的每一个在最终结果和性能上都是不同的。查看下面的[链接](https://konlpy.org/en/latest/morph/#comparison-between-pos-tagging-classes)，了解更多关于时间和性能分析的信息。

*   `[Kkma](https://konlpy.org/en/latest/api/konlpy.tag/#konlpy.tag._kkma.Kkma)`
*   `[Komoran](https://konlpy.org/en/latest/api/konlpy.tag/#konlpy.tag._komoran.Komoran)`
*   `[Hannanum](https://konlpy.org/en/latest/api/konlpy.tag/#konlpy.tag._hannanum.Hannanum)`
*   `[Okt](https://konlpy.org/en/latest/api/konlpy.tag/#konlpy.tag._okt.Okt)`
*   `[Mecab](https://konlpy.org/en/latest/api/konlpy.tag/#konlpy.tag._mecab.Mecab)`—Windows 不支持

在本教程中，我将使用 Okt 来保持事情简单和容易。让我们从在 Python 文件上添加以下导入声明开始。

```
from konlpy.tag import Okt
from konlpy.utils import pprint
```

我将使用以下文本作为输入。在韩语里是`I am eating an apple`的意思。希望我没弄错。

```
text = '나는 사과를 먹고있다'
```

将类初始化为对象

```
okt = Okt()
```

之后，我们可以调用其中的函数，并使用`pprint` utils 类将结果打印出来。

## 名词

让我们尝试使用`nouns`函数从句子中提取名词

```
pprint(okt.nouns(text))
```

您应该会得到以下结果。

```
['나', '사과']
```

*   `나` —我
*   `사과` —苹果

## 改变

如果你想对句子进行分词，你可以使用`morphs`功能。它接受三个输入参数。

*   `phrase` —输入文本
*   `norm` —一个布尔值表示是否规范化句子
*   `stem` —布尔值表示是否对句子进行词干处理。词干化是将句子中的单词缩减成基本形式或词根形式的过程。

它可以被称为如下:

```
pprint(okt.morphs(text, norm=True, stem=True))
```

您应该在控制台上看到以下输出。

```
['나', '는', '사과', '를', '먹다']
```

你可以注意到`먹고있다`(吃)这个词正在被简化为`먹다`(吃)。

*   `나` —我
*   `는` —粒子
*   `사과` —苹果公司
*   `를` —粒子
*   `먹다` —吃

## 词性标签

`pos`如果您在标记化过程后寻找词性标签，此功能非常有用。与前面的函数不同，这个函数中的参数根据您使用的类而有所不同。有些类有`flatten`参数，如果设置为 False，它会保留 eojeols。Okt 类有以下输入参数。

*   `phrase` —输入文本
*   `norm` —一个布尔值表示是否规范化句子
*   `stem` —布尔值表示是否对句子进行词干处理。词干化是将句子中的单词缩减成基本形式或词根形式的过程。
*   `join` —一个布尔值表示是否返回由`/`符号分隔的变形和标签的联合集合。

让我们用下面的代码来测试一下

```
pprint(okt.pos(text, norm=True, stem=True))
```

控制台将显示以下结果。

```
[('나', 'Noun'), ('는', 'Josa'), ('사과', 'Noun'), ('를', 'Josa'), ('먹다', 'Verb')]
```

修改前一行并将`join`参数设置为`True`

```
pprint(okt.pos(text,norm=True, stem=True, join=True))
```

结果现在被捆绑在一起，作为一个由`/`符号分隔的元素。

```
['나/Noun', '는/Josa', '사과/Noun', '를/Josa', '먹다/Verb']
```

# 3.结论

让我们回顾一下今天所学的内容。

我们开始安装必要的模块和库。如果您是 Windows 用户，设置会稍微复杂一些。

接下来，我们深入探讨了`koNLPy`模块提供的功能。它可以用来从输入的句子中提取名词。此外，你还可以把它标记成单个的单词。您可以对它进行微调，只获得单词的基本形式作为输出。

此外，我们还测试了`pos`函数，该函数允许我们识别朝鲜语的词性标签。

感谢你阅读这篇文章。希望在下一篇文章中再见到你！

# 参考

1.  [KoNLP 的 Github](https://github.com/konlpy/konlpy)
2.  [康力普的文档](http://konlpy.org/en/latest/)