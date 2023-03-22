# Apache/Airflow 和 PostgreSQL 与 Docker 和 Docker Compose

> 原文：<https://towardsdatascience.com/apache-airflow-and-postgresql-with-docker-and-docker-compose-5651766dfa96?source=collection_archive---------4----------------------->

## 带有码头工人的 ETL

## 如何使用 Docker 和 Docker Compose 设置带有 PostgreSQL 和 LocalExecutor 的官方 Apache/Airflow 容器

![](img/e5ffcad7c2b07e53225c7c8d4a4792d1.png)

照片由[乔纳森派](https://unsplash.com/@r3dmax?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

你好，在这篇文章中，我将向你展示如何使用 docker 和 docker-compose 用 PostgreSQL 和 LocalExecutor 设置官方的 Apache/Airflow。在这篇文章中，我不会讨论气流，它是什么，以及它是如何使用的。请查看**[**官方文档**](https://airflow.apache.org/docs/stable/)**了解更多相关信息。****

****在设置和运行 Apache Airflow 之前，请安装[对接器](https://docs.docker.com/get-docker/)和[对接器组合](https://docs.docker.com/compose/install/)。****

# ****对于那些赶时间的人...****

****在这一章中，我将向您展示运行 airflow 所需的文件和目录，在下一章中，我将逐文件、逐行解释正在发生的事情。****

****首先，在根目录下再创建三个目录: **dags** 、 **logs、**和 **scripts** 。此外，创建以下文件:**。env、docker-compose.yml、entrypoint.sh** 和 **dummy_dag.py.** 请确保这些文件和目录遵循以下结构。****

```
****#project structure**root/
├── dags/
│   └── dummy_dag.py
├── scripts/
│   └── entrypoint.sh
├── logs/
├── .env
└── docker-compose.yml**
```

****创建的文件应包含以下内容:****

```
****#docker-compose.yml**version: '3.8'
services:
    postgres:
        image: postgres
        environment:
            - POSTGRES_USER=airflow
            - POSTGRES_PASSWORD=airflow
            - POSTGRES_DB=airflow
    scheduler:
        image: apache/airflow
        command: scheduler
        restart_policy:
            condition: on-failure
        depends_on:
            - postgres
        env_file:
            - .env
        volumes:
            - ./dags:/opt/airflow/dags
            - ./logs:/opt/airflow/logs
    webserver:
        image: apache/airflow
        entrypoint: ./scripts/entrypoint.sh
        restart_policy:
            condition: on-failure
        depends_on:
            - postgres
            - scheduler
        env_file:
            - .env
        volumes:
            - ./dags:/opt/airflow/dags
            - ./logs:/opt/airflow/logs
            - ./scripts:/opt/airflow/scripts
        ports:
            - "8080:8080"**
```

```
****#entrypoint.sh**#!/usr/bin/env bash
airflow initdb
airflow webserver**
```

```
****#.env**AIRFLOW__CORE__SQL_ALCHEMY_CONN=postgresql+psycopg2://airflow:airflow@postgres/airflow
AIRFLOW__CORE__EXECUTOR=LocalExecutor**
```

```
****#dummy_dag.py**from airflow import DAG
from airflow.operators.dummy_operator import DummyOperator
from datetime import datetimewith DAG('example_dag', start_date=datetime(2016, 1, 1)) as dag:
    op = DummyOperator(task_id='op')**
```

****在根目录中定位并在终端中执行**“docker-compose up”**应该可以在 [localhost:8080](http://localhost:8080/) 上访问气流。下图显示了最终结果。****

> ****如果您遇到权限错误，请在所有子目录上运行“chmod -R 777”，例如“chmod -R 777 logs/”****

****![](img/6e6c91ae28176f39a45d5f701e0a5006.png)****

# ****为了好奇的人...****

****按照 Leyman 的说法， **docker** 用于管理单个容器，而 **docker-compose** 可用于管理多容器应用程序。它还将您在 **docker run** 中输入的许多选项移动到 **docker-compose.yml** 文件中，以便于重用。它在 docker 使用的相同 docker API 之上作为前端“脚本”工作。您可以使用 docker 命令和大量 shell 脚本完成 docker-compose 所做的一切。****

****在运行我们的多容器 docker 应用程序之前，必须配置 **docker-compose.yml** 。有了这个文件，我们定义了将在 **docker-compose up** 上运行的服务。****

****docker-compose.yml 的第一个属性是**版本，**是合成文件格式版本。有关文件格式和所有配置选项的最新版本，请单击此处的。****

****第二个属性是**服务**，服务下一层的所有属性表示我们的多容器应用程序中使用的容器。这些是 **postgres、调度器**和**web 服务器。**每个容器都有**图像**属性，该属性指向用于该服务的基本图像。****

****对于每个服务，我们定义服务容器内部使用的环境变量。对于 postgres，它是由**环境**属性定义的，但是对于 scheduler 和 webserver，它是由**定义的。env** 文件。因为**。env** 是一个外部文件，我们必须用 **env_file** 属性指向它。****

****通过打开**。env** 文件我们可以看到定义了两个变量。一个定义将要使用的执行器，另一个表示连接字符串。每个连接字符串必须以下列方式定义:****

```
**dialect+driver://username:password@host:port/database**
```

****方言名称包括 SQLAlchemy 方言的识别名称，如`sqlite`、`mysql`、`postgresql`、`oracle`或`mssql`。Driver 是用于连接数据库的 DBAPI 的名称，全部使用小写字母。在我们的例子中，连接字符串由以下内容定义:****

```
**postgresql+psycopg2://airflow:airflow@postgres/airflow**
```

****在主机部分后省略 port 表示我们将使用在自己的 docker 文件中定义的默认 postgres 端口。****

****每个服务都可以定义**命令**，该命令将在 Docker 容器中运行。如果一个服务需要执行多个命令，可以通过定义一个可选的**来完成。sh** 文件并用 **entrypoint** 属性指向它。在我们的例子中，在**脚本**文件夹中有 **entrypoint.sh** ，一旦执行，就会运行 **airflow initdb** 和 **airflow webserver** 。两者都是气流正常运行的必要条件。****

****定义**依赖于**属性，我们可以表达服务之间的依赖关系。在我们的示例中，webserver 仅在 scheduler 和 postgres 都已启动时启动，而且 scheduler 仅在 postgres 启动后启动。****

****如果我们的容器崩溃，我们可以通过 **restart_policy** 重启它。restart_policy 配置容器退出时是否以及如何重新启动容器。其他选项包括条件、延迟、最大尝试次数和窗口。****

****一旦服务运行，它就在容器定义的端口上被服务。要访问该服务，我们需要将容器端口暴露给主机端口。这是由**端口**属性完成的。在我们的例子中，我们将容器的端口 **8080** 暴露给主机的 **127.0.0.1 (localhost)** 上的 TCP 端口 **8080** 。`:`左侧定义主机端口，右侧定义容器端口。****

****最后，**卷**属性定义了主机文件系统和 docker 容器之间的共享卷(目录)。因为 airflows 的默认工作目录是 */opt/airflow/* 我们需要将我们指定的卷从根文件夹指向 airflow containers 工作目录。这是通过以下命令完成的:****

```
****#general case for airflow**- ./<our-root-subdir>:/opt/airflow/<our-root-subdir>**#our case**- ./dags:/opt/airflow/dags
- ./logs:/opt/airflow/logs
- ./scripts:/opt/airflow/scripts
           ...**
```

****这样，当调度程序或 web 服务器将日志写入其 **logs** 目录时，我们可以从 **logs** 目录中的文件系统访问它。当我们向 dags 文件夹添加新的 dag 时，它将自动添加到容器 dag 包中，依此类推。****

****今天到此为止，谢谢你阅读这个故事，我会很快发布更多。如果你注意到任何错误，请让我知道。****

******编辑**:“重新启动 _ 策略:”前需要“部署:”根据意见****