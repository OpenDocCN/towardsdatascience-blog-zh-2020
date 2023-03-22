# 使用 scispaCy 进行命名实体识别(第 1 部分)

> 原文：<https://towardsdatascience.com/using-scispacy-for-named-entity-recognition-785389e7918d?source=collection_archive---------20----------------------->

## 从生物医学文献中提取数据的分步指南

![](img/22f9c63fb74a974ae135a49b56dd6f03.png)

比阿特丽斯·佩雷斯·莫亚在 [Unsplash](https://unsplash.com/backgrounds/art/paper?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

2019 年，艾伦人工智能研究所(AI2)开发了 scispaCy，这是一个完整的开源 Python 空间管道，旨在使用自然语言处理(NLP)分析生物医学和科学文本。scispaCy 是一个强大的工具，特别是用于命名实体识别(NER)，或识别关键字(称为实体)并将其分类。我将向您介绍在 NER 使用 scispaCy 的基本知识，您将很快成为 NLP 大师。

# 议程

1.  设置环境
2.  安装熊猫
3.  安装 scispaCy
4.  选择型号
5.  导入包
6.  输入数据
7.  选择数据
8.  实现命名实体识别
9.  更大的数据

# 设置环境

第一步是选择一个工作环境。我用过 Google Colab，但 Jupyter 笔记本或简单地从终端工作也很好。如果您确实在终端上工作，请确保创建一个虚拟的工作环境。如果你是在 Google Colab 工作，没必要这么做。关于创建虚拟环境的简单易懂的说明可以在[这里](https://uoa-eresearch.github.io/eresearch-cookbook/recipe/2014/11/26/python-virtual-env/)找到。

*因为我使用的是 Google Colab，所以使用的语法可能与其他环境下使用的略有不同。*

这个项目的完整代码可以在这里找到。

自从写了这篇博客之后，这个项目的代码已经被更新了。总体布局保持不变。

# 安装熊猫

Pandas 是一个用于数据操作的 Python 库。这将有助于导入和表示我们将要分析的数据(在下一节中讨论)。如果你使用 Google Colab，pandas 是预装的，所以你可以忽略这一步。否则，使用 Conda 或 PyPI(无论您喜欢哪个)安装 pandas。您可以在此查看安装过程[的所有步骤。](https://pandas.pydata.org/pandas-docs/stable/getting_started/install.html)

# 安装 scispaCy

安装 scispaCy 非常简单。它的安装就像其他 Python 包一样。

```
!pip install -U spacy
!pip install scispacy
```

# 选择一个预先训练好的科学模型

安装 scispaCy 之后，接下来需要安装他们的一个预制模型。科学模型有两种风格:核心和 NER。基于存储的词汇数量，核心模型有三种大小(小、中、大),它们识别实体但不对它们进行分类。另一方面，NER 模型对实体进行识别和分类。有 4 种不同的 NER 模型建立在不同的实体类别上。您可能需要尝试不同的模型，以找到最适合您需求的模型。型号和规格的完整列表可在[这里](https://allenai.github.io/scispacy/)找到。一旦你选择了一个模型，使用模型 URL 安装它。

```
!pip install [https://s3-us-west-2.amazonaws.com/ai2-s2-scispacy/releases/v0.2.4/en_core_sci_sm-0.2.4.tar.gz](https://s3-us-west-2.amazonaws.com/ai2-s2-scispacy/releases/v0.2.4/en_core_sci_sm-0.2.4.tar.gz)
```

*安装“en_core_sci_sm”模型的例子*

# 导入您的包

一旦您安装了所有的软件包，并且创建了一个虚拟环境，只需导入您刚刚下载的软件包。

```
import scispacy                                                            import spacy                                                                import en_core_sci_sm                                                       from spacy import displacy                                                 import pandas as pd
```

您可能会注意到我们还导入了一个额外的包“displacy”。Displacy 不需要执行任何 NER 动作，但它是一个可视化工具，可以帮助我们看到正在发生的事情。

# 导入数据

对于这个例子，我们使用了来自 CORD-19 的元数据，这是一个关于新冠肺炎研究论文的开放数据库。元数据，以及文章的完整收藏，可以在[这里](https://www.semanticscholar.org/cord19/download)找到。

元数据文件有点挑剔，所以如果该文件不符合您的要求，只需将内容复制到一个新文件中。这应该可以解决您在使用该文件时遇到的任何问题。

为了导入数据，我们使用 pandas *read_csv()* 函数。

```
df = pd.read_csv(“content/sample_data/metadata.csv”)
```

该函数读入文件路径并将数据存储为 DataFrame，这是 Pandas 的主要数据结构。关于 pandas 如何存储和操作数据的更多信息，你可以在这里查看文档[。](https://pandas.pydata.org/pandas-docs/stable/index.html)

*如果您使用的是 Colab，可以将文件拖到“文件”部分，然后右键单击并选择“复制路径”来轻松访问您想要的文件的路径。*

# 选择相关数据

元数据提供了大量关于 CORD-19 中 60，000 多篇论文的有用信息，包括作者、参考号等。然而，出于我们的目的，我们关心的数据是摘要。每篇论文的完整摘要列在“摘要”一栏下。所以，我们的下一步是选择这段文字。我们将使用 DataFrame *loc* 函数来实现。该函数接收数据帧中单元格的位置，并返回该单元格中的数据。要访问特定的摘要，只需键入所需的特定行和列标题，并存储为字符串变量。

```
text = meta_df.loc[0, “abstract”]
```

这将找到位于表的第一行的摘要(记住，在 Python 中，索引从 0 开始)。然后，您可以打印新创建的字符串，以验证您是否拥有所需的数据。它应该是这样的:

![](img/93bf89ed910a14a0b942f7e8db362e2c.png)

# 命名实体识别

现在你有了你的文本，你可以进入有趣的部分。多亏了 scispaCy，实体提取相对容易。我们将使用核心模型和 NER 模型来强调两者之间的差异。

**核心型号:**

```
nlp = en_core_sci_sm.load()
doc = nlp(text)
displacy_image = displacy,render(doc, jupyter = True, style = ‘ent’)
```

您的输出应该如下所示:

![](img/c8fbf97b502bff17fcb13d65c9d3d846.png)

**NER 模式:**

```
nlp = en_ner_jnlpba_md.load()
doc = nlp(text)
displacy_image = displacy,render(doc, jupyter = True, style = ‘ent’)
```

*在这里，我们使用了一个模型来识别 DNA、细胞类型、RNA、蛋白质、细胞系类型的实体*

输出应该如下所示:

![](img/05c37c1d68339e6f307bb19266ed35c5.png)

# 扩展到更大的数据

就这样，你成功地在一个样本文本上使用了 NER！但是，这只是 CORD-19 元数据中超过 60，000 个摘要中的一个。如果我们想在 100 篇摘要中使用 NER 会怎样？1000 呢？他们所有人呢？这个过程，虽然需要更多的技巧，但本质上和以前是一样的。

当你阅读这一部分的时候，我强烈推荐跟随 [Google Colab 项目](https://github.com/akash-kaul/Using-scispaCy-for-Named-Entity-Recognition)来充分理解我们正在做的事情。

所以，第一步和之前一样。我们需要读入数据。

```
meta_df = pd. read_csv(“/content/metadata.csv”)
```

*再次使用元数据文件的特定路径*

接下来，我们加载我们的模型。对于这个例子，我们将使用所有 4 个 NER 模型，所以如果您还没有安装和导入它们，您将需要安装和导入它们。只需按照前面描述的说明，然后加载它们。

```
nlp_cr = en_ner_craft_md.load()
nlp_bc = en_ner_bc5cdr_md.load()
nlp_bi = en_ner_bionlp13cg_md.load()
nlp_jn = en_ner_jnlpba_md.load()
```

接下来，我们想要创建一个空表来存储实体和值对。该表将有 3 列:“*doi“*”、“*实体”*和“*类”*。该表将被规范化，以便每个实体/类对的 doi 将在“*doi”*列中，即使该 doi 已经被列出。这样做是为了在任何列中都没有空格，如果您以后想将数据用于其他程序，这将很有帮助。要创建这个表，您需要创建一个包含 3 个列表的字典。

```
table= {“doi”:[], “Entity”:[], “Class”:[]}
```

现在事情变得有点复杂了。我们将从遍历整个文件开始。为此，我们使用 pandas *index* 函数，它给出了值的范围(行数)。然后我们使用 *itterrows()* 函数迭代整个文件。所以，你的循环应该是这样的。

```
meta_df.index
for index, row in meta_df.iterrows():
```

对于循环的每次迭代，我们想要提取相关的抽象和 doi。我们也想忽略任何空洞的摘要。在 Python 中，空单元格存储为 nan，其类型为 float。

```
text = meta_df.loc[index, “abstract”]
doi = meta_df.loc[index, “doi”]
if type(text) == float:
    continue
```

*continue 语句结束循环的当前迭代，并继续下一次迭代。这允许我们跳过任何带有空白摘要的行。*

现在我们有了文本，我们需要使用之前加载的一个模型来提取实体。如果你在 Google Colab 上查看代码，这个步骤被分成了几个独立的方法，但是也可以不使用任何 helper 方法来编写。但是，请注意，最好是一次运行一个模型，尤其是在 Colab 中，读写文件需要相当长的时间。使用 4 个 NER 模型之一的聚合代码应该如下所示:

```
doc = nlp_bc(text)
ent_bc = {}
for x in doc.ents:
    ent_bc[x.text] = x.label_for key in ent_bc:
    table[“doi”].append(doi)
    table[“Entity”].append(key)
    table[“Class”].append(ent_bc[key])
```

*记住所有这些代码都在最初的 for 循环中*

这段代码可能看起来很吓人，但实际上，它与我们已经练习过的非常相似。我们通过一个模型传递文本，但是这次不是使用 displacy 显示结果，而是将它存储在一个字典中。然后，我们遍历字典，将结果以及我们正在查看的文章的相应 doi 添加到我们的表中。我们继续这样做，循环遍历文件中的每一行。一旦表被填充，并且 for 循环已经结束，最后一步是创建一个输出 CSV 文件。感谢熊猫，这很容易。

```
trans_df = pd.DataFrame(table)
trans_df.to_csv (“Entity.csv”, index=False)
```

您可以为输出文件选取任何标题，只要它遵循所示的格式。一旦你的代码运行完毕，输出文件就会出现在 Google Colab 的“文件”部分。然后你可以下载文件，欣赏你所有的辛勤工作。

![](img/ade56233c755afa69be733ebaf1fb14c.png)

使用 bc5cdr 模型的 CSV 输出示例(在 Excel 中打开)

或者，您可以从 [Gofile](https://gofile.io/d/mMh5zc) 下载所有 4 个输出 CSV 文件。

# 进一步探索

有许多方法可以使用我们刚刚收集的数据。我们可以采取的一个方向是使用图形数据库在语义上链接发布。查看这个项目的[第二部分](/linking-documents-in-a-semantic-graph-732ab511a01e)，我将带你了解如何创建一个图表并对我们的数据建模。

# 结论

如果你关注了这篇文章，那么恭喜你！你刚刚在科学文献的科学空间和 NER 的世界里迈出了第一步；然而，还有更多的东西需要探索。仅在 scispaCy 中，就有缩写检测、依存解析、句子检测等方法。我希望您喜欢学习一些关于 scispaCy 的知识，以及如何将它用于生物医学 NLP，并且我希望您继续尝试、探索和学习。

# **资源:**

1.  [https://uoa-e search . github . io/e search-cookbook/recipe/2014/11/26/python-virtual-env/](https://uoa-eresearch.github.io/eresearch-cookbook/recipe/2014/11/26/python-virtual-env/)
2.  [https://github . com/akash-Kaul/Using-scis pacy-for-Named-Entity-Recognition](https://github.com/akash-kaul/Using-scispaCy-for-Named-Entity-Recognition)
3.  [https://pandas . pydata . org/pandas-docs/stable/getting _ started/install . html](https://pandas.pydata.org/pandas-docs/stable/getting_started/install.html)
4.  https://allenai.github.io/scispacy/
5.  【https://www.semanticscholar.org/cord19/download 
6.  [https://pandas.pydata.org/pandas-docs/stable/index.html](https://pandas.pydata.org/pandas-docs/stable/index.html)