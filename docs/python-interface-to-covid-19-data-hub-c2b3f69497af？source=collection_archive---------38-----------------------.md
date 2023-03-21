# 新冠肺炎数据中心的 Python 接口

> 原文：<https://towardsdatascience.com/python-interface-to-covid-19-data-hub-c2b3f69497af?source=collection_archive---------38----------------------->

## 统一数据集有助于更好地了解新冠肺炎。

***编者注:*** [*走向数据科学*](http://towardsdatascience.com/) *是一份以研究数据科学和机器学习为主的中型刊物。我们不是健康专家或流行病学家，本文的观点不应被解释为专业建议。想了解更多关于疫情冠状病毒的信息，可以点击* [*这里*](https://www.who.int/emergencies/diseases/novel-coronavirus-2019/situation-reports) *。*

![](img/1697514e35e864a32ae75287ac752066.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=4948866) 的[米洛丝拉娃·克里斯诺娃](https://pixabay.com/users/MiroslavaChrienova-6238194/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=4948866)

新冠肺炎数据中心的目标是通过收集世界范围内的精细案例数据，结合有助于更好地了解新冠肺炎的外部变量，为研究社区提供一个统一的数据集。请同意[使用条款](https://covid19datahub.io/LICENSE.html)，并在使用时引用以下参考:

**参考**

Guidotti，e . Ardia，d .(2020 年)。
https://doi.org/10.21105/joss.02376 数据中心
*开源软件杂志*，**5**(51):2376
T37

## 设置和使用

从[管道](https://pypi.org/project/covid19dh/)安装

```
pip install covid19dh
```

导入主功能`covid19()`

```
from covid19dh import covid19
x, src = covid19()
```

## 返回值

函数`covid19()`返回 2 个熊猫数据帧:

*   数据和
*   对数据源的引用。

## 参数化

## 国家

国家名称(不区分大小写)或 ISO 代码(字母 2、字母 3 或数字)的列表。ISO 代码列表可在的[处找到。](https://github.com/covid19datahub/COVID19/blob/master/inst/extdata/db/ISO.csv)

从特定国家获取数据:

```
x, src = covid19("USA") # Unites States
```

同时指定多个国家:

```
x, src = covid19(["ESP","PT","andorra",250])
```

如果省略`country`，则返回整个数据集:

```
x, src = covid19()
```

## 原始数据

合乎逻辑。跳过数据清理？默认`True`。如果是`raw=False`，则通过用`NaN`值填充缺失的日期来清除原始数据。这确保了所有地点共享相同的日期网格，并且没有一天被跳过。然后，`NaN`值被替换为之前的非`NaN`值或`0`。

```
x, src = covid19(raw = False)
```

## 日期过滤器

日期可以用`datetime.datetime`、`datetime.date`或格式`YYYY-mm-dd`中的`str`指定。

```
from datetime import datetime
x, src = covid19("SWE", start = datetime(2020,4,1), end = "2020-05-01")
```

## 水平

整数。数据的粒度级别:

1.  国家一级
2.  州、地区或县一级
3.  城市或自治市一级

```
from datetime import date
x, src = covid19("USA", level = 2, start = date(2020,5,1))
```

## 隐藏物

合乎逻辑。内存缓存？显著提高连续呼叫的性能。默认情况下，启用使用缓存数据。

可以通过以下方式禁用缓存(例如，对于长时间运行的程序):

```
x, src = covid19("FRA", cache = False)
```

## 过时的

合乎逻辑。检索在`end`日期生成的数据集快照，而不是使用最新版本。默认`False`。

例如，获取 2020 年 4 月 22 日*可访问的美国数据*类型

```
x, src = covid19("USA", end = "2020-04-22", vintage = True)
```

葡萄酒数据在一天结束时收集，但在所有时区的一天结束后，会延迟大约 48 小时发布。

因此，如果`vintage = True`未置位，但`end`未置位，则发出警告并返回`None`。

```
x, src = covid19("USA", vintage = True) # too early to get today's vintageUserWarning: vintage data not available yet
```

## 数据源

数据源作为第二个值返回。

```
from covid19dh import covid19
x, src = covid19("USA")
print(src)
```

## 结论

新冠肺炎数据中心协调了疫情各地的大量异构数据。它代表了新冠肺炎在开放公共数据标准和共享方面的首次努力。使用新冠肺炎数据中心的出版物可在[此处](https://scholar.google.com/scholar?oi=bibs&hl=en&cites=1585537563493742217)获得。

## 感谢

新冠肺炎数据中心得到了加拿大数据评估研究所的支持。 [covid19dh](https://pypi.org/project/covid19dh/) 包是由[马丁·贝内什](https://pypi.org/user/martinbenes1996/)开发的。

[1]吉多蒂，即阿尔迪亚，d .(2020)。[新冠肺炎数据中心](https://doi.org/10.21105/joss.02376)，《开源软件杂志》，5(51):2376