# 销售和数据科学:使用 Python 分析竞争对手的分步指南

> 原文：<https://towardsdatascience.com/sales-data-science-a-step-by-step-guide-to-competitor-analysis-using-python-7dc03461d284?source=collection_archive---------18----------------------->

## 是什么让卖家在亚马逊或易贝这样的在线市场上获得成功？数据科学有助于回答这个问题。

像亚马逊或易贝这样的在线市场到处都是卖家，每一步都在互相竞争。当其他人提供的产品与你的非常相似时，增加销售并不容易。

你有没有想过，如果卖家提供的东西都差不多，是什么让买家选择一个卖家而不是另一个？

你可能会认为这都是价格的问题。

**我也是，但后来我禁不住诱惑，想看看这里是否有其他因素在起作用。**

在本文中，我站在一个在线卖家的角度，来看看使用 Python 编程工具的数据驱动方法如何帮助回答这个问题。

![](img/4370acff9f39989f025ce2a7a7f435c1.png)

*来源:我的团队在*[*Sunscrapers*](https://sunscrapers.com/?utm_source=medium&utm_medium=article)*提供。*

为此，我采取了两个步骤。

**第一步:找到一个有丰富销售历史内容的在线市场。**这篇文章是为教育目的而写的，所以我想对市场名称保密。

第二步:想出一个可以比较不同报价的方法。这里的诀窍是找到一种产品，它有许多相似的变体，并由大量的卖家出售。我为这项研究挑选的在线市场上最常出售的产品之一是我们都在使用的:

**一款智能手机屏幕保护器**。

**在这篇文章中，我要问和回答 4 个问题:**

Q1。我们产品类型的平均价格是多少？

Q2。定价如何影响销售？

**Q3。最受欢迎的屏幕保护器品牌有哪些？**

**Q4。哪些品牌最赚钱？**

# 贸易工具

python——我们为数据科学选择的编程语言。

Python 工具:

*   scrapy——这个 web 抓取/爬行框架提供了一些便利的特性，比如字段值的编组和预处理。你可以通过在线清理平台运行它，这有助于减轻你的抓取过程。
*   Pandas —我将使用它将数据加载到表中(然后清理、处理和分析它)。
*   Seaborn 和 Matplotlib——这是 Python 中一些方便的数据可视化库。

顺便说一句。我还写了两篇文章来帮助你研究熊猫，看看吧:

[如何在熊猫小组中运用分-用-合策略](/how-to-use-the-split-apply-combine-strategy-in-pandas-groupby-29e0eb44b62e)

[如何用熊猫和 Dask 在 Python 中处理大型数据集](/how-to-handle-large-datasets-in-python-with-pandas-and-dask-34f43a897d55)

# 数据挖掘和准备

# 1.获取数据

第一步是找到数据源。我需要选择一个能够提供某种销售业绩指标的在线市场——这样我就可以根据其他产品特性对其进行评估。我选的平台提供了最近 100 笔交易的信息。

注意:网络爬虫代码内容相当广泛，并且会因不同的市场网站而异，所以我决定不在本文中包含抓取代码的例子。用于抓取和网页抓取的 Python 框架 Scrapy 提供了大量文档和易于理解的教程，所以如果需要的话可以参考它们。

以下是我如何获得本文数据的简短描述:

首先，我在网上市场手动搜索智能手机屏幕保护器，并从那里开始爬行过程。通常，搜索模式从某种商品列表开始，每个列表指向一个包含更多信息的商品专用页面，甚至可能是过去购买的列表。

请注意，您通常可以在搜索结果列表中获得关于产品展示的有价值信息(如卖家状态、积分、过去购买次数)。毕竟，这是客户第一次接触该优惠，这可能会影响他们的决策过程。

爬行之后，我得到了两张熊猫桌子。主表(`df`)包含所有的产品信息，每一行对应一个商品。另一个表(`sale_history`)存储了关于过去购买的信息，每个产品包含许多行单独的销售事件数据。

稍后我将向您展示表格示例。

# 2.处理数据

在数据提取步骤之后，是时候做一些清理和数据准备了。除了所有通常的步骤(删除空值、将列转换成正确的数据类型等。)，这里有几个有趣的步骤我想提一下——同样，不用深入细节。

作为第一步，我倾向于用熊猫`unique()`的方法浏览各个专栏。这就是我如何能看到价值观是否一致和明智——并捕捉任何潜在的问题。然后，我通过按列对行进行分组来检查数据重复，该列是特定项目的惟一标识符——在本例中，我使用了`product_id`。

我注意到的第一件事是，一些产品页面链接到搜索结果页面中的多个列表(巧合？我不这么认为！).我去掉了重复的，但是决定保留这些信息用于分析。因此，我创建了一个新列，首先列出每件商品的列表数量，然后删除了副本，只留下一个:

```
df[‘same_offer_count’] = df.groupby(‘product_id’)[‘product_id’]\
    .transform(‘count’)
df = df.drop_duplicates(subset=’product_id’, keep=’first’)
```

另一个有趣的问题是处理市场上使用的多种货币。我的原始数据表包含裸价格字符串值以及带引号的货币符号(例如，“US $1.09”或“C $2.42”)，因此我需要提取数值，并通过将它们转换为 USD 来统一所有价格货币。以下是转换前的几个示例行:

![](img/4a5583ddf40a1e56ce245807124ea82c.png)

下面是我用来转换它的代码:

```
import refrom currency_converter import CurrencyConvertercc = CurrencyConverter()
currency_shortcuts = {‘C’:’CAD’, ‘US’:’USD’, ‘AU’:’AUD’} # first I checked only these occur…
regx_str=r’(\w+\s*)\$[ ]?(\d+[.|,]?\d+)’ # note the two ‘re’ groups!df[[‘currency’, ‘quoted_price’]] = df[‘current_price’]\
    .str.extract(pat=regx_str)df[‘currency’] = df[‘currency’].str.replace(‘ ‘, ‘’)
df[‘currency’] = df[‘currency’].map(currency_shortcuts)
df[‘price_USD’] = df[‘quoted_price’].copy()for currency in [ c for c in df[‘currency’].unique() 
                  if c not in [‘USD’]]:
    fltr = df[‘currency’].isin([currency]) 
    df.loc[fltr, ‘price_USD’] = df.loc[fltr, ‘quoted_price’]\
        .apply(lambda x: cc.convert(x, currency, ‘USD’))
```

这导致了:

![](img/0b3f6b0a60041a84d27ba2dd8be25438.png)

接下来，我处理了销售历史表(`sale_history`)。我执行了一些基本的类型修复，提取并转换了价格和货币，并填充了空值(代码未显示)。我最终得到了这个表(同样，它只是行的快照):

![](img/cad3b444e025e30475f392ea2b2fb6a2.png)

为了让它对我的分析和绘图有用，我按日期(当然还有`product_id`)汇总了条目，并计算了售出商品的数量和每日销售率。将所有这些打包到一个函数中，可以将它按行应用于数据框:

```
def calculate_sale_history_stats(df):
    *“””Calculates statistics on sale history, 
    returns new dataframe”””* delta = df[‘purchase_date’].max() — df[‘purchase_date’].min()
    days = int(delta.days)
    values = list(df[‘quantity_sold’])
    earnings = list(df[‘total_price’])
    sold_count = sum(values)

    if len(values) < days:
        values.extend([0]*(len(values) — days))
        earnings.extend([0]*(len(earnings) — days))

    res = pd.Series(
        [ sold_count, np.mean(values), 
          np.std(values), np.mean(earnings), 
          np.std(earnings)
        ], 
        index=[‘Sold_count’, ‘Mean_daily_sold_count’,
               ‘Sold_count_St.Dev’, ‘Daily_earnings’,
               ‘Daily_earnings_St.Dev’]
    )

    return round(res, 2)
```

并将其应用于`sale_history`数据帧:

```
sale_history_stats = sale_history.groupby(‘brand’)\
    .apply(calculate_sale_history_stats)
```

导致了:

![](img/f8c5c98927b52245303cdd0153c1ad6b.png)

最后，我将汇总的销售统计数据(`sale_history_stats`)合并到主 df 表中:

```
df = pd.merge(
    how=’left’,
    on=’product_id’,
    left=aggreg_sale_history,
    right=df[[‘product_id’,’shipping_cost’, ‘shipping_from’,
              ‘top_rating_badge’, ‘seller_feedback_score’, 
              ‘seller_feedback_perc’,]]
)
```

下面是生成的`df`表(同样，只显示了选择的列):

![](img/9e860e2f2d1c2b716d24d53a2dbee30b.png)

现在我们可以走了。因此，让我们开始我们的竞争对手分析。

# Q1:我们这种产品的平均价格是多少？

让我们看看我们能从屏幕保护装置中获得多少利润。这类产品卖家一般收费多少？

我可以分析市场上的价格，看看客户通常会为这样的智能手机屏幕保护器支付多少钱:

```
import matplotlib.pyplot as plt
import seaborn as snsprices = df[‘price_USD’]sns.distplot(df[‘price_USD’], ax=ax, bins=200)paid_prices = sale_history[‘sell_price’]sns.distplot(paid_prices, ax=ax, bins=100)
```

下面是两个叠加了附加信息的直方图(代码未显示):

![](img/d17b541053309445fb162f0e8204edb3.png)![](img/905affd36e43a9b3b01cee338feb40b9.png)

如你所见，大多数屏幕保护器的价格约为 1.15 美元(平均约为 3.9 美元)。然而，顾客似乎更喜欢在购买时额外投入一些钱(平均约 5 美元，中位数约 3.8 美元)。“越便宜越好”的规则在这里不适用。

基于这种认识，我们可以假设选择将我们的产品定价在 4 美元左右就可以了。

# Q2:定价如何影响销售？

定价可能是客户决策过程中最重要的因素。销售者通常认为高价格会阻止消费者购买他们的产品。在盈利性和可承受性之间取得恰当的平衡可能会成为一项挑战。

让我们看看售出商品的数量和每日收入如何与单价相匹配(作为每日平均值):

```
*# The daily earnings vs price:*
sns.lmplot(
    x=’sell_price’, 
    y=’Daily_earnings’, 
    data=df[[‘sell_price’, ‘Daily_earnings’]]
)*# Plot the sales frequency vs price:*
sns.lmplot(
    x=’sell_price’, 
    y=’Mean_daily_sold_count’, 
    data=df[[‘sell_price’, ‘Mean_daily_sold_count’]]
)
```

![](img/0491bc5c1cbf2252019620789eeb2dd8.png)![](img/064f98597ad3d7c884f24768e5c0490c.png)

不出所料，更高的价格意味着平均销量的下降。但是当我们看每天的收入时，似乎利润倾向于随着更高的价格而增加。

在这一点上，发现定价是否反映了产品的质量和/或声誉是很有意思的。不幸的是，这超出了本研究的范围。

# Q3。最受欢迎的屏幕保护器品牌有哪些？

让我们仔细看看品牌名称。有一系列的价值观指向“没有品牌”

所以，让我们先把这些乱七八糟的东西收拾干净，给它们都贴上“无品牌”的标签:

```
ubranded_values = [ 
    ‘Does not apply’, 
    ‘Does Not Apply’, 
    ‘Unbranded/Generic’, 
    ‘unbranded’
]df[‘brand’] = df[‘brand’].apply(
    lambda s: ‘Unbranded’ if s in ubranded_values else s
)
```

![](img/a11604b3a14266a8d3c5b2a6ca596d39.png)

现在，我可以将数据输入到工作中，并得到一个显示品牌名称的饼图。这表明我们在线市场上提供的大多数产品(约 60%)根本没有品牌(或没有标明品牌)。

消费者可能希望坚持一个可识别的名称，所以让我们暂时忽略未命名的产品，而是专注于市场上每天卖出最多产品的前 20 个品牌。

为此，我将使用包含所有事务数据的`sale_history`表。

让我们创建一个包含市场上提供的品牌信息的表格:

```
sold_brands = sale_history\
    .groupby(‘brand’)\
    .apply(calculate_sale_history_stats)
```

接下来，让我们来看看迄今为止销量最高的 10 个品牌——创建一个表格并绘制成这样:

```
top_sold_brands = sold_brands.sort_values(
    by=[‘Sold_count’, ‘Daily_earnings’, ‘Mean_daily_sold_count’], 
    ascending=False
).reset_index()sns.barplot(
    data=top_sold_brands.iloc[1:21], 
    x=’brand’, 
    y=’Sold_count’)
```

![](img/04f553d2276c650b7e42b25e5c50f6e4.png)![](img/3806adac2740f263d718f26b5a426638.png)

一眼就足以看出，所有未命名的品牌加起来已经累积了最高的销售数量。然而，Spigen 似乎是这一类别的亚军，并在命名品牌产品中占据市场主导地位。

# Q4。哪些品牌最赚钱？

哪些屏幕保护器品牌在最短的时间内给卖家带来了最高的收益？让我们回到无品牌产品的话题上来，因为当谈到收益时，情况可能会略有不同:

```
most_profitable_brands = sold_brands.sort_values(
    by=[‘Daily_earnings’, ‘Mean_daily_sold_count’, ‘Sold_count’],
    ascending=False
).reset_index()most_profitable_brands = most_profitable_brands[[
    ‘brand’, ‘Daily_earnings’, 
    ‘Daily_earnings_St.Dev’,’Sold_count’,
    ‘Mean_daily_sold_count’, ‘Mean_Sold_count_St.Dev’]]
```

我得到了这张表:

![](img/2b962784315b8d922e72150c76e303db.png)

让我们把它形象地画在柱状图上，就像这样:

```
plt.bar(x, y, width=0.85, yerr=y_err, 
        alpha=0.7, color=’darkgrey’,  
        ecolor=’black’
)
```

这应该会创建以下图形:

![](img/b06988123ff0764a4211bc49caa276f6.png)

很明显，没有品牌的产品利润并不高。请注意，当我们比较收入而不是销售商品总数时，品牌的顺序发生了很大变化。中档价位的品牌现在排在榜首(比如售价约 9.5 美元的 PureGear)。尽管他们的日销售率相对较低(每天 1-2 英镑)。

回答这四个问题向我们展示了“质量重于数量”可能是制定在线市场销售策略的最聪明的方法。

在本文中，我将重点带您完成数据挖掘和准备过程，最终回答我在本次研究中选择的关于在线市场销售趋势的四个关键问题。

**以下是我可以从数据中回答的一些其他问题:**

*   运输成本会影响客户决策吗？
*   有多少卖家对自己的产品打折？
*   打折会带来更高的销售额吗？
*   顾客从哪里购买产品会有什么不同吗？
*   卖家反馈分数如何影响销量？
*   拥有最高评级的徽章会促进产品销售吗？

幸运的是，我已经做到了。你可以在我的研究*中找到所有答案，如何用数据科学*增加在线市场的销售额。在这里免费下载[](https://sunscrapers.com/ebook/how-to-increase-sales-on-online-marketplaces-with-data-science/?utm_source=blog&utm_medium=article)****。****

***原载于 2020 年 3 月 16 日 https://sunscrapers.com*[](https://sunscrapers.com/blog/sales-data-science-a-step-by-step-guide-to-competitor-analysis-using-python/)**。****