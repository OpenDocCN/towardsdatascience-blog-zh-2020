# 卡夫卡，为了你的数据管道？为什么不呢？

> 原文：<https://towardsdatascience.com/kafka-for-your-data-pipeline-why-not-5a14b50efe7f?source=collection_archive---------13----------------------->

## 使用 Docker、Kafka 和 Kafka Connect 创建流管道

![](img/ae06133ce6291df41ce063d1cf002e93.png)

我们在这个项目中所建造的

Kafka 于 2011 年由 LinkedIn 开发并开源，此后它迅速从消息队列发展成为一个成熟的流媒体平台，拥有丰富的生态系统。除了 LinkedIn 之外，许多科技公司，如 Airbnb、Spotify 或 Twitter，都将 Kafka 用于其关键任务应用程序。

Kafka 可以用于很多事情，从消息传递、web 活动跟踪到日志聚合或流处理。从我作为数据专业人士的角度来看，Kafka 可以用作数据流管道的核心组件，以支持实时用例，如欺诈检测、预测性维护或实时分析。

有整本书都是关于卡夫卡的，开始阅读可能会让人望而生畏。然而，在这个项目中，我将向您展示使用 Docker、Kafka 和 Kafka Connect 创建流数据管道是多么容易。

# 商业问题

让我们定义一些要努力解决的业务问题。

假设你是一名数据工程师，在一家不断有用户注册的电子商务网站工作。营销团队希望向每一位注册的客户发送一封个性化的电子邮件。新提出的功能有一些问题:

*   注册服务与电子邮件服务相结合。换句话说，每次市场需求发生变化，你都必须对注册服务做出改变，如果你不小心的话，这可能会导致整个事情的发生。
*   市场营销要求电子邮件应该在用户注册时立即发送。用工程术语来说，他们指的是从客户注册开始的 5 秒钟内。
*   个性化模型与您的注册服务托管在不同的网络上。

与客户数据相关的另一个业务问题是，您的 CEO 希望每个员工都知道当前的客户数量以及他们从哪里注册。她让管理团队在办公室中央安装几个大显示器，而您的团队必须创建一个将在这些显示器上显示的仪表板。

*   您当前的数据仓库每天只获取用户数据，因此您不能使用现有的批处理管道。
*   仪表板服务也在不同的网络上，因此您不能直接查询生产数据库。

在研究了几个选项后，您意识到使用 Kafka 和运行在 Docker 上的 Kafka Connect 似乎是解决您的问题的最佳选项。以下是你研究后的发现。

## 卡夫卡是什么？

![](img/6842f5d5b6b762d34b025234fcce8636.png)

[图像信用](https://www.confluent.jp/blog/using-apache-kafka-drive-cutting-edge-machine-learning/)

Kafka 是一个用 Scala 和 Java 编写的开源流处理平台。根据 Kafka [网站](https://kafka.apache.org/intro)的说法，流媒体平台有三个关键能力:

*   发布和订阅记录流，类似于消息队列或企业消息传递系统。
*   以容错的持久方式存储记录流。
*   在记录流出现时处理它们。

Kafka 通常用于构建对数据流做出反应的实时应用程序，或者可靠地在系统或应用程序之间获取数据的实时数据管道。在我们的用例中，我们需要将生产系统(Postgres DB)中的数据传输到 se 团队正在开发的一个单独的电子邮件服务(MySQL DB ),以及数据团队的数据湖 S3。

关于卡夫卡，这里有一些你应该熟悉的概念:

*   Broker: Kafa broker 接收来自生产者的消息，并通过惟一偏移量存储它们。代理还允许消费者通过主题、分区和偏移量获取消息。
*   消息:是卡夫卡中的一个数据单位。您可以将每条消息视为数据库中的一条记录。
*   主题和分区:每个主题都是一个命名的消息流。一个主题由一个或多个分区组成。分区允许 Kafka 通过跨代理分发数据来进行水平扩展。

## 卡夫卡连线？

![](img/0729ef368221537bc28660f76f24f14d.png)

[形象信用](https://www.confluent.io/blog/simplest-useful-kafka-connect-data-pipeline-world-thereabouts-part-1/)

[Kafka Connect](https://docs.confluent.io/current/connect/index.html#:~:text=Kafka%20Connect%2C%20an%20open%20source,Kafka%20Connect%20for%20Confluent%20Platform.) ，Kafka 的一个开源组件，是一个将 Kafa 与数据库、键值存储、搜索索引、文件系统等外部系统连接起来的框架。以下是一些与 Kafka Connect 相关的概念:

*   连接器:连接器是一个逻辑作业，负责管理 Kafka 和其他系统之间的数据复制
*   源连接器:将数据从系统复制到 Kafka 的连接器
*   接收器连接器:将数据从一个或多个 Kafka 主题复制到系统的连接器
*   任务:每个连接器实例协调一组任务，将数据从一个系统复制到 Kafka，反之亦然

## Docker 和 docker-compose？

Docker 是一种容器技术，它允许我们将一个应用程序打包成它需要的所有部分，比如库和依赖项。您可以使用 Docker 在本地计算机上快速有效地部署这些服务，而不必在本地计算机上安装 Kafka、Kafka Connect 和所有数据库。

Docker-compose 是一个高级命令，它允许您使用 YAML 配置文件，通过一个命令来部署 Docker 容器。

## 建筑

抛开所有术语，让我们来看看这个解决方案的架构。

![](img/ae06133ce6291df41ce063d1cf002e93.png)

我们解决方案的架构

对于我们的示例，我们将使用 Kafka connect 从本地生产数据库中捕获 Users 表中的更改，并写入 Kafka 主题。两个连接器将订阅上面的主题，并将任何更改写入我们的电子邮件服务的 MySQL 数据库以及我们的数据湖 S3。

# 有趣的部分

现在，这是有趣的部分！让我们开始吧。

## 克隆我的回购

首先，通过在您的终端上键入以下命令来克隆我的回购:

```
[g](https://github.com/tuanchris/kafka-pipeline)it clone [https://github.com/tuanchris/kafka-pipeline](https://github.com/tuanchris/kafka-pipeline)
cd kafka-pipeline
```

[](https://github.com/tuanchris/kafka-pipeline) [## 图安克里斯/卡夫卡管道

### Kafka 最近越来越受欢迎，因为企业依赖它来驱动任务关键型应用程序和数据…

github.com](https://github.com/tuanchris/kafka-pipeline) 

## 安装 Docker 和 docker-compose

我们将在这个项目中使用 Docker 和 [docker-compose](https://docs.docker.com/compose/install/) ，你可以快速查找如何为你的操作系统安装它们。

## 创造环境

假设您已经安装了`conda`,那么您可以创建一个新的 env 并通过运行以下命令来安装所需的包:

```
conda create -n kafka-pipeline python=3.7 -y
conda activate kafka-pipeline
pip install -r requirements.txt
```

我们将需要 PostgreSQL 来连接到我们的源数据库(Postgres)并生成流数据。在 Mac OS 上，您可以通过运行以下命令使用 Homebrew 安装 PostgreSQL:

```
brew install postgresql
pip install psycopg2
```

你可以谷歌一下如何为其他平台安装 PostgreSQL

## 启动生产数据库(Postgres)

我们使用 docker-compose 以最小的努力启动服务。您可以使用以下命令启动 Postgres 生产数据库:

```
docker-compose -f docker-compose-pg.yml up -d
```

您的 Postgres 数据库应该运行在端口 5432 上，您可以通过在终端上键入`docker ps`来检查容器的状态。

## 生成流数据

我写了一个简短的脚本来使用 Faker 库生成用户数据。该脚本将在我们的 Postgres 数据库中每秒生成一条记录，模拟一个生产数据库。您可以使用以下命令在单独的终端标签中运行脚本:

```
python generate_data.py
```

如果一切设置正确，您将看到如下输出:

```
Inserting data {'job': 'Physiotherapist', 'company': 'Miller LLC', 'ssn': '097-38-8791', 'residence': '421 Dustin Ramp Apt. 793\nPort Luis, AR 69680', 'username': 'terri24', 'name': 'Sarah Moran', 'sex': 'F', 'address': '906 Andrea Springs\nWest Tylerberg, ID 29968', 'mail': 'nsmith@hotmail.com', 'birthdate': datetime.date(1917, 6, 3), 'timestamp': datetime.datetime(2020, 6, 29, 11, 20, 20, 355755)}
```

据此，我们模拟了每秒钟都有新客户数据的生产数据库。很整洁，是吧？

![](img/3197a13f6dde965478d046b23d067d18.png)

您可以使用 SQL 客户机(如 DataGrip 或 DBeaver)连接到 Postgres 数据库，以双重检查数据是否正在写入 Users 表。连接字符串应该是`jdbc:postgresql://TEST:password@postgres:5432/TEST`

## 开始我们的卡夫卡经纪人

很好，现在我们已经有了一个运行着数据流的生产数据库，让我们开始模拟的主要组件。我们将运行以下服务:

*   Kafka broker: Kafa broker 接收来自生产者的消息，并按唯一偏移量存储它们。代理还允许消费者通过主题、分区和偏移量获取消息。
*   Zookeeper: Zookeeper 跟踪 Kafka 集群节点以及 Kafka 主题和分区的状态
*   Schema registry: Schema registry 是一个获取和服务元数据(关于数据的数据)的层，比如数据类型、精度、小数位数…并提供不同服务之间的兼容性设置。
*   Kafka Connect: Kafka Connect 是一个框架，用于连接 Kafka 与外部系统，如数据库、键值存储、搜索索引和文件系统。
*   Kafdrop: Kafdrop 是一个开源的 web UI，用于查看 Kafka 主题和浏览消费群体。这将使检查和调试我们的消息更加容易。

我们可以通过运行以下命令来启动所有这些服务:

```
docker-compose -f docker-compose-kafka.yml up -d
```

等待几分钟，让 docker 下载图像并启动服务，然后您可以继续下一步。您可以使用以下命令查看上一个命令完成后的日志输出:

```
docker-compose -f docker-compose-kafka.yml logs -f
```

## 配置源连接器

接下来，我们将使用 Kafka connect rest API 配置我们的生产数据库(Postgres)的源连接器。将以下内容粘贴到您的终端:

```
curl -i -X PUT http://localhost:8083/connectors/SOURCE_POSTGRES/config \
     -H "Content-Type: application/json" \
     -d '{
            "connector.class":"io.confluent.connect.jdbc.JdbcSourceConnector",
            "connection.url":"jdbc:postgresql://postgres:5432/TEST",
            "connection.user":"TEST",
            "connection.password":"password",
            "poll.interval.ms":"1000",
            "mode":"incrementing",
            "incrementing.column.name":"index",
            "topic.prefix":"P_",
            "table.whitelist":"USERS",
            "validate.non.null":"false"
        }'
```

当您看到`HTTP/1.1 201 Created`时，连接器已成功创建。这个命令的作用是向 Kafka Connect 实例发送一个包含我们的配置的 JSON 消息。我将在这里解释一些配置，但是您可以在这里参考配置的完整列表[。](https://docs.confluent.io/current/connect/kafka-connect-jdbc/source-connector/source_config_options.html)

*   `connector.class`:我们使用 JDBC 源连接器连接到我们的生产数据库并提取数据。
*   `connection.url`:我们源数据库的连接字符串。由于我们用的是 Docker 的内网，所以数据库地址是 Postgres。如果要连接到外部数据库，请用数据库的 IP 替换 Postgres。
*   `connection.user` & `connection.password`:我们数据库的凭证。
*   `poll.interval.ms`:轮询新数据的频率。我们每秒都在轮询。
*   `mode`:轮询时更新每个表的模式。我们使用增量键(索引)，但是我们也可以使用时间戳或批量更新来更新。
*   `topic.prefix`:向卡夫卡写数据的题目前缀。
*   `table.whitelist`:要在我们的数据库中查找的表名列表。您还可以设置一个`query`参数来使用自定义查询。

随着 Kafdrop 实例的运行，您可以打开浏览器并转到`localhost:9000`来查看我们的`P_USERS`主题。你可以进入主题，看看我们主题的一些示例消息。

![](img/6390ac80a2e81ffb41afc762810fabca.png)

就这样，你就有了流向卡夫卡的用户数据流。

## 创建 MySQL 接收器连接器

先从 Mysql 说起吧。通过运行以下命令启动 Mysql 数据库:

```
docker-compose -f docker-compose-mysql.yml up -d
```

这是我们的配置:

```
curl -i -X PUT http://localhost:8083/connectors/SINK_MYSQL/config \
     -H "Content-Type: application/json" \
     -d '{
       		"connector.class":"io.confluent.connect.jdbc.JdbcSinkConnector",
       		"tasks.max":1,
       		"topics":"P_USERS",
           "insert.mode":"insert",
       		"connection.url":"jdbc:mysql://mysql:3306/TEST",
       		"connection.user":"TEST",
       		"connection.password":"password",
       		"auto.create":true
     	}'
```

就是这样。您生成的数据现在应该从 Postgres 流到 Mysql。让我们回顾一下 Mysql sink 连接器的属性:

*   `insert.mode`:如何将数据插入数据库。你可以在`insert`和`upsert`之间选择。
*   `topics`:从中读取数据的主题
*   `connection.url`:接收器连接 URL
*   `connection.user` & `connection.password`:汇凭证
*   `auto.create`:不存在时自动创建表格

让我们查询 MySQL 数据库，看看我们的数据是否在那里。我们可以看到记录计数和最大时间戳都在更新！

![](img/dafd55b2c1eae3e4b74068de56232ca1.png)

通过查询 SQL 数据库，我们可以实时看到新的数据

## 创建 S3 水槽连接器

要将数据写入 S3，同样简单明了。您需要在`docker-compose-kafka.yml`文件中设置环境变量:`AWS_ACCESS_KEY_ID`和`AWS_SECRET_ACCESS_KEY`。之后，您可以使用以下配置创建 S3 连接器:

```
curl -i -X PUT -H "Accept:application/json" \
    -H  "Content-Type:application/json" http://localhost:8083/connectors/SINK_S3/config \
    -d '
{
    "connector.class": "io.confluent.connect.s3.S3SinkConnector",
    "s3.region": "ap-southeast-1",
    "s3.bucket.name": "bucket-name",
    "topics": "P_USERS",
    "flush.size": "5",
    "timezone": "UTC",
    "tasks.max": "1",
    "value.converter.value.subject.name.strategy": "io.confluent.kafka.serializers.subject.RecordNameStrategy",
    "locale": "US",
    "format.class": "io.confluent.connect.s3.format.json.JsonFormat",
    "partitioner.class": "io.confluent.connect.storage.partitioner.DefaultPartitioner",
    "internal.value.converter": "org.apache.kafka.connect.json.JsonConverter",
    "storage.class": "io.confluent.connect.s3.storage.S3Storage",
    "rotate.schedule.interval.ms": "6000"
}'
```

一些值得注意的配置:

*   `s3.region`:你的 S3 桶的区域
*   `s3.bucket.name`:写入数据的桶名
*   `topics`:读取数据的主题
*   `format.class`:数据格式。您可以从`JSON`、`Avro`和`Parquet`中选择

## 管道

瞧，你的管道现在完成了。通过几个 docker-compose 配置文件和连接器配置，您已经创建了一个支持近实时数据分析功能的流管道。相当强大的东西！

## 后续步骤

现在用户数据在 Kafka 和我们的水槽中。电子邮件服务可以实时获取客户数据，轮询推荐 API，并在 2 秒钟内向客户发送欢迎电子邮件。同样，利用数据湖中的数据，您可以为您的 CEO 创建一个实时仪表板。

但是您不必就此止步，因为 Kafka 及其组件是可横向扩展的。您可以使用 Kafka 为大部分(如果不是全部)管道供电。一些公司运行 Kafka 集群，拥有数千个生产者和订户。当然，在这样的规模下，除了简单的概念验证设置之外，还有更多工作要做。但是有一些托管的 Kafka 服务，你可以开箱即用，比如 AWS 上的 MSK，或者 Confluent(多平台)。您还可以添加更多组件来处理实时数据，如 Spark Streaming、KSQL、Beam 或 Flink。

## 打扫

如果您没有运行任何其他 docker 容器，您可以使用以下命令关闭这个项目的 docker 容器:

```
docker stop $(docker ps -aq)
```

或者，您可以通过运行以下命令来清理本地下载的 docker 映像:

```
docker system prune
```

# 结论

在这个项目中，我们使用 Docker、Kafka 和 Kafka Connect 构建了一个流数据管道，这让我们的营销团队和首席执行官非常高兴。有了我们构建的东西，其他团队可以很容易地从那里得到它，交付我们的涉众所要求的东西。

如果你以前从未使用过 Kafka，我鼓励你自己尝试这个项目。如果你有兴趣阅读类似的东西，neptune.ai 的人写了一篇关于如何用 Kedro 构建数据科学管道的优秀文章。

快乐学习:)