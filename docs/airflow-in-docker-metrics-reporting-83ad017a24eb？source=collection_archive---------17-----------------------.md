# Docker 指标报告中的气流

> 原文：<https://towardsdatascience.com/airflow-in-docker-metrics-reporting-83ad017a24eb?source=collection_archive---------17----------------------->

## 在官方的 Apache 气流图像上使用 Grafana 来监控队列健康状况等等。

一个令人不安但可能熟悉的情况:您成功地部署了 Airflow，但发现自己不断刷新 webserver UI 以确保一切顺利运行。

您依赖某些警报任务在上游故障时执行，但是如果队列已满并且任务停止，您将如何得到通知？

一个解决方案是:在 Airflow 上部署 Grafana，一个开源的报告服务。

![](img/ca323d54465af7ac0a820df27d5929f1.png)

由[马库斯·斯皮斯克](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

## 提议的架构

![](img/7f683a6738074e2884f0df8ae9dcb4cf.png)

作者图片

首先，我假设对使用 Docker 和 Docker Compose 的气流功能和容器化有基本的了解。更多资源可以在[这里找到气流](https://airflow.apache.org/docs/stable/concepts.html)，在[这里找到对接](https://docs.docker.com/get-started/overview/)，在[这里找到对接合成](https://docs.docker.com/compose/)。

参考代码跟随:[https://github.com/sarahmk125/airflow-docker-metrics](https://github.com/sarahmk125/airflow-docker-metrics)

现在，有趣的事情。

# 用过的服务

为了将气流度量放入支持警报的视觉上吸引人的仪表板中，在`docker-compose.yml`文件中声明的 Docker 容器中增加了以下服务:

*   **Airflow:** Airflow 在 Dag 中运行任务，这些任务在存储于`./dags/`文件夹中的 Python 文件中定义。已经有一个示例 DAG 声明文件。运行多个容器，特别是使用官方`apache/airflow`图像的细微差别。稍后会详细介绍。
*   **StatsD-Exporter**:StatsD-Exporter 容器将 StatsD 格式的气流度量转换为 Prometheus 格式，即报告层(Grafana)的数据源。有关 StatsD-Exporter 的更多信息，请点击查看[。容器定义包括启动时要执行的命令，定义如何使用公开的端口。](https://github.com/prometheus/statsd_exporter)

```
statsd-exporter:
  image: prom/statsd-exporter
  container_name: airflow-statsd-exporter
  command: "--statsd.listen-udp=:8125 --web.listen-address=:9102"
  ports:
    - 9123:9102
    - 8125:8125/udp
```

*   Prometheus : Prometheus 是一个常用于时间序列数据报告的服务。由于 Prometheus 是一个受支持的数据源，所以在使用 Grafana 作为报告 UI 时特别方便。更多关于普罗米修斯的信息请点击查看[。容器定义中装载的卷指示数据如何流入/流出 Prometheus。](https://prometheus.io/)

```
prometheus:
  image: prom/prometheus
  container_name: airflow-prometheus
  user: "0"
  ports:
    - 9090:9090
  volumes:
    - ./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
    - ./prometheus/volume:/prometheus
```

*   Grafana 是一个报告 UI 服务，通常用于连接非关系数据库。在中描述的[代码中，Grafana 使用 Prometheus 作为仪表板的数据源。容器定义包括门户的管理员用户，以及定义已经预先配置的数据源和仪表板的卷。](https://github.com/sarahmk125/airflow-docker-metrics)

```
grafana:
  image: grafana/grafana:7.1.5
  container_name: airflow-grafana
  environment:
    GF_SECURITY_ADMIN_USER: admin
    GF_SECURITY_ADMIN_PASSWORD: password
    GF_PATHS_PROVISIONING: /grafana/provisioning
  ports:
    - 3000:3000
  volumes:
    - ./grafana/volume/data:/grafana
    - ./grafana/volume/datasources:/grafana/datasources
    - ./grafana/volume/dashboards:/grafana/dashboards
    - ./grafana/volume/provisioning:/grafana/provisioning
```

# 让它走

要启动一切，需要以下工具:Docker、docker-compose、Python3、Git。

**步骤(在终端中运行):**

*   克隆存储库:`git clone [https://github.com/sarahmk125/airflow-docker-metrics.git](https://github.com/sarahmk125/airflow-docker-metrics.git)`
*   导航到克隆的文件夹:`cd airflow-docker-metrics`
*   启动容器:`docker-compose -f docker-compose.yml up -d`(注意:除了最后分别用`stop`或`down`外，可以通过运行相同的命令来停止或移除容器)

**结果:**

*   air flow web server UI:[http://localhost:8080](http://localhost:8080)

![](img/f1e50e55272b5e6ca2fccfa75d3c7db5.png)

作者图片

*   StatsD 指标列表:[http://localhost:9123/metrics](http://localhost:9123/metrics)

![](img/c3c3caceb4a256907e4eaff5d7336b80.png)

作者图片

*   普罗米修斯: [http://localhost:9090](http://localhost:9090)

![](img/6bbd35efa989269487fe186a60dcb1d1.png)

作者图片

*   grafana:[http://localhost:3000](http://localhost:3000)(登录:username= *admin* ，password =*password*)
    该存储库包括一个气流指标仪表板，可以设置警报，显示一段时间内正在运行和排队的任务数量:

![](img/35cd5ba68df2b0a1a9503989dd2af164.png)

作者图片

# 解释的步骤

## 普罗米修斯实际上是如何得到度量的？

Prometheus 在启动时配置在作为卷挂载的`./prometheus/prometheus.yml`文件中:

```
global:
  scrape_interval: 30s
  evaluation_interval: 30s
  scrape_timeout: 10s
  external_labels:
    monitor: 'codelab-monitor'scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['airflow-prometheus:9090']

  - job_name: 'statsd-exporter'
    static_configs:
      - targets: ['airflow-statsd-exporter:9102']

    tls_config:
      insecure_skip_verify: true
```

特别是，`scrape_configs`部分声明了要抓取的目的地(`airflow-prometheus`容器)和源(`airflow-statsd-exporter`容器)。

## 如何在 Grafana 中创建仪表板和警报？

供应是您的朋友！

Grafana 中的供应意味着使用代码来定义数据源、仪表板和启动时存在的警报。当启动容器时，在[localhost:3000/data sources](http://localhost:3000/datasources)中已经配置了一个`Prometheus`数据源，并且在[localhost:3000/dashboards](http://localhost:3000/dashboards)中列出了一个`Airflow Metrics`仪表板。

**如何调配:**

*   所有相关数据都作为卷安装到在`docker-compose.yml`文件中定义的`grafana`容器中(如上所述)
*   `./grafana/volume/provisioning/datasources/default.yaml`文件包含所有数据源的定义:

```
apiVersion: 1
datasources:
  - name: Prometheus
    type: prometheus
    access: proxy
    url: http://prometheus:9090
```

*   `./grafana/volume/provisioning/dashboards/default.yaml`文件包含关于在容器中何处安装仪表板的信息:

```
apiVersion: 1
providers:
  - name: dashboards
    folder: General
    type: file
    editable: true
    updateIntervalSeconds: 10
    allowUiUpdates: true
    options:
      path: /grafana/dashboards
      foldersFromFilesStructure: true
```

*   `./grafana/volume/dashboards/`文件夹包含`.json`文件，每个文件代表一个仪表板。 [airflow_metrics.json](https://github.com/sarahmk125/airflow-docker-metrics/blob/master/grafana/volume/dashboards/airflow_metrics.json) 文件在上面显示的仪表板中产生结果。

按照这些[指令](https://grafana.com/docs/grafana/latest/reference/dashboard/)可以从 Grafana UI 中检索 JSON。

UI 中的警报可以按照这里的[所述进行设置；这里还有一篇优秀的中型文章](https://grafana.com/docs/grafana/latest/alerting/create-alerts/)[关于设置 Grafana alerting with Slack 的](https://medium.com/@_oleksii_/grafana-alerting-and-slack-notifications-3affe9d5f688)。可以采用与仪表板和数据源相同的方式提供警报。

# 额外话题:官方气流图像

在有官方 Docker 形象之前，[Matthieu“puck El _”rois il](https://github.com/puckel/docker-airflow)发布了 Docker 对气流的支持。从 Airflow 版本 1.10.10 开始，Apache 软件基金会在 DockerHub 上发布了一个[官方图像，这是唯一一个当前持续更新的图像。然而，许多人仍然依赖传统的和非官方的库。](https://hub.docker.com/r/apache/airflow/tags)

为什么这是一个问题？好吧，依赖遗留库意味着在版本 1.10.9 中限制气流。Airflow 1.10.10 开始支持一些很酷的功能，比如在 Kubernetes 上运行任务。官方仓库也将是即将到来的(和高度期待的)气流 2.0 将被释放。

在描述的仓库中为`webserver`找到的新的`docker-compose`声明看起来像这样:

```
webserver:
  container_name: airflow-webserver
  image: apache/airflow:1.10.12-python3.7
  restart: always
  depends_on:
    - postgres
    - redis
    - statsd-exporter
  environment:
    - LOAD_EX=n
    - EXECUTOR=Local
    - POSTGRES_USER=airflow
    - POSTGRES_PASSWORD=airflow
    - POSTGRES_DB=airflow
    - AIRFLOW__SCHEDULER__STATSD_ON=True
    - AIRFLOW__SCHEDULER__STATSD_HOST=statsd-exporter
    - AIRFLOW__SCHEDULER__STATSD_PORT=8125
    - AIRFLOW__SCHEDULER__STATSD_PREFIX=airflow
    -AIRFLOW__CORE__SQL_ALCHEMY_CONN= postgresql+psycopg2://airflow:airflow@postgres:5432/airflow
    -AIRFLOW__CORE__FERNET_KEY= pMrhjIcqUNHMYRk_ZOBmMptWR6o1DahCXCKn5lEMpzM=
    - AIRFLOW__CORE__EXECUTOR=LocalExecutor
    - AIRFLOW__CORE__AIRFLOW_HOME=/opt/airflow/
    - AIRFLOW__CORE__LOAD_EXAMPLES=False
    - AIRFLOW__CORE__LOAD_DEFAULT_CONNECTIONS=False
    - AIRFLOW__WEBSERVER__WORKERS=2
    - AIRFLOW__WEBSERVER__WORKER_REFRESH_INTERVAL=1800
  volumes:
    - ./dags:/opt/airflow/dags
  ports:
    - "8080:8080"
  command: bash -c "airflow initdb && airflow webserver"
  healthcheck:
    test: ["CMD-SHELL", "[ -f /opt/airflow/airflow-webserver.pid ]"]
    interval: 30s
    timeout: 30s
    retries: 3
```

从`puckel/docker-airflow`配置的一些变化来突出:

*   以前在`airflow.cfg`文件中发现的自定义参数，如`AIRFLOW__CORE__SQL_ALCHEMY_CONN` ，现在被声明为`docker-compose`文件中的环境变量。
*   初始化后端数据库的`airflow initdb`命令现在在`docker-compose`文件中被声明为一个命令，而不是一个入口点脚本。

# 瞧啊。

这就是了。再也不用担心你的任务被无限地排队而没有运行。气流在 Docker 中运行，Grafana 中的仪表盘和警报触手可及。相同的架构可以在部署在 GCP 或 AWS 的实例上运行，以实现全天候监控，就像在本地运行一样。

成品可以在这里找到:[https://github.com/sarahmk125/airflow-docker-metrics](https://github.com/sarahmk125/airflow-docker-metrics)

需要注意的是，总有改进的空间:

*   此监视设置不会捕获容器或实例故障；需要一个单独的或扩展的解决方案来在容器或实例级别进行监控。
*   当前代码使用 LocalExecutor 运行，这对于大型工作负载来说不太理想。可以用 CeleryExecutor 做进一步的测试。
*   在 StatsD 中还有许多没有突出显示的可用指标(例如 DAG 或任务持续时间、任务失败次数等。).可以在 Grafana 中构建和提供更多的仪表板，以利用所有相关的指标。
*   最后，本文重点关注自托管(或高度可配置的云)气流部署，但这不是部署气流的唯一选择。

# 有问题吗？评论？

感谢阅读！我喜欢谈论数据堆栈。[给我发个消息](https://www.linkedin.com/in/sarah-krasnik/)。