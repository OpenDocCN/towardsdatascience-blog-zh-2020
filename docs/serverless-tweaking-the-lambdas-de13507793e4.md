# 无服务器:调整 Lambdas

> 原文：<https://towardsdatascience.com/serverless-tweaking-the-lambdas-de13507793e4?source=collection_archive---------34----------------------->

## 调整 lambdas 以确保不可伸缩资源的高可伸缩性

![](img/2fe3a0a9607e01dbf1a6c672f65963be.png)

图片由来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=362150) 的 [Ryan McGuire](https://pixabay.com/users/RyanMcGuire-123690/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=362150) 拍摄

无服务器编程很容易部署。无服务器的可伸缩性只受您指定的上限的限制(至少文档是这么说的)。然而，除非你做了正确的设计，否则整个系统可能会立刻失效。这是因为我们插入到 **AWS** 部署中的大部分外部资源不像 lambda 函数那样可伸缩。此外，您的设计应该基于您正在为无服务器环境编写程序的想法。

在这篇文章中，我将分享我在一个具有大并发性的大中型项目中工作时所面临的困难。我将使用**节点。JS** 作为编程环境。

# 基础知识

有几个事实你应该记住。

*   您永远不应该假设同一个实例将用于处理请求，尽管我们可能会利用这种可能性来提高性能。
*   没有用于数据序列化的永久存储。然而，您可以访问一个小的 ***/tmp*** 路径来在单个调用中进行序列化。因此，确保使用唯一的文件名以避免冲突，并在同一次调用中完成。
*   不应对服务请求的顺序做出任何假设。
*   根据 CPU 小时数(和 RAM)收取使用费。所以要确保你的代码运行得很快，没有太多的颠簸。

# 处理瓶颈资源

几乎在所有场景中，web 应用程序都希望连接到数据库并读取/写入数据。然而，数据库的并发连接数是有限的，可以通过 lambda 函数的并发级别轻松通过。此外，您可以拥有多个独立的函数，这些函数都有数据库连接。例如，如果几乎同时调用 10 个函数，那么最少需要 10 个连接。然而，在高峰场景中，这很容易被乘以一个大到足以使数据库达到极限的因子。

## 降低可扩展性

大多数情况下，您不会达到希望有 1000 个并发调用的程度。您可以从 **AWS 控制台**或在 ***serverless.yml*** 中更改此设置。([看看我的无服务器样板文章，了解更多关于组织无服务器项目的信息](/serverless-a-painless-aws-boilerplate-e5ec3b4fb609))。

```
functions:
  hello:
    handler: handler.hello
      events:
        - http:
            path: /hello
          method: get
    provisionedConcurrency: 5
    reservedConcurrency: 5
```

**保留的并发性**将确保函数总是可以伸缩那么多倍，而**提供的并发性**决定最初将有多少实例是活动的。这确保了扩展时间不会增加服务延迟。在一个示例场景中，您可能希望在主页上设置多一点的并发性，而在用户管理服务上设置很少的并发性，因为它们不能应对高需求。

## 缓存数据并避免数据库连接

在某些情况下，不要求数据一致。例如，博客网站可能不希望总是从数据库中获取数据。用户可能很乐意阅读一些陈旧的数据，但他们永远不会注意到。看看下面的函数。

```
exports.main = (event, context, callback) => {
    // connect to database, send data in callback(null, data);
}
```

这将始终与数据库通信。然而，可以这样做。

```
// cache data in a variable and close DB
exports.main = (event, context, callback) => {
    // send data if event.queryParameters are same
    // else: fetch data and send over
}
```

通过这种修改，很可能不再从数据库中一次又一次地寻找冗余数据。甚至可以使用时间戳来检查数据是否太陈旧。通过使用一个 chron 作业调用函数，可以有更多的技巧来保持函数的热度。如果负载分布随时间不均匀，则不应这样做。

# 保持功能温暖

这是 AWS 自己提出的一个著名的保持功能温暖的建议。我们可以简单地通过回调发送数据来做到这一点，而不必完成事件循环。

```
exports.main = (event, context, callback) => {
    context.callbackWaitsForEmptyEventLoop = false;
}
```

可以在**上下文**对象中将***callbackWaitsForEmptyEventLoop***设置为假。这有助于维护数据库连接，同时在关闭数据库连接之前处理请求。这类似于在全局变量中缓存数据，但在这里，我们缓存整个连接。确保检查连接是否处于活动状态，因为它可能会被数据库服务本身中断。

在这种方法中，有可能超出数据库连接限制。但是这可能是一个方便的技巧，因为建立数据库连接本身可能会增加一些延迟。

# 处理 CPU 密集型任务

CPU 密集型任务主要是矩阵计算。然而，我们可以把 ***水印*** 图像看作是这样一个任务。让我们看看下面的例子。

```
const Jimp = require('jimp');create_watermarks = async (key) => {
  // get the images from s3
  const s3Obj = await s3.getObject(params).promise();
  // read watermark image
  // for each image from s3
      watermarkImage = await Jimp.read(__dirname + '/wm.png');
      const img = await Jimp.read(s3Obj.Body);
      // add watermark
}
```

在这个函数中，我们多次读取图像。但是，添加以下内容可以节省大量时间，加快一切。

```
const img = await Jimp.read(watermark);
watermarkImage = await Jimp.read(__dirname + '/wm.png');
// for each image from s3
    const watermarkCopy = watermarkImage.clone();
    // add watermakr
```

这可以通过在全局变量中缓存已经读取的水印图像来进一步优化。

```
const Jimp = require('jimp');
watermarkCache = null;create_watermarks = async (key) => {
    if watermarkCache==null <- read and set variable
    // copy watermark and add to each image
}
```

# 管理函数内部的并发性

**节点。JS** 有事件循环架构。在这种情况下，所有项目都在单个线程中调度和运行。这就是为什么我们在**节点没有观察到任何竞争情况。JS** 并发。事件循环不断轮询异步任务以获得结果。所以有太多的并发任务会使事件循环更长。此外，如果并发任务使用衍生的进程，系统可能会被不必要的颠簸淹没。

```
add_watermark (img, wm) => {add watermark, return new image}
```

现在，如果你用下面的函数从 S3 和水印中加载 100 个潜在的图像键，会发生什么？

```
s3keys_list = [...100 keys...]
await Promise.all(map(add_watermark, s3keys_list))
```

事件循环将发出 100 个并发的 S3 对象请求。这可能会使您超过内存限制，甚至可能会在 S3 或其他地方产生并发费用。我们如何解决这个问题？一个简单的解决方案如下。

```
s3keys_list = [...100 keys...]
tmp_list = []
for(x = 0; x < s3keys_list.length; x++)
{
   // s3keys_list[x] to tmp_list
   // if tmp_list.length == 5
   // await Promise.all(map(add_watermark, tmp_list))
   tmp_list = []
{
// process any remaining items in tmp_list the same way
```

使用这种方法，可以保证有特定的并发性。你不会有一大群超过 RAM 的克隆水印。

除了我讨论的上述技巧之外，我们还可以根据领域和应用程序的知识使用许多其他的优化。此外，我们可以简单地观察日志，并使用它们来进一步调整并发性需求。有时，以牺牲并发性为代价增加一点内存甚至是值得的。这是因为很多人很少使用站点维护端点。

我希望你喜欢阅读这篇文章。这些是我们在开发有许多用户的平台时使用的一些技巧。这有助于我们应对大范围促销活动期间的一次性高需求，在该活动中，许多用户进行了注册。