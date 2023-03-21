# python 语言入门指南

> 原文：<https://towardsdatascience.com/beginners-guide-to-pythainlp-4df4d58c1fbe?source=collection_archive---------22----------------------->

## 泰语的文本处理和语言学分析

![](img/ec56f2d01e881441d481bb0c76ba2fd8.png)

照片由 [Mateusz Turbiński](https://unsplash.com/@mateuszturbinski?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/thai?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

通过阅读这篇文章，您将了解更多关于 PyThaiNLP 背后的基本概念和特性。基于官方文档，PyThaiNLP 提供了

> “…泰语的标准 NLP 功能，例如词性标记、语言单位分割(音节、单词或句子)。”

本教程有 3 个部分:

1.  设置
2.  履行
3.  结论

让我们继续下一部分，开始安装必要的模块

# 1.设置

强烈建议您在继续安装过程之前创建一个虚拟环境。你可以很容易地用 pip install 安装它。

```
pip install pythainlp
```

如果您已经有一个现有的包，并且想要更新它，请运行以下命令。

```
pip install -U pythainlp
```

某些功能需要额外的软件包，这些软件包可以按如下方式安装:

```
pip install pythainlp[package1, package2, ...]
```

完整列表如下:

*   `attacut` —支持 attacut，一个快速准确的记号赋予器
*   `icu` —对于 ICU，Unicode 的国际组件，支持音译和标记化
*   `ipa` —国际音标，支持音译
*   `ml` —支持分类的 ULMFiT 模型
*   `thai2fit` —为泰语词矢量
*   `thai2rom` —用于机器学习的罗马化
*   `ner` —用于命名实体识别

您可以通过以下命令安装它们

```
pip install pythainlp[full]
```

一旦你完成了，让我们进入下一节，开始写 Python 代码。

# 2.履行

## 效用

在 Python 文件的顶部添加以下导入声明。我将在本教程中使用 Jupyter 笔记本。

```
import pythainlp.util
```

PyThaiNLP 为我们提供了相当多的内置函数。例如，您可以使用以下内容来确定输入文本是否是泰语。

```
pythainlp.util.isthai("สวัสดี")
#True
```

此外，您甚至可以获得泰语字符在整个字符串中的百分比计数。

```
pythainlp.util.countthai("Hello สวัสดี")
#54.54545454545454
```

## 排序和整理

可以通过调用`collate`函数，根据泰语词典中的实际顺序按字母顺序排列。添加以下导入内容

```
from pythainlp.util import collate
```

将列表传递给 collate 函数

```
words = ['แอปเปิ้ล', 'กล้วย', 'ส้ม', 'สับปะรด', 'มะละกอ', 'ทุเรียน']
collate(words)
#['กล้วย', 'ทุเรียน', 'มะละกอ', 'ส้ม', 'สับปะรด', 'แอปเปิ้ล']
```

您可以指定 reverse 函数以相反的顺序显示它

```
collate(words, reverse=True)
#['แอปเปิ้ล', 'สับปะรด', 'ส้ม', 'มะละกอ', 'ทุเรียน', 'กล้วย']
```

## 日期和时间

供您参考，泰语中的日期时间遵循泰国佛教纪元(公元前)。这个模块提供了一个方便的函数，可以将日期时间对象解析成您想要的格式，就像您通过`datetime.strftime()`所做的那样。添加以下导入语句。

```
import datetime
from pythainlp.util import thai_strftime
```

`thai_strftime`接受两个参数:

*   日期时间对象
*   格式字符串

如下初始化两个变量:

```
fmt = "%Aที่ %d %B พ.ศ. %Y เวลา %H:%M น. (%a %d-%b-%y)"
date = datetime.datetime(2020, 7, 27, 1, 40)thai_strftime(date, fmt)
#วันจันทร์ที่ 27 กรกฎาคม พ.ศ. 2563 เวลา 01:40 น. (จ 27-ก.ค.-63)
```

从 2.2 版开始，您可以在 main 指令前添加以下修饰符。

*   `- (minus)` —不填充数字结果字符串
*   `_ (underscore)` —用空格填充数字结果字符串
*   `0 (zero)` —用零填充数字结果字符串
*   `^ (caret)` —将结果字符串中的字母字符转换为大写
*   `# (pound sign)` —交换结果字符串的大小写
*   `O (capital letter o)` —使用区域设置的替代数字符号(泰语数字)

让我们再试一次，但这次我们将使用泰语数字显示日期。我在`%d`中间加了一个大写`O`。

```
fmt = "%Aที่ %Od %B พ.ศ. %Y เวลา %H:%M น. (%a %d-%b-%y)"
date = datetime.datetime(2020, 7, 27, 1, 40)thai_strftime(date, fmt)
#วันจันทร์ที่ ๒๗ กรกฎาคม พ.ศ. 2563 เวลา 01:40 น. (จ 27-ก.ค.-63)
```

## 拼出时间

您可以通过`time_to_thaiword()`函数完全用泰语显示日期时间对象或字符串(HH:MM:SS)。在此之前，添加以下导入语句

```
from pythainlp.util import time_to_thaiword
```

按如下方式传递日期时间对象或字符串(HH:MM:SS):

```
time_to_thaiword("10:05:34")
#สิบนาฬิกาห้านาทีสามสิบสี่วินาที
```

您可以指定以下附加参数:

*   `fmt` —确定拼写方式。它接受以下字符串输入:`24h`、`6h`或`m6h`
*   `precision` —接受分钟级别的`m`或二级级别的`s`。如果没有，它将只读取非零值。

## 句子标记化

您可以通过内置的`sent_tokenize()`功能将一个长段落拆分成句子。按如下方式导入它:

```
from pythainlp import sent_tokenize
```

然后，您可以通过向它传递一个字符串来轻松地调用它。让我们看看下面的例子。

```
text = "เมื่อวันที่ 28 ก.ค.เวลา 07.00 น. ณ บริเวณท้องสนามหลวง พล.อ.อนุพงษ์ เผ่าจินดา รมว.มหาดไทย พร้อมด้วย นายนิพนธ์ บุญญามณี รมช.มหาดไทย นายทรงศักดิ์ ทองศรี รมช.มหาดไทย นายฉัตรชัย พรหมเลิศ ปลัดกระทรวงมหาดไทย และผู้บริหารระดับสูงของกระทรวงมหาดไทย ร่วมพิธีเจริญพระพุทธมนต์ และพิธีทำบุญตักบาตรถวายเป็นพระราชกุศล เนื่องในโอกาสวันเฉลิมพระชนมพรรษา พระบาทสมเด็จพระเจ้าอยู่หัว ประจำปี 2563  ในเวลา 07.30 น. พล.อ. อนุพงษ์ และคณะ ร่วมประกอบพิธีถวายสัตย์ปฏิญาณ เพื่อเป็นข้าราชการที่ดีและพลังของแผ่นดิน"sent_tokenize(text)
```

默认情况下，它使用`crfcut`引擎进行句子分词。您可以指定它使用其他引擎。下面的例子使用`whitespace+newline`引擎。

```
sent_tokenize(text, engine="whitespace+newline")
```

## 单词标记化

PyThaiNLP 还附带了一个分词标记器功能。它使用最大匹配算法，`newmm`作为默认引擎。添加以下导入语句:

```
from pythainlp import word_tokenize
```

您可以指定`keep_whitespace`参数从最终输出中删除空格

```
text = "เมื่อวันที่ 28 ก.ค.เวลา 07.00 น. ณ บริเวณท้องสนามหลวง พล.อ.อนุพงษ์ เผ่าจินดา รมว.มหาดไทย พร้อมด้วย นายนิพนธ์ บุญญามณี รมช.มหาดไทย"word_tokenize(text, keep_whitespace=False)
```

可能的引擎包括:

*   `longest`
*   `newmm`
*   `newmm-safe`
*   `attacut` —需要额外的依赖性。通过`pip install attacut`安装。

## 正常化

您可以调用`normalize()`函数来删除零宽度空格、重复空格、重复元音和悬空字符。在去除重复元音的过程中，它还对元音和声调标记进行重新排序。让我们看看下面的例子:

```
from pythainlp.util import normalize

normalize("เเปลก") == "แปลก"
# เ เ ป ล ก  vs แ ป ล ก
```

## 数字转换

如果您正在寻找将泰语和数字相互转换的方法，以下函数非常适合您:

*   `arabic_digit_to_thai_digit` —将阿拉伯数字转换为泰国数字
*   `thai_digit_to_arabic_digit` —将泰语数字转换为阿拉伯数字
*   `digit_to_text` —将数字转换为泰语文本

下面的例子说明了调用函数的正确方法

```
from pythainlp.util import arabic_digit_to_thai_digit, thai_digit_to_arabic_digit, digit_to_texttext = "สำหรับการโทรฉุกเฉิน 999 ๙๙๙"arabic_digit_to_thai_digit(text)
thai_digit_to_arabic_digit(text)
digit_to_text(text)
```

## 词性标注

PyThaiNLP 提供了两个分析词性标签的函数。

*   `pos_tag` —基于文字输入
*   `pos_tag_sents` —基于句子输入

供您参考，您不能简单地将一个字符串列表传递给`pos_tag`，因为这会影响结果。最好的方法是循环遍历列表，一个一个地调用。导入`post_tag()`函数，并如下循环列表:

```
from pythainlp.tag import pos_tagdoc = ["แอปเปิ้ล", "ส้ม", "กล้วย"]
for i in doc:
    print(pos_tag([i], corpus="orchid_ud"))
```

您可以指定如下`engine`参数:

*   `perceptron` —感知器标记器(默认)
*   `unigram` —单字标签

`corpus`参数有三个选项:

*   `orchid` —带注释的泰国学术文章(默认)
*   `orchid_ud` —带注释的泰语学术文章，但 POS 标签被映射以符合通用依赖性 POS 标签
*   `pud` —并行通用依赖(PUD)

您应该得到以下输出

```
[('แอปเปิ้ล', 'NOUN')]
[('ส้ม', 'NOUN')]
[('กล้วย', 'NOUN')]
```

## 命名实体识别

您必须为 NER 安装额外的依赖项。在您的终端中运行以下命令

```
pip install pythainlp[ner]
```

导入`ThaiNameTagger`并初始化如下

```
from pythainlp.tag.named_entity import ThaiNameTagger

ner = ThaiNameTagger()
ner.get_ner("24 มิ.ย. 2563 ทดสอบระบบเวลา 6:00 น. เดินทางจากขนส่งกรุงเทพใกล้ถนนกำแพงเพชร ไปจังหวัดกำแพงเพชร ตั๋วราคา 297 บาท")
```

# 3.结论

让我们回顾一下今天所学的内容。

我们首先根据我们需要的特性安装必要的模块和依赖项。

然后，我们继续深入探讨 PyThaiNLP 背后的基本概念和功能。我们特别在文本标记化、词性标注和命名实体识别方面做了一些尝试。

感谢你阅读这篇文章。希望在下一篇文章中再见到你！

# 参考

1.  [PyThaiNLP Github](https://github.com/PyThaiNLP/pythainlp)
2.  [PyThaiNLP 文档](https://thainlp.org/pythainlp/docs/2.2/notes/getting_started.html)