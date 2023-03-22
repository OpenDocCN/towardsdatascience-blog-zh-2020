# 如何发现财务数据中的欺诈和异常

> 原文：<https://towardsdatascience.com/sap-data-analytics-examples-for-internal-audit-financial-tax-accounting-and-controlling-analysis-1fbc81d01eac?source=collection_archive---------47----------------------->

## 内部审计、财务和税务会计及控制等领域的一些数据分析示例

![](img/b359cd89bc200c9db861d928f39f2e43.png)

莎伦·麦卡琴在 [Unsplash](https://unsplash.com/s/photos/finance?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

***来自《走向数据科学》编辑的提示:*** *虽然我们允许独立作者根据我们的* [*规则和指导方针*](/questions-96667b06af5) *发表文章，但我们并不认可每个作者的贡献。你不应该在没有寻求专业建议的情况下依赖一个作者的作品。详见我们的* [*读者术语*](/readers-terms-b5d780a700a4) *。*

像 SAP 这样的软件，用于处理公司的所有业务流程，如记账、控制、销售等。托管大量数据，尤其是财务数据，这些数据可能会带来重要的见解，需要由商业智能、会计或内部审计等部门进行控制。下面对常见检查的概述应该提供了一个实用的分析用例列表。

## 可疑的变化

在 CDHDR 和 CDPOS 表的帮助下，您可以分析表中的变化，还可以识别可疑的过程，如订单中的不同值，该值从 20.000 €变为 19.999 €，刚好低于现有的限制。您需要的是对提到的表的访问(至少是读取)，对这些表如何工作的理解和一些 SQL。

*示例:SQL 结果—值的变化:*

```
UDATE       |CHANGENR |VALUE_NEW |VALUE_OLD01/01/2020  |1234     |20.000    | 
02/03/2020  |1234     |19.999    |20.000
```

另一个例子是查看客户的信用限额更改的频率——在某个订单之前的许多更改或更新也值得查看。

*示例:SQL 结果—计数变化[1]:*

```
OBJECTCLAS  |OBJECTID  |FNAME    |Count_ChangesKLIM        |543       |KLIMK    |6
```

## 检查重复项

为了监控主数据的质量，同时防止不正确的预订甚至欺诈，检查重复数据总是一个好主意—一个著名的例子是 SAP 数据中的客户数据。对于像这样的更深入的分析，您可能需要 SQL 之外的其他方法，并且您可能更喜欢 python 笔记本。

*示例:使用 Python 进行字符串相似性检查[2]:*

```
import distance
distance.levenshtein("customer_abc", "customer_abcd")
```

## 双重支付

重复支付意味着赔钱，因此你可以检查 BSEG 表中的财务记录是否有重复。以下 SQL 连接 BSEG 的 BSEG，以标识具有相同公司代码、金额等的记录。但是具有不同文档编号。

*示例:SQL 检查重复项[3]:*

```
SELECT B1.MANDT,B1.BUKRS,B1.GJAHR,B1.BELNR BELNR1,B1.BUZEI BUZEI1,B2.BELNR BELNR2,B2.BUZEI BUZEI2,B1.DMBTR FROM BSEG BSEG_1 JOIN BSEG BSEG_2 ON (BSEG_1.MANDT=BSEG_2.MANDT AND BSEG_1.BUKRS=BSEG_2.BUKRS AND BSEG_1.DMBTR=BSEG_2.DMBTR AND BSEG_1.SHKZG=BSEG_2.SHKZG) WHERE B1.BELNR!=B2.BELNR 
```

## 周末和节假日交易

由于权责发生制会计(年度账目、流转税的提前返还等),在不寻常的日期过账可能会有风险。)或者诈骗。要获取日期为创建日期或更改日期的记录，您可能需要来自 getfestivo 等开放 API 的假日日期，并调用类似以下的 API:

*示例 API 调用[4]:*

```
[https://getfestivo.com/v2/holidays?api_key=**[**](https://getfestivo.com/v2/holidays?api_key=[) **YOUR API KEY HERE]**&country=DE&year=2020
```

## 不寻常的预订文本

要确定不应该存在的费用，您可以在 BKPF 表中使用以下内容搜索不寻常的预订文本:

*SQL 文本搜索示例:*

```
... FROM BKPF
WHERE UPPER(BKTXT) LIKE = “Cancellation”
OR UPPER(BKTXT)= “Credit note”
UPPER(BKTXT)= “fee”
UPPER(BKTXT)= “Switzerland”
.....
```

## 结论

这些只是 SAP 系统中少数可能的分析问题，但由于我们讨论的是财务数据，这是一个重要的话题，您和您的公司应该意识到这些问题，并渴望培养分析能力。在本文中，示例与 SAP 相关，但是其他具有财务模块的 ERP 系统，如 Oracle 或 DATEV，也将以相同的方式工作，并且案例也是相似的。

## 进一步的资料和阅读

[1]石，董，(2015)使用 CAATs 对 SAP 进行审计。

[2]pypi.org，[https://pypi.org/project/Distance/](https://pypi.org/project/Distance/)

[3]DAB-GmbH，[https://www . da b-Europe . com/file admin/public data/Downloads/2016 _ 04 _ DAB _ Gesamtkatalog _ es . pdf](https://www.dab-europe.com/fileadmin/PublicData/Downloads/2016_04_dab_Gesamtkatalog_ES.pdf)

[4]费斯蒂沃，[https://getfestivo.com/documentation](https://getfestivo.com/documentation)