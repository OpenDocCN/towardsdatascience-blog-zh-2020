# 10 分钟打造多功能语音助手

> 原文：<https://towardsdatascience.com/building-a-multi-functionality-voice-assistant-in-10-minutes-3e5d87e164f0?source=collection_archive---------33----------------------->

## 一步一步的初学者指南，建立您的第一个语音助手，执行 8 个基本和引人注目的功能

![](img/108aeac5154fe27c6803377b64e6925c.png)

图像取自— [图像 1](https://pngarchive.com/public/uploads/preview/man-talking-bubble-11560022724ipfasdgubi.png) 、[图像 2](https://static.botsrv.com/website/img/quriobot_favicon.1727b193.png) 、[图像 3](https://www.pinclipart.com/picdir/big/11-116901_images-for-cartoon-quote-bubble-saying-balloon-clipart.png) 、[图像 4](https://www.plumbingwebmasters.com/site/wp-content/uploads/web-design-coding-and-programming-for-plumbers.png)

如今，人们没有时间手动在互联网上搜索信息或问题的答案，而是希望有人为他们做这件事，就像个人助理一样，听取提供的命令并据此采取行动。由于人工智能，这种个人助理现在可以以语音助理的形式提供给每个人，比人类快得多，也可靠得多。一个甚至能够完成困难任务的助手，比如在线下单、播放音乐、开灯等。仅仅通过听用户的命令

在本文中，我将向您展示如何构建一个语音助手来响应基本的用户查询。读完这篇文章后，你会对什么是网络抓取以及如何用它来构建一个语音助手有一个基本的概念。

注意:要阅读本文，您需要对 Python 语言有一个基本的了解。

# **目录**

1.  语音助手

2.网页抓取

3.履行

# 语音助手

[语音助手](https://en.wikipedia.org/wiki/Virtual_assistant)是一个软件代理，可以根据命令或问题为个人执行任务或服务。一般来说，语音助手对语音命令做出反应，并向用户提供关于他/她的查询的相关信息。

该助手可以理解用户发出的特定命令并做出反应，如在 YouTube 上播放歌曲或了解天气。它将搜索和/或抓取网页以找到对命令的响应，从而满足用户。

目前，语音助手已经能够处理产品订单，回答问题，执行播放音乐或与朋友打电话等操作。

实现的语音助手可以执行以下任务:

1.  提供天气详情
2.  提供电晕更新
3.  提供最新消息
4.  搜索一个词的意思
5.  做笔记
6.  播放 YouTube 视频
7.  在谷歌地图上显示位置
8.  在谷歌浏览器上打开网站

在构建语音助手时，有两个重要的库是你应该考虑的。Python 的 **SpeechRecognition** 包帮助语音助手理解用户。它可以按如下方式实现:

```
import speech_recognition as sr#initalises the recognizer
r1 = sr.Recognizer()#uses microphone to take the input
with sr.Microphone() as source:
    print('Listening..') #listens to the user
    audio = r1.listen(source) #recognises the audio and converts it to text
    audio = r1.recognize_google(audio)
```

Python 的 **pyttsx3** 包通过将文本转换成音频来帮助语音助手响应用户。它可以按如下方式实现:

```
import pyttsx3#initialises pyttsx3
engine = pyttsx3.init('sapi5')#converts the text to audio
engine.say('Hello World')
engine.runAndWait()
```

# **网页抓取**

[网页抓取](https://en.wikipedia.org/wiki/Web_scraping)是指从网站中提取数据。网站上的数据是非结构化的。Web 抓取有助于收集这些非结构化数据，并以结构化形式存储。

网页抓取的一些应用包括:

**搜集社交媒体**如 Twitter，以收集推文和评论来进行情感分析。

**抓取亚马逊等电子商务网站**提取产品信息进行数据分析，预测市场趋势。

**收集电子邮件地址**以收集电子邮件 id，然后发送大量电子邮件用于营销和广告目的。

**抓取谷歌图片**以创建数据集，用于训练机器学习或深度学习模型。

虽然可以手动完成，但是 Python 的库**美汤**让抓取数据变得更加简单快捷。

要通过 python 使用 web 抓取来提取数据，您需要遵循以下基本步骤:

1.  找到您想要抓取的网站的 URL
2.  提取网站的全部代码
3.  检查网站并找到您想要提取的数据
4.  使用 html 标签过滤代码以获得所需的数据
5.  以要求的格式存储数据

# 履行

让我们首先在您的 python 笔记本中导入以下库，如下所示:

```
import requests 
from bs4 import BeautifulSoup 
import re
import speech_recognition as sr 
from datetime import date
import webbrowser
import pyttsx3
```

现在让我们创建我们的主函数，它由一系列 if-else 语句组成，告诉助手在特定条件下如何响应。

```
engine = pyttsx3.init('sapi5')r1 = sr.Recognizer()
with sr.Microphone() as source:
    print('Listening..')
    engine.say('Listening')
    engine.runAndWait()
    audio = r1.listen(source)
    audio = r1.recognize_google(audio) if 'weather' in audio:
        print('..')
        words = audio.split(' ')
        print(words[-1])
        scrape_weather(words[-1]) elif 'covid' in audio:
        print('..')
        words = audio.split(' ')
        corona_updates(words[-1]) elif 'meaning' in audio:
        print('..')
        words = audio.split(' ')
        print(words[-1])
        scrape_meaning(words[-1])

    elif 'take notes' in audio:
        print('..')
        take_notes()
        print('Noted!!')

    elif 'show notes' in audio:
        print('..')
        show_notes()
        print('Done')

    elif 'news' in audio:
        print('..')
        scrape_news()

    elif 'play' in audio:
        print('..')
        words = audio.split(' ')
        print(words[-1])
        play_youtube(audio)

    elif 'open' in audio:
        print('..')
        words = audio.split('open')
        print(words[-1])
        link = str(words[-1])
        link = re.sub(' ', '', link)
        engine.say('Opening')
        engine.say(link)
        engine.runAndWait()
        link = f'[https://{link}.com'](/{link}.com')
        print(link)
        webbrowser.open(link)

    elif 'where is' in audio:
        print('..')
        words = audio.split('where is')
        print(words[-1])
        link = str(words[-1])
        link = re.sub(' ', '', link)
        engine.say('Locating')
        engine.say(link)
        engine.runAndWait()
        link = f'[https://www.google.co.in/maps/place/{link}'](https://www.google.co.in/maps/place/{link}')
        print(link)
        webbrowser.open(link)

    else:
        print(audio)
        print('Sorry, I do not understand that!')
        engine.say('Sorry, I do not understand that!')
        engine.runAndWait()
```

**案例一:**如果用户想了解**天气**，他/她可以问助手*“嘿！孟买今天的天气怎么样？”*

由于音频中出现了单词 *"weather"* ，函数**scrape _ weather(words[-1])**将被调用，参数为 *"Mumbai "。*

让我们来看看这个函数。

```
def scrape_weather(city):
    url = '[https://www.google.com/search?q=accuweather+'](https://www.google.com/search?q=accuweather+') + city
    page = requests.get(url)soup = BeautifulSoup(page.text, 'lxml')links = [a['href']for a in soup.findAll('a')]
    link = str(links[16])
    link = link.split('=')
    link = str(link[1]).split('&')
    link = link[0]

    page = requests.get(link, headers={'User-Agent': 'Mozilla/5.0'})soup = BeautifulSoup(page.content, 'lxml') 

    time = soup.find('p', attrs = {'class': 'cur-con-weather-card__subtitle'})
    time = re.sub('\n', '', time.text)
    time = re.sub('\t', '', time)
    time = 'Time: ' + timetemperature = soup.find('div', attrs = {'class':'temp'})
    temperature = 'Temperature: ' + temperature.text

    realfeel = soup.find('div', attrs = {'class': 'real-feel'})
    realfeel = re.sub('\n', '',realfeel.text)
    realfeel = re.sub('\t', '',realfeel)
    realfeel = 'RealFeel: ' + realfeel[-3:] + 'C'climate = soup.find('span', attrs = {'class':'phrase'})
    climate = "Climate: " + climate.text

    info = 'For more information visit: ' + link 

    print('The weather for today is: ')
    print(time)
    print(temperature)
    print(realfeel)
    print(climate)
    print(info)
    engine.say('The weather for today is: ')
    engine.say(time)
    engine.say(temperature)
    engine.say(realfeel)
    engine.say(climate)
    engine.say('For more information visit accuweather.com' )
    engine.runAndWait()
```

我们将使用“*accuweather.com”*网站搜集所有与天气相关的信息。函数 **request.get(url)** 向使用 **BeautifulSoup(page.text，' lxml')** 提取完整 HTML 代码的 url 发送 get 请求

一旦提取了代码，我们将检查代码以找到感兴趣的数据。例如，温度的数值以下列格式表示

```
<div class = "temp">26°</div>
```

可以使用提取

```
soup.find('div', attrs = {'class':'temp'})
```

类似地，我们提取时间、真实感觉和气候，并使用 engine.say()让助手对用户做出响应。

**案例二:**如果用户想要当前**新冠肺炎**的更新，他/她可以问助手*嘿！你能给我印度的 COVID 更新吗？”*或*“嘿！你能给我世界的 COVID 更新吗？”*

由于单词*“covid”*出现在音频中，函数**corona _ updates(words[-1])**将被调用，参数为*“印度”或“世界”*

让我们来看看这个函数。

```
def corona_updates(audio):

    audio = audiourl = '[https://www.worldometers.info/coronavirus/'](https://www.worldometers.info/coronavirus/')
    page = requests.get(url)soup = BeautifulSoup(page.content, 'lxml')totalcases = soup.findAll('div', attrs =  {'class': 'maincounter-number'})
    total_cases = []
    for total in totalcases:
        total_cases.append(total.find('span').text)world_total = 'Total Coronavirus Cases: ' + total_cases[0]
    world_deaths = 'Total Deaths: ' + total_cases[1]
    world_recovered = 'Total Recovered: ' + total_cases[2]

    info = 'For more information visit: ' + '[https://www.worldometers.info/coronavirus/#countries'](https://www.worldometers.info/coronavirus/#countries')if 'world' in audio:
        print('World Updates: ')
        print(world_total)
        print(world_deaths)
        print(world_recovered)
        print(info)else:
        country = audiourl = '[https://www.worldometers.info/coronavirus/country/'](https://www.worldometers.info/coronavirus/country/') + country.lower() + '/'
        page = requests.get(url)soup = BeautifulSoup(page.content, 'lxml')totalcases = soup.findAll('div', attrs =  {'class': 'maincounter-number'})
        total_cases = []
        for total in totalcases:
            total_cases.append(total.find('span').text)total = 'Total Coronavirus Cases: ' + total_cases[0]
        deaths = 'Total Deaths: ' + total_cases[1]
        recovered = 'Total Recovered: ' + total_cases[2]info = 'For more information visit: ' + urlupdates = country + ' Updates: 'print(updates)
        print(total)
        print(deaths)
        print(recovered)
        print(info)

        engine.say(updates)
        engine.say(total)
        engine.say(deaths)
        engine.say(recovered)
        engine.say('For more information visit: worldometers.info')
        engine.runAndWait()
```

我们将使用网站“[world ometers . info](https://www.worldometers.info/coronavirus/country/')*”*搜集所有与日冕相关的信息。函数 **request.get(url)** 向使用 **BeautifulSoup(page.text，' lxml')** 提取完整 HTML 代码的 url 发送 get 请求

一旦提取了代码，我们将检查代码以找到总电晕情况、总恢复和总死亡的数值。

这些值存在于具有类“maincounter-number”的 div 范围内，如下所示。

```
<div id="maincounter-wrap" style="margin-top:15px">
<h1>Coronavirus Cases:</h1>
<div class="maincounter-number">
<span style="color:#aaa">25,091,068 </span>
</div>
</div>
```

这些可以提取如下。

```
totalcases = soup.findAll('div', attrs =  {'class': 'maincounter-number'})
    total_cases = []
    for total in totalcases:
        total_cases.append(total.find('span').text)
```

我们首先找到所有具有类“maincounter-number”的 div 元素。然后我们遍历每个 div 以获得包含数值的跨度。

案例三:如果用户想了解**新闻**，可以问助理*“嘿！你能告诉我最新的消息吗？”*

由于音频中出现了单词*“news”*，因此将调用函数 **scrape_news()** 。

```
def scrape_news():
    url = '[https://news.google.com/topstories?hl=en-IN&gl=IN&ceid=IN:en](https://news.google.com/topstories?hl=en-IN&gl=IN&ceid=IN:en) '
    page = requests.get(url)
    soup = BeautifulSoup(page.content, 'html.parser')
    news = soup.findAll('h3', attrs = {'class':'ipQwMb ekueJc RD0gLb'})
    for n in news:
        print(n.text)
        print('\n')
        engine.say(n.text)
    print('For more information visit: ', url)
    engine.say('For more information visit google news')
    engine.runAndWait()
```

我们将使用“[谷歌新闻](https://news.google.com/topstories?hl=en-IN&gl=IN&ceid=IN:en)”来抓取新闻标题。函数 **request.get(url)** 向使用 **BeautifulSoup(page.text，' lxml')** 提取完整 HTML 代码的 url 发送 get 请求

一旦提取了代码，我们将检查代码以找到最新新闻的标题。

这些标题出现在 h3 标记的 href 属性中，具有如下所示的类“ipQwMb ekueJc RD0gLb”。

```
<h3 class="ipQwMb ekueJc RD0gLb"><a href="./articles/CAIiEA0DEuHOMc9oauy44TAAZmAqFggEKg4IACoGCAoww7k_MMevCDDW4AE?hl=en-IN&amp;gl=IN&amp;ceid=IN%3Aen" class="DY5T1d">Rhea Chakraborty arrest: Kubbra Sait reminds ‘still not a murderer’, Rhea Kapoor says ‘we settled on...</a></h3>
```

我们首先找到所有具有类“ipQwMb ekueJc RD0gLb”的 h3 元素。然后，我们遍历每个元素以获取 href 属性中的文本(新闻标题)。

案例四:如果用户想知道任意单词的**含义**，可以问助手*“嘿！刮是什么意思？”*

由于音频中出现了单词*，意思是*，函数 scrape_meaning(单词[-1])将被调用，参数为*“scraping”*

让我们来看看这个函数。

```
def scrape_meaning(audio):
    word = audio
    url = '[https://www.dictionary.com/browse/'](https://www.dictionary.com/browse/') + word
    page = requests.get(url)
    soup = BeautifulSoup(page.content, 'html.parser')
    soup
    meanings = soup.findAll('div', attrs =  {'class': 'css-1o58fj8 e1hk9ate4'})
    meaning = [x.text for x in meanings]
    first_meaning = meaning[0]
    for x in meaning:
        print(x)
        print('\n')
    engine.say(first_meaning)
    engine.runAndWait()
```

我们将使用网站“[*”*](https://www.dictionary.com/browse/)*”*来刮取意思。函数 **request.get(url)** 向使用 **BeautifulSoup(page.text，' lxml')** 提取完整 HTML 代码的 url 发送 get 请求

一旦提取了代码，我们将检查代码，找到所有包含作为参数传递的单词含义的 html 标签。

这些值存在于具有类“css-1o58fj8 e1hk9ate4”的 div 中，如下所示。

```
<div value="1" class="css-kg6o37 e1q3nk1v3"><span class="one-click-content css-1p89gle e1q3nk1v4" data-term="that" data-linkid="nn1ov4">the act of a person or thing that <a href="/browse/scrape" class="luna-xref" data-linkid="nn1ov4">scrapes</a>. </span></div>
```

我们首先找到所有具有类“css-1o58fj8 e1hk9ate4”的 div 元素。然后，我们遍历每个元素以获取 div 中的文本(单词的含义)。

情况 5:如果用户想让助手**做笔记，**他/她可以问助手*“嘿！你能帮我记笔记吗？”*

由于单词*“做笔记”*出现在音频中，将调用函数 take_notes() 。

让我们来看看这个函数。

```
def take_notes():r5 = sr.Recognizer()  
    with sr.Microphone() as source:
        print('What is your "TO DO LIST" for today')
        engine.say('What is your "TO DO LIST" for today')
        engine.runAndWait()
        audio = r5.listen(source)
        audio = r5.recognize_google(audio)
        print(audio)
        today = date.today()
        today = str(today)
        with open('MyNotes.txt','a') as f:
            f.write('\n')
            f.write(today)
            f.write('\n')
            f.write(audio)
            f.write('\n')
            f.write('......')
            f.write('\n')
            f.close() 
        engine.say('Notes Taken')
        engine.runAndWait()
```

我们首先初始化识别器，向用户询问他们的“任务清单”。然后我们听用户说话，并使用 *recognize_google 来识别音频。现在我们将打开一个名为“MyNotes.txt”的记事本，记下用户给出的笔记和日期。*

然后，我们将创建另一个名为 show_notes()的函数，它将从名为“MyNotes.txt”的记事本中读出今天的笔记/待办事项列表。

```
def show_notes():
    with open('MyNotes.txt', 'r') as f:
        task = f.read()
        task = task.split('......')
    engine.say(task[-2])
    engine.runAndWait() 
```

案例六:如果用户想**播放 YouTube 视频，**他/她可以问助手*“嘿！能不能玩催眠？”*

由于音频中出现了单词*“play”*，因此将调用函数 play _ YouTube(words[-1])*，并传递“催眠”作为参数。*

*让我们来看看这个函数。*

```
*def play_youtube(audio):url = '[https://www.google.com/search?q=youtube+'](https://www.google.com/search?q=youtube+') + audio
    headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36'
    }
    engine.say('Playing')
    engine.say(audio)
    engine.runAndWait()
    page = requests.get(url, headers=headers)
    soup = BeautifulSoup(page.content, 'html.parser')
    link = soup.findAll('div', attrs = {'class':'r'})
    link = link[0]
    link = link.find('a')
    link = str(link)
    link = link.split('"')
    link = link[1]webbrowser.open(link)*
```

*我们将使用 G [oogle Videos](https://www.google.com/search?q=youtube+) 来搜索视频标题，并打开第一个链接来播放 YouTube 视频，该视频出现在具有类“r”的 div 元素中。*

*情况 7:如果用户想要**搜索位置，**他/她可以问助手*“嘿！IIT 孟买在哪里？”**

*由于单词*“where is”*出现在音频中，下面的代码将被执行。*

*(这段代码出现在主函数的 if-else 循环中)*

```
*elif 'where is' in audio:
        print('..')
        words = audio.split('where is')
        print(words[-1])
        link = str(words[-1])
        link = re.sub(' ', '', link)
        engine.say('Locating')
        engine.say(link)
        engine.runAndWait()
        link = f'[https://www.google.co.in/maps/place/{link}'](https://www.google.co.in/maps/place/{link}')
        print(link)
        webbrowser.open(link)*
```

*我们将加入谷歌地图链接用户提供的位置，并使用 webbrowser.open(链接)打开链接定位' *IIT 孟买'。**

*案例 8:如果用户想**打开一个网站，**他/她可以问助手*嘿！能不能向数据科学开放？”**

*由于单词*“打开”*出现在音频中，下面的代码将被执行。*

*(这段代码出现在主函数的 if-else 循环中)*

```
*elif 'open' in audio:
        print('..')
        words = audio.split('open')
        print(words[-1])
        link = str(words[-1])
        link = re.sub(' ', '', link)
        engine.say('Opening')
        engine.say(link)
        engine.runAndWait()
        link = f'[https://{link}.com'](/{link}.com')
        print(link)
        webbrowser.open(link)*
```

*我们将把用户提供的网站名称与任何 URL 的标准格式连接起来，即[https://{网站名称}。com](/{link}.com') 打开网站。*

*这就是我们如何创建一个简单的语音助手。您可以修改代码以添加更多功能，如执行基本的数学计算、讲笑话、创建提醒、更改桌面壁纸等。*

*你可以在下面链接的我的 GitHub 资源库中找到完整的代码。*

*[](https://github.com/sakshibutala/VoiceAssistant) [## sakshibutala/语音助手

### 在这个笔记本中，我建立了一个语音助手，可以回答基本的询问。“VoiceAssistant.ipynb”包含…

github.com](https://github.com/sakshibutala/VoiceAssistant) 

# **参考文献**

[](https://www.parsehub.com/blog/what-is-web-scraping/) [## 什么是网页抓取，它是用来做什么的？ParseHub

### 一些网站可能包含大量宝贵的数据。股票价格，产品详情，体育统计，公司…

www.parsehub.com](https://www.parsehub.com/blog/what-is-web-scraping/) [](https://www.edureka.co/blog/web-scraping-with-python/) [## 使用 Python 进行 Web 抓取-初学者指南| Edureka

### 使用 Python 进行 Web 抓取想象一下，您必须从网站中提取大量数据，并且您希望尽可能快地完成这些工作…

www.edureka.co](https://www.edureka.co/blog/web-scraping-with-python/) [](https://onlim.com/en/what-are-voice-assistants-and-how-do-they-work/) [## 什么是语音助手，它们是如何工作的？Onlim

### 科幻电影中的场景，我们回到家，开始对着我们的个人家用电脑说话…

onlim.com](https://onlim.com/en/what-are-voice-assistants-and-how-do-they-work/)*