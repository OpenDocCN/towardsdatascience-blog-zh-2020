# 如何预测 Sparkify 中的搅拌

> 原文：<https://towardsdatascience.com/how-to-predict-churns-in-sparkify-ab9a5c3f218d?source=collection_archive---------63----------------------->

## 使用 Spark 预测哪些用户有可能因取消服务而流失

![](img/fa56b75f9d2b521e39374c370c3a071b.png)

由[马尔特·温根](https://unsplash.com/@maltewingen?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

**简介**

parkify 是一种流行的数字音乐服务，类似于由 Udacity 创建的 Spotify 或 Pandora。该项目的目标是预测哪些用户有被取消服务的风险。如果我们能在这些用户离开之前识别出他们，我们就能为他们提供折扣或激励。

用户可以做的不同页面动作是，例如，播放一首歌、喜欢一首歌、不喜欢一首歌、登录、注销、添加朋友或者在最坏的情况下取消服务。我们所知道的关于用户的信息是 id、名字和姓氏、性别、他们是否登录、他们是否是付费用户、他们的位置、他们注册的时间戳以及用户使用的浏览器。我们对他们正在听的歌曲的了解是艺术家的名字和歌曲的长度。

![](img/2a0bb98f0d54c2315e8bb55248bf6893.png)

作者在 Sparkify data 上的图片

**第一部分:用户故事示例**

让我们以单个用户为例来更好地理解用户可以做的交互。让我们研究一下来自北卡罗来纳州罗利的用户 id 2 Natalee Charles 的三个操作。

```
Row(artist=None, auth='Logged In', firstName='Natalee', gender='F', **itemInSession=1**, lastName='Charles', length=None, level='paid', location='Raleigh, NC', method='GET', **page='Home'**, sessionId=1928, song=None, status=200, userAgent='"Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/36.0.1985.143 Safari/537.36"', userId='2', timestamp='2018-11-21 18:04:37', registration_timestamp='2018-09-13 02:49:30'),Row(**artist='Octopus Project'**, auth='Logged In', firstName='Natalee', gender='F', **itemInSession=2**, lastName='Charles', **length=184.42404**, level='paid', location='Raleigh, NC', method='PUT', **page='NextSong'**, sessionId=1928, **song='What They Found'**, status=200, userAgent='"Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/36.0.1985.143 Safari/537.36"', userId='2', timestamp='2018-11-21 18:04:45', registration_timestamp='2018-09-13 02:49:30'),Row(**artist='Carla Bruni'**, auth='Logged In', firstName='Natalee', gender='F', **itemInSession=3**, lastName='Charles', length=123.48036, level='paid', location='Raleigh, NC', method='PUT', page='NextSong', sessionId=1928, song="l'antilope", status=200, userAgent='"Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/36.0.1985.143 Safari/537.36"', userId='2', timestamp='2018-11-21 18:07:49', registration_timestamp='2018-09-13 02:49:30'),...Row(artist=None, auth='Logged In', firstName='Natalee', gender='F', itemInSession=106, lastName='Charles', length=None, level='paid', location='Raleigh, NC', method='PUT', **page='Add Friend'**, sessionId=1928, song=None, status=307, userAgent='"Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/36.0.1985.143 Safari/537.36"', userId='2', timestamp='2018-11-21 23:30:04', registration_timestamp='2018-09-13 02:49:30')
```

第一个动作是会话中的第一个动作:访问主页。第二个动作是听一首歌，这允许我们收集关于歌曲本身的信息:艺术家、歌曲和长度。第三个动作是听一首不同的歌(卡拉·布鲁尼:l'antilope)。整个会话的最后一个操作是添加一个朋友。

**第二部分:识别特征**

用于搅动的用户的定义是提交取消确认的用户。我们需要找到代表两种用户类型之间巨大差异的特征:一个用户频繁使用，一个用户不频繁使用。困难在于，数据中包含的搅动数(233290)比非搅动数(44864)多得多，这意味着，尽管数据不平衡，我们也必须找到具有可比性的特征。

![](img/02bd712582762db78778ec7ed35cccfe.png)

作者图片

第一个想法是按会话比较项目，因为会话的持续时间不受用户数量的影响，只受会话中项目数量的影响。音乐服务平台最重要的项目是歌曲的播放。这导致决定比较两种用户类型之间的*下一首歌曲*的数量。恶心的人最多听 708 首歌，而不恶心的人最多听 992 首。流失用户平均听了 70 首歌，非流失用户平均听了 105 首。这是一个很好的特性，因为我们已经看到了这两种用户类型的很多差异。

![](img/e4ebb256b690b3a787a218bd301226dc.png)

作者图片

第二个想法是检查两种用户类型之间的比例差异。数据中已经有用户类型定义，级别(免费/付费用户)。我们想研究搅动和非搅动的比例分布是否不同。我们可以看到，非用户的付费用户比例高于用户，而用户的免费用户比例也高于用户。这也是一个很棒的特性，因为在这种情况下，免费用户比付费用户更容易流失。

**第三部分:建模**

我们使用逻辑回归来寻找预测搅拌的最佳模型。因为我们有一个二元决策，所以我们使用 BinaryClassificationEvaluator 来评估结果。由于被搅动的用户是一个相当小的子集，我们使用 f1 分数(结合准确度)作为优化的指标。我们使用三种不同的输入类型来测试，例如，在训练之前对数据进行加权是否会提高 f1 分数。另一个测试是删除重复项。

![](img/1e99a1c40ce7fb91575c3f59cd54c736.png)

基于三种不同输入数据类型的作者图像

在第一轮中，通过*所有数据*和*加权数据*获得最佳精度和 f 值。他们都取得了同样的结果。但是唯一的数据集实现了标签 1 的最佳 f 值。

```
**All data**
Accuracy: 0.9023548583351629
F-measure: 0.8560382799176114
F-measure by label:
label 0: 0.9486714367527147
label 1: 0.0**Weighted data**
Accuracy: 0.9023548583351629
F-measure: 0.8560382799176114
F-measure by label:
label 0: 0.9486714367527147
label 1: 0.0**Unique data**
Accuracy: 0.6666666666666666
F-measure: 0.5859374999999999
F-measure by label:
label 0: 0.7906666666666665
label 1: 0.18229166666666666
```

在第二轮中，我们使用基于阈值的 f 得分的最大值的最佳阈值。总准确度和 f 值下降，但标签 1 的 f 值上升。最好的 f 1 测量(标签 1)是由唯一的数据集实现的，而最好的总精度和 f 测量是由另外两个数据集实现的。

```
**All data**
Accuracy: 0.7611244603498374
F-measure: 0.7955698747928712
F-measure by label:
label 0: 0.8594335689341074
label 1: 0.20539494445808643**Weighted data**
Accuracy: 0.7611244603498374
F-measure: 0.7955698747928712
F-measure by label:
label 0: 0.8594335689341074
label 1: 0.20539494445808643**Unique data** Accuracy: 0.5859872611464968
F-measure: 0.5914187270302529
label 0: 0.6044624746450304
label 1: 0.5657015590200445
```

最后一轮(参数调整)使用不同的参数来优化平均指标。在参数调整后，独特数据集预测 317 个预期标签 1 值中的 597 个，在测试数据上的准确度为 0.5754，而所有数据集预测 73990 个预期标签 1 值中的 134437 个，在测试数据上的准确度为 0.7786。

```
**All data** Average Metric Max: 0.5703188574707733
Average Metric Min: 0.5
Accuracy on test data: 0.7786
Total count of Label 1 prediction: 134437
Total count of Label 1 expectations: 73990**Unique data** Average Metric Max: 0.6750815894510674
Average Metric Min: 0.5
Accuracy on test data: 0.5754
Total count of Label 1 prediction: 597
Total count of Label 1 expectations: 317
```

**结论**

![](img/cf32da35bb19bef5406f638f70be036a.png)

作者根据 1.0 =搅动的最佳预测制作的图片

最好的模型是使用我们选择的特征的所有数据的模型。总精度和 f 值低于其他测试模型，但它提供了标签 1 值的最佳预测，这是该模型的主要用例。

通过使用其他算法，例如随机森林分类器或梯度增强树分类器，可以进行进一步的改进。另一个改进的来源可能是更多的特征，这些特征改进了最终模型的预测。

要了解这个项目的更多信息，请点击这里查看我的 Github 的链接。