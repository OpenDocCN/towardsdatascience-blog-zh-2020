# mad Maps——可视化地理数据以获得最大影响

> 原文：<https://towardsdatascience.com/mad-maps-visualizing-geographical-data-for-maximum-impact-d7e2b5ff2471?source=collection_archive---------29----------------------->

## [入门](https://towardsdatascience.com/tagged/getting-started)

## 如何使用 Python(包括代码和数据)在地图上有效地交流数据，以获得清晰、深刻的见解

美国总统大选又要来临了。毫无疑问，地图已经淹没了你的 Twitter 时间轴和新闻源，11 月 3 日之后的几周内，这种情况将会升级。

如今，我们认为地图是理所当然的，不难看出为什么。智能手机已经商品化了技术，可以准确定位我们，并提供到最近的 5 家评分至少为 4.0 的日本餐馆的实时路线。地图从未像现在这样融入我们的生活，尽管就在十年前，人们还在使用*的街道目录*。(颤抖)

但是，如果说谷歌地图是地图的最终进化，那将是对制图学领域的极大伤害。这与事实相去甚远，虽然有用，但这只是地图*能做的*的一小部分。

地图是人类智慧的杰作，可以帮助获得独特的见解，并有效地传达它们，无论是人口统计、经济、健康，还是真正的政治。

想象一下，作为一名早期的制图师，从地面精确地描绘出我们世界的表面轮廓。想象一下，你是第一批探索世界某些地区的人，并在页面上捕捉他们的形态、居民、植物和动物，以便下一批到达的人可以更安全、更快速地在世界上导航。想象一下，创造一些东西来帮助通知和指导从渔民到将军的每一个人会是什么感觉。

换句话说，制图学创造了视觉辅助工具，帮助用户在变幻莫测的数据海洋中导航，这是数据可视化更广阔领域的先驱。[数据可视化的第一个已知示例之一是(霍乱爆发的)增强地图](https://www.theguardian.com/news/datablog/2013/mar/15/john-snow-cholera-map)这一知识也具有某种完整循环的性质，尤其是考虑到当前的疫情和仪表板的扩散。

![](img/eaabec93977fbf016e66198bc401d79a.png)

约翰·斯诺的霍乱爆发地图

有了这张相对简单的地图，John Snow 能够可视化疾病发病率数据，并说明爆发的中心位置(以及位于那里的水泵)很可能是霍乱病例的原因。

快进到今天，已经有惊人的数据可视化工具(如 [Plotly](https://plotly.com) 、 [DataWrapper](https://www.datawrapper.de) 或 [Tableau](https://www.tableau.com) )能够绘制出不仅信息丰富，而且视觉效果惊人的地图。

举个例子，看看这幅美国农作物的图片:

![](img/f5908889752387ac66e8b90ff3b0a700.png)

各县用于种植各种作物的土地百分比(截图:[比尔·兰金](http://www.radicalcartography.net/index.html?crops)

或者这张疫情期间各国政府提供的收入补助图:

![](img/8eb4010f6cf6ecd95ff2f7b7db1bb7cf.png)

OurWorldInData.Org 新冠肺炎时期的政府支持(截图:)

地图是可视化数据的真正神奇的工具。因此，在本文中，让我们来看看如何创建我们自己的地图来可视化空间数据。

## 在开始之前

要跟进，安装`plotly`、`pandas`和`numpy`。用一个简单的`pip install [PACKAGE_NAME]`安装每一个(在您的虚拟环境中)。

使用以下内容导入相关模块:

```
import pandas as pd
import numpy as np
import plotly.express as px
```

你可以在这里找到所有需要的数据和代码(请看`draw_map.py`文件):

[](https://github.com/databyjp/mad_maps) [## databyjp/mad_maps

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/databyjp/mad_maps) 

# 我们的第一张地图

## 加载基础地图数据

在本文中，我们将在美国县级地图上绘制数据。要做到这一点，我们需要一个基本数据集布局县的位置。让我们使用 Plotly 推荐的 [GeoJSON](https://geojson.org) 文件:

```
from urllib.request import urlopen
import json
with urlopen('https://raw.githubusercontent.com/plotly/datasets/master/geojson-counties-fips.json') as response:
    counties = json.load(response)
```

## 加载数据

一个容易获得的县级数据集是按收入分类的。我已经对数据集(来自美国人口普查)进行了预处理，并将其保存到一个数据帧中，所以只需像这样将其加载到这里:

```
data_df = pd.read_csv("srcdata/proc_data.csv", index_col=0)
```

通过运行`data_df.head()`、`data_df.info()`等命令，检查是否包含适当的数据和正确的行数/列数:

(数据帧应包括 3223 行，每行应包括一个 fips 代码。)

示例输出的前几行—来自。信息()方法

最简单，也是最显而易见的做法是尊重地理边界，将数据绘制到所谓的 [choropleth 地图](https://en.wikipedia.org/wiki/Choropleth_map)上。Choropleth 地图根据所描述的质量给每个区域着色。

这是我的例子:

我在这里做的事情很简单。数据作为第一个参数(`data_df`)传递；其中`fips`列和`Median_Household_Income_2018`列分别作为位置和颜色数据被传递；而`counties`变量是我们之前下载的 GeoJSON 数据。

`update_layout`和`update_traces`方法是出于美学原因而运行的；设置地图样式、缩放级别、位置和图形边距。

![](img/d8fae081c35408428508b5d9715686c0.png)

美国各县家庭收入的 choropleth 地图(图片:作者)

*你可能已经注意到，我将这里的颜色范围设置为 0 到 100，000 之间。我选择这个值是因为 50，000 大约是数据集的中间值。它旨在使那些“中间”值看起来尽可能的中性。*

这是一张好看的地图！你可以清楚地看到全国的总体趋势。

***编辑:*** *这里有一些东西，是一个* [*超级乐于助人的 Redditor*](https://www.youtube.com/c/alexkolkena) *能够从刚才看的地图中挑出来的(* [*完整评论此处*](https://www.reddit.com/r/dataisbeautiful/comments/jmnvwz/oc_household_income_in_the_us_at_county_level/gayk995?utm_source=share&utm_medium=web2x&context=3) *):*

> 看到新墨西哥州北部那个小小的蓝点了吗？那是洛斯阿拉莫斯县。它包括两个镇，其经济围绕着当地的国家实验室，该实验室雇佣了数千名科学家和工程师。洛斯阿拉莫斯的人均博士数量是所有县中最高的。
> 
> DC 地区那块巨大的蓝色区域向我们展示了美国八个最富裕的县中有五个与 DC 接壤。他们有几十个联邦机构，这些机构的雇员并不依靠良好的经济来保住他们的工作……也没有任何迎合他们的当地企业。因此，DC 是不受衰退影响的城市。
> 
> 肯塔基州东部和西弗吉尼亚州南部的橙色斑点是生活贫困的下岗矿工和阿巴拉契亚人。
> 
> 南达科他州的橘色地区和四角地区是印第安人保留地。这些地方长期以来遭受贫困的打击最大。
> 
> 沿着密西西比三角洲和南方腹地的黑带清晰可见。农村的非洲裔美国人挣扎着维持生计，并且不成比例地患有艾滋病、糖尿病和肥胖症。

*是不是很神奇？再次提醒您，* ***数据在上下文中无限丰富，*** *，并且数据来自真实的人。如果你对他们还知道什么感兴趣，你可以在这里找到评论者(亚历克斯)。*

让我们更进一步。如果我们想确定某些人口统计数据，如低收入或高收入县，会怎么样？我们可以从这张地图中得到一些想法，但我们可以做得更好。

实现这一点的一种方法是过滤数据帧。由于 Plotly 将简单地忽略丢失的数据点(正如您可能已经注意到的上面那个没有颜色的县)，我们甚至不需要用占位符替换丢失的值。

您会注意到代码或多或少是完全相同的。我在这里做的唯一不同的事情是过滤数据并传递过滤后的数据帧。瞧啊。

![](img/66207ec3b3a18f0d03c8c2002268339e.png)

美国低收入县的 choropleth 地图(图片:作者)

我们可以做类似的改变来绘制高收入县的地图。

![](img/8e435d6704347d9c7ef58d44fc1cb396.png)

美国高收入县的 choropleth 地图(图片:作者)

这些地图在突出显示数据子集方面做得更好——大多数低收入县在南方，许多高收入县在沿海地区。

只绘制该数据子集的另一个优点是，通过操纵色阶，可以更容易地看到子集内的梯度。看一看:

我在这里所做的是压缩范围(到 20，000 到 40，000 之间)，然后将色阶改为顺序色阶(橙色)，反过来使较低的颜色更暗。下面是结果:

![](img/a883553670ab060589483d7e2c82804b.png)

突出显示美国低收入县的 choropleth 地图(图片:作者)

收入较高的县也是如此，但颜色是蓝色和未绑定的颜色(因此它自然延伸到数据集的界限)。

![](img/0518a48fc03ec48b93783eb69b5ea393.png)

突出显示美国高收入县的 choropleth 地图(图片:作者)

有了这些地块，收入最低的县和收入最高的县才真正能够凸显出来。

# 不仅仅是一匹只会一招的小马

但是可能存在其他什么模式呢？这就是 choropleth 地图开始显示其局限性的地方。它只允许*一个*变量被绘制到固定区域。

为了研究数据，让我们通过将数据点(县)分组来简化数据的表示；在这种情况下，根据县人口规模和家庭收入。

例如，我们可以使用`pd.qcut`函数按四分位数分割县:

```
x = pd.qcut(tmp_df["POP_ESTIMATE_2018"], 4)
```

这将按县的数量对数据进行分组。然而，这可能不是我们想要的——因为较小的县在人口方面的代表性不足。

![](img/845be0c73e13f53240a842d99cd11525.png)

我们已经可以看到，第二个情节揭示了第一个没有的细节。虽然我们看不出地理和变量之间有太多的相关性，但后一张图显示了明显的相关性。通过这个简单的图表，我们可以看到县*人口*对典型收入有一些影响。

那么，我们如何将这样的数据绘制到特定的地理位置上呢？

# 不仅仅是大小的问题

尽管 choropleth 地图很棒，但它确实有一个很大的缺陷。T2 的大小总是很重要。由于我们的大脑自然倾向于将大小作为地图上重要性的指标，所以 choropleth 地图上每个区域的大小总是看起来很重要。即使真正重要的数字可能是，比如说，*人口*而不是*规模*。

这可能有点抽象，所以这里有一个真实的例子。这是一张世界各国的地图。我们都见过无数次了:

![](img/6821d6fd167d4b6f5c984b4a755d7954.png)

世界地图(图片:[维基百科](https://en.wikipedia.org/wiki/File:CIA_WorldFactBook-Political_world.pdf))

这是世界，这里的边界是根据人口来划分的。请注意某些地方，比如我的祖国(澳大利亚)是如何缩小到被遗忘的，而其他地方，比如印度尼西亚，现在却大得多。

![](img/8cff784a90aff92839d2e4fcb708e560.png)

按人口划分的世界(图片: [OurWorldInData](https://ourworldindata.org/world-population-cartogram)

想象用收入之类的东西给第一张地图着色，而不是用同样的东西给第二张地图着色。它们看起来会非常不同。

因此，让我们建立一个气泡图。我是泡泡图的超级粉丝；它允许我们用气泡大小来表示另一个维度的数据。

因此，本着同样的精神，让我们将我们的县地图绘制成一个气泡图，其中一个气泡代表一个县，其大小基于人口。我们不会像上面那样调整地图的大小，但是会调整绘制到地图中的内容。

# 在表面冒泡

第一步是为每个县选择一个位置来容纳每个泡泡。作为一个粗略的解决方案，我将选择每个 GeoJSON 形状的边界框的中心。

这需要熟悉 GeoJSON 数据格式，但是一旦你花几秒钟浏览它，你就会明白这里发生了什么。

基本上，数据是以嵌套列表的形式存在的(在字典中)，所以这个脚本遍历列表以获得左下(min，min)边界和右上(max，max)坐标。随后，我们收集盒子的中心，并建立一个带有 fips 代码的数据框架。

您应该有一个如下所示的 county_df:

从那时起，抓取收入和人口数据，并绘制图表是一件小事。

我在这里选择使用`[.join](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.join.html)` [方法](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.join.html)来做，然后简单地绘制数据。注意，我在这里使用的 Ploty Express 函数是`scatter_mapbox`，而不是`choropleth_mapbox`，但是这里的一切应该是不言自明的。

我唯一需要修改的是`size_max`属性；这或多或少取决于个人喜好。

我们得到了这张漂亮的图表:

![](img/24839d91a684262c36819b600c82798b.png)

美国各县家庭收入的 choropleth 地图(图片:作者)

为了视觉效果，我压缩了这里的范围；但是这幅图与上面的完全不同，上面画的是同样的数据，没有人口。看看这个对比:

![](img/820801a8742ad780a2a6eae26ae58f40.png)

两次观想的比较——印象非常不同(图片:作者)

随着对县人口的关注，该国中部的大部分完全消失，沿海地区被突出显示。另一方面，关注县的规模会让你的目光立即投向南部各州。

我并不是说观想更好。我想指出的是，它确实强调了相同(收入)数据的可视化表示看起来会有很大不同。

作为这些图表的创作者，值得思考如何最好地支持你的信息。

作为读者，可能值得考虑的是，你可能从视觉效果中得出的结论是否真的有根据。

这只是一个旁注，但我认为在我们这个视觉世界中这是一个重要的问题。

如果你喜欢这个，在 twitter 上打招呼/关注，或者点击这里更新。ICYMI:我还写了这篇关于 Streamlit 和 Dash 的文章，这两个包非常流行，用于使用 Python 创建在线仪表盘。

[](/plotly-dash-vs-streamlit-which-is-the-best-library-for-building-data-dashboard-web-apps-97d7c98b938c) [## Plotly Dash 与 Streamlit——哪个是构建数据仪表板 web 应用程序的最佳库？

### 用于共享数据科学/可视化项目的两个顶级 Python 数据仪表板库的比较——

towardsdatascience.com](/plotly-dash-vs-streamlit-which-is-the-best-library-for-building-data-dashboard-web-apps-97d7c98b938c) 

这是另一篇关于地理数据可视化的文章——这次是可视化 NBA 旅行！

[](/6-degrees-of-separation-in-the-nba-why-suspending-it-was-inevitable-9c646db99e4c) [## NBA 的 6 度分离——为什么暂停它是不可避免的

### 用 Python 可视化 NBA 的旅游连接链(带数据和代码)

towardsdatascience.com](/6-degrees-of-separation-in-the-nba-why-suspending-it-was-inevitable-9c646db99e4c) 

保持安全；下次再见！