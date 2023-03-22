# 使用 Docker、SQL 客户端和 Python 进行数据分析的 SQL-lise Elasticsearch 查询

> 原文：<https://towardsdatascience.com/sql-lise-elasticsearch-query-for-data-analysis-using-docker-sql-client-and-python-part-1-bd4db524c452?source=collection_archive---------28----------------------->

## 在本文中，我将解释使用简单的 SQL 语言分析 Elasticsearch 中的原始数据的两种不同方法

![](img/34b30fef633e55105624dd59e6cd5a9f.png)

> **关于弹性搜索:**

Elasticsearch 是一个非 SQL 数据库，由一家名为 Elastic 的公司作为开源和商业产品提供。它用于存储和执行对数据的大数据分析，如 web 服务器日志、来自任何应用程序的事件数据或物联网中的传感器数据。

elasticsearch 之所以受欢迎，是因为它安装简单，无需任何额外的软件包就可以从几个节点扩展到数百个节点。它使用内置的 REST API 对数据执行查询。

所以你听说了 Elasticsearch 可以做的所有很酷的事情，你决定在你的项目中使用它来存储和分析你的日志或事件或任何其他数据。但是您发现用 JSON 编写一个复杂的嵌套查询很麻烦，只是为了对存储在 Elasticsearch 中的数据进行简单的分析。

例如，Elasticsearch 中的一个简单过滤查询如下所示:

```
GET /_search
{
 "query":{
    "bool":{
      "filter":[
                {"term":{"status":"running"}},
                {"range":{"last_updated_date":{"gte":"2019–12–31"}}}
              ]
      } 
   }
}
```

相同的查询可以用 SQL 编写，如下所示:

```
SELECT * 
FROM   sample_table 
WHERE  status = "running" 
       AND last_updated_date > "2019–12–31"
```

现在，从 Elasticsearch 6.3 版本开始，其中一个插件 X-Pack 是开放的，并默认集成为 Elasticsearch 堆栈(ELK)中的一个功能。

我们可以使用 X-Pack 插件提供的特性，使用类似 SQL 的查询来分析 Elasticsearch 中的数据，而不是编写复杂的嵌套 JSON 查询。

我们将看看两种不同的方法:

1.  使用通用数据库客户机(Dbeaver):连接到我们的 Elasticsearch 服务器，用 SQL 方言运行我们的查询。
2.  使用 Python 模块(elasticsearch-dbapi 和 Sqlalchemy):连接到我们的 elasticsearch 服务器，并将数据查询到 pandas dataframe 中，以执行进一步的分析和可视化。

> **设置及要求:**

1.  *Ubuntu OS:* 我用 Ubuntu 做演示，但是所有需要的工具和模块也可以在其他 OS 上使用。
2.  这是一个通用的数据库工具，也不支持 SQL。你可以从[这里](https://dbeaver.io/download/)下载。
3.  *Docker:* 为快速旋转的麋鹿群在我的本地进行演示。你可以从[这里](https://docs.docker.com/install/)下载安装。如果您已经运行了一个 Elasticsearch 实例，那么您可以跳过这一步。
4.  *Jupyter notebook:* 对于第二种方法，我们在获取后用 python 分析数据。

> **第一种方法-分析来自数据库客户端的弹性搜索数据:**

首先，我将克隆一个免费的 git 存储库，其中包含了在本地运行 ELK 集群所需的所有 docker 文件。您也可以从[这里](https://github.com/deviantony/docker-elk)下载/克隆这个库。

现在进入克隆项目的目录，在 docker 命令下运行:

```
$ sudo docker-compose up -d
```

如果一切正常，您将得到下面的输出:

![](img/cf864caad01ea0ffe732b82069795106.png)

docker-compose up 命令的输出

让我们使用以下命令检查所有服务是否都已启动:

```
$ sudo docker-compose ps
```

![](img/f9c9305d86596ed8a05b735c1f67972a.png)

docker-compose ps 命令的输出

我们看到我们的 Elasticsearch 服务在端口 9200 运行，而 Kibana 在端口 5601 运行。

出于演示目的，我添加了一些示例数据。如果您不是在处理自己的数据，那么您也可以使用 Kibana 的主页进行同样的操作，如下所示:

![](img/a0e1020b2af6321b2dc625104b480dcf.png)

用于添加示例数据的 Kibana 主页

首先，让我们使用 Elasticsearch REST API 格式获取数据。我正在使用 *curl* 运行以下对飞行样本数据的查询:

```
curl -X GET "elastic:changeme@localhost:9200/_search?pretty" -H 'Content-Type: application/json' -d'
{
"query": { 
 "bool": {
   "filter": [ 
   { "term":  { "DestCountry": "IT" }},
   { "range": { "timestamp": { "gte": "2020-03-06T21:08:31.000Z" }}}
      ]
    }
  }
}
'
```

以下是输出:

![](img/35a8d220aa84273d95a1c062073236ef.png)

Elasticsearch REST API 格式的简单提取查询的输出

让我们用 SQL 语言运行同样的查询。为此，打开 Dbeaver 客户端并单击 add connection:

![](img/9f76b16595795cc29e0ee42a47902f7b.png)

在 Dbeaver 中添加连接

现在添加凭据和主机详细信息:

![](img/5045a350e86c306532d0f5027d467675.png)

添加主机和凭据

Dbeaver 将自动下载并安装连接到 Elasticsearch 服务器所需的驱动程序。

![](img/ad9a98655b7ecae573168247d2987e05.png)

通过 Dbeaver 自动下载驱动程序

如果您面临驱动程序版本的任何问题，请在此从[下载所需版本。并将其添加到*编辑驱动*设置中，点击*确定*。](https://www.elastic.co/downloads/past-releases#jdbc-client)

![](img/0b2010360db84118798e3dc233a54fdb.png)

在编辑驱动程序设置中添加下载的驱动程序(jar 文件)

现在点击*测试连接*如果一切正常，您将得到如下所示的成功消息:

![](img/84e99b65b7733f81095a9439c459b371.png)

测试连接的成功消息

现在点击*完成。*您将在左侧看到与 Elasticsearch 服务器的连接:

![](img/580a328e0fb45b88e10382fed9e99d4c.png)

现在点击*新建 SQL 编辑器:*

![](img/678bcddb74b9eec1a7b55a6f08ea2896.png)

现在，我们将使用 SQL 语言编写相同的查询:

```
SELECT
 *
FROM
 kibana_sample_data_flights
WHERE
 "DestCountry" = 'IT'
 AND "timestamp" > '2020-03-06T21:08:31.000Z'
```

瞧啊。！您将得到一个漂亮的表格格式的输出:

![](img/d5bcec4a46ff573e86bcf6bab26e5f41.png)

我们的 SQL 查询的最终输出

> **第二种方法-将数据查询到 pandas dataframe:**

从 Elastic search 获取数据到 python 的老方法是使用 *Python Elasticsearch 客户端*(此处阅读文档)，它使用我们在 *Curl* 中看到的相同的 REST API 查询格式。

我们将通过使用简单的 SQLAlchemy 和 elasticsearch-dbapi 模块来简化这一点。

下面是示例代码:

```
##Importing required library
from sqlalchemy import create_engine
import pandas as pd## Creating connection to our Elasticsearch server
## format of connection string is: 
## "dialect://username:password@host:port/"
connection_es=create_engine("""elasticsearch+[http://elastic:changeme@localhost:9200/](http://elastic:changeme@localhost:9200/)""")##Fetching Data using SQL language and Pandas read_sql function
result_data=pd.read_sql("""
SELECT
 *
FROM
 kibana_sample_data_flights
WHERE
 "DestCountry" = 'IT'
 AND "timestamp" > '2020-03-06T21:08:31.000Z'
""", connection_es)
```

在上面的代码中，我们使用了之前在 SQL 客户机上运行的相同的 SQL 查询。

最后，我们将得到熊猫数据帧的输出，您可以使用该数据帧进行进一步的分析和可视化

![](img/e1fae51ca1c660a7ddd4a8d930585238.png)

Python 代码的最终输出

> **结论:**

在本文中，我们看到了如何使用简单的 SQL 语言而不是复杂的嵌套 REST API 查询来分析 Elasticsearch 中的数据。我们看到了从 Elasticsearch 查询数据的两种不同方式，一种是用 DB client，另一种是用 python。

**我希望你喜欢我的文章，如果你想了解更多关于这个话题的信息，你可以在**[**insta gram**](https://www.instagram.com/_aakash.rathore/)**或**[**LinkedIn**](http://www.linkedin.com/in/aakash-rathore-big-data-engineer)**上关注并留言给我。**