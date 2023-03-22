# 通过 Boto3 使用亚马逊 S3 桶

> 原文：<https://towardsdatascience.com/working-with-amazon-s3-buckets-with-boto3-785252ea22e0?source=collection_archive---------7----------------------->

## 完整的备忘单。

![](img/6600ad655167e15a8713ff8170bd32e4.png)

[杰夫·金玛](https://unsplash.com/@kingmaphotos?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/bucket?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

亚马逊简单存储服务(Amazon Simple Storage Service，简称 S3)通过微调访问控制提供了存储、保护和共享数据的空间。当使用 Python 时，人们可以通过 Boto3 包轻松地与 S3 进行交互。在这篇文章中，我将把我在使用 S3 时经常用到的 Python 命令汇总在一起。希望你会觉得有用。

![](img/4fa79d8e1efffb34c1c49ca6a5ee5eb5.png)

让我们从关于 S3 数据结构的几句话开始。在你自己的电脑上，你将**文件**存储在**文件夹**中。在 S3，这些文件夹被称为**桶**。在桶里面，你可以存储**对象**，比如。csv 文件。您可以通过名称**来引用存储桶，而通过关键字**来引用对象。为了使代码块更易处理，我们将使用表情符号。以下是符号的关键:****

```
🗑 — a bucket’s name, e.g. “mybucket”🔑 — an object’s key, e.g. "myfile_s3_name.csv"📄 - a file's name on your computer, e.g. "myfile_local_name.csv"
```

🗑和🔑可以表示 S3 上已经存在的名称，也可以表示您希望为新创建的桶或对象指定的名称。📄表示您已经拥有或希望拥有在您的计算机本地某处的文件。

![](img/4fa79d8e1efffb34c1c49ca6a5ee5eb5.png)

## 设置客户端

要用 Boto3 访问任何 AWS 服务，我们必须用一个客户机连接到它。在这里，我们创建一个 S3 客户端。我们指定数据所在的区域。我们还必须传递访问密钥和密码，这可以在 AWS 控制台中生成，如这里的[所述](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html#Using_CreateAccessKey)。

## 存储桶:列出、创建和删除

要列出 S3 上现有的存储桶，删除一个或创建一个新的，我们只需分别使用`list_buckets()`、`create_bucket()`和`delete_bucket()`函数。

## 对象:列表、下载、上传和删除

在一个桶中，存在对象。我们可以用`list_objects()`把它们列出来。`MaxKeys`参数设置列出的对象的最大数量；这就像在打印结果之前调用`head()`一样。我们还可以使用`Prefix`参数只列出键(名称)以特定前缀开头的对象。
我们可以使用`upload_file()`上传一个名为📄以这个名字去 S3🔑。同样，`download_file()`会保存一个名为🔑在 S3 当地以这个名字📄。
为了获得一些关于对象的元数据，比如创建或修改时间、权限或大小，我们可以调用`head_object()`。
删除一个对象的工作方式与删除一个桶相同:我们只需要将桶名和对象键传递给`delete_object()`。

## 将多个文件加载到单个数据框中

通常，数据分布在几个文件中。例如，您可以将不同商店或地区的销售数据保存在具有匹配列名的不同 CSV 文件中。对于分析或建模，我们可能希望将所有这些数据放在一个 pandas 数据框中。下面的代码块就可以做到这一点:下载🗑境内名称以“some_prefix”开头的所有数据文件，并将其放入单个数据框中。

## 使用访问控制列表(ACL)将对象设为公共或私有

在 S3 上管理访问权限的一种方法是使用访问控制列表或 ACL。默认情况下，所有文件都是私有的，这是最好的(也是最安全的！)练习。您可以将文件指定为`"public-read"`，在这种情况下，每个人都可以访问它，或者指定为`"private"`，使您自己成为唯一授权的人。查看[此处](https://docs.aws.amazon.com/AmazonS3/latest/dev/acl-overview.html#canned-acl)获取访问选项的详细列表。
当文件已经在 S3 上时，你可以使用`put_object_acl()`来设置它的 ACL，也可以在上传时通过将适当的`ExtraArgs`传递给`upload_file()`来设置。

## 使用预签名的 URL 访问私有文件

您还可以通过使用`generate_presigned_url()`函数生成一个临时的预签名 URL 来授予任何人对私有文件的短期访问权。这将产生一个字符串，可以直接插入到 pandas 的`read_csv()`中，例如，下载数据。您可以通过`ExpiresIn`参数指定该临时访问链接的有效时间。这里，我们创建一个有效期为 1 小时(3600 秒)的链接。

![](img/4fa79d8e1efffb34c1c49ca6a5ee5eb5.png)

感谢阅读！

如果你喜欢这篇文章，为什么不在我的新文章上 [**订阅电子邮件更新**](https://michaloleszak.medium.com/subscribe) ？通过 [**成为媒介会员**](https://michaloleszak.medium.com/membership) ，你可以支持我的写作，并无限制地访问其他作者和我自己的所有故事。

需要咨询？你可以问我任何事情，也可以在这里 预定我 1:1 [**。**](http://hiretheauthor.com/michal)

也可以试试 [**我的其他文章**](https://michaloleszak.github.io/blog/) 中的一篇。不能选择？从这些中选择一个:

[](/working-with-amazon-sns-with-boto3-7acb1347622d) [## 通过 Boto3 使用 Amazon SNS

### 完整的备忘单。

towardsdatascience.com](/working-with-amazon-sns-with-boto3-7acb1347622d) [](/boost-your-grasp-on-boosting-acf239694b1) [## 增强你对助推的把握

### 揭秘著名的竞赛获奖算法。

towardsdatascience.com](/boost-your-grasp-on-boosting-acf239694b1) [](/a-comparison-of-shrinkage-and-selection-methods-for-linear-regression-ee4dd3a71f16) [## 线性回归中收缩法和选择法的比较

### 详细介绍 7 种流行的收缩和选择方法。

towardsdatascience.com](/a-comparison-of-shrinkage-and-selection-methods-for-linear-regression-ee4dd3a71f16)