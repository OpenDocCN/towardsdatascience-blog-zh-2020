# 金融服务中的大数据

> 原文：<https://towardsdatascience.com/big-data-in-financial-services-f3d4fd7a30cd?source=collection_archive---------19----------------------->

## 数据科学、大数据、金融

## 现在的情况与 10 年前有所不同，因为我们现在拥有的数据比以前更多。

![](img/60911508bf98a9070dc1a760ebddc167.png)

约书亚·索蒂诺在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

我们已经分析了千万亿次级的大数据，zettabytes 将是下一个(Kaisler 等人，2013)。

但是什么是大数据呢？简而言之，就是前所未有的保留、处理和理解数据的能力(Zikopoulos，2015)。大数据是指您面临传统数据库系统无法应对的挑战。

在数量、多样性、速度和准确性方面的挑战。这些都可以在金融服务中找到。事实上，技术是银行业不可或缺的一部分，以至于金融机构现在几乎与 IT 公司无法区分。

本文着眼于金融服务行业，研究大数据和所采用的技术。它进一步涵盖了投资回报、大数据分析、监管、治理、安全性和存储，以及造就该行业今天的障碍和挑战。

# 金融服务中的大数据

许多金融服务机构已经通过投资计算和存储大数据的平台开始了他们的大数据之旅。为什么？因为这里面有价值，而寻找新的价值形式是金融服务业务的全部内容。

大数据客户分析通过观察消费模式、信用信息、财务状况以及分析社交媒体来更好地了解客户行为和模式，从而推动收入机会。

大数据也是金融服务数据提供的核心业务模式的关键，例如彭博、路透社、数据流和金融交易所，它们提供金融交易价格并记录每秒数百万笔日常交易，以供客户分析使用和满足监管合规性要求(SOX、GDPR)。

所有这些大数据都必须存储起来(参见 NASDAQ 案例研究),因为无论计算机的速度有多快，无论组织的规模有多大，总有比组织知道如何处理或如何组合的数据更多的数据源，并且数据已经静止和流动了很长时间。

以下是大数据对金融行业的规模和社会影响的图示。

![](img/f298791b77caac0fda09b959bf7eb91d.png)

(来源:[https://www . businessprocessincubator . com/content/technology-empowers-financial-services/](https://www.businessprocessincubator.com/content/technology-empowers-financial-services/))

![](img/37820d7abe13c31fd8b702238c9ef1e0.png)

(来源:[https://www . slide share . net/swift community/nybf 2015 big-dataworksession final 2 mar 2015](https://www.slideshare.net/SWIFTcommunity/nybf2015big-dataworksessionfinal2mar2015))

经过深入的小组研究，大数据已被发现存在于价值链的每个部分，下面给出了一些确定了收益实现(ROI)的示例:

● **VISA** 于 2011 年通过使用 IMC 的“内存计算”平台和网格计算来分析信用卡欺诈检测监控的大数据，从而获得了竞争优势。以前只有 2%的交易数据受到监控，现在有 16 种不同的欺诈模式，分布在不同的地区和市场(Celent，2013)。

● **Garanti Bank，**土耳其第二大盈利银行，通过使用复杂实时数据(如 web、呼叫中心和社交媒体数据)的 IMC 进行大数据分析，降低了运营成本并提高了绩效(Karwacki，2015)。

● **BBVA，**西班牙第二大银行集团使用大数据交互工具 Urban Discovery 检测潜在的声誉风险，提高员工和客户满意度。他们还了解了客户在分析大数据时的感受，从而制定公共关系和媒体策略(Evry，2014)。

● **花旗集团**(美国第四大银行)将大数据技术 Hadoop 用于客户服务、欺诈检测和网络分析(Marr，2016)。根据(花旗报告，2017)金融机构使用大数据进行利率和 GDP 预测。

● **Zestfinance** 使用大数据 ZAML 技术密切监控其交易对手的信用质量，并试图评估其整体信用风险敞口(Lippert，2014)。

● **BNY 梅隆**通过潘兴 NetX360 平台实施了大数据技术，为其投资者审视风险和机遇(BNY 梅隆报告，2016)。

● **美国银行**利用大数据增强多渠道客户关系，并在从网站、呼叫中心、柜员开始的客户旅程中使用，以提高其服务水平和客户留存率(Davenport，2013)。

● **纳斯达克**使用AWS S3 大数据存储技术，每天向 AWS 提供数十万个小文件，然后在几毫秒内返回给客户，用于其市场回放和数据点播服务，以满足客户快速访问历史股价信息的需求，查看任何历史时间点的账面报价和交易价格。交易员和经纪人也用它来寻找错过的机会或潜在的不可预见的事件。低成本的理想解决方案(AWS 案例研究)。

![](img/f8817f4af8a792cd4aeacb46cd960993.png)

(来源:[https://www . data Maran . com/blog/the-big-data-revolution-doing-more-for-less/](https://www.datamaran.com/blog/the-big-data-revolution-doing-more-for-less/))

以下**案例研究**更详细地探讨了金融服务中的大数据技术。

![](img/5013b01dde4f5b7c6f46e3158ffbb75b.png)

照片由 [CHUTTERSNAP](https://unsplash.com/@chuttersnap?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 案例研究 1-摩根大通和 Hadoop

**JP Morgan Chase** 银行面临着理解信用卡交易(消费模式)产生的大数据以及非结构化和结构化数据集数量大幅度增长的挑战。

他们选择 Hadoop(开源框架)来利用大数据分析来分析客户的支出和收入模式。Hadoop 现在可以帮助挖掘非结构化数据(如电子邮件、脸书或 Twitter 标签)，这是传统数据库无法做到的。

借助大数据分析，it 可以确定最适合其客户的最佳产品，即在客户需要时为其提供合适的产品。因此，该银行选择了 Hadoop 来支持不断增长的数据量以及快速处理复杂非结构化数据的需求。

类似地，(Pramanick，2013 年)声称，金融机构使用大数据来分析他们的呼叫中心，通过分析非结构化语音记录进行情感分析，这有助于他们降低客户流失率，并向客户交叉销售新产品。

# 案例研究 2-德意志银行、算法匹配和 Hadoop

**德意志银行**选择使用多种大数据平台和技术，例如构建风险平台，在此平台上挖掘非结构化数据并将其处理为可理解和可展示的格式，以便德意志银行的数据科学专家获得分析活动的洞察力和灵感。大数据技术应用于公司的交易和风险数据(收集时间超过 10 年)。

数据量和多样性需要大量的存储、排序和筛选，但借助大数据技术，可以快速高效地获取和提取数据，以提高性能，并通过使用匹配算法获得竞争优势，该算法挖掘数据集告诉他们的关于销售、满意率和投资回报的信息，从而从数字中获得更真实的意义和更深入的视角。

(Sagiroglu 等人，2013 年)还列举了金融公司使用自动化个性化推荐算法来提高客户亲密度的例子。因此，识别客户需求并提供量身定制的信息对金融机构来说是有利的。

大数据有助于识别合法交易与欺诈交易(洗钱、盗窃信用卡)，并通过设置以下参数预测贷款违约支付行为、借款人与银行接触点的互动历史、征信机构信息、社交媒体活动和其他外部公共信息。(《经济情报》，2014 年)

该银行在 2013 年使用 Hadoop 来管理非结构化数据，但对与传统数据系统的集成持怀疑态度，因为存在多个数据源和数据流、价值数百万英镑的大型传统 IBM 大型机和存储库，并且需求与其传统系统和较新数据技术的功能相匹配，并对其进行简化。

最终，Hadoop、NoSQL 和大数据预测分析的组合适应了其所有运营领域，技术现在被视为德意志银行决策和战略管理行为中的重要因素。

# 案例研究 3-纳斯达克、存储(AWS 红移)和可扩展性(虚拟化)

所有金融交易交易所都存在大数据问题，纳斯达克也不例外。价值 7.9 万亿美元，每秒数百万次交易，一天内交易 29 亿股的世界纪录(2000 年)，与容量相关的延迟是一个大问题，例如，处理 650 万个报价会因处理大量数据而导致处理延迟(Greene，2017)。

2014 年，这个问题变得更加复杂，因为公司的业务战略是从一个主要的美国股票交易所转变为一个企业、交易、技术和信息解决方案的全球提供商。这意味着他们需要进行更多的数据处理，并存储法律要求他们持有的大量数据内容。

金融服务行业的公司通常需要将交易信息保留七年，这是一项法规要求，可能会产生巨额存储费用。在纳斯达克，他们有一个传统的仓库(每年维护 116 万美元)和有限的容量(即只有一年的在线数据)。

他们的大数据量包括每个交易日插入的 40-80 亿行数据。数据构成是订单、交易、报价、市场数据、安全主数据和会员数据(即结构化数据)。数据仓库用于分析市场份额、客户活动、监控和计费。

还有一个性能挑战。当客户进行查询时，纳斯达克负担不起加载昨天的数据。灾难恢复是纳斯达克的另一个重要因素。

最初的要求范围是通过迁移到 AWS Redshift 来替换本地仓库，同时保持等同的模式和数据，并且大数据解决方案必须满足两个最重要的考虑因素，即安全性和法规要求。方法是从众多来源提取数据，验证数据，即数据清理，并安全地加载到 AWS Redshift 中。

更详细地说，数据验证需要每天对坏数据/丢失数据发出警报，接收必须强制执行数据约束，即不重复，并且对于敏感的交易和公司数据来说，速度和安全性是最重要的。这最初导致了一个问题，因为 AWS 红移不强制数据约束。如果有欺骗，它接受它。

# **符合监管要求的安全解决方案**

数据是他们业务的基础，处理敏感数据意味着纳斯达克的安全要求很高，因为他们是一家受监管的公司，必须对 SEC(美国证券交易委员会)负责。

因此，他们需要安全的系统，因为大数据的一大挑战是，它将使个人财务信息更加分散，因此更容易被窃取。AWS Redshift 通过提供多层安全来满足安全要求:

1.  直接连接(专线，即没有公司数据通过互联网线路传输)。
2.  VPC-将 Nasdaq Redshift 服务器与其他设备隔离/互联网连接+安全组限制入站/出站连接。
3.  动态加密(HTTPS/SSL/TLS)。所有 AWS API 调用都是通过 HTTPS 进行的。SSL(安全套接字层)意味着有一个验证集群身份的集群证书认证系统。
4.  红移中的静态加密(AES-256)。这意味着红移加载文件是在客户端加密的。
5.  CloudTrail/STL_connection_Log 来监控未授权的数据库连接。这意味着监控集群访问并对任何未经授权的连接做出反应。

此迁移项目的优势实现如期完成，并带来了以下优势:

●红移成本是传统数据仓库预算的 43%(对于大约 1100 个表的相同数据集)

●2014 年平均每天 55 亿行

●高水位线=一天 14 行

●下午 4 点到 11 点摄入

●最佳写入速率约为 2.76 兆行/秒

●现在查询运行速度比传统系统更快。

●数据存储负载增加到 1200 多张表。

●通过安全审核。内部控制；信息安全，内部审计，纳斯达克风险委员会。外部审计；SOX，SEC。

Redshift 通过在多个数据中心复制数据解决了灾难恢复问题。

纳斯达克(2014 年)的当前数据存储需求= 450 GB/天(压缩后)加载到 Redshift 中。之前的压缩平均值，即未压缩= 1，895 GB/天。集群每季度调整一次大小以满足需求。NDW(纳斯达克数据仓库)目前每季度增长 3 倍！

纳斯达克的另一个问题是在不影响性能和成本的情况下的可扩展性。NASDAQ 发现 Apache Hadoop 和大数据分析所需的基础架构可能难以管理和扩展。所有这些过去的不同技术不能很好地协同工作，管理所有这些技术的成本也很高。

因此，It 使用云虚拟化来消除硬件限制，降低软件基础架构软件复杂性、成本和部署 Hadoop 和 Spark 的时间。虚拟化简化了所需的技术和资源数量，最大限度地提高了处理器利用率(18–20%到 80–90%)，保持了安全性，并且通过虚拟化将性能提高了 2%。

从工程和运营的角度来看，纳斯达克能够满足客户的大数据分析需求，而无需增加员工数量。因此，蓝色数据帮助纳斯达克推动了创新(Greene，2017)。

# 挑战

![](img/b0b42c8df91d502b17ce9eb9fb21a241.png)

Jukan Tateisi 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

**GDPR**(2018 年 5 月 25 日生效):(Zerlang，2017 年)确认数据处理的透明度是 GDPR 的核心。跨境金融服务必须将所有对话记录至少 5 年，否则可能会被罚款，例如巴克莱因错误报告 5750 万笔交易而被罚款 245 万英镑。同样，(Gilford，2016)指出，GDPR 可能会因数据安全违规对乐购银行罚款 19 亿英镑。

金融机构必须审查当前的合规流程。客户同意的数据只能用于同意的目的，这意味着任何历史大数据分析示例客户信用卡消费数据都必须销毁。价值数十亿美元的信息将会丢失。因此，金融机构应该重新审视收集数据的过程。

未来的预测建模技术必须是非歧视性的。金融机构必须根据新的 GDPR 准则重新设计它们的算法和自动化流程。

(Lahiri，2017 年)声明 GDPR 将更新欧洲数据保护规则，违反规则将付出高昂代价；2000 万欧元或 4%的营业额。(LeRoux，2017)声称 GDPR 的新法规将成为大数据的杀手。(Woodie，2017 年)证实，GDPR 的新修正案将为欧盟 7.43 亿消费者制定强制性数据处理流程、使用透明度和消费者友好型隐私规则。

**数据隐私**共享机密信息会导致法律和商业风险(Deutsche，2014)。此外，交叉链接异构数据以重新识别个人身份是一个隐私问题(Fhom，2015)。因此，不当使用此类敏感数据可能会引起法律纠纷。

**熟练的劳动力**解读大量的大数据也是一个巨大的挑战，因为错误预测的结果可能代价高昂。因此，训练有素的大数据技术员工至关重要，因为(Fhom，2015)预测将会出现大数据专业人员短缺。

因此，公司应该在实施大数据项目之前计划培训或雇佣新员工。(McAfee 等人，2012 年)还提倡将强大的领导力、良好的公司文化、适应新技术的意愿作为大数据成功的关键因素。

**构建大数据基础设施的成本**短期来看非常昂贵，但长期来看将为金融公司带来竞争优势。根据(Davenport，2013)，花费在大数据上的时间和金钱的合理性 **(ROI)** 并不透明。因此，该公司的高层管理人员将长期战略押在了大数据上。

# 结论

![](img/40f1d36fb5bd793a712bc930498c93ee.png)

由[凯利·西克玛](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

总之，所研究的不同案例显示了正在使用的各种大数据技术和优势，如 Nasdaq & AWS RedShift 案例中的存储减少，以及由此带来的从 6PB 到 500GB 的存储减少。

从这些案例研究中，我们发现接受调查的所有公司都非常重视积极利用大数据来实现自己的使命和目标，并有着共同的目标。它们之间有一些共性，例如更有针对性的营销，也有一些差异，例如存储所采用的不同类型的技术。

通过分析大数据，行业参与者可以提高组织效率，改善客户体验，增加收入，提高利润，更好地预测风险，并可以洞察进入新市场的情况。(尹等，2015)行情大数据提升运营效率 18%。

同样，(McAfee 等人，2012 年)指出，与市场上的竞争对手相比，数据驱动型公司的生产率平均高出 5%至 6%。银行将快速获得更好的数据洞察力，以便做出有效的决策。

# 参考

● AWS 案例研究。可从[https://aws.amazon.com/solutions/case-studies/nasdaq-omx/](https://aws.amazon.com/solutions/case-studies/nasdaq-omx/)获得

●大数据革命。可从[https://www . data Maran . com/the-big-data-revolution-doing-more-for-less/](https://www.datamaran.com/the-big-data-revolution-doing-more-for-less/)获取

● BNY 梅隆大学(2016)，BNY 梅隆大学的潘兴引入了额外的功能，使客户能够将大数据转化为大见解。可从[https://www . Pershing . com/news/press-releases/2016/bny-mellons-Pershing-introduces-additional-capabilities-enabled-clients-to-transform-big-data 获取。](https://www.pershing.com/news/press-releases/2016/bny-mellons-pershing-introduces-additional-capabilities-enabling-clients-to-transform-big-data.)

● Celent (2013 年)。风险管理中的大数据:提供新见解的工具。可从 https://www.celent.com/insights/903043275.[获得](https://www.celent.com/insights/903043275.)

●花旗(2017)。寻找阿尔法:大数据。导航新的备选数据集。可从 https://eaglealpha.com/citi-report/.[获得](https://eaglealpha.com/citi-report/.)

达文波特(2013 年)。大公司的大数据。可从[http://resources . idgenterprise . com/original/AST-0109216 _ Big _ Data _ in _ Big _ companies . pdf 获取](http://resources.idgenterprise.com/original/AST-0109216_Big_Data_in_Big_Companies.pdf.)

●德意志银行(2014 年)。大数据:它如何成为差异化优势。可从[http://CIB . db . com/docs _ new/GTB _ 大数据 _ 白皮书 _(DB0324)_v2.pdf 获得](http://cib.db.com/docs_new/GTB_Big_Data_Whitepaper_(DB0324)_v2.pdf.)

●经济学人信息部(2014 年)。零售银行和大数据:大数据是更好的风险管理的关键。可从[https://www . eiuperspectives . economist . com/sites/default/files/retailbanksandbigdata . pdf 获得](https://www.eiuperspectives.economist.com/sites/default/files/RetailBanksandBigData.pdf.)

●埃夫里(2014 年)。面向营销人员的银行业大数据。可从 https://www . evry . com/global assets/insight/bank 2020/bank-2020-big-data-whitepaper.pdf 获得。

Fhom，H.S. (2015)。大数据:机遇和隐私挑战。可从 http://arxiv.org/abs/1502.00823.[获得](http://arxiv.org/abs/1502.00823.)

福斯特，赵，杨，莱库，陆，2008 年 11 月。云计算和网格计算 360 度对比。2008 年网格计算环境研讨会。GCE’08(第 1-10 页)。IEEE。吉尔福德·g .(2016 年)。GDPR 银行违规罚款 19 亿英镑。可从[https://www . HR solutions-uk . com/1-9bn-fines-gdpr-bank-breach/](https://www.hrsolutions-uk.com/1-9bn-fines-gdpr-bank-breach/)获取

●格林，M. (2017)。纳斯达克:用蓝色数据克服大数据挑战。可从 https://itpeer network . Intel . com/Nasdaq-comprising-big-data-challenges-bluedata/获得。

●凯斯勒，s .，阿默，f .，埃斯皮诺萨，J.A .和钱，w .，2013 年 1 月。大数据:前进中的问题和挑战。在系统科学(HICSS)，2013 年第 46 届夏威夷国际会议(第 995-1004 页)。IEEE。

●卡尔瓦基(2015 年)。土耳其五大银行。可从[http://www . The Banker . com/Banker-Data/Bank-Trends/The-top-five-banks-in-Turkey 获得？ct =真](http://www.thebanker.com/Banker-Data/Bank-Trends/The-top-five-banks-in-Turkey?ct=true)

●诺克斯(2015)。金融服务中的大数据。可从[https://www . slide share . net/swift community/nybf 2015 big-dataworksession final 2 mar 2015](https://www.slideshare.net/SWIFTcommunity/nybf2015big-dataworksessionfinal2mar2015)获取

● Lahiri，K. (2017) GDPR 和英国退出欧盟:离开欧盟如何影响英国的数据隐私。可从[https://www . cbronline . com/verticals/CIO-agenda/gdpr-英国退出欧盟-离开-欧盟-影响-英国-数据-隐私/](https://www.cbronline.com/verticals/cio-agenda/gdpr-brexit-leaving-eu-affects-uk-data-privacy/) 获取

●纽约勒鲁(2017 年)。GDPR:即将出台的法规会扼杀大数据吗？可从[https://www . finance digest . com/gdpr-is-the-about-regulation-killing-off-big-data . html](https://www.financedigest.com/gdpr-is-the-upcoming-regulation-killing-off-big-data.html)获得

Lippert，J. (2014 年)。Zest Finance 发放小额高利率贷款，利用大数据剔除赖账者。可从[https://www . Washington post . com/business/zest finance-issues-small-high-rate-loans-uses-big-data-to-weed-out-dead beats/2014/10/10/e 34986 b 6-4d 71-11e 4-aa5e-7153 e 466 a02d _ story . html？utm_term=.6767c7abfefb.](https://www.washingtonpost.com/business/zestfinance-issues-small-high-rate-loans-uses-big-data-to-weed-out-deadbeats/2014/10/10/e34986b6-4d71-11e4-aa5e-7153e466a02d_story.html?utm_term=.6767c7abfefb.)

刘，杨，张，2013 年 9 月。大数据技术概述。工程和科学互联网计算(ICICSE)，2013 年第七届国际会议(第 26-29 页)。IEEE。

马尔，B. (2016 年)。花旗集团利用大数据改善银行绩效的惊人方式。可从[https://www . LinkedIn . com/pulse/amazing-ways-Citigroup-using-big-data-improve-bank-performance-marr/获得。](https://www.linkedin.com/pulse/amazing-ways-citigroup-using-big-data-improve-bank-performance-marr/.)

● McAfee，a .，Brynjolfsson，e .和 Davenport，T.H .，2012。管理革命。哈佛商业评论，90(10)，第 60-68 页。

普拉曼尼克，S. (2013 年)。大数据用例-银行和金融服务。可从[https://thebigdata institute . WordPress . com/2013/05/01/big-data-use-cases-banking-and-financial-services/](https://thebigdatainstitute.wordpress.com/2013/05/01/big-data-use-cases-banking-and-financial-services/)获取

Preez 博士(2013 年)。德意志银行:遗留系统阻碍了大数据计划。可从[https://www . computer world uk . com/data/Deutsche-bank-big-data-plans-hold-back-by-legacy-systems-3425725/](https://www.computerworlduk.com/data/deutsche-bank-big-data-plans-held-back-by-legacy-systems-3425725/)获得

s .萨吉罗格卢和 d .西纳克，2013 年 5 月。大数据:综述。协作技术与系统(CTS)，2013 年国际会议(第 42-47 页)。IEEE。

● Statista (2016)。大数据。可从 https://www-statista-com . ez proxy . Westminster . AC . uk/study/14634/big-data-statista-filoral/获取。

● Timmes，J，2014，《地震式转变:纳斯达克向亚马逊红移的迁移》。可从[https://www . slide share . net/Amazon web services/fin 401-seismic-shift-nas daqs-migration-to-Amazon-redshift-AWS-reinvent-2014](https://www.slideshare.net/AmazonWebServices/fin401-seismic-shift-nasdaqs-migration-to-amazon-redshift-aws-reinvent-2014)获取

●M . Trombly，2000 年，创纪录的交易量使纳斯达克陷入困境。可从[https://www . computer world . com/article/2593644/it-management/record-volume-stalls-Nasdaq . html](https://www.computerworld.com/article/2593644/it-management/record-volume-snarls-nasdaq.html)获得

●判决，(2017)。GDPR 的监管规定可能会让银行在头三年被处以超过€47 亿英镑的罚款。可从[https://www . verdict . co . uk/what-is-gdpr-regulations-could-cost-banks-over-E4-70 亿-in-fines-first-three-years/](https://www.verdict.co.uk/what-is-gdpr-regulations-could-cost-banks-over-e4-7bn-in-fines-in-first-three-years/)

● Woodie，A. (2017)。GDPR:告别大数据的蛮荒西部。可从[https://www . datanami . com/2017/07/17/gdpr-say-goodbye-big-datas-wild-west/](https://www.datanami.com/2017/07/17/gdpr-say-goodbye-big-datas-wild-west/)获取

●尹，s .和凯纳克，o . 2015。现代工业的大数据:挑战和趋势[观点]。IEEE 会议录，103(2)，第 143-146 页。

●泽朗，J. (2017 年)。大数据和 GDPR；金融中的风险和机遇。可从[https://www . accounting web . co . uk/community/blogs/jesper-zer lang/big-data-and-gdpr-risk-and-opportunity-in-finance](https://www.accountingweb.co.uk/community/blogs/jesper-zerlang/big-data-and-gdpr-risk-and-opportunity-in-finance)获取

*现在，把你的想法放在* ***Twitter*** *，****Linkedin****，以及****Github****！！*

***同意*** *还是* ***不同意*** *与 Saurav Singla 的观点和例子？想告诉我们你的故事吗？*

*他对建设性的反馈持开放态度——如果您对此分析有后续想法，请在下面的* ***评论*** *或联系我们！！*

*推文*[***@ SauravSingla _ 08***](https://twitter.com/SAURAVSINGLA_08)*、评论*[***Saurav _ Singla***](http://www.linkedin.com/in/saurav-singla-5b412320)*，还有明星*[***SauravSingla***](https://github.com/sauravsingla)*马上！*