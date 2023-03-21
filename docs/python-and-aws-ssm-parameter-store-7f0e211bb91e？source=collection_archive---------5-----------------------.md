# Python 和 AWS SSM 参数存储

> 原文：<https://towardsdatascience.com/python-and-aws-ssm-parameter-store-7f0e211bb91e?source=collection_archive---------5----------------------->

## 保护应用程序秘密的一种方法是通过 AWS SSM 参数存储。我们将看看如何以编程方式检索和解密这个秘密。

![](img/205c03094f4dad7d45b19e2cbe86b976.png)

照片由[克里斯·帕纳斯](https://unsplash.com/@chrispanas?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 介绍

在任何应用程序中，拥有一些秘密是很常见的，我们的应用程序需要这些秘密来提供它想要的功能。禁止把那些秘密放在我们的项目文件夹里，提交给 Github repo。这是一个很大的不，不。🙅‍♂

有几个安全存储秘密的替代方法，但是，在本教程中，我将向您展示如何使用 AWS 密钥管理服务(KMS)和系统管理器的参数存储(SSM)来实现。我们将使用 Python 3.8 作为编程语言，并使用官方的 AWS Boto3 库来与 AWS 资源进行交互。

# 你将学到什么

本教程结束时，您将能够完成以下练习:

*   用 KMS 创建一个密钥
*   把一个参数/秘密作为进入 SSM 的安全措施
*   检索并解密 SSM 秘密参数

本教程的 Github repo 可从[这里](https://github.com/billydh/kms-ssm-decrypt)获得。

# 先决条件

本教程假设您已经拥有一个 AWS 帐户，并且可以访问它。这是做 KMS 和 SSM 相关练习所必需的，特别是创建 KMS 键并将一个参数放入 SSM。我们将使用`aws-cli`来做这件事。你可以按照 AWS 官方[文档](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv1.html)来安装和设置凭证。

或者，您可以直接在 AWS 控制台上完成。

# 准备 Python 环境和依赖性

在本节中，我们将设置 SSM 参数解密所需的所有组件。

## 创建虚拟环境

让我们使用`virtualenv`创建一个虚拟环境来包含我们的项目依赖关系。从您的终端运行以下命令来创建虚拟环境并激活它。

```
$ ❯ mkdir -p ./demo/kms-ssm-decrypt$ ❯ cd ./demo/kms-ssm-decrypt$ ~/demo/kms-ssm-decrypt ❯ virtualenv ./venv
Using base prefix '/Library/Frameworks/Python.framework/Versions/3.8'
New python executable in /Users/billyde/demo/kms-ssm-decrypt/venv/bin/python3.8
Also creating executable in /Users/billyde/demo/kms-ssm-decrypt/venv/bin/python
Installing setuptools, pip, wheel...
done.$ ~/demo/kms-ssm-decrypt ❯ source ./venv/bin/activate$ ~/demo/kms-ssm-decrypt (venv) ❯
```

## 安装 Boto3 库

唯一需要的库是 Boto3。让我们创建一个文本文件来保持项目的依赖性，并将其命名为`requirements.txt`。

requirements.txt

我们之所以有这个文本文件而不是直接安装库，是因为我们可以将它提交给 Github repo，而不是整个`venv`以保持 repo 大小紧凑。然后，任何人都可以克隆 repo，并通过`pip`在他们的机器上安装所有的需求。

让我们继续从我们的终端安装这个包。

```
$ ~/demo/kms-ssm-decrypt (venv) ❯ pip install -r requirements.txt
Collecting boto3==1.11.0
  Using cached boto3-1.11.0-py2.py3-none-any.whl (128 kB)
Collecting s3transfer<0.4.0,>=0.3.0
  Downloading s3transfer-0.3.2-py2.py3-none-any.whl (69 kB)
     |████████████████████████████████| 69 kB 1.5 MB/s
Collecting botocore<1.15.0,>=1.14.0
  Downloading botocore-1.14.9-py2.py3-none-any.whl (5.9 MB)
     |████████████████████████████████| 5.9 MB 4.6 MB/s
Collecting jmespath<1.0.0,>=0.7.1
  Using cached jmespath-0.9.4-py2.py3-none-any.whl (24 kB)
Collecting urllib3<1.26,>=1.20
  Using cached urllib3-1.25.8-py2.py3-none-any.whl (125 kB)
Collecting python-dateutil<3.0.0,>=2.1
  Using cached python_dateutil-2.8.1-py2.py3-none-any.whl (227 kB)
Collecting docutils<0.16,>=0.10
  Using cached docutils-0.15.2-py3-none-any.whl (547 kB)
Collecting six>=1.5
  Using cached six-1.14.0-py2.py3-none-any.whl (10 kB)
Installing collected packages: urllib3, six, python-dateutil, docutils, jmespath, botocore, s3transfer, boto3
Successfully installed boto3-1.11.0 botocore-1.14.9 docutils-0.15.2 jmespath-0.9.4 python-dateutil-2.8.1 s3transfer-0.3.2 six-1.14.0 urllib3-1.25.8
```

# 创建 KMS 键

既然我们要存储 secret/s，最好在静态时加密它，以提供额外的安全层。我们将创建一个 KMS 密钥，用于加密和解密我们的秘密参数

从您的终端，运行以下命令，这将创建一个 KMS 密钥。

```
$ ~/demo/kms-ssm-decrypt (venv) ❯ aws kms create-key --description "A key to encrypt-decrypt secrets"
{
    "KeyMetadata": {
        "AWSAccountId": "xxxxxxxxxxxx",
        "KeyId": "ca83c234-7e63-4d8d-1234-28603cb10123",
        "Arn": "arn:aws:kms:ap-southeast-2:xxxxxxxxxxxx:key/ca83c234-7e63-4d8d-1234-28603cb10123",
        "CreationDate": 1580217378.859,
        "Enabled": true,
        "Description": "A key to encrypt-decrypt secrets",
        "KeyUsage": "ENCRYPT_DECRYPT",
        "KeyState": "Enabled",
        "Origin": "AWS_KMS",
        "KeyManager": "CUSTOMER"
    }
}
```

很好，如果运行成功，该命令会将密钥元数据输出到控制台上。我们可以继续下一步了。

# 将秘密放入 SSM 参数存储中

我们将把一个秘密参数作为`SecureString`存储到 SSM 参数库中。`SecureString`参数类型仅仅表示我们存储的参数值将被加密。

为了放置一个秘密参数，让我们从终端执行下面的命令。

```
$ ~/demo/kms-ssm-decrypt (venv) ❯ aws ssm put-parameter --name "/demo/secret/parameter" --value "thisIsASecret" --type SecureString --key-id ca83c234-7e63-4d8d-1234-28603cb10123 --description "This is a secret parameter"
{
    "Version": 1,
    "Tier": "Standard"
}
```

该命令的参数是不言自明的，但是，我只想强调其中的几个:

*   `--name`:是你正在存储的参数的名称或路径，例如，它可能只是一个类似于“somesecretpathname”的名称，而不是上面的路径。
*   `--value`:您正在存储的参数值。
*   `--type`:参数的类型，在这个例子中是一个`SecureString`，告诉 SSM 在将值存储到参数存储之前加密它。如果你只是存储一个非秘密的参数，那么 SSM 参数类型应该是`String`。
*   `--key-id`:您想要用来加密参数值的 KMS 密钥的 id。这是上一节中生成 KMS 键的`KeyId`的值。
*   `--description`:是您正在存储的参数的描述，以便您知道它的用途。

太棒了。现在，我们将秘密参数存储在 SSM 参数存储中。让我们看看如何以编程方式检索和解密它。

# 编写一个 Python 脚本来检索和解密机密

现在，是时候编写脚本来检索我们刚刚存储在 SSM 参数存储中的秘密参数，并解密它，以便我们可以在我们的应用程序中使用它。

让我们在项目目录中创建新的 Python 文件，并将其命名为`retrieve_and_decrypt_ssm_secret.py`。

`retrieve_and_decrypt_ssm_secret.py`

注意`get_parameter()`函数名为`WithDecryption`的参数。在本练习中，我们将它指定为`True`,因为我们想要获得这个秘密值并在我们的应用程序中使用它。你可能会问，为什么我们不指定用来加密它的`KeyId`？答案是，`KeyId`信息实际上包含在加密的参数中，因此，SSM 知道用哪个密钥来解密这个秘密。

该脚本可从终端运行，它接受一个参数，即参数的名称或路径。像这样从终端执行它。(注意，Boto3 将在您的环境中寻找 AWS 凭证)

```
$ ~/demo/kms-ssm-decrypt (venv) ❯ python3 retrieve_and_decrypt_ssm_secret.py "/demo/secret/parameter"Secret is
thisIsASecret
=====
Parameter details
{'Parameter': {'Name': '/demo/secret/parameter', 'Type': 'SecureString', 'Value': 'thisIsASecret', 'Version': 1, 'LastModifiedDate': datetime.datetime(2020, 1, 30, 23, 26, 35, 169000, tzinfo=tzlocal()), 'ARN': 'arn:aws:ssm:ap-southeast-2:xxxxxxxxxxxx:parameter/demo/secret/parameter'}, 'ResponseMetadata': {'RequestId': '5ad11d35-axz6-42a0-90g5-0b7682a79321', 'HTTPStatusCode': 200, 'HTTPHeaders': {'x-amzn-requestid': '5ad07d20-ama6-43c0-90a4-0b7682a11267', 'content-type': 'application/x-amz-json-1.1', 'content-length': '239', 'date': 'Thu, 30 Jan 2020 12:48:53 GMT'}, 'RetryAttempts': 0}}
```

Tada！我们拿回了秘密参数— `thisIsASecret`。🙂

# 包裹

到目前为止，您已经掌握了如何使用 AWS KMS 和 SSM 参数库保护您的应用程序机密的基本技能和知识。您还学习了如何以编程方式检索机密，以便可以在您的应用程序中使用它。

我希望本教程是有用的，并帮助您了解一种方法来保护您的应用程序的秘密。

只是提醒一下，Github 回购在这里[可用](https://github.com/billydh/kms-ssm-decrypt)。

![](img/08b25afd9f095c9f6c580d6de8a931f0.png)

Mauro Sbicego 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片