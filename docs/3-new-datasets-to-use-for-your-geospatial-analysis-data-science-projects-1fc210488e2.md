# 用于地理空间分析数据科学项目的 3 个新数据集

> 原文：<https://towardsdatascience.com/3-new-datasets-to-use-for-your-geospatial-analysis-data-science-projects-1fc210488e2?source=collection_archive---------37----------------------->

## 新数据科学项目的新数据集

![](img/e4fcaaaf13d022bed426882e8eb2f5f2.png)

格雷格·罗森克在 [Unsplash](https://unsplash.com/s/photos/globe?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

早在三月份，我写了一篇关于“[冠状病毒数据可视化使用 Plotly](/coronavirus-data-visualizations-using-plotly-cfbdb8fcfc3d) ”的文章，这篇文章引起了很多关注。由于受欢迎程度，我带着更多专注于地理空间分析的数据科学项目回来了。

*如果你想学习如何使用 Plotly 创建可视化效果(如下图所示)，点击这里* *查看我的教程* [*。*](/visualizing-the-coronavirus-pandemic-with-choropleth-maps-7f30fccaecf5)

这一次，我带来了三个新的数据集，您可以使用它们来构建新的、令人兴奋的可视化效果！

说到这里，我们开始吧…

# 北美停车统计

[](https://www.kaggle.com/terenceshin/searching-for-parking-statistics-in-north-america) [## 北美停车统计

### 在北美搜索停车位所需时间的数据

www.kaggle.com](https://www.kaggle.com/terenceshin/searching-for-parking-statistics-in-north-america) 

该数据集可识别城市中驾驶员在寻找停车位时遇到困难的区域。城市可以利用这些数据来识别问题区域、调整标识等。仅包括人口超过 10 万的城市。

下面列出了一些有趣的统计数据:

*   AvgTimeToPark:搜索停车场所用的平均时间(分钟)
*   AvgTimeToParkRatio:在当前 geohash 中搜索停车场所花费的平均时间与未搜索停车场所花费的平均时间之比
*   TotalSearching:寻找停车位的司机数量
*   搜索百分比:搜索停车位的司机的百分比

# 世界各地的危险驾驶点

[](https://www.kaggle.com/terenceshin/hazardous-driving-spots-around-the-world) [## 世界各地的危险驾驶点

### 事故发生率最高、刹车最猛的地方

www.kaggle.com](https://www.kaggle.com/terenceshin/hazardous-driving-spots-around-the-world) 

该数据集根据特定区域内的紧急制动和事故级别事件来识别驾驶的危险区域。每个月都会生成一组新的危险驾驶区域，并封装一年的滚动数据(即从前一个月到前一年)。与每个区域相关联的是基于该区域中的发生频率和所述发生的严重性的严重性分数。数据是过去 12 个月的汇总数据。

下面列出了一些有趣的统计数据:

*   SeverityScore:每个区域的严重性分数，即每 100 个交通流量单位的紧急制动事件和事故级别事件的数量。交通流量定义为 geohash 中每小时的车辆总量
*   事件总数:geohash 中发生的紧急制动事件和事故级事件的总数

# 新冠肺炎对机场交通的影响

[](https://www.kaggle.com/terenceshin/covid19s-impact-on-airport-traffic) [## 新冠肺炎对机场交通的影响

### COVID 后交通量分析

www.kaggle.com](https://www.kaggle.com/terenceshin/covid19s-impact-on-airport-traffic) 

该数据集显示了进出机场的交通量占基线期交通量的百分比。用于计算此指标的基线期是从 2020 年 2 月 1 日到 3 月 15 日。数据集每月更新一次。

下面列出了一些有趣的统计数据:

*   PercentofBaseline:该日期的旅行与基线期内一周同一天的平均旅行次数相比所占的比例

# 感谢阅读！

我希望你觉得这些数据集有趣/有用。欢迎在评论中分享你的可视化链接。一如既往，我祝你在努力中好运！

不确定接下来要读什么？我为你挑选了另一篇文章:

[](/visualizing-the-coronavirus-pandemic-with-choropleth-maps-7f30fccaecf5) [## 如何用 Choropleth 图显示疫情冠状病毒

### 关于 Choropleth 地图的介绍和教程

towardsdatascience.com](/visualizing-the-coronavirus-pandemic-with-choropleth-maps-7f30fccaecf5) 

# 特伦斯·申

*   *如果你喜欢这个，* [*跟我上 Medium*](https://medium.com/@terenceshin) *了解更多*
*   *关注我*[*Kaggle*](https://www.kaggle.com/terenceshin)*了解更多内容！*
*   *我们连线上*[*LinkedIn*](https://www.linkedin.com/in/terenceshin/)