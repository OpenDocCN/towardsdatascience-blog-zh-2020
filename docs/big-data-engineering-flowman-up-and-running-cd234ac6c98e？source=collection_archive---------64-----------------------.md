# 大数据工程— Flowman 启动并运行

> 原文：<https://towardsdatascience.com/big-data-engineering-flowman-up-and-running-cd234ac6c98e?source=collection_archive---------64----------------------->

## 在您的机器上查看名为 Flowman 的开源、基于 Spark 的 ETL 工具。

![](img/c057eba6f9be09b943985e532ed07582.png)

西蒙·威尔克斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

这是大数据环境中的数据工程系列的第 4 部分。它将反映我个人的经验教训之旅，并在我创建的开源工具 [Flowman](https://flowman.readthedocs.io) 中达到高潮，以承担在几个项目中一遍又一遍地重新实现所有锅炉板代码的负担。

*   [第 1 部分:大数据工程—最佳实践](https://medium.com/@kupferk/big-data-engineering-best-practices-bfc7e112cf1a)
*   [第 2 部分:大数据工程— Apache Spark](https://medium.com/@kupferk/big-data-engineering-apache-spark-d67be2d9b76f)
*   [第 3 部分:大数据工程——声明性数据流](/big-data-engineering-declarative-data-flows-3a63d1802846)
*   第 4 部分:大数据工程——flow man 启动和运行

# 期待什么

本系列是关于用 Apache Spark 构建批处理数据管道的。上次我介绍了 Flowman 的核心思想，这是一个基于 Apache Spark 的应用程序，它简化了批处理数据管道的实现。现在是时候让 [Flowman](https://flowman.readthedocs.io) 在本地机器上运行了。

# 先决条件

为了按照说明在您的机器上安装一个正常工作的 Flowman，您不需要太多:

*   要求:64 位 Linux(抱歉，目前没有 Windows 或 Mac OS)
*   必选:Java (OpenJDK 也可以)
*   可选:Maven 和 npm，如果您想从源代码构建 Flowman
*   推荐:访问 S3 上的一些测试数据的 AWS 凭证

# 安装 Hadoop 和 Spark

尽管 Flowman 直接建立在 Apache Spark 的能力之上，但它并没有提供一个有效的 Hadoop 或 Spark 环境——这是有原因的:在许多环境中(特别是在使用 Hadoop 发行版的公司中),一些平台团队已经提供了 Hadoop/Spark 环境。Flowman 尽最大努力不搞砸，而是需要一个工作的火花装置。

幸运的是，Spark 很容易安装在本地机器上:

## 下载并安装 Spark

目前最新的 Flowman 版本是 0.14.2，可以在 [Spark 主页](https://spark.apache.org/downloads.html)上获得 Spark 3.0.1 的预构建版本。因此，我们从 Apache 归档文件中下载适当的 Spark 发行版，并对其进行解压缩。

```
# Create a nice playground which doesn't mess up your system
$ **mkdir playground**
$ **cd playground**# Download and unpack Spark & Hadoop
$ **curl -L** [**https://archive.apache.org/dist/spark/spark-3.0.1/spark-3.0.1-bin-hadoop3.2.tgz**](https://archive.apache.org/dist/spark/spark-3.0.1/spark-3.0.1-bin-hadoop3.2.tgz) **| tar xvzf -**# Create a nice link
$ **ln -snf spark-3.0.1-bin-hadoop3.2 spark**
```

Spark 包中已经包含了 Hadoop，所以只需下载一次，您就已经安装了 Hadoop，并且彼此集成。

# 安装流量计

## 下载和安装

你可以在 GitHub 的相应[发布页面上找到预建的 Flowman 包。对于这个研讨会，我选择了](https://github.com/dimajix/flowman/releases)[flowman-dist-0.14.2-oss-spark3.0-hadoop3.2-bin.tar.gz](https://github.com/dimajix/flowman/releases/download/0.14.2/flowman-dist-0.14.2-oss-spark3.0-hadoop3.2-bin.tar.gz)，它非常适合我们之前刚刚下载的 Spark 包。

```
# Download and unpack Flowman
$ **curl -L** [**https://github.com/dimajix/flowman/releases/download/0.14.2/flowman-dist-0.14.2-oss-spark3.0-hadoop3.2-bin.tar.gz**](https://github.com/dimajix/flowman/releases/download/0.14.2/flowman-dist-0.14.2-oss-spark3.0-hadoop3.2-bin.tar.gz) **| tar xvzf -**# Create a nice link
$ **ln -snf flowman-0.14.2 flowman**
```

## 配置

现在，在使用 Flowman 之前，您需要告诉它在哪里可以找到我们在上一步中刚刚创建的 Spark 主目录。这可以通过在`flowman/conf/flowman-env.sh`中提供一个有效的配置文件来完成(模板可以在`flowman/conf/flowman-env.sh.template`中找到)，或者我们可以简单地设置一个环境变量。为了简单起见，我们遵循第二种方法

```
# This assumes that we are still in the directory "playground"
$ **export SPARK_HOME=$(pwd)/spark**
```

为了在下面的例子中访问 S3，我们还需要提供一个包含一些基本插件配置的默认名称空间*。我们简单地复制提供的模板如下:*

```
# Copy default namespace
$ **cp flowman/conf/default-namespace.yml.template flowman/conf/default-namespace.yml**# Copy default environment
$ **cp flowman/conf/flowman-env.sh.template flowman/conf/flowman-env.sh**
```

这就是我们运行 Flowman 示例所需的全部内容。

# 天气示例

下面的演练将使用“天气”示例，该示例对来自 NOAA 的[“综合地表数据集”的一些天气数据执行一些简单的处理。](https://www.ncdc.noaa.gov/isd)

## 项目详情

“天气示例”执行三个典型的 ETL 处理任务:

*   读入原始测量数据，提取一些属性，并将结果存储在拼花地板文件中
*   读入工作站主数据并存储在拼花文件中。
*   将测量值与主数据整合，汇总每个国家和年份的测量值，计算最小值、最大值和平均值，并将结果存储在拼花文件中。

这三个任务包括许多 ETL 管道中常见的典型的“读取、提取、写入”、“集成”和“聚合”操作。

为了简单起见，该示例没有使用 Hive 元存储，尽管我强烈建议将它用于任何严肃的 Hadoop/Spark 项目，作为管理元数据的中央权威。

## Flowman 项目结构

您将在目录`flowman/examples/weather`中找到 Flowman 项目。该项目由几个子文件夹组成，反映了 Flowman 的基本实体类型:

*   **config** —该文件夹包含一些配置文件，这些文件包含 Spark、Hadoop 或 Flowman 本身的属性。
*   **模型**—模型文件夹包含存储在磁盘或 S3 上的物理数据模型的描述。有些模型是指读入的源数据，有些模型是指将由 Flowman 生成的目标数据。
*   **映射** —映射文件夹包含处理逻辑。这包括读入数据等简单步骤，以及连接和聚合等更复杂的步骤。
*   **目标** —目标文件夹包含*构建目标*的描述。Flowman 认为自己是一个数据构建工具，因此它需要知道应该构建什么。目标通常只是简单地将映射的结果与数据模型耦合起来，用作接收器。
*   **作业**—最后，作业文件夹包含作业(在本例中只有一个)，它或多或少只是应该一起构建的目标列表。Flowman 将负责正确的建造顺序。

这种目录布局有助于将一些结构带入项目，但是您可以使用您喜欢的任何其他布局。你只需要指定`project.yml`文件中的目录，这个文件在例子的根目录下(即`flowman/examples/weather`)。

# Flowman 手动操作

示例数据存储在我自己提供的 S3 存储桶中。为了访问数据，您需要在您的环境中提供有效的 AWS 凭据:

```
$ **export AWS_ACCESS_KEY_ID=<your aws access key>**
$ **export AWS_SECRET_ACCESS_KEY=<your aws secret key>**
```

我们通过运行交互式 Flowman shell 来启动 Flowman。虽然这不是自动批处理中使用的工具(`flowexec`是该场景的合适工具)，但它让我们很好地了解了 Flowman 中的 ETL 项目是如何组织的。

```
# This assumes that we are still in the directory "playground"
$ **cd flowman**# Start interactive Flowman shell
$ **bin/flowshell -f examples/weather**20/10/10 09:41:21 INFO SystemSettings: Using default system settings
20/10/10 09:41:21 INFO Namespace: Reading namespace file /home/kaya/tmp/playgroud/flowman-0.14.2-SNAPSHOT/conf/default-namespace.yml
20/10/10 09:41:23 INFO Plugin: Reading plugin descriptor /home/kaya/tmp/playgroud/flowman-0.14.2-SNAPSHOT/plugins/flowman-example/plugin.yml
...
```

日志输出完成后，您应该会看到一个提示`flowman:weather>`，现在您可以输入一些要执行的命令。

## 建筑物

首先，我们要执行`main`任务，并*构建*所有已定义的目标。因为作业定义了一个参数`year`，每次调用只处理一年，所以我们需要为这个参数提供一个值。因此，我们通过下面的命令开始 2011 年`main`中所有目标的构建过程:

```
flowman:weather> **job build main year=2011**20/10/10 09:41:33 INFO Runner: Executing phases 'create','build' for job 'weather/main'
20/10/10 09:41:33 INFO Runner: Job argument year=2011
20/10/10 09:41:33 INFO Runner: Running phase create of job 'weather/main'   with arguments year=2011
20/10/10 09:41:33 INFO Runner: Environment (phase=create) basedir=file:///tmp/weather
20/10/10 09:41:33 INFO Runner: Environment (phase=create) force=false
20/10/10 09:41:33 INFO Runner: Environment (phase=create) job=main
20/10/10 09:41:33 INFO Runner: Environment (phase=create) namespace=default
20/10/10 09:41:33 INFO Runner: Environment (phase=create) phase=create
20/10/10 09:41:33 INFO Runner: Environment (phase=create) project=weather
20/10/10 09:41:33 INFO Runner: Environment (phase=create) year=2011
...
```

再次产生大量输出(部分由 Spark 的启动产生，但也由 Flowman 产生，因此您实际上可以跟踪 Flowman 在做什么。日志记录总是一个困难的主题——过多的日志记录会分散用户的注意力，而过少的输出会使问题的解决更加困难。

具体来说，您应该看到两个重要的事实:

```
...
20/10/10 09:41:33 INFO Runner: Executing phases 'create','build' for job 'weather/main'
20/10/10 09:41:33 INFO Runner: Running phase '**create**' of job 'weather/main'   with arguments year=2011
...
```

后来呢

```
...
20/10/10 09:41:37 INFO Runner: Running phase '**build**' of job 'weather/main'   with arguments year=2011
...
```

这意味着指示 flow man*构建*一个任务的所有目标实际上执行了完整的*构建生命周期*，它包括以下两个阶段

*   “ **create** ”阶段创建和/或迁移构建目标中引用的所有物理数据模型，如目录结构、配置单元表或数据库表。这个阶段只关注*模式管理*。
*   然后'**构建**'阶段将用从数据流中创建的记录填充这些关系，如映射和目标所定义的。

Flowman 还支持清理生命周期，首先在“ **truncate** 阶段删除数据，然后在“ **destroy** 阶段删除所有目录和表格。

## 目标构建顺序

当您研究 weather 项目的细节时，您会发现构建目标有一些隐含的运行时依赖性:首先，来自 S3 的原始数据需要传输到您的本地机器，并存储在一些本地目录中(实际上在`/tmp/weather`)。然后这些目录作为构建`aggregates`目标的输入。

您不会发现任何显式的依赖关系信息，Flowman 会自己找出这些构建时依赖关系，并以正确的顺序执行所有操作。您可以在以下日志记录输出中看到这一点:

```
20/10/12 20:29:15 INFO Target: Dependencies of phase 'build' of target 'weather/measurements': 
20/10/12 20:29:15 INFO Target: Dependencies of phase 'build' of target 'weather/aggregates': weather/stations,weather/measurements
20/10/12 20:29:15 INFO Target: Dependencies of phase 'build' of target 'weather/stations': 
20/10/12 20:29:15 INFO Runner: Executing phase 'build' with sequence: weather/measurements, weather/stations, weather/aggregates
```

Flowman 首先收集所有隐式依赖项(前三个日志行)，然后找到所有目标的适当执行顺序(最后一个日志行)。

所有构建目标的自动排序在具有许多依赖输出的复杂项目中非常有用，在这些项目中很难手动跟踪正确的构建顺序。使用 Flowman，您只需添加一个新的构建目标，所有的输出都会自动以正确的顺序写入。

## 重建

当您现在尝试重建同一年时，Flowman 将自动跳过处理，因为所有目标关系都已建立并包含 2011 年的有效数据:

```
flowman:weather> **job build main year=2011**...
20/10/10 10:56:03 INFO Runner: Target 'weather/measurements' not dirty in phase build, skipping execution
...
20/10/10 10:56:03 INFO JdbcStateStore: Mark last run of phase 'build' of job 'default/weather/main' as skipped in state database
...
```

同样，这个逻辑是从经典的构建工具领域借用来的，比如`make`，它也跳过现有的目标。

当然，您可以通过在命令末尾添加一个`--force`标志来强制 Flowman 重新处理数据。

## 检查关系

现在我们想直接在 Flowman 中检查一些结果(不需要借助一些命令行工具来检查 Parquet 文件)。这可以通过检查*关系*来完成，这些关系总是代表存储在一些磁盘或数据库中的物理模型。

首先，让我们获得项目中定义的所有关系的列表:

```
flowman:weather> **relation list**aggregates
measurements
measurements-raw
stations
stations-raw
```

当您查看了`model`目录中的项目文件后，您应该会觉得很熟悉。该列表包含项目中定义的所有模型名称。

现在让我们检查存储在`stations`关系中的主数据:

```
flowman:weather> **relation show stations**...
usaf,wban,name,country,state,icao,latitude,longitude,elevation,date_begin,date_end
007018,99999,WXPOD 7018,null,null,null,0.0,0.0,7018.0,20110309,20130730
007026,99999,WXPOD 7026,AF,null,null,0.0,0.0,7026.0,20120713,20170822
007070,99999,WXPOD 7070,AF,null,null,0.0,0.0,7070.0,20140923,20150926
008260,99999,WXPOD8270,null,null,null,0.0,0.0,0.0,20050101,20100920
008268,99999,WXPOD8278,AF,null,null,32.95,65.567,1156.7,20100519,20120323
008307,99999,WXPOD 8318,AF,null,null,0.0,0.0,8318.0,20100421,20100421
008411,99999,XM20,null,null,null,null,null,null,20160217,20160217
008414,99999,XM18,null,null,null,null,null,null,20160216,20160217
008415,99999,XM21,null,null,null,null,null,null,20160217,20160217
008418,99999,XM24,null,null,null,null,null,null,20160217,20160217
...
```

目前，所有数据都显示为 CSV——这并不意味着数据存储为 CSV(事实并非如此，它存储在 Parquet 文件中)。

现在让我们来考察一下`measurements`的数据:

```
flowman:weather> **relation show measurements**...
usaf,wban,date,time,wind_direction,wind_direction_qual,wind_observation,wind_speed,wind_speed_qual,air_temperature,air_temperature_qual,year
999999,63897,20110101,0000,155,1,H,7.4,1,19.0,1,2011
999999,63897,20110101,0005,158,1,H,4.8,1,18.6,1,2011
999999,63897,20110101,0010,159,1,H,4.4,1,18.5,1,2011
999999,63897,20110101,0015,148,1,H,3.9,1,18.3,1,2011
999999,63897,20110101,0020,139,1,H,3.6,1,18.1,1,2011
999999,63897,20110101,0025,147,1,H,3.6,1,18.1,1,2011
999999,63897,20110101,0030,157,1,H,4.0,1,18.0,1,2011
999999,63897,20110101,0035,159,1,H,3.5,1,17.9,1,2011
999999,63897,20110101,0040,152,1,H,3.0,1,17.8,1,2011
999999,63897,20110101,0045,140,1,H,3.4,1,17.8,1,2011
```

最后，让我们来看看总量。类似于`measurements`关系，`aggregates`关系也是分区的。我们还可以选择为分区列指定一个值:

```
flowman:weather> **relation show aggregates -p year=2011**country,min_wind_speed,max_wind_speed,avg_wind_speed,min_temperature,max_temperature,avg_temperature,year
SF,0.0,12.3,2.1503463,2.3,34.9,17.928537,2011
US,0.0,36.0,2.956173,-44.0,46.4,11.681804,2011
RS,0.0,12.0,3.3127716,-33.0,32.0,4.5960307,2011
MY,0.0,10.8,2.0732634,21.0,34.0,27.85072,2011
GM,0.0,15.4,3.910823,-10.0,30.0,9.442137,2011
FI,0.0,21.0,3.911331,-34.4,30.7,3.1282191,2011
IC,0.0,31.9,7.235976,-14.0,17.2,5.138462,2011
SC,0.0,14.0,4.618577,20.0,32.0,27.329834,2011
NL,0.0,31.4,4.969081,-8.2,34.0,10.753498,2011
AU,0.0,15.9,1.9613459,-15.0,35.0,9.814931,2011
```

## 检查映射

在 ETL 应用程序的开发过程中，能够查看一些中间结果通常非常有用。这些映射的中间结果可能并不总是在磁盘上或数据库中有物理表示。它们只是概念，记录在处理过程中只在内存中具体化。但是 Flowman shell 也支持通过查看映射的结果来检查这些中间结果。

在我们展示这个特性之前，我们首先需要提前执行一个有点笨拙的步骤:与关系相反，数据流本身依赖于参数`year`，并且在没有设置该参数的情况下无法执行或检查。此外，Flowman 作业还可能设置一些执行所需的附加环境变量。

为了减轻对这种情况的处理，Flowman 提供了一个特殊的命令来设置执行环境，因为它将由特定的作业提供:

```
flowman:weather> **job enter main year=2011** flowman:weather/main>
```

作为回报，提示符发生变化，现在还包含作业的名称。

现在我们可以开始检查中间结果了。首先，让我们获得所有映射的列表:

```
flowman:weather/main> **mapping list**aggregates
facts
measurements
measurements-extracted
measurements-joined
measurements-raw
stations
stations-raw
```

现在，让我们从检查存储在 S3 的原始测量数据开始:

```
flowman:weather/main> **mapping show measurements-raw**043499999963897201101010000I+32335-086979CRN05+004899999V0201551H007419999999N999999999+01901+99999999999ADDAA101000991AO105000491CF1105210CF2105210CF3105210CG1+0120410CG2+0126710CG3+0122710CN1012610012110999990CN2+999990+0219100010CN30149971005638010CN40100000104001016010CO199-06CR10510210CT1+019010CT2+019110CT3+019010CU1+999990000410CU2+999990000410CU3+999990000410CV1+019010999990+020310999990CV2+019110999990+020310999990CV3+019010999990+020310999990CW100330101076010KA1010M+02031KA2010N+01901KF1+01951OB10050100101571099999900105210
013399999963897201101010005I+32335-086979CRN05+004899999V0201581H004819999999N999999999+01861+99999999999ADDAO105000091CG1+0120410CG2+0126710CG3+0122710CO199-06CT1+018610CT2+018610CT3+018610CW100330102311010OB10050064101581099999900096910
013399999963897201101010010I+32335-086979CRN05+004899999V0201591H004419999999N999999999+01851+99999999999ADDAO105000091CG1+0120410CG2+0126710CG3+0122710CO199-06CT1+018410CT2+018510CT3+018510CW100330102901010OB10050054101541099999900105010
...
```

如您所见，原始数据很难处理。但是幸运的是，我们已经有了提取至少一些简单字段的映射:

```
flowman:weather/main> **mapping show measurements-extracted**wind_speed_qual,wban,usaf,air_temperature,date,wind_speed,air_temperature_qual,wind_direction,report_type,wind_direction_qual,time,wind_observation
1,63897,999999,19.0,20110101,7.4,1,155,CRN05,1,0000,H
1,63897,999999,18.6,20110101,4.8,1,158,CRN05,1,0005,H
1,63897,999999,18.5,20110101,4.4,1,159,CRN05,1,0010,H
1,63897,999999,18.3,20110101,3.9,1,148,CRN05,1,0015,H
1,63897,999999,18.1,20110101,3.6,1,139,CRN05,1,0020,H
1,63897,999999,18.1,20110101,3.6,1,147,CRN05,1,0025,H
1,63897,999999,18.0,20110101,4.0,1,157,CRN05,1,0030,H
1,63897,999999,17.9,20110101,3.5,1,159,CRN05,1,0035,H
1,63897,999999,17.8,20110101,3.0,1,152,CRN05,1,0040,H
1,63897,999999,17.8,20110101,3.4,1,140,CRN05,1,0045,H
```

使用相同的命令，我们还可以检查`stations`映射:

```
flowman:weather/main> **mapping show stations**usaf,wban,name,country,state,icao,latitude,longitude,elevation,date_begin,date_end
007018,99999,WXPOD 7018,null,null,null,0.0,0.0,7018.0,20110309,20130730
007026,99999,WXPOD 7026,AF,null,null,0.0,0.0,7026.0,20120713,20170822
007070,99999,WXPOD 7070,AF,null,null,0.0,0.0,7070.0,20140923,20150926
008260,99999,WXPOD8270,null,null,null,0.0,0.0,0.0,20050101,20100920
008268,99999,WXPOD8278,AF,null,null,32.95,65.567,1156.7,20100519,20120323
008307,99999,WXPOD 8318,AF,null,null,0.0,0.0,8318.0,20100421,20100421
008411,99999,XM20,null,null,null,null,null,null,20160217,20160217
008414,99999,XM18,null,null,null,null,null,null,20160216,20160217
008415,99999,XM21,null,null,null,null,null,null,20160217,20160217
008418,99999,XM24,null,null,null,null,null,null,20160217,20160217
```

最后，我们可以再次离开作业上下文(这将清除所有参数和特定于作业的环境变量):

```
flowman:weather/main> **job leave**
```

## 执行历史

Flowman 还可以选择跟踪已经执行的所有过去运行的作业。这些信息存储在一个小型数据库中。这个历史数据库背后的想法是提供一个机会来记录成功和失败的运行，而不需要额外的外部监控系统。

我们在开始时从一个模板复制的示例配置`default-namespace.yml`支持这种历史记录，并将信息存储在一个小型 Derby 数据库中。

```
flowman:weather> **history job search**+---+---------+-------+----+------+----+-------+-----------------------------+-----------------------------+
| id|namespace|project| job| phase|args| status|                     start_dt|                       end_dt|
+---+---------+-------+----+------+----+-------+-----------------------------+-----------------------------+
|  1|  default|weather|main|create|    |success|2020-10-09T11:37:35.161Z[UTC]|2020-10-09T11:37:40.211Z[UTC]|
|  2|  default|weather|main| build|    |success|2020-10-09T11:37:40.230Z[UTC]|2020-10-09T11:38:43.435Z[UTC]|
+---+---------+-------+----+------+----+-------+-----------------------------+-----------------------------+
```

我们也可以搜索所有已经建成的目标。在以下示例中，我们指定只搜索作为 id 为 1 的作业运行的一部分而构建的所有目标:

```
flowman:weather> **history target search -J 1**+--+-----+---------+-------+------------+----------+------+-------+-----------------------------+-----------------------------+
|id|jobId|namespace|project|      target|partitions| phase| status|                     start_dt|                       end_dt|
+--+-----+---------+-------+------------+----------+------+-------+-----------------------------+-----------------------------+
| 1|    1|  default|weather|measurements|          |create|success|2020-10-12T18:02:50.633Z[UTC]|2020-10-12T18:02:52.179Z[UTC]|
| 2|    1|  default|weather|  aggregates|          |create|success|2020-10-12T18:02:52.244Z[UTC]|2020-10-12T18:02:52.261Z[UTC]|
| 3|    1|  default|weather|    stations|          |create|success|2020-10-12T18:02:52.304Z[UTC]|2020-10-12T18:02:52.320Z[UTC]|
+--+-----+---------+-------+------------+----------+------+-------+-----------------------------+-----------------------------+
```

## 放弃

最后，我们通过`exit`或`quit`退出 Flowman shell:

```
flowman:weather> **quit**
```

# 执行项目

到目前为止，我们只将 Flowman shell 用于项目的交互工作。实际上，开发 shell 的第二步是帮助分析问题和调试数据流。使用 Flowman 项目的主要命令是`flowexec`，用于非交互式批处理执行，例如在 cron-jobs 中。

它与 Flowman shell 共享大量代码，因此命令通常完全相同。主要的区别是使用`flowexec`你可以在命令行上指定命令，而`flowshell`会提供自己的提示。

例如，要运行 2014 年天气项目的“构建”生命周期，您只需运行:

```
$ **bin/flowexec -f examples/weather job build main year=2014**...
```

# 谢谢！

非常感谢您花时间阅读这篇冗长的文章。非常感谢您！发给所有试图在本地机器上实践这个例子的人。如果你对这个例子有疑问，请给我留言——简化这样的过程总是很困难的，我可能会忽略一些问题。

# 最后的话

这是关于使用 Apache Spark 构建健壮的数据管道的系列文章的最后一部分。现在，您应该对我构建数据管道的最佳实践的偏好以及我在 Flowman 中的实现有了一个概念，Flowman 目前在两家不同的公司的生产中使用。整个方法已经为我提供了很好的服务，我仍然更喜欢它，而不是一个不统一的单个 Spark 应用程序集合，它们本质上做着非常相似的事情。

如果您对在您的项目中使用 Flowman 感兴趣，那么非常欢迎您尝试一下，并与我联系以解决任何问题。