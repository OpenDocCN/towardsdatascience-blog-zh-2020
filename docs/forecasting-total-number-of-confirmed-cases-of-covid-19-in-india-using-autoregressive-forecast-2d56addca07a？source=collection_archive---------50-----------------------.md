# 用自回归预测模型预测印度新冠肺炎确诊病例总数

> 原文：<https://towardsdatascience.com/forecasting-total-number-of-confirmed-cases-of-covid-19-in-india-using-autoregressive-forecast-2d56addca07a?source=collection_archive---------50----------------------->

## 使用自回归预测 COVID 的全部确诊病例

***编者按:*** [*走向数据科学*](http://towardsdatascience.com) *是一份以数据科学和机器学习研究为主的中型刊物。我们不是健康专家或流行病学家，本文的观点不应被解释为专业建议。想了解更多关于疫情冠状病毒的信息，可以点击* [*这里*](https://www.who.int/emergencies/diseases/novel-coronavirus-2019/situation-reports) *。*

![](img/9fb9ba86a0a7ea08a9bb94077ef85f04.png)

作者图片

作为我之前关于新冠肺炎数据端点及其可视化的文章的续篇，这里有一个快速的方法可以让你建立一个简单的时间序列预测模型

对于那些不熟悉什么是时间序列预测的人来说:时间序列是按时间顺序索引(或列出或绘制)的一系列数据点。最常见的是，时间序列是在连续的等间隔时间点取得的序列。在预测设置中，我们发现自己在时间 t，我们对估计 Y(t+h)感兴趣，只使用时间 t 可用的信息。

我个人用过自回归预测模型。自回归是一种时间序列模型，它使用以前时间步长的观测值作为回归方程的输入，来预测下一个时间步长的值。这是一个非常简单的想法，可以导致对一系列时间序列问题的准确预测。

*   和往常一样，我们从导入所需的模块开始

```
import requests
import pandas as pd
from statsmodels.tsa.ar_model import AR
```

然后，我们从相应的端点获取所需的数据，并用它来训练自回归模型。

```
x = requests.get('https://api.covid19api.com/total/country/india/status/confirmed').json()
df = pd.DataFrame.from_dict(x)
model = AR(df['Cases'][:-1])
model_fit = model.fit()
```

在这里，为了进行培训，我只使用了截至 2020 年 4 月 17 日的数据，以便我可以用它来预测 2020 年 4 月 18 日的确诊病例总数，并将其与实际值进行比较。

现在，为了预测 18–04–2020 的值(即数据列表中的第 87 个值，因此预测值旁边的数字是 87)，我已经将开始和结束索引参数作为输入数据的总长度传递给预测函数，这将给出所需预测的索引(即，如果我有 87 个值，则最后一个输入值的索引将是 86，作为开始和结束参数传递的值将是 87，告诉它预测第 87 个值)。

```
prediction_for_18_04_2020 = model_fit.predict(start=len(df['Cases'][:-1]), end=len(df['Cases'][:-1]))
prediction_for_18_04_2020
```

这给了我一个大概的预测。15879，非常接近实际值 15722

![](img/40a26d840bafe4e96a94335af039c35c.png)

作者图片

**供你参考:**

传递自回归模型索引的数据时，将数据作为“['Cases'][:-1]”传递，以便仅传递数据帧的已确认病例列，并且“:-1”用于跳过最后一个值，即日期 18–04–2020 的值。

当你训练一个模型来拟合它时，拟合一个模型意味着在数据中找到一个模式。这就是我在培训过程中使用 model.fit()的原因。

开始和结束参数:这些参数用于定义要预测多少个值。在我的例子中，我为它们传递了相同的数字，因为我只想预测一个值。但是，如果您想让我们预测未来 4 天的确诊病例总数，那么在“开始”中，您将传递从预测开始的时间点的索引，在“结束”中，您将传递到需要预测值的时间点，即，由于您的列表在 86 索引处结束，您可以静态地传递“开始”作为 87，传递“结束”作为 90，或者如果您想要传递它 动态地在“开始”发送数据的长度，然后在“结束”传递总长度+ 3，这将会给你相同的结果

关于 statsmodels AR()库需要注意的一点是，它很难以“在线”方式使用(例如，训练一个模型，然后在新数据点出现时添加它们)。您需要根据添加的新数据点重新训练您的模型，或者只保存模型中的系数并根据需要预测您自己的值。

你可以在这里找到我的代码:

[](https://github.com/nischalmadiraju/COVID-19-Autoregression-forecasting-with-indian-Data) [## nischalmadiraju/新冠肺炎-用印度数据进行自回归预测

### 通过创建一个帐户，为 nischalmadiraju/新冠肺炎-用印度数据进行自回归预测的发展做出贡献…

github.com](https://github.com/nischalmadiraju/COVID-19-Autoregression-forecasting-with-indian-Data) 

请随意浏览，如果您想让我介绍其他内容，也请在评论中告诉我