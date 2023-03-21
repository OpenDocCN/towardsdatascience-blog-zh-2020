# 用 Python 构建个性化智能闹钟

> 原文：<https://towardsdatascience.com/build-a-personalized-smart-alarm-with-python-ceae3682a761?source=collection_archive---------16----------------------->

## 让我们建立一个基于人工智能技术的个性化智能闹钟，它将为你设置最佳的闹钟铃声，帮助你早点醒来。

![](img/78c51722d93785860326cacb1d5f1284.png)

布兰迪·里德在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

你好，读者，你经常会看到或使用 Python 语言创建闹钟，它会帮助你醒来或提醒你一个重要的会议。

几乎所有这些都很简单，没有任何智能，它们所做的只是播放你设置的闹钟或选择一个随机的 YouTube 视频或歌曲来播放。

所以，让我们更上一层楼，建造一些智能的东西，一些更个性化的东西，理解你，帮助你以更好更快的方式醒来。

我们将在本文中构建的警报将从过去的事件中学习并理解它们，以便在下一个警报中有更好的性能。一次比一次好用。它会记录用户关闭闹铃所用的时间(用户醒来所用的时间)，并推荐有助于你更快醒来的闹铃。

所以，事不宜迟，让我们开始建立我们的警报。下面我们将一步一步来构建。

## 导入所需的包

第一步是将所需的包导入到我们的 Python 代码中，以便在构建警报时使用它们。

如果没有安装，您需要使用 pip 安装方法进行安装。完成安装步骤后，继续将它们导入代码。

```
import datetime
import os
import time
import random
import csv
from pygame import mixer
import pandas as pd
import numpy as np
```

## 设置音乐文件夹

下一步是建立一个闹铃音乐文件夹，用户可以在其中存储自己喜欢的闹铃音乐。

您可以为警报曲调选择任何路径，我更喜欢在与 Python 脚本相同的文件夹中创建文件夹。我们只需要创建该文件夹一次，因此我们需要检查该文件夹是否存在。如果该文件夹不存在，我们将创建一个。

```
# Getting the current path of the script
path = os.getcwd()
# Setting up the alarm path
alarm_path = path + '\Alarm_Tunes'
# If no directory present, create one.
if not os.path.isdir(alarm_path):
    os.makedirs(alarm_path)
```

在我们的文件夹被创建后，当且仅当文件夹当前为空时，我们会要求用户向文件夹添加一些闹钟铃声。

```
# Ask user to add some alarm tunes to the folder.
while len(os.listdir(alarm_path))==0:
    print("No Alarm Tunes Present. Please add some tunes to the folder before proceeding.")
    confirm = input("Have you added songs? Press Y or N:\t")
    if(confirm=="Y"):
        print("Good! Let's continue!")
        continue
    else:
        continue
```

因此，如上所述，我们要求用户至少添加一个闹钟铃声。如果没有报警音，则发出警告并再次询问用户。

## 创建 CSV 文件并定义助手函数

在进入 CSV 文件创建部分之前，让我们定义一个助手函数。

这个帮助函数帮助我们计算两个 Python 列表之间的差异。这将在我们的程序中用到。

```
def List_diff(list1, list2): 
    if len(list1)>=len(list2):
        return (list(set(list1) - set(list2)))
    else:
        return (list(set(list2) - set(list1)))
```

现在，既然我们已经编写了帮助函数来计算两个列表之间的差异，那么让我们继续创建一个 CSV 文件(如果它还不存在的话)。

```
# If no csv file, create the lists with parameters as zero
if not os.path.isfile("tune_parameters.csv"):
    tune_list = os.listdir(alarm_path)
    tune_time = [60]*len(tune_list)
    tune_counter = [1]*len(tune_list)
    tune_avg = [60]*len(tune_list)
    tune_prob_rev = [1/len(tune_list)]*len(tune_list)
    tune_prob = [1/len(tune_list)]*len(tune_list)
```

所以，上面的代码检查我们是否有一个 CSV 文件；如果没有，我们将创建列表，正如您在上面看到的。我们将在程序结束时将这些保存在一个 CSV 文件中。

现在，让我们解释代码中每个列表的意义。让我们一个一个来看。

1.  **tune_list:** 它存储报警声音的名称，从代码中可以明显看出，因为它在 alarm_path 中存储文件列表。
2.  **tune_time:** 它存储了用户关闭特定闹铃所花费的总时间，即用户醒来所花费的时间。
3.  **tune_counter:** 记录到目前为止每首闹钟铃声播放的次数。
4.  **tune_avg:** 它会找出用户在每一个闹钟铃声中唤醒和关闭闹钟所花费的平均时间。
5.  **tune_prob_rev:** 它根据用户每次闹钟调谐所需的平均时间来计算一种反向概率。
6.  **tune_prob:** 是闹钟铃声每次播放的概率。它根据之前的结果进行自我更新，并使用 **tune_prob_rev** 进行计算。

这里要注意的一点是，我已经为所有这些列表设置了一些默认值，而不是提供零，因为这将对模型产生负面影响，因为从未玩过的人将永远不会有机会，因为概率为零。

所以，我更倾向于假设这些游戏每个都玩过一次，平均时间是 60 秒。因此它使我们的工作更容易。

现在，如果 CSV 文件已经存在，我们需要从 CSV 文件加载数据。

此外，我们需要注意，如果有任何改变，以报警曲调文件夹。用户可能已经添加了新的歌曲或者删除了一些当前的歌曲。因此，我们需要通过添加新的歌曲到我们的列表中或者删除那些已经从文件夹中删除的歌曲来进行更新。

因此，我们使用前面定义的 helper 函数来找出从文件夹获得的列表和从 CSV 文件获得的列表之间的任何差异。因此，我们可以对代码执行所需的操作，并使用各自的公式分别更新 **tune_prob_rev** 和 **tune_prob，**。

```
# If csv file is present, read from csv file
else:
    tune_df = pd.read_csv("tune_parameters.csv")
    tune_list_os = os.listdir(alarm_path)
    tune_list = list(tune_df['Tunes'])
    tune_diff = List_diff(tune_list_os, tune_list)
    tune_time = list(tune_df['Delay Times'])
    tune_counter = list(tune_df['Count'])
    tune_avg = list(tune_df['Average'])
    tune_prob_rev = list(tune_df['Reverse Probability'])
    tune_prob = list(tune_df['Probability'])

    if len(tune_list_os)>=len(tune_list):
        for i in range(0,len(tune_diff)):
            tune_list.append(tune_diff[i])
            tune_time.append(60)
            tune_counter.append(1)
            tune_avg.append(60)
            tune_prob_rev.append(0.1)
            tune_prob.append(0.1)

    else:
        for i in range(0,len(tune_diff)):
            tune_diff_index = tune_list.index(tune_diff[i])
            tune_list.pop(tune_diff_index)
            tune_time.pop(tune_diff_index)
            tune_counter.pop(tune_diff_index)
            tune_avg.pop(tune_diff_index)
            tune_prob_rev.pop(tune_diff_index)
            tune_prob.pop(tune_diff_index)

    avg_sum = sum(tune_avg)

    for i in range(0,len(tune_prob_rev)):
        tune_prob_rev[i] = 1 - tune_avg[i]/avg_sum

    avg_prob = sum(tune_prob_rev)

    for i in range(0,len(tune_prob)):
        tune_prob[i] = tune_prob_rev[i]/avg_prob
```

## 设置闹钟并核实时间

现在，我们需要定义另一个助手函数来检查用户输入的时间是否正确。因此，我们定义了函数 **verify_alarm** 来实现这一点。

```
# Verify whether time entered is correct or not.
def verify_alarm(hour,minute,seconds):
    if((hour>=0 and hour<=23) and (minute>=0 and minute<=59) and (seconds>=0 and seconds<=59)):
        return True
    else:
        return False
```

现在，我们已经准备好了助手函数。所以，我们需要向用户询问闹铃时间。我们将使用一个循环来请求警报，一旦我们验证时间有效，我们将中断。如果无效，我们将再次询问用户，直到他输入一个有效的时间。

```
# Asking user to set alarm time and verifying whether true or not.
while(True):
    hour = int(input("Enter the hour in 24 Hour Format (0-23):\t"))
    minute = int(input("Enter the minutes (0-59):\t"))
    seconds = int(input("Enter the seconds (0-59):\t"))
    if verify_alarm(hour,minute,seconds):
        break
    else:
        print("Error: Wrong Time Entered! Please enter again!")
```

现在，在接受用户输入后，我们将找出当前时间，并将这两个时间转换为秒，并找出两个时间之间的差异。如果差值为负，则意味着闹钟是第二天的。

然后，我们将让 python 代码休眠几秒钟，以便警报只在需要的时间响起。

```
# Converting the alarm time to seconds
alarm_sec = hour*3600 + minute*60 + seconds

# Getting current time and converting it to seconds
curr_time = datetime.datetime.now()
curr_sec = curr_time.hour*3600 + curr_time.minute*60 + curr_time.second

# Calculating the number of seconds left for alarm
time_diff = alarm_sec - curr_sec

#If time difference is negative, it means the alarm is for next day.
if time_diff < 0:
    time_diff += 86400

# Displaying the time left for alarm
print("Time left for alarm is %s" % datetime.timedelta(seconds=time_diff))

# Sleep until the time at which alarm rings
time.sleep(time_diff)
```

**响起闹铃**

现在，我们将敲响我们的闹钟，我们需要根据概率列表随机选择闹钟曲调。为了播放闹铃，我们将使用 **pygame.mixer.music** 库。我们将无限循环闹钟铃声，直到用户停止它。

```
print("Alarm time! Wake up! Wake up!")

# Choose a tune based on probability
tune_choice_np = np.random.choice(tune_list, 1, tune_prob)
tune_choice = tune_choice_np[0]

# Getting the index of chosen tune in list
tune_index = tune_list.index(tune_choice)

# Play the alarm tune
mixer.init()
mixer.music.load(alarm_path+"/"+tune_choice)

# Setting loops=-1 to ensure that alarm only stops when user stops it!
mixer.music.play(loops=-1)

# Asking user to stop the alarm
input("Press ENTER to stop alarm")
mixer.music.stop()
```

**列表的计算和更新**

现在，我们将根据用户停止警报所需的时间来更新列表的值。

我们将找到报警和当前停止报警时间之间的时间差。我们将把它转换成秒，然后相应地更新它。

```
# Finding the time of stopping the alarm
time_stop = datetime.datetime.now()
stop_sec = time_stop.hour*3600 + time_stop.minute*60 + time_stop.second

# Calculating the time delay
time_delay = stop_sec - alarm_sec

# Updating the values
tune_time[tune_index] += time_delay
tune_counter[tune_index] += 1
tune_avg[tune_index] = tune_time[tune_index] / tune_counter[tune_index]
new_avg_sum = sum(tune_avg)

for i in range(0,len(tune_list)):
    tune_prob_rev[i] = 1 - tune_avg[i] / new_avg_sum

new_avg_prob = sum(tune_prob_rev)

for i in range(0,len(tune_list)):
    tune_prob[i] = tune_prob_rev[i] / new_avg_prob
```

**合并列表并保存为 CSV 文件**

现在，我们将所有列表合并成一个多维列表，然后我们将它转换为 pandas 数据框并保存为 CSV 文件。

```
#Create the merged list of all six quantities
tune_rec = [[[[[[]]]]]]

for i in range (0,len(tune_list)):
    temp=[]
    temp.append(tune_list[i])
    temp.append(tune_time[i])
    temp.append(tune_counter[i])
    temp.append(tune_avg[i])
    temp.append(tune_prob_rev[i])
    temp.append(tune_prob[i])
    tune_rec.append(temp)

tune_rec.pop(0)

#Convert merged list to a pandas dataframe
df = pd.DataFrame(tune_rec, columns=['Tunes','Delay Times','Count','Average','Reverse Probability','Probability'],dtype=float)

#Save the dataframe as a csv (if already present, will overwrite the previous one)
df.to_csv('tune_parameters.csv',index=False)
```

使用 Python 的智能警报的完整代码是:

我们终于完成了智能闹钟的制作。关于更新，请访问我的 [Github 资源库](https://github.com/shubham1710/Personalised-Alarm-using-Python)，如果您有一些改进或新想法，请对资源库做出贡献。

我希望你觉得这篇文章很有见地。尝试建立你的版本，并在评论中分享你的想法。感谢阅读！

这篇文章之后还有更多的文章可以阅读:

[](/build-a-blog-website-using-django-rest-framework-overview-part-1-1f847d53753f) [## 使用 Django Rest 框架构建博客网站——概述(第 1 部分)

### 让我们使用 Django Rest 框架构建一个简单的博客网站，以了解 DRF 和 REST APIs 是如何工作的，以及我们如何添加…

towardsdatascience.com](/build-a-blog-website-using-django-rest-framework-overview-part-1-1f847d53753f) [](https://shubhamstudent5.medium.com/build-a-job-search-portal-with-django-overview-part-1-bec74d3b6f4e) [## 用 Django 构建求职门户——概述(第 1 部分)

### 让我们使用 Django 建立一个工作搜索门户，允许招聘人员发布工作和接受候选人，同时…

shubhamstudent5.medium.com](https://shubhamstudent5.medium.com/build-a-job-search-portal-with-django-overview-part-1-bec74d3b6f4e) [](https://medium.com/javascript-in-plain-english/build-a-simple-todo-app-using-react-a492adc9c8a4) [## 使用 React 构建一个简单的 Todo 应用程序

### 让我们用 React 构建一个简单的 Todo 应用程序，教你 CRUD 的基本原理(创建、读取、更新和…

medium.com](https://medium.com/javascript-in-plain-english/build-a-simple-todo-app-using-react-a492adc9c8a4) [](/build-a-social-media-website-using-django-setup-the-project-part-1-6e1932c9f221) [## 使用 Django 构建一个社交媒体网站——设置项目(第 1 部分)

### 在第一部分中，我们集中在设置我们的项目和安装所需的组件，并设置密码…

towardsdatascience.com](/build-a-social-media-website-using-django-setup-the-project-part-1-6e1932c9f221)