# 从结构化来源构建知识图

> 原文：<https://towardsdatascience.com/building-knowledge-graphs-from-structured-sources-346c56c9d40e?source=collection_archive---------19----------------------->

## [KGs 洞察](https://towardsdatascience.com/tagged/kgs-insights)

## 关于集成异构数据所采用的映射方法的说明

![](img/b321aa9ab8b51fa43aabf811892b4a01.png)

构图——原图来自 [Pixabay](https://pixabay.com/it/photos/archivio-cassette-documenti-1850170/)

K 知识图(KGs)被标记为有向多重图，以与特定领域或组织相关的实体和关系的形式编码信息。kg 是捕获和组织大量结构化和多关系数据的有效工具，这些数据可以使用查询机制进行研究。考虑到这些特性，KGs 正在成为不同研究领域和工业应用中的 Web 和遗留信息系统的主干。关于 KG 特性的更多细节，包括 RDF、本体和 SPARQL，可以在下面的文章中找到:

[](/knowledge-graphs-at-a-glance-c9119130a9f0) [## 知识图表一览

### 利用语义图视角，将人类知识融入智能系统

towardsdatascience.com](/knowledge-graphs-at-a-glance-c9119130a9f0) 

将数据发布到 KGs 是一个复杂的过程，因为它需要提取和整合来自不同来源的信息。整合这些来源的目标是协调它们的数据，并导致对整体信息的一致看法。异构数据源包括从非结构化数据(如纯文本)到结构化数据(包括表格式，如 CSV 和关系数据库)以及树结构格式(如 JSONs 和 XML)。本文的贡献是通过基于映射的方法将结构化数据源中的数据发布到 KGs 中。

## 结构化来源的整合

结构化数据源在数据生态系统中扮演着重要的角色，因为组织和网络中的许多有价值(且可重用)的信息都是结构化数据。

不像其他来源，比如纯文本，结构化信息可以通过一个语义集成过程映射到 kg。语义网(SW)社区应用这一过程的共同策略是采用参考本体作为全局模式。然后，构建映射来描述目标数据源的全局模式和局部模式之间的关系。从数据集成的角度来看，这种方法被归类为全局视图(GAV)。

从这个角度来看，数据集成性能基于作为全局模式的本体的一致性和表达能力。为了阐明真实场景中的映射过程，下一节将介绍公共采购领域中的一个运行示例。

## 公共采购示例:数据源和参考本体

公共采购是指公共机构和行政部门从公司购买货物或服务的过程。在这一过程中，公共当局宣布招标，公司带着具体的标书参加招标，公共当局将授予其中一份标书。

假设我们有一个目标数据源 *ds* ，包括一组属性 *ds{a* ₁ *，a* ₂ *，a* ₃ *，…}* 。在公共采购中，目标数据源是表示特定公共合同的 JSON 文件。JSON (JavaScript Object Notation)是一种独立于语言的数据交换格式。它使用人类可读的文本来存储和传输以属性-值对和任何可序列化的数据类型(包括数组)为特征的数据对象。

```
{
   "contract_id":"Z4ADEA9DE4",
   "contract_object":"MANUTENZIONE ORDINARIA MEZZI DI TRASPORTO",
   "proposing_struct":{
      "business_id":"80004990927",
      "business_name":"Ministero dell'Interno"
   },
   "participants":[
      {
         "business_id":"08106710158",
         "business_name":"CAR WASH CARALIS"
      }
   ]
}
```

该 JSON 描述了以下数据:

*   *contract_id* 包括所需服务的标识符(“Z4ADEA9DE4”)。
*   *合同对象*包括服务的描述(“MANUTENZIONE ORDINARIA MEZZI DI TRASPORTO”)。
*   *business_id* 和 *business_name* ，嵌套在 *proposing_struct* 字段*，*包括提出招标的公共机构的标识符(“80004990927”)和名称(“Ministero dell'Interno”)。
*   *business_id* 和 *business_name* ，嵌套在*参与者*字段中，包括标识符(“Z4ADEA9DE4”)和名称(“洗车卡利斯”)。

映射过程需要参考本体*或*作为全局模式。捷克 OpenData.cz 倡议发布了公共采购中采用的最常见的本体之一，可在 GitHub 上获得:[https://github.com/opendatacz/public-contracts-ontology](https://github.com/opendatacz/public-contracts-ontology)。

与我们的运行示例相关的映射过程中涉及的本体公理可以用 Turtle 格式表示如下:

```
@prefix dcterms: <[http://purl.org/dc/terms/](http://purl.org/dc/terms/)> .
[@prefix](http://twitter.com/prefix) gr: <[http://purl.org/goodrelations/v1#](http://purl.org/goodrelations/v1#)> .
[@prefix](http://twitter.com/prefix) owl: <[http://www.w3.org/2002/07/owl#](http://www.w3.org/2002/07/owl#)> .
[@prefix](http://twitter.com/prefix) pc: <[http://purl.org/procurement/public-contracts#](http://purl.org/procurement/public-contracts#)> .
[@prefix](http://twitter.com/prefix) rdf: <[http://www.w3.org/1999/02/22-rdf-syntax-ns#](http://www.w3.org/1999/02/22-rdf-syntax-ns#)> .
[@prefix](http://twitter.com/prefix) rdfs: <[http://www.w3.org/2000/01/rdf-schema#](http://www.w3.org/2000/01/rdf-schema#)> .####### 
####### Classes
#######pc:Contract a owl:Class .
gr:Offering a owl:Class .
pc:Tender a owl:Class ;
    rdfs:subClassOf gr:Offering .####### 
####### Relations (or Object Properties)
#######pc:contractingAuthority a owl:FunctionalProperty, owl:ObjectProperty ;
    rdfs:domain pc:Contract ;
    rdfs:range gr:BusinessEntity .pc:tender a owl:ObjectProperty ;
    rdfs:domain pc:Contract ;
    rdfs:range pc:Tender .pc:awardedTender a owl:FunctionalProperty, owl:ObjectProperty ;
    rdfs:subPropertyOf pc:tender .pc:bidder a owl:ObjectProperty ;
    rdfs:domain pc:Tender ;
    rdfs:range gr:BusinessEntity .####### 
####### Datatype properties
#######dcterms:identifier a owl:DatatypeProperty ;
    rdfs:domain pc:Contract ;
    rdfs:domain gr:BusinessEntity ;
    rdfs:range rdfs:Literal .rdfs:label a owl:DatatypeProperty ;
    rdfs:domain pc:Contract ;
    rdfs:domain gr:BusinessEntity ;
    rdfs:range rdfs:Literal .rdfs:description a owl:DatatypeProperty ;
    rdfs:domain pc:Contract ;
    rdfs:range rdfs:Literal .
```

## 映射过程的详细信息

整个映射过程包括两个主要步骤。第一步是在目标数据源的本地模式和参考本体之间创建一个映射。第二步是将源数据具体化为 KG 语句或虚拟化对源的访问，定义遗留信息的基于图形的视图。物化的语句可以直接发布到 KG 中，而基于图形和虚拟化的访问允许我们检索和浏览目标数据源的数据，就像它是 KG 一样。

映射步骤最广泛采用的方法是基于所谓的*定制映射*。这些方法利用用声明性语言编写的可定制文档来执行地图生成步骤。声明性语言利用 SW 形式来描述局部模式和全局模式之间的关系。

研究团体采用的最突出的语言是 R2RML，它表示用 RDF 编写的关系数据库到 kg 之间的定制映射。这种语言的扩展，称为 RML，是一种更通用的映射语言，其适用性扩展到其他类型的表，如 CSV 文件和树结构模式。其他类型的语言，如 TARQL 和 JARQL，分别采用 SPARQL 语法为特定格式(如 CSV 和 JSON 文件)创建映射。描述 *ds* 和 *O* 之间映射的 JARQL 示例如下:

```
[@prefix](http://twitter.com/prefix) dcterms: <[http://purl.org/dc/terms/](http://purl.org/dc/terms/)> .
[@prefix](http://twitter.com/prefix) pc: <[http://purl.org/procurement/public-contracts#](http://purl.org/procurement/public-contracts#)> .
[@prefix](http://twitter.com/prefix) gr: <[http://purl.org/goodrelations/v1#](http://purl.org/goodrelations/v1#)> .
[@prefix](http://twitter.com/prefix) rdf: <[http://www.w3.org/1999/02/22-rdf-syntax-ns#](http://www.w3.org/1999/02/22-rdf-syntax-ns#)> .CONSTRUCT {
    ?BusinessEntity0 dcterms:identifier ?proposing_struct__business_id ;
        rdf:type gr:BusinessEntity. ?BusinessEntity1 dcterms:identifier ?participants__business_id ;
        rdf:type gr:BusinessEntity . ?Tender0 pc:bidder ?BusinessEntity1 . ?Contract0 dcterms:identifier ?contract_id ;
        rdf:type pc:Contract ;
        pc:contractingAuthority ?BusinessEntity0 ;
        pc:tender ?Tender0 .
}WHERE {
    ?root a jarql:Root.
    OPTIONAL { ?root jarql:contract_id ?contract_id . }
    OPTIONAL { ?root jarql:proposing_struct ?proposing_struct . }
    OPTIONAL { ?proposing_struct jarql:proposing_struct__business_id
    ?proposing_struct__business_id . }
    OPTIONAL { ?root jarql:participants ?participants . }
    OPTIONAL { ?participants jarql:participants__business_id
    ?participants__business_id . }
    BIND (URI(CONCAT('[http://purl.org/procurement/public-contracts/contract/'](http://purl.org/procurement/public-contracts/contract/'),
    ?contract_id)) as ?Contract0)
    BIND (URI(CONCAT('[http://purl.org/goodrelations/v1/businessentity/'](http://purl.org/goodrelations/v1/businessentity/'),
    ?proposing_struct__business_id)) as ?BusinessEntity0)
    BIND (URI(CONCAT('[http://purl.org/goodrelations/v1/businessentity/'](http://purl.org/goodrelations/v1/businessentity/'),
    ?participants__business_id)) as ?BusinessEntity1)
    BIND (URI(CONCAT('[http://purl.org/procurement/public-contracts/tender/'](http://purl.org/procurement/public-contracts/tender/')
    ?contract_id + + '_' participants__business_id)) as ?Tender0)
}
```

这个 JARQL 文件包括 3 个主要部分。第一个包含在 CONSTRUCT 部分，而其他的包含在 WHERE 部分。

CONSTRUCT 部分描述了对*语义类型*进行编码的图形模式，例如

```
?BusinessEntity0 dcterms:identifier ?proposing_struct__business_id .
```

和*语义关系*，如

```
?Contract0 pc:contractingAuthority ?BusinessEntity0 .
```

WHERE 部分的第一部分包括可选的操作符，描述了如何解析 JSON 文件以提取创建 KG 语句所需的数据。例如:模式

```
?proposing_struct jarql:proposing_struct__business_id ?proposing_struct__business_id .
```

表明变量。proposing_struct__business_id 必须替换为 JSON 的 proposing_struct__business_id 属性。

WHERE 部分的第二段包括不同的绑定操作符，声明如何为从 JSON 中提取的数据生成实体 URIs。这条线

```
BIND (URI(CONCAT(‘[http://purl.org/procurement/public-contracts/contract/'](http://purl.org/procurement/public-contracts/contract/'), ?contract\_id)) as ?Contract0) .
```

指示合同的 URIs 是结合 http://purl.org/procurement/public-contracts/contract/和从绑定到？合同 id 变量。

语言驱动的引擎旨在按照使用声明性语言编写的映射文档的指令，物化或虚拟化 KG 语句。这些引擎执行两个不同的任务:第一个任务是将目标数据源字段链接到由引用本体定义的类或属性。一旦创建了这个链接，这些引擎就会具体化或虚拟化 URIs 和 KG 语句，检索数据源中包含的遗留信息。JARQL 文件描述了属性集 *ds{a* ₁ *，a* ₂ *，a* ₃ *，…}* 和由 o 表示的全局模式之间的链接，允许具体化以下语句:

```
[@prefix](http://twitter.com/prefix) dcterms: <[http://purl.org/dc/terms/](http://purl.org/dc/terms/)> .
[@prefix](http://twitter.com/prefix) pc: <[http://purl.org/procurement/public-contracts#](http://purl.org/procurement/public-contracts#)> .
[@prefix](http://twitter.com/prefix) gr: <[http://purl.org/goodrelations/v1#](http://purl.org/goodrelations/v1#)> .
[@prefix](http://twitter.com/prefix) rdf: <[http://www.w3.org/1999/02/22-rdf-syntax-ns#](http://www.w3.org/1999/02/22-rdf-syntax-ns#)> .
[@prefix](http://twitter.com/prefix) contract: <[http://purl.org/procurement/public-contracts/contract/](http://purl.org/procurement/public-contracts/contract/)> .
[@prefix](http://twitter.com/prefix) be: <[http://purl.org/goodrelations/v1/businessentity/](http://purl.org/goodrelations/v1/businessentity/)> .
[@prefix](http://twitter.com/prefix) tender: <[http://purl.org/procurement/public-contracts/tender/](http://purl.org/procurement/public-contracts/tender/)> .# ?BusinessEntity0 dcterms:identifier ?proposing_struct__business_id ;
#      rdf:type gr:BusinessEntity .be:08106710158 dcterms:identifier 08106710158 ;
    rdf:type gr:BusinessEntity .# ?BusinessEntity1 dcterms:identifier ?participants__business_id ;
#        rdf:type gr:BusinessEntity .be:08106710158 dcterms:identifier 08106710158 ;
    rdf:type gr:BusinessEntity .# ?Tender0 pc:bidder ?BusinessEntity1 .tender:Z4ADEA9DE4-80004990927 pc:bidder be:08106710158 .# ?Contract0 dcterms:identifier ?contract_id ;
#     rdf:type pc:Contract ;
#     pc:contractingAuthority ?BusinessEntity0 ;
#     pc:tender ?Tender0 .contract:Z4ADEA9DE4 dcterms:identifier Z4ADEA9DE4 ;
    rdf:type pc:Contract ;
    pc:contractingAuthority 80004990927 ;
    pc:tender tender:Z4ADEA9DE4-80004990927 .
```

为了阐明 JARQL 文件支持的转换过程，本例将包含在其 CONSTRUCT 部分的图形模式作为注释进行报告。使用 JARQL 作为声明性语言的主要原因之一是它能够创建 URIs，组合位于 JSON 树结构不同层次的数据。在运行的示例中，投标人的 URI 是结合位于 JSON 根级别的合同 id(“z 4 ade a9 de 4”)和位于 JSON 嵌套结构中的业务实体 id(“80004990927”)构建的。

## 下一步是什么

在下一篇文章中，我将向您展示一种基于语义模型的特殊方法，用于捕获具有基于图的结构的数据源的语义。

[](/semantic-models-for-constructing-knowledge-graphs-38c0a1df316a) [## 构建知识图的语义模型

### 用基于图的结构捕获数据源的语义

towardsdatascience.com](/semantic-models-for-constructing-knowledge-graphs-38c0a1df316a) 

*如果你喜欢我的文章，你可以支持我使用这个链接*[](https://medium.com/@giuseppefutia/membership)**成为中等会员。**