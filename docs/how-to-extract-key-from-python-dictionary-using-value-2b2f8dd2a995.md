# 如何使用值从 Python 字典中提取键

> 原文：<https://towardsdatascience.com/how-to-extract-key-from-python-dictionary-using-value-2b2f8dd2a995?source=collection_archive---------2----------------------->

## 从 python 字典给定值中提取键的四种方法

如果我们有钥匙，提取价值就像用钥匙开锁一样简单。但是，反过来就没那么简单了，像“*开锁”*，可能！

有时候提取键变得很重要，尤其是当键和值是一对一的关系时。也就是说，当它们中的任何一个是唯一的，因此可以充当键时。

在研究各种方法之前，首先我们将创建一个字典来进行说明。
currency_dict 是以货币缩写为键，货币名称为值的字典。

```
currency_dict={'USD':'Dollar',
             'EUR':'Euro',
             'GBP':'Pound',
              'INR':'Rupee'}
```

如果有键，只需将键加在方括号中就可以得到值。
例如， *currency_dict['GBP']* 将返回 *'Pound '。*

# 方法 1:使用列表

步骤 1:将字典键和值转换成列表。
第二步:从值列表中找到匹配的索引。
步骤 3:使用索引从密钥列表中找到合适的密钥。

```
key_list=list(currency_dict.keys())
val_list=list(currency_dict.values())
ind=val_list.index(val)
key_list[ind]Output: 'GBP'
```

所有这三个步骤可以合并为一个步骤，如下所示:

```
list(currency_dict.keys())[list(currency_dict.values()).index(val)]
```

# 方法 2:使用 For 循环

方法 1 可以使用一个 for 循环稍微修改一下，如下:
步骤 1:将字典键和值转换成列表。
第二步:遍历值列表中的所有值，找到需要的值
第三步:从键列表中返回对应的键。

```
def return_key(val):
    for i in range(len(currency_dict)):
        if val_list[i]==val:
            return key_list[i]
    return("Key Not Found")return_key("Rupee")Output: 'INR'
```

# 方法 3:使用项目()

items()将字典元素保持为键值对。
步骤 1:遍历 item()中的所有键值对。
步骤 2:找到与所需值匹配的值
步骤 3:从键-值对中返回键。

```
def return_key(val):
    for key, value in currency_dict.items():
        if value==val:
            return key
    return('Key Not Found')return_key('Dollar')Output: 'USD'
```

# 方法 4:使用 Pandas 数据框架

在我看来，在转换成数据帧后获取密钥是最简单易懂的方法。
然而，这并不是最有效的方法，在我看来，这应该是*方法 1* 中的一行程序。
字典可以转换成如下熊猫数据帧:

```
df=pd.DataFrame({'abbr':list(currency_dict.keys()),
                 'curr':list(currency_dict.values())})
```

所有键都在列'*缩写'*中，所有值都在数据帧' *df'* 的' *curr'* 列中。
现在查找值非常简单，只需从*【curr】*列的值为所需值的行返回*【缩写】*列的值。

```
df.abbr[df.curr==val]Output: 2    GBP
```

比听起来简单多了！
注意，输出也包含索引。输出不是字符串格式，但它是熊猫系列对象类型。

可以使用许多选项将 series 对象转换为 string，下面是几个选项:

```
df.abbr[df.curr==val].unique()[0]
df.abbr[df.curr==val].mode()[0]
df.abbr[df.curr==val].sum()Output : 'GBP'
```

## 资源

这篇文章的代码可以在[我的 GitHub Repo](https://github.com/abhijith-git/Publications/tree/main/Medium) 中找到。

如果你对视觉格式更感兴趣，你可以看看我的 YouTube 视频

作者的 YouTube 教程

## 成为会员

我希望你喜欢这篇文章，我强烈推荐 [**注册*中级会员***](https://abhijithchandradas.medium.com/membership) 来阅读更多我写的文章或成千上万其他作者写的各种主题的故事。
[你的会员费直接支持我和你看的其他作家。你还可以在 Medium](https://abhijithchandradas.medium.com/membership) 上看到所有的故事。

## 这里是我的一些其他文章，你可能会觉得有趣…干杯！

[](https://abhijithchandradas.medium.com/how-to-embed-code-in-medium-4bd380c8102d) [## 如何在媒体中嵌入代码

### 在你的媒体文章中嵌入代码的两种简单方法。

abhijithchandradas.medium.com](https://abhijithchandradas.medium.com/how-to-embed-code-in-medium-4bd380c8102d) [](/how-to-add-text-labels-to-scatterplot-in-matplotlib-seaborn-ec5df6afed7a) [## 如何在 Matplotlib/ Seaborn 中向散点图添加文本标签

### 使用 Seaborn 或 Matplotlib 库时，如何在 python 中向散点图添加文本标签的分步指南。

towardsdatascience.com](/how-to-add-text-labels-to-scatterplot-in-matplotlib-seaborn-ec5df6afed7a) [](/covid-19-bar-race-chart-using-flourish-89136de75db3) [## 新冠肺炎酒吧比赛图表使用繁荣

### 竞赛条形图是一个非常简单的工具，可以用来比较随时间变化的数量。简单来说，就是一个条形图…

towardsdatascience.com](/covid-19-bar-race-chart-using-flourish-89136de75db3) ![](img/002fca2de3be71601bb12acae7e2c30b.png)

照片由 [Hitesh Choudhary](https://unsplash.com/@hiteshchoudhary?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄