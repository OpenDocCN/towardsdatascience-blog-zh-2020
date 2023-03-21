# 2020 年如何做数据科学

> 原文：<https://towardsdatascience.com/how-to-do-data-science-in-2020-e8f729cb65e3?source=collection_archive---------44----------------------->

## 数据科学项目创意

2020 年数据科学的嗡嗡声就像特斯拉股票；它每天都在增加。这个领域非常热门，从机械工程师到医生，每个人都想成为数据科学家。但是，您将如何进入数据科学领域？加入 DS 训练营？做两三门 MOOCs？参加 Kaggle 比赛？纲要是无止境的。我不是在反驳 MOOCs 甚至 Kaggle 竞赛的优势，它们是学习数据科学的绝佳地点。

然而，问题是每个人都在这么做！我们多久会在泰坦尼克号数据集上看到一些关于他们的 Coursera 专业化或 GitHub 机器学习档案的帖子？如果你不得不找一份数据科学家的工作，你必须完成一些没有人做的事情。从根本上说，你必须将自己与其他竞争者区分开来。做全栈(问题制定、数据收集、数据清理、EDA、ML、部署)数据科学项目将为你提供制高点，同时在你是试图闯入数据科学的新生的情况下明确会面。

![](img/9cbadf849cdf654d0ed95a48b037a608.png)

[https://cdn . pix abay . com/photo/2012/03/01/15/43/brain-20424 _ 1280 . jpg](https://cdn.pixabay.com/photo/2012/03/01/15/43/brain-20424_1280.jpg)

我最近偶然发现了苏珊的[博客](https://datasciencecareermap.com/2019/05/28/how-to-find-the-best-data-science-jobs-the-insight-hack/)，她在博客中解释了我们如何找到最好的数据科学工作。她建议我们应该试着在[有识之士](https://www.insightdatascience.com/)工作的地方找一份工作。因此，出于好奇，我最终浏览了 [Insight](https://medium.com/u/7b169ef2b4c1?source=post_page-----e8f729cb65e3--------------------------------) 的网站，主要是为了找到他们工作的公司。但是随着我对他们同事的项目了解的越来越多，我感到惊讶。当然，他们有定量领域的博士学位，但他们的一些项目可以转化为产品。以下是我用的刮刀。

```
import requestsfrom bs4 import BeautifulSoupresult=requests.get("[https://www.insightdatascience.com/fellows](https://www.insightdatascience.com/fellows)")src=result.contentsoup=BeautifulSoup(src,'lxml')div=soup.divname=[]for row in soup.find_all('div',attrs={"class" : "tooltip_name"}):name.append(row.text)project=[]for row in soup.find_all('div',attrs={"class" : "tooltip_project"}):project.append(row.text)

title=[]for row in soup.find_all('div',attrs={"class" : "toottip_title"}):title.append(row.text)

company=[]for row in soup.find_all('div',attrs={"class" : "tooltip_company"}):company.append(row.text)

d={'Name':name,'Project':project,'Company':company,'Title':title }df=pd.DataFrame(d)
```

万一你需要做一些很酷的项目，不要停下来从[这里](https://drive.google.com/file/d/1tL8dm8z1TSgm1erma0PB3K-ytMRGAh98/view?usp=sharing)获取动力或想法。不管你是否能复制或扩展这个项目，如果你能理解他们做了什么以及他们为什么这么做，这将是巨大的帮助。同样，这也将是面试中一个非同寻常的话题，让你从不同的竞争对手中脱颖而出。