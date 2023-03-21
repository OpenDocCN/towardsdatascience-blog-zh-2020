# 当 Pandas 失败时，将 Word 和 PDF 数据导入 Python 的 4 种简单方法

> 原文：<https://towardsdatascience.com/4-simple-ways-to-import-word-and-pdf-files-into-python-when-pandas-fails-43cf81599461?source=collection_archive---------14----------------------->

## 导入非结构化文本/图像数据的实用指南

作为数据科学/分析团队的一员，您可能会遇到许多要在 Python 中导入和分析的文件类型。在理想世界中，我们所有的数据都驻留在基于云的数据库(例如 SQL、NoSQL)中，这些数据库易于查询和提取。然而，在现实世界中，我们很少得到整洁的表格数据。此外，如果我们需要额外的数据(结构化或非结构化的)来增强分析，我们将不可避免地使用不同格式的原始数据文件。

![](img/acc56ac7d797af3836d7a2f9af4746a1.png)

[照片由 Pexels 的 Skitterphoto 拍摄](https://www.pexels.com/photo/words-text-scrabble-blocks-695571/)

最近，我的团队开始了一个项目，作为第一步，包括将原始数据文件整合成*格式。csv，。xlsx，。pdf，。docx、*和*。doc* 。我的第一反应:威武 ***熊猫*** ！其中当然处理了*。csv* 和*。xlsx* ，但是关于*。pdf* 和*。docx* ，我们将不得不探索超越 ***熊猫*** 的可能性。

在这篇博客中，我将分享我的技巧和诀窍，以帮助您轻松地导入 PDF 和 Word 文档(到 Python 中),以防它出现在您自己的工作中，尤其是在您的 NLP 自然语言处理项目中。所有样本数据文件都可以公开访问，文件副本以及相应的下载链接可以在 [my Github repo](https://github.com/YiLi225/Import_PDF_Word_Python) 中找到。

1.  [***Python-docx →* 用MS Word 工作。docx 文件**](https://python-docx.readthedocs.io/en/latest/user/documents.html)

作为最常用的文档工具之一，MS Word 经常是人们编写和共享文本的首选。对于带有。 *docx* 扩展，Python 模块**docx是一个得心应手的工具，下面展示如何导入*。docx* 段落只有 2 行代码，**

现在让我们打印出输出信息，

docx 返回的信息和示例段落

正如我们所看到的，返回的是一个字符串/句子的列表，因此我们可以利用字符串处理技术和正则表达式来获取文本数据以备进一步分析(例如，NLP)。

**2。**[***win 32 com*→用 MS Word 工作。doc 文件**](https://github.com/mhammond/pywin32)

尽管很容易使用，python-docx 模块不能接受老化的 T2。doc 分机，信不信由你，*。doc* 文件仍然是许多利益相关者的首选文字处理器(尽管有*)。docx* 已经存在十多年了)。如果在这种情况下，转换文件类型不是一个选项，我们可以求助于***win32 com . client***包中的几个技巧。

基本技术是首先启动一个 Word 应用程序作为活动文档，然后读取 Python 中的内容/段落。下面定义的函数 docReader()展示了如何(并且[完全烘焙的代码片段链接到这里](https://github.com/YiLi225/Import_PDF_Word_Python))，

运行这个函数后，我们应该会看到与第 1 部分相同的输出。**两个小技巧:** (1)我们设置**字。Visible = False** 隐藏物理文件，这样所有的处理工作都在后台完成；(2)参数 **doc_file_name** 需要完整的文件路径，而不仅仅是文件名。否则，函数文档。Open()不会识别文件，即使将工作目录设置为当前文件夹。

加入我们的 YouTube 社区🎦 [***【数据与 Kat 谈判】***](https://www.youtube.com/channel/UCbGx9Om38Ywlqi0x8RljNdw) ***😄***

现在，让我们来看看 PDF 文件，

**3。** [***Pdfminer(代替 pypdf F2)*→使用 PDF 文本**](https://github.com/euske/pdfminer)

说到在 Python 中处理 PDF 文件，大家熟知的模块[***py PDF 2***](https://github.com/mstamy2/PyPDF2)大概会是包括我自己在内的大部分分析师最初的尝试。因此，我使用 ***PyPDF2*** (完整代码可在我的 [Github repo](https://github.com/YiLi225/Import_PDF_Word_Python) 中获得)对其进行编码，这给出了文本输出，如下所示。

嗯，显然这看起来不对，因为所有的空格都不见了！没有合适的空格，我们就无法正确解析字符串。

事实上，这个例子揭示了 ***PyPDF2*** 中的函数 extractText()的一个警告:它对于包含复杂文本或不可打印的空格字符的 PDF 执行得不好。因此，让我们切换到 ***pdfminer*** 并探索如何将这个 pdf 文本导入，

现在，输出看起来好多了，可以用文本挖掘技术轻松清理，

**4。**[***Pdf 2 image+Pytesseract*→处理 PDF 扫描图像**](https://github.com/madmaze/pytesseract)

对于数据科学家来说更复杂的是(当然), pdf 可以(并且经常)由扫描图像代替文本文档创建；因此，它们不能被 pdf 阅读器呈现为纯文本，不管它们被组织得多么整齐。

在这种情况下，我发现的最佳技术是首先显式地提取图像，然后用 Python 读取和解析这些图像。我们将用模块[***pdf 2 image***](https://github.com/Belval/pdf2image)和[***pytesract***](https://github.com/madmaze/pytesseract)***来实现这个想法。*** 如果后者听起来比较陌生，那么***pytesserac***是 Python 的 OCR 光学字符识别工具，可以识别和读取图像中嵌入的文字。现在，这是基本功能，

在输出中，您应该会看到如下所示的扫描图像文本:

```
PREFACE   In 1939 the Yorkshire Parish Register Society, of which the Parish Register Section of the Yorkshire Archaeological Society is the successor (the publications having been issued in numerical sequence without any break) published as its Volume No. 108 the entries in the Register of Wensley Parish Church from 1538 to 1700 inclusive. These entries comprised the first 110 pages (and a few lines of p. 111) of the oldest register at Wensley.
```

任务完成！另外，您现在还知道了如何从图像中提取数据，即***pytesserac***模块中的 image_to_string()。

**注意:**要使 ***宇宙魔方*** 模块成功运行，您可能需要执行额外的配置步骤，包括安装 ***poppler*** 和 ***宇宙魔方*** 包。同样，请在 [my Github 这里](https://github.com/YiLi225/Import_PDF_Word_Python)随意抓取更健壮的实现和详细的配置列表。

最后，Greg Horton 提到了一个数据科学笑话，他在文章中谈到了[数据争论中的 80-20 法则](https://blog.timextender.com/reversing-the-80-20-rule-in-data-wrangling):

> 数据科学家花 80%的时间处理数据准备问题，另外 20%的时间抱怨处理数据准备问题需要多长时间。

通过经历从 Word 和 PDF 文件中抓取文本的不同方式，我希望这个博客可以让你的 80%变得更容易/不那么无聊，这样你就不会拔头发，也可以减少另外 20%，这样你就可以花更多的时间阅读有趣的中等文章。

**最后提示**:一旦完成了对文件的处理，关闭连接以便其他应用程序可以访问该文件总是一个好的编码实践。这就是为什么你在上面每个函数的末尾看到了`close()`方法。😃

***想要更多的数据科学和编程技巧？使用*** [***我的链接***](https://yilistats.medium.com/membership) ***注册 Medium，获得我所有内容的完全访问权限。***

***还订阅我新创建的 YouTube 频道*** [***《数据与吉谈》***](https://www.youtube.com/channel/UCbGx9Om38Ywlqi0x8RljNdw)

## *喜欢这个博客吗？这是另一个你可能喜欢的数据科学博客:*

*[](/6-sql-tricks-every-data-scientist-should-know-f84be499aea5) [## 每个数据科学家都应该知道的 6 个 SQL 技巧

### 提高分析效率的 SQL 技巧

towardsdatascience.com](/6-sql-tricks-every-data-scientist-should-know-f84be499aea5)*