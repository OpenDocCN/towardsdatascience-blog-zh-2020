# 为什么建立机器学习模型就像烹饪一样

> 原文：<https://towardsdatascience.com/why-building-a-machine-learning-model-is-like-cooking-4bed1f6115d1?source=collection_archive---------48----------------------->

## 逐步比较

![](img/efed8b10d3b5e8fb587f19144bdbac74.png)

来自 [Pexels](https://www.pexels.com/photo/pensive-grandmother-with-granddaughter-having-interesting-conversation-while-cooking-together-in-light-modern-kitchen-3768146/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 Andrea Piacquadio 的照片

W 当我第一次成为[数据科学家时，我意识到机器学习是一个模糊的概念，人们听说过但不太理解](/my-experience-as-a-data-scientist-vs-a-data-analyst-91a41d1b4ab1)。我努力用一种非技术人员也能理解的方式来解释机器学习。

快进到今天，我突然意识到，建立一个机器学习模型就像烹饪——一项人人都可以参与的普遍活动，除非你对烹饪的想法是把冷冻晚餐扔进微波炉。事不宜迟，让我带你了解构建机器学习模型就像烹饪一样。

## 1.数据准备

建立机器学习模型的第一步是准备数据。根据数据基础设施的不同，这可能涉及从各种来源提取原始数据并加载到数据库中。在成熟的公司中，数据在数据库中，数据科学家只需要找到模型所需的数据。

同样，烹饪的第一步是获取原料(数据)。你可能需要去杂货店买家里没有的食材(从各种来源拉)。

## 2.探索性数据分析

接下来，数据科学家研究数据以分析趋势，删除重复和缺失值等不良数据，并将其转换为可用于建模的形式。对于可用的形式，我的意思是如果模型期望每个用户一行，那么数据需要被聚合和转置以满足这个需求。

类似地，你必须探索配方成分，以决定你是想用新鲜的还是冷冻的，或者如果你在商店找不到的话，用替代品代替。食谱可能要求配料在加入菜肴之前进行预混合或预烹饪(转化为可用的形式)。

## 3.选择一个模型

第三步，根据建模问题选择一个[模型算法](/do-you-know-how-to-choose-the-right-machine-learning-algorithm-among-7-different-types-295d0b0c7f60)。

这类似于选择烹饪方法——烤、炸、蒸等。

## 4.训练模型

现在，数据科学家将使用选定的算法来训练模型。为了确定模型的准确性，从训练中拿出一部分数据来评估模型使用从未见过的数据预测结果的能力。

同样，你按照食谱烹饪你的菜肴，并把它的味道与你过去做过的菜肴进行比较。

## 5.评估模型结果

数据科学家检查模型结果，并根据结果，使用另一种模型算法重复步骤 3 和 4。有时，数据科学家可能需要从步骤 1 重新开始，以评估是否可以引入任何新数据来改进模型结果。

根据你对菜肴味道的满意程度，你可以调整你的烹饪方法或者用不同的材料重新开始。

## 6.参数调整(可选)

可以调整模型参数以提高精度，但如果模型结果可以接受，这是一个可选步骤。

在烹饪中，如果你对菜肴的味道不满意，可以调整配料或调味料的比例。

## 7.将模型部署到生产中

使用历史数据训练模型。在模型被训练之后，它将被投入生产，在那里它被用来使用当前的数据预测未来的结果。

同样，一旦你准备好了你的菜，并且对它的味道感到满意，你就可以把它端给你的家人和朋友了。

## 8.模型再训练

这是很少提到的最后一步。已经投入生产的模型偶尔需要用更多的最新数据[重新训练](https://hackernoon.com/how-to-keep-your-machine-learning-models-up-to-date-vd5z3yzw)。模型使用某个时间点的数据来预测未来的用户行为。如果用户行为随时间变化，模型就不能捕捉到这一点，预测精度就会下降。由于疫情，这一点尤其如此。用户行为在 2020 年发生了巨大变化，使用一年前的数据建立的模型预测将受到影响。

我想到的最贴切的烹饪比喻是，如果你的菜需要季节性的或关键的配料，而这些配料并不容易获得。在这种情况下，你使用了一种接近的替代方法，但是这道菜没有原来的食谱好。

下次你听数据科学家谈论他们的机器学习模型时，我希望烹饪成为你的通用翻译器。

## 你可能也会喜欢…

[](https://medium.com/swlh/how-i-used-a-machine-learning-model-to-generate-actionable-insights-3aa1dfe2ddfd) [## 我如何使用机器学习模型来生成可操作的见解

### 将数据科学与数据分析相结合

medium.com](https://medium.com/swlh/how-i-used-a-machine-learning-model-to-generate-actionable-insights-3aa1dfe2ddfd) [](/how-to-translate-machine-learning-results-into-business-impact-d0b323112e87) [## 如何将机器学习成果转化为商业影响

### 向高管解释模型结果

towardsdatascience.com](/how-to-translate-machine-learning-results-into-business-impact-d0b323112e87)