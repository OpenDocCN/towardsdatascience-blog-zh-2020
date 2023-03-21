# 郡:R 中简单的街道地图

> 原文：<https://towardsdatascience.com/the-county-simple-street-maps-in-r-89cce35d05e5?source=collection_archive---------35----------------------->

## R 中的街道地图

![](img/1472aa9f12b5069f572a4bb73ec07802.png)

来源:作者，见下文。

我把这篇文章命名为“郡”在西弗吉尼亚州的家乡，人们经常根据他们的管辖权或他们获得权力的地方来称呼政府实体。我们把警察称为“法律”，把公共卫生或税收等各种职能称为“国家”。今天，我在画西弗吉尼亚州费耶特维尔县的地图。甚至当你进城交税的时候，你也会说“去看看县城”这对于家乡的人们来说是正确的，可能对于其他农村地区也是如此。

从我年轻的时候起，我就对地图、地球仪(尤其是有贸易路线的旧地球仪)和一般的地图学着迷。所以，我很高兴最近在 Twitter 上看到了几个融合了数据可视化的地图制作的例子。

我的灵感来自于对斯普林菲尔德、密苏里州康纳·罗斯柴尔德和北卡罗来纳州阿什维尔的美妙演绎。查看他们的更多信息。谢谢你们！！

# 收集工具

除非您正常使用映射数据和函数，否则下面的库并不常见。这是我的第一张地图，所以我需要安装这些。使用`remotes`包需要更少的依赖，因此在安装其他包时通常比`devtools`更快。

*   `osmdata`包含用于绘制道路、桥梁和水路的开放街道地图数据；费耶特维尔展示了数据中的一些漏洞——更多内容见下文
*   `ggspatial`为我们地图上的北向罗盘提供功能
*   `showtext`支持在 ggplot2 图形中使用谷歌字体

```
#library(remotes) 
#remotes::install_github("ropensci/osmdata") #remotes::install_github("paleolimbot/ggspatial") #remotes::install_github("yixuan/showtext")
```

r 用户对下面常见的疑点都很熟悉，尤其是`tidyverse`。特别是，我使用`ggmap`包来绘制空间数据所需的形状文件(Kahle 和 Wickham 2013)。

```
library(tidyverse) 
library(osmdata) 
library(showtext) 
library(ggmap) 
library(ggExtra) 
library(ggspatial) 
library(showtext)
```

一旦加载了`osmdata`包，你可以查看下面给定标签的可用特性。桥标签上的注释:它将返回“是”。这是令人困惑的，直到我看了数据，因为似乎没有桥梁类型的区别-在这种情况下没有害处。

# 位置，位置，位置

为了绘制费耶特维尔的街道地图，我需要知道它的地理位置。下面的代码将返回定义费耶特维尔地区的两个纬度和两个经度。这些坐标构成了下图的基础。

```
fa_wv <- getbb("Fayetteville West Virginia") 
fa_wv
```

## 摄取 OSM 数据

接下来，我从定义大、中、小街的开放街道地图包中提取实际的街道数据(取自上面的帖子)。除了街道，费耶特维尔还有两个举世闻名的特色——新河及其世界级的白水和[新河峡谷桥](https://www.nps.gov/neri/planyourvisit/nrgbridge.htm)(西半球最长的拱桥，以前是世界上最长的)。你可以在我上面的“关于”页面从不同的角度看到它。我也从 OSM 的数据中获取了这些数据。这段代码摘自 Connor 和 Josh 的优秀示例。

下面的代码提供了数据视图。滚动查看地图上值得高亮显示的其他街道或特征也很有用。

```
View(big_streets[["osm_lines"]]) 
View(med_streets[["osm_lines"]]) 
View(small_streets[["osm_lines"]]) 
View(bridge[["osm_lines"]]) 
View(river[["osm_lines"]])
```

定义要在`ggplot2`中绘制的特征集很有用。下面的代码提取了用于绘制给定位置的各个街道的数据。记住，我们从上面的费耶特维尔拉街道。

```
us_19 <- big_streets[["osm_lines"]] %>% 
  filter(ref == "US 19") 
wv_16 <- med_streets[["osm_lines"]] %>% 
  filter(ref == "WV 16") 
fayette_station <- small_streets[["osm_lines"]] %>% 
  filter(name =="Fayette Station Road") 
west_maple_ave <- small_streets[["osm_lines"]] %>% 
  filter(name == "West Maple Avenue") 
maple_ave_west <- small_streets[["osm_lines"]] %>% 
  filter(name == "Maple Avenue West") maple_lane <- small_streets[["osm_lines"]] %>% 
  filter(name == "Maple Lane") 
north_court_st <- med_streets[["osm_lines"]] %>% 
  filter(name == "North Court Street") nr_gorge_bridge <- bridge[["osm_lines"]] %>% 
  filter(name == "New River Gorge Bridge") 
new_river <- river[["osm_lines"]] %>% 
  filter(name == "New River")
```

# 字体

我对任何与字体和写作有关的东西都是绝对的极客。`showtext`包提供了在`ggplot2`图形中使用谷歌字体等功能。我喜欢添加一个字体就像告诉它是哪一个，然后用你的代码命名它一样简单。下面的`showtext_auto`函数告诉`ggplot2`使用这些字体自动渲染文本。

```
## Loading Google fonts (https://fonts.google.com/) font_add_google("Libre Baskerville", "baskerville") font_add_google("Playfair Display", "playfair") 
showtext_auto()
```

在地图上标出一些名胜很好。下面的代码为此提供了数据。我从谷歌地图上获得了下面的经度和纬度数据。上面引用的例子使用了`rvest`来抓取位置，但是嘿，费耶特维尔并没有那么大！！

# 凉爽的地方

```
essential <- tribble( ~place, ~lon, ~lat, 
  "Bridge Brew Works", 81.11368, 38.01651, 
  "Court House", -81.103755, 38.053422, 
  "Pies & Pints", -81.105514, 38.050783, 
  "Wood Iron Eatery", -81.102757, 38.053110 )
```

就这些说几句。虽然下面没有标出，但桥酿酒厂很容易酿造出本州最好的啤酒。特别是贮藏啤酒，令人印象深刻(由于发酵和贮藏对温度的严格要求，贮藏啤酒通常比麦芽啤酒更难酿造，但我离题了)。法耶特法院是一座历史建筑，拥有所有的盛况和魅力，是真正的县城——所有重要的业务都在那里进行。

T4 有最好的披萨和啤酒。它供应前面提到的 Bridge Brew Works 啤酒，以及来自 [Hawk Knob 苹果酒& Mead](https://www.hawkknob.com) 的优质苹果酒。我强烈推荐鸡翅。最后，Sheri 和我爱吃早午餐，县城最好的地方是[木铁饮食店](https://www.woodironeatery.com)。我在北美的咖啡圣地——西雅图获得了博士学位。Wood Iron 的咖啡与西雅图的咖啡店不相上下，高度赞扬我是一个咖啡势利者(茶势利者也是，但那是另一个帖子)。你会发现他们在那里出售世界闻名的 J.Q. Dickinson 盐，有普通版本和烟熏版本。非常适合牛排、烧烤或配餐。大教堂咖啡馆有很好的食物，但咖啡还有待改进。如果你在费耶特维尔漂流、爬山或骑自行车，那么你需要去看看这些地方——它们彼此都在步行距离内。我希望当我们都从新冠肺炎瘟疫中走出来的时候，这些地方仍然存在。

# 剧情，终于！

在我压抑自己被困在俄克拉荷马州的新冠肺炎诱导的隔离状态之前，继续剧情。大多数情况下，下面的图是典型的`ggplot2`设置，除了我使用的非典型 geom，`geom_sf`从一个形状文件绘制数据，这是映射的必要条件。

我对`ggspatial`包进行了一次调用，以在地图的简化版上获得想要的北向罗盘。我试图改变“北”箭头的样式和颜色，但无济于事。不知道这是一个错误，还是我，但这就是为什么这个符号只出现在左边的地图上。

```
solar_light <- ggplot() + 
  geom_sf(data = big_streets$osm_lines, inherit.aes = FALSE, 
    color = "#585858", size = .8, alpha = .8) + 
  geom_sf(data = med_streets$osm_lines, inherit.aes = FALSE, 
    color = "#585858", size = .6, alpha = .6) + 
  geom_sf(data = small_streets$osm_lines, inherit.aes = FALSE, 
    color = "#585858", size = .4, alpha = .3) + 
  geom_sf(data = fayette_station, inherit.aes = FALSE, 
    color = "#d75f00", size = .6, alpha = .6) + 
  geom_sf(data = west_maple_ave, inherit.aes = FALSE, 
    color = "#d70000", size = .4, alpha = .5) + 
  geom_sf(data = maple_ave_west, inherit.aes = FALSE, 
    color = "#d70000", size = .4, alpha = .5) + 
  geom_sf(data = north_court_st, inherit.aes = FALSE, 
    color = "#0087ff", size = .6, alpha = .6) + 
  geom_sf(data = nr_gorge_bridge, inherit.aes = FALSE, 
    color = "#5f8700", size = .8, alpha = 1) + 
  geom_sf(data = new_river, inherit.aes = FALSE, 
    color = "#00afaf", size = 1, alpha = 1) +
  ggspatial::annotation_north_arrow(location = "tl", 
    which_north = "true", height = unit(5, "mm")) + 
    coord_sf(xlim = c(-81.150, -81.060), 
    ylim = c(38.010, 38.080),   expand = FALSE) + theme_void() + 
  geom_point(data=essential, aes(x=lon, y=lat), size = 1.5,
    alpha=.8, fill="#d75f00", color="#d75f00", pch=19, 
    inherit.aes = F) + 
  theme(panel.background = element_rect(fill = "#ffffd7"))
```

注意，我喜欢的地方的位置是在`theme_void`之后用我从谷歌地图复制的经度和纬度编码的。与 Connor 和 Josh 的主要例子不同的是，我使用 Ethan Schoonover 开发的曝光调色板来曝光地图的调色板。我是你的超级粉丝。马克·艾维的[日晒备忘单](http://www.zovirl.com/2011/07/22/solarized_cheat_sheet/)对了解调色板的工作方式大有帮助。下面是日晒黑暗版本的情节代码。除了背景颜色没有什么不同。

```
solar_dark <- ggplot() + 
  geom_sf(data = big_streets$osm_lines, inherit.aes = FALSE, 
    color = "#585858", size = .8, alpha = .8) + 
  geom_sf(data = med_streets$osm_lines, inherit.aes = FALSE, 
    color = "#585858", size = .6, alpha = .6) + 
  geom_sf(data = small_streets$osm_lines, inherit.aes = FALSE, 
    color = "#585858", size = .4, alpha = .3) + 
  geom_sf(data = fayette_station, inherit.aes = FALSE, 
    color = "#d75f00", size = .6, alpha = .6) + 
  geom_sf(data = west_maple_ave, inherit.aes = FALSE, 
    color = "#d70000", size = .4, alpha = .5) + 
  geom_sf(data = maple_ave_west, inherit.aes = FALSE, 
    color = "#d70000", size = .5, alpha = 1) + 
  geom_sf(data = north_court_st, inherit.aes = FALSE, 
    color = "#0087ff", size = .6, alpha = 1) + 
  geom_sf(data = nr_gorge_bridge, inherit.aes = FALSE, 
    color = "#5f8700", size = .8, alpha = 1) + 
  geom_sf(data = new_river, inherit.aes = FALSE, 
    color = "#00afaf", size = 1, alpha = 1) + 
  coord_sf(xlim = c(-81.150, -81.060), 
    ylim = c(38.010, 38.080), expand = FALSE) + 
  theme_void() + 
  geom_point(data=essential, aes(x=lon, y=lat), size = 1.5,
    alpha=.8, fill="#d75f00", color="#d75f00", pch=19, 
    inherit.aes = F) + 
  theme(panel.background = element_rect(fill = "#1c1c1c"))
```

最后，我想把这些图放在一起。这是我第一次使用`patchwork`包，但它真的很好。语法比旧的`grid.arrange`函数简单得多，它本身就相当简单。

```
library(patchwork) 
solar_fa <- solar_light + solar_dark solar_fa + plot_annotation( 
  title = “Fayetteville, WV”, 
  subtitle = “38.053°N / 81.104°W”) & 
  theme(plot.title = element_text(size = 50, 
                                  family = “baskerville”,
                                  face=”bold”,
                                  hjust=.5), 
        plot.subtitle = element_text(family = “playfair”, 
                                     size = 30, 
                                     hjust=.5, 
                                     margin=margin(2, 0, 5, 0))) ggsave(file=”fayetteville_str_map.jpg”, 
       units=”in”, 
       width = 6,
       height=4.5)
```

`patchwork`软件包也使得注释完成的情节变得容易，无论是整体还是小部分。我也终于谈到了字体。我喜欢老式的，甚至是中世纪的字体。谷歌字体提供了“Libre Baskerville”和“Playfair Display ”,分别取自 18 世纪和 19 世纪的本地字体。使用 Playfair 字体，我也可以得到我喜欢的老式数字，尤其是像所有权地图这样的东西。

瓦拉！！

![](img/842b09e34db91faeb848886a34ee65c3.png)

关于 OSM 的数据，我只想说一句。在地图的右上角，你会看到一条直线。那是新河峡大桥。然而，连接它和费耶特维尔的高速公路 *US 19* 却不在现场。US 19 穿过费耶特维尔向西延伸，出现在图表的最底部，以绿色显示，就像大桥一样。这是一个重大的疏忽，因为 *US 19* 是从布拉克斯顿公司到贝克利南部的主要公路，连接州际公路 *I79* 到 *I64* 和 *I77* 。它是州际公路系统的一部分，也是美国东部的紧急疏散路线之一。即使数据总体良好，也要小心。

*原载于 2020 年 3 月 27 日*[*https://www.samuelworkman.org*](https://www.samuelworkman.org/blog/the-county/)*。*

## 参考

卡尔、大卫和哈德利·韦翰。2013." Ggmap:用 Ggplot2 实现空间可视化."*R 轴颈*5(1):144–61。[https://journal . r-project . org/archive/2013-1/kahle-Wickham . pdf](https://journal.r-project.org/archive/2013-1/kahle-wickham.pdf)。