# 为您的分析堆栈设置雪花

> 原文：<https://towardsdatascience.com/setting-up-snowflake-for-your-analytics-stack-b252b063f20c?source=collection_archive---------39----------------------->

![](img/846e852b21445c03c4ac7a2357be991b.png)

本学长在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

雪花是一个基于云的分析数据仓库，由于其无需维护、易于使用和灵活的服务而变得越来越受欢迎。通过基于使用的定价模式将计算和存储分开，雪花能够根据不断变化的数据、工作负载和并发需求轻松扩展。

如果您刚刚开始使用 Snowflake，请稍等片刻，然后再开始部署一些特定的资源并构建您的分析应用程序。要充分利用雪花的每秒成本结构和出色的安全模型，需要从一开始就考虑配置，而不是 6 个月以后。

在这里，我们将总结来自 [dtb 的](https://www.getdbt.com/)团队的建立雪花环境的最佳实践，以及我们从实践中学到的东西。

# 有用的东西

首先，让我们熟悉一下《雪花》中的所有主要概念。

**数据库**:在传统的本地数据仓库中，您可能有多台服务器为各种数据源的多个数据库提供服务，而在雪花中，最高级别的抽象是一个数据库，您可以在每个帐户中拥有多个数据库。每个数据库中的数据被拆分成压缩文件，存储在云存储中，只有雪花专有的 SQL 引擎才能查询。这使得数据存储非常便宜，而且几乎是无限的。

**仓库:**仓库为雪花提供了在任何数据库上执行查询的计算能力。每个仓库都是一个 MPP 计算集群，基本上是一组执行实际数据处理的虚拟计算机。仓库是根据它们的大小和运行时间来收费的。

**用户:**可以用登录名和密码连接到雪花的单个用户。通过分配角色，用户被授予雪花资源的特权和权限。

**角色:**一个对象，为角色所分配到的任何用户定义一组雪花资源的特权和权限。

雪花让所有与帐户关联的用户及其所有资源使用一个如下所示的连接:

```
**Account =** [**host.region.cloud_service**](https://wh31430.ca-central-1.aws.snowflakecomputing.com/)**Url =** [**https://host.region.cloud_service.snowflakecomputing.com/**](https://wh31430.ca-central-1.aws.snowflakecomputing.com/)
```

默认情况下，雪花允许访问所有 IP 地址，但可以通过在帐户或用户级别将某些 IP 地址列入白名单来阻止流量。查看[网络政策](https://docs.snowflake.com/en/user-guide/network-policies.html)页面。

# 配置您的数据库

根据您的团队的数据加载和暂存策略以及您希望如何设置开发和生产环境，有多种方法可以设置数据库。但是，如果你不确定，那么这个有两个数据库的例子非常适合分析用例。

**原始:**

在基于 ELT 的转换工具对原始数据进行查询以进行清理、反规范化和建模，并进行分析之前，该数据库可作为一个登陆区。该数据库具有严格的权限，因此不能被 BI 和报告工具等最终用户访问。

批量数据可以使用内部阶段从本地机器复制到原始数据库，或者使用外部阶段从云存储(如 S3)复制到原始数据库。您还可以使用 Snowpipe 从 stages 加载连续数据，snow pipe 是雪花的连续数据摄取服务。

这是创建数据库的方法:

```
USE ROLE SYSADMIN;CREATE DATABASE IF NOT EXISTS RAW;
```

**分析:**

该数据库包含为分析师、报告工具和其他分析用例准备的转换数据。这个数据库中的所有表都是由 ELT 工具创建和拥有的，ELT 工具从原始数据库中提取数据。分析数据库中的表和视图可以根据特定用途组织成模式，然后可以将粒度权限分配给用户角色，以便对特定模式进行操作。例如，可以为审计员分配只允许访问模式视图的角色，这些视图可以通过对个人信息进行适当的转换和散列来准备。

# 设置您的仓库

雪花仓库是按大小和每分钟的活动收费的，所以重要的是要考虑你将使用每个仓库做什么，并相应地进行配置。仓库在不使用时可以手动或自动挂起，但在使用它们运行查询时会非常快地加速运行，因此建议在几分钟后将它们配置为 auto_suspend。您为仓库选择的规模取决于它们将要执行的工作负载的规模，我们建议从小规模开始，然后根据需要增加规模。对于分析用例，三个仓库的设置会很好。

**加载:**

这是将用于处理数据加载到雪花的仓库。像 Fivetran 或 Stitch 这样的工具或特定用户将被分配角色，以授予使用该仓库加载作业的权限。为这个特定的任务分离这个仓库是理想的，因为数据负载可以改变，给仓库带来压力，并且我们不希望其他工具因为使用同一个仓库而变慢。

这就是创建仓库的方式:

```
USE ROLE ACCOUNTADMIN;CREATE WAREHOUSE LOADING WITH
WAREHOUSE_SIZE = SMALL
MAX_CLUSTER_COUNT = 2
SCALING_POLICY = ECONOMY
AUTO_SUSPEND = 120
ALTER RESUME = TRUE
COMMENT = ‘Warehouse for performing data loads’;
```

**转换:**

您的 ELT 工具(如 dbt)从原始数据库执行数据转换，并将其保存到分析数据库，这些工具将与有权使用此仓库的角色连接。仓库只会在 ELT 作业被执行时运行，所以你只需在那段时间付费。

**报告:**

这是您的 BI 工具(如 Looker 或 Chartio)将使用的数据仓库。仓库查询数据的频率取决于 BI 工具的设置。

仓库中意想不到的高负荷会导致比预期更高的成本。雪花提供了[资源监视器](https://docs.snowflake.com/en/user-guide/resource-monitors.html)，可以用来限制你的仓库在特定时间内使用的信用数量。我们强烈建议设置资源监视器来指定每个仓库的限制，以及当达到限制时应该采取什么措施。下面是如何创建一个:

```
USE ROLE ACCOUNTADMIN;CREATE RESOURCE MONITOR ACCOUNT_LIMIT_250
WITH CREDIT_QUOTA = 250
FREQUENCY = MONTHLY
START_TIMESTAMP = IMMEDIATELY
TRIGGERS ON 100 PERCENT DO NOTIFY;
```

# 定义您的角色

设置角色可能是早期最重要的任务。确定层次结构将指导您分配角色。雪花角色可以授予其他角色，并且与角色相关联的特权由层次结构中的父角色继承。有许多系统定义的角色，如 accountadmin、securityadmin、useradmin、sysadmin 和 public。Public 是分配给每个用户的默认角色，其他角色根据用户需要的权限添加到用户中。以下是我们将设置的角色。

**装载机:**

该角色被授予原始数据库中所有模式的权限。它连接到装载仓库以执行所有数据装载任务。sysadmin 角色可用于创建此角色，并授予装载仓库和原始数据库的权限。

以下是创建角色的方法:

```
USE ROLE SYSADMIN;CREATE OR REPLACE ROLE LOADER 
COMMENT=’role holds privileges for loading data to raw database’;GRANT ALL ON WAREHOUSE LOADING TO ROLE LOADER;GRANT ALL ON DATABASE RAW TO ROLE LOADER;
```

**变压器:**

该角色被分配给使用 ELT 工具进行连接的用户。它被授予查询原始数据库的权限和拥有分析数据库上的表的权限。

**记者:**

该角色将分配给与 BI 工具连接的分析师和用户。该角色将只对分析数据库拥有权限。

# 创建您的用户

由于我们在角色级别分配权限，因此我们可以将这些角色分配给新用户，并为每个独立的工具和人员创建用户。这意味着您团队中的每个成员以及您的分析堆栈中与雪花集成的每个工具都将拥有自己的用户凭证。通常，您会希望用户身份由您的[身份管理解决方案](https://docs.snowflake.com/en/user-guide/scim.html)来管理，比如 OKTA。一旦完成，你就可以创建你的用户了。

**管理账户:**

在我们前面提到的系统定义的角色中，accountadmin 是顶级角色。root 用户—创建帐户的人将有权访问所有这些角色。这些角色非常强大，不应该分配给执行日常分析的用户。根用户将由您的 DBA 或 CTO 担任。

**分析师:**

分析师将通过各种方式连接到雪花，并执行各种任务。他们可能正在运行 SQL、python 脚本，或者使用 jupyter 笔记本和其他 ELT 工具对数据进行建模。可以为这些用户分配加载者、转换者或报告者角色，或者多个角色，具体取决于他们的功能。

要创建用户，您需要作为管理用户进行连接。以下是创建用户并为其分配角色的方法。

```
USE ROLE USERADMIN;CREATE USER <USER>
PASSWORD = ‘’
EMAIL = ‘’
MUST_CHANGE_PASSWORD = TRUE;USE ROLE SECURITYADMIN;GRANT ROLE LOADER TO USER <USER>;
```

然后，用户可以使用这些用户凭据登录并更改密码。

**数据加载工具:**

一些用户将被分配 loader 角色，按照角色权限的指定在原始数据库上执行数据加载。这些用户是为 Fivetran 和 Stitch 等数据集成工具创建的。

**ETL 工具:**

这些用户将被分配 transformer 角色，执行从原始数据库到分析数据库的转换工作。这适用于像 dbt 这样的 ELT 工具。

**商务智能工具:**

这些用户将被分配为报告者角色，以连接到分析数据库来使用数据。这些用户将成为你的商业智能和报告工具，如 Looker 和 Chartio。

保护用户帐户对于确保数据的最高安全级别也很重要。雪花有许多用于账户认证的[安全特性](https://docs.snowflake.com/en/user-guide/admin-security.html)。我们建议对管理帐户使用多因素身份认证，您可能也想为您的分析师设置它。对于您的云工具，OAuth 也是一个选项。

# 就这样…

我们已经为您如何为您的分析团队设置雪花提供了很好的指导。这样做不会花你很长时间，但会让你有条理地前进。现在，您可以继续处理更令人兴奋的问题，并实际推出分析解决方案来推动您的公司向前发展。

[Waterfront Analytics](http://www.waterfrontanalytics.com) 帮助公司建立他们的分析堆栈并构建分析用例。如果你需要更多关于雪花的帮助，或者想聊聊你的项目，请通过我们的网站联系。