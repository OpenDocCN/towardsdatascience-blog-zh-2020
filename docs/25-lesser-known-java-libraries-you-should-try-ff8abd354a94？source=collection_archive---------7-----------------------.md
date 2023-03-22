# 2020 年你应该尝试的 25 个鲜为人知的 Java 库

> 原文：<https://towardsdatascience.com/25-lesser-known-java-libraries-you-should-try-ff8abd354a94?source=collection_archive---------7----------------------->

## 对 Java 和 JVM 软件开发有很大帮助的库

![](img/28bd015154177a24e1f0873c8b279a18.png)

图片由 [StartupStockPhotos](https://pixabay.com/users/startupstockphotos-690514/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=593313) 来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=593313)

25 年前，**詹姆斯·高斯林**创造了 Java，永远改变了编程语言的版图。与许多其他编程语言不同，Java 在其整个生命周期中都享有很高的流行度和业界的高需求。

Java 有一个引人注目的核心库，它提供了许多基本功能。由于它的高普及性，存在许多成熟而强大的 Java 库。作为一名务实的软件开发人员，我更喜欢解决业务问题。为了解决常见的或重复出现的问题，我更喜欢使用成熟的库，而不是重新发明轮子。

在之前的一篇博文中，我列出了每个 Java 开发人员都应该知道的 10 个最重要的第三方 Java 库:

[](/top-10-libraries-every-java-developer-should-know-37dd136dff54) [## 每个 Java 开发人员都应该知道的 10 大库

### Java 和 JVM 软件开发中基本 Java 库的精选列表

towardsdatascience.com](/top-10-libraries-every-java-developer-should-know-37dd136dff54) 

在这篇文章中，我想介绍另外 25 个 Java 库来庆祝 Java 的 25 周年。这些库是成熟的，可以为您在 Java 软件开发中可能面临的常见问题提供久经考验的解决方案。

请注意，我不包括那些我在以前的帖子中已经列出的库。另外，我在这里主要列出了库，没有列出框架。

# 1.RxJava

reactive Extensions(react vex)是一种流行的软件开发范例，用于处理异步和事件驱动的编程。RxJava 是使用 Observables 的反应式扩展的 Java VM 实现。它扩展了 Observer 模式，通过以声明的方式在事件/数据序列上添加可组合的操作符来支持事件驱动的编程。它还隐藏了底层的复杂性，如线程、线程安全、同步和并发数据结构。

如果想用 Java 做反应式编程，那是必备库。

## 链接:

[](https://github.com/ReactiveX/RxJava) [## react vex/rx Java

### RxJava 是反应式扩展的 Java VM 实现:一个用于组成异步的和基于事件的…

github.com](https://github.com/ReactiveX/RxJava) 

# 2.OkHttp

HTTP 是目前使用最多的应用层协议。有许多优秀的基于 Java 的 HTTP 客户端库。但是 OkHttp 是 JVM 中最简单但功能强大的 Http 客户端库。它为用 Java 开发 HTTP 客户端提供了流畅、简洁的 API。

它还支持一些高级特性:连接池、GZIP 收缩、响应缓存、现代 TLS 特性等等。

## 链接:

[](https://github.com/square/okhttp) [## square/okhttp

### 有关文档和 API，请参见项目网站。HTTP 是现代应用程序联网的方式。这是我们交换的方式…

github.com](https://github.com/square/okhttp) 

# 3.米巴蒂斯

在大多数软件开发项目中，我们需要存储数据。虽然有许多类型的数据存储，SQL 仍然是最常用的数据存储类型。作为一名 Java 开发人员，我们需要将 Java 对象与 SQL 表相匹配。实现映射的一种方法是使用 ORM(例如 Hibernate)。但是当您想要完全控制对象-表映射(例如，性能)时，有许多用例。在这些情况下，您可以直接使用 JDBC 并编写 SQL 查询。另一种方法是使用 MyBatis 将 Java 对象映射到存储过程或 SQL 语句。它提供了基于注释或基于 XML 描述符的映射。

比起普通的 JDBC，我更喜欢 MyBatis，尤其是在大型项目中，因为它改善了关注点的分离。

## 链接:

[](https://github.com/mybatis/mybatis-3) [## mybatis/mybatis-3

### MyBatis SQL mapper 框架使得在面向对象的应用程序中使用关系数据库变得更加容易…

github.com](https://github.com/mybatis/mybatis-3) 

# 4.HikariCP

HikariCP 是这个列表中第二个与数据库相关的库。建立 JDBC 连接是资源昂贵的。如果每次访问数据库时都创建一个新的连接，并在完成后关闭它，这会严重影响应用程序的性能。更不用说未能正确关闭连接或允许无限制数据库连接会导致应用程序崩溃。

使用连接池意味着在每次请求连接时重用连接，而不是创建连接。HikariCP 是 JVM 中一个非常快速但轻量级的数据库连接池。它也非常可靠，是一个“零开销”的 JDBC 连接池。

## 链接:

[](https://github.com/brettwooldridge/HikariCP) [## brettwood ridge/hikar ICP

### 快速、简单、可靠。HikariCP 是一个“零开销”的生产就绪型 JDBC 连接池。大约 130 kb……

github.com](https://github.com/brettwooldridge/HikariCP) 

# 5.龙目岛

在现代，Java 经常被批评为一种冗长而臃肿的编程语言。与其他流行语言(JavaScript、Python、Scala、Kotlin)相比，开发人员需要用 Java 编写大量样板代码。尽管 Java 在 JDK 15 中引入了记录以减少 Java 中的样板代码，但它不是 LTS 版本。幸运的是，一个库已经可以显著减少 Java 中的样板代码:Project Lombok。通过添加一些注释，可以生成 getters、setters、hashcode、equals、toString、Builder 类。此外，它还提供了空指针检查、记录器等等。

## 链接:

[](https://github.com/rzwitserloot/lombok) [## rzwitserloot/龙目岛

### Project Lombok 是一个 java 库，可以自动插入到您的编辑器和构建工具中，为您的 java 增添趣味。从不…

github.com](https://github.com/rzwitserloot/lombok) 

# 6.VAVR

Java 终于在版本 8 中发布了期待已久的通过 Lambda 和 Streaming 的函数式编程。如果你习惯于函数式编程或者想要深入函数式编程，你可能会发现 Java 的函数式编程有所欠缺。相比其他很多函数式编程语言(Haskell，Scala)，Java 就显得苍白无力了。VAVR 是一个可以填补 Java 中函数式编程特性空白的库。它提供了持久集合、错误处理的功能抽象、并发编程、模式匹配等等。

## 链接:

[](https://www.vavr.io/) [## Vavr

### Oracle 和 Java 是 Oracle 和/或其附属公司的注册商标。其他名称可能是他们的商标…

www.vavr.io](https://www.vavr.io/) 

# 7.Gson

多年来，JSON 已经成为事实上的数据交换格式。在 Java 中，也存在一些优秀的库来处理 JSON。其中一个是杰克逊，我在以前的文章中提到过。另一个优秀的库是谷歌的 Gson。不像 Jackson，它是一个极简主义的库，只支持 JSON。它提供数据绑定、广泛的通用支持、灵活的定制。Gson 的一个主要优点(或者缺点取决于您的喜好)是它不需要注释。

## 链接:

[](https://github.com/google/gson) [## 谷歌/gson

### Gson 是一个 Java 库，可以用来将 Java 对象转换成它们的 JSON 表示。它也可以用来…

github.com](https://github.com/google/gson) 

# 8.jsoup

如果你正在用 Java 开发你的应用程序，并且需要处理 HTML，你应该使用 jsoup。这是一个 Java 库，用于处理现实世界中的 HTML。它为获取 URL、提取和操作数据提供了一个非常方便的 API。它实现了 WHATWG HTML5 规范，并使用最好的 HTML5 DOM 方法解析 HTML。它支持从 URL/字符串解析 HTML，查找和提取数据，操作 HTML 元素，清理 HTML，输出 HTML。

## 链接:

[](https://github.com/jhy/jsoup) [## jhy/j 组

### jsoup 是一个用于处理真实世界 HTML 的 Java 库。它提供了一个非常方便的 API 来获取 URL 和…

github.com](https://github.com/jhy/jsoup) 

# 9.移转

如果您正在开发一个企业级应用程序，它至少应该是云就绪的。使您的应用程序云就绪的第一步是将您的应用程序容器化，即，将您的 artifactory 二进制文件放入 Docker 映像。对 Java 应用程序进行 Docker 化是一件有点繁琐的工作:你需要对 Docker 有深入的了解，你需要创建一个 Dockerfile，你还需要 Docker Daemon。对 Java 开发者来说幸运的是，Google 已经使用现有的工具创建了一个开源的 Java 容器。您可以使用 JIB 作为 Java 库来构建优化的 Docker 和 OCI 映像。

## 环

[](https://github.com/GoogleContainerTools/jib) [## 谷歌集装箱工具/三角帆

### 你最喜欢 Jib 的什么？有哪些需要改进的地方？请通过一分钟的调查告诉我们。你的…

github.com](https://github.com/GoogleContainerTools/jib) 

# 10.（发）丁当声

Tink 是 Google 列表中另一个方便的 Java 库。密码学和安全性在软件开发中变得越来越重要。加密技术用于保护用户数据。正确实现加密需要大量的专业知识和努力。Google 的一组密码学家和安全工程师编写了多语言密码库 Tink。它提供了易于使用但难以误用的安全 API。Tink 通过不同的原语提供加密功能。它提供对称密钥加密、流式对称密钥加密、确定性对称密钥加密、数字签名、混合加密和许多其他加密功能。

## 链接:

[](https://github.com/google/tink) [## 谷歌/tink

### Tink 是一个多语言、跨平台的开源库，它提供了安全、易于使用的加密 API

github.com](https://github.com/google/tink) 

# 11.Webmagic

如果你从事网络爬虫工作，你可以自己编写爬虫，既费时又繁琐。在 Java 中，Webmagic 是一个优秀的网络爬虫库，涵盖了爬虫的整个生命周期:下载、URL 管理、内容提取、持久化。它提供了一个简单而灵活的核心、注释支持、多线程和易于使用的 API。

## 链接:

[](https://github.com/code4craft/webmagic) [## code4craft/webmagic

### 一个可扩展的爬虫框架。它涵盖了爬虫的整个生命周期:下载、url 管理…

github.com](https://github.com/code4craft/webmagic) 

# 12.ANTLR 4

如果您致力于解析和处理数据，那么 ANTLR 库可能会很方便。它是一个强大的解析器生成器，用于读取、处理、执行或翻译结构化文本或二进制文件。它让我们能够访问语言处理原语，如词法分析器、语法分析器和运行时来处理文本。

它通常用于构建工具和框架。

## 链接:

[](https://github.com/antlr/antlr4) [## antlr/antlr4

### ANTLR(另一个语言识别工具)是一个强大的解析器生成器，用于读取、处理、执行或…

github.com](https://github.com/antlr/antlr4) 

# 13.咖啡因

如果您的应用程序需要大量读取，那么缓存可以显著提高应用程序的数据访问性能。Java 有很多很棒的缓存库。咖啡因是其中最好的。这是一个基于 Java 的高性能、接近最优的缓存库。它提供了一个流畅的缓存 API 和一些高级特性，如异步加载条目、异步刷新、弱引用键等。

## 链接:

[](https://github.com/ben-manes/caffeine) [## 苯海拉明/咖啡因

### Caffeine 是一个基于 Java 8 的高性能、近乎最优的缓存库。有关更多详情，请参见我们的用户指南…

github.com](https://github.com/ben-manes/caffeine) 

# 14.韵律学

一旦您的 Java 应用程序运行到生产环境中，您将希望深入了解应用程序的关键组件。Dropwizard 框架中的 Metrics 是一个简单但引人注目的 Java 库，它提供了对您的应用程序和 JVMs KPI 的洞察，例如，事件率、挂起的作业、服务健康检查等等。它是模块化的，为其他库/框架提供模块。

## 链接:

[](https://github.com/dropwizard/metrics) [## 下拉向导/指标

### 📈捕获 JVM 和应用程序级别的指标。所以你知道发生了什么。有关更多信息，请参见…

github.com](https://github.com/dropwizard/metrics) 

# 15.gRPC-Java

Google 在 2015 年创建了 gRPC 作为一个现代远程过程调用系统。从那以后，gRPC 变得非常流行，成为现代软件开发中使用最多的 RPC 系统之一。gRPC-Java 库是 gRPC 客户机的 Java 实现。如果您想在 Java 中使用 gRPC，那么这个库会很方便。

## 链接:

[](https://github.com/grpc/grpc-java) [## grpc/grpc-java

### gRPC-Java 适用于 JDK 7。gRPC-Java 客户端在 Android API 级别 16 及以上(Jelly Bean 及更高版本)上受支持…

github.com](https://github.com/grpc/grpc-java) 

# 16.Java WebSocket

传统的客户端-服务器单向通信。WebSocket 是通过单一 TCP 连接的双向通信协议。Java WebSocket 是一个用 Java 实现的 WebSocket 服务器和客户端的准系统。如果您是 Java 开发人员，并且希望使用 WebSocket，那么强烈推荐这个库。

## 链接:

[](https://github.com/TooTallNate/Java-WebSocket) [## TooTallNate/Java-WebSocket

### 这个存储库包含一个用 100% Java 编写的准系统 WebSocket 服务器和客户端实现。潜在的…

github.com](https://github.com/TooTallNate/Java-WebSocket) 

# 17.JJWT

JSON Web Token (JWT)是现代软件开发中事实上的授权和安全信息交换格式。无论您是使用简单的基于会话的授权还是高度高级的基于 OAuth2 的授权，您都可能会使用 JWT。JJWT 是一个简单的 Java 库，用于在 Java 和 JVM 环境中创建和验证 JWT。它在所有实现的功能上都完全符合 RFC 规范。它支持可读和方便的流畅的 API。

## 链接:

[](https://github.com/jwtk/jjwt) [## jwtk/jjwt

### JJWT 的目标是成为在 JVM 上创建和验证 JSON Web 令牌(JWT)的最容易使用和理解的库…

github.com](https://github.com/jwtk/jjwt) 

# 18.招摇核心

OpenAPI 是机器可读接口文件的规范，用于描述、生成、消费和可视化 RESTful web 服务。Swagger-Core 是 OpenAPI 规范的 Java 实现。如果在 Java/JavaEE 应用程序中公开 REST API，可以使用 Swagger-Core 自动提供和公开 API 定义。

## 链接:

[](https://github.com/swagger-api/swagger-core) [## swagger-API/swagger-核心

### 注意:如果你找的是 Swagger Core 1.5.X 和 OpenAPI 2.0，请参考 1.5 分支。Swagger Core 是一个 Java…

github.com](https://github.com/swagger-api/swagger-core) 

# 19.异步 Http 客户端

异步编程最近变得越来越流行，因为它具有非阻塞的特性。大多数流行的 Java HTTP 客户端库只提供有限的异步 HTTP 响应处理。异步 HTTP 客户端是一个流行的 Java 库，它提供异步 Http 响应处理。作为一个额外的特性，这个库还支持 WebSocket 协议。

## 链接:

[](https://github.com/AsyncHttpClient/async-http-client) [## 异步客户端/异步 http 客户端

### 在 Twitter 上关注@AsyncHttpClient。异步客户端(AHC)库允许 Java 应用程序轻松执行 HTTP…

github.com](https://github.com/AsyncHttpClient/async-http-client) 

# 20.液态碱

作为软件开发人员，我们都知道版本控制、DevOps 和代码的 CI/CD 的重要性。在一篇博客文章中:[进化数据库设计](https://martinfowler.com/articles/evodb.html)，伟大的**马丁·福勒**认为我们也需要版本控制和代码的 CI/CD。Liquibase 是一个工具，支持 Java 应用程序中 SQL 数据库变更的跟踪、版本控制和部署。如果您正在使用一个数据库不断发展的 SQL 数据库，这个工具可以极大地简化您的数据库迁移。

## 链接:

[](https://www.liquibase.org/) [## Liquibase |数据库的开源版本控制

### 灵活数据库更改很容易用 SQL、XML、JSON 或 YAML 定义更改。您的数据库订单的版本控制…

www.liquibase.org](https://www.liquibase.org/) 

# 21.Springfox

我已经在这个列表中列出了 Swagger-Core，它可以为 vanilla Java 或 Java EE 应用程序自动生成 REST API 文档。在企业应用开发中，Spring MVC 已经超越 Java EE 成为头号应用开发平台。在基于 Spring 的 Java 应用中，Springfox 库可以从源代码自动生成 REST API 文档。

## 链接:

[](https://github.com/springfox/springfox) [## 弹簧狐狸/弹簧狐狸

### 关于这个项目的更多信息，请访问 Springfox 网站或…

github.com](https://github.com/springfox/springfox) 

# 22.JavaCV

**OpenCV** 是一个计算机视觉和机器学习软件库。它是开源的，旨在为计算机视觉应用程序提供一个通用的基础设施。JavaCV 是 OpenCV 和计算机视觉领域许多其他流行库(FFmpeg，libdc 1394，PGR FlyCapture)的包装器。JavaCV 还配备了硬件加速全屏图像显示、在多个内核上并行执行代码的易用方法、用户友好的几何和相机及投影仪的颜色校准、特征点的检测和匹配以及许多其他功能。

## 链接:

[](https://github.com/bytedeco/javacv) [## bytedeco/javacv

### JavaCV 使用来自计算机视觉领域研究人员常用库的 JavaCPP 预置的包装器…

github.com](https://github.com/bytedeco/javacv) 

# 23.Joda 时间

在 Java8 之前的核心库中，Java 的日期和时间功能很差。Java8 在其 **java.time** 包中发布了急需的高级日期和时间功能。如果您使用的是旧版本的 Java(Java 8 之前)，Joda time 可以为您提供高级的日期和时间功能。但是，如果您正在使用较新版本的 Java，您可能不需要这个库。

## 链接:

[](https://github.com/JodaOrg/joda-time) [## JodaOrg/joda-time

### Joda-Time 为 Java 日期和时间类提供了高质量的替代品。该设计允许多个日历…

github.com](https://github.com/JodaOrg/joda-time) 

# 24.Wiremock

HTTP 是现代应用程序开发中最受欢迎的传输协议，而 REST 是基于微服务的应用程序开发中事实上的通信协议。在编写单元测试期间，最好关注 SUT(被测系统)并模拟 SUT 中使用的服务。Wiremock 是 REST API 的模拟器，使开发人员能够针对不存在或不完整的 API 编写代码。在基于微服务的软件开发中，Wiremock 可以显著提高开发速度。

## 链接:

[](https://github.com/tomakehurst/wiremock) [## tomakehurst/wiremock

### HTTP 响应存根、URL 匹配、标题和正文内容模式请求验证在单元测试中运行，因为…

github.com](https://github.com/tomakehurst/wiremock) 

# 25.MapStruct

在 Java 应用程序开发中，您经常需要将一种类型的 POJO 转换成另一种类型的 POJO。实现这种 POJO 或 Bean 转换的一种方法是显式地对转换进行编码，这很繁琐。聪明的方法是使用专门开发的库来转换 POJO/Bean。MapStruct 是一个代码生成器，它根据配置方法的约定实现 POJO/Bean 之间的映射。生成的映射代码使用简单的方法调用，因此快速、类型安全且易于理解。

## 链接:

[](https://github.com/mapstruct/mapstruct) [## 地图结构/地图结构

### MapStruct 是一个 Java 注释处理器，用于为 Java bean 类生成类型安全和高性能的映射器…

github.com](https://github.com/mapstruct/mapstruct) 

# 结论

在本文中，我列出了 25 个 Java 库，通过利用测试库的常见任务，它们可以帮助您的软件开发工作。这些库不是特定领域的，无论您是为商业应用程序、机器人、Android 应用程序还是个人项目开发软件，它们都可以帮助您。请注意，对于像 Java 这样大而广的生态系统，这个列表并不是结论性的。有许多优秀的 Java 库我没有在这里列出，但是值得一试。但是这个库列表可以提供一个快速窥视 Java 生态系统世界的窗口。

# 类似文章:

[](https://md-kamaruzzaman.medium.com/coding-languages-for-fintech-how-will-jvm-make-you-succeed-89f84af22296) [## 金融科技的编码语言:JVM 如何让你成功？

### Java，Kotlin，Scala，Groovy，Clojure

md-kamaruzzaman.medium.com](https://md-kamaruzzaman.medium.com/coding-languages-for-fintech-how-will-jvm-make-you-succeed-89f84af22296) [](/top-10-libraries-every-java-developer-should-know-37dd136dff54) [## 每个 Java 开发人员都应该知道的 10 大库

### Java 和 JVM 软件开发中基本 Java 库的精选列表

towardsdatascience.com](/top-10-libraries-every-java-developer-should-know-37dd136dff54) [](/10-excellent-github-repositories-for-every-java-developer-41084a91ade9) [## 10 个优秀的 GitHub 库，适合每一个 Java 开发者

### 面向 Java 开发人员的基本 GitHub 库的精选列表

towardsdatascience.com](/10-excellent-github-repositories-for-every-java-developer-41084a91ade9)