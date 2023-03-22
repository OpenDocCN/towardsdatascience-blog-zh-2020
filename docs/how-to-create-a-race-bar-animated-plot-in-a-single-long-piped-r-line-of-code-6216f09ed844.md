# 如何在一行代码中创建一个种族栏动画

> 原文：<https://towardsdatascience.com/how-to-create-a-race-bar-animated-plot-in-a-single-long-piped-r-line-of-code-6216f09ed844?source=collection_archive---------55----------------------->

## *也许这不是最优雅的方式，但为了吸引人的标题，这是值得的*

长管道 R 命令…有些人喜欢它，有些人不喜欢！

当你想要拍摄一个令人印象深刻的复杂的多米诺骨牌视频时，你是要冒险在完成之前意外地翻转一个立方体，并毁掉整个东西，还是更喜欢将它分成独立的单元，更好地隔离，这也将允许你探索和测试它的功能，而不伤害其他部分？

![](img/6d07b11cadb809a9a5a6881f4127e6e3.png)

[https://pix abay . com/photos/domino-hand-stop-corruption-665547/](https://pixabay.com/photos/domino-hand-stop-corruption-665547/)

在 R 的情况下，管道操作符，最初来自包[马格里特](https://cran.r-project.org/web/packages/magrittr/vignettes/magrittr.htmlhttps://cran.r-project.org/web/packages/magrittr/vignettes/magrittr.html)，允许你传递一个对象到下一个函数。所以从技术上来说，你可以在不中断链的情况下，或者通过避免可读性较差的嵌套函数，将对象传递给其他函数。

就像上面的多米诺类比一样，我不认为有一个规则，它应该基于您对使用管道的舒适程度以及您可能具有的其他限制(如连接到数据框的外部源(或其他输入参数)、计算内存等)来自我指导。

建议打破链主要是为了可读性和文档，但就性能而言，可能不需要。这就像在一个长句子中阅读一长串步骤(功能)，而没有停下来喘口气。

在下面的例子中，为了便于演示，我创建了一个很长的 R 管道代码行，可以一次性运行。同样，这可能不是最推荐的风格，但为了吸引人的标题和{}中嵌套管道的使用，这种风格值得一试。

```
JSON_object %>% {setNames(data.frame(.$data), .$columns)}
```

我使用了新冠肺炎开放数据项目[中美国各州的新冠肺炎累计死亡人数。](https://github.com/GoogleCloudPlatform/covid-19-open-data)

```
library(robservable)
library(jsonlite)
library(tidyverse)robservable::robservable(
  "[https://observablehq.com/@juba/bar-chart-race](https://observablehq.com/@juba/bar-chart-race)",
  include = c("viewof date", "chart", "draw", "styles"),
  hide = "draw",
  input = list(
  data = 
    fromJSON('[https://storage.googleapis.com/covid19-open-data/v2/epidemiology.json'](https://storage.googleapis.com/covid19-open-data/v2/epidemiology.json')) %>% 
    {setNames(data.frame(.$data), .$columns)}  %>% as_tibble %>% 
    filter(key %in% c("US", "US_AK", "US_AL", "US_AR", "US_AS", "US_AZ", "US_CA", 
"US_CA", "US_CO", "US_CT", "US_DC", "US_DE", "US_FL", "US_GA", 
"US_GA", "US_GU", "US_HI", "US_IA", "US_ID", "US_IL", "US_IN", 
"US_KS", "US_KY", "US_LA", "US_MA", "US_MD", "US_ME", "US_MI", 
"US_MN", "US_MO", "US_MP", "US_MS", "US_MT", "US_NC", "US_ND", 
"US_NE", "US_NH", "US_NJ", "US_NM", "US_NV", "US_NY", "US_NY", 
"US_OH", "US_OK", "US_OR", "US_PA", "US_PR", "US_RI", "US_SC", 
"US_SD", "US_TN", "US_TX", "US_UT", "US_VA", "US_VI", "US_VT", 
"US_WA", "US_WI", "US_WV", "US_WY")) %>% 
    separate(key, c('US', 'state'), sep = '_') %>% 
    mutate(month = month(date)) %>%
    arrange(date, state) %>% 
    select(id = state, date = date, value = total_deceased) %>% 
    filter(!is.na(value), value != 0),
        title = "COVID-19 deaths count",
        subtitle = "Cumulative number of COVID-19 deaths by US state",
        source = "Source : Johns Hopkins University"
    ),
  width = 700,
  height = 710
)
```

上面的代码将创建动画，以 HTML 文件格式保存。

[HTML 动画文件](https://github.com/drorberel/mediumHTML/blob/main/race2.html)可以在我的 GitHub repo 找到。下面是从我的屏幕上录制的视频，因此质量下降。

[https://share.getcloudapp.com/6quPGBbN](https://share.getcloudapp.com/6quPGBbN)作者

这篇博文与我前一天发表的标题与新冠肺炎相关的[博文非常相似。虽然侧重点不同，但发布类似帖子的原因是为了测试每个帖子会吸引多少“观众注意力”(掌声和观看次数)。当然，每篇文章都有不同的推广方式:在聚合出版物下发布，LinkedIn，脸书，Twitter，R-bloggers，发布日效应，等等。我很乐意稍后分享我的发现。](https://drorberel.medium.com/covid-19-death-visualization-race-bar-chart-for-us-states-96edc59102a9)