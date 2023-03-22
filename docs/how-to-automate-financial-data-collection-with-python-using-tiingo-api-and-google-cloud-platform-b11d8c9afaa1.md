# 如何使用 API 和 Google Cloud 通过 Python 自动收集金融数据

> 原文：<https://towardsdatascience.com/how-to-automate-financial-data-collection-with-python-using-tiingo-api-and-google-cloud-platform-b11d8c9afaa1?source=collection_archive---------4----------------------->

## Python 程序员分析股票价格的教程

![](img/30fe90a694598f08f66b7666c0df2e8b.png)

资料来源:联合国人类住区规划署

# 介绍

本文将向您展示在 Google 云平台上托管 Python 脚本的一种方法。对于希望自动化从 API 收集财务数据的过程，并且正在寻找一种高效、安全的方式将整个过程存储在云平台上的人来说，学习这样做尤其有用。

我将通过收集给定公司样本的每日股票价格的 API 数据来说明这一过程。这些数据可以用来进行金融建模。

# **步骤 1:获取感兴趣证券的股票代码列表**

第一步是收集相关公司列表的股票代码列表(每种证券的股票标识符)；在这种情况下，我使用的是 SP500 索引组件，它的列表很容易在维基百科的相关页面上[在线](https://en.wikipedia.org/wiki/List_of_S%26P_500_companies)获得。

*在许多情况下，股票代码列表可能已经以 csv/excel 格式存在，并从其他来源自动下载；在这种情况下，请跳到第 2 步。*

从该表中，您可以识别对应于每种证券的符号，稍后您将使用该符号从 API 中获取股票价格数据。这是一个有用的表，可以作为目标数据库中的参考，因为您可以定期运行脚本来从该表中收集数据，并获得 SP500 成员的最新视图。考虑到这些公司的市场资本总额，变化并不频繁，但最好定期更新，以考虑进入/退出指数的公司。对于这种用例，一个季度一次被认为是一个很好的更新频率。

在下面的步骤中，脚本将每月执行一次，但目标频率应适应您希望完成的任务。

为了收集成员公司的表格，您需要编写一个 Python 脚本。r *equests* 和 *Beautiful Soup* Python 包是抓取维基百科页面并从其 HTML 代码中提取表格元素的好选择。

现在让我们来看一下相关的 Python 代码部分，以便在脚本中实现这一点。

1.  **导入相关的包**:作为第一步，您需要导入相关的 Python 包，并定义指向在线数据源的 *url* 变量。您还应该导入熊猫包(" *import pandas as pd* ")，因为您稍后会需要它。

```
import requests
from bs4 import BeautifulSoup
import pandas as pdurl="[https://en.wikipedia.org/wiki/List_of_S%26P_500_companies](https://en.wikipedia.org/wiki/List_of_S%26P_500_companies)" 
```

2.**抓取数据:**然后你需要用 *request.get* 方法获取相关的 url 内容。然后，你可以用*提取它的文本。文本方法，*，最后把它解析成一个 Beatiful Soup 对象。

```
r = requests.get(url,timeout=2.5)
r_html = r.textsoup = BeautifulSoup(r_html, 'html.parser')
components_table = soup.find_all(id="constituents")headers_html = soup.find_all("th")
df_columns=[item.text.rstrip("\n") for item in headers_html]
```

为了在 HTML 中定位表格，您可以检查页面的代码(这可以通过在 Google Chrome 上单击 Ctrl + Shift + I 来完成),并看到您感兴趣的表格元素有一个由成分组成的 *ID，*,您可以使用它将表格内容存储到 *components_table* 变量中。然后就可以用*了。find_all* 用给定的 ID 定位页面中的所有元素。在这个特定的页面中，有两个具有该 ID 的表，由 *components_table* 对象引用。鉴于此，我们将只使用第一个，这是我们在步骤 3 中特别选择的。

您还可以使用相同的方法( *soup.find_all)* 来标识表格标题，您将在我们的数据集中使用这些标题作为列名，并将它们存储到一个名为 *df_columns 的列表变量中。*

3.**将数据存储到 Pandas Dataframe:** 接下来，您需要通过索引 *components_table* 对象(驻留在索引 0 处)来隔离第一个表，然后找到表体中的所有行，这些行不是标题；继续将结果存储到*数据行*中。

然后，您可以遍历这些行，将每一行的文本内容拆分到一个列表元素中，过滤掉空值并将其存储到一个名为 *stock、*的列表中，然后您必须将它追加到一个名为 *rows 的列表中。*

然后，您可以将*行*转换为 pandas DataFrame 对象，将 *component_headers* 解析为列名。 *Component_headers* 只不过是 *df_columns* 的一个子列表，它标识了属于两个表中第一个表的列，这两个表的 ID 为 *constituents。*

```
components_headers=df_columns[:9]data_rows=components_table[0].find("tbody").find_all("tr")[1:]rows=[]for row in range(len(data_rows)):

    stock=list(filter(None,data_rows[row].text.split("\n")))
    rows.append(stock)S_P_500_stocks=pd.DataFrame(rows,columns=components_headers)
```

**4。将数据集保存到 csv 文件:**如下面的代码所示，然后您将删除名为 SEC Filings 的冗余列，并使用*。to_csv* 方法在本地保存数据集。现在可以使用这个表从 API 中获取股票价格数据。

```
S_P_500_stocks.drop("SEC filings",inplace=True,axis=1)S_P_500_stocks.to_csv(r"/home/edo_romani1/my-python-directory/SP500stocks.csv",index=False)
```

# 步骤 2:使用 Tiingo API 收集历史股票价格数据

Tiingo 是一个金融市场 API，它提供了与多个市场的上市公司相关的各种信息；在本例中，我们对收集每日历史股票价格信息(日期、开盘、收盘、盘高、盘低、成交量)感兴趣，因此将参考相关的 Tiingo API [文档](https://api.tiingo.com/documentation/end-of-day)。

让我们一步一步地分解代码，看看如何收集每个 SP500 成员公司 20 多年的每日股票价格信息。

**1。导入相关包**:和之前一样，导入需要的包。这一次，您还需要导入 *datetime* 模块，因为稍后您将使用它来定义我们希望从 API 中提取数据的时间窗口。

```
import requests
import pandas as pd
import datetime
from datetime import datetime,timedelta
```

2.**定义 API 令牌:**要与 API 交互，您需要获得一个 API 密钥或令牌，以便将我们自己标识为注册的 Tiingo API 用户。使用 Tiingo，您可以轻松地[建立一个帐户](https://api.tiingo.com/documentation/general/overview)并获得您的令牌。一旦完成，您需要做的就是在代码中声明它，并将其设置为一个变量，如下图所示。

```
token1="123RUXBJKSJKFJD23Y348"
```

3.**导入您在步骤 1 中**保存为 csv 文件的 SP500 成员表；这可以通过*轻松完成。熊猫里的 read_csv* 方法。

```
SP=pd.read_csv(r"/home/edo_romani1/my-python-repository/SP500stocks.csv")
```

4.**调用 Tiingo API 并获取历史股票价格数据**

您首先必须建立一个要迭代的股票代码列表，以便对每个股票代码进行 API 调用，将它们存储在变量 *ticker_symbols* 中，您可以在下面一行中添加 SP500 索引值(“SPY”)来扩展这个变量。

您还可以设置想要从 API 调用数据的最晚日期( *end_date 变量*)，因为对于每只股票，您将调用股票价格历史，因此必须设置一个时间窗口来划分。将 *end_date* 设置为昨天(下面使用 *datetime* 和 *timedelta* 模块计算得出— *datetime.now.date()* 得到今天的日期，使用 *timedelta* 函数减去 1 天，得到今天-1 天=昨天)。

*注意:Tiingo 有每小时和每天的* [*API 调用限制*](https://api.tiingo.com/about/pricing) *，它们是通过使用作为上下文管理器的*[*rate _ limiter*](https://pypi.org/project/ratelimiter/)*包来处理的。知道你是否每小时/每天打很多电话是很重要的。*

最后，您需要初始化一个空列表 *data1* ，它将用于存储每个股票价格/ API 调用的历史数据集。

```
ticker_symbols=list(SP["Symbol"])
ticker_symbols.append("SPY")
end_date=str(datetime.now().date()-timedelta(days=1))
```

接下来的几行代码是您实际调用 Tiingo API 来获取每个感兴趣的股票代码的数据。首先需要在 Tiingo url 中设置*符号、end_date 和 token 参数*提交到 *request.get* 方法中；然后，您需要将响应存储到变量 *r，*中，该变量对应于包含给定股票行情自动收录器的股票价格历史的 csv 文件。下面代码中的几行代码将这些值转换成一个列表，然后是一个 dataframe *df* ，在以迭代的方式移动到下一个 ticker_symbol 之前，您将把它追加到 *data1* 集合中。

*注意*:*try*子句用于处理由请求包引起的超时错误，下面的 except 子句用延长的超时时间重复相同的代码来处理更长的响应；代码与下面的代码相同，唯一的区别是超时参数的值(是双精度的)，因此为了简单起见，省略了它。

```
data1=[] for symbol in ticker_symbols:
        #with rate_limiter:
                try:
                        url = "[https://api.tiingo.com/tiingo/daily/{}/prices?startDate=2000-01-03&endDate={}&format=csv&token={](https://api.tiingo.com/tiingo/daily/{}/prices?startDate=2000-01-03&endDate={}&format=csv&token={)}".format(symbol,end_date,token1) r = requests.get(url,timeout=10) rows=r.text.split("\n")[1:-1] df=pd.DataFrame(rows)
                                                                        df=df.iloc[:,0].str.split(",",13,expand=True) df["Symbol"]=symbol
                        data1.append(df)
```

**5。将数据集保存到 csv 文件:**作为最后一步，您可以使用*。concat* 方法将数据帧列表连接成一个最终数据集，然后像前面一样将其保存到一个 csv 文件中。

```
df_final=pd.concat(data1)
df_final.drop(df_final.iloc[:,6:13],axis=1,inplace=True)df_final.to_csv(r"/home/edo_romani1/my-python-repository/historicalSP_quotes.csv",index=False)
```

下面是最终输出的 csv 视图，从中我们可以观察到样本中第一家公司的初始日期和相对价格数据。

![](img/1afcb237135abf1471906f5f0aa987a3.png)

输出包含 SP500 会员股票价格历史记录的 csv 文件；来源:作者

现在，您已经有了两个从 Tiingo API 获取股票数据的全功能 Python 脚本，让我们看看如何使用谷歌云平台(GCP)自动运行它们，以便每天开市时您都可以收集前一天的最新报价。我将展示我们如何利用 Google 的计算引擎、云功能、云存储和 GoogleBigQuery 来建立一个简单的数据摄取工作流，它将基于这两个运行在云上的脚本，从而远离我们的本地机器。

# 步骤 3:在 Google 云计算引擎上自动化和调度 Python 脚本

您可以使用 GoogleCloud ComputeEngine 启动一个 VM 实例，而不是在本地运行我们的两个 Python 脚本。您可以轻松地[启动一个 Linux 虚拟机，并通过 SSH](https://cloud.google.com/compute/docs/quickstart-linux) 连接到它。

连接后，您将看到以下终端视图:

![](img/7f09c8e57d54483d86019ea04e31821f.png)

GoogleCloud 计算引擎上的 Linux 虚拟机终端。来源:作者

此时，使用右上角的 settings 按钮，您可以选择“Upload file ”,在 VM 的文件系统中快速移动我们的 Python 脚本。

一旦完成，您的脚本将被存储在虚拟机上。当数据集保存为 csv 文件时，不要忘记更改 python 脚本中的本地路径引用；在终端上，您可以用 *mkdir* 命令建立一个新目录，并用 *pwd* 命令获取给定文件的文件路径。更多详细信息可在[这里](https://www.w3resource.com/linux-system-administration/working-with-directories.php)找到。您可以使用 *python* (或 *python3* ，取决于您的 python 版本)命令，后跟要执行的 py 文件的位置，从终端启动并运行这两个脚本中的任何一个。

首先，您可能希望首先运行 SP500 成员集合脚本，以便第二个脚本能够正确导入从中提取股票代码列表的 csv 表。

最后，您可以使用 cron 作业直接从 linux 终端自动执行这两个脚本；您可以使用 *crontab -e* 命令访问 crontab，然后设置 [cron 作业计划](https://cloud.google.com/scheduler/docs/configuring/cron-job-schedules)，随后执行 python 文件。

在这个设置中，我每月运行一次第一个脚本，以便在股票市场开放的每一天更新 SP500 组件和股票价格 API 脚本，因此是每周二到周六(以便获得每周星期一到星期五最后一天的数据)。下面是所描述的 crontab 的一个示例视图。

![](img/879debbf49abb4a40c91c50e9309d6a1.png)

Crontab 计划视图。来源:作者

# 步骤 4:将输出表存储到 GoogleCloudStorage 中

现在您已经自动化了脚本的执行，下一步是将两个输出表(SP500 成员表和所有成员的股票价格数据表)移动到 GoogleCloud 中的一个存储位置以供参考。我已经使用 GoogleCloud Storage 实现了这一点，并将展示如何在将数据转移到数据仓库解决方案(在本例中是 GoogleBigQuery)之前，在 GoogleCloudStorage 中暂存数据。

在你这么做之前，你可以使用谷歌云的控制台为你各自的文件快速创建两个不同的存储桶。创建之后，您可以简单地将两个 csv 输出表上传到每个 bucket。

要与 GoogleCloud Storage 通信，您可以使用 GoogleCloud 提供的 p [ython CloudStorage 客户端库](https://cloud.google.com/storage/docs/reference/libraries);不要忘记设置认证凭证，以便与您的 GoogleCloud 帐户资源进行通信。

设置完成后，您只需将以下代码块添加到我们两个 Python 脚本的末尾，即可完成该过程:

```
import os
from google.cloud.storage.blob import Blobos.environ['GOOGLE_APPLICATION_CREDENTIALS']=r"/home/edo_romani1/My First Project-69347b3b5af2.json"storage_client = storage.Client()bucket = storage_client.get_bucket("spcomponents")
data=bucket.blob("SP500stocks.csv")data.upload_from_filename(r"/home/edo_romani1/my-python-directory/SP500stocks.csv")
```

前两行导入必要的包；然后，代码通过 *os.environ、*设置应用程序的 credentials，它指向包含我们的 GoogleCloud 个人凭证的 json 文件的位置(在 Linux VM 上)。

然后可以初始化存储客户机，用*获取存储桶信息。get_bucket* 方法，选择要用 *bucket.blob* 更新的对象，然后在每次 python 脚本运行时用*更新对象。upload_from_filename* 命令，指向相关文件的 Linux VM 位置。上面的示例只显示了第一个输出表的流程，您只需要更改必要的文件桶/路径/名称/位置细节，就可以复制股票价格数据集的流程。

现在，每次运行 python 脚本时，GoogleCloudStorage 上相应的存储桶都会更新最新的数据。

# 步骤 5:使用 CloudFunctions 自动将文件从 CloudStorage 传输到 GoogleBigQuery

[云函数](https://cloud.google.com/functions)允许我们运行针对特定 GoogleCloud 资源的事件驱动代码，例如，在我们的例子中，GoogleCloud 存储桶。

让我们看看如何设置一个来完成以下任务:每次将 csv 输出表上传到 GoogleCloudStorage 时，您可以将这些表移动并上传到 [GoogleBigQuery](https://cloud.google.com/bigquery?hl=it) ，这是 GoogleCloud 的数据仓库解决方案(众多解决方案之一)，非常适合处理关系数据，如本教程中的数据。

1.  [**创建一个 GoogleBigQuery 数据集**](https://cloud.google.com/bigquery/docs/quickstarts/quickstart-web-ui?hl=it#create_a_dataset) **和** [**表**](https://cloud.google.com/bigquery/docs/quickstarts/quickstart-web-ui?hl=it#load_the_data_into_a_new_table) **作为必要的设置步骤。**您可以将 GoogleCloudStorage 设置为我们两个表的表源，并临时加载我们最新的可用表。
2.  **编写将表格数据从 GoogleCloudStorage 上传到 GoogleCloudBigQuery 的代码；**这将是每次在 GoogleCloudStorage 上生成更新时 CloudFunction 将运行的实际代码。该表的代码如下:

```
def csvloader(data,context):
    from google.cloud import bigquery client = bigquery.Client()
    table_ref = client.dataset("csm").table("SPhistorical") job_config = bigquery.LoadJobConfig()
    job_config.write_disposition =     bigquery.WriteDisposition.WRITE_TRUNCATE
    job_config.skip_leading_rows = 1

    job_config.source_format = bigquery.SourceFormat.CSV
    uri = "gs://csm_erat/Data/historicalSP_quotes.csv" load_job = client.load_table_from_uri(uri, table_ref, job_config=job_config)load_job.result()
    destination_table = client.get_table(table_ref)
```

代码定义了一个 *csv_loader* 函数，该函数导入必要的 BigQuery 客户端包，将其初始化为 c *lient，*连接到之前创建的 BigQuery 表(该示例仅针对历史股票价格数据，但对于我们要移动的每个文件，其结构都是相同的)作为 *table_ref。*然后，代码在下面几行代码中用某些配置参数建立一个加载作业(比如在加载新表时覆盖当前 BigQuery 表的指示，读取时跳过的 *n* 行数和源文件格式的指示- *csv* -)。然后，它定义源路径( *uri* )，在其中找到要上传的新数据，并用*加载它。load_table_from_uri* 方法及相关参数。uri 指向数据的 GoogleCloudStorage 位置。*数据*和*上下文*参数与事件有效载荷和触发事件的元数据有关，但在这方面不是核心加载作业的一部分。更多信息可在[这里](https://cloud.google.com/functions/docs/calling/storage)找到。

关于如何使用 BigQuery 客户端库的完整细节可以在[这里](https://googleapis.dev/python/bigquery/latest/index.html)找到。它还包含有关如何设置身份验证的有用信息。

3.**使用步骤 2** 中定义的代码设置 Google CloudFunction

您最终可以从 [GCP 控制台](https://cloud.google.com/functions/docs/deploying/console)部署我们的云功能，方法是点击云功能资源，然后点击“创建功能”；然后可以输入函数名、触发器(在我们的例子中是云存储)和运行时(Python3)。完成后，您可以粘贴您的功能代码并部署它；该功能将启动并运行，您将能够通过 GCP 控制台中的事件日志对其进行监控。

**第 6 步:在 BigQuery 中查看数据集和表**

现在，您可以通过访问其 UI，选择我们的 GCP 项目>数据集>表格>预览，在 GoogleBigQuery 中直接从 API 访问最新的数据拉取。您也可以在详细信息部分查看最新更新时间。

# 后续步骤

您的工作流程已正式建立，从开始到结束只需 5 分钟。

这是一个非常简单的解决方案，你只需要几个 Python 脚本和 [GCP 的免费等级账户](https://cloud.google.com/free/?utm_source=google&utm_medium=cpc&utm_campaign=emea-gb-all-en-dr-bkws-all-all-trial-e-gcp-1008073&utm_content=text-ad-none-any-DEV_c-CRE_431053247446-ADGP_Hybrid+%7C+AW+SEM+%7C+BKWS+~+EXA_M:1_GB_EN_General_Cloud_gcp+free+tier-KWID_43700053279219280-aud-606988878374:kwd-310728589823-userloc_9045999&utm_term=KW_gcp%20free%20tier-NET_g-PLAC_&gclid=Cj0KCQjwwr32BRD4ARIsAAJNf_3F3Fl-IXzaeYmwDyaiogohf87-x3pMWRVqc_MEEvGdgeNLjBIEHawaAl7kEALw_wcB)就可以完成。

一个不错的下一步是只调用一次股票价格数据的整个历史，并修改 API 调用(url 变量中的*)以只获取昨天的值。*

希望这份循序渐进的指南将有助于您使用 GCP 的金融数据建立下一个 Python 项目！

[](/how-to-process-and-visualize-financial-data-on-google-cloud-with-big-query-data-studio-f37c2417d4ef) [## 如何使用 Big Query & Data Studio 在 Google Cloud 上处理和可视化财务数据

### GCP 从业者从金融数据开始的教程

towardsdatascience.com](/how-to-process-and-visualize-financial-data-on-google-cloud-with-big-query-data-studio-f37c2417d4ef) 

**访问我的免费数据科学资源清单** [**这里**](https://landing.mailerlite.com/webforms/landing/k1n3r2)

[](https://edo-romani1.medium.com/membership) [## 通过我的推荐链接加入 Medium-Edoardo Romani

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

edo-romani1.medium.com](https://edo-romani1.medium.com/membership)