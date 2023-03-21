# Python 中的 PDF 文本提取

> 原文：<https://towardsdatascience.com/pdf-text-extraction-in-python-5b6ab9e92dd?source=collection_archive---------2----------------------->

## 如何使用 PyPDF2 和 PDFMiner 从 PDF 文件中拆分、保存和提取文本。

![](img/7985e39ab08c0fc24fe7d4553a94dd63.png)

来自 [Pexels](https://www.pexels.com/photo/three-yellow-excavators-near-front-end-loader-1238864/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的[亚历山大·帕萨里克](https://www.pexels.com/@apasaric?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的照片

我不认为在为一篇关于从 pdf 文件中提取文本的文章撰写介绍段落时有多少创造性的空间。有一个 pdf，里面有文本，我们希望文本出来，我将向您展示如何使用 Python 来实现这一点。

在第一部分，我们将看看两个 Python 库，PyPDF2 和 PDFMiner。顾名思义，它们是专门为处理 pdf 文件而编写的库。我们将讨论我们需要的不同的类和方法。

然后，在第二部分，我们要做一个项目，就是把一个 708 页长的 pdf 文件拆分成单独的小文件，提取文本信息，清理，然后导出到易读的文本文件。关于这个项目的更多信息，请参考我的 [GitHub repo](https://github.com/MatePocs/lovecraft) 。

## PyPDF2

第一步，安装软件包:

```
pip install PyPDF2
```

我们需要的第一个对象是一个 [PdfFileReader](https://pythonhosted.org/PyPDF2/PdfFileReader.html) :

```
reader = PyPDF2.PdfFileReader('Complete_Works_Lovecraft.pdf')
```

该参数是我们想要使用的一个`pdf`文档的路径。使用这个`reader`对象，您可以获得关于您的文档的许多一般信息。例如，`reader.documentInfo`是包含以下格式的文档信息词典的属性:

```
{'/Author': 'H.P. Lovecraft',
 '/Creator': 'Microsoft® Word 2010',
 '/CreationDate': "D:20110729214233-04'00'",
 '/ModDate': "D:20110729214233-04'00'",
 '/Producer': 'Microsoft® Word 2010'}
```

还可以用`reader.numPages`得到总页数。

也许最重要的方法是`getPage(page_num)`，它将文件的一页作为单独的 [PageObject](https://pythonhosted.org/PyPDF2/PageObject.html#PyPDF2.pdf.PageObject) 返回。注意，PageObjects 在一个列表中，所以该方法使用一个从零开始的索引。

我们不打算大量使用 PageObject 类，你可以考虑做的另外一件事是`extractText`方法，它将页面的内容转换成一个字符串变量。例如，要获取 pdf 的第 7 页(记住，零索引)上的文本，您将首先从 PdfFileReader 创建一个 PageObject，并调用此方法:

```
reader.getPage(7-1).extractText()
```

然而，即使是官方文档也是这样描述这个方法的:*“这对于一些 PDF 文件来说效果很好，但是对于其他文件来说效果很差，这取决于所使用的生成器。”这并不完全令人放心，根据我的经验，`extractText`并没有正常工作，它遗漏了页面的第一行和最后一行。这是我在项目中使用另一个库 PDFMiner 的主要原因。*

接下来我们需要一个 [PdfFileWriter](https://pythonhosted.org/PyPDF2/PdfFileWriter.html) 对象。这个类没有参数，您可以像这样创建它:

```
writer = PyPDF2.PdfFileWriter()
```

对象将跟踪我们想要创建的`pdf`文件。为了向要创建的文件添加页面，使用`addPage`方法，该方法需要一个 PageObject 对象作为参数。例如，要从我们的输入 pdf 中添加某个页面:

```
my_page = reader.getPage(7)
writer.addPage(my_page)
```

最后，PdfFileWriter 对象有一个 write 方法，将内容保存在文件中。该方法需要一个参数，一个文件对象，这意味着简单地输入文件路径是行不通的。创建文件对象的一个简单方法是使用 Python 内置的`open`方法:

```
output_filename = 'pages_we_want_to_save.pdf'with open(output_filename, 'wb') as output:
    writer.write(output)
```

这些是我们将要使用的所有类和方法，参见 [PyPDF2 文档](https://pythonhosted.org/PyPDF2/)了解更多功能的信息。

现在我们可以读写 pdf 文件，但是我们仍然需要一个重要的功能:将内容转换成文本文件。为此，我们需要使用一个不同的库，PDFMiner。

## PDFMiner

我们将使用 pdfminer.six，它是原始 pdfminer 库的一个社区维护的分支。(自 2020 年起，PDFMiner 项目[不再维护](https://github.com/euske/pdfminer)。)

首先，您需要安装它:

```
pip install pdfminer.six
```

与 PyPDF2 相比，PDFMiner 的范围要有限得多，它实际上只专注于从 PDF 文件的源信息中提取文本。[文档](https://pdfminersix.readthedocs.io/en/latest/index.html)也非常集中，其中有大约三个例子，我们将基本上使用指南中提供的[代码](https://pdfminersix.readthedocs.io/en/latest/tutorial/composable.html)。由于代码看起来工作正常，我觉得没有必要深入研究。

这就是我们现在在实际项目中所需要的！

## Lovecraft 项目简介

只是为了给你一些我们为什么做这个项目的背景，我想对 T4 的作品做一个全面的 NLP 分析。如果你不知道他是谁，他是 20 世纪初的一位美国作家，以其怪异和宇宙恐怖小说而闻名，对现代流行文化有着巨大的影响。如果你想一窥他的写作风格，看看[这篇文章](https://medium.com/@katherineluck/how-to-write-like-h-p-lovecraft-4ae2e31ade32)，如果你想了解他性格中一些更有问题的方面，我推荐[这篇](https://medium.com/@keeltyc/reading-h-p-lovecraft-in-2018-28ee106f7069)。

最棒的是，他的作品在他去世 70 年后，在欧盟已经正式成为公共领域，在美国基本上也是公共领域，因为似乎没有人拥有版权。这就是为什么现在所有的东西都有克苏鲁，这是免费的知识产权。网上有很多来源可以收集他的作品，但有趣的是，它们不在古腾堡项目中。我发现其他一些集合有轻微的文字变化，一个例子是“经常”与“经常”在第一句墙外和睡眠，或不包含引号等。

最后，我发现 [Arkham Archivist](https://arkhamarchivist.com/free-complete-lovecraft-ebook-nook-kindle/) 是一个相对有据可查的选项(我也很喜欢这个名字)，下载了 pdf，并开始分割文件和提取文本。

## 将理论付诸实践

在本节中，我们将结合目前所学的内容。我们所需要做的就是从阿克汉姆档案馆[的大 pdf 文件开始。](https://arkhamarchivist.com/free-complete-lovecraft-ebook-nook-kindle/)

我们将使用几个通用函数，我将它们保存在一个单独的`data_func.py`文件中:

功能:

*   `convert_pdf_to_string`:这是我们从 pdfminer.six [文档](https://pdfminersix.readthedocs.io/en/latest/tutorial/composable.html)中复制的通用文本提取器代码，稍加修改，我们就可以将它用作一个函数；
*   `convert_title_to_filename`:一个函数，获取目录中出现的标题，并将其转换为文件的名称——当我开始工作时，我认为我们需要更多的调整；
*   `split_to_title_and_pagenum`:这个函数的用途在后面会变得更清楚，它基本上是用来判断目录中的一行是否实际上是一个标题-页码对，如果是，就以元组的形式返回。

下一步是设置环境，我们导入库(包括上面块中的函数)，检查文档的一些属性。最重要的是，我们设置了将在整个项目中使用的 PyPDF2 PdfFileReader 对象:`reader`。

现在我们可以开始处理文件了。看一下 pdf，似乎最好的办法是从目录中提取页码，然后用它们来分割文件。目录在 pdf 中的第 3 页和第 4 页，这意味着在 PageObjects 的 PdfFileReader 列表中的第 2 页和第 3 页。一旦我们在一个单独的文件中有了 pdf，我们就可以使用 pdfminer.six 代码来提取文本信息。(注意:我们也可以直接调整相关页面而不分割文件，但是我也想创建单独的 pdf 文件，并且有一个单独的目录文件也是有意义的。)

截至目前，`text`变量如下所示:

```
'Table of Contents \n\nPreface ............................................................................................................................. 2 \nThe Tomb .......................................................................................................................... 5 \nDagon ............................................................................................................................. 12 
...
```

诸如此类。几个简单的字符串替换了:

会将`text`转换成更好的格式。进行这样的调整时需要小心，例如，它还会删除任何“.”一个标题可能有，但幸运的是，我们没有这样的标题。我们现在有了目录中的行列表:

```
['Table of Contents ',
 '',
 'Preface  2 ',
 'The Tomb  5 ',
 'Dagon  12 ',
...
```

我们希望将标题和页码收集到列表中。这是通过去掉空格字符，然后检查该行是否以数字结尾来完成的。我们需要小心，因为我们确实希望保留标题中的空格。

现在我们有了列表，我们可以将文件分割成更小的 pdf 文件，保存在单独的文件夹中:

如果我们查看单个 pdf 文件，我们希望进行三项额外的调整:

*   每个故事的标题后面的括号里都有写作的年份。这不是原文的一部分，我们想把它们排除在外。但是，我们确实希望将它们保存在一个列表中，以便在将来的分析中使用它们。
*   每个故事都有一个返回目录的链接，“返回目录”，它显然不是原文的一部分。我检查了一下，这个单词组合(5-gram，我们将在后面的 NLP 项目中看到)没有出现在任何原始文本中，所以我们可以删除它。
*   最后一个故事的结尾有一个“Fin ”,还有一堆空格。因为字符“f-i-n”最有可能出现在英文文本的某个地方，所以我最后只删除了最后一个字符串的末尾。

运行所有故事的代码可能需要一分钟。

最后，我们将原始标题、文件名、页码和写作年份保存在一个 CSV 文件中。

这样，我们就完成了，我们成功地清理了文本，并将它们保存在单独的文本文件中。

## 结论

我想主要的问题是:这样做项目值得吗？我不能在别的地方找到文本文件吗？正如我提到的，我找到了文本来源，但这似乎是最可靠的。好吧，但是简单地手动复制文本不是更快吗？我正在考虑那件事，但是我不这样认为。想想我们应用的所有细微差别，收集它写的年份，删除非核心文本，等等。

无论如何，我学到了一些新东西，希望你觉得有用。看到一个人在做这样一个相对简单的项目时会遇到多少小问题是很有趣的，我已经迫不及待地想开始写这篇文章了。我真的很好奇洛夫克拉夫特在他的作品中到底用了多少次“恐怖”这个词！

[](https://matepocs.medium.com/membership) [## 加入我的推荐链接-伴侣概念

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

matepocs.medium.com](https://matepocs.medium.com/membership) 

## 参考

[](https://realpython.com/pdf-python/) [## 如何在 Python 中使用 PDF 真正的 Python

### 在这个循序渐进的教程中，您将学习如何在 Python 中使用 PDF。您将看到如何从…中提取元数据

realpython.com](https://realpython.com/pdf-python/) [](https://arkhamarchivist.com/free-complete-lovecraft-ebook-nook-kindle/) [## 面向 Nook 和 Kindle 的免费 H.P. Lovecraft 全集

### 在 2010 年 12 月初，我用一个来自澳大利亚古腾堡项目的文件创建了一个大多数 Lovecraft 的 EPUB

arkhamarchivist.com](https://arkhamarchivist.com/free-complete-lovecraft-ebook-nook-kindle/)  [## PyPDF2 文档- PyPDF2 1.26.0 文档

### 编辑描述

pythonhosted.org](https://pythonhosted.org/PyPDF2/)  [## pdfminer.six

### Pdfminer.six 是一个 python 包，用于从 PDF 文档中提取信息。查看 github 上的源代码。这个…

pdfminersix.readthedocs.io](https://pdfminersix.readthedocs.io/en/latest/index.html)