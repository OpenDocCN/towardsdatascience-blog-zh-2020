# 数据仓库转换代码有味道

> 原文：<https://towardsdatascience.com/data-warehouse-transformation-code-smells-17877bb6521d?source=collection_archive---------32----------------------->

## *留意 SQL 转换中的这些问题迹象*

![](img/68a3cf52237c27354be0b951752e6de8.png)

照片由[雅各布·卡普斯纳克](https://unsplash.com/@foodiesfeed?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

当涉及到转换代码时，在数据工程中有一个奇怪的范例。虽然我们越来越多地将提取和加载(“EL”)编程作为生产软件标准，但是转换代码仍然被视为二等公民。具有讽刺意味的是，转换代码通常包含复杂的业务逻辑，这可以从像对待软件一样对待软件中获益匪浅。

代码气味是一种表面迹象，通常对应于系统中更深层次的问题。“更简单地说，代码气味是软件中的模式，它要求我们更仔细地观察。您应用程序中的代码气味与冰箱中的实际气味没有什么不同:刺鼻的气味可能表明有令人讨厌的东西存在(就像那盒放了十年的木须肉)，或者它可能像林堡干酪一样无害。代码味道并不保证问题存在，通常情况下，最佳重构类似于不同的味道。其价值在于，每一次出现都会提示您，哪种解决方案提供了最易读、最易维护的转换代码。

下面是一组特定于多维数据仓库转换的代码味道。遇到它们会让你停下来，给你机会离开代码库，比你发现它的时候更好。

# 多列联合语句

```
COALESCE(reciept_price,  
         COALESCE(label_price,  
                  COALESCE(catalog_price),0))) AS item_price
```

**翻译成英文:**“如果没有收据价格，试试标签价格，如果没有标签价格，试试目录价格，如果其他都不行，把价格算成 0。”

**为什么会有味道:**在少数几列中摸索以获取第一个可用值表明数据没有被很好地理解。要么代码不知道*为什么*一个列值值得优先选择，要么结果列是几个应该独立的状态的混搭。

**可能的重构:**上面的嵌套联合很可能表示多个独立的状态被强制为假条件。考虑用显式决策树(通常是一个`CASE`语句)替换，或者将每个状态分解成不同的事实。

# 作为标识符的保留字

```
SELECT user_sp_role AS "ROLE"
```

**翻译成英文:**“命名 user_sp_role 列`ROLE`，大家就知道是什么意思了。”

数据仓库设计的一个核心原则是界面应该简单。使用保留字(甚至是你的特定方言允许的保留字)会带来复杂性和混淆的机会。

**可能的重构:**坚持使用易于使用的详细标识符，不需要引号，并且将保持所有 SQL 能力的用户都可以访问数据仓库。`ROLE`可以更直观地命名为`web_application_role`，避免无谓的混淆。

# 维度中的基本空值

```
SELECT  
    customer_name  
    ,customer_address_id # null means we have no address on file  
    ,customer_phone  
FROM   
   customer
```

**翻译成英文:**“如果您想要所有没有注册电话号码的客户，只需选择电话号码为`NULL`的位置。”

**为什么有味道:** `NULL`在数据仓库世界里是一个非常重要的值。如果一个 join 出错，将会有`NULL`值。如果一个`group by`失败或者一个窗口函数没有像我们预期的那样滑动，那么就有`NULL`值。当`NULL`作为合法数据值执行双重任务时，调试变得几乎不可能。除此之外，BI 工具在呈现`NULL`值时通常表现不一致，这为 bug 提供了一个完美的藏身之处。

**可能的重构:**不要在维度中使用`NULL`值；明确陈述每个可能的条件(即在`CASE`语句中使用`ELSE`),这样任何`NULL`值都会立即引起审查。这不仅会强化您的转换代码，而且有助于最终产品数据的直观性。`NULL`可以表示很多东西，但是`'No Phone Number Available'`是非常清楚的。

这种味道只适用于维度属性。`NULL`数值不仅是正确的，而且是附加事实的重要数据点(如`total_sale_value`)。

# 神奇的数字

```
SELECT  
    customer_name  
    ,customer_address  
    ,customer_phone  
... 
WHERE  
    customer_unit_id IN (1,3,19)  
AND  
    customer_value_type = "a"
```

**翻译成英文:**“我们不再使用客户单元 8 或 13，所以我们忽略它们(ted 说 1、3 和 19 才是最重要的)。我们也只关心主要的网站客户价值类型(Bob 说这些是用‘a’表示的)。

**为什么有味道:**好的代码是自文档化的。这通常意味着你可以在没有解码环的情况下阅读代码并理解它的作用。上面的例子并不是因为复杂的业务逻辑或技术复杂性而具有挑战性，而是因为它充满了[部落知识](https://yoyodynedata.com/blog/tribal-knowledge.html)。

**可能的重构:**cte 是很好的数据映射工具:

```
WITH   
value_types AS (  
    "a" AS primary_website_customer  
...
)  
...  
AND  
    customer_value_type = value_types.primary_website_customer
```

当更大的重构不可行时，注释总比没有好。寻找可以更形象地命名的变量和常量，作为一种大大改进代码库的廉价方法。

# 数据消除

```
WITH all_visits AS (  
    SELECT   
        *  
    FROM  
        website_visits  
),  
SELECT  
    *  
FROM   
    website_visits  
WHERE   
    visit_id IS NOT NULL
```

**翻译成英语:**“网站访问应该总是有一个`visit_id`，所以如果他们没有，记录就是坏的，我们应该把它扔出去。”

**为什么有味道:**任何数据仓库的基础都是*真理*。不仅仅是一些，而是全部的真相，这是破坏性的转换所不能提供的。缺少记录(甚至是“坏”记录)的数据仓库没有可信度，您会很快发现消费者要求访问原始源。

**可能的重构:**转换逻辑应该是附加的，为最终用户提供更大的价值。在上面的例子中，一个新的列`valid_record`将过滤到 BI 层中的同一个数据集，同时为消费者提供访问“所有数据”的信心。

# 假设的商业逻辑

```
CASE   
   WHEN user_time_zone IS NULL THEN (last_login time at time zone 'pst')  
   ELSE (last_login_time at time zone user_time_zone)  
END AS last_login_time
```

**翻译成英语:**“我们的大部分网络流量来自旧金山湾区，所以如果一个网络访问丢失了时间戳，我们就把它更新到太平洋标准时间。”

**为什么有味道:**数据仓库的工作是为用户提供做出明智决策的能力，而不是为他们做决策。每次转换逻辑为数据选择路径时，它都不可避免地在这个过程中删除消费者的选项。

**可能的重构:**在上面的例子中，最初的`last_login_time`将理想地呈现`last_login_time_without_timezone`和`last_login_time_with_timezone`；最终用户可以自行决定对缺失的时区做出假设。

# 运行时间作为输入

```
SELECT  
   *  
FROM  
   all_records  
WHERE  
   created_at::DATE >= DATEADD('days',-1, CURRENT_DATE())
```

**翻译成英文:**“创建日期大于昨天的记录是新记录。”

**为什么有异味:**任何时候，相同的代码可以针对相同的数据运行两次，并返回不同的结果，请考虑这是一个问题。好的转换逻辑既是[幂等的](https://en.wikipedia.org/wiki/Idempotence)又是[确定性的](https://en.wikipedia.org/wiki/Deterministic_system)。当前日期或时间等不稳定元素会使代码变得脆弱，如果转换作业失败或运行两次，很容易使系统处于不可纠正的状态。

**可能的重构:**以自我修复的方式设计转换。使用相同的示例:

*   如果记录*保证*增加(没有迟到的记录)，只需要稍微修改。

```
SELECT 
   * 
FROM 
   all_records 
WHERE
   created_at > (SELECT MAX(created_at) FROM target_table)
```

*   源数据的更大波动性要求更大的转换复杂性(和更大的计算成本)。根据记录到达的时间有多晚，可以使用谓词语句将代码限制在一个窗口中。

```
SELECT  
all_records.*  
FROM  
all_records 
WHERE  
MD5(all_records.*::TEXT) NOT IN (SELECT MD5(target_table.*::TEXT) FROM target_table)

/*   
if records are always < 30 days late, you could restrict the lookup ie (SELECT MD5(target_table.*::TEXT) FROM target_table WHERE target_table.created_at::DATE >= DATEADD('days',-30, CURRENT_DATE()))  
*/
```

# 不一致的时态、前缀和后缀

```
SELECT 
   user_id 
   ,id 
   ,identifier 
FROM 
   users 
JOIN 
   site 
... 
JOIN 
   dim_visits
```

**翻译成英语:**围绕标识符的非结构化语法，列名的不规则前缀，以及缺乏词汇系统。

在数据仓库中，模式就是产品接口。不可预测的词汇会给用户带来不必要的摩擦。桌子是`order`还是`orders`？栏目是`sale_price`还是`order_sale_price`？没有模式，这些都是数据仓库可用性的开销。

**可能的重构:**选择约定。记录下来。更新转换代码以反映它们。使用同类语言的相同查询可能如下所示:

```
SELECT
    user_id
    ,site_id
    ,visit_id
FROM
    user
JOIN
    site
...
JOIN
    visit
```

# 标识符中的技术参考

```
CREATE OR REPLACE TABLE POSTGRES_USERS AS ...
```

**翻译成英文:**任何名称反映源系统(即`postgres_user`)、提取-加载介质(即`DATA_WAREHOUSE.STITCH.USERS`)或 ELT 过程的任何其他机械组件(即`cron_daily.users`)的表、视图、模式、数据库或列。

为什么会有气味:工程师很难走出我们自己的生活空间。这种味道通常是由于设计一个“源代码向下”而不是“最终用户向上”的模式而产生的。数据仓库*必须*以反映业务领域对象的方式表示信息；举个例子，某医院并没有把自己的消费者想成“*账单用户*”和“*图表系统用户*”和“*处方用户*”，他们都是简单的“*患者*”。

这是一种特别难察觉的味道，因为业务领域经常与技术领域非常接近，用户可能已经训练自己不正确地将一个与另一个对齐。如果零售商有不同的电子商务和物理销售点系统，很容易认为电子商务系统代表`web_users`而 POS 系统代表`in_store_users`。但事实并非如此；这家企业只有`CUSTOMERS`可能会在商店、网上或两者兼而有之。

可能的重构:把你的数据产品想象成 UX 设计师设计意图驱动的应用程序界面的方式。如果您登录您的 Medium 帐户，系统会要求您输入您的*用户名*和*密码*，而不是您的“[dynamo _ db](https://medium.engineering/the-stack-that-helped-medium-drive-2-6-millennia-of-reading-time-e56801f7c492#5371)”*用户名*和*密码*。按照同样的逻辑，你的数据仓库用户群感兴趣的是*页面访问量，*而不是*谷歌分析页面访问量*或 *Adobe 分析页面访问量*。

# 代码库外部的过程/函数

```
SELECT  
   super_amazing_stored_proc(122, 'another_magic_value') AS RPI_VALUE
```

**翻译成英语:**不是目标数据仓库的本地 SQL 方言的一部分，也不是作为代码库的一部分创建的函数。

**为什么会有味道:**如果我们将转换代码库视为构建数据仓库的蓝图，那么存储过程(不是作为代码库的一部分创建的)就是“帐外工作”。代码库不再拥有机器的所有元素，不能有效地复制仓库。这种危险而脆弱的状态使得仓库在实例崩溃时面临灾难性的失败。

**可能的重构:**如果你正在使用像 DBT 这样的 SQL 框架(或者任何真正的 SQL 预编译)，完全避免存储过程和函数。对于那些存储过程或函数是唯一可行的解决方案的罕见情况(或者如果您使用存储过程作为您的转换层)，在您的代码库中包含具有`DROP.. CREATE`或`CREATE OR REPLACE`模式的过程的定义，以确保每次运行都从您的代码中重新创建*。这将最小化代码状态和产品状态之间的差距。*

# 引用标识符

```
SELECT 1776 AS "FOUNDING_YEAR" FROM countries."America"."Important Dates"
```

**翻译成英文:**区分大小写或包含特殊字符或保留字的标识符。

**为什么这么说:** SQL 是第四代语言，像大小写折叠(将标识符视为不区分大小写的值)这样的约定的目的是为了更好地模拟人与人之间的交流。引用的标识符通常与这种意图背道而驰，迫使用户考虑大写，并可能导致混淆`"Leads_Prod"`与`"leads_prod"`的情况(这是两个不同的表！).

**可能的重构:**永远不要引用标识符。通过对数据库、表/视图和列使用详细的描述性名称来避免混淆和开销。额外的好处是，这样你的代码将是可移植的(case folding 在不同的平台上是*不*一致的，所以任何引用的标识符都是立即不可移植的)。

在数据仓库的早期，有一种勇敢的努力来引用所有的东西，使得标识符尽可能的漂亮，并且可以用像 T2 这样的列名来准备报告。在当时，这很有意义，因为大部分消费是直接从数据仓库表到报告和电子表格摘录。今天，我认为 BI 工具是这种“表示抛光”的最佳场所，并且数据仓库通过保持标识符的干净和冗长而受益更多。

# 没有时区的时间戳/时区不在 UTC 中

```
SELECT  
   TIMESTAMP '2020-01-01 13:10:02' AS questionable_tstamp  
   ,TIMESTAMP WITH TIME ZONE '2020-01-01 01:11:21+04' AS another_confusing_tstamp
```

**翻译成英文:**任何未显式转换为 UTC 值的时间戳，尤其是使用“本地时间”作为标准。

**为什么有味道:**时间戳是最混乱的数据类型。时间戳的实现和处理因平台、语言和工具的不同而有很大的差异。

**可能的重构:**显式地将所有时间戳转换为 UTC 进行存储。请注意，这与转换然后剥离时区*不同*(这是一种奇怪但痛苦的常见做法，可能源于一种信念，即没有时区的时间戳“更容易”)。
统一使用 UTC 将简化新数据集的入职流程，消除夏令时混乱，以及超越单一时区的面向未来的组织知识。让 BI 工具去担心时间戳表示(大多数工具都会这样做，那些“有用的”上游转换可能弊大于利)。

# 正常化

```
SELECT  
   u.user_name  
   ,u.user_area_code  
   ,si.site_name  
FROM   
   users u  
INNER JOIN  
   sites s  
ON   
   u.site_id = s.id  
INNER JOIN  
   site_identifiers si  
ON   
   s.id = si.site_id
```

**翻译成英语:**反映传统的 [BCNF](https://en.wikipedia.org/wiki/Boyce%E2%80%93Codd_normal_form) 的模式，你会期望在事务数据库设计中找到这些模式。在这个例子中，`site_identifiers`已经从`site`中规范化出来，以保护引用完整性。

**闻起来的原因:**数据仓库是 [OLAP](https://en.wikipedia.org/wiki/Online_analytical_processing) 结构，它满足了与事务数据库完全不同的需求。规范化和引用约束是 OLTP 系统如何工作的重要部分——但是这些工具不利于知识库的目标。数据仓库并不代表期望的状态(即所有的`page_views`都有一个存在于`traffic_sources`表中的`source_id`，它们代表的是*现实*(即一个 bug 将一百万个`page_views`关联到一个不存在的源)。从更高的角度来看，大量规范化的存在可能是一个强有力的指标，表明在整个代码库中已经遵循了其他 OLTP 约定。

**可能的重构:**维度模型设计超出了本文的范围(为了更好地理解维度模型与事务模型的区别，我强烈推荐 Ralph Kimball 的[数据仓库工具包](https://www.amazon.com/gp/product/1118530802/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1118530802&linkCode=as2&tag=ethanknox-20&linkId=fc8ac37534bc1fe08f5177a015dfba5d))。一般来说，这些标准化值应该“退化”成宽而平的维度表，如下所示:

```
SELECT  
   name  
   ,area_code  
   ,site_name  
FROM   
   users
```

# 隐藏的“模糊”逻辑

```
-- DDL for DATA_WAREHOUSE.SESSIONS  
WITH   
ordered_sessions AS (  
SELECT   
   *   
FROM   
   sessions   
ORDER BY insert_id  
)  
,session_merger AS (  
SELECT  
   CASE  
     WHEN TIMESTAMP_DIFF(a.session_end,b.session_start) < 60  
   AND  
     ABS(a.thumbprint_hash_points - b.thumbprint_hash_points) < 5   
   AND   
     EARTH_DISTNACE(a.location,b.location) < 0.5  
   THEN LEAST(a.session_id,b.session_id)  
   ELSE NULL   
   END AS merged_session_id  
FROM  
   ordered_sessions a  
INNER JOIN  
   ordered_sessions b   
ON   
a.insert_id +1 = b.insert_id  
)  
,refined_sessions AS (  
SELECT   
   o.*  
FROM  
   ordered_sessions o  
WHERE  
   o.session_id IN (SELECT merged_session_id FROM session_merger)   
)  
CREATE TABLE sessions AS 
SELECT * FROM refined_sessions
```

**翻译成英文:**被看似稳定的标识符掩盖的复杂变换。

**为什么有味道:**“粘糊糊的”逻辑是任意合理的业务逻辑:在上面的例子中，代码判定“两个间隔不到一分钟、指纹非常接近、来自(几乎)相同位置的会话很可能是同一个用户会话。”这里的味道不是逻辑——这是否是合并浏览器会话的准确方式取决于企业；气味代表“可能与绝对值相同的用户会话”`session`。

**可能的重构:**数据仓库转换代码表示*已知*为真的东西。在这个例子中，我们*知道*每个会话都存在，而我们*假设*某些会话实际上是同一个会话。如果假设得到业务的支持，它可以很容易地以`likely_parent_session`列的形式表示为*附加信息*。这个假设之上的聚合可以存在于另外的物化中，即`dim_collapsed_session`和`fact_collapsed_conversion`等。通常需要不止一个假设来支持业务用例的范围。在这种情况下，每个假设要么可以在特定领域的市场中进一步具体化，要么被“标记”并用于丰富数据仓库中的`dim_session`。

```
/* downstream mart */
SELECT
	amalgamated_session_id 
	,duration
	...
FROM marketing.amalgamated_sessions

/* "branded" in dim_session */
SELECT
	session_id	
	,aggressive_merge_session_id
    ,conservative_merge_session_id
    ,halifax_merge_session_id
...
FROM
    dim_session
```

# 没有消费者文件

```
/* what do these mean?!? */
SELECT
	halifax_merge_session_id 
	,aggressive_merge_session_id 
FROM
	dim_session
```

**翻译成英语:**对于使用数据仓库的消费者来说，他们需要来自转换作者的输入。

**为什么有味道:**数据仓库既是商业工具，也是消费品。像任何商业用途的复杂工具一样，它*必须附带全面的文档。想象一下，学习使用 Excel 中的`VLOOKUP`函数的唯一方法是打电话给微软工程师！没有[面向消费者的文档](https://support.office.com/en-us/article/vlookup-function-0bbc8083-26fe-4963-8ab8-93a18ad188a1)，产品将无法使用。*

可能的重构:文档可以存在于很多地方。几乎所有的数据仓库平台都支持 SQL `comment` meta for objects。如果你使用像 DBT 这样的转换框架，那么面向消费者的文档就和 T6 结合在一起了。文档也可以用类似于 [Sphinx](https://www.sphinx-doc.org/en/master/) 、[Read Docs](https://readthedocs.org/)的工具来管理，甚至是简单的 markdown 文件。文档解决方案至少必须:*易于消费者访问。*作为数据产品的一部分进行维护。*支持有效的搜索和导航。*尽可能完整,“内部”参考

# 查询式标识符命名

```
SELECT
	s.order_id
	,w.order_id
...
FROM
	confirmed_ecom_system_orders s
JOIN
	client_side_web_orders w
```

**翻译成英文:**使用速记模式的别名，往往一两个字母长。

**为什么有味道:**缩写的速记对于编写快速的即席查询非常有用。但是像所有好的软件一样，转换代码应该是自文档化的，并使用有*含义的对象名。*

**可能的重构:**命名标识符是软件开发中的两大难题之一。使用在转换中具有描述性和唯一性的别名，并传达所表示的表/CTE/数据集的内容:

```
SELECT
	ecom_orders.order_id
	,web_orders.order_id
...
FROM
	confirmed_ecom_system_orders ecom_orders
JOIN
	client_side_web_orders web_orders
```

# KPI 中的迎合逻辑

```
SELECT
	is_conversion_marketing
	,is_conversion_business_development
	,is_conversion_finance
FROM 
	web_orders
```

**翻译成英语:**“垂直行业拒绝就围绕 KPI 的业务逻辑达成一致，因此我们支持真相的多个版本。”

**为什么这么臭:** [组织成熟度](https://yoyodynedata.com/blog/is-your-company-too-dumb-to-be-data-driven.html)是任何成功的数据计划的关键要素。如果企业不愿意(或不能)做出有时很困难的决策，并使用统一的事实来源向前推进，这种犹豫不决将反映在数据仓库代码库中。

**可能的重构:**对这种气味的重构在技术上很简单，但实际上很难。业务必须发展，并宣布一个所有垂直行业都将采用的单一定义。在 SQL 中，这很简单:

```
SELECT
	is_conversion
FROM 
	web_orders
```

在现实世界中，这可能是一个政治雷区。

最初发表于[https://yoyodynedata.com](https://yoyodynedata.com)。