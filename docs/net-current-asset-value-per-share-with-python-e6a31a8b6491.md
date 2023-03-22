# Python 的每股流动资产净值

> 原文：<https://towardsdatascience.com/net-current-asset-value-per-share-with-python-e6a31a8b6491?source=collection_archive---------38----------------------->

## 筛选纳斯达克找出廉价公司

**每股净资产值(NCAVPS)** 是[本杰明·格拉哈姆](https://en.wikipedia.org/wiki/Benjamin_Graham)用来识别有吸引力股票的指标之一。他积极寻找低于每股流动资产净值的股票。

在这篇文章中，我们将使用 Python 构建一个脚本来计算**的每股净流动资产价值，并自动识别低于其 NCAVPS 的股票交易。**

![](img/90272ffe1cbe650b917274d1b7c6f4c1.png)

照片由 [**佳伟崔**](https://www.pexels.com/@jiawei-cui-1213899?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 发自 [**Pexels**](https://www.pexels.com/photo/aerial-photography-of-city-buildings-under-cloudy-sky-2310885/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

# 什么是每股净资产值(NCAVPS)？

NCAVPS 是一种衡量公司价值的方法，不同于其他传统的估值方法，后者更多地关注不同的指标，如收益。 [NACVPS 取流动资产减去总负债，所得结果除以发行在外的股票总数。](https://investinganswers.com/dictionary/n/net-current-asset-value-share-ncavps)

NCAVPS =(流动资产-总负债)/已发行股票

当股票交易低于 NCAVPS 价值时，这意味着公司交易低于其清算价值，因此这可能被视为一个良好的买入机会。格雷厄姆感兴趣的是交易价值低于其 NCA VPS 66%的公司。让我们建立一个算法，让 Python 为我们找到这样的公司。

# Python 的每股流动资产净值

现在让我们构建一个脚本，它将为我们自动执行以下财务分析:

1.  在纳斯达克综合指数中搜索 100 只股票，并将每只股票添加到 Python 列表中。
2.  使用自动收报机从免费 API 中提取第一点中找到的每只股票的 ***流动资产、总负债和已发行股票***
3.  计算 ***每股流动资产净值【NCAVPS】***
4.  检查公司交易的 NCAVPS 是否少于 67%
5.  如果交易量低于 NCAVPS 的 67%,将股票添加到 Python 字典中

为了搜索在纳斯达克交易的公司并获得财务报表信息，我们将使用一个包含大量免费财务数据的 API。

导入所有包后，我们要做的第一件事是发出一个 http 请求，获取纳斯达克综合指数中的 100 只股票。您可以在下面的[链接](https://financialmodelingprep.com/api/v3/search?query=&limit=100&exchange=NASDAQ)中看到示例响应。

响应是一个字典列表。每本词典都包含一种股票的信息。为了提取每家公司的股票代码，我们只需要提取关键的*符号*的值。

最后，我们将每个符号附加到一个 Python 列表中。这正是我们用下面的代码实现的。

```
import requests
import pandas as pd

NetCurrentAssetValueperShare = {}

querytickers = requests.get(f'https://financialmodelingprep.com/api/v3/search?query=&limit=100&exchange=NASDAQ')
querytickers = querytickers.json()

list_500 = querytickers
stocks = []
count = 0
for item in list_500:
    count = count +1
    #Stop after storing 100 stocks
    if count < 100:
        stocks.append(item['symbol'])
print(stocks)
['HRTX', 'GRIF', 'WAFU', 'VOD', 'TRMD', 'KTOV', 'HYXE', 'CLXT', 'TRMB', 'NBL', 'IGIB', 'CDMOP', 'VALX', 'IBTA', 'IBTE', 'FHK', 'IDXG', 'PIE', 'EMMA', 'IBTH', 'SGBX', 'SIFY', 'SUMR', 'TRVI', 'RNSC', 'PLUG', 'AGNC', 'BLUE', 'CENTA', 'BL', 'BSMO', 'ARCE', 'ARCB', 'ARCC', 'AEIS', 'AMTBB', 'CDLX', 'PFIE', 'BYND', 'RFEU', 'DFBH', 'TTD', 'TURN', 'CECE', 'COWN', 'PRPO', 'AXNX', 'BJRI', 'PLC']
```

# 用 Python 提取财务报表数据

现在我们有了多家公司的报价器，我们可以使用它们来提取每家公司的 ***资产负债表*** 数据。我们将遍历每个公司，并发出一个 http 请求来获得:

*   流动资产
*   负债总额
*   已发行股票

API 端点将公司的股票代码作为参数。我们将把它作为 *{company}* 传递给每个循环迭代中的 url。检查在[以下链接](https://financialmodelingprep.com/api/v3/financials/balance-sheet-statement/AAPL?period=quarter)中获得的响应。

**在*键* *金融*** 中，我们有一个字典列表，我们只想保留列表[0]中的第一个元素，因为它代表最近的季度。我们可以通过使用如下代码所示的相应键来提取流动资产和总负债:

```
for company in stocks:
    Balance_Sheet = requests.get(f'https://financialmodelingprep.com/api/v3/financials/balance-sheet-statement/{company}?period=quarter')

    Balance_Sheet = Balance_Sheet.json()
    try:
        total_current_assets = float(Balance_Sheet['financials'][0]['Total current assets'])
        total_liabilities = float(Balance_Sheet['financials'][0]['Total liabilities'])
        sharesnumber = requests.get(f'https://financialmodelingprep.com/api/v3/enterprise-value/{company}')
        sharesnumber = sharesnumber.json()
        sharesnumber = float(sharesnumber['enterpriseValues'][0]['Number of Shares'])

        NCAVPS = (total_current_assets-total_liabilities)/sharesnumber
        NetCurrentAssetValueperShare[company] = NCAVPS
    except:
        pass
```

可以从不同的 API 端点提取已发行股票的数量，从而为我们提供关于公司的[企业价值的信息。](https://financialmodelingprep.com/api/v3/enterprise-value/AAPL?period=quarter)

有了所有需要的信息，我们可以很容易地计算每只股票的 NCAVPS，并将其添加到字典中。

请注意，除了之外，我还使用了 *try* 和*，以便在 API 请求过程中遇到错误时，脚本不会被中断。*

# 解释每股净资产值(NCAVPS)

根据本杰明·格拉哈姆的说法，投资者应该寻找股价不超过其每股 NCAV 67%的公司。67%代表一个安全边际，因为一些流动资产(即存货)可能不那么容易转换成现金。

因此，我们可以在脚本中通过获取股票价格并将其与 NCAVPS 进行比较来实现这一点。我们只会将 NCAVPS 比股价低 67%或更低的股票添加到我们的字典中。

```
price = float(sharesnumber['enterpriseValues'][0]['Stock Price']) 

if NCAVPS < 0.67 * price:
     NetCurrentAssetValueperShare[company] = NCAVPS
```

# 包扎

现在，如果我们打印我们的 NetCurrentAssetValueperShare 字典，我们会得到下面的结果:

```
print(NetCurrentAssetValueperShare) {}
```

不幸的是，我们的脚本返回了一个空字典，这意味着目前在纳斯达克没有股票符合我们的标准。这是有道理的，因为发现公司交易低于其清算价值实际上并不常见。

请随意搜索其他交易所的股票。你可能比我幸运，拿回一些公司。

参见下面用 Python 计算**每股净资产值的脚本。很高兴在[我的推特账户](https://twitter.com/CodingandF)继续讨论。**

```
import requests
import pandas as pd

NetCurrentAssetValueperShare = {}

querytickers = requests.get(f'https://financialmodelingprep.com/api/v3/search?query=&limit=1000&exchange=NASDAQ')
querytickers = querytickers.json()

list_500 = querytickers
stocks = []
count = 0
for item in list_500:
    count = count +1
    #Stop after storing 50 stocks
    if count < 500:
        stocks.append(item['symbol'])

for company in stocks:
    Balance_Sheet = requests.get(f'https://financialmodelingprep.com/api/v3/financials/balance-sheet-statement/{company}?period=quarter')

    Balance_Sheet = Balance_Sheet.json()
    try:
        total_current_assets = float(Balance_Sheet['financials'][0]['Total current assets'])
        total_liabilities = float(Balance_Sheet['financials'][0]['Total liabilities'])
        sharesnumber = requests.get(f'https://financialmodelingprep.com/api/v3/enterprise-value/{company}')
        sharesnumber = sharesnumber.json()
        sharesnumber = float(sharesnumber['enterpriseValues'][0]['Number of Shares'])

        NCAVPS = (total_current_assets-total_liabilities)/sharesnumber

        price = float(sharesnumber['enterpriseValues'][0]['Stock Price']) 
        #only companies where NCAVPS is below the stock price
        if NCAVPS < 0.67 * price:
            NetCurrentAssetValueperShare[company] = NCAVPS
    except:
        pass

NetCurrentAssetValueperShare
```

*原载于 2020 年 3 月 19 日 https://codingandfun.com**[*。*](https://codingandfun.com/net-current-asset-value-per-share-with-python/)*