# 云中的 MySQL 8.0

> 原文：<https://towardsdatascience.com/mysql-8-0-in-the-cloud-927c8dddd908?source=collection_archive---------43----------------------->

![](img/7447f3170258b4d9ad8d6ddfa5a8c7c6.png)

照片由[晨酿](https://unsplash.com/@morningbrew?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/google?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

## Azure、Google 和 AWS 现在都支持 MySQL 8.0

# [万岁！](https://linktr.ee/kovid)

两天前，当 Google Cloud 宣布在 Cloud SQL 上支持 MySQL 8.0 GA 时，最受欢迎和使用最广泛的开源数据库终于在 cloud trinity 上实现了。第一个在云中提供 MySQL 8.0 升级的显然是 AWS，然后是 Azure，现在是 Google。

[](https://cloud.google.com/blog/products/databases/mysql-8-is-now-on-cloud-sql) [## MySQL 8 现已登陆云 SQL |谷歌云博客

### 云 SQL 旨在提供不复杂的多层安全性，无论您是想保护您的…

cloud.google.com](https://cloud.google.com/blog/products/databases/mysql-8-is-now-on-cloud-sql) 

MySQL 8 提供了一系列奇妙的功能，如在线 DDL、期待已久的窗口函数、增强的 JSON 功能、用于分层查询的递归公共表表达式(cte)、默认`UTF8MB4`排序&编码、软删除、不可见索引、递减索引、分阶段推出等等。Cloud SQL 确实提供了标准的安全特性，可以选择用自己的密钥加密数据。传输中加密和静态加密都受支持。您可以在 MySQL 服务器团队的以下博客文章中找到特性列表

[](https://mysqlserverteam.com/whats-new-in-mysql-8-0-generally-available/?fbclid=IwAR1mUEwsOGtj4J7agfCAHeceQl9Pguya2Ecez_bPDfGNwt8nW7EnPnaiIMw) [## MySQL 8.0 有什么新特性？(正式上市)

### 我们自豪地宣布 MySQL 8.0 正式上市。立即下载！MySQL 8.0 是一个非常令人兴奋的新版本…

mysqlserverteam.com](https://mysqlserverteam.com/whats-new-in-mysql-8-0-generally-available/?fbclid=IwAR1mUEwsOGtj4J7agfCAHeceQl9Pguya2Ecez_bPDfGNwt8nW7EnPnaiIMw) 

要探索 MySQL 8.0 的详尽特性列表，请访问 MySQL 服务器团队发布的[这篇博客文章](https://mysqlserverteam.com/the-complete-list-of-new-features-in-mysql-8-0/)。

支持 BCP 的其他标准功能，例如通过使系统具有容错能力来实现高可用性，以及可以选择将相同的数据复制到多个区域。还有云 SQL 代理——

> *云 SQL 代理允许具有适当权限的用户连接到第二代云 SQL 数据库，而不必手动处理 IP 白名单或 SSL 证书。它的工作方式是在本地机器上打开 unix/tcp 套接字，并在使用套接字时代理到相关云 SQL 实例的连接。*

如果您还没有将本地或云 MySQL 实例升级到 MySQL 8，现在正是时候。与每次升级一样，MySQL & SeveralNines 的以下博客文章中有许多警告(没有任何限制)

[](https://severalnines.com/database-blog/moving-mysql-57-mysql-80-what-you-should-know) [## 从 MySQL 5.7 迁移到 MySQL 8.0 —您应该知道的

### MySQL 8.0 在性能和安全性方面有重大改进，在此之前有几件事需要考虑…

severalnines.com](https://severalnines.com/database-blog/moving-mysql-57-mysql-80-what-you-should-know) [](https://mysqlserverteam.com/upgrading-to-mysql-8-0-here-is-what-you-need-to-know/) [## 升级到 MySQL 8.0？以下是你需要知道的…

### 在我之前的博文中，我描述了从 MySQL 5.7 升级到…的就地升级步骤

mysqlserverteam.com](https://mysqlserverteam.com/upgrading-to-mysql-8-0-here-is-what-you-need-to-know/) 

# 竞争对手

MySQL 最接近的竞争对手是 PostgreSQL。如果您尝试并理解这两个数据库的产品路线图，您会发现它们的目标都是做对方做的事情。PostgreSQL 大概是想实现一层存储引擎。另一方面，MySQL 正在尽力提供强大的 JSON、数组和地理空间支持。MySQL 最后引入了窗口函数。PostgreSQL 已经拥有它们很多年了。

优步的工程团队有一篇颇具争议的博文，是关于几年前他们为什么从 PostgreSQL 转向 MySQL 的。这是一本非常有趣的读物，但也受到了批评。

[](https://eng.uber.com/postgres-to-mysql-migration/) [## 为什么优步工程公司从 Postgres 转向 MySQL

### 优步的早期架构包括一个用 Python 编写的单片后端应用程序，用于数据…

eng.uber.com](https://eng.uber.com/postgres-to-mysql-migration/) 

在过去几年中，谷歌在诸如 Airflow、Kubernetes 等开源工具的支持下，加速了其云扩张。并公开 Google 自己的高度可扩展的服务，如 BigTable。随着像 Kaggle 和 Looker 这样的收购，谷歌在未来几年的前景看起来很好。我想说的就是——在谷歌云平台上运行 MySQL 8.0 的时候要有安全感。

谷歌云 SQL 简介