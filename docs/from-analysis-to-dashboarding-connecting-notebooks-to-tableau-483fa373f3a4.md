# 从分析到仪表板——连接笔记本和表格

> 原文：<https://towardsdatascience.com/from-analysis-to-dashboarding-connecting-notebooks-to-tableau-483fa373f3a4?source=collection_archive---------19----------------------->

## 现实世界中的数据科学。

## 这个故事讲述了在尝试和失败了许多方法后，我是如何从笔记本上创建梦想仪表盘的。在这里，我分享我是如何将我的数据帧连接到 Tableau 的。希望这篇博客能帮助那些在笔记本和仪表盘之间挣扎的人。

![](img/1d2cb8c53f8b21debf72e928cdad1843.png)

卢克·切瑟在 [Unsplash](https://unsplash.com) 上的照片

数据分析的大背景不仅仅是数据和获得见解，还包括让每个人都能理解这些信息。当时，作为数据科学的初学者，我接触了 Jupyter 笔记本电脑。笔记本就像其他演示媒体一样。它由单元格组成，您可以在其中放置代码，然后在单元格下运行和输出，它还带有一个 markdown 功能，您可以用基本的 HTML 记录您的代码。这些笔记本用于探索性数据分析，便于与队友分享您的工作成果。尽管它有 Plotly、matplotlib 等库，但对于非技术用户来说，它仍然不是现成的。

例如，以新冠肺炎为例。世界各地的每个人都关心了解他们国家的日常状况。他们会选择一个记录了更多单元格、代码的笔记本，还是一个以交互形式分享你的分析可视化的仪表板？这就是为什么每个数据分析师都需要具备讲故事的技能。仪表板就像讲故事一样，你从你所获得的信息中构建一个故事，这个故事应该以一种不会让他们转移视线的方式呈现。

所以问题是，我如何将我的 jupyter 笔记本连接到 Tableau，而不需要在 Tableau 中安装 python？

答案很简单。Tableau 只需要数据。jupyterlab 的数据帧可以保存为 CSV、Xls、JSON 等格式，但在**实时分析**中，当数据每天都在更新时，每天从 jupyter 笔记本下载数据并上传到 Tableau 是低效的。

但是有一个解决方案，尽管笔记本电脑不支持数据库，因为它们的唯一目的是分析数据集。

如何这些分析的数据集，准备可视化是发送到谷歌使用谷歌 API 表。因此，即使数据在每次刷新我们的笔记本时不断更新，我们从在线门户网站读取的数据也会在分析后自动保存到 google sheets，tableau 可以连接到这些数据表。

因此，创建实时仪表板的任务非常简单。你要做的就是

*   创建笔记本
*   从在线门户数据 API 读取数据到熊猫数据框架
*   分析并创建一个干净而精确的数据报。
*   将数据发送到 google sheets。
*   让我们将以下库导入笔记本

```
import gspread
from oauth2client.service_account import ServiceAccountCredentials
from df2gspread import df2gspread as d2g
```

*   现在，在做任何事情之前，请按照以下说明获得对您的工作表的凭据访问权限:

1.  前往[谷歌开发者控制台](https://console.developers.google.com/project)创建一个新项目(或者选择一个你已经有的项目)。

2.在“APIs & Services > Library”下，搜索“Drive API”并启用它。

3.在“APIs & Services > Library”下，搜索“Sheets API”并启用它。

4.[启用项目的 API 访问](https://gspread.readthedocs.io/en/latest/oauth2.html#enable-api-access)如果你还没有这么做的话。

5.转到“APIs &服务>凭据”并选择“创建凭据>服务帐户密钥”。

6.填写表格

7.点击“创建密钥”

8.选择“JSON”并点击“创建”

您将自动下载一个带有凭证的 JSON 文件。它可能看起来像这样:

```
{
    "type": "service_account",
    "project_id": "api-project-XXX",
    "private_key_id": "5ty … bg4",
    "private_key": "-----BEGIN PRIVATE KEY-----**\n**NrDyLw … jINQh/9**\n**-----END PRIVATE KEY-----**\n**",
    "client_email": "443000000000-msmhj@developer.gserviceaccount.com",
    "client_id": "445 … hd.apps.googleusercontent.com",
    ...
}
```

记住下载的凭证文件的路径。同样，在下一步中，您将需要该文件中 client_email 的值。转到您的电子表格，并与上面步骤中的 client_email 共享。

*   创建一个新的 google 工作表，将键复制到一个名为 spreadsheet_key 的变量中。粗体字将是我的工作表的关键。可在打开工作表的链接中找到)

```
[https://docs.google.com/spreadsheets/d/**1vd6hDT6k8lcWNMDYz13kqW7zXnIX5TVED9–4Kq7EAQnvI**/edit#gid=0](https://docs.google.com/spreadsheets/d/1vdDT6k8lcWNMDYz13kqW7zXnIX5TVED9-4Kq7EAQnvI/edit#gid=0)
```

*   将以下代码添加到您的 python 脚本或笔记本中。另外，友情提示，在复制下面的代码之前，您刚刚下载的 JSON 文件和 python 脚本应该在同一个文件夹中。

```
scope = [‘[https://spreadsheets.google.com/feeds'](https://spreadsheets.google.com/feeds'),
 ‘[https://www.googleapis.com/auth/drive'](https://www.googleapis.com/auth/drive')]
credentials = ServiceAccountCredentials.from_json_keyfile_name(
 ‘myfile.json’, scopes=scope)
gc = gspread.authorize(credentials)spreadsheet_key=’[**1vd6hDT6k8lcWNMDYz13kqW7zXnIX5TVED9–4Kq7EAQnvI**](https://docs.google.com/spreadsheets/d/1vdDT6k8lcWNMDYz13kqW7zXnIX5TVED9-4Kq7EAQnvI/edit#gid=0)’
```

*   现在清理您的数据集，并将准备可视化的数据保存到数据框中。
*   复制并粘贴以下代码，将您的数据帧发送到 google sheets。

```
wks_name= “nameofyoursheet”
d2g.upload(df, spreadsheet_key, wks_name, credentials=credentials, row_names=True)
print(“Data upload success”)
```

wks_name 将是您的工作表的名称，df 将是您的 dataframe 变量。现在，每次刷新笔记本时，数据都会自动更新到工作表中，您可以通过数据 API 将数据读取到笔记本中，这样就可以轻松地使用 jupyter notebook 进行实时分析。

*   完成以上步骤后，打开 Tableau，连接你的 google sheets。要下载 Tableau，请点击[这里](https://public.tableau.com/en-us/s/download)
*   若要连接，请前往服务器> google sheets

![](img/86776c16a1b39369c3008c73033bf2a1.png)

将 google sheet 添加到 Tableau 数据连接后(数据来源: [OGD 平台印度](http://data.gov.in)

*   使用工作表中的数据框在 Tableau 中创建可视化效果。每个可视化都可以在表格中完成，这就是他们所说的 Tableau。这些表可以拖放到仪表板中。将您的仪表板发布到 Tableau Public，就大功告成了。

现在，由于数据每天都在变化，我们所要做的就是刷新笔记本和 Tableau 数据连接。你所有的可视化会自动更新。

Tableau 相对于其他可视化库(如 Plotly 和 matplotlib)的一个优势是，你可以零编码地获得可视化效果，这是非常专业的。

点击[此处](https://public.tableau.com/views/Project_AQIanalysis/Dashboard1?:display_count=y&:origin=viz_share_link)查看我使用 jupyter notebook(用于分析)和 Tableau(用于可视化)制作的一个仪表板。