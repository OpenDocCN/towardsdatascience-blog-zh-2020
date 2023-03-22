# 如何用 Python 分析冠状病毒变异

> 原文：<https://towardsdatascience.com/python-and-numpy-for-sars-cov-2-gene-mutation-analysis-805833fbb23f?source=collection_archive---------23----------------------->

## 基于 Python 和 NumPy 的新冠肺炎突变分析

![](img/f548d9757e61ea582932030dc79d6d63.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 什么是冠状病毒变异？

新型冠状病毒 **Sars Cov-2** 在中国武汉首次被发现并被测序。任何生物的基因组都会由四个核苷酸碱基( **A** 腺嘌呤- **A** 、 **G** 鸟嘌呤- **G** 、 **C** 胞嘧啶- **C、T** 腺嘌呤- **T** )以特定的顺序排列在一个名为 **DNA (D** 脱氧核糖 **N** 细胞核的分子中这四个碱基的排列顺序至关重要。生物体基因中任何偏离这一序列的现象都被称为基因突变。在新型冠状病毒的情况下，它具有单链 RNA。在酶的帮助下，RNA 可以构成 DNA 分子。下面是新型冠状病毒 Sars Cov-2 的 **M** 膜基因的基因序列。如前所述，这是一个按特定顺序排列的序列。在我们的冠状病毒突变分析中，我们将比较新型冠状病毒的两种不同基因组序列。这两个序列是从 NCBI 基因库下载的。一个[序列来自美国](https://www.ncbi.nlm.nih.gov/labs/virus/vssi/#/virus?SeqType_s=Nucleotide&VirusLineage_ss=Wuhan%20seafood%20market%20pneumonia%20virus,%20taxid:2697049)，另一个来自中国[序列](https://www.ncbi.nlm.nih.gov/labs/virus/vssi/#/virus?SeqType_s=Nucleotide&VirusLineage_ss=Wuhan%20seafood%20market%20pneumonia%20virus,%20taxid:2697049)。

![](img/c507eb4d7185cfbf79ae29654326c230.png)

作者图片

# 我们为什么要分析冠状病毒变异？

每种生物都在不断进化/经历突变，以更好地适应环境。同样，新型冠状病毒也将经历突变，以更好地适应人类。了解它变异的模式是非常重要的。理解这种模式对疫苗/药物开发工作有重大影响。例如，如果刺突蛋白正在经历突变，那么针对刺突蛋白开发的疫苗/药物可能不是在所有情况下都有效。这意味着我们将不得不重新开始寻找对抗新冠肺炎的新药/疫苗。

[](https://github.com/tonygeorge1984/Python-Sars-Cov-2-Mutation-Analysis) [## tonygeorge 1984/Python-Sars-Cov-2-突变-分析

### 这是为分析 Sars Cov-2 的各种基因的核苷酸突变而创建的报告。两种不同的…

github.com](https://github.com/tonygeorge1984/Python-Sars-Cov-2-Mutation-Analysis) 

# 冠状病毒的背景

让我们从冠状病毒的一些背景开始。命名为**SARS**T2【COV-2】的新型冠状病毒是当前新冠肺炎疫情的原因。这种病毒于 2019 年 12 月在中国武汉首次被鉴定和测序。这种病毒得名于它与之前已知的冠状病毒 **SARS COV** (这种病毒导致了 2002-2003 年的严重急性呼吸道综合征)的相似性。在写这篇文章的时候，科学家已经识别并研究了 500 多种不同的冠状病毒。在这 500 多种冠状病毒中， **7 种**冠状病毒已知会导致人类感染。SARS COV-2 是名单中最新的一个。 **SARS COV** 、**MERS COV**(2012 年引起的**M**iddle**E**ast**R**S**S**yndrome、 **HKU1** 、 **NL63** 、 **OC43** 、 **229E** 是其他已知的冠状病毒

## 近距离观察 SARS COV-2

![](img/5895b0a89fd1f8f4e88d0bdf7337f9a0.png)

来自 [NCBI](https://www.ncbi.nlm.nih.gov/books/NBK554776/figure/article-52171.image.f3/?report=objectonly) 的医学博士 Rohan Bir Singh 的照片。图像信息:冠状病毒的特征、评估和治疗(新冠肺炎)。作者——马尔科·卡斯塞拉；迈克尔·拉杰尼克；阿图罗·科莫；斯科特·c·杜勒博恩；那不勒斯的拉斐尔。医学博士 Rohan Bir Singh 提供的数字；Biorender.com 制造

让我们仔细看看 **SARS COV-2** 。SARS COV-2 对人类细胞上的 ACE2 受体具有高亲和力。他们利用 ACE2 受体在细胞内吞作用前与人类细胞对接。从图中可以很清楚地看出，该病毒具有以下结构蛋白，即**刺突**蛋白、**核壳**蛋白、**膜**蛋白和**包膜**蛋白。疫苗/药物开发工作的目标是特定的病毒蛋白质结构/酶。在决定这样一个目标之前，我们了解病毒突变是非常重要的。如果靶蛋白结构发生突变，那么该药物/疫苗将来可能无效。这就是突变分析在创造药物/疫苗的努力中发挥关键作用的地方。SARS COV-2 的基因组由 11 个基因组成。每个基因及其表达(蛋白质)如下所列。

> 基因= **ORF1ab** 。编码蛋白质 ORF1a 和 ORF1ab 的开放阅读框 1。这也产生了几种非结构蛋白。
> 
> 基因= **S.** 该基因编码 **S** pike 蛋白，该蛋白在内吞作用前与人体细胞的 ACE2 受体对接。这是一种具有 S1 和 S2 亚单位的三聚体蛋白质。
> 
> 基因= **ORF3a。**开放阅读框 3 编码 ORF3a 蛋白。
> 
> E 。这个基因编码 E 包膜蛋白。
> 
> 基因= **M.** 该基因编码为 **M** 膜蛋白。
> 
> 基因= **ORF6** 。开放阅读框 6 编码 ORF6 蛋白。
> 
> 基因= **ORF7a。**开放阅读框 7a 编码 ORF7a 蛋白。
> 
> 基因= **ORF7b** 。编码 ORF7b 蛋白的开放阅读框 7b。
> 
> 基因= **ORF8** 。开放阅读框 8 编码 ORF8 蛋白。
> 
> 基因= **N.** 该基因编码 **N** 核衣壳磷蛋白。
> 
> 基因= **ORF10。**开放阅读框 10 编码 ORF10 蛋白。

# P ython 冠状病毒变异分析

[链接到 Github 获取完整代码](https://github.com/tonygeorge1984/Python-Sars-Cov-2-Mutation-Analysis)

在我们进入 Main.py 之前，让我们看看 **dna** 类、python 字典 **numpy_image_dict** 和辅助函数，如 **read_dna_seq** 和 **gene_mod。**

## 班级 dna:

首先，我用 **__init__** 方法和重要的方法 **numpfy 创建了一个新的类 **dna** 。__init__ 方法将检查所提供的基本序列是否有效。如果基本序列带有无效字符，将会抛出一个错误。该方法通过转换 A -0、T -255、G -200、C -100 和虚拟碱基 N -75 将碱基核苷酸序列转换成 numpy 阵列。**

作者创建的要点

NCBI 网站收集了超过 4500 种不同的 SARS COV-2 病毒基因组序列。任何人都可以去 NCBI 网站下载基因组序列进行分析。或者，他们也提供 API 以编程方式访问基因库。在这个例子中，我已经下载了 2 个不同的核苷酸。第一个，是 2020 年 1 月发布的中国参考序列。另一个核苷酸序列来自美国，于 2020 年 4 月发布。通过分析这两个核苷酸，我们将在 3-4 个月内看到每个基因在不同地区的突变。

## 辅助函数 read_dna_seq:

阅读从 NCBI 下载的基因组序列。核苷酸碱基序列将有 **A** 腺嘌呤、 **G** 鸟嘌呤、 **C** 胞嘧啶和 **T** 腺嘌呤。下载的两个文件都有每个基因的碱基序列。下面的方法将读取文件，并找出元数据和每个基因的碱基序列。在这一步的最后，我们将得到名为 **genome** 的 python 字典。字典将基因名称作为关键字，值对将是该基因的碱基序列列表。

作者创建的要点

## Python 字典 **numpy_image_dict 和辅助函数 gene_mod:**

我们已经创建了一个字典，其中预填充了为每个基因创建 numpy 数组所需的所有信息。这里，基因名是键，值对是一个列表。第一个元素是一个元组，该元组具有该基因的预定义数组形状。第二个元素是要添加到序列中的虚拟元素' **N** 的数量。比如基因 **ORF1ab** ，我计算过数组维数为 **115** * **115** 。这意味着 A、T、C 和 g 必须有 13225 个碱基序列。我所考虑的序列只有 13218 个碱基序列。为了弥补这个不足，我们在两个文件的基因序列中添加了 **7** 虚拟字符' **N** '。方法 **gene_mod** 将读取每个基因的字典 numpy_image_dict，并添加数字“N ”(如果字典中有指定)。

作者创建的要点

# 让我们浏览一下 Main.py 脚本

已经完成了所有需要的类、字典和助手函数，现在让我们完成主模块并开始使用所有前面提到的方法。我们需要导入 **numpy** 和 **matplotlib** 来完成这项工作。再来导入上面提到的自定义构建的 **dna** 类、 **numpy_image_dictionary** 和 helper 函数( **read_dna_seq，gene_mod** )。导入所有需要的模块后，让我们读取从 NCBI 下载的两个文件，并对每个基因进行必要的修改，将它们转换成 numpy 数组。

作者创建的要点

![](img/0f632e58ada4de1dc75c498878efec2c.png)

作者创建的图像

为 11 个基因中的每一个创建 3 个 matplotlib 子图(来自文件 1 的基因，来自文件 2 的基因，结果突变图)。

```
f,ax = plt.subplots(nrows=11,ncols=3,figsize=(25,30))
```

从 read_dna_seq 方法返回的字典中创建所有 gene_name 的列表。

```
gene_name = list(numpy_image_dict.keys())
```

对于 gene_name 列表中的每个基因，创建一个 dna 对象并调用方法 numpfy 将其转换为 numpy 数组。这意味着对于序列中的每个基因，都将有一个 dna 对象，该对象将具有 numpy 数组属性。第二个列表中的所有基因也是如此。使用 matplotlib.pyplot 的 **pcolor** 方法将每个 numpy 数组转换为子图。

```
# Create the dna object for the USA sequence
gene_us = dna(dict_seq_1['gene=ORF10')# Call the numpfy method
numpfy_usa = gene_us.numpfy()# After numpfy do a reshape with numpy_image_dict tuple
numpfy_usa = numpfy_usa.reshape(numpy_image_dict['gene=ORF10'][0])# Create the matplotlib subplot
ax[row][col].pcolor(numpfy_usa)# Create the dna object for the CHINA sequence
gene_ch = dna(dict_seq_2['gene=ORF10')# Call the numpfy method
numpfy_china = gene_ch.numpfy()# After numpfy do a reshape with numpy_image_dict tuple
numpfy_china = numpfy_china.reshape(numpy_image_dict['gene=ORF10'][0])# Create the matplotlib subplot
ax[row][col].pcolor(numpfy_china)
```

最后我们发现了基因突变。这通过简单地从两个序列中减去相同基因的 numpy 阵列来完成。例如，来自美国序列的 ORF10 基因将有一个 numpy 阵列，来自中国序列的 ORF10 基因将有一个 numpy 阵列。要找到突变，只需减去 numpy 数组。在该减法之后，将检查结果 numpy 数组，以查看是否存在任何非零值。如果存在一个非零值，那就是我们要找的突变。我们将在 python 字典中捕获这种变异，以便进一步分析。变异的基因和具体位置会在 python 控制台打印出来。

作者创建的要点

Python 控制台打印出变异信息。

![](img/a5cf7bde9f1e12e892240fa160019963.png)

作者图片

matplotlib 子图将为所有 11 个有突变结果的基因创建。每行显示第一个序列中的一个基因，接着是第二个基因中的相同基因，接着是该基因上的突变。如果每行的第三个子图中有一个亮点，这意味着该基因至少有一个突变。如果没有突变，那么第三条支线剧情将是黑暗的，没有亮点。

![](img/7fc7852e04902eb095507a4e1a33db79.png)

作者图片

通过这种方法，整个基因组及其来自 2 个不同序列的突变可以在单个图像中可视化。这是市场上其他突变分析工具的显著区别。这只是一个开始，我们现在有了 numpy 数组形式的数据。这意味着我们可以在这些 numpy 数组上应用更多的变换，无论是对某个特定国家或地区的少数序列还是所有序列。这将帮助我们获得比其他工具更多的洞察力和预测。

这个项目是从头开始写的。我已经构建了这个项目的所有代码，除了 numpy 和 matplotlib 之外，没有使用任何其他库。如果你已经读到这里，我感谢你阅读这篇文章。

特别感谢[马库斯·斯皮斯克](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral)。我在这篇文章开头使用的第一张图片是他创作的。

*编者按:*[*【towardsdatascience.com】*](https://slack-redir.net/link?url=http%3A%2F%2Ftowardsdatascience.com)*是一家以数据科学和机器学习研究为主的中型刊物。我们不是健康专家或流行病学家。想了解更多关于疫情冠状病毒的信息，可以点击* [*这里*](https://slack-redir.net/link?url=https%3A%2F%2Fwww.who.int%2Femergencies%2Fdiseases%2Fnovel-coronavirus-2019) *。*