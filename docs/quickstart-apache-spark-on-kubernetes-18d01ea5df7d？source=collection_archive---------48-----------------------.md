# 快速入门:Kubernetes 上的 Apache Spark

> 原文：<https://towardsdatascience.com/quickstart-apache-spark-on-kubernetes-18d01ea5df7d?source=collection_archive---------48----------------------->

## 使用 Kubernetes 的本地 Apache Spark 操作器，让您的大负载平稳运行

![](img/b08ef9ae33760d99a5af47cc33adc8f4.png)

[金赛](https://unsplash.com/@finalhugh?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 介绍

## Kubernetes 的 Apache Spark 操作员

自 2014 年由谷歌推出以来，Kubernetes 与 Docker 本身一起获得了很大的人气，自 2016 年以来，它已成为事实上的容器编排者，并成为市场标准。在**所有***主要云*中提供云管理版本。[【1】](https://cloud.google.com/kubernetes-engine/)[【2】](https://aws.amazon.com/eks/)[【3】](https://docs.microsoft.com/en-us/azure/aks/)(包括[数字海洋](https://www.digitalocean.com/products/kubernetes/)[阿里巴巴](https://www.alibabacloud.com/product/kubernetes))。

随着这种流行，出现了 orchestrator 的各种实现和*用例*，其中包括使用容器的[有状态应用](https://kubernetes.io/docs/tutorials/stateful-application/)和[数据库的执行。](https://vitess.io/zh/docs/get-started/kubernetes/)

托管编排数据库的动机是什么？这是一个很好的问题。但是让我们关注一下在 Kubernetes 上运行工作负载的 [Spark 操作符](https://github.com/GoogleCloudPlatform/spark-on-k8s-operator/blob/master/docs/design.md)。

一个本地 Spark 运营商[的想法在 2016 年](https://github.com/kubernetes/kubernetes/issues/34377)出现，在此之前你不能本地运行 Spark 作业，除非是一些*的 hacky 替代品*，比如[在 Kubernetes 内部运行 Apache Zeppelin](https://kubernetes.io/blog/2016/03/using-spark-and-zeppelin-to-process-big-data-on-kubernetes/) 或者在 Kubernetes 内部创建你的 [Apache Spark 集群(来自 GitHub 上的官方 Kubernetes 组织)](https://github.com/kubernetes/examples/tree/master/staging/spark)引用独立模式下的[Spark workers](http://spark.apache.org/docs/latest/spark-standalone.html)。

然而，本地执行更有意思的是利用负责分配资源的 Kubernetes 调度器，提供弹性和更简单的接口来管理 Apache Spark 工作负载。

考虑到这一点， [Apache Spark Operator 开发得到关注](https://issues.apache.org/jira/browse/SPARK-18278)，合并发布为 [Spark 版本 2.3.0](https://spark.apache.org/releases/spark-release-2-3-0.html) 于 2018 年[2 月](https://spark.apache.org/news/index.html)推出。

如果你渴望阅读更多关于 Apache Spark 提案的内容，你可以前往 Google Docs 上发布的[设计文档。](https://docs.google.com/document/d/1_bBzOZ8rKiOSjQg78DXOA3ZBIo_KkDJjqxVuq0yXdew/edit#heading=h.9bhogel14x0y)

## 为什么是 Kubernetes？

由于公司目前正在寻求通过广为流传的数字化转型来重塑自己，以提高竞争力，最重要的是，在日益活跃的市场中生存下来，因此常见的方法包括大数据、人工智能和云计算[【1】](https://www.zdnet.com/article/how-to-use-cloud-computing-and-big-data-to-support-digital-transformation/)[【2】](https://digitalhealth.london/cloud-big-data-ai-lead-nhs-digital-transformation/)[【3】](https://www.ibm.com/blogs/cloud-computing/2018/11/05/guiding-framework-digital-transformation-garage/)。

关于在大数据环境中使用云计算而不是本地服务器的优势的有趣对比，可以在 [Databricks 博客](https://databricks.com/blog/2017/05/31/top-5-reasons-for-choosing-s3-over-hdfs.html)上阅读，该博客是由 Apache Spark 的创建者创建的[公司。](https://www.washingtonpost.com/news/the-switch/wp/2016/06/09/this-is-where-the-real-action-in-artificial-intelligence-takes-place/)

随着我们看到云计算的广泛采用(即使是那些能够负担得起硬件并在本地运行的公司)，我们注意到大多数云实施都没有 [Apache Hadoop](https://hadoop.apache.org/) ，因为数据团队(BI/数据科学/分析)越来越多地选择使用像 [Google BigQuery](https://cloud.google.com/bigquery/) 或 [AWS Redshift](https://aws.amazon.com/redshift/) 这样的工具。因此，仅仅为了使用 [YARN](https://hortonworks.com/apache/yarn/) 作为资源管理器而加速运行 Hadoop 是没有意义的。

另一种方法是使用 Hadoop 集群提供者，如 [Google DataProc](https://cloud.google.com/dataproc) 或 [AWS EMR](https://aws.amazon.com/emr/) 来创建临时集群。仅举几个例子。

为了更好地理解 Spark Operator 的设计，GitHub 上来自 [GCP 的 doc 是显而易见的。](https://github.com/GoogleCloudPlatform/spark-on-k8s-operatoR/blob/master/docs/design.md#the-crd-controller)

# 让我们动手吧！

## 预热发动机

既然话已经传开了，那就让我们把它弄到手，展示一下发动机的运转吧。为此，让我们使用:

一旦安装了必要的工具，就有必要在`PATH`环境变量中包含 Apache Spark path，以简化 Apache Spark 可执行文件的调用。只需运行:

```
export PATH=${PATH}:/path/to/apache-spark-X.Y.Z/bin
```

## 创建 Minikube“集群”

最后，为了拥有一个 Kubernetes“集群”,我们将启动一个`minikube`,目的是运行一个来自 [Spark 库](https://github.com/apache/spark/blob/master/examples/src/main/scala/org/apache/spark/examples/SparkPi.scala)的示例，名为`SparkPi`,作为演示。

```
minikube start --cpus=2 \
  --memory=4g
```

## 建立码头工人形象

让我们使用 Minikube Docker 守护进程来不依赖于外部注册表(并且只在 VM 上生成 Docker 映像层，便于以后的垃圾处理)。Minikube 有一个让我们生活更轻松的包装:

```
eval $(minikube docker-env)
```

配置完守护进程环境变量后，我们需要一个 Docker 映像来运行作业。Spark 仓库中有一个 [shell 脚本来帮助解决这个问题。考虑到我们的`PATH`已经正确配置，只需运行:](https://github.com/apache/spark/blob/master/bin/docker-image-tool.sh)

```
docker-image-tool.sh -m -t latest build
```

*参考消息:*这里的`-m`参数表示一个迷你库的构建。

让我们快速执行 SparkPi，使用与 Hadoop Spark 集群 [spark-submit](https://spark.apache.org/docs/latest/submitting-applications.html) 相同的命令。

然而，Spark Operator 支持使用 [CRD](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) 、[用“Kubernetes 方言”定义作业，下面是一些例子](https://github.com/GoogleCloudPlatform/spark-on-k8s-operator/tree/master/examples)——稍后介绍。

# 向洞里开火！

中间是 Scala 版本和*的差距。jar* 当您使用 Apache Spark 版本进行参数化时:

```
spark-submit --master k8s://https://$(minikube ip):8443 \
    --deploy-mode cluster \
    --name spark-pi \
    --class org.apache.spark.examples.SparkPi \
    --conf spark.executor.instances=2 \
    --executor-memory 1024m \
    --conf spark.kubernetes.container.image=spark:latest \
    local:///opt/spark/examples/jars/spark-examples_2.11-X.Y.Z.jar # here
```

新的是:

*   `--master`:在 URL 中接受前缀`k8s://`，用于 Kubernetes 主 API 端点，由命令`https://$(minikube ip):8443`公开。BTW，万一你想知道，这是一个 shell 命令替换；
*   `--conf spark.kubernetes.container.image=`:配置 Docker 镜像在 Kubernetes 中运行。

样本输出:

```
...19/08/22 11:59:09 INFO LoggingPodStatusWatcherImpl: State changed,
new state: pod name: spark-pi-1566485909677-driver namespace: default
labels: spark-app-selector -> spark-20477e803e7648a59e9bcd37394f7f60,
spark-role -> driver pod uid: c789c4d2-27c4-45ce-ba10-539940cccb8d
creation time: 2019-08-22T14:58:30Z service account name: default
volumes: spark-local-dir-1, spark-conf-volume, default-token-tj7jn
node name: minikube start time: 2019-08-22T14:58:30Z container
images: spark:docker phase: Succeeded status:
[ContainerStatus(containerID=docker://e044d944d2ebee2855cd2b993c62025d
6406258ef247648a5902bf6ac09801cc, image=spark:docker,
imageID=docker://sha256:86649110778a10aa5d6997d1e3d556b35454e9657978f3
a87de32c21787ff82f, lastState=ContainerState(running=null,
terminated=null, waiting=null, additionalProperties={}),
name=spark-kubernetes-driver, ready=false, restartCount=0,
state=ContainerState(running=null,
terminated=ContainerStateTerminated(containerID=docker://e044d944d2ebe
e2855cd2b993c62025d6406258ef247648a5902bf6ac09801cc, exitCode=0,
finishedAt=2019-08-22T14:59:08Z, message=null, reason=Completed,
signal=null, startedAt=2019-08-22T14:58:32Z,
additionalProperties={}), waiting=null, additionalProperties={}),
additionalProperties={})]19/08/22 11:59:09 INFO LoggingPodStatusWatcherImpl: Container final
statuses: Container name: spark-kubernetes-driver Container image:
spark:docker Container state: Terminated Exit code: 0
```

为了查看作业结果(以及整个执行过程),我们可以运行一个`kubectl logs`,将驱动程序 pod 的名称作为参数传递:

```
kubectl logs $(kubectl get pods | grep 'spark-pi.*-driver')
```

这将产生输出(省略了一些条目)，类似于:

```
...
19/08/22 14:59:08 INFO TaskSetManager: Finished task 1.0 in stage 0.0
(TID 1) in 52 ms on 172.17.0.7 (executor 1) (2/2)
19/08/22 14:59:08 INFO TaskSchedulerImpl: Removed TaskSet 0.0, whose
tasks have all completed, from pool19/08/22 14:59:08 INFO
DAGScheduler: ResultStage 0 (reduce at SparkPi.scala:38) finished in
0.957 s
19/08/22 14:59:08 INFO DAGScheduler: Job 0 finished: reduce at
SparkPi.scala:38, took 1.040608 s Pi is roughly 3.138915694578473
19/08/22 14:59:08 INFO SparkUI: Stopped Spark web UI at
[http://spark-pi-1566485909677-driver-svc.default.svc:4040](http://spark-pi-1566485909677-driver-svc.default.svc:4040)
19/08/22 14:59:08 INFO KubernetesClusterSchedulerBackend: Shutting
down all executors
19/08/22 14:59:08 INFO
KubernetesClusterSchedulerBackend$KubernetesDriverEndpoint: Asking
each executor to shut down
19/08/22 14:59:08 WARN ExecutorPodsWatchSnapshotSource: Kubernetes
client has been closed (this is expected if the application is
shutting down.)
19/08/22 14:59:08 INFO MapOutputTrackerMasterEndpoint:
MapOutputTrackerMasterEndpoint stopped!
19/08/22 14:59:08 INFO MemoryStore: MemoryStore cleared
19/08/22 14:59:08 INFO BlockManager: BlockManager stopped
19/08/22 14:59:08 INFO BlockManagerMaster: BlockManagerMaster stopped
19/08/22 14:59:08 INFO
OutputCommitCoordinator$OutputCommitCoordinatorEndpoint:
OutputCommitCoordinator stopped!
19/08/22 14:59:08 INFO SparkContext: Successfully stopped SparkContext
19/08/22 14:59:08 INFO ShutdownHookManager: Shutdown hook called
19/08/22 14:59:08 INFO ShutdownHookManager: Deleting directory
/tmp/spark-aeadc6ba-36aa-4b7e-8c74-53aa48c3c9b2
19/08/22 14:59:08 INFO ShutdownHookManager: Deleting directory
/var/data/spark-084e8326-c8ce-4042-a2ed-75c1eb80414a/spark-ef8117bf-90
d0-4a0d-9cab-f36a7bb18910
...
```

结果出现在:

```
19/08/22 14:59:08 INFO DAGScheduler: Job 0 finished: reduce at SparkPi.scala:38, took 1.040608 s Pi is roughly 3.138915694578473
```

最后，让我们删除 Minikube 生成的 VM，以清理环境(除非您想继续玩它):

```
minikube delete
```

# 临终遗言

我希望您的好奇心得到了*的激发*，并且为您的大数据工作负载提出了一些进一步开发的想法。如果你有任何疑问或建议，请在评论区分享。

*原载于 2020 年 5 月 21 日*[*https://ma Cunha . me*](https://macunha.me/en/post/2020/05/quickstart-apache-spark-on-kubernetes/)*。*