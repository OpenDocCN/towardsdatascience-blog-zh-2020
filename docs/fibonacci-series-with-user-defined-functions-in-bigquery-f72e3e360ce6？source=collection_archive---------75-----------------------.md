# BigQuery 中的斐波那契数列

> 原文：<https://towardsdatascience.com/fibonacci-series-with-user-defined-functions-in-bigquery-f72e3e360ce6?source=collection_archive---------75----------------------->

## 在 BigQuery 中使用用户定义的 JavaScript 函数来计算 Fibonacci

BigQuery 有一些很好的功能来支持用 SQL 和 JavaScript 编写的用户定义函数，所以我想我会很乐意用 Fibonacci 数来解决这个问题。

![](img/24839f9b2c70567ce81660b1dd29e0aa.png)

自然界中的斐波那契数列——图片来源: [pxfuel](https://www.pxfuel.com/en/free-photo-qqlvf)

我很确定我在 [Project Euler](https://projecteuler.net) 或者类似的地方看到过这个挑战，所以请不要用这个来作弊。挑战在于:

找出前 1000 个斐波那契数列的最后 6 个 6 位数的总和。现在，这听起来可能有点复杂，所以我们将从计算完整的斐波纳契数开始，然后从那里开始。

# 什么是斐波那契数列？

该序列被定义为:

*   a(0) = 0
*   a(1) = 1
*   a(n) = a(n-2) + a(n-1)

所以每个元素都是前面 2 个的和。这就是它的作用:

**0，1，1，2，3，5，8，13，21…**

可以想象这些数字会很快变大。例如，第 50 个数字是 *12，586，269，025* ，即超过 120 亿，带 b！(第 100 个号码是:354224848179261915075😬)

# 在 BigQuery 中我们到底是如何做到这一点的？

现在我们知道了什么是斐波那契数列，显而易见的问题是我们如何在 BigQuery 中进行循环。嗯，有很多方法可以通过 BigQuery 脚本来实现，但是这里我将使用 JavaScript 用户定义的函数来代替。

下面是计算第 n 个斐波那契数列的 JavaScript 函数:

我对 JavaScript 一无所知，所以如果我在这里做了什么傻事，请在评论中指出我！

如果你想玩玩它，这里是它的行动。

# 使用该功能的时间

为了在 BigQuery 中创建这个函数，我们需要使用`CREATE TEMP FUNCTION`声明:

我们可以使用与上面相同的 JavaScript 函数体。我们需要做的就是从 JavaScript 位中删除`function fib(n)`位，并将输入和输出类型添加到 SQL 位。请注意我们如何使用`fibonacci(n INT64)`来声明输入必须是整数，使用`RETURNS INT64`来指定输出类型。

让我们在大查询中得到一些数字:

如果你不知道数组生成器和`with`语句中的 unnesting 发生了什么，那么看看我关于 [FizzBuzz 和 BigQuery](https://medium.com/@niczky12/fizzbuzz-in-bigquery-e0c4fbc1d195) 的文章🍾—都在那解释了。

结果如下:

```
╔═════╦═════╦═════════════╗
║ Row ║ num ║    fib_n    ║
╠═════╬═════╬═════════════╣
║   1 ║   0 ║           0 ║
║   2 ║   1 ║           1 ║
║   3 ║   2 ║           1 ║
║   4 ║   3 ║           2 ║
║   5 ║   4 ║           3 ║
║   6 ║   5 ║           5 ║
║   7 ║   6 ║           8 ║
║   8 ║   7 ║          13 ║
║   9 ║   8 ║          21 ║
║  10 ║   9 ║          34 ║
║  11 ║  10 ║          55 ║
║  12 ║  20 ║        6765 ║
║  13 ║  30 ║      832040 ║
║  14 ║  40 ║   102334155 ║
║  15 ║  50 ║ 12586269025 ║
╚═════╩═════╩═════════════╝
```

这花了 0.5s 运行。不算太差，而且看起来工作得很好，因为我们得到了与上面相同的第 50 个元素。

# 现在是真正的挑战

如果你回到顶部，你可以看到最初的挑战是计算前 1000 个斐波那契数的最后 6 位数的总和。现在这样做很容易。这里的技巧是要认识到，为了精确计算最后 6 位数字，你只需要最后 6 位数字，所以我们可以通过使用模运算符来丢弃其余的数字。我们需要这个技巧，否则我们的`INT64`会变得太大而溢出。

我们只需要在我们的 JavaScript 函数中添加:`var new_num = (numbers[0] + numbers[1]) % 1000000;`，然后对所有结果求和。以下是用于此目的的 SQL:

这用了 0.9 秒，所以 BigQuery 现在真的是汗流浃背…结果是:

```
╔═════╦═══════════╗
║ Row ║  result   ║
╠═════╬═══════════╣
║   1 ║ 477632375 ║
╚═════╩═══════════╝
```

我建议您更改上面的 SQL，并查看第 100 个斐波那契数列的最后 6 位数字，看看是否正确！

# 包扎

总的来说，JavaScript 函数很容易添加到 BigQuery 中，您可以一起破解一些有趣的计算。更好的是，如果您在 Google 云存储桶中包含了`.js`文件，您甚至可以使用 [JavaScript 库](https://cloud.google.com/bigquery/docs/reference/standard-sql/user-defined-functions#including-javascript-libraries)。多棒啊。

[](/load-files-faster-into-bigquery-94355c4c086a) [## 将文件更快地加载到 BigQuery 中

### 针对摄取的 CSV、GZIP、AVRO 和拼花文件类型进行基准测试

towardsdatascience.com](/load-files-faster-into-bigquery-94355c4c086a) [](/loops-in-bigquery-db137e128d2d) [## BigQuery 中的循环

### 了解如何使用 BigQuery 脚本来计算斐波那契数

towardsdatascience.com](/loops-in-bigquery-db137e128d2d)