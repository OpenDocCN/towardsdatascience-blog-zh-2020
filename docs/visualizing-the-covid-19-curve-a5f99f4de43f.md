# 可视化新冠肺炎曲线

> 原文：<https://towardsdatascience.com/visualizing-the-covid-19-curve-a5f99f4de43f?source=collection_archive---------41----------------------->

## 每个人都知道我们需要使曲线变平。我们进展如何？

![](img/2a454d7d19224c678fd9fe0f4f22d063.png)

图片: [navy.mil](https://www.cpf.navy.mil/COVID19/)

*《纽约时报》*[发布了一组数据](https://github.com/nytimes/covid-19-data),让我们可以查看美国各州、各县的新冠肺炎和相关死亡病例的传播情况。

我们将利用这些数据来观察病毒传播迅速的两个地方:纽约和加利福尼亚。这些数据意义重大，不仅因为它们是最大的州，还因为这些数据可以让我们深入了解社会隔离和就地安置等“曲线拉平”程序有多有效。

但是，由于这只是对数据的快速回顾和可视化，我们将尽量不要得出太多的结论。事实上，我们可以从数据中得出的一个结论是，基于简单的分析就自满太容易了。另一个结论是，即使在加州，我们的情况也很糟糕。除此之外，我邀请你检查数据并得出你自己的结论。

# 用 Python 提取数据

如果您对提取数据的技术方面不感兴趣，请随意跳到下一节，我们将在这里查看线图。

首先，我们从*纽约时报的* [github 账号](https://github.com/nytimes/covid-19-data) *中提取数据。*一旦我们建立了这个过程，我们可以简单地重新运行相同的代码，每天重新提取数据，以获得最新的数据集。本页结果完成于 2020 年 3 月 29 日。毫无疑问，接下来的几天和几周，剧情会有所不同。

一个文件是县的，一个是州的，两者都包括累积的(不是新的，而是总数)病例和死亡。

```
import pandas as pdcounties_url = '[https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-counties.csv'](https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-counties.csv')
df = pd.read_csv(counties_url)
cal_counties_df = df[df['state'] == 'California']
ny_counties_df = df[df['state'] == 'New York']states_url = '[https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv'](https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv')
df = pd.read_csv(states_url)
ny_df = df[df['state'] == 'New York']
cal_df = df[df['state'] == 'California']
```

请注意，第一次感染发生在纽约(3 月 1 日)晚于加州(1 月 25 日)，因此纽约的数据集不包括任何更早的日期。您可以通过运行以下命令来验证这一点:

```
cal_df['date'].min()
ny_df['date'].min()
```

为了使折线图正确排列，我们需要将那些具有零值的较早日期添加到纽约数据帧中。

```
ny_add_dates = pd.date_range(start=cal_df['date'].min(), end=ny_df['date'].min()).sort_values(ascending=True)[0:-1]ny_append_list = []for dt in ny_add_dates:
    ny_append_list.append([dt.strftime("%Y-%m-%d"), 'New York', 36, 0, 0])
new_ny = pd.DataFrame(
    ny_append_list,
    columns = ["date", "state", "fips", "cases", "deaths"])ny_extended_df = pd.concat([ny_df, new_ny]).sort_values(by='date')
```

现在，让我们设置 Python 方法，这将允许我们进行一些绘图:

```
import matplotlib.pyplot as plt# Plot a single line/location
def plot_loc(loc_df, name, col_name):
    fig, ax = plt.subplots()
    loc_df.plot(ax=ax, x='date', y=col_name, label=name, kind = 'line')
    plt.setp(ax.get_xticklabels(), rotation=30, horizontalalignment='right')
    plt.show()# Plot multiple lines/locations
def plot_locs(loc1, loc2, loc1_name, loc2_name, col_name):
    ax = loc1.plot(x='date', y=col_name, label=loc1_name, kind='line')
    loc2.plot(ax=ax, x='date', y=col_name, label=loc2_name, kind='line')
    plt.setp(ax.get_xticklabels(), rotation=30, horizontalalignment='right')
    plt.show()
```

# 看着这些数据

首先，我们将看看加利福尼亚州与纽约州的比赛:

```
plot_locs(cal_df, ny_extended_df, 'CA', 'NY', 'cases')
```

![](img/66feab9dba6e2a3af2514e021bd26616.png)

纽约(橙色线)和加州(蓝色线)的感染情况

如果你想对纽约和加州的感染情况做一个简单的比较，那就是了。如果你想知道纽约的情况有多糟糕，那就再考虑几件事情。首先，纽约比加州晚了一个多月才出现第一例感染。其次，加州的面积是纽约的两倍多。

换句话说，纽约的情况很糟糕。

但是我们加州人不应该如此自满。这可能不是因为我们在控制病毒方面比纽约做得更好(尽管也许我们的行动有所帮助)，这可能只是因为纽约受到的打击要严重得多。加州早期的就地安置行动可能已经产生了影响(虽然我们不能仅仅通过查看这些数据来说)，但加州的情况仍然非常糟糕。

让我们来看看纽约和加州各自的地块，而不是将加州的景色挤进纽约旁边的地块:

```
plot_loc(cal_df, 'CA', 'cases')
plot_loc(ny_extended_df, 'NY', 'cases')
```

![](img/b521f5e5daa82d86e387b6b572db6811.png)

纽约(左)和加州(右)的感染情况

注意 y 轴(每个图左侧的数字)。左侧图中纽约的最大值为 40，000。对于右边的加利福尼亚，最大值是 5000。换句话说，纽约的感染人数是加州的近十倍。这与我们看到的第一个联合地块非常一致。

但是曲线并没有完全不同。纽约和加州的感染率都呈指数级增长。换句话说，加州落后于纽约，但以这种速度，我们将很快赶上他们现在的水平。

现在，让我们问一个不同的问题:为什么纽约如此糟糕？从这些数据中很难回答这个问题，但是我们可以问*纽约哪里这么糟糕？*这个就好回答多了，应该很明显:纽约市。

我们可以用下面的代码提取纽约市的数据:

```
nyc_df = ny_counties_df[ny_counties_df['county']=='New York City'].sort_values(by='date')
nyc_add_dates = pd.date_range(start=cal_df['date'].min(), end=nyc_df['date'].min()).sort_values(ascending=True)[0:-1]nyc_append_list = []for dt in nyc_add_dates:
    nyc_append_list.append([dt.strftime("%Y-%m-%d"),'New York City', 'New York', 36, 0, 0])
new_nyc = pd.DataFrame(
    nyc_append_list,
    columns = ["date", "county", "state", "fips", "cases", "deaths"])nyc_extended_df = pd.concat([nyc_df, new_nyc]).sort_values(by='date')
```

请注意，*纽约时报*数据将所有纽约市列在一个名为“纽约市”的县下，而不是五个区中的每一个区下。

将纽约市与纽约州的其他城市相比较，我们会看到以下情况:

![](img/bf94489a78e1f7622c463f19ed36841f.png)

纽约市所有五个区(蓝线)和整个纽约州的感染情况

蓝线是纽约市，蓝线以上到橙线的所有区域都是州内城市以外的额外感染，包括附近和接壤的县。所以我们可以看到，不出所料，纽约市占了该州感染人数的很大一部分。考虑到纽约州的人口集中在纽约市，这是意料之中的。

但是，为了了解纽约市的情况有多糟糕，让我们将它与加州进行比较:

![](img/b1676dc7f7ab6c593dc4dbd2208b9467.png)

同样，我们不应该在加州过于自满，但要注意的是，仅纽约市*的情况就相当糟糕。*

最后，关于指数增长的一个注记。如果你已经学习了计算机科学的入门理论，不仅仅是编程，还有算法分析，一个很大的收获就是指数增长率是不好的。我们经常听到这个词*棘手，*也就是说，一个非常难以处理的问题，即使对于相对较小的指数曲线也是如此。

与纽约相比，加州的感染增长率可能相对较小，但仍呈指数增长，这仍然是一个非常糟糕的预兆。