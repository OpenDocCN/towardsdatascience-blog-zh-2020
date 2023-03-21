# COVID19 语音助手

> 原文：<https://towardsdatascience.com/covid19-voice-assistant-63c37b1f02f9?source=collection_archive---------58----------------------->

## 让我们听听这种病毒对我们世界的影响

冠状病毒已经在全球范围内造成了巨大的破坏。数百万人被感染，数千人死亡。疫情仍在上升，因为世界上所有国家都实行了封锁。每个人都有权知道这种病毒是如何影响我们的生活的，因为我们仍在继续与它作战。所以，我写了这篇关于我的项目的文章，用一种声音来表达世界的现状。

![](img/274a9fae2ce56af1ad665a343a7e9bd6.png)

格伦·卡丽在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 语音助手项目

在这个项目中，您将学习如何使用 Python 构建一个 COVID19 语音助手。该项目主要涵盖了 python 中**网页抓取**、**语音辅助**和**多线程**的基础知识。

## 网页抓取

该项目使用了一个名为 [**的工具。ParseHub 是免费的 web scraper，功能非常强大，并且易于使用。这个工具允许你仅仅通过点击你想从那个网站得到的元素来抓取网页。所以，第一步是下载**](https://www.parsehub.com/) **[ParseHub](https://www.parsehub.com/quickstart) 。**

我们将从非常著名的统计网站 [**Worldometers**](https://www.worldometers.info/coronavirus/) 上搜集定期更新的冠状病毒信息。打开 ParseHub - >开始新项目- >粘贴链接到 worldometers。

单击您想要从页面中抓取的元素，并为它们分配唯一的标签。使用相关工具将页面上的“冠状病毒病例”与其编号联系起来。ParseHub 显然非常高效，它使用人工智能来理解您想要哪些元素。要尝试这种方法，将“美国”与“总病例数”联系起来，列表中的其他国家就会显示出来。太神奇了！这在幕后所做的是为上面链接的内容构建一个 JSON 格式。

完成链接后，点击“获取数据”，这可能需要一段时间。然后确保在下一个屏幕上记下 API 密钥、项目令牌和运行令牌。

## 语音助手

复制粘贴 [Part1.py](https://github.com/K-G-PRAJWAL/Python-Projects/blob/master/Coronavirus%20Voice%20Assistant/Part1.py) 中给出的代码，我会带你了解到底发生了什么。

以下几点解释了 Part1.py 文件:

*   这部分项目的需求是以下 python 包: **requests** ， **json** ， **pyttsx3** ，**speecher recognition**， **re** 。因此，pip 安装了所有这些包。
*   在各自的变量中使用 API 密钥、项目令牌和运行令牌。
*   类**数据**有以下方法:

1.  **get_data()** :使用您最近在他们的服务器上运行的项目从 parsehub 获取数据，并将其返回。
2.  **get_total_cases()** :获取新冠肺炎在全球的总病例数。
3.  **get_country_deaths()** :获取全球新冠肺炎死亡总人数。
4.  **get_country_data()** :获取任何特定国家的新冠肺炎病例和死亡人数。
5.  get_list_of_countries() :获取所有国家的列表。

*   **speak()** 方法初始化 pyttsx3 引擎，说出作为参数传递给它的文本。
*   **get_audio()** 方法使用 google 的 **recognize_google()** 方法监听用户通过麦克风输入的语音，并返回输入的文本。
*   最后，main()函数负责使用正则表达式模式识别输入语音，并将它们分类为世界上的病例和死亡或任何国家的病例和死亡的模式。
*   匹配的模式调用特定的函数并大声说出数字。

## 多线程

到目前为止，您的助手只说出您最初获取的数据。保持数据更新是很重要的，因为这个全球疫情仍在进行，当你的助手说数字正在下降时，这将是令人欣慰的。

删除 Part1.py 并将 [Part2.py](https://github.com/K-G-PRAJWAL/Python-Projects/blob/master/Coronavirus%20Voice%20Assistant/main.py) 复制到您的应用程序中。

除了 Part1.py 之外，Part2.py 还有一个额外的函数和一个处理程序。这个函数就是 **update()** 函数。该函数不从 parsehub 上的最后一次运行中获取数据，而是在 parsehub 服务器上为您的项目初始化一次新的运行。这将需要一段时间，所以请在更新时给它一些时间。

最有趣的部分是减少这个时间。因此，我们使用 python 中的**多线程**库来利用多线程。通过这样做，我们可以确保语音助手在一个线程上运行，而数据更新在另一个线程上并行进行。

## 结果

这个项目最有趣的部分是它的结果。

运行 Part2.py 文件，当控制台显示“Listening…”时，说“number of total cases ”,助手将回复全世界的 COVID19 病例数。现在说，“印度总死亡人数”，助手会回复这个数字。

您可以说，“更新”，助手将为您更新这些值，这可能需要一些时间。请耐心等待。

你必须说“停止”才能退出正在运行的应用程序。

😲耶！！！你做到了！你刚刚建立了一个语音助手，只需使用你的声音就可以更新关于冠状病毒的信息。你可以进一步把这个项目发展成惊人的东西。要获得完整的见解，请查看我的 GitHub。

如果你有任何错误，请随时通过我的 [LinkedIn](https://www.linkedin.com/in/k-g-prajwal-a6b3b517a/) 联系我。

希望你喜欢并理解这个项目。

谢谢你。