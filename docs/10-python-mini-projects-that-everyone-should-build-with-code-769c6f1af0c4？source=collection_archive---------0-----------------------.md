# 每个人都应该构建的 10 个 Python 迷你项目(带代码)

> 原文：<https://towardsdatascience.com/10-python-mini-projects-that-everyone-should-build-with-code-769c6f1af0c4?source=collection_archive---------0----------------------->

## 今天你能在互联网上找到的最好的 python 迷你项目

![](img/0ecb5ac63a90b3c725cc1e2a7b2ef848.png)

戴维·克洛德在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

你好，我是阿沛

我们在 2020 年。我们都知道这个行业正在日益发展。如果你看到从 2013 年到 2019 年，python 在行业中的增长在 40%左右，据说在未来几年将增长 20%以上。在过去的几年中，python 开发者的增长率增加了 30%,所以没有比学习 python 更好的时间了，也没有比做项目更好的方法了。

**在这个博客中，我们将创建 10 个 python 迷你项目。**

1.  ***掷骰模拟器***
2.  ***猜数字游戏***
3.  ***随机密码生成器***
4.  ***二分搜索法***
5.  ***使用 python 发送邮件***
6.  ***中型文章阅读器***
7.  ***闹钟***
8.  ***网址缩写***
9.  ***天气 app***
10.  ***按键记录器***

![](img/ea0ea6335cb6979a7500a53e49848dea.png)

照片由 [Riz Mooney](https://unsplash.com/@rizmooney?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 掷骰模拟器

> 目标是创建一个模拟掷骰子的程序。

**主题:**随机模块、循环和 if-else

**提示:** *使用随机模块生成一个用户想要的 1 到 6 之间的随机数。*

**自己试试**

```
import random 
while True:     
     print(''' 1\. roll the dice             2\. exit     ''')    
     user = int(input("what you want to do\n"))     
     if user==1:         
        number = random.randint(1,6)         
        print(number)     
     else:         
        break
```

# ***猜数字游戏***

> 该项目的主要目标是创建一个程序，在一个范围内随机选择一个数字，然后用户必须猜测这个数字。用户有三次机会猜测数字，如果他猜对了，然后打印一条消息说“你猜对了”，否则打印一条负面消息。

主题:随机模块，for 循环，f 字符串

**自己试一试**

```
import random
number = random.randint(1,10)
for i in range(0,3):
    user = int(input("guess the number"))
    if user == number:
        print("Hurray!!")
        print(f"you guessed the number right it's {number}")
        break
if user != number:
    print(f"Your guess is incorrect the number is {number}")
```

# ***随机密码生成器***

> 创建一个程序，接受一个数字并生成该数字的随机密码长度。

主题:随机模块，连接字符串，接受输入

提示:*创建一个包含所有字符的字符串，然后从中随机抽取字符，并将每个字符连接起来，形成一个大字符串。*

**自己试试**

```
import random
passlen = int(input("enter the length of password"))
s="abcdefghijklmnopqrstuvwxyz01234567890ABCDEFGHIJKLMNOPQRSTUVWXYZ!@#$%^&*()?"
p =  "".join(random.sample(s,passlen ))
print (p)
```

# ***二分搜索法***

> 二分搜索法的目标是搜索一个给定的数字是否出现在字符串中。

主题:列表、排序、搜索

提示:*先检查中间是否有，再检查前后。*

**自己尝试一下**

```
lst = [1,3,2,4,5,6,9,8,7,10]
lst.sort()
first=0
last=len(lst)-1
mid = (first+last)//2
item = int(input("enter the number to be search"))
found = False
while( first<=last and not found):
    mid = (first + last)//2
    if lst[mid] == item :
         print(f"found at location {mid}")
         found= True
    else:
        if item < lst[mid]:
            last = mid - 1
        else:
            first = mid + 1 

if found == False:
    print("Number not found")
```

# ***使用 python 发送邮件***

> 创建一个可以发送电子邮件的 python 脚本

主题:导入包，SMTP 库，向服务器发送请求

**自己试一试**

```
import smtplib from email.message import EmailMessageemail = EmailMessage() ## Creating a object for EmailMessageemail['from'] = 'xyz name'   ## Person who is sendingemail['to'] = 'xyz id'       ## Whom we are sendingemail['subject'] = 'xyz subject'  ## Subject of emailemail.set_content("Xyz content of email") ## content of emailwith smtlib.SMTP(host='smtp.gmail.com',port=587)as smtp:     
## sending request to server 

    smtp.ehlo()          ## server objectsmtp.starttls()      ## used to send data between server and clientsmtop.login("email_id","Password") ## login id and password of gmailsmtp.send_message(email)   ## Sending emailprint("email send")    ## Printing success message
```

![](img/d616a74135ae4caea392a798e5972009.png)

由 [Fab Lentz](https://unsplash.com/@fossy?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# ***中型文章阅读器***

任务是创建一个可以从链接中读取文章的脚本。

**主题:**网页抓取，文字转语音

**提示:**

**自己试试**

```
import pyttsx3
import requests
from bs4 import BeautifulSoup
engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[0].id)def speak(audio):
  engine.say(audio)
  engine.runAndWait()text = str(input("Paste article\n"))
res = requests.get(text)
soup = BeautifulSoup(res.text,'html.parser')
articles = []
for i in range(len(soup.select('.p'))):
    article = soup.select('.p')[i].getText().strip()
    articles.append(article)
text = " ".join(articles)
speak(text)# engine.save_to_file(text, 'test.mp3') ## If you want to save the speech as a audio fileengine.runAndWait()
```

# ***闹钟***

> 我们的任务是使用 python 构建一个闹钟

主题:日期时间模块，播放声音库

# ***网址缩写***

> 使用 API 创建一个短 URL 的 python 脚本。

要运行上述代码，请执行`python filename.py url`。

```
***python tinyurl_url_shortener.py*** [***https://www.wikipedia.org/***](https://www.wikipedia.org/)
```

# ***天气 app***

> 我们的任务是创建一个能够给出天气信息的 python 脚本。

**主题:**网页抓取，谷歌抓取

****提示:** *使用网页抓取直接从谷歌抓取天气信息。***

**![](img/519bdc60be94bf7a7b0b028d0993985e.png)**

**照片由[在](https://unsplash.com/@drew_beamer?utm_source=medium&utm_medium=referral) [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上绘制光束**

# *****按键记录器*****

> **我们的任务是创建一个按键记录器，可以将按键存储在一个 txt 文件中。**

****主题:** pynput 库，与文件交互**

****提示:**使用 pynput 读取按键，然后保存到一个 txt 文件中。**

**谢谢你的阅读😄**

# **未来阅读**

**[](https://medium.com/@parasharabhay13/5-python-intermediate-project-that-everyone-should-build-with-codes-5039f13feadb) [## 6 每个人都应该构建的 python 中间项目(带代码)

### 今天你能在互联网上找到的最好的 python 项目。

medium.com](https://medium.com/@parasharabhay13/5-python-intermediate-project-that-everyone-should-build-with-codes-5039f13feadb)** 

**感谢你读到这里，如果你喜欢我的内容并想支持我，最好的方式是—**

1.  **跟我上 [***中***。](http://abhayparashar31.medium.com/)**
2.  **在[***LinkedIn***](https://www.linkedin.com/in/abhay-parashar-328488185/)上联系我。**
3.  **使用 [***我的推荐链接***](https://abhayparashar31.medium.com/membership) 用一个披萨的费用成为中等会员。你会费的一小部分会归我。**
4.  **订阅 [***我的邮件列表***](https://abhayparashar31.medium.com/subscribe) 从不会错过我的一篇文章。**