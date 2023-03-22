# 如何使用 Python 和 Boto3 库为 DynamoDB 插入记录和进行条件删除

> 原文：<https://towardsdatascience.com/how-to-insert-records-and-do-conditional-delete-for-dynamodb-using-python-and-boto3-library-3303f74c18b9?source=collection_archive---------11----------------------->

## 一个教程，展示了如何使用 Python 和 Boto3 库以编程方式插入多条记录并按条件删除项目

![](img/d64c4ec1a6f194b1d5a11ff3fbc88873.png)

丹尼尔·冯·阿彭在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 介绍

出于测试目的，我们的 DynamoDB 表中有许多垃圾或虚假记录是很常见的。它可能是我们正在开发的一个应用程序，甚至只是一个功能。它也可以是我们现有的应用程序生成的一些记录，但实际上，它们没有值。不管它是什么，我们最终都会在数据库中创建许多记录。

我们时常想要清理数据库表，因此只存储有价值和有意义的记录。例如，我们做了一个负载测试来测试应用程序中的用户帐户创建 API。这显然导致了表中的许多记录，这些记录只是“测试”记录。所有这些“测试”记录都具有邮件以`testing`开头，姓氏以`TEST`开头的特征。你知道，不管怎样。😆显然，一旦我们完成了负载测试，这些记录就可以从表中删除。

# 你会学到什么

在本教程中，您将学习如何使用 Python 从 DynamoDB 表中插入项目和有条件地删除项目，无论出于什么原因，您都不想再保留这些项目。我们将使用 Python 的官方 AWS SDK 库，它被称为`Boto3`。

# 先决条件

## 设置虚拟环境

我不打算做详细的演练，但会通过`virtualenv`快速展示给你如何做。

首先，你需要通过`pip3`安装`virtualenv`，如果你还没有安装的话。这很简单，因为:

```
$ ~/demo > pip3 install virtualenv
```

然后，创建一个目录来存放项目文件(或者在本例中是 Python 脚本)。

```
$ ~/demo > mkdir batch-ops-dynamodb
```

转到目录并在那里创建虚拟环境。

```
$ ~/demo > cd ./batch-ops-dynamodb$ ~/demo/batch-ops-dynamodb > virtualenv ./venvUsing base prefix '/Library/Frameworks/Python.framework/Versions/3.8'
New python executable in /Users/billyde/demo/batch-ops-dynamodb/venv/bin/python3.8
Also creating executable in /Users/billyde/demo/batch-ops-dynamodb/venv/bin/python
Installing setuptools, pip, wheel...
done.
```

最后，我们希望激活虚拟环境。快速提醒一下，虚拟环境非常有用，因为它将您的开发与机器的其他部分隔离开来。可以将它想象成一个容器，其中所有的依赖项都在虚拟环境中提供，并且只能从环境中访问。

```
$ ~/demo/batch-ops-dynamodb > source ./venv/bin/activate$ ~/demo/batch-ops-dynamodb (venv) >
```

注意`(venv)`。这表明您处于虚拟环境中。但是，根据终端的设置，您可能看不到这一点。因此，作为一种替代方法，这就是为什么您可以验证您的虚拟环境是否处于活动状态。

```
$ ~/demo/batch-ops-dynamodb (venv) > which python
/Users/billyde/demo/batch-ops-dynamodb/venv/bin/python$ ~/demo/batch-ops-dynamodb (venv) > which pip
/Users/billyde/demo/batch-ops-dynamodb/venv/bin/pip
```

您可以看到 Python 和 pip 可执行文件来自我们的虚拟环境。一切都好。🙂

## 安装依赖项

本教程唯一的依赖项是`Boto3`库。因此，让我们继续在我们的虚拟环境中安装它。

```
$ ~/demo/batch-ops-dynamodb (venv) > pip install boto3Collecting boto3
  Downloading boto3-1.11.7-py2.py3-none-any.whl (128 kB)
     |████████████████████████████████| 128 kB 1.0 MB/s
Collecting botocore<1.15.0,>=1.14.7
  Downloading botocore-1.14.7-py2.py3-none-any.whl (5.9 MB)
     |████████████████████████████████| 5.9 MB 462 kB/s
Collecting s3transfer<0.4.0,>=0.3.0
  Downloading s3transfer-0.3.1-py2.py3-none-any.whl (69 kB)
     |████████████████████████████████| 69 kB 2.1 MB/s
Collecting jmespath<1.0.0,>=0.7.1
  Using cached jmespath-0.9.4-py2.py3-none-any.whl (24 kB)
Collecting urllib3<1.26,>=1.20
  Downloading urllib3-1.25.8-py2.py3-none-any.whl (125 kB)
     |████████████████████████████████| 125 kB 5.3 MB/s
Collecting python-dateutil<3.0.0,>=2.1
  Using cached python_dateutil-2.8.1-py2.py3-none-any.whl (227 kB)
Collecting docutils<0.16,>=0.10
  Using cached docutils-0.15.2-py3-none-any.whl (547 kB)
Collecting six>=1.5
  Downloading six-1.14.0-py2.py3-none-any.whl (10 kB)
Installing collected packages: urllib3, six, python-dateutil, docutils, jmespath, botocore, s3transfer, boto3
Successfully installed boto3-1.11.7 botocore-1.14.7 docutils-0.15.2 jmespath-0.9.4 python-dateutil-2.8.1 s3transfer-0.3.1 six-1.14.0 urllib3-1.25.8
```

一旦您完成了先决条件，我们就可以开始做有趣的事情了，那就是编写实际的脚本来批量删除我们的垃圾记录。

# 启动本地 DynamoDB 实例

为了帮助我们了解脚本是如何工作的，我们将在 Docker 容器中启动一个本地 DynamoDB 实例。你可以按照这个[教程](https://medium.com/better-programming/how-to-set-up-a-local-dynamodb-in-a-docker-container-and-perform-the-basic-putitem-getitem-38958237b968)来实现这个。

本质上，你需要做的是启动 DynamoDB Docker 并创建教程中写的表`demo-customer-info`。

# 在表中创建虚拟记录

让我们创建虚拟记录，这样我们可以看到批处理操作是如何工作的。为此，我们将编写一个 Python 脚本，在一个循环中调用 DynamoDB `PutItem`操作。在`batch-ops-dynamo`中新建一个 Python 文件，命名为`insert_dummy_records.py`。

```
~/demo/batch-ops-dynamodb ❯ touch insert_dummy_records.py
```

正如前面介绍部分提到的，我们的虚拟记录将具有以下特征:

*   姓氏以`TEST`开头
*   电子邮件地址以`testing`开头

我们的脚本将包含以下组件:

*   `insert_dummy_record`:执行`PutItem`操作的功能
*   `for loop`:将调用`insert_dummy_record`函数 10 次以插入虚拟记录的循环。

我们还利用`random.randint`方法生成一些随机整数，添加到我们的虚拟记录的属性中，它们是:

*   `customerId`
*   `lastName`
*   `emailAddress`

insert _ dummy _ records.py 将虚拟记录插入到表中

我们的剧本看起来不错！现在，是时候从命令行运行它了，使用来自我们虚拟环境的 Python 可执行文件。

```
~/demo/batch-ops-dynamodb ❯ python3 insert_dummy_records.pyInserting record number 1 with customerId 769
Inserting record number 2 with customerId 885
Inserting record number 3 with customerId 873
Inserting record number 4 with customerId 827
Inserting record number 5 with customerId 231
Inserting record number 6 with customerId 199
Inserting record number 7 with customerId 272
Inserting record number 8 with customerId 268
Inserting record number 9 with customerId 729
Inserting record number 10 with customerId 289
```

好的。这个脚本看起来像预期的那样工作。万岁！😄

让我们通过调用本地 DynamoDB `demo-customer-info`表上的`scan`操作来检查记录。

```
~/demo/batch-ops-dynamodb ❯ aws dynamodb scan --endpoint-url [http://localhost:8042](http://localhost:8042) --table-name demo-customer-info{
    "Items": [
        {
            "customerId": {
                "S": "199"
            },
            "lastName": {
                "S": "TEST199"
            },
            "emailAddress": {
                "S": "[testing199@dummy.com](mailto:testing199@dummy.com)"
            }
        },
        {
            "customerId": {
                "S": "769"
            },
            "lastName": {
                "S": "TEST769"
            },
            "emailAddress": {
                "S": "[testing769@dummy.com](mailto:testing769@dummy.com)"
            }
        },
... truncated
... truncated
... truncated
        {
            "customerId": {
                "S": "827"
            },
            "lastName": {
                "S": "TEST827"
            },
            "emailAddress": {
                "S": "[testing827@dummy.com](mailto:testing827@dummy.com)"
            }
        }
    ],
    "Count": 10,
    "ScannedCount": 10,
    "ConsumedCapacity": null
}
```

完美！我们的表中现在有一些虚拟记录。

# 将“真实”记录插入表中

我们将快速向表中插入两条“真实”的记录。为此，我们将编写另一个 Python 脚本，它将命令行参数作为输入。我们把这个文件命名为`insert_real_record.py`。

```
~/demo/batch-ops-dynamodb ❯ touch insert_real_record.py
```

该文件的内容如下。

insert _ real _ record.py 将“真实”记录插入到表中

让我们继续向表中插入 2 条记录。

```
~/demo/batch-ops-dynamodb ❯ python insert_real_record.py 11111 jones [sam.jones@something.com](mailto:sam.jones@something.com)Inserting record with customerId 11111~/demo/batch-ops-dynamodb ❯ python insert_real_record.py 22222 smith [jack.smith@somedomain.com](mailto:jack.smith@somedomain.com)Inserting record with customerId 22222
```

# 编写根据条件删除记录的脚本

最后，我们将编写脚本，从表中删除满足某些指定条件的记录。

再次提醒，我们要删除我们插入的虚拟记录。我们需要应用的过滤器是姓氏以`TEST`开头，电子邮件地址以`testing`开头。

脚本的工作原理:

*   用给定的过滤表达式在表上执行`scan`操作
*   只从所有记录中检索符合我们的过滤表达式的`customerId`属性，因为这是我们做`DeleteItem`操作所需要的。记住，`customerId`是表的分区键。
*   在`for loop`中，对于我们`scan`操作返回的每个`customerId`，进行`DeleteItem`操作。

Delete _ records _ conditionally . py-按过滤器删除记录

继续从命令行运行这个脚本。

```
~/demo/batch-ops-dynamodb ❯ python delete_records_conditionally.pyGetting customer ids to delete
============
['199', '769', '873', '268', '289', '231', '272', '885', '729', '827']
Deleting customer 199
Deleting customer 769
Deleting customer 873
Deleting customer 268
Deleting customer 289
Deleting customer 231
Deleting customer 272
Deleting customer 885
Deleting customer 729
Deleting customer 827
```

太好了！该脚本按预期工作。现在，让我们检查表中剩余的记录。

```
~/demo/batch-ops-dynamodb ❯ aws dynamodb scan --endpoint-url [http://localhost:8042](http://localhost:8042) --table-name demo-customer-info{
    "Items": [
        {
            "customerId": {
                "S": "22222"
            },
            "lastName": {
                "S": "smith"
            },
            "emailAddress": {
                "S": "[jack.smith@somedomain.com](mailto:jack.smith@somedomain.com)"
            }
        },
        {
            "customerId": {
                "S": "11111"
            },
            "lastName": {
                "S": "jones"
            },
            "emailAddress": {
                "S": "[sam.jones@something.com](mailto:sam.jones@something.com)"
            }
        }
    ],
    "Count": 2,
    "ScannedCount": 2,
    "ConsumedCapacity": null
}
```

只剩下 2 条记录，这是我们插入的“真实”记录。我们现在可以确定我们的`delete_records_conditionally.py`脚本做了它想要做的事情。

很棒的东西。👍

## 删除脚本的一些细节

让我们来看看我们刚刚编写的`delete_records_conditionally.py`脚本中的一些内容。

注意，我们有一个`deserializer`，我们用它来获取`customerId`。原因是因为，`scan`操作实际上会返回这样的结果。

```
# scan operation response{'Items': [{'customerId': {'S': '300'}}, {'customerId': {'S': '794'}}, {'customerId': {'S': '266'}}, {'customerId': {'S': '281'}}, {'customerId': {'S': '223'}}, {'customerId': {'S': '660'}}, {'customerId': {'S': '384'}}, {'customerId': {'S': '673'}}, {'customerId': {'S': '378'}}, {'customerId': {'S': '426'}}], 'Count': 10, 'ScannedCount': 12, 'ResponseMetadata': {'RequestId': 'eb18e221-d825-4f28-b142-ff616d0ca323', 'HTTPStatusCode': 200, 'HTTPHeaders': {'content-type': 'application/x-amz-json-1.0', 'x-amz-crc32': '2155737492', 'x-amzn-requestid': 'eb18e221-d825-4f28-b142-ff616d0ca323', 'content-length': '310', 'server': 'Jetty(8.1.12.v20130726)'}, 'RetryAttempts': 0}}
```

由于我们只对`customerId`的值感兴趣，我们需要使用 Boto3 库提供的`TypeDeserializer`去序列化 DynamoDB 项。

另一个值得一提的组件是`Select`和`ProjectionExpression`，它们是`scan`函数的参数。这两个参数密切相关。我们将`Select`的值设置为`SPECIFIC_ATTRIBUTES`，根据 Boto3 官方文档，这将只返回`AttributesToGet`中列出的属性。`AttributesToGet`已被标记为遗留参数，AWS 建议我们改用`ProjectionExpression`。

来自 Boto3 官方文档:

> 如果使用 ProjectionExpression 参数，则 Select 的值只能是 SPECIFIC_ATTRIBUTES。Select 的任何其他值都将返回错误。

这 2 个参数基本上是在说`scan`操作应该只返回`customerId`属性，这是我们需要的。

最后，我们使用 AWS 提供的`begins_with()`函数。这个函数使用属性名和前缀来检查指定属性的值。

# 包裹

至此，您已经学会了如何使用 Python 和 Boto3 插入和删除 DynamoDB 记录。您当然可以调整和修改脚本来满足您的需要。例如，本教程中使用的过滤器表达式相对简单，如果需要，您可以添加更复杂的条件。

不管怎样，我希望这篇教程已经让你对如何使用 Python 进行 DynamoDB 操作有了基本的了解。所以，继续发挥你的创造力，利用这个脚本，增强它，做更高级的东西。黑客快乐！🙂

注:这里是 Github [回购](https://github.com/billydh/batch-ops-dynamodb)。

![](img/d3847ce046362cfc93e19a3591a52f56.png)

安东尼奥·加波拉在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片