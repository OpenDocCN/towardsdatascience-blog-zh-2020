# 如何创建数据质量仪表板

> 原文：<https://towardsdatascience.com/data-quality-dashboard-9c60f72b245c?source=collection_archive---------14----------------------->

## 提高数据质量的基石

仪表板的主要目的是提供一个全面的性能快照，这意味着您应该包含大量的细节，而不需要使用太多的向下钻取。它使用过去的数据来确定趋势和模式，这些趋势和模式可以帮助设计未来的流程改进。

数据质量仪表板是一种信息管理工具，它可以直观地跟踪、分析和显示关键绩效指标，突出显示关键数据点，以监控业务、部门或特定流程的运行状况。它们可以定制，以满足企业的特定需求，它显示了您对数据的信任程度。

提高数据质量是一个长期的过程，这种计划的最佳结果是防弹过程，它将在未来为您服务，而不仅仅是及时清理数据。如果你想有效率，你应该让你的过程在形状监视器中变化并控制它，而不是定期执行数据清理练习。纠正数据非常耗时，因此在设计和实施新流程时，请尝试提前考虑。在质量保证上投入时间可以为你节省很多后期工作。

如果您仍然不知道为什么需要可靠的数据质量，请查看本文:

[***在这里，你可以选择 10 个让数据质量井然有序的理由。***](http://Completeness doesn’t bring to much value to the table. It can be misleading as you can have all the attributes completed but it`s about the content of the field you are validating and it still can be garbage.   Compliance/Validity - This should be the focus when starting a data quality program. Does your data is satisfying business usage requirements? We can split this to : 	• Format checks 		○ This depends on company standards and markets but examples are: 			§ Formats of postal codes (you need to define this per countries) 			§ Minimum and Maximum Number of characters - so there are no 1 character addresses or company names 			§ In the case of a global company you can check if local characters are used in global names   	• External Reference data compliance 		○ Checking external standard can be very beneficial as this will be very usable in reporting but also many of those classifications can be regulatory ones that are mandatory to run business (Customs Tariffs numbers) 		○ Are countries codes are compliant with ISO codes standard. 			§ Are Customers classified with SIC codes that exist 			§ Various Product classifications are mandatory in many markets: Wee classification, ETIM, UNSPC, Customs Tariffs. You can get the list valid codes and perform validation of your products   	• Internal Master Data compliance: 		○ For better Strategic reporting companies are implementing internal segmentation or classification of customers products and orders. 			§ You need to create a master data reference table and then validate your records classifications against it. 			§ Various internal business rules can be implemented here but this should be suited to your needs and started with the design phase to address actual issues of organizations: 				□ Each product need to have assigned active profit center  				□ Each Client needs to have an active owner.   Consistency - is your data the same in different sources? This should be relatively easy if you have Master data in place and each local records are connected to your master data source of truth with a global identifier. Just compare key attributes in different sources. In an ideal world data should be syndicated from source of truth to consuming systems: 	• Addresses in CRM against SAP against your Master Data     Timeliness - is your data up to date 	• schedule and monitor the data review process. Each records with  	•      defined last edit date should be reviewed by the owner 	• Is your data provided on time? - Creating new customers is a workflow and you can have different requirements on different steps of the sales process, always consider the business process when monitoring such aspects.  		○ For example, newly locally created customers records should be cleared and assigned with Global Customer Id withing 2 working days   Correctness/Accuracy - This is the most difficult one as there is no easy way to check that from the IS point of view. How can you check if product weight is 10 kg and not 15 kg? Ideally it`s established through primary research. In practice: 	• Manual auditing of sample 	• use 3rd party reference data from sources which are deemed trustworthy 	• Data Profiling and finding coherence patterns 	• Manual validation by owner – akin of crowdsourcing It is not possible to set up a validation rule. Instead, you should focus on creating logic that will help discover any suspicious pattern in your data that varies from what you saw in past     What to measure : Start with talking to data users and define your top elements and attributes and then the purpose of it. Look into user problems try a design thinking approach to discover opportunities. Define more crucial ones and start measuring them. You can group your KPI` into domains and accountable parties, then calculate quality indexes. Customer Data Quality can be built as a weighted average of attributes quality (Address, Vat, Owner) but also can include processes whenever these records are updated on time or are consistent in different sources. Juggle with weights  based on Business Impact     Make them SMART   Specific - specifically designed to fulfill certain criteria Measurable - KPI Status is constantly monitored and available on the dashboard Accountable - Clearly defined responsible party Relevant - They have an impact on business Time-based - there is time scope within they should be reach       My KPI checklist: 	• The object or its attribute is defined in the metadata tool rulebook or instruction 	• There is a clear business objective - so people now why are they doing it this is important and it is building data quality awareness across the organization 	• Clearly accountable function/person/organization 	• Communication plan 	• Reviewed with reference team and stakeholders         To recap, to improve and manage your data quality you need to know where you are now and where you want to be in a defined period so: 	• Describe your data with clear definition and rules 	• Implement a continuous monitoring process and keep systems and processes up to date. 	• Engage with everyone responsible for the data tell them why and build data quality awareness Increase the accountability for the data by assigning responsible)

![](img/98f64fd4a02bfe9bcef78c0a8bed127e.png)

斯蒂芬·道森在 [Unsplash](https://unsplash.com/s/photos/dashboard?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

这些是主要的数据质量维度:

**完整性—** 不会给桌子带来太多价值。这可能会产生误导，因为您可以完成所有属性，但它是关于您正在验证的字段的内容，它仍然可能是垃圾。

**合规性/有效性—** 这应该是启动数据质量计划的重点。您的数据是否满足业务使用需求？首先定义规则，然后根据规则分析数据。我们可以把它分成:

1.  格式检查，取决于公司标准、地点和市场，例如:

*   邮政编码的格式(您需要根据国家定义)
*   最小和最大字符数—因此没有 1 个字符的地址或公司名称
*   如果是全球性公司，您可以检查全球名称中是否使用了本地字符

2.外部参考数据合规性

*   检查外部标准可能非常有益，因为这将在报告中非常有用，而且这些分类中的许多可能是经营业务所必需的监管分类(关税号码)
*   根据 ISO 代码标准验证国家代码。
*   根据全球列表，使用有效 SIC 代码分类的客户
*   各种产品分类在许多市场是强制性的:杂草分类，ETIM，UNSPC，关税代码。您可以获得有效代码的列表，并对您的产品进行验证，以便它们能够满足业务需求。

3.主数据合规性—为了更好地进行战略报告，公司正在对客户产品和订单进行内部细分或分类。您需要创建一个主数据参考表，然后根据它验证您的记录分类。这里可以实现各种内部业务规则，但这应该适合您的需求，并从设计阶段开始，以解决组织的实际问题，如:

*   每种产品都需要有一个指定的活动利润中心
*   每个客户端都需要有一个活动的所有者。

**一致性—** 不同来源的数据是否相同？如果您有主数据，并且每个本地记录都通过一个全局标识符连接到您的主数据源，那么这应该相对容易。只需比较不同来源中的关键属性。在理想世界中，数据应该从真实来源整合到消费系统:

*   根据 SAP 和您的主数据在 CRM 中查找客户地址

**及时性—** 您的数据是最新的吗

*   安排和监控数据审查过程。每个记录有
*   确定的最后编辑日期应由所有者审核
*   您的数据是否按时提供？—创造新客户是一个工作流程，您可能对销售流程的不同步骤有不同的要求，在监控这些方面时，请始终考虑业务流程。
*   例如，本地新创建的客户记录应在 2 个工作日内被清除并分配全球客户 Id

**正确性/准确性** —这是最困难的一个，因为从 is 的角度来看，没有简单的方法来检查这一点。你怎么能检查产品重量是 10 公斤而不是 15 公斤？理想情况下，它是通过初步研究建立起来的。实际上:

*   样本的人工审核
*   使用来自可信来源的第三方参考数据
*   数据剖析和寻找一致性模式
*   所有者手动验证——类似于众包

无法设置验证规则。相反，您应该专注于创建逻辑，这将有助于发现您的数据中与过去不同的任何可疑模式。

*你应该测量什么:*

从与数据用户交谈开始，定义你的主要元素和属性，然后定义它的目的。

调查用户问题，尝试用设计思维的方法来发现机会。

定义更重要的，并开始衡量它们。

您可以将 KPI 分组到业务领域和责任方，然后计算质量指数。

客户数据质量可以构建为属性质量(地址、增值税、所有者)的加权平均值，但也可以包括这些记录按时更新或在不同来源中保持一致时的流程。基于业务反馈校准度量迭代的权重。

*让他们变聪明*

特定—专为满足特定标准而设计

可衡量—状态受到持续监控，并可在仪表板上查看

负责——明确定义的责任方

相关—它们对业务有影响

基于时间—在他们应该到达的时间范围内

*我的 KPI 清单:*

*   对象/属性在元数据工具规则手册或说明中定义
*   有一个明确的业务“为什么”，所以人们现在为什么要这样做这很重要，它在整个组织中建立了数据质量意识
*   明确负责的职能/人员/组织
*   沟通计划
*   与参考团队和利益相关者一起审查

概括地说，为了提高和管理您的数据质量，您需要知道您现在所处的位置，以及在规定的时间段内您想要达到的位置，因此:

*   用清晰的定义和规则描述你的数据
*   实施持续的监控流程，并保持系统和流程最新。
*   与负责数据的每个人接触，告诉他们原因，并建立数据质量意识
*   通过分配负责人来增加数据的责任

在 [Twitter](https://twitter.com/Solution_Tailor) 上关注我，或者订阅[我的博客](https://www.solution-tailor.com/)以获得数据质量主题的更新。

*原载于 2020 年 11 月 2 日 https://www.solution-tailor.com*[](https://www.solution-tailor.com/post/data-quality-dashboard-your-cornerstone-in-improving-data-quality)**。**