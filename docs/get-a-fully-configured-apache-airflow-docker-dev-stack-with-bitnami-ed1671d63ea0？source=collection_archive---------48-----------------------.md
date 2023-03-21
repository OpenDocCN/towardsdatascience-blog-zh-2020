# 使用 Bitnami 获得完全配置的 Apache Airflow Docker 开发堆栈

> 原文：<https://towardsdatascience.com/get-a-fully-configured-apache-airflow-docker-dev-stack-with-bitnami-ed1671d63ea0?source=collection_archive---------48----------------------->

![](img/f94b63f6041508c71dbc7d541cf1172f.png)

我已经使用它大约两年了，来构建定制的工作流界面，如用于实验室信息管理系统(LIMs)、计算机视觉预处理和后处理管道的界面，以及设置和忘记其他基因组学管道。

我最喜欢的气流特征是，它对你正在做的工作或工作发生的地点完全不可知。它可以发生在本地、Docker 映像上、Kubernetes 上、任意数量的 AWS 服务上、HPC 系统上等等。使用 Airflow 可以让我专注于我试图完成的业务逻辑，而不会在实现细节上陷入太多。

在那段时间里，我采用了一套系统，通过 Docker 和 Docker Compose 快速构建主开发堆栈，使用了[Bitnami Apache air flow stack](https://github.com/bitnami/bitnami-docker-airflow)。一般来说，我要么使用相同的 Docker compose 堆栈(如果它是一个足够小的独立实例)将堆栈部署到生产环境中，要么在我需要与其他服务或文件系统交互时使用 Kubernetes。

# 比特纳米 vs 滚自己的

我曾经用康达卷自己的气流容器。我仍然对我的大多数其他容器使用这种方法，包括与我的气流系统交互的微服务，但是配置气流不仅仅是安装包。此外，即使只是安装这些软件包也是一件痛苦的事情，我很少能指望一个没有痛苦的重建工作。然后，在包的顶部，您需要配置数据库连接和消息队列。

在来了 [Bitnami 阿帕奇气流 docker 组成栈](https://github.com/bitnami/bitnami-docker-airflow)为开发和 [Bitnami 阿帕奇气流舵图](https://bitnami.com/stack/apache-airflow/helm)为生产！

用他们自己的话说:

> *Bitnami 使您可以轻松地在任何平台上安装和运行您最喜爱的开源软件，包括您的笔记本电脑、Kubernetes 和所有主要的云。除了流行的社区产品之外，现已成为 VMware 一部分的 Bitnami 还为 IT 组织提供了一种安全、合规、持续维护且可根据组织策略进行定制的企业产品。*[*https://bitnami.com/*](https://bitnami.com/)

Bitnami 堆栈(通常)从 Docker 构建堆栈到舵图的工作方式完全相同。这意味着我可以使用我的 compose 栈进行本地测试和开发，构建新的映像、版本、包等，然后部署到 Kubernetes。配置、环境变量和其他一切都是一样的。从头开始做这一切将是一项相当大的任务，所以我使用 Bitnami。

他们有大量的企业产品，但这里包含的所有东西都是开源的，不涉及付费墙。

不，我不属于 Bitnami，虽然我的孩子吃得很多，对出卖没有任何特别的道德厌恶。；-)我刚刚发现他们的产品非常棒。

# 项目结构

我喜欢把我的项目组织起来，这样我就可以运行`tree`并且对正在发生的事情有一个大概的了解。

Apache Airflow 有 3 个主要组件，应用程序、工作程序和调度程序。每个都有自己的 Docker 映像来区分服务。此外，还有一个数据库和一个消息队列，但是我们不会对它们进行任何定制。

```
.
└── docker
    └── bitnami-apache-airflow-1.10.10
        ├── airflow
        │   └── Dockerfile
        ├── airflow-scheduler
        │   └── Dockerfile
        ├── airflow-worker
        │   └── Dockerfile
        ├── dags
        │   └── tutorial.py
        ├── docker-compose.yml
```

所以我们这里有一个名为`bitnami-apache-airflow-1.10.10`的目录。这就引出了非常重要的一点！钉住你的版本！它会让你省去那么多的痛苦和挫折！

那么我们有一个 docker 文件每个气流件。

使用以下内容创建此目录结构:

```
mkdir -p docker/bitnami-apache-airflow-1.10.10/{airflow,airflow-scheduler,airflow-worker,dags}
```

# Docker 撰写文件

这是我对`docker-compose.yml`文件的偏好。我根据自己的喜好做了一些更改，主要是我*固定版本*，构建我自己的 docker 映像，我为`dags`、`plugins`和`database backups`安装了卷，并添加了 Docker 插槽，这样我就可以在我的堆栈中运行`DockerOperators`。

你随时可以去抢原文`docker-compose` [这里](https://github.com/bitnami/bitnami-docker-airflow/blob/master/docker-compose.yml)。

```
version: '2'

services:
  postgresql:
    image: 'docker.io/bitnami/postgresql:10-debian-10'
    volumes:
      - 'postgresql_data:/bitnami/postgresql'
    environment:
      - POSTGRESQL_DATABASE=bitnami_airflow
      - POSTGRESQL_USERNAME=bn_airflow
      - POSTGRESQL_PASSWORD=bitnami1
      - ALLOW_EMPTY_PASSWORD=yes
  redis:
    image: docker.io/bitnami/redis:5.0-debian-10
    volumes:
      - 'redis_data:/bitnami'
    environment:
      - ALLOW_EMPTY_PASSWORD=yes
  airflow-scheduler:
#    image: docker.io/bitnami/airflow-scheduler:1-debian-10
    build:
      context: airflow-scheduler
    environment:
      - AIRFLOW_DATABASE_NAME=bitnami_airflow
      - AIRFLOW_DATABASE_USERNAME=bn_airflow
      - AIRFLOW_DATABASE_PASSWORD=bitnami1
      - AIRFLOW_EXECUTOR=CeleryExecutor
      # If you'd like to load the example DAGs change this to yes!
      - AIRFLOW_LOAD_EXAMPLES=no
      # only works with 1.10.11
      #- AIRFLOW__WEBSERVER__RELOAD_ON_PLUGIN_CHANGE=true
      #- AIRFLOW__CORE__DAGS_ARE_PAUSED_AT_CREATION=False
    volumes:
      - airflow_scheduler_data:/bitnami
      - ./plugins:/opt/bitnami/airflow/plugins
      - ./dags:/opt/bitnami/airflow/dags
      - ./db_backups:/opt/bitnami/airflow/db_backups
      - /var/run/docker.sock:/var/run/docker.sock
  airflow-worker:
#    image: docker.io/bitnami/airflow-worker:1-debian-10
    build:
      context: airflow-worker
    environment:
      - AIRFLOW_DATABASE_NAME=bitnami_airflow
      - AIRFLOW_DATABASE_USERNAME=bn_airflow
      - AIRFLOW_DATABASE_PASSWORD=bitnami1
      - AIRFLOW_EXECUTOR=CeleryExecutor
      - AIRFLOW_LOAD_EXAMPLES=no
      # only works with 1.10.11
      #- AIRFLOW__WEBSERVER__RELOAD_ON_PLUGIN_CHANGE=true
      #- AIRFLOW__CORE__DAGS_ARE_PAUSED_AT_CREATION=False
    volumes:
      - airflow_worker_data:/bitnami
      - ./plugins:/opt/bitnami/airflow/plugins
      - ./dags:/opt/bitnami/airflow/dags
      - ./db_backups:/opt/bitnami/airflow/db_backups
      - /var/run/docker.sock:/var/run/docker.sock
  airflow:
#    image: docker.io/bitnami/airflow:1-debian-10
    build:
      # You can also specify the build context
      # as cwd and point to a different Dockerfile
      context: .
      dockerfile: airflow/Dockerfile
    environment:
      - AIRFLOW_DATABASE_NAME=bitnami_airflow
      - AIRFLOW_DATABASE_USERNAME=bn_airflow
      - AIRFLOW_DATABASE_PASSWORD=bitnami1
      - AIRFLOW_EXECUTOR=CeleryExecutor
      - AIRFLOW_LOAD_EXAMPLES=no
      # only works with 1.10.11
      #- AIRFLOW__WEBSERVER__RELOAD_ON_PLUGIN_CHANGE=True
      #- AIRFLOW__CORE__DAGS_ARE_PAUSED_AT_CREATION=False
    ports:
      - '8080:8080'
    volumes:
      - airflow_data:/bitnami
      - ./dags:/opt/bitnami/airflow/dags
      - ./plugins:/opt/bitnami/airflow/plugins
      - ./db_backups:/opt/bitnami/airflow/db_backups
      - /var/run/docker.sock:/var/run/docker.sock
volumes:
  airflow_scheduler_data:
    driver: local
  airflow_worker_data:
    driver: local
  airflow_data:
    driver: local
  postgresql_data:
    driver: local
  redis_data:
    driver: local
```

# 锁定您的版本

这里用的阿帕奇气流的版本是`1.10.10`。`1.10.11`有一些我想加入的很酷的更新，所以我会继续关注它！

您可以通过查看主站点上的[变更日志](https://airflow.apache.org/docs/stable/changelog.html)来随时了解最新的 Apache Airflow 版本。

我们正在使用 Bitnami，它的机器人可以在新版本发布时自动构建和更新它们的映像。

虽然这种方法对机器人来说很棒，但我不推荐*仅仅希望最新版本能够向后兼容并与你的设置兼容。*

相反，固定一个版本，当新版本出现时，在您的开发堆栈中测试它。在写这篇文章的时候，最新的版本是`1.10.11`，但是它不能开箱即用，所以我们使用`1.10.10`。

# 比特纳米阿帕奇气流码头标签

一般来说，一个 docker 标签对应一个应用版本。有时也有其他变体，如基本操作系统。在这里，我们可以只使用应用程序版本。

[Bitnami Apache air flow Scheduler 图像标签](https://hub.docker.com/r/bitnami/airflow-scheduler/tags)

[Bitnami Apache 气流工人图片标签](https://hub.docker.com/r/bitnami/airflow-worker/tags)

[Bitnami Apache Airflow 网页图片标签](https://hub.docker.com/r/bitnami/airflow/tags)

# 构建自定义图像

在我们的`docker-compose`中，我们有占位符来构建自定义图像。

我们现在只创建一个最小的 Docker 文件。稍后我将展示如何用额外的系统或 python 包定制 docker 容器。

# 气流应用

```
echo "FROM docker.io/bitnami/airflow:1.10.10" > docker/bitnami-apache-airflow-1.10.10/airflow/Dockerfile
```

会给你这个气流申请 docker 文件。

```
FROM docker.io/bitnami/airflow:1.10.10
```

# 气流调度程序

```
echo "FROM docker.io/bitnami/airflow-scheduler:1.10.10" > docker/bitnami-apache-airflow-1.10.10/airflow-scheduler/Dockerfile
```

会给你这个气流调度 docker 文件。

```
FROM docker.io/bitnami/airflow-scheduler:1.10.10
```

# 气流工人

```
echo "FROM docker.io/bitnami/airflow-worker:1.10.10" > docker/bitnami-apache-airflow-1.10.10/airflow-worker/Dockerfile
```

会给你这个气流工 docker 文件。

```
FROM docker.io/bitnami/airflow-worker:1.10.10
```

# 调出堆栈

抓住上面的`docker-compose`文件，让我们开始吧！

```
cd docker/bitnami-apache-airflow-1.10.10 
docker-compose up
```

如果这是您第一次运行该命令，这将需要一些时间。Docker 将获取它还没有的任何图像，并构建所有的 airflow-*图像。

# 导航到用户界面

一旦一切就绪并开始运行，导航到位于`[http://localhost:8080](http://localhost:8080.)` [的 UI。](http://localhost:8080.)

除非您更改了配置，否则您的默认`username/password`是`user/bitnami`。

登录查看您的气流网络用户界面！

# 添加自定义 DAG

这里有一个我从[阿帕奇气流教程中抓取的 DAG。为了完整起见，我把它包括在这里。](https://airflow.apache.org/docs/stable/tutorial.html)

```
from datetime import timedelta

# The DAG object; we'll need this to instantiate a DAG
from airflow import DAG
# Operators; we need this to operate!
from airflow.operators.bash_operator import BashOperator
from airflow.utils.dates import days_ago
# These args will get passed on to each operator
# You can override them on a per-task basis during operator initialization
default_args = {
    'owner': 'airflow',
    'depends_on_past': False,
    'start_date': days_ago(2),
    'email': ['airflow@example.com'],
    'email_on_failure': False,
    'email_on_retry': False,
    'retries': 1,
    'retry_delay': timedelta(minutes=5),
    # 'queue': 'bash_queue',
    # 'pool': 'backfill',
    # 'priority_weight': 10,
    # 'end_date': datetime(2016, 1, 1),
    # 'wait_for_downstream': False,
    # 'dag': dag,
    # 'sla': timedelta(hours=2),
    # 'execution_timeout': timedelta(seconds=300),
    # 'on_failure_callback': some_function,
    # 'on_success_callback': some_other_function,
    # 'on_retry_callback': another_function,
    # 'sla_miss_callback': yet_another_function,
    # 'trigger_rule': 'all_success'
}
dag = DAG(
    'tutorial',
    default_args=default_args,
    description='A simple tutorial DAG',
    schedule_interval=timedelta(days=1),
)

# t1, t2 and t3 are examples of tasks created by instantiating operators
t1 = BashOperator(
    task_id='print_date',
    bash_command='date',
    dag=dag,
)

t2 = BashOperator(
    task_id='sleep',
    depends_on_past=False,
    bash_command='sleep 5',
    retries=3,
    dag=dag,
)
dag.doc_md = __doc__

t1.doc_md = """\
#### Task Documentation
You can document your task using the attributes `doc_md` (markdown),
`doc` (plain text), `doc_rst`, `doc_json`, `doc_yaml` which gets
rendered in the UI's Task Instance Details page.
![img](http://montcs.bloomu.edu/~bobmon/Semesters/2012-01/491/import%20soul.png)
"""
templated_command = """
{% for i in range(5) %}
    echo "{{ ds }}"
    echo "{{ macros.ds_add(ds, 7)}}"
    echo "{{ params.my_param }}"
{% endfor %}
"""

t3 = BashOperator(
    task_id='templated',
    depends_on_past=False,
    bash_command=templated_command,
    params={'my_param': 'Parameter I passed in'},
    dag=dag,
)

t1 >> [t2, t3]
```

不管怎样，把这个文件放到你的`code/bitnami-apache-airflow-1.10.10/dags`文件夹里。文件名本身并不重要。DAG 名称将是您在文件中设置的名称。

气流会自动重启，如果你刷新用户界面，你会看到新的`tutorial` DAG 列表。

# 建立自定义气流码头集装箱

如果你想添加额外的系统或 python 包，你可以这样做。

```
# code/bitnami-apache-airflow-1.10.10/airflow/Dockerfile
FROM docker.io/bitnami/airflow:1.10.10
# From here - https://github.com/bitnami/bitnami-docker-airflow/blob/master/1/debian-10/Dockerfile

USER root

RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y vim && \
    rm -r /var/lib/apt/lists /var/cache/apt/archives

RUN bash -c "source /opt/bitnami/airflow/venv/bin/activate && \
    pip install flask-restful && \
    deactivate"
```

明确地说，我不再特别赞同这种方法，除了我喜欢添加`flask-restful`来创建定制的 REST API 插件。

我喜欢像对待 web 应用程序一样对待 Apache Airflow。我已经被*烧了太多次*，所以现在我的 web 应用程序只负责路由和渲染视图，其他的什么都不管。

除了它处理我的工作流的业务逻辑之外，气流几乎是一样的。如果我有一些疯狂的熊猫/tensor flow/opencv/任何我需要做的东西，我会将它们构建到一个单独的微服务中，而不会触及我的主要业务逻辑。我喜欢把气流想象成盘踞在网中的蜘蛛。

不过，我有足够的偏执，我喜欢建立自己的图像，这样我就可以把它们推到我自己的 docker repo。

# 总结和从这里去哪里

现在您已经有了基础，是时候构建您的数据科学工作流了！添加一些自定义 Dag，创建一些自定义插件，一般*构建东西*。

如果你想要一个教程，请随时联系我，在 jillian@dabbleofdevops.com 或者在推特上。

# 小抄

这里是一些有希望帮助的命令和资源。

# 登录到 Apache Airflow 实例

默认的用户名和密码是`user`和`bitnami`。

# Docker 编写命令

建设

```
cd code/bitnami-apache-airflow-1.10.10/ 
docker-compose build
```

拿出你的筹码！运行`docker-compose up`会让您的所有日志出现在 STDERR/STDOUT 上。

```
cd code/bitnami-apache-airflow-1.10.10/ 
docker-compose build && docker-compose up
```

如果您想在后台运行，请使用`-d`。

```
cd code/bitnami-apache-airflow-1.10.10/ 
docker-compose build && docker-compose up -d
```

# 比特纳米阿帕奇气流配置

您可以使用传递到 docker-compose 文件中的环境变量来进一步定制气流实例。查看[自述文件](https://github.com/bitnami/bitnami-docker-airflow/blob/master/README.md)了解详情。

# 加载 DAG 文件

定制的 DAG 文件可以挂载到`/opt/bitnami/airflow/dags`或者在 Docker 构建阶段复制。

# 使用 Docker Compose 指定环境变量

```
version: '2'

services:
  airflow:
    image: bitnami/airflow:latest
    environment:
      - AIRFLOW_FERNET_KEY=46BKJoQYlPPOexq0OhDZnIlNepKFf87WFwLbfzqDDho=
      - AIRFLOW_EXECUTOR=CeleryExecutor
      - AIRFLOW_DATABASE_NAME=bitnami_airflow
      - AIRFLOW_DATABASE_USERNAME=bn_airflow
      - AIRFLOW_DATABASE_PASSWORD=bitnami1
      - AIRFLOW_PASSWORD=bitnami123
      - AIRFLOW_USERNAME=user
      - AIRFLOW_EMAIL=user@example.com
```

# 码头后清理

Docker 会在你的文件系统上占据很多空间。

如果您只想清理气流通道，那么:

```
cd code/docker/bitnami-apache-airflow-1.10.10 
docker-compose stop 
docker-compose rm -f -v
```

运行`docker-compose rm -f`会强制删除所有容器，`-v`也会删除所有数据卷。

# 移除所有 docker 图像

这将停止所有正在运行的容器并删除它们。

```
docker container stop $(docker container ls -aq) 
docker system prune -f -a
```

这将删除所有容器和数据卷

```
docker system prune -f -a --volumes
```

*最初发表于*[*【https://www.dabbleofdevops.com】*](https://www.dabbleofdevops.com/blog/get-a-fully-configured-apache-airflow-docker-dev-stack-with-bitnami)*。*