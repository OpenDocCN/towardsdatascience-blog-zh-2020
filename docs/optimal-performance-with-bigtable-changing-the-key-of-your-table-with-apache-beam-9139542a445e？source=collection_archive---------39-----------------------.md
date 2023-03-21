# 使用 Bigtable 优化性能:使用 Apache Beam 更改表的键

> 原文：<https://towardsdatascience.com/optimal-performance-with-bigtable-changing-the-key-of-your-table-with-apache-beam-9139542a445e?source=collection_archive---------39----------------------->

![](img/881c9b5d708babee4e03de09bcf8009a.png)

克里斯蒂安·恩格梅尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

[Cloud Bigtable](https://cloud.google.com/bigtable) 是一个高性能的分布式 NoSQL 数据库，可以存储数 Pb 的数据，并以低于 10 毫秒的延迟响应查询。然而，为了达到这一性能水平，[为您的表选择正确的键](https://cloud.google.com/bigtable/docs/schema-design)非常重要。此外，您能够进行的查询类型取决于您为表选择的键。

Bigtable 附带了 [Key Visualizer](https://cloud.google.com/bigtable/docs/keyvis-overview) 工具，用于诊断我们的键是如何执行的。为了开始产生结果，Key Visualizer 至少需要 30 GB 的数据和一些工作负载。您拥有的数据越多越好，因为结果会越接近您在生产环境中部署时的结果。

所以当你在 Bigtable 中存储了很多数据之后，**如果你发现你的键性能不好怎么办？如何更新密钥？需要再从头开始吗？**

不，您可以使用 Apache Beam 管道来更新密钥。

在这篇文章中，我们描述了如何使用 Apache Beam 管道来更改表的键，只需编写几行代码来定义新的键。我们将使用演示数据在本地运行所有内容(Apache Beam 使用 DirectRunner，Bigtable 使用模拟器)，因此没有必要为了跟进这篇文章而使用 Google Cloud 项目。但是你需要安装[Google Cloud SDK](https://cloud.google.com/sdk/docs/quickstarts)。

对于有大量数据的真实情况，建议在[数据流](https://cloud.google.com/dataflow)中运行这个管道，但是对于演示来说，DirectRunner 就足够了。

Google Cloud 专业服务团队的 Github repo 中提供了管道:

[](https://github.com/GoogleCloudPlatform/professional-services/tree/master/examples/bigtable-change-key) [## Google cloud platform/professional-services/examples/bigtable-change-

### 更改 Bigtable 中表的键的数据流管道

github.com](https://github.com/GoogleCloudPlatform/professional-services/tree/master/examples/bigtable-change-key) 

克隆 repo 并转到此管道的目录。您可以在本地或在云外壳中克隆回购。

```
git clone [https://github.com/GoogleCloudPlatform/professional-services](https://github.com/GoogleCloudPlatform/professional-services)cd professional-services/examples/bigtable-change-key
```

如果您已经有了一个 Bigtable 实例，可以转到下一节。在这里，我们将展示如何启动 Google Cloud SDK 附带的 [Bigtable 模拟器](https://cloud.google.com/bigtable/docs/emulator),因此您不需要为 Bigtable 实例付费来试用这篇博客文章。

为了启动模拟器，我们在终端中运行以下命令:

```
gcloud beta emulators bigtable start
```

它通常会在端口 8086 启动一个服务器。但是不要担心这些细节，因为我们稍后将使用`gcloud`实用程序来设置它们。

现在，在不同的 shell 会话中(例如，新的终端窗口或新的终端标签)，我们将设置一些环境变量来使用模拟器。我们将不得不使用该会话来运行管道，因此请确保在新的终端中继续运行命令。现在，通过运行以下命令来设置环境变量:

```
$(gcloud beta emulators bigtable env-init)
```

我们可以检查一下，现在有一些变量指向我们的本地 Bigtable 服务器:

```
echo $BIGTABLE_EMULATOR_HOST
```

您应该会看到类似于`localhost:8086`的输出

如果您得到一个空的输出，那么一定是出了问题(也许您不是在同一个 shell 会话中运行？).

我们现在需要配置`cbt`来使用这个模拟器作为 Bigtable 实例。为此，我们需要在`~/.cbtrc`处编辑文件，并设置以下内容(我们必须在管道配置中使用相同的*项目*和*实例*名称):

```
project = my-fake-project
instance = my-fake-instance
```

现在您已经有了一个正在运行的 Bigtable 实例，让我们用数据填充它。为此，我们可以在管道中使用 Github 中包含的一个脚本。在管道的主目录中(在我们的 repo 克隆中)，我们运行以下脚本，创建一个名为`taxis`的表:

```
./scripts/create_sandbox_table.sh taxis
```

现在我们可以检查表是否已经创建。记住，您需要配置`cbt`来访问您的实例。在上一节中，我们已经为 Bigtable 模拟器完成了这项工作。如果您跳过了前一部分，请[检查如何配置 cbt](https://cloud.google.com/bigtable/docs/quickstart-cbt) 。

我们运行这个命令

```
cbt ls taxis
```

我们应该得到这样的输出

```
Family Name      GC Policy
-----------      ---------
id               <never>
loc              <never>
```

我们还可以通过运行以下命令来检查该表中有 3 行

```
cbt count taxis
```

Bigtable 模拟器将这些数据保存在内存中。也就是说，它是短暂的。如果停止模拟器，数据将会丢失。因此，您需要再次填充它才能使用它。**不要在仿真器中存储任何有价值的数据，它不会被保存**。

现在我们在一个表中有了一些数据，我们可以编写代码来更改表的键。我们将对密钥做一个简单的更改:我们将反转它。对于自动递增的值，比如时间戳，这是一个常见的*技巧*来尝试改善表中数据的分布。有更多的考虑和可能的改变，但是对于这篇文章，我们将只做这些改变。

一般来说，要更改现有表的键，在我们的管道中，我们将使用以下两个元素来决定新键:当前(旧)键和完整记录(包含所有列)。

我们应该能够根据这些信息决定新的密钥。因为我们只是反转旧密钥，所以在本例中，我们将忽略该记录来生成新密钥。

为了实现这种密钥更改，我们需要更新管道的代码。你需要改变[类](https://github.com/GoogleCloudPlatform/professional-services/blob/433da74fa5665dff9bb368133b46c384731eb0e2/examples/bigtable-change-key/src/main/java/com/google/cloud/pso/pipeline/BigtableChangeKey.java#L41-L50) `[com.google.cloud.pso.pipeline.BigtableChangeKey](https://github.com/GoogleCloudPlatform/professional-services/blob/433da74fa5665dff9bb368133b46c384731eb0e2/examples/bigtable-change-key/src/main/java/com/google/cloud/pso/pipeline/BigtableChangeKey.java#L41-L50)`中的 `[transformKey](https://github.com/GoogleCloudPlatform/professional-services/blob/433da74fa5665dff9bb368133b46c384731eb0e2/examples/bigtable-change-key/src/main/java/com/google/cloud/pso/pipeline/BigtableChangeKey.java#L41-L50)` [函数。代码如下面的代码片段所示:](https://github.com/GoogleCloudPlatform/professional-services/blob/433da74fa5665dff9bb368133b46c384731eb0e2/examples/bigtable-change-key/src/main/java/com/google/cloud/pso/pipeline/BigtableChangeKey.java#L41-L50)

函数来更新记录的键

该函数需要返回一个带有新密钥的`String`。输入参数是当前键(作为一个`String`)，以及包含所有列的完整记录(作为一个`Row`)。记录的类型是`Row`，可以在 Google Bigtable 库中找到[。](http://googleapis.github.io/googleapis/java/all/latest/apidocs/com/google/bigtable/v2/Row.html)

如何恢复该记录中的列和所有数据？参见[管道中的一个例子，它遍历记录以创建它的副本](https://github.com/GoogleCloudPlatform/professional-services/blob/433da74fa5665dff9bb368133b46c384731eb0e2/examples/bigtable-change-key/src/main/java/com/google/cloud/pso/dofns/UpdateKey.java#L52-L69)(这就是为什么它创建一个突变列表，在 Bigtable 的隐语中更改一行)。但是总的来说，只需遍历列族、列和单元格值，就可以恢复该记录中包含的所有元素(请记住，一个单元格可能有一个以上的单元格版本，这就是为什么会有多个单元格)。这段代码片段展示了如何遍历一个`Row`

显示如何在 Java 中遍历 Bigtable 行的代码片段

管道中包含的默认代码反转密钥，因此不需要实际做任何更改来反转它。但是请随意更改代码，尝试不同的密钥更新功能。请记住，每个记录的键应该是唯一的；具有相同键的记录将被管道覆盖。

现在我们已经编写了更新密钥的代码，让我们运行管道。在这个例子中，我们将使用 Apache Beam 的`DirectRunner`(而不是，例如，`DataflowRunner`)，因为我们正在处理一个只有 3 条记录存储在 Bigtable 模拟器中的表。

在运行管道之前，我们必须确保目标表存在，并且它与我们从中复制的表具有相同的列族。为此，我们运行以下脚本。该脚本使用的是`cbt`，我们已经将其配置为指向我们的 Bigtable 实例。

```
./scripts/copy_schema_to_new_table.sh taxis taxis_newkey
```

这应该会生成一个额外的表。如果我们执行`cbt ls`，我们应该得到这个输出:

```
taxis
taxis_newkey
```

新表应该有两个柱族。`cbt ls taxis_newkey`的输出应该返回类似如下的内容:

```
Family    Name GC Policy
------    --------------
id        <never>
loc       <never>
```

我们可以检查一下桌子是不是空的。命令`cbt count taxis_newkey`应该返回 0。

现在，让我们编译并构建这个包。为此，我们跑

```
mvn package
```

构建应该成功，并在*target/bigtable-change-key-bundled-0.1-snapshot . jar*中创建一个包

我们现在可以使用本地运行器运行管道:

```
JAR_LOC=target/bigtable-change-key-bundled-0.1-SNAPSHOT.jar

INPUT_TABLE=taxis
OUTPUT_TABLE=taxis_newkey

RUNNER=DirectRunner

java -cp ${JAR_LOC} com.google.cloud.pso.pipeline.BigtableChangeKey \
        --runner=${RUNNER} \
        --project=my-fake-project \
        --bigtableInstance=my-fake-instance \
        --inputTable=${INPUT_TABLE} \
        --outputTable=${OUTPUT_TABLE}
```

运行管道后，我们可以检查命令`cbt count taxis_newkey`现在返回 3。让我们比较输入表`taxis`和输出表`taxis_newkey`中的元素。

如果我们执行`cbt read taxis`,我们将获得三条记录，为了简洁起见，我在这里只包括一条:

```
----------------------------------------
**33cb2a42-d9f5–4b64–9e8a-b5aa1d6e142f#132**
 **id:point_idx**                        @ 2020/05/31–01:35:26.865000
 “132”
 **id:ride_id**                          @ 2020/05/31–01:35:25.198000
 “33cb2a42-d9f5–4b64–9e8a-b5aa1d6e142f”
 **loc:latitude**                        @ 2020/05/31–01:35:28.591000
 “41.7854”
```

所以你可以看到，这个键实际上是由`ride_id`和`point_idx`串联而成，由`#`分隔。

现在执行`cbt read taxis_newkey`并尝试找到具有相同`ride_id`和相同`point_idx`的记录:

```
----------------------------------------
**231#f241e6d1aa5b-a8e9–46b4–5f9d-24a2bc33**
 **id:point_idx**                          @ 2020/05/31–01:35:26.865000
 “132”
 **id:ride_id**                            @ 2020/05/31–01:35:25.198000
 “33cb2a42-d9f5–4b64–9e8a-b5aa1d6e142f”
 **loc:latitude**                          @ 2020/05/31–01:35:28.591000
 “41.7854”
```

你注意到什么了吗？该行的键现在是原始表中键的反转:`33cb2....132`是原始键，`231....2bc33`是新键。如果你检查其他记录，你会注意到钥匙被颠倒了。

现在回到`transformKey`函数的代码，尝试编写不同的键。使用相同的模式创建一个新的输出表，重新编译并再次运行本地管道，以查看代码的效果。

**那么如果你需要改变一个有 GBs 或者 TBs 数据的表的键呢？**在[数据流](https://cloud.google.com/dataflow)中运行此管道，创建一个副本表，但使用一个新的键。只是不要浪费云资源来检查新的 transform key 函数是否按预期工作。首先利用 Bigtable 模拟器和 Apache Beam Direct Runner 在本地运行管道，并确保在激发数据流中的数百个工人之前生成正确的密钥。