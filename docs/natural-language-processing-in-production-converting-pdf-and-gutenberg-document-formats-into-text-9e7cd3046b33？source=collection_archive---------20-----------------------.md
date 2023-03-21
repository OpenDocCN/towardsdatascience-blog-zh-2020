# 将 PDF 和 Gutenberg 文档格式转换为文本:生产中的自然语言处理

> 原文：<https://towardsdatascience.com/natural-language-processing-in-production-converting-pdf-and-gutenberg-document-formats-into-text-9e7cd3046b33?source=collection_archive---------20----------------------->

## 在生产级**自然语言处理(NLP** )中最关键的是将流行的文档格式快速预处理成文本。

![](img/fae8865e4c34fca18635ef253457f99a.png)

企业中有大量不同的文档。资料来源:联合国人类住区规划署

据估计，世界上 70%–85%的数据是文本(非结构化数据)。大多数英语和欧盟商务数据格式为字节文本、MS Word 或 **Adobe** PDF。[1]

组织 web 显示一个**dobe**Postscript 文档格式文档( **PDF** )。[2]

在这篇博客中，我详述了以下内容:

1.  从 web 文件名和本地文件名创建文件路径；
2.  将字节编码的古腾堡项目文件转换成文本语料库；
3.  将 PDF 文档转换成文本语料库；
4.  将连续文本分割成单词文本语料库。

# 将流行的文档格式转换为文本

## 1.从 web 文件名或本地文件名创建本地文件路径

以下函数将采用本地文件名或远程文件 URL 并返回一个文件路径对象。

```
#in file_to_text.py
--------------------------------------------
from io import StringIO, BytesIO
import urllib

def file_or_url(pathfilename:str) -> Any:
    *"""
    Reurn filepath given local file or URL.
    Args:
        pathfilename:

    Returns:
        filepath odject istance

    """* try:
        fp = open(pathfilename, mode="rb")  # file(path, 'rb')
    except:
        pass
    else:
        url_text = urllib.request.urlopen(pathfilename).read()
        fp = BytesIO(url_text)
    return fp
```

## 2.将 Unicode 字节编码文件转换成一个 o Python Unicode 字符串

您将经常遇到 8 位 Unicode 格式的文本 blob 下载(在浪漫的语言中)。您需要将 8 位 Unicode 转换为 Python Unicode 字符串。

```
#in file_to_text.py
--------------------------------------------
def unicode_8_to_text(text: str) -> str:
    return text.decode("utf-8", "replace")import urllib
from file_to_text import unicode_8_to_texttext_l = 250text_url = r'[http://www.gutenberg.org/files/74/74-0.txt'](http://www.gutenberg.org/files/74/74-0.txt') 
gutenberg_text =  urllib.request.urlopen(text_url).read()
%time gutenberg_text = unicode_8_to_text(gutenberg_text)
print('{}: size: {:g} \n {} \n'.format(0, len(gutenberg_text) ,gutenberg_text[:text_l]))
```

输出= >

```
CPU times: user 502 µs, sys: 0 ns, total: 502 µs
Wall time: 510 µs
0: size: 421927 

The Project Gutenberg EBook of The Adventures of Tom Sawyer, Complete by
Mark Twain (Samuel Clemens)

This eBook is for the use of anyone anywhere at no cost and with almost
no restrictions whatsoever. You may copy it, give it away or re-use
it under the terms of the Project Gutenberg License included with this
eBook or online at www.guten
```

结果是`text.decode('utf-8')` 可以在大约 1/1000 秒内格式化成一个包含一百万个字符的 **Python** 字符串。这一速度远远超过了我们的生产率要求。

## 3.将 PDF 文档转换为文本语料库。

***“将 PDF 文档转换成文本语料库*** ”是我为 **NLP** 文本预处理做的最麻烦也是最常见的任务之一。

```
#in file_to_text.py
--------------------------------------------
def PDF_to_text(pathfilename: str) -> str:
    *"""
    Chane PDF format to text.
    Args:
        pathfilename:

    Returns:

    """* fp = file_or_url(pathfilename)
    rsrcmgr = PDFResourceManager()
    retstr = StringIO()
    laparams = LAParams()
    device = TextConverter(rsrcmgr, retstr, laparams=laparams)
    interpreter = PDFPageInterpreter(rsrcmgr, device)
    password = ""
    maxpages = 0
    caching = True
    pagenos = set()

    for page in PDFPage.get_pages(
        fp,
        pagenos,
        maxpages=maxpages,
        password=password,
        caching=caching,
        check_extractable=True,
    ):
        interpreter.process_page(page)

    text = retstr.getvalue()

    fp.close()
    device.close()
    retstr.close()

    return text
-------------------------------------------------------arvix_list =['[https://arxiv.org/pdf/2008.05828v1.pdf'](https://arxiv.org/pdf/2008.05828v1.pdf')
             , '[https://arxiv.org/pdf/2008.05981v1.pdf'](https://arxiv.org/pdf/2008.05981v1.pdf')
             , '[https://arxiv.org/pdf/2008.06043v1.pdf'](https://arxiv.org/pdf/2008.06043v1.pdf')
             , 'tmp/inf_finite_NN.pdf' ]
for n, f in enumerate(arvix_list):
    %time pdf_text = PDF_to_text(f).replace('\n', ' ')
    print('{}: size: {:g} \n {} \n'.format(n, len(pdf_text) ,pdf_text[:text_l])))
```

输出= >

```
CPU times: user 1.89 s, sys: 8.88 ms, total: 1.9 s
Wall time: 2.53 s
0: size: 42522 
 On the Importance of Local Information in Transformer Based Models  Madhura Pande, Aakriti Budhraja, Preksha Nema  Pratyush Kumar, Mitesh M. Khapra  Department of Computer Science and Engineering  Robert Bosch Centre for Data Science and AI (RBC-DSAI)  Indian Institute of Technology Madras, Chennai, India  {mpande,abudhra,preksha,pratyush,miteshk}@ 

CPU times: user 1.65 s, sys: 8.04 ms, total: 1.66 s
Wall time: 2.33 s
1: size: 30586 
 ANAND,WANG,LOOG,VANGEMERT:BLACKMAGICINDEEPLEARNING1BlackMagicinDeepLearning:HowHumanSkillImpactsNetworkTrainingKanavAnand1anandkanav92@gmail.comZiqiWang1z.wang-8@tudelft.nlMarcoLoog12M.Loog@tudelft.nlJanvanGemert1j.c.vangemert@tudelft.nl1DelftUniversityofTechnology,Delft,TheNetherlands2UniversityofCopenhagenCopenhagen,DenmarkAbstractHowdoesauser’sp 

CPU times: user 4.82 s, sys: 46.3 ms, total: 4.87 s
Wall time: 6.53 s
2: size: 57204 
 0 2 0 2     g u A 3 1         ]  G L . s c [      1 v 3 4 0 6 0  .  8 0 0 2 : v i X r a  Ofﬂine Meta-Reinforcement Learning with  Advantage Weighting  Eric Mitchell1, Rafael Rafailov1, Xue Bin Peng2, Sergey Levine2, Chelsea Finn1  1 Stanford University, 2 UC Berkeley  em7@stanford.edu  Abstract  Massive datasets have proven critical to successfully 

CPU times: user 12.2 s, sys: 36.1 ms, total: 12.3 s
Wall time: 12.3 s
3: size: 89633 
 0 2 0 2    l u J    1 3      ]  G L . s c [      1 v 1 0 8 5 1  .  7 0 0 2 : v i X r a  Finite Versus Inﬁnite Neural Networks:  an Empirical Study  Jaehoon Lee  Samuel S. Schoenholz∗  Jeffrey Pennington∗  Ben Adlam†∗  Lechao Xiao∗  Roman Novak∗  Jascha Sohl-Dickstein  {jaehlee, schsam, jpennin, adlam, xlc, romann, jaschasd}@google.com  Google Brain
```

在这个硬件配置上，“将 PDF 文件转换成 Python 字符串需要 150 秒。对于 Web 交互式生产应用程序来说不够快。

您可能希望在后台设置格式。

## 4.将连续文本分割成单词文本语料库

当我们阅读[https://arxiv.org/pdf/2008.05981v1.pdf'](https://arxiv.org/pdf/2008.05981v1.pdf')时，它返回的是没有分隔字符的连续文本。使用来自 **wordsegment，**的包，我们把连续的字符串分成单词。

```
from wordsegment import load,  clean, segment
%time words = segment(pdf_text)
print('size: {:g} \n'.format(len(words)))
' '.join(words)[:text_l*4]
```

输出= >

```
CPU times: user 1min 43s, sys: 1.31 s, total: 1min 44s
Wall time: 1min 44s
size: 5005'an and wang loog van gemert blackmagic in deep learning 1 blackmagic in deep learning how human skill impacts network training kanavanand1anandkanav92g mailcom ziqiwang1zwang8tudelftnl marco loog12mloogtudelftnl jan van gemert 1jcvangemerttudelftnl1 delft university of technology delft the netherlands 2 university of copenhagen copenhagen denmark abstract how does a users prior experience with deep learning impact accuracy we present an initial study based on 31 participants with different levels of experience their task is to perform hyper parameter optimization for a given deep learning architecture the results show a strong positive correlation between the participants experience and then al performance they additionally indicate that an experienced participant nds better solutions using fewer resources on average the data suggests furthermore that participants with no prior experience follow random strategies in their pursuit of optimal hyperparameters our study investigates the subjective human factor in comparisons of state of the art results and scientic reproducibility in deep learning 1 introduction the popularity of deep learning in various elds such as image recognition 919speech1130 bioinformatics 2124questionanswering3 etc stems from the seemingly favorable tradeoff between the recognition accuracy and their optimization burden lecunetal20 attribute their success t'
```

你会注意到**单词切分**完成了相当精确的单词切分。有一些错误，或者我们不想要的单词， **NLP** 文本预处理清除掉。

阿帕奇语段速度很慢。对于少于 1000 字的小文档，它几乎不能满足生产的需要。我们能找到更快的分割方法吗？

## 4b。将连续文本分割成单词文本语料库

似乎有一种更快的方法来“将连续文本分割成单词文本的语料库”

正如以下博客中所讨论的:

[](/symspell-vs-bk-tree-100x-faster-fuzzy-string-search-spell-checking-c4f10d80a078) [## 符号拼写与 BK 树:模糊字符串搜索和拼写检查快 100 倍

### 传统智慧和教科书说 BK 树特别适合拼写纠正和模糊字符串搜索…

towardsdatascience.com](/symspell-vs-bk-tree-100x-faster-fuzzy-string-search-spell-checking-c4f10d80a078) 

**SymSpell** 快 100-1000 倍。哇！

*注:编辑:2020 年 8 月 24 日沃尔夫·加贝指出*值得称赞

> SymSpell 博客文章中给出的基准测试结果(快 100-1000 倍)仅指拼写纠正，而非分词。在那篇文章中，SymSpell 与其他拼写纠正算法进行了比较，而不是与分词算法进行比较。2020 年 8 月 23 日

和

> 此外，从 Python 调用 C#库还有一种更简单的方法:[https://stack overflow . com/questions/7367976/calling-a-C-sharp-library-from-Python](https://stackoverflow.com/questions/7367976/calling-a-c-sharp-library-from-python)—Wolfe Garbe 8/23/2020

注:编辑日期:2020 年 8 月 24 日。我打算试试 Garbe 的 C##实现。如果我没有得到相同的结果(很可能如果我得到了)，我将尝试 cython port，看看我是否能作为管道元素适合 spacy。我会让你知道我的结果。

但是在 **C#** 中实现。因为我不会掉进无限的老鼠洞:

*   把我所有的 **NLP** 转换成 **C#** 。不可行的选择。
*   从 **Python** 调用 **C#** 。我和 Python 团队的两位工程师经理谈过。他们拥有 **Python-C#** 功能，但是它涉及到:

注意:

1.  翻译成**VB**-香草；
2.  人工干预和翻译必须通过再现性测试；
3.  从 **VB** -vanilla 翻译成**C**；
4.  手动干预和翻译必须通过重复性测试。

相反，我们使用一个到 Python 的端口。下面是一个版本:

```
def segment_into_words(input_term):
    # maximum edit distance per dictionary precalculation
    max_edit_distance_dictionary = 0
    prefix_length = 7
    # create object
    sym_spell = SymSpell(max_edit_distance_dictionary, prefix_length)
    # load dictionary
    dictionary_path = pkg_resources.resource_filename(
        "symspellpy", "frequency_dictionary_en_82_765.txt")
    bigram_path = pkg_resources.resource_filename(
        "symspellpy", "frequency_bigramdictionary_en_243_342.txt")
    # term_index is the column of the term and count_index is the
    # column of the term frequency
    if not sym_spell.load_dictionary(dictionary_path, term_index=0,
                                     count_index=1):
        print("Dictionary file not found")
        return
    if not sym_spell.load_bigram_dictionary(dictionary_path, term_index=0,
                                            count_index=2):
        print("Bigram dictionary file not found")
        returnresult = sym_spell.word_segmentation(input_term)
    return result.corrected_string%time long_s = segment_into_words(pdf_text)
print('size: {:g} {}'.format(len(long_s),long_s[:text_l*4]))
```

输出= >

```
CPU times: user 20.4 s, sys: 59.9 ms, total: 20.4 s
Wall time: 20.4 s
size: 36585 ANAND,WANG,LOOG,VANGEMER T:BLACKMAGICINDEEPLEARNING1B lack MagicinDeepL earning :HowHu man S kill Imp acts Net work T raining Ka nav An and 1 an and kana v92@g mail . com ZiqiWang1z. wang -8@tu delft .nlM arc oLoog12M.Loog@tu delft .nlJ an van Gemert1j.c. vang emert@tu delft .nl1D elf tUniversityofTechn ology ,D elf t,TheN ether lands 2UniversityofC open hagen C open hagen ,Den mark Abs tract How does a user ’s prior experience with deep learning impact accuracy ?We present an initial study based on 31 participants with different levels of experience .T heir task is to perform hyper parameter optimization for a given deep learning architecture .T here -s ult s show a strong positive correlation between the participant ’s experience and the ﬁn al performance .T hey additionally indicate that an experienced participant ﬁnds better sol u-t ions using fewer resources on average .T he data suggests furthermore that participants with no prior experience follow random strategies in their pursuit of optimal hyper pa-ra meters .Our study investigates the subjective human factor in comparisons of state of the art results and sci entiﬁc reproducibility in deep learning .1Intro duct ion T he popularity of deep learning in various ﬁ eld s such as image recognition [9,19], speech [11,30], bio informatics [21,24], question answering [3] etc . stems from the seemingly fav or able trade - off b
```

在 **Python** 中实现的 SymSpellpy 大约快了 5 倍。我们没有看到 100-1000 倍的速度。

我猜 **SymSpell-C#** 是在比较用 **Python** 实现的不同分割算法。

也许我们看到的加速是由于 **C#** ，一种编译过的静态类型语言。由于 **C#** 和 **C** 的计算速度差不多，我们应该期待 **C#** 比 **Python** 实现快 100-1000 倍。

> 注意:有一个 **spacy** 流水线实现 **spacy_symspell，**直接调用 **SymSpellpy。**我建议你**不要**使用 spacy_symspell。Spacy 首先生成令牌作为流水线的第一步，这是不可变的。 **spacy_symspell** 从分割连续文本生成新文本。它不能在空间**中生成新的令牌，因为空间**已经生成了令牌**。。一个** **空间**管道工作于一个令牌序列，而不是一个文本流**。人们将不得不衍生出一个改变了的版本。何必呢？**代替**，**将连续文本分割成单词文本语料库。然后更正单词中嵌入的空格和带连字符的单词。做任何其他你想做的原始清洁。然后将原始文本输入到**空间**。

我显示**空间 _ 符号拼写。**再一次我的建议是 ***不要*** 使用它。

```
import spacy
from spacy_symspell import SpellingCorrector def segment_into_words(input_term):
nlp = spacy.load(“en_core_web_lg”, disable=[“tagger”, “parser”])
corrector = SpellingCorrector()
nlp.add_pipe(corrector)
```

# 结论

在以后的博客中，我会详细介绍许多常见和不常见的**快速文本预处理方法。**此外，我将展示从移动 **SymSpellpy** 到 **cython 的预期加速。**

在“将 X 格式转换成文本语料库”的世界中，您需要支持更多的格式和 API。

我详细介绍了两种更常见的文档格式， **PDF** 和**古腾堡** **项目**格式。另外，我给出了两个 **NLP** 实用函数`segment_into_words`和`file_or_url.`

我希望你学到了一些东西，并且可以使用这个博客中的一些代码。

如果你有一些格式转换或者更好的软件包，请告诉我。

# 参考

[1] [我们每天会产生多少数据？](https://www.forbes.com/sites/bernardmarr/2018/05/21/how-much-data-do-we-create-every-day-the-mind-blowing-stats-everyone-should-read/#4eb3f0f960ba)

[2][https://en.wikipedia.org/wiki/PDF](https://en.wikipedia.org/wiki/PDF)

【3】[将 DNA 序列分割成‘单词’](https://arxiv.org/pdf/1202.2518.pdf)