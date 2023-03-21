# 按行业和城市分列的就业增长趋势:数据研究

> 原文：<https://towardsdatascience.com/employment-growth-trends-by-industry-and-city-a-data-study-5002bf20692?source=collection_archive---------32----------------------->

根据劳动统计局的报告，就业市场继续增长。然而，无论是在国家层面还是在地方层面，并非所有行业都以同样的速度增长。这份报告(由[美国社区调查的 2017 年县商业模式数据集](https://www.census.gov/data/datasets/2017/econ/cbp/2017-cbp.html)提供支持)考察了最大的大都市区顶级就业部门的就业增长情况。

*这里有一些高层次的见解，* [*但是到这里来使用我的免费 Tableau 仪表板，进一步了解不同的行业、领域和其他就业趋势*](https://public.tableau.com/profile/david.peterson#!/vizhome/NumberofEmployeesbySectorWIP/EmploymentGrowthbySector) *。*

**按城市/大都市区划分的最集中行业(** [**查看仪表盘**](https://public.tableau.com/shared/T7XCF8XH9?:display_count=y&:origin=viz_share_link) **)**

哪个行业的单位面积雇佣人数最多？

当观察每个顶级市场的顶级行业时，医疗保健和社会援助部门占据了市场的最大份额，包括占大费城地区的 19.3%。有几个主要地区与这一趋势不同，如华盛顿 DC 和圣何塞，专业、科学和技术服务分别占所有工作岗位的 21%和 13%)。住宿和餐饮服务占圣地亚哥就业的 14%，奥兰多的 16%。

![](img/db26b3ede5d2cf8f675c490baffcd20a.png)

**按地区划分的最集中行业(与全国平均水平相比)(** [**查看仪表盘**](https://public.tableau.com/shared/RWC6PR5PX?:display_count=y&:origin=viz_share_link) **)**

与全国平均水平相比，一个行业在一个地区的就业集中度如何？

现在，我们将某个行业在该地区的就业集中度与该行业的全国平均就业水平进行比较。

软件出版在多个主要市场中被过度指数化，在旧金山(+高于全国平均水平 380%)、圣何塞(+622%)和西雅图(+597%)，软件出版就业超过全国平均水平 380%。大学是匹兹堡(+187%)和圣路易斯(+106%)指数最高的行业。波士顿(+269%)和费城(+225%)的投资组合管理指数过高。与此同时，DC 和巴尔的摩的计算机系统设计服务指数过高(分别为+485%和+276%)。

![](img/3d0a36a7c039921ca1fa670f3208eba1.png)

**分行业就业增长(** [**查看仪表盘**](https://public.tableau.com/shared/SP4B5GPFQ?:display_count=y&:origin=viz_share_link) **)**

*在全国范围内，哪些行业增长最快？*

根据这组数据，在过去的五年里，总就业率增长了 9.3%。某些部门的增长一直高于平均水平。当看全国总数时，专业贸易承包商(+27%)、建筑(+25%)和餐馆(+20%)出现了大幅增长。与此同时，废物管理(-1.6%)和行政服务(-2.1%)在过去 5 年中有所缩减。对于食品行业来说，这也是一个不错的五年，食品服务和饮料场所(+19.6%)、全方位服务餐馆(+15.5%)和有限服务餐馆(+24.7%)的就业人数连续五年增长。

![](img/4e72b940844309a679c5b21ea8a222ff.png)

**行业就业增长高于或低于全国平均水平的地理视图(** [**查看仪表板**](https://public.tableau.com/shared/RJJQSSPHR?:display_count=y&:origin=viz_share_link) **)**

*Color: Green = Area 的部门工作比例高于美国平均水平。红色=该地区的部门工作比例低于美国平均水平*

*区域气泡尺寸基于区域(所有行业)的员工总数*

*我将在下面展示几个主要行业作为例子，* [*但请到这里使用我的免费 Tableau 仪表盘来进一步了解不同的行业、领域和其他就业趋势*](https://public.tableau.com/profile/david.peterson#!/vizhome/NumberofEmployeesbySectorWIP/EmploymentGrowthbySector) *。*

**健康护理**

在最大的市场中，东北部的医疗保健就业指数过高，但密西西比以西的主要市场的医疗保健就业指数往往偏低。

![](img/1ce121c8e78ef9bffe28f4e9fdb90438.png)

**信息**

只有少数市场的信息就业指数过高，但它分散在全国各地。然而，中北部市场(包括芝加哥、明尼苏达、底特律和克利夫兰等主要地区)在该行业中指数偏低。

![](img/0796a9482597d42003d20450fca0aa48.png)

**零售业**

在全国范围内，零售贸易几乎完全在最大的市场中指数化不足，而在较小的市场中指数化过度。

![](img/f182031e6a2a20043924e5a400c8f370.png)

**制造业**

与信息中的地理趋势相反，芝加哥、明尼苏达、底特律和克利夫兰等主要中北部地区的制造业工作指数过高，而沿海主要市场(佛罗里达以外)的制造业工作指数往往过低。

![](img/696cead7ecc402f00021232e8e0125e4.png)

**建设**

西部、中部和南部地区的建筑业就业指数往往过高，而东北部地区的集中度较低。

![](img/44eb7c4a22adf0fe5b280cedb22c6f7e.png)

*感谢您阅读我最新的数据帖子。作为一名产品营销和数据分析专业人士，我对数据支持的讲故事充满热情。这些数据新闻帖子是我将真实数据与我个人的不同兴趣整合起来的方式，这些兴趣包括从旅行和娱乐到经济和社会问题的任何事情。
如果您有任何评论、故事想法或预期的数据项目，请随时发送电子邮件至 dwpwriting <邮箱至> gmail < dot > com，访问我的网站至*[*【dwpeterson.com*](http://dwpeterson.com/)*，或通过*[*LinkedIn*](https://www.linkedin.com/in/davidwpeterson/)*联系我。*