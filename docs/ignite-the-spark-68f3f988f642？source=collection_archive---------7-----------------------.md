# 点燃火花！

> 原文：<https://towardsdatascience.com/ignite-the-spark-68f3f988f642?source=collection_archive---------7----------------------->

## 使用 PySpark 在 Kubernetes 上运行 Apache Spark

![](img/7c08466d571ed02634f52c28f3b2abac.png)

来源:免费图片来自 Pixabay

# **简介**

在这本初级读本中，您将首先了解一些 Apache Spark 的集群管理器是如何工作的，然后了解如何在现有的 Kubernetes (k8s)集群上以交互方式在 Jupyter 笔记本中运行 PySpark。

完成本文后，您应该能够自信地在任何 Kubernetes 集群上开发 Spark 应用程序，并对两者之间的联系有更深的理解。

# 这都是关于环境的

![](img/afde587281ce3d826aa7b549a74dc53b.png)

来源:Apache 文档

Spark 是一个快速通用的**集群计算系统** ，这意味着根据定义，计算是以分布式方式在许多互连的节点上共享的。

但是 Spark 实际上是如何在集群中分配给定的工作负载的呢？

Spark 采用主/从方式，其中一个**驱动程序**(“主”)创建一个 **SparkContext** 对象，该对象连接到一个**集群管理器**。

创建的 SparkContext(由您喜欢的语言绑定以编程方式创建，或者在您提交作业时以您的名义创建)将集群抽象为一个大型计算节点，您的应用程序使用它来执行工作。

另一方面，**集群管理器**负责生成和管理大量**工作节点**(“从节点”)，每个节点代表 SparkContext 运行一个**执行器进程**来完成实际工作。

当 Spark 应用程序操作**弹性分布式数据帧(RDD)** 形式的数据时，RDD 被分割成多个分区，分布在这些工作节点/执行器组合上进行处理。最终结果在节点间汇总并发送回驱动程序。

# 选择一个管理器，任何集群管理器

这种设计的一个主要优点是集群管理器与应用程序是分离的，因此可以互换。

传统上，Spark 支持三种类型的集群管理器:

*   **独立**
*   **阿帕奇 Mesos**
*   **Hadoop YARN**

[独立](https://spark.apache.org/docs/latest/spark-standalone.html)集群管理器是默认的集群管理器，随 Spark 的每个版本一起提供。这是一个没有虚饰，有能力的经理，这意味着让你尽快启动和运行。

Apache Mesos 本身就是一种集群技术，旨在将集群的所有资源抽象化，就像一台大型计算机一样。Mesos 附带了一个集群管理器，您可以在 Spark 中利用它。

[Hadoop YARN](https://spark.apache.org/docs/latest/running-on-yarn.html) (“又一个资源谈判者”)是作为 Apache Hadoop 项目的副产品开发的，主要专注于分发 MapReduce 工作负载。因此，它也是 Spark 可以与之进行本地对话的集群管理器。

# 进入 Kubernetes

![](img/3aef08845c50891b3a469fa52d14ee6d.png)

来源:Apache 文档

从 2.3.0 开始，Spark 现在支持直接使用 Kubernetes 作为集群管理器。

但是那个*到底是什么意思呢？*

这取决于你想如何在 Kubernetes 上运行 Spark。

在**集群模式**中，在您使用 **spark-submit** 提交应用程序之后，代表您的应用程序创建的 SparkContext 将要求 **kube-apiserver** 设置一个驱动程序节点和一些相应的工作节点(Pods ),并在它们之上运行您的工作负载。

一旦所有数据处理完成，在拆除时，临时工作节点窗格将自动终止，但驱动程序节点窗格将保留，以便您可以在手动删除它之前检查任何日志。

在**客户端模式**中，您创建 Spark driver 节点作为 Pod，然后在最终提交工作之前，使用您最喜欢的语言的 API 绑定创建 SparkContext。

出于几个原因，本初级读本将着重于配置客户端模式:

*   web 上的大多数文档都是以集群模式提供的
*   使用 spark-submit 提交应用程序的步骤与使用现有的集群管理器没有什么不同
*   大多数 Spark 开发人员都希望使用客户机模式，这样他们就可以开发新的应用程序并交互式地探索数据

# 先决条件

以下说明假设如下:

*   您对 Kubernetes 集群或该集群中的专用名称空间拥有管理权限

我正在运行一个小型的 1.17.2 八节点集群，它是使用 [kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/) 工具安装的。我相信任何集群 1.15+都应该工作得很好。

如果你没有集群，我强烈建议在你的桌面上安装 [MiniKube](https://kubernetes.io/docs/tasks/tools/install-minikube/) 作为一种跟进方式。

*   您对 Kubernetes 的核心概念有些熟悉，比如 pod、部署和服务
*   您已经安装了 Docker，理解了容器背后的概念，并且能够轻松地使用 Docker 文件构建自定义映像
*   您可以访问内部、云中或外部的 Docker 注册中心，如 [Docker Hub](https://hub.docker.com/) 。
*   在撰写本文时，您使用的是 Spark 的最新稳定版本 v2.4.4

# 集群设置

在我们开始构建映像和部署 Pods 之前，让我们设置一个专用的名称空间，我们的 combo Jupyter notebook/Spark driver 节点以及所有对应的 worker 节点都将部署在这个名称空间中。

```
$ kubectl create namespace spark
namespace/spark created
```

为了让你的驱动程序 Pod 从集群中请求资源，它必须使用某种类型的**服务帐户**，该帐户拥有正确的**角色基础访问控制(RBAC)** 。

Spark 文档建议创建一个**角色绑定**或**集群角色绑定**来完成这个任务。选择实现哪种方法完全取决于集群的安全策略。

对于本例，我们将在 **spark 名称空间** 中创建一个名为 **spark** 的 ServiceAccount，并使用 ClusterRoleBinding 赋予该帐户适当的权限:

```
$ kubectl create serviceaccount spark -n spark
serviceaccount/spark created$ kubectl create clusterrolebinding spark-role --clusterrole=edit --serviceaccount=spark:spark --namespace=spark
clusterrolebinding.rbac.authorization.k8s.io/spark-role created
```

# Docker 设置

如上所述，由于每个 worker 节点实际上只是我们集群中的一个 Pod，所以我们需要一个 docker 映像和正确的 Spark 运行时。

幸运的是，Spark 团队提供了一组 **Dockerfiles** 和一个工具( **docker-image-tool.sh** )来创建映像，这些映像可以部署在任何 Kubernetes 集群上。

每个 docker 文件都要与 Spark 支持的三种主要语言绑定一起使用——Scala、Python 和 r。

让我们构建默认图像，这样我们至少有一些基础图像可以使用:

```
$ wget -qO- [http://mirrors.gigenet.com/apache/spark/spark-2.4.4/spark-2.4.4-bin-hadoop2.7.tgz](http://mirrors.gigenet.com/apache/spark/spark-2.4.4/spark-2.4.4-bin-hadoop2.7.tgz) | tar -xzf -$ cd spark-2.4.4-bin-hadoop2.7 && bin/docker-image-tool.sh build
Sending build context to Docker daemon  262.2MB
...
Successfully built 02fb36ac3ee0
Successfully tagged spark:latest
Sending build context to Docker daemon  262.2MB
...
Successfully built 0d33be47d094
Successfully tagged spark-py:latest
Sending build context to Docker daemon  262.2MB
...
Successfully built dc911ac3678e
Successfully tagged spark-r:latest
```

docker-image-tool.sh 脚本完成后，您应该有三个新的映像准备好部署在 Kubernetes 集群上:

```
$ docker images spark:latest
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
spark               latest              02fb36ac3ee0        7 days ago          344MB$ docker images spark-py:latest
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
spark-py            latest              0d33be47d094        7 days ago          432MB$ $ docker images spark-r:latest
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
spark-r             latest              dc911ac3678e        7 days ago          539MB
```

由于本教程将集中使用 PySpark，我们将为我们的 worker Pod 使用 **spark-py** 图像。通常，您只需将这些映像推送到集群使用的任何 docker 注册表中。但是，我们将创建它们的自定义版本，以便解决一个 bug。

# 通信故障

在撰写本文时，由于最近修补了一个安全 CVE，在较新的集群上使用现有的 Spark v2.4.4 Kubernetes 客户端 jar 文件存在问题。

你可以在这里和[这里](https://stackoverflow.com/questions/57643079/kubernetes-watchconnectionmanager-exec-failure-http-403)阅读更多关于这个问题的信息。

解决方法是构建一个启用 Spark 的 docker 映像，但是其中包含较新的客户机 jar 文件。

让我们使用我们刚刚构建的基础映像来实现这一点。因为我们关注的是使用 PySpark，所以我们将只重建 spark-py 映像，但是相同的步骤适用于所有映像。

```
$ mkdir my-spark-py && cd my-spark-py$ wget -qO- [http://apache.mirrors.hoobly.com/spark/spark-3.0.0-preview2/spark-3.0.0-preview2-bin-hadoop2.7.tgz](http://apache.mirrors.hoobly.com/spark/spark-3.0.0-preview2/spark-3.0.0-preview2-bin-hadoop2.7.tgz) | tar -xzf -$ cp spark-3.0.0-preview2-bin-hadoop2.7/jars/kubernetes*jar . && rm -f [spark-3.0.0-preview2/spark-3.0.0-preview2-bin-hadoop2.7.tgz](http://apache.mirrors.hoobly.com/spark/spark-3.0.0-preview2/spark-3.0.0-preview2-bin-hadoop2.7.tgz)$ cat << EOF > Dockerfile
FROM spark-py:v2.4.4COPY *.jar /opt/spark/jars/
RUN rm /opt/spark/jars/kubernetes-*-4.1.2.jar
EOF$ docker build --rm -t my-spark-py:v2.4.4 .
...
Successfully built 29abceec5cb2
Successfully tagged my-spark-py:v2.4.4
```

所以现在 **my-spark-py:v2.4.4** 包含了相同的官方 spark 运行时，但是更新了来自 v3.0.0-preview2 版本的 Kubernetes 客户端 jar 文件。

每当我们生成 SparkContext 时，Kubernetes 会根据需要创建一个 worker Pod，并提取该图像。

# 创建“驱动程序”窗格图像

因为我们的最终目标是在 Jupyter 笔记本中的 Kubernetes 上交互开发 PySpark 应用程序，所以这个 Pod 也将充当我们的“驱动程序”节点。

因此，您有两种选择:

*   使用[官方 Jupyter PySpark 图片](https://hub.docker.com/r/jupyter/pyspark-notebook/)
*   [打造自己的定制形象](https://github.com/pisymbol/docker/blob/master/spark/Dockerfile)

PySpark docker 官方图片是一个很好的起点，我强烈推荐它。

我创建了一个自定义的 docker 图片，你可以点击上面的链接找到它。我把这张图片叫做**我的笔记本:最新的**。

您应该构建、标记和推送您最终选择的任何映像到您的集群使用的 docker 注册表。

# 设置您的部署

正如 Kubernetes 的所有事情一样，我们需要编写一个 YAML 文件来部署我们的驱动程序 Pod，它将用于登录 Jupyter 并创建一个 SparkContext。

以下是我的部署情况:

```
apiVersion: apps/v1
kind: Deployment
metadata:
 **namespace: spark**
  name: my-notebook-deployment
  labels:
    app: my-notebook
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-notebook
  template:
    metadata:
      labels:
        app: my-notebook
    spec:
      **serviceAccountName: spark**
      containers:
      - name: my-notebook
        image: pidocker-docker-registry.default.svc.cluster.local:5000/my-notebook:latest
        ports:
          - containerPort: 8888
        volumeMounts:
          - mountPath: /root/data
            name: my-notebook-pv
        workingDir: /root
        resources:
          limits:
            memory: 2Gi
      volumes:
        - name: my-notebook-pv
          persistentVolumeClaim:
            claimName: my-notebook-pvc
---
apiVersion: v1
kind: Service
metadata:
  namespace: spark
  name: my-notebook-deployment
spec:
  selector:
    app: my-notebook
  **ports:
    - protocol: TCP
      port: 29413
  clusterIP: None**
```

突出显示的文本是您应该注意的指令:

*   我们希望我们的部署存在于 **spark** 名称空间中
*   我们希望将这个部署的 **ServiceAccount** 设置为 **spark** ，它具有我们在上面创建的 ClusterRoleBinding**spark-role**
*   根据 [Spark 客户端模式文档](https://spark.apache.org/docs/latest/running-on-kubernetes.html#client-mode)，我们希望通过将服务的 ClusterIP 设置为 None 来将我们的部署公开为一个无头服务。这允许工人舱通过端口 29413 与司机舱通信。您选择的端口号可以是任何有效的未使用的端口，但是我在这个例子中选择 29413 作为对[这个有用帖子](http://blog.brainlounge.de/memoryleaks/getting-started-with-spark-on-kubernetes/)的敬意。

# 创建部署

当您对部署感到满意时，通过以下方式创建它:

```
$ kubectl create -f k8s/deploy.yaml
deployment.apps/my-notebook-deployment created
service/my-notebook-deployment created
```

检查您的部署是否正在运行:

```
$ kubectl get all -n spark
NAME                                          READY   STATUS    RESTARTS   AGE
pod/my-notebook-deployment-7bf574447c-pdn2q   1/1     Running   0          19sNAME                             TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)     AGE
service/my-notebook-deployment   ClusterIP   None         <none>        29413/TCP   19sNAME                                     READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/my-notebook-deployment   1/1     1            1           19sNAME                                                DESIRED   CURRENT   READY   AGE
replicaset.apps/my-notebook-deployment-7bf574447c   1         1         1       19s
```

现在将端口转发到 8888，这样您就可以登录 Jupyter:

```
$ kubectl port-forward -n spark deployment.apps/my-notebook-deployment 8888:8888
Forwarding from 127.0.0.1:8888 -> 8888
Forwarding from [::1]:8888 -> 8888
```

现在，您应该能够将您最喜欢的浏览器指向 [http://localhost:8888](http://localhost:8888) 并登录 Jupyter(如果您使用我的图像作为参考，密码是**‘Jupyter’**，否则您将需要复制/粘贴在容器的日志文件中显示的生成的 auth token)。

# 点燃它！

启动一个新的 Python3 笔记本，并在单元格中键入以下内容:

```
import osfrom pyspark import SparkContext, SparkConf
from pyspark.sql import SparkSession# Create Spark config for our Kubernetes based cluster manager
sparkConf = SparkConf()
sparkConf.setMaster("**k8s://**[**https://kubernetes.default.svc.cluster.local:443**](https://kubernetes.default.svc.cluster.local:443)**"**)
sparkConf.setAppName(**"spark"**)
sparkConf.set(**"spark.kubernetes.container.image", "pidocker-docker-registry.default.svc.cluster.local:5000/my-spark-py:v2.4.4"**)
sparkConf.set(**"spark.kubernetes.namespace", "spark"**)
sparkConf.set(**"spark.executor.instances", "7"**)
sparkConf.set("spark.executor.cores", "2")
sparkConf.set("spark.driver.memory", "512m")
sparkConf.set("spark.executor.memory", "512m")
sparkConf.set("spark.kubernetes.pyspark.pythonVersion", "3")
sparkConf.set(**"spark.kubernetes.authenticate.driver.serviceAccountName", "spark"**)
sparkConf.set(**"spark.kubernetes.authenticate.serviceAccountName", "spark"**)
sparkConf.set(**"spark.driver.port", "29413**")
sparkConf.set(**"spark.driver.host", "my-notebook-deployment.spark.svc.cluster.local**")# Initialize our Spark cluster, this will actually
# generate the worker nodes.
spark = SparkSession.builder.config(conf=sparkConf).getOrCreate()
sc = spark.sparkContext
```

让我们来分解一下上面的 Spark 配置:

*   我们将主 URL 设置为"**k8s://**[**https://Kubernetes . default . SVC . cluster . local:443**](https://kubernetes.default.svc.cluster.local:443)**"**，这告诉 Spark 我们的集群管理器是 Kubernetes (k8s)并且在哪里可以找到请求资源的 kube-apiserver
*   我们将应用程序名称设置为**“spark”**，它将被添加到 worker Pod 名称的前面。你可以随意使用任何你认为合适的名字
*   我们将 worker Pods 使用的容器映像设置为我们在上面构建的自定义映像，其中包含更新的 Kubernetes 客户端 jar 文件
*   我们指定希望所有工作节点都创建在 **spark** 名称空间中
*   我将工作节点的数量设置为**“7”**，因为我在八个节点上(一个节点是主节点，七个可用于工作)。您需要选择一个对您的集群有意义的数字。
*   我们将驱动程序和工人的服务帐户名称设置为我们在上面创建的服务帐户(**“spark”**)
*   最后，我们将端口号设置为 **29413** ，并将集群内服务的完全限定域名指定给工作节点，以便联系驱动节点。

在执行这个单元之后，您应该会看到 spark 名称空间中产生了许多 worker Pods。

让我们检查一下:

```
$ kubectl get pods -n spark
NAME                                      READY   STATUS    RESTARTS   AGE
my-notebook-deployment-7bf574447c-pdn2q   1/1     Running   0          5m6s
spark-1581043649282-exec-1                1/1     Running   0          76s
spark-1581043651262-exec-2                1/1     Running   0          75s
spark-1581043651392-exec-3                1/1     Running   0          75s
spark-1581043651510-exec-4                1/1     Running   0          75s
spark-1581043651623-exec-5                1/1     Running   0          75s
spark-1581043656753-exec-6                1/1     Running   0          70s
spark-1581043656875-exec-7                1/1     Running   0          70s
```

您现在有一个运行在 Kubernetes 上的 Spark 集群等待工作！

按照传统，让我们在新的单元格中使用我们的集群来计算数字 Pi:

```
from random import random
from operator import addpartitions = 7
n = 10000000 * partitionsdef f(_):
    x = random() * 2 - 1
    y = random() * 2 - 1

    return 1 if x ** 2 + y ** 2 <= 1 else 0count = sc.parallelize(range(1, n + 1), partitions).map(f).reduce(add)
print("Pi is roughly %f" % (4.0 * count / n))
```

您的输出应该是:

```
Pi is roughly 3.141397
```

# 熄灭火花

完成开发后，只需执行以下命令就可以关闭 Spark 集群:

```
sc.stop()
```

这将指示您的 SparkContext 通过 kube-apiserver 拆除上面创建的 worker Pods。

要验证:

```
$ kubectl get pods -n spark
NAME                                      READY   STATUS        RESTARTS   AGE
my-notebook-deployment-7bf574447c-pdn2q   1/1     Running       0          20m
spark-1581043649282-exec-1                0/1     Terminating   0          16m
spark-1581043651262-exec-2                1/1     Terminating   0          16m
spark-1581043651392-exec-3                1/1     Terminating   0          16m
spark-1581043651510-exec-4                0/1     Terminating   0          16m
spark-1581043651623-exec-5                0/1     Terminating   0          16m
spark-1581043656753-exec-6                0/1     Terminating   0          16m
spark-1581043656875-exec-7                0/1     Terminating   0          16m
```

现在您只剩下 spark 名称空间中的部署/驱动程序 Pod。

# 结论

我希望您现在更有信心使用 PySpark 在 Kubernetes 上创建 Spark 集群，并欣赏其背后的架构。

我知道在我早期的 Spark 开发工作中，大部分时间我都是在独立模式下运行的，但是现在有了 Kubernetes 的支持，它要么集群，要么破产！

欢迎在下面发表任何问题、更正或担忧，祝您愉快！