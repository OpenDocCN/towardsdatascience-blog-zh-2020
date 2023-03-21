# 使用朴素贝叶斯和 TextAnalysis.jl 创建垃圾邮件过滤器

> 原文：<https://towardsdatascience.com/implementing-a-spam-filter-using-naive-bayes-and-textanalysis-jl-on-julia-e321180ea301?source=collection_archive---------23----------------------->

![](img/0c06e787dd13043efcbe34a7b9f6edf1.png)

Christopher Gower 在 [Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

你好！今天，我将告诉你更多关于我做了什么来制作一个垃圾邮件过滤器，使用朴素贝叶斯从 UCI 机器学习的 kaggle 上的数据集中检测垃圾邮件数据，以及在 Julia 上使用 [TextAnalysis.jl](https://github.com/JuliaText/TextAnalysis.jl) 。

我从查看 TextAnalysis.jl 的文档开始，以了解更多关于 [NaiveBayes 分类器](https://juliatext.github.io/TextAnalysis.jl/latest/classify/)的工作原理。

```
using TextAnalysis: NaiveBayesClassifier, fit!, predict
m = NaiveBayesClassifier([:legal, :financial])
fit!(m, "this is financial doc", :financial)
fit!(m, "this is legal doc", :legal)
predict(m, "this should be predicted as a legal document")
```

我运行了文档中的示例，并了解到函数 NaiveBayesClassifier 接受一组可能的类的参数，这些数据可能属于这些类。

在这种情况下，是`:legal`和`:financial`。我还了解到，我们通过用`fit!`函数拟合相关数据来训练模型，该函数接受模型本身的参数、我们试图训练的数据串以及数据所属的类。这里的数据是一个字符串，例如`“this is financial doc”`，在本例中，它所属的类是`:financial`。

最后，我了解到`predict`函数允许我们输入一串数据，并使用 NaiveBayesClassifier 算法，根据使用`fit!`函数之前训练的数据串来预测该字符串属于哪个类。`predict`函数接受模型本身的参数以及我们试图预测的数据字符串。

我解决这个问题的第一个方法是，我认为首先导入所有的数据是一个好主意。因为我有使用 [CSV.jl](https://github.com/JuliaData/CSV.jl) 和 [DataFrames.jl](https://github.com/JuliaData/DataFrames.jl) 软件包的经验，所以我熟悉数据的导入。

```
using CSV, DataFrames
spamdata = DataFrame(CSV.read("spam.csv"; allowmissing=:none))---------------------------------------------------------------julia> showall(spamdata)
5572×5 DataFrame
│ Row  │ v1     │
│      │ String │
├──────┼────────┤
│ 1    │ ham    │
│ 2    │ ham    │
│ 3    │ spam   │
│ 4    │ ham    │
│ 5    │ ham    │
│ 6    │ spam   │
│ 7    │ ham    │
│ 8    │ ham    │
│ 9    │ spam   │
│ 10   │ spam   │
```

下图显示了包含数据的原始`.csv`文件的结构。

![](img/e04ba7afe049c5de997759a620839706.png)

csv 文件有两列。`v2`是我们想要用来训练的数据串，`v1`是特定数据串的类，对应于`v2`。

我想要一种方法来循环遍历文件的每一行，并在一个条件下拆分 ham 数据，在另一个条件下拆分 spam 数据，这样，当进行训练时，我将能够使用`fit!`函数来训练`ham`数据，这需要我指定该数据的类别。

```
for row in eachrow(spamdata)
    if row.v1 == "ham"
        println("ham")
    elseif row.v1 == "spam"
        println("spam")
    end
end
```

这是一个成功，我最终有火腿和垃圾邮件被打印！

```
ham
spam
ham
spam
ham
ham
spam
spam
ham
spam
⋮
```

既然已经解决了，我可以使用 NaiveBayesClassifier 函数来定义我的模型了。我想定义两个类，`:ham`和`:spam`。

```
using TextAnalysis: NaiveBayesClassifier, fit!, predict
m = NaiveBayesClassifier([:ham, :spam])
```

接下来，我想开始训练我的模型。正如我们从最初的`.csv`文件的结构中看到的，`v2`是我们试图训练的字符串。将`for`循环与`fit!`函数结合起来，我做了以下工作来尝试训练我们所有可用的数据。

```
using CSV, DataFrames
using TextAnalysis: NaiveBayesClassifier, fit!, predictspamdata = DataFrame(CSV.read("spam.csv"; allowmissing=:none))
global m = NaiveBayesClassifier([:ham, :spam])
for row in eachrow(spamdata)
    if row.v1 == "ham"
        fit!(m, row.v2, :ham)
    elseif row.v1 == "spam"
        fit!(m, row.v2, :spam)
    end
end
```

但是当我运行它时，我得到了下面的错误:`LoadError: Base.InvalidCharError{Char}('\xe5\xa3')`

就在那时，我意识到数据集在`v2`列的某些字符串中有无效字符。为了消除这个错误，我们需要使用下面的函数过滤掉不支持的字符:`filter(isvalid, <string>)`

```
for row in eachrow(spamdata)
    if row.v1 == "ham"
        fit!(m, filter(isvalid, row.v2), :ham)
    elseif row.v1 == "spam"
        fit!(m, filter(isvalid, row.v2), :spam)
    end
end
```

当我将字符串`row.v2`替换为`filter(isvalid, row.v2)`并再次运行程序时，没有出现错误。因此，该模型被成功训练，到目前为止一切顺利！

在 REPL 中，实际上什么都没有打印出来，因为没有错误，程序中没有任何东西被打印出来。为了全面测试模型是否有效，我们可以尝试创建一个预测，并打印出预测的结果，以查看训练是否真正完成。

```
prediction1 = predict(m, "hello my name is kfung")
prediction2 = predict(m, "text 31845 to get a free phone")
println(prediction1)
println(prediction2)
```

在这里，我创建了两个预测，其中第一个看起来像一个火腿消息(因为它看起来不可疑)，第二个看起来像一个垃圾邮件，以测试模型。

我得到的结果如下:

```
Dict(:spam => 0.013170434049325023, :ham => 0.986829565950675)
Dict(:spam => 0.9892304346396908, :ham => 0.010769565360309069)
```

正如我们所知，预测非常准确，因为第一个预测的`:ham`值接近 1，这意味着它最有可能是一封垃圾邮件，第二个预测的:`spam value`值接近 1，这意味着它最有可能是一封垃圾邮件。正如我们所料。

但是对于不熟悉字典或 Julia 语法的用户来说，他们可能会对上面的字典的含义感到困惑。我修改了代码，以便它检查字典中的`:spam`和`:ham`值，并打印出这两个值中较大值的类。

```
prediction = predict(m, "hello my name is kfung")
if prediction[:spam] > prediction[:ham]
    println("spam")
else
    println("ham")
end
```

正如我们所料，这个程序的结果是字符串`“ham”`，因为它预测的`:ham`值大于`:spam`值，因此它更有可能是一个火腿消息。

最后，我将所有内容都包装在一个函数中，该函数以一个字符串作为参数，这样当您用一个字符串调用该函数时，如果模型预测它是垃圾邮件，它将输出`“spam”`,如果模型预测它不是垃圾邮件，则输出`“ham”`。

```
using CSV, DataFrames
using TextAnalysis: NaiveBayesClassifier, fit!, predictfunction checkspam(msg::String)
    spamdata = DataFrame(CSV.read("spam.csv"; allowmissing=:none))
    m = NaiveBayesClassifier([:ham, :spam])
    for row in eachrow(spamdata)
        if row.v1 == "ham"
            fit!(m, filter(isvalid, row.v2), :ham)
        elseif row.v1 == "spam"
            fit!(m, filter(isvalid, row.v2), :spam)
        end
    end
    prediction = predict(m, msg)
    if prediction[:spam] > prediction[:ham]
        println("spam")
    else
        println("ham (not spam)")
    end
end
```

最后，我意识到每次对信息进行分类时，我是如何训练模型的。这使得程序在运行时效率不高，因为每次我们试图使用模型进行预测时，它都要从头到尾检查 5600 行数据。相反，我们可以将模型置于函数之外(这样它只在开始时运行一次)，并将模型存储在一个全局变量中，这样以后它就可以使用存储在全局变量中的预训练模型来分类任何其他消息。

```
using CSV, DataFrames
using TextAnalysis: NaiveBayesClassifier, fit!, predictspamdata = DataFrame(CSV.read("spam.csv"; allowmissing=:none))
global m = NaiveBayesClassifier([:ham, :spam])
for row in eachrow(spamdata)
    if row.v1 == "ham"
        fit!(m, filter(isvalid, row.v2), :ham)
    elseif row.v1 == "spam"
        fit!(m, filter(isvalid, row.v2), :spam)
    end
endfunction checkspam(msg::String)
    prediction = predict(m, msg)
    if prediction[:spam] > prediction[:ham]
        println("spam")
    else
        println("ham (not spam)")
    end
end
```

总的来说，我认为这是创建新的垃圾邮件过滤器的一次非常成功的尝试。我学到了更多关于如何应用朴素贝叶斯为概率分类的独立假设建模的知识。朴素贝叶斯目前仍然是文本分类的一个相当受欢迎的选项，这就是一个例子，并且可以与其他模型训练方法形成对比，例如逻辑回归，其中数据是有条件训练的。这个模型还有其他应用，比如情绪分析，这是我将在另一篇文章中重点介绍的，所以请继续关注！

非常感谢您的阅读！