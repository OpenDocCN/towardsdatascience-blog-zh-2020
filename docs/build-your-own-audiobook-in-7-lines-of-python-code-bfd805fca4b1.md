# 用 7 行 Python 代码构建您自己的有声读物

> 原文：<https://towardsdatascience.com/build-your-own-audiobook-in-7-lines-of-python-code-bfd805fca4b1?source=collection_archive---------20----------------------->

## 不买有声读物，用 Python 实现文本转语音有声读物

![](img/b31da90b7c9f6f222d3beb1d670b7381.png)

图片由来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=3106986) 的 [Felix Lichtenfeld](https://pixabay.com/users/sik-life-2171488/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=3106986) 拍摄

有声读物是大声朗读的一本书或其他作品的录音或画外音。可用的流行有声读物列表有:

*   [可听](https://www.audible.com/)
*   [有声读物](https://www.audiobooks.com/browse)
*   [Scribd](https://www.scribd.com/audiobooks)

如果你有一本 pdf 格式的有声读物，你就不需要订阅。在本文中，您将知道如何用 7 行 python 代码开发一个基本的有声读物。

## 安装:

Python 有大量服务于各种目的的库。在本文中，我们将需要两个库( **pyttsx3，PyPDF2** )来开发一本有声读物。

您可以从 PyPl 安装这些库，

```
**pip install PyPDF2
pip install pyttsx3**
```

在这里阅读 pyttsx3 库[的完整文档。](https://pyttsx3.readthedocs.io/en/latest/)

# 1)阅读 PDF 文件:

Python 有库 **PyPDF2** ，它是作为 PDF 工具包构建的。它允许在内存中操作 pdf。该库能够:

*   提取文档信息，如标题、作者等
*   按页面拆分文档
*   按页面合并文档
*   裁剪页面
*   将多页合并成一页
*   加密和解密 PDF 文件
*   还有更多！

我们正在使用这个库将 pdf 文件逐页拆分，读取每页上的文本，并将文本发送到下一层/步骤。

```
import PyPDF2
pdfReader = PyPDF2.PdfFileReader(open('file.pdf', 'rb'))
```

# 2)初始化扬声器:

Python 有一个 pyttsx3 库，能够离线转换文本到语音。我们使用 pypdf2 库从 pdf 文件中读取的文本作为文本到语音转换引擎的输入。

```
import pyttsx3
speaker = pyttsx3.init()
```

# 3)播放有声读物:

使用 **PyPDF2** 实现从 pdf 文件中逐页提取文本。通过阅读文本并将其提供给 **pyttsx3** 扬声器引擎，循环浏览每一页。它将从 pdf 页面中大声读出文本。

循环处理 pdf 文件中的每一页，最后停止 pyttsx3 扬声器引擎。

```
for page_num in range(pdfReader.numPages):
    text =  pdfReader.getPage(page_num).extractText()
    speaker.say(text)
    speaker.runAndWait()
speaker.stop()
```

## 全面实施:

# 改变 **pyttsx3** 扬声器的声音、速率和音量:

你可以调整说话的速度和音量，并根据你的要求将配音从男声改为女声，反之亦然。

## 速率:

初始化 pyttsx3 库，使用`getProperty(‘rate’)`获取当前的语速。使用`setProperty(‘rate’, x)`改变说话速度，其中 x=100 为正常速度(1x)。

```
# Initialize the speaker
speaker = pyttsx3.init()rate = speaker.getProperty('rate')   
print(rate)speaker.setProperty('rate', 125)
```

## 声音:

初始化 pyttsx3 库，使用`getProperty(‘voice’)`获取当前说话者的性别。使用`setProperty(‘voice’, voice[x].id)`改变说话者的性别，其中 x=0 表示男性，x=1 表示女性。

```
voices = speaker.getProperty('voices')
print(voices)#changing index, changes voices, 0 for male
speaker.setProperty('voice', voices[0].id)#changing index, changes voices, 1 for female
speaker.setProperty('voice', voices[1].id)
```

## 音量:

初始化 pyttsx3 库，并使用`getProperty(‘volume’)`获取当前卷。使用`setProperty(‘volume’, x)`改变扬声器的音量。音量范围从 0 到 1，其中 0 是最小音量，1 是最大音量。

```
volume = engine.getProperty('volume')
print(volume)engine.setProperty('volume',1.0)
```

## 将语音保存到音频文件:

使用以下方法将音频输出(有声读物)保存到 mp3 文件。

```
engine.save_to_file(text, 'audio.mp3')
engine.runAndWait()
```

# 结论:

在本文中，我们介绍了一个基本有声读物的实现，它可以使用几行 python 代码阅读整本 pdf 书籍。为了获得更好的音频效果，您还可以调整扬声器库的声音、速率和音量。

有声读物的最终音频效果不是很好，因为文本在馈送到扬声器引擎之前需要一些预处理。

# 参考资料:

[1] PyPDF2 文档:[https://pypi.org/project/PyPDF2/](https://pypi.org/project/PyPDF2/)

[2] pyttsx3 文档:[https://pyttsx3.readthedocs.io/en/latest/](https://pyttsx3.readthedocs.io/en/latest/)

> 感谢您的阅读