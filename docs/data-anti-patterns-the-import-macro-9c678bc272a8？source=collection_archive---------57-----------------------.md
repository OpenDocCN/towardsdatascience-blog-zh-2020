# 数据反模式—导入宏

> 原文：<https://towardsdatascience.com/data-anti-patterns-the-import-macro-9c678bc272a8?source=collection_archive---------57----------------------->

![](img/98b76220e21e4f571ec3f827a2b330a2.png)

作者照片

大多数反模式都是从堆栈溢出或者更好的 Github 开始的。假设我需要将数据导入到我的分析中，并且我想要获得数据的最快方法。代码片段和电子表格的世界让我可以毫不费力地导入数据和生产数据产品。

什么是数据反模式？它们是看起来合理的解决方案，但是可能不可伸缩、不安全、引入了质量问题，或者对良好的数据治理有所冒犯。

没有人打算实现反模式。说选举快到了；确实是！我需要一些数据来分析。

与其麻烦数据工程师加载我需要的数据，为什么不在 Github 上找到一个电子表格宏，自己把它导入到我的模型中呢？

我使用著名的数据源 FiveThirtyEight，并将我需要的投票数据嵌入到我的电子表格中。

```
=ImportCSV("[https://projects.fivethirtyeight.com/polls-page/senate_polls.csv](https://projects.fivethirtyeight.com/polls-page/senate_polls.csv)")
```

反模式已实现。我有数据。它很小，所以加载很快，我可以开始我的分析。以下是一些问题:我阻止了别人检查或监控我的数据。我也排除了自己跟踪这些数据的演变。

怎么修？首先，中转位置应该是共享的、可靠的和安全的。在 AWS 中，暂存 S3 的数据。在 Azure 中，它将是 blob 存储。在谷歌上，它将是一个云存储桶。

电子表格不能代替数据库。确定数据库是否足够好的关键语句是，“它支持 COPY 语句吗？”Postgres 将扩展到万亿字节级别，而像雪花这样的数据库允许您将数据处理与数据存储分开。

遵循以下最佳实践:

1.  创造一个舞台
2.  将数据复制到 Stage 表中，表名为第行的唯一性，并且**不要**删除或覆盖该数据

```
copy into OWLMTN.STAGE.SENATE_POLLS (FILENAME, 
                                     FILE_ROW_VARCHAR,
                                     question_id, ...        
                                     candidate_party, 
                                     pct
    )
    from (
        select METADATA$FILENAME        filedate,
               METADATA$FILE_ROW_NUMBER filename,
               t.$1, ...
               t.$37,
               t.$38
        from @owlmtn.stage.FIVE_THIRTY_EIGHT_POLLS t
    )
    pattern = 'stage/fivethirtyeight/polls/SENATE_POLLS_.*\.csv'
    on_error = continue
    force = false
    file_format = (field_optionally_enclosed_by = '"'
        type = 'csv'
        field_delimiter = ','
        skip_header = 1
        encoding = 'utf-8');
```

# 结论

反模式通常只有在大范围内才变得可见。来自 Stack Overflow 或 Github 的解决方案会给数据基础设施带来严重的操作问题。在解决数据问题时，利用您的组织基础设施。如果没有，就从一个支持 COPY 的可靠的 SQL 数据库开始。

在 FiveThirtyEight 选举数据的情况下，反模式会将选举数据带入我的电子表格，但是这些数据很快就没用了。通过将数据存储在关系存储中并保留所有数据，我可以在 11 月 3 日之后使用历史数据来验证模型。