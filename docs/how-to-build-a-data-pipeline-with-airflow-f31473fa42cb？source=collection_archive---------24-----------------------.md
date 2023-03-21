# 如何建立一个有气流的数据管道

> 原文：<https://towardsdatascience.com/how-to-build-a-data-pipeline-with-airflow-f31473fa42cb?source=collection_archive---------24----------------------->

## 开始在 Python 中使用 Airflow 时需要知道的一些基础知识

![](img/435eae324e0113cd62efca84ef3ef01b.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3382863) 的 [Nico Wall](https://pixabay.com/users/VanVangelis-7215570/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3382863)

**简介**

Airflow 是一个允许调度和监控数据管道的工具。这个工具是用 Python 编写的，它是一个开源的工作流管理平台。

Airflow 可以用来编写机器学习管道、ETL 管道，或者一般来说用来调度作业。因此，感谢气流，我们可以自动化工作流程，避免许多无聊的手动任务。

在 Apache Airflow 中，工作流中的各种任务形成了一个图表。这些任务通过依赖关系链接在一起。连接一个任务和另一个任务的箭头有一个特定的方向，没有循环，因此在气流中我们有 DAGs，意思是有向无环图。

**第一部分:如何创建 DAG 和操作符来执行任务？**

当我们想要创建 DAG 时，我们必须提供名称、描述、开始日期和间隔，如下所示。

```
from airflow import DAGfirst_dag = DAG( ‘first’, 
            description = ‘text’, 
            start_date = datetime(2020, 7, 28),
            schedule_interval = ‘@daily’)
```

操作符是 DAG 的构造块。它们定义了 DAG 将执行的实际工作。我们需要通过设置 task_id、python_callable 和 dag 来参数化操作符。

```
from airflow import DAGfrom airflow.operators.python_operator import PythonOperatordef my_func():
   print('done')first_dag = DAG(...)task = PythonOperator(
     task_id = 'first_task',
     python_callable=my_func ,
     dag=first_dag)
```

其他常见的运算符有:

```
- PostgresOperator- RedshiftToS3Operator- S3ToRedshiftOperator- BashOperator- SimpleHttpOperator- Sensor
```

计划是可选的，默认情况下，如果我们忽略计划间隔，我们的 DAG 将从开始日起每天运行一次。无论如何，在下面你可以找到设置你的时间间隔的所有选择。

```
@once →run a DAG only once time@hourly → run the DAG every hour@daily → run the DAG every day@weekly → run the DAG every week@monthly → run the DAG every month@yearly → run the DAG every year
```

**第二部分:任务依赖和气流挂钩**

在 Airflow 中，每个有向无环图都由节点(即任务)和边来表征，这些节点和边强调了任务之间的顺序和依赖性。

我们可以使用双箭头操作符“> >”来描述依赖关系。所以:

*   **a>b**表示 a 在 b 之前
*   **甲< <乙**的意思是乙先于甲

另一种方法是使用**设置 _ 下游**和**设置 _ 上游:**

*   **a.set_downstream(b)** 表示 a 在 b 之前
*   **a.set_upstream(b)** 意思是 a 在 b 之后

为了与我们的数据存储和其他外部系统(如 Redshift、Postgres 或 MySQL)进行交互，我们可以配置**气流挂钩。**下面用举例说明一下它的样子。

```
from airflow import DAGfrom airflow.hooks.postgres_hook import PostgresHookform airflow.operators.python_operator import PythonOperatordef load():
    #create  a PostgresHook option using the 'example' connection
     db_hook = PostgresHook('example')
     df = db_hook.get_pandas_df('SELECT * FROM my_table')load_task = PyhtonOperator(task_id='load',                                     python_callable=my_func_name, ...)
```

上面你可以看到我导入了一个 PostgresHook，我通过给它命名为‘example’来初始化它。

然后通过函数**db _ hook . get _ pandas _ df(' SELECT * FROM my _ table '**)我请求给我一个 pandas 数据框。因此，Airflow 开始运行 SQL 语句，最后结果被加载到 pandas 数据框中并返回给 Airflow。

然而，气流还有其他的挂钩，比如:

```
- HttpHook- MySqlHook- SlackHook
```

**第三部分:上下文和模板**

气流提供了几个特定于运行时给定 DAG 和任务的执行的上下文变量。在处理之前访问或分割数据的过程中，上下文变量非常有用。你可以在这里找到一个变量列表，可以作为夸尔格包含在内。

下面，我们有一个函数叫做 **my_func。**函数中有*args 和**kwargs(即关键字参数列表)。此外，您可以注意到，在**任务**中，**提供 _ 上下文**被设置为 True。

```
from airflow import DAG
from airflow.operators.python_operator import PythonOperatordef my_func(*args, **kwargs): 
     logging_info(f"start{kwargs['ds']}")first_dag = DAG(...)
task = PythonOperator(
     task_id = 'date',
     python_callable = my_func,
     provide_context = True,
     dag=first_dag)
```

**结论**

现在，您已经具备了构建气流管道的基础。我认为理解这些概念的最好方法是建立你的气流环境并自己尝试。

我发出一份期刊简讯。如果您想加入，请通过此链接 注册。

除了我的**简讯**，我们还可以在我的电报群 [**数据科学初学者**](https://t.me/DataScienceForBeginners) **中取得联系。**