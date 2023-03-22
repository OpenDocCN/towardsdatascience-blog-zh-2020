# 每个 Java 开发人员都应该知道的 10 大库

> 原文：<https://towardsdatascience.com/top-10-libraries-every-java-developer-should-know-37dd136dff54?source=collection_archive---------1----------------------->

## Java 和 JVM 软件开发中基本 Java 库的精选列表

![](img/fa94d6194dea564e808d49fd1a5f0df2.png)

照片由[安民](https://www.pexels.com/@minan1398?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从[派克斯](https://www.pexels.com/photo/black-and-white-cat-sitting-beside-clear-glass-beverage-dispenser-table-lamp-and-books-on-brown-wooden-table-1441597/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

Java 是商业应用程序开发领域的头号编程语言。它也是顶级编程语言之一。

Java 的一个关键特性是它有一个功能丰富且庞大的核心库。虽然标准 Java 库功能强大，但在专业软件开发中，您还需要其他 Java 库。经过 25 年的积极开发以及业界和社区的采纳，Java 有了许多成熟而有用的库。

这里我列出了在所有领域的 Java 应用程序中使用的前 10 个 Java 库。无论您是为业余爱好项目还是企业级项目开发软件，您可能都需要我在下面列出的大多数库。

# Apache Commons

Apache Commons 就像 Java 软件开发中的一把瑞士刀，它扩展了许多 Java 核心库。如果您想在自己的项目中编写一个实用程序类，那么很有可能已经存在一个成熟而强大的 Apache Commons 库。Apache Commons 由 43 个模块化库组成，涵盖了集合、数学、类、数据库、缓存、I/O 实用程序等领域。

它被广泛应用于业界和几乎非官方的 Java 标准库增强。如果您正在进行一个大项目，并且没有使用任何 Apache Commons 库，那么您可能正在重新发明轮子。

## 主要特点:

*   Java 集合框架扩展。
*   数学和统计部分。
*   JDBC 助手。
*   Java 类。
*   I/O 实用程序。
*   日志记录实用程序。

## 链接:

 [## Apache Commons — Apache Commons

### Apache Commons 是一个 Apache 项目，专注于可重用 Java 组件的所有方面。Apache Commons 项目是…

commons.apache.org](https://commons.apache.org/) 

# 谷歌番石榴

Google Guava 是另一个顶级的通用 Java 库。最初由 Google 开发，由著名软件工程师和作家 Joshua Bloch 设计。它现在是一个开源项目，谷歌以外的许多工程师都为此做出了贡献。像 Apache Commons 一样，它也是模块化的，包含许多独立的库。

它涵盖了基本的实用程序、集合、字符串操作、并发实用程序、图形库、I/O 实用程序、哈希等等。谷歌番石榴比阿帕奇公共图书馆有更好的软件设计。如果你觉得有必要创建一个共享库或实用类，那么先看看谷歌番石榴库。

## 主要特点:

*   Java 集合框架扩展。
*   I/O 实用程序。
*   并发实用程序。
*   字符串实用程序。
*   缓存。
*   哈希。

## 链接:

[](https://github.com/google/guava) [## 谷歌/番石榴

### Guava 是来自 Google 的一组核心 Java 库，包括新的集合类型(如 multimap 和 multiset)…

github.com](https://github.com/google/guava) 

# 杰克逊

在软件开发中，你必须处理不同格式的数据。要么你必须以不同的格式加载或保存数据，要么你必须以不同的格式传输数据。JSON 是现代软件开发中事实上的数据交换格式。其他常见的数据格式有 Avro、XML、YAML、Protobuf、CSV、BSON、CBR。

Jackson 是一套用于 Java 的数据处理库。Jackson JSON 是事实上的流 JSON 解析器/生成器库。它还支持其他数据格式，如 Avro、BSON、CBOR、CSV、Smile、Protobuf、XML 或 YAML，以及数据类型，如 Guava、Joda、PCollections 等等。

Jackson 还提供了数据绑定和注释。您可以将 POJO 转换为数据，或者借助 Jackson 注释从数据生成 POJO。如果您处理数据格式，Jackson 是一个必备的工具集。它是高度模块化的，具有提供基本功能的核心模块和各种扩展模块。

## 主要特点:

*   流、注释、数据绑定的核心模块，并支持 JSON 数据格式。
*   针对 Avro、CBOR、CSV、Ion、Protobuf、Smile、XML、YAML 等数据类型的特定模块。
*   科特林土著类型。
*   JSON 模式。
*   标准和集合数据类型。
*   支持第三方数据类型(Yandex Bolts、GeoJSON、Lombok、MongoDB 等等)。

## 链接:

[](https://github.com/FasterXML/jackson) [## FasterXML/jackson

### 这是 Jackson 项目的主页，以前被称为 Java 标准 JSON 库(或 JVM 平台……

github.com](https://github.com/FasterXML/jackson) 

# JAXB

正如上一节所讨论的，XML 是另一种流行的数据格式，它提供了更严格的数据验证、存储和传输。直到 Java 8，Java 标准库才有 XML 支持，包括数据绑定。从 Java 9 开始，XML 处理功能不再是标准 Java 库的一部分，而是转移到了一个单独的库 JAXB 中。

JAXB 提供了用 Java 处理 XML 所需的一切。它为 XML 和 Java 代码之间的映射提供了一种标准而有效的方式。它还包括基于注释的数据绑定。

## 主要特点:

*   支持所有 W3C XML 模式特性。
*   基于注释的 Java 到 XML 数据绑定。
*   验证。

## 链接:

 [## JAXB

### 用于 XML 绑定的 Java 架构(JAXB)提供了一个 API 和工具，可以自动化 XML 文档之间的映射…

javaee.github.io](https://javaee.github.io/jaxb-v2/) 

# SLF4J

日志记录是生产级软件开发中不可或缺的一部分。仔细的日志记录将有助于您理解软件的工作，并找到错误的根本原因，尤其是在生产系统中。Java 标准库在 **java.util.Logging** 中提供了基本的日志记录。还有其他日志库，如 Log4j、Log4j 2、Logback，它们提供了高级的 Java 日志功能。虽然这些日志库提供了具体的实现，但 SLF4J 为各种日志库提供了抽象或门面。它允许用户在部署期间更改所需的日志库。

起初，使用额外的 facade 库 SLF4J 进行日志记录听起来可能会适得其反。但是使用 SLF4J 会给你额外的灵活性，如果需要的话，可以毫不费力地修改具体的日志库。使用您首选的日志记录框架作为 SLF4J 的可插拔日志记录程序总是一个好主意。

**主要特点:**

*   提供底层日志框架的抽象。
*   日志框架可以在运行时更改。
*   支持所有主要的日志框架。
*   提供了一个包含有用工具和特性的库(slf4j-ext.jar)。
*   记录事件的事件记录器。

## 链接:

 [## SLF4J

### Java 的简单日志门面(SLF4J)作为各种日志框架的简单门面或抽象…

www.slf4j.org](http://www.slf4j.org/) 

# Log4j 2

Java 中有很多优秀的日志库:java.util.logging，Log4j，Log4j 2，Logback。其中，Log4j 2 和 Logback 是两个最强大的日志库。与 Logback 相比，我更喜欢 Log4j 2，尤其是对于大型项目，因为它提供了更好的性能。对于大型项目，日志库的性能至关重要，尤其是异步日志、峰值吞吐量和延迟。就这些标准而言，Log4j 2 比 Logback 略胜一筹，如下所述:

 [## Log4j —性能

### 除了功能需求之外，选择日志库的一个重要原因通常是它能很好地满足…

logging.apache.org](https://logging.apache.org/log4j/2.x/performance.html) 

**主要特点:**

*   通过异步日志记录提高性能。
*   将 API 与实现分开。
*   高级过滤。
*   插件架构。
*   云支持。

## 链接:

 [## Apache Log4j 2

### Apache Log4j 2 是 Log4j 的升级版，对其前身 Log4j 1.x 进行了重大改进，并且…

logging.apache.org](https://logging.apache.org/log4j/2.x/) 

# 莫奇托

单元/集成测试是软件开发过程中不可或缺的一部分。通常您想要测试一个单独的类(SUT)，但是它依赖于其他重量级的类或者外部功能(例如，数据库操作，I/O 操作)。在这种情况下，编写单元/集成测试的一种方法是模仿。您可以模仿其他外部服务调用的行为，只关注您想要测试的类。

Mockito 是 Java 中使用最广泛的模仿库。无论您是在测试一个小项目，还是一个庞大、复杂的企业 Java 项目，您都可以在任何地方使用 Mockito。它提供了一个非常简单、干净的 API，让你的单元/集成测试保持干净。

**主要特点:**

*   精益清洁的 API。
*   提供简化的存根模型。
*   经由间谍**的局部嘲讽。**
*   基于注释的模拟/间谍注入。
*   使用 BDDMockito 的行为驱动开发语法。

## 链接:

[](https://site.mockito.org) [## Mockito 框架站点

### Szczepan Faber and friends 为您提供摩奇托。第一批在生产中使用 Mockito 的工程师是…

site.mockito.orgHa](https://site.mockito.org) 

# AssertJ

AssertJ 是我列表中第二个与 TDD 相关的库。测试的主要特征之一是验证测试结果是否与预期结果相匹配。JUnit 在 org.junit.Assert 类中有一个内置的断言机制。对于专业开发人员来说，这两种方法是不够的。

幸运的是，Java 环境中存在两个强大的断言库: **Hamcrest** matchers 和 **AssertJ** 断言。相比 Hamcrest，我更喜欢 AssertJ，因为它的 API 很流畅。它也是高度模块化的，在其核心模块中提供必要的功能，在其他模块中提供一些高级功能。

**主要功能:**

*   流畅的断言 API 提供更好的代码可读性。
*   丰富的断言集和有用的错误消息。
*   标准 Java 库的核心模块。
*   在流行的 Java 库中提供断言的模块，例如 Guava、Joda、Neo4j。
*   模块为 SQL 数据库提供断言。

**链接:**

[](https://assertj.github.io/doc/) [## AssertJ——流畅断言 java 库

### 所有资产的入口点方法和实用程序方法(例如入口)导入静态…

assertj.github.io](https://assertj.github.io/doc/) 

# 冬眠

在我们作为软件工程师的日常生活中，我们必须使用数据存储。在现代，有许多类型的数据存储:SQL 和无数的 NoSQL 数据存储。处理数据存储的一种方式是使用低级 API(例如，SQL 的 JDBC)。这种方法的缺点是不可移植。因此，处理数据存储的最佳方式是在应用程序和数据存储之间引入一个抽象层。这个抽象层(ORM)将 Java 类与数据库表/集合进行映射。Hibernate 是所有编程语言中最早的 ORM 库之一，并启发了业界许多类似的技术。

尽管 Hibernate 主要以 SQL 数据库的 ORM 功能而闻名，但它也扩展到了 NoSQL 数据库。Hibernate 也是模块化的，它提供了一个核心模块和许多基于功能的模块。

**主要特点:**

*   关系数据库(ORM)的域模型持久性。
*   NoSQL 数据存储(OGM)的域模型持久性。
*   领域模型的基于注释的验证。
*   领域模型的全文搜索。

## 链接:

 [## 冬眠

### 编辑描述

hibernate.org](https://hibernate.org/) 

# Apache HTTPComponents

HTTP 是迄今为止使用最多、最流行的应用层协议。Java 标准库没有提供太多处理 HTTP 的功能。幸运的是，Apache HTTPComponents 提供了一套专注于 HTTP 和相关协议的 Java 组件工具。Apache HTTPComponents 也是高度模块化的，它提供了一个核心模块来开发定制的客户机/服务器 HTTP 服务，占用空间很小。它还为异步 HTTP 客户端等高级功能提供了增值模块。

**主要特点:**

*   用于客户机/服务器服务的低级 HTTP 传输组件。
*   提供阻塞和非阻塞 I/O 型号。
*   用于客户端身份验证、状态管理和连接管理的同步 HTTP 客户端。
*   异步 HTTP 客户端处理大量并发连接。

## 链接:

 [## Apache http components—Apache http components

### Apache HttpComponents 项目负责创建和维护低级 Java 组件的工具集…

hc.apache.org](https://hc.apache.org/) 

# 类似文章:

[](https://md-kamaruzzaman.medium.com/coding-languages-for-fintech-how-will-jvm-make-you-succeed-89f84af22296) [## 金融科技的编码语言:JVM 如何让你成功？

### Java，Kotlin，Scala，Groovy，Clojure

md-kamaruzzaman.medium.com](https://md-kamaruzzaman.medium.com/coding-languages-for-fintech-how-will-jvm-make-you-succeed-89f84af22296) [](/10-excellent-github-repositories-for-every-java-developer-41084a91ade9) [## 10 个优秀的 GitHub 库，适合每一个 Java 开发者

### 面向 Java 开发人员的基本 GitHub 库的精选列表

towardsdatascience.com](/10-excellent-github-repositories-for-every-java-developer-41084a91ade9) [](/25-lesser-known-java-libraries-you-should-try-ff8abd354a94) [## 您应该尝试的 25 个鲜为人知的 Java 库

### 对 Java 和 JVM 软件开发有很大帮助的库

towardsdatascience.com](/25-lesser-known-java-libraries-you-should-try-ff8abd354a94)