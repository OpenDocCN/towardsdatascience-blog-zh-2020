# Pomodoro 命令行定时器

> 原文：<https://towardsdatascience.com/pomodoro-command-line-timer-439b1b07161f?source=collection_archive---------35----------------------->

![](img/077939b48d5fd61181af49f7ae56ede4.png)

[@carlheyerdahl](https://unsplash.com/@carlheyerdahl) Unsplash

## 用 python 提高生产力

在本文中，您将了解到

1.  番茄工作法
2.  如何使用 python 对番茄工作法建模
3.  如何在命令行中创建动态计时器
4.  如何用 python 播放声音

# 番茄工作法简介

番茄工作法是用来把工作分成几个间隔，中间有短暂的休息。它是由弗朗西斯科·西里洛在 20 世纪 80 年代末创建的，在过去的 40 年里，它已经为那些想要提高连续几个小时有效学习而不感到明显疲劳的能力的人取得了巨大的成功。

这项技术的关键是间隔时间不要太长，以免疲劳，但也不要太短，以便进行深度工作。传统上，工作间隔是 25 分钟，之后有 5 分钟的休息时间。这些间隔和休息循环进行，直到任务完成。

这项技术的目的是减轻对成为的焦虑，提高你的注意力，改善你的工作/学习过程。

欲了解更多信息，请参见弗朗西斯科的[文件。](http://baomee.info/pdf/technique/1.pdf)

在这篇文章中，我们将使用 python 实现一个番茄定时器。我们不需要上发条或者重置计时器。我们可以改变时间间隔来适应我们个人的需要。我们开始吧！

# 规划阶段

让我们获取关于技术信息，并开始计划我们想要创建的程序。

这是我们需要的:

1.  倒计时的计时器
2.  优选地，定时器应该是动态的。也就是说，它在屏幕上变化，而不是程序在倒计时的每一秒钟打印新的行
3.  播放声音的能力，以适应学习时间和休息时间，所以我们知道什么时候回去学习
4.  能够指定我们打算进行的研究时间间隔、休息时间和番茄工作法的次数。

# 进口货

```
import time
import sysfrom playsound import playsound
```

让我们一次分解一个。

时间模块是一个内置的 python 模块，具有与时间相关的功能，我们将使用它来暂停程序的执行。

sys 模块是 python 解释器的另一个内置 python 模块。在这种情况下，我们将使用它来创建计时器的动态部分，以不断更新命令行。

最后导入的是包播放声音。这是一个第三方模块，允许我们使用 python 来指导播放我们选择的特定声音文件。

现在，我们对将要使用的模块有了全面的了解，并且我们知道开始分解代码需要什么。

# 指定学习和休息的时间间隔

大多数程序的开始是初始化变量。这没有什么不同，第一步是指定我们希望程序运行的间隔、休息和总时间的长度。Python 有 input 函数，它允许我们将用户输入存储到变量中。

```
interval = int(input('Please Enter duration (mins) of interval: '))break_duration = int(input('Please Enter break duration(mins) of interval: '))total_duration = int(input('Please enter number of sessions: '))
```

笔记

1.  我们指定了`interval` 、`break_duration`、`total_duration`
2.  使用`int()`我们将用户输入转换成一个数字

# 计时器

我们希望能够使用 python 指定倒计时分钟和秒。我们可以很容易地使用 for 循环倒数秒。但是分分秒秒都倒计时呢？为此，我们需要一个嵌套的 For 循环。

一个很好的类比是时钟。钟的秒针每分钟移动 60 次。每走 60 步，分针就换一次。这正是嵌套 for 循环所做的。对于外部循环的每一次迭代，内部循环完全迭代。让我们看看这在代码中是什么样子的。

```
interval = 5
for i in range(interval,-1,-1): for j in range(59,-1,-1): 

        time.sleep(1)
```

笔记

1.  出于说明的目的，我们指定 5 分钟的研究间隔。

内部循环:

2.内部循环我们使用`range`函数从 60 秒向下迭代到 0 秒。

3.`range(59,-1,-1)`第一个(start)参数从 59 开始，但令人困惑的是第二个(stop)参数是上限，在这种情况下是-1，它不包括这个数字，因此 I 停止在 0。最后一个参数叫做步长参数，我们希望 59 减少 1，因此是-1。这都意味着`i`从 59 倒数到 0。

4.该函数暂停执行一秒钟。在下一次 for 循环迭代之前。

外部循环:

5.对于一个几秒钟的完整循环，我们指定我们的外部循环`range(interval,-1,-1)`这里我们取我们指定的间隔，因此迭代将从 5 开始到我们想要的 0 结束。

这段代码将倒数每分钟的秒数，从 5 分钟开始倒数到 0。厉害！现在让我们让它出现在终端输出上，每秒都在变化。

![](img/7c05dfeeb9c3a611e6d386d5493946f3.png)

[@veri_ivanova](https://unsplash.com/@veri_ivanova) Unsplash

# 使计时器动态化

到目前为止，我们已经讨论了嵌套循环来倒计时秒和分钟。但是我们还没有在命令行中显示这些，在这里我们可以利用 python 的内置模块让我们的计时器在屏幕上倒计时。但重要的是，我们指定我们不希望为每秒钟的倒计时创建一个新行，我们希望终端输出出现在同一行上，并且每秒钟都发生变化。

为此，我们使用了`sys.stdout.write`函数。这将打印出一个终端输出，但与 print 函数不同，我们可以清除输出，而不用换到新的一行。

让我们修改上面的代码来做到这一点

```
for i in range(interval,-1,-1):
    for i in range(59,-1,-1):
        sys.stdout.write(f'\rDuration: Minutes {j} Seconds {i} to go')

        time.sleep(1)
        sys.stdout.flush()
```

笔记

1.  我们有和以前一样的嵌套 for 循环，但是做了一些修改
2.  `sys.stdout.write()`功能是在终端屏幕上打印信息的另一种方式，但有所不同。与打印功能不同，我们可以在屏幕上更改信息，而无需另起一行。这是我们计划的关键！
3.  我们使用 f 字符串打印出一个字符串，但是 f 字符串允许我们根据`i`和`j`的值改变字符串。
4.  注意在我们的写函数中，`\r`。这是一个转义字符，用于将输出重置到行首。这对于保持我们在终端行的相同部分输出的字符串是必要的，这是我们如何使我们的定时器动态的一部分。字符串中唯一改变的部分是 I 和 j 的值，它们对应于我们的分和秒。
5.  `sys.stdout.flush()`清除之前打印的行，准备打印另一个报表。这意味着我们可以动态地改变计时器，对于屏幕上的每一秒钟，只要一秒钟的时间过去，我们就刷新它。

我们做到了！我们现在有一种方法来创建每秒钟动态变化的命令行计时器。

# Python 和声音文件

Python 有很多播放声音的模块。最简单的一个是名为`playsound`的第三方模块。使用`playsound`功能允许我们指定要播放的文件。

```
playsound(r'c:\windows\media\alarm02.wav')
```

笔记

1.  我们正在播放一个标准的 Windows 声音文件。注意，我们使用的是被称为原始字符串的`r`，因为我们希望将`\`包含在字符串中。通常`\`不会包含在字符串输出中，因为它被称为转义字符，可以用于在字符串中打印换行符，例如`\n`。

![](img/b34b14c2b5ee6b53b796fb20ffc4efbe.png)

[@feelfarbig](https://unsplash.com/@feelfarbig) Unsplash

# 设置程序

在编写脚本时，如果合适的话，从函数或类的角度来考虑是一个好习惯。我们几乎拥有了创建这个脚本的所有部分。但是我们需要协调所有我们想要添加的潜在功能和声音。

让我们一次创建一个函数。如果你对函数不熟悉，可以在这里看到这篇详细的文章[。](https://realpython.com/defining-your-own-python-function/)

## 用于设置时间的功能

```
def user_input():
    interval = int(input('Please Enter duration (mins) of interval: ')) break_duration = int(input('Please Enter break duration(mins) of interval: ')) total_duration = int(input('Please enter number of sessions: '))
    return interval, duration, break_session
```

这里我们设置了制作番茄定时器的用户输入。我们已经看到了更简单格式的代码。

## 倒计时定时器

```
def countdown(interval):
    for i in range(interval,-1,-1):
        for j in range(59,-1,-1):
            sys.stdout.write(f'\rDuration: \
            Minutes {j} Seconds {i} to go')
            time.sleep(1)
            sys.stdout.flush()
```

这是我们在上一节中看到的代码，但是不同之处在于我们封装到了一个函数中！

# 设置

这个脚本的最后一部分是协调这些功能。我们想设置一种方法来打印我们当前正在进行的会话以及何时停止。所以设置这些，并使用 while 循环继续运行，直到满足一个条件，这就是我们如何保持计时器连续运行。

```
if __name__ == "__main__": session_count = 0 interval, total_duration, break_duration = user_input() while session_count < total_duration: playsound(r'c:\windows\media\alarm02.wav') countdown(interval) playsound(r'c:\windows\media\alarm02.wav') print('\nBreak time!') countdown(break_duration) session_count += 1 print('\nsession number: ',session_count) print('\nEnd of Session!')
```

笔记

1.  如果你不熟悉`if __name__ = "__main__"`，请看这里的。这是维护代码只在这个脚本中运行的一种方式。
2.  我们设置了`session_count`的初始值，它将在 while 循环的每次迭代中改变
3.  我们允许用户定义间隔的长度，休息时间以及我们想要做多少番茄大战
4.  我们在间隔开始和结束时播放声音
5.  然后我们在 while 循环的每一次迭代中把`session_count`加 1。`+=`是`session_count + 1`的简写，它被称为增广算子。然后我们打印`session_count`，这样我们就可以看到我们正在进行哪个会话。
6.  当会话计数大于持续时间时，while 循环完成，我们打印会话已经结束。

# 轻微修改

在测试这个脚本时，当倒计时从 10 秒切换到 9 秒时，终端中出现的文本有一个小问题。

10 秒钟后，终端输出如下所示

```
Duration: Minute 1 Seconds 10 to go
```

在 9 秒钟时，终端输出如下所示

```
Duration: Minute 1 seconds 9 seconds to goo
```

这是因为我们在同一行上不断刷新终端输出。但是屏幕上的字符数变了。下到 9 秒就少了一个字符。因此，尽管大部分字符会刷新，但 10 秒后输出的最后一个字符(在本例中为“0”)会保留在终端输出中。

为了解决这个问题，我们必须稍微改变计时器，当计时器倒数到 9 秒或更少时。

```
def countdown(interval):
    for j in range(interval-1,-1,-1):
        for i in range(59,-1,-1):
            if i <= 9:
                sys.stdout.write \
                (f'\rDuration: Minutes {j} Seconds 0{i} to go') else:
                sys.stdout.write \
                (f'\rDuration: Minutes {j} Seconds {i} to go')

            time.sleep(1)
            sys.stdout.flush()
```

我们包含了一个 if 语句，当 I 小于或等于 9 时，我们将秒计数的终端输出从 9 秒改为 09 秒。对于每次循环迭代，终端输出的字符数与倒数 10 秒时的终端输出的字符数相同。

完整代码请见[这里](https://github.com/medic-code/timer/blob/timer/timer.py)

# 摘要

我们已经用 python 创建了一个自动化的生产力黑客！我们已经学习了如何在终端中创建一个动态计时器，以及如何在编码之前开始考虑构建脚本。

希望，这将有助于你的学习，并保持你在使用番茄工作法的轨道上！

# 其他文章

[](/approach-to-learning-python-f1c9a02024f8) [## 学习 Python 的方法

### 今天如何最大限度地学习 python

towardsdatascience.com](/approach-to-learning-python-f1c9a02024f8) [](/how-to-run-scrapy-from-a-script-ff07fd6b792b) [## 如何从脚本运行 Scrapy

### 忘记 scrapy 的框架，全部用使用 scrapy 的 python 脚本编写

towardsdatascience.com](/how-to-run-scrapy-from-a-script-ff07fd6b792b) [](/efficient-web-scraping-with-scrapy-571694d52a6) [## 使用 Scrapy 进行有效的网页抓取

### Scrapy 的新功能使您的刮削效率

towardsdatascience.com](/efficient-web-scraping-with-scrapy-571694d52a6) 

# 关于作者

我是一名执业医师和教育家，也是一名网站开发者。

请点击此处查看我的博客和其他帖子中关于项目的更多细节。更多技术/编码相关内容，请点击这里[订阅我的时事通讯](https://aaronsmith.substack.com/p/coming-soon?r=6yuie&utm_campaign=post&utm_medium=web&utm_source=copy)

我将非常感谢任何评论，或者如果你想与 python 合作或需要帮助，请联系我。如果你想和我联系，请在这里或者在[推特](http://www.twitter.com/@aaronsmithdev)上联系我。