# 建立一个简单的网页刮新冠肺炎可视化与散景按地区

> 原文：<https://towardsdatascience.com/building-a-simple-web-scraped-covid-19-visualization-with-bokeh-by-region-84aa351f668?source=collection_archive---------29----------------------->

## 每个数据问题都始于良好的可视化

![](img/6509a313ecce2f31fda3e051d82e193c.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Jochem Raat](https://unsplash.com/@jchmrt?utm_source=medium&utm_medium=referral) 拍摄的照片

就像任何数据爱好者所说的那样；有洞察力的数据可视化使我们能够获得对问题的直觉，对于任何类型的问题解决都是一个很好的起点，而不仅仅是数据繁重的问题。

在这个项目中，我致力于开发一个可视化工具，让人们了解我所在地区(加拿大安大略省)的哪些县市仍然是当前新冠肺炎疫情的重灾区。

您将需要什么:

1.  要可视化的区域的 shapefile。我使用了大多数 GIS(地理信息软件)生成的. shp 文件格式。这里有一个[来源](http://www.geographynetwork.ca/website/obm/viewer.htm)的链接。(这是近 20 年前的资料，因此不是最新的)
2.  Bokeh 和 GoogleMaps 包已安装。在终端/命令提示符下安装软件包的代码如下:

```
**pip** **install** bokeh
**pip install** googlemaps
```

接下来，我们加载数据和必要的库。

```
**import** requests
**import** numpy as np
**import** pandas as pd
**import** googlemaps
**import** geopandas as gpd
**import** seaborn
**from** shapely.geometry **import** Point, Polygon
```

下面这段简单的代码支持将数据集自动下载、写入(或覆盖)到所选的目的地。

```
url = ‘[data_source_url'](https://data.ontario.ca/dataset/f4112442-bdc8-45d2-be3c-12efae72fb27/resource/455fd63b-603d-4608-8216-7d8647f43350/download/conposcovidloc.csv')
myfile = requests**.get**(url)
**open**(‘data/conposcovidloc.csv’, ‘wb’).**write**(myfile.**content**)
```

这可能是一个简单的周末项目的很大一部分原因是因为 data.ontario.ca 的研究员保持了数据的干净。然而，我们确实需要将数据集塑造成更好地用于此目的的方式。

```
covid_19 = pd.**read_csv**(‘covid_19_data_path.csv’)cv0 = pd.**get_dummies**(covid_19, columns = [‘OUTCOME1’,’CLIENT_GENDER’,’Age_Group’])
# this creates dummy variables for the categorical variables names
cv0 = cv0.**drop**(cv0.columns[0],axis = 1)
cv0 = cv0.**replace**(0,np.nan)cv1 = cv0.**groupby**(['Reporting_PHU','Reporting_PHU_Latitude','Reporting_PHU_Longitude','Reporting_PHU_City']).**count**().**reset_index**()
```

上面的代码块做了几件好事。首先，它使分类变量的结果，客户性别和年龄组列。但是，这就产生了 np。NaN 对象，我们希望在新创建的分类变量中使用零。上面的最后一行按可视化和。count()方法将 NaN 对象转换为零。

```
Counties = gpd.**read_file**(‘geo_data_path.shp’)
Counties = Counties.**sort_values**(‘OFF_NAME’)
for **i,v** in **enumerate**(city_order):
 city = cv1[‘Reporting_PHU_City’][v]
 county_city.**append**(city)

Counties[‘CITY’]=county_city
```

接下来，我们加载了地理数据。本项目中使用的地理数据源不包含报告新冠肺炎病例的公共卫生城市的数据。 *city_order* 是一个包含列表索引的列表，我们可以从新冠肺炎数据集中的列 Reporting _ PHU _ 城市生成该列表。这为合并两个数据集创建了一个起点。

```
# longitude must always come before latitude
geometry = [**Point**(xy) for xy in zip(cv1[‘Reporting_PHU_Longitude’],cv1[‘Reporting_PHU_Latitude’])]
geo_cv = gpd.**GeoDataFrame**(cv1, geometry = geometry)
```

这个简单的代码块做得相当不错。它根据城市的经度和纬度创建几何图形，并由此创建新的地理数据框架。

接下来，我使用*均值归一化*进行了一些基本的特征缩放，以产生一个更具代表性的疫情迄今为止受灾最严重的城市分布。然后，新列应合并成一个数据帧，用于散景图。

以下是数据帧的快照:

最后两个代码块在 GoogleMaps 库的帮助下生成散景图。

```
import json
from **bokeh**.io import output_notebook, show, output_file
from **bokeh**.plotting import figure
from **bokeh**.models import GeoJSONDataSource,LinearColorMapper, ColorBar, Label, HoverTool
from **bokeh**.models import Range1d, ColorMapper
from **bokeh**.models.glyphs import MultiLine
from **bokeh**.palettes import brewer, mplapi_key = ‘***YOUR_API_KEY***’
gmaps = googlemaps.**Client**(key=api_key)#Read data to json and convert to string-like object
Counties_json = json.**loads**(Counties.**to_json**())
json_data = json.**dumps**(Counties_json)#Provide GeoJSONData source for plotting.
geosource = GeoJSONDataSource(geojson = json_data)#Repeat for geo_cv GeoPandas DataFrame
geo_csv_json = json.**loads**(geo_cv.**to_json**())
json_data2 = json.dumps(geo_csv_json)geosource2 = GeoJSONDataSource(geojson = json_data2)
```

而对于剧情本身。

```
fig = figure(title = ‘Ontario Covid 19 Cases by Municipality’, plot_height = 750 , plot_width = 900
 ,active_scroll=’wheel_zoom’)
fig.**xgrid**.**grid_line_color** = None
fig.**ygrid**.**grid_line_color** = None# initialize the plot on south and eastern ontario
left, right, bottom, top = -83.5, -74.0, 41.5, 47.0
fig.**x_range**=Range1d(left, right)
fig.**y_range**=Range1d(bottom, top)# Creating Color Map by Recovery Rate
palette1 = brewer[‘RdYlGn’][11] 
cmap1 = LinearColorMapper(palette = palette1,
 low = Counties[‘PERCENTAGE_TOTAL_log’].**min**(),
 high = Counties[‘PERCENTAGE_TOTAL_log’].**max**())cmap2 = LinearColorMapper(palette = palette1,
 low = geo_cv[‘TOTAL_CASES’].**min**(),
 high = geo_cv[‘TOTAL_CASES’].**max**())#Add patch renderer to figure. 
f2 = fig.**patches**(‘xs’,’ys’, source = geosource, line_color = ‘black’, line_width = 0.25, line_alpha = 0.9, fill_alpha = 0.6,
 fill_color = {‘field’:’PERCENTAGE_TOTAL_log’,’transform’:cmap1})fig.**add_layout**(ColorBar(color_mapper=cmap2, location=’bottom_right’,label_standoff=10))hover2 = HoverTool(renderers = [f2], tooltips = [(‘Municipality Name’,’[@OFF_NAME](http://twitter.com/OFF_NAME)’)])
fig.add_tools(hover2)f1 = fig.**circle**(x=’Reporting_PHU_Longitude’,y=’Reporting_PHU_Latitude’,size = 10,
 color = ‘black’, line_color = ‘white’, source = geosource2, fill_alpha = 0.55)
hover1 = HoverTool(renderers = [f1], tooltips=[(‘Health Unit’,’[@Reporting_PHU](http://twitter.com/Reporting_PHU)’)
 ,(‘Health Unit City’,’[@Reporting_PHU_City](http://twitter.com/Reporting_PHU_City)’)
 ,(‘Fatalities’,’[@Outcome1_Fatal](http://twitter.com/Outcome1_Fatal)’)
 ,(‘Percentage Recovered’,’[@PERCENTAGE_Recovered](http://twitter.com/PERCENTAGE_Recovered)’),(‘Total Cases’,’[@TOTAL_CASES](http://twitter.com/TOTAL_CASES)’)
 ,(‘Male to Female Ratio’,’[@GENDER_RATIO1](http://twitter.com/GENDER_RATIO1)')])bfig.**add_tools**(hover1)
output_notebook()
show(fig)
```

# 感谢您的关注，我相信您会发现这很有价值

请随时通过以下平台联系我

[Twitter](https://twitter.com/MeloKanjo) ， [LinkedIn](https://www.linkedin.com/in/kanjo-melo/) ， [Github](https://github.com/jomelo23) 或者在 Medium 上关注我！