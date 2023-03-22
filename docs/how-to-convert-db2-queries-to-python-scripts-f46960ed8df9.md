# 如何将 DB2 查询转换成 python 脚本

> 原文：<https://towardsdatascience.com/how-to-convert-db2-queries-to-python-scripts-f46960ed8df9?source=collection_archive---------29----------------------->

## 提供一个简单的 3 步模板来提升技能并过渡到 python

![](img/205f8cc0beb76ba11bbfecb25a4f922b.png)

克里斯蒂娜@ wocintechchat.com 在 [Unsplash](https://unsplash.com/s/photos/server?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

许多公司正在使用 python 脚本运行常见的数据分析任务。他们要求员工将目前可能存在于 SAS 或其他工具集中的脚本转换为 python。这个过程的一个步骤是能够用新技术获取相同的数据。本文是关于将 DB2 查询转换成 python 脚本的。

如何将查询转换成 python？这听起来很难，但比你想象的要简单。一旦有了数据源的模板，您需要做的就是更改查询和输出文件名。

有几种方法可以做到这一点，但是我将概述一个直观的模板，它允许您在本地笔记本电脑/台式机上运行 DB2 查询。

## 在 DiscoDonuts 使用您的 DB2 专业知识

让我们假设你在一家大型甜甜圈公司工作，DiscoDonuts。您需要对 DB2 运行以下查询。通常，您可能会使用 DataStudio 之类的工具。很简单。

```
SELECT store_id, donut_style, date, volume, net_sales
 FROM donutsdb.sales_data 
 WHERE date = '2020-08-16'
 WITH UR;
```

现在，您的经理要求您开始使用 python。深呼吸；没那么难。在设置好代码之后，您只需要更新两个字段，输出文件的名称和查询本身。然后你点击运行。这有多简单？

## 初始一次性设置

如果您还没有，您需要联系您的 IT 部门安装一个工具(“IDE”)(如 PyCharm、VSCode、Jupyter Notebooks)。

要连接到 DB2，您需要输入您自己公司的数据库、主机名和端口 id。最有可能的是，您在当前使用的任何工具中都已经有了这些信息。

## 模板

首先，填写数据库连接信息。现在你可以保存这个模板，以备不时之需。

对于您想要运行的每个查询，您需要更新输出文件名和实际的查询本身。该查询被传递给 DB2，因此它与您已经在使用的 DB2 格式相同。

```
import ibm_db
import ibm_db_dbi
import pandas as pd# name your output file (and path if needed)
output_filename = "donut_sales.csv"# enter your query between the triple quotes
query = """ SELECT store_id, donut_style, date, volume, net_sales
 FROM donutsdb.sales_data 
 WHERE date = '2020-08-16'
 WITH UR; 
 """# one way to do credentialing
import getpass as gp                                     
uid=input('Enter uid:   ')                                                  
pwd=gp.getpass('Enter password (hidden): ')# connect to your database
db = (
    "DRIVER = {IBM DB2 ODBC DRIVER - DB2COPY1};"
    "DATABASE=<your donut database>;"
    "HOSTNAME=<your db2 hostname>;"
    "PORT=<your db2 port ####>;"
    "PROTOCAL=TCPIP;"
    'UID='+uid+';'
    'PWD='+pwd+';')
ibm_db_conn = ibm_db.connect(db, "", "")
pconn = ibm_db_dbi.Connection(ibm_db_conn)#optional if you are using the accelerator #ibm_db.exec_immediate(ibm_db_conn, "SET CURRENT QUERY ACCELERATION = ALL") df = pd.read_sql(query, pconn)
df.to_csv(output_filename,index=False)
```

点击运行。您将被要求输入您的凭证，您的查询将在 DB2 上运行，数据将被传输回您的脚本，您的文件将被创建！

如果您愿意，所创建的数据框可作为您的数据在 python 脚本中进行进一步分析。

## 下一个查询的三个步骤

1.  更新您的输出文件名
2.  更新您的查询
3.  点击运行！

## 结论

将 DB2 SQL 知识转移到 python 并不困难。这是一项很好的技能，可以与他人分享。

我总是欢迎反馈。如果你有另一种技巧，请在回复中分享。解决问题的方法有很多，我只介绍了其中一种。代码是不断发展的，所以今天有效的可能明天就无效了。