# 高效迭代您的 R 代码！

> 原文：<https://towardsdatascience.com/iterate-your-r-code-efficiently-3c621998eaea?source=collection_archive---------10----------------------->

## 在 r 中执行干净有效的迭代的一步一步的指南。

因纽特人实际上并没有一百个关于雪的名字。原来这是部分神话和部分误解。将类似的类比扩展到 web speak，大约有一百种迭代代码的方法！除此之外，语法可能会令人困惑，我们中的一些人还会冒险懒散地求助于*复制粘贴*代码 4 次。但重要的是要认识到，这是一条次优路线，坦率地说不切实际。根据一般经验，如果我们需要运行一个代码块两次以上，迭代是一个好主意！这有两个原因可以让你的代码更加丰富

1.  它将注意力吸引到不同的代码部分，从而更容易发现操作的意图。
2.  由于其简洁的本质，您可能会遇到更少的错误。

可能需要一些时间来理解迭代的概念，但是相信我，这是值得的。

既然你已经被说服去迭代——让我们直接开始吧！

让我们挑选内置的 R 数据集- *空气质量。*以下是其中的一个片段-

```
 Ozone Solar.R  Wind Temp Month Day
    41     190    7.4   67     5   1
    36     118    8.0   72     5   2
    12     149   12.6   74     5   3
    18     313   11.5   62     5   4
    23     299    8.6   65     5   7
    19      99   13.8   59     5   8
```

**问题 1** :你想求数据集中每个变量的标准差-

你可以为每一列复制粘贴相同的代码

```
sd(airquality$Ozone)
 *33.27597*sd(airquality$Solar.R)
*91.1523*sd(airquality$Wind)
*3.557713*sd(airquality$Temp)
*9.529969*sd(airquality$Month)
*1.473434*sd(airquality$Day)
*8.707194*
```

然而，这对于大型数据集来说是不切实际的，并且打破了相同操作不运行两次以上的经验法则。我们有一个坚实的案例来迭代！

我们可以写一个`for()`循环-

```
stddev = vector("double", ncol(airquality))for(i in seq_along(airquality))             
{
 stddev[[i]] = sd(airquality[[i]])          

}
stddev
*33.275969 91.152302  3.557713  9.529969  1.473434  8.707194*
```

该循环消除了任何重复，确实比第一种方法更有效。

还必须暂停一下，注意虽然`seq_along()`和`length()`在 for 循环中通常交替使用来构建序列，但是有一个关键的区别。在零长度向量的情况下，`seq_along()`做正确的事情，但是`length()`取 0 和 1 的值。虽然您可能不会故意创建零长度向量，但很容易意外地创建它们。如果你使用`1:length()`而不是`seq_along()`，你可能会得到一个令人困惑的错误信息

或者你可以*跳过*循环，用*只用*一行来自 base R 的`apply()`系列的`sapply()`代码来完成这个技巧

```
sapply(airquality, sd) Ozone      Solar.R      Wind      Temp     Month     Day 
*33.275969 91.152302  3.557713  9.529969  1.473434  8.707194*
```

这是对 R 函数式编程能力的一个很好的应用，确实非常灵活地完成了工作。

现在让我们在复杂性的阶梯上再跨一步，看看另一个问题。

**问题 2** :你想求出你的数据集中每一列的标准差*和*中位数。

因为我们已经确定第一种方法*复制粘贴*是不切实际的，我们权衡我们的迭代选项。

我们从编写一个`for()`循环开始-

```
stddev =vector("double", ncol(airquality))
 median =vector("double", ncol(airquality))for(i in seq_along(airquality))
 {
   stddev[[i]] = sd(airquality[[i]])
   median[[i]] = median(airquality[[i]])
  } stddev
 33.275969 91.152302  3.557713  9.529969  1.473434  8.707194
 median
 31.0 207.0   9.7  79.0   7.0  16.0
```

接下来，我们走函数式编程路线。这里，不像前面的例子，我们可以直接使用 R 的内置`sd()`函数来计算标准偏差并通过`sapply()`传递，我们需要创建一个自定义函数，因为我们需要计算标准偏差和中位数。

```
f <- function(x){
    list(sd(x),median(x))
 }sapply(airquality, f)Ozone    Solar.R Wind     Temp     Month    Day     
33.27597 91.1523 3.557713 9.529969 1.473434 8.707194
31       207     9.7      79       7        16
```

这是一个非常坚实的想法！将用户构建的函数传递给另一个函数的能力令人激动，并且清楚地展示了 R 解决各种任务的函数式编程能力。事实上，经验丰富的 R 用户很少使用循环，而是求助于函数式编程技术来解决所有的迭代任务。如上所述，在 base R 中应用函数族(`apply()`、`lapply()`、`tapply()`等)是一种很好的方式，但是即使在函数式编程领域，也有一个包成为了人们的最爱——*Purrr*。 *purrr* 系列函数具有更加一致的语法，并且内置了执行各种常见迭代任务的功能。

Map() 函数构成了 purrr 迭代能力的基础。这是它的一些表现形式-

*   `map()`列清单。
*   `map_lgl()`做一个逻辑向量。
*   `map_int()`生成一个整数向量。
*   `map_dbl()`生成双向量。
*   `map_chr()`生成一个字符向量。

让我们用这个想法来解决我们之前计算每一列的中值和标准差的问题-

```
map_df(airquality, ~list(med = median(.x), sd = sd(.x)))
```

接下来，为了在复杂性阶梯上的另一个飞跃，让我们从 *gapminder* 库*中挑选 *gapminder* 数据集。下面是其中的一个片段。(注:如果你没有听说过 gapminder 基金会，请点击这里查看它的网站。该基金会在将基本的全球事实放入背景中做了一些开创性的工作)*

```
 country   continent year lifeExp  pop  gdpPercap
  Afghanistan Asia     1952   28.8  8425333  779.
  Afghanistan Asia     1957   30.3  9240934  821.
  Afghanistan Asia     1962   32.0  10267083 853.
  Afghanistan Asia     1967   34.0  11537966 836.
  Afghanistan Asia     1972   36.1  13079460 740.
  Afghanistan Asia     1977   38.4  14880372 786.
```

**问题三:**我想知道各大洲，各年份，哪个国家的人均 GDP 最高？

使用`for()`循环方法-

```
list = c(“continent”, “year”)
DF= data.frame()for( i in list)
{
 df = gapminder %>% group_by_at(i) %>% 
 top_n(1, gdpPercap) %>% 
 mutate(Remark = paste0(“Country Max GDP Per capita in the “,i)) %>% 
 data.frame()
 DF = rbind(df,DF)
}
DF
```

使用`Apply()`方法-

```
do.call(rbind, lapply(list, function(x)
{
  gapminder %>% group_by_at(x) %>% 
    top_n(1, gdpPercap)%>%
    mutate(Remark = paste0("Country with the max GDP Per capita in the ",x)) %>% 
    data.frame
}))
```

使用`Purrr::Map()`方法-

```
gapminder$year = as.character(gapminder$year)
map_dfr(list, ~gapminder %>% group_by(!!sym(.x)) %>% 
 top_n(1, gdpPercap)%>%
 mutate(Remark = paste0(“Country with the max GDP Per capita in the “,.x)) %>% data.frame()
```

以上三种方法都产生相同的输出(为了简洁起见，我没有在这里包括输出。你可以在我的 Github [这里](https://github.com/manasi1096/Iterations-in-R)看看。同样，虽然您可以选择想要采用的迭代路线，但是函数式编程方式在 cogency 方面是明显的赢家。

*Purrr* 也有一些处理日常迭代任务的内置函数！下面我列出了几个受欢迎的。

***任务*** :对每一段数据进行分段回归。(这里，大陆):

***Purrr 解决方案*** :

```
gapminder %>% 
 split(.$Continent) %>% 
 map(~lm(gdpPercap ~ lifeExp, data = .))
```

**任务**:保持变量基于任意条件。(此处，如果变量是因子):

*:*

```
*gapminder %>% 
  keep(is.factor) %>% 
  str()*
```

***任务:**检查任意变量是否满足任意条件(此处，任意变量是否为字符):*

****Purrr 解决方案:****

```
*gapminder%>% 
  some(is_character)*
```

***任务:**检查每个变量是否满足任意条件(这里，如果每个变量都是整数):*

****Purrr 解决方案:****

```
*gapminder %>% 
  every(is.integer))*
```

*一旦你习惯了 *purrr* 的语法，你将需要更少的时间来用 r 编写迭代代码。然而，你绝不能因为用 r 编写循环而感觉不好。事实上，它们是编程的基本模块之一，并且在其他语言中被大量使用。有些人甚至称循环为慢。他们错了！(好吧至少它们已经相当过时了，至于 loops 已经很多年没慢过了)。使用像`map()`和`apply()`这样的函数的主要好处不是速度，而是清晰:它们使你的代码更容易编写和阅读。*

> *重要的是你要解决你正在处理的问题，而不是写出最简洁优雅的代码(尽管那绝对是你想要努力的方向！)——哈德利·韦翰*

*感谢阅读！你可以在我的 Github [这里](https://github.com/manasi1096/Iterations-in-R)查看代码，或者在这里联系我[。](https://www.linkedin.com/in/manasi-mahadik-a66b26146/)*