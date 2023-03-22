# 使用 Mozilla DeepSpeech 为任何视频文件生成字幕

> 原文：<https://towardsdatascience.com/generating-subtitles-automatically-using-mozilla-deepspeech-562c633936a7?source=collection_archive---------10----------------------->

## 对于那些嘴里薯条的噪音让你无法看电影的时候:)

![](img/a5146d2623793743586bd1b852daccc9.png)

玛利亚·特内娃在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在 OTT 平台时代，仍然有一些人喜欢从 YouTube/脸书/Torrents 下载电影/视频(嘘🤫)过流。我就是其中之一，有一次，我找不到我下载的某部电影的字幕文件。然后， **AutoSub** 的想法打动了我，因为我以前和 [DeepSpeech](https://github.com/mozilla/DeepSpeech) 合作过，我决定用它来为我的电影制作字幕。

给定一个视频文件作为输入，我的目标是生成一个**。srt** 文件。字幕可以导入任何现代视频播放器。在本文中，我将带您浏览一些代码。你可以在我的 GitHub [上找到这个项目，这里有关于如何在本地安装的说明。](https://github.com/abhirooptalasila/AutoSub)

先决条件:对 Python 的中级理解，对[自动语音识别引擎](https://heartbeat.fritz.ai/a-2019-guide-for-automatic-speech-recognition-f1e1129a141c)的一些熟悉，以及对信号处理的基本理解将会很棒。

注意:这是我第一篇关于媒体的文章。如果你有任何建议/疑问，请写下来。快乐阅读:)

# Mozilla DeepSpeech

DeepSpeech 是基于百度原创[深度语音研究论文的开源语音转文本引擎。鉴于其多功能性和易用性，它是最好的语音识别工具之一。它是使用](https://arxiv.org/abs/1412.5567) [Tensorflow 构建的，](https://github.com/tensorflow/tensorflow)可以使用自定义数据集进行训练，在庞大的 [Mozilla Common Voice](https://commonvoice.mozilla.org/en) 数据集上进行训练，并在 Mozilla Public License 下获得许可。最大的好处是我们可以下载模型文件并在本地执行推理**只需几分钟！**

虽然，DeepSpeech 确实有它的问题。该模型与非母语英语口音的语音进行斗争。对此有一个解决方法——使用我们想要预测的语言中的自定义数据集来微调模型。我将很快就如何做到这一点写另一篇文章。

如果你正在处理语音识别任务，我强烈建议你看看 DeepSpeech。

# 自动 Sub

让我们从安装一些我们需要的包开始。所有命令都已经在 Ubuntu 18.04 的 pip 虚拟环境中进行了测试。

1.  FFmpeg 是领先的多媒体框架，能够解码、编码、转码、复用、解复用、流式传输、过滤和播放人类和机器创造的几乎任何东西。我们需要它从我们的输入视频文件中提取音频。

```
$ sudo apt-get install ffmpeg
```

2. **DeepSpeech** :从 PyPI 安装 python 包，下载模型文件。记分员文件是可选的，但大大提高了准确性。

```
*$* pip install deepspeech*==0.8.2**# Model file (~190 MB)* 
$ wget https://github.com/mozilla/DeepSpeech/releases/download/v0.8.2/deepspeech-0.8.2-models.pbmm*# Scorer file (~900 MB)* $ wget [https://github.com/mozilla/DeepSpeech/releases/download/v0.8.2/deepspeech-0.8.2-models.scorer](https://github.com/mozilla/DeepSpeech/releases/download/v0.8.2/deepspeech-0.8.2-models.scorer)
```

现在我们都设置好了，让我们首先使用 FFmpeg 从我们的输入视频文件中提取音频。我们需要创建一个子进程来运行 UNIX 命令。DeepSpeech 期望输入音频文件以 16kHz 采样，因此 ffmpeg 的参数如下。

现在，假设我们的输入视频文件有 2 小时长。通常不建议对整个文件运行 DeepSpeech 推断。我试过了，效果不太好。解决这个问题的一个方法是将音频文件分割成无声片段。分割后，我们有多个小文件包含我们需要推断的语音。这是使用 [pyAudioAnalysis](https://github.com/tyiannak/pyAudioAnalysis) 完成的。

以下函数使用 pyAudioAnalysis 中的函数 **read_audio_file()** 和 **silenceRemoval()** ，并从语音开始和结束的位置生成分段限制。参数以秒为单位控制平滑窗口大小，以(0，1)为单位控制权重因子。使用段限制，较小的音频文件被写入磁盘。

我们现在需要分别对这些文件运行 DeepSpeech 推断，并将推断的文本写入一个 SRT 文件。让我们首先创建一个 DeepSpeech 模型的实例，并添加 scorer 文件。然后，我们将音频文件读入一个 NumPy 数组，并将其送入语音转文本功能以产生推断。如上所述，pyAudioAnalysis 保存的文件具有以秒为单位的段限制时间。在写入 SRT 文件之前，我们需要提取这些限制并将其转换成合适的形式。这里的定义了写功能[。](https://github.com/abhirooptalasila/AutoSub/blob/4f65218ee40207e1ccf3ec3ed49fa9dd300721f4/autosub/writeToFile.py#L7)

整个过程不应超过原始视频文件持续时间的 60%。这里有一个视频，展示了在我的笔记本电脑上运行的一个例子。

自动 Sub 演示

还有一个领域我希望在未来改进。推断的文本是无格式的。我们需要添加适当的标点符号，纠正单词中可能的小错误(一个字母之外)，并将很长的片段分成较小的片段(尽管这很难自动化)。

就是这样！如果您已经到达这里，感谢您的坚持:)

你可以在这里找到我: [LinkedIn](https://www.linkedin.com/in/abhiroop1999/) ， [GitHub](https://github.com/abhirooptalasila)