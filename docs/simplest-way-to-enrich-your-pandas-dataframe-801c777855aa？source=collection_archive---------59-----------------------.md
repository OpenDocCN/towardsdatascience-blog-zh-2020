# 最简单的方法来丰富你的熊猫数据框架

> 原文：<https://towardsdatascience.com/simplest-way-to-enrich-your-pandas-dataframe-801c777855aa?source=collection_archive---------59----------------------->

![](img/c190c4ba8e81aa2a60b3494c431c6022.png)

作者图片

## 熊猫真的很棒，有深度，有文档。您可以快速导航并了解有关重塑数据、处理不同格式(如文本、时间序列、分类数据、合并、可视化等)的一切。但是，如果我说你的数据框架可以通过 https 与外部 API“对话”,你会怎么想？

如果你想一想，这里没有使用革命性的技术。我敢打赌，如果你现在停止阅读，思考几分钟如何让你的数据框架从“外部世界”获取数据，你会很快意识到事情很简单:

```
df.apply(lambda i: get_result(i[‘input_column_1’], i[‘input_column_2’]), axis=1)
```

其中 get_result()是一个与数据框架之外的世界“对话”的函数。在本例中，它采用 2 个参数作为输入:数据帧中的 2 列“输入 _ 列 _1”和“输入 _ 列 _2”。

这种方法有什么好处？

首先你需要意识到在 get_result()函数背后可能隐藏着什么。这里有很多选项，简单地说，任何将从外部 api、数据库或您能想到的任何其他东西获取数据的客户端库。

这里唯一的限制是您的结果必须以某种方式放回 dataframe 行:

```
df[‘result’] = df.apply(lambda i: 
    get_result(i[‘input_column_1’], i[‘input_column_2’]), axis=1)
```

*注意:我们可以简化我们的。跳过 lambda:* 应用()函数

```
*df.apply(get_result, axis=1)*
```

*和处理熊猫系列里面的 get_result()函数。我更喜欢保持客户端 API 逻辑干净，避免不必要的 Pandas 处理，特别是如果来自其他系统的预测可能没有在输入上使用 Pandas 系列。*

**还不见效益？**

这里最重要的是，您不必将数据帧转换为其他格式，或者将其保存在 API 客户端类可以从中访问数据、获取 API 结果并创建结果与原始数据帧合并的数据帧的地方。你只是一直呆在数据框“领域”里！

有些事情你必须知道。例如，get_result()必须处理出错且没有数据返回的情况。您需要使它适应您的情况，但是您可以尝试做的最简单的事情是实现自动“失败时恢复”。

另一种方法是用空值填充数据帧的所有失败结果。然后再次调用 apply()并只处理失败的行:

```
df[df.result.isnull()].apply( … )
```

**“分批谈”**

到目前为止一切顺利，但是如果您的外部 API 批量接受并返回数据会怎么样呢？假设你的数据框架有一个文本列，你想用基于该文本的情感分析预测来丰富你的数据框架。在这种情况下，您必须对数据帧进行切片，并将每批数据传递给我们的 API 客户端函数:

```
batch_size = 100for i in range(0, len(df), batch_size):
    df.loc[df.index[i:i+batch_size], 'sentiment_prediction']
        df.loc[df.index[i:i+batch_size],'text_column']\
            .apply(lambda i: 
                   predict_sentiment(i['text_column']), axis=1)
```

我们的数据帧正在按 batch_size 进行切片，其中“text_column”被发送到 be predict _ perspective()函数。

这两种解决方案都有一个问题。apply()逐行执行，或者在批处理的情况下逐批执行。换句话说，API 调用是按顺序执行的。

**加速发展/走向“大数据”**

像往常一样，这可以通过多种方式完成，但我将使用 Dask 来完成这项工作。

*注意:这篇文章不是关于 Dask 的介绍，如果你不熟悉它，去看看关于 dask.org*[](http://dask.org)**的真正友好的文档。**

*只需几个额外的步骤，您就可以将数据帧转换为 Dask 数据帧，从而处理内存不足的数据集。Dask 的伟大之处在于它试图尽可能地“模仿”熊猫 API。您可以在您的笔记本电脑上使用“本地集群”进行开发，或者使用多台机器相当容易地设置实际的生产就绪集群。*

*现在，使用 Dask dataframe 可以加快速度！假设我们的 API 可以同时处理多个请求，但是我们也应该尽量不要同时处理太多的请求。*

*因为 Dask 数据帧仅仅是 Pandas 数据帧的分区列表，所以对其调用 apply()将在每个分区上分别分配工作和并行执行。*

*让我们看看我们目前有多少分区:*

```
*>> df.npartitions
14*
```

*让我们更改块大小以获得 50 个分区*

```
*>> df = df.repartition(npartitions=50)
>> df.npartitions
50*
```

*现在调用 apply()将生成至少 50 个并行任务！让我们修改我们的代码以适应 Dask *(注意:这里的 df 是 Dask dataframe，不是 Pandas dataframe)* :*

```
*df[‘result’] = df.apply(lambda i: 
    get_result(i[‘input_column_1’], i[‘input_column_2’]), axis=1)*
```

*有什么变化？没什么！甜甜:)*

**注意:这里假设您正在使用有许多工作人员的集群。如果你在你的笔记本电脑上这样做，把你的分区数量设置为内核数量。**

***扩展批次***

*这里事情变得有点复杂，因为我们需要:*

*   *将我们的 Dask 数据帧分割成单独的 Pandas 数据帧(分区)*
*   *在每个分区上创建批处理*
*   *更新每个熊猫数据帧，这将导致更新整个 Dask 数据帧*

*Dask 没有提供多少方法来做到这一点。我将使用 map_partitions()，它只是在每个分区上应用函数:*

```
*batch_size = 100def make_per_dataframe_predictions(pdf): # pdf is Pandas dataframe
   for i in range(0, len(pdf), batch_size):
     pdf.loc[pdf.index[i:i+batch_size], 'sentiment_prediction'] = \
       pdf.loc[pdf.index[i:i+batch_size], ['text_column']]\
         .apply(lambda i: get_result(i['text_column']), axis=1)
return pdfdf.map_partitions(make_per_dataframe_predictions).compute()*
```

**注意:pdf.loc 索引在额外的括号中有“text_column ”,即使我们传递的是单列。因为我们想。在 dataframe 上应用()，如果没有括号，我们将对 Pandas 系列进行操作。**

*另外，请注意 Dask 要求。compute()实际结果。*

***警告词***

*在盲目应用上面介绍的任何解决方案之前，你需要考虑它如何适合你的情况。*

*一般来说，特别是在处理真正的大数据时，我建议避免这种新的“强大的知识”,并使用以下步骤将您的管道分成单独的任务:*

*   *[…在外部 API 的数据“丰富”您的数据框架之前，完成所有步骤…]*
*   *存储您希望用作 API 输入的数据帧和(可能是单独的)输入*
*   *编写单独的任务来加载预测的输入，生成预测并存储它们*
*   *加载你的数据框架，将你的预测加载到另一个数据框架中，然后合并它们*

*是的，看起来需要更多的工作，但是这种方法更安全，并且允许对每个步骤有更大的控制。*

*记住:这篇文章的目的不是向你展示你的数据框架如何与外界交流的实际技术。更多的是心态，如果运用得当，可以为你节省很多时间。*