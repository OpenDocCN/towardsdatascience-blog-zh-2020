# 库存预测的合理性

> 原文：<https://towardsdatascience.com/justification-of-stock-market-projections-a954caf49b80?source=collection_archive---------37----------------------->

## 机器学习算法能证明有效市场假说是错误的吗？

![](img/2f80b4e189e1c9cb5031b82498fa2d7c.png)

*Github 资源库:*[](https://github.com/waiyannaing/MSFT-Equity-Projection)

****来自《走向数据科学》编辑的提示:*** *虽然我们允许独立作者根据我们的* [*规则和指南*](/questions-96667b06af5) *发表文章，但我们并不认可每个作者的贡献。你不应该在没有寻求专业建议的情况下依赖一个作者的作品。详见我们的* [*读者术语*](/readers-terms-b5d780a700a4) *。**

# *简介(预测的有效性)*

*根据美国经济学家、畅销书《华尔街随想》的作者伯顿·戈登·马尔基尔的说法:“在科学的审视下，图表阅读必须与炼金术共享一个基座。”马尔基尔和学术界的许多其他人都认为，根据有效市场假说的条款，股票市场是一个股票价格波动非常大的地方，其结果无法根据过去的数据合理预测。话说回来，是不是经常这样？*

*有效市场假说认为:证券在交易所总是以公允价值交易，股票价格应该代表所有相关信息，即阿尔法的产生是不可实现的。简单地说，该假设断言，在一段时间内的任何给定时间，市场都有能力适应新信息，因此不可能超越它。换句话说，基本面分析比技术分析更有优势。一个人的猜测和另一个人的一样好，或者如马尔基尔的名言所说:“你的猜测和猿的一样好。”*

*现在应该更清楚为什么作为现代金融理论基石的有效市场假说的支持者认为股票市场的预测是无效的。然而，这并不能解释金融服务领域的绝大多数机构，如交易公司、共同基金和对冲基金，它们采用定量交易算法和图表数据，通过技术分析每年产生数十亿美元的利润。行为金融学还表明，股票市场的投资者远非理性，这意味着未来的价格事实上可以在“一定程度上”基于对历史趋势的分析来预测。这意味着股票市场预测的概念肯定有一些优点。*

# *摘要*

*本报告旨在探讨预测证券价格的有效性。更具体地说，它解决了一个研究问题:**根据历史股票数据训练的机器学习模型能否准确预测未来数据，以及预测到什么程度？***

*因此，它的特点是使用一种称为 LSTM(长短期记忆)的深度学习类的机器学习算法进行单变量时间序列分析。微软公司(MSFT)的历史股票数据用于数据探索、预处理、后处理、可视化以及模型拟合和评估。从最近的日期开始的十五个营业日的股票市场预测也在接近结束时进行。此外，均方误差、均方根误差和 R 平方作为评估模型总体性能的主要估计量。*

*与实际价格相比，该模型生成的结果值相当准确。因此，可以得出结论，机器学习确实可以是在很大程度上预测股票数据的有效方法。*

*包含整个程序的 GitHub 库可以在[这里](https://github.com/waiyannaing/MSFT-Equity-Projection)访问。*

# *方法学*

## *数据采集-*

*MSFT 的数据集是通过名为 *Tinngo* 的金融市场 API(应用程序编程接口)获得的，并使用 pandas_datareader 包进行访问。为了了解正在处理的数据，下面是将其转换为数据框后的原始形式。*

*![](img/a6f00cbc7580be7061cc939f6c8b8034.png)*

*数据集由 14 个要素组成，如列所示。*

## *数据预处理-*

*在最初的清理/格式化阶段实施了一些简单的过程，以便为将来使用数据集做准备。*

*   *所有列标题的第一个字母都大写。*
*   *日期列中的值被转换为日期时间对象。*
*   *从日期列的值中删除了时间值，因为它不涉及 MSFT 价格的日常时间序列模式。*
*   *创建了一个循环来检查任何缺少的值；对于所有列中的所有行，它都返回 false，这表明没有丢失值。*

*生成的新数据框如下所示:*

*![](img/b969af63522c480decfbf74a3c18a3c2.png)*

## *初步数据可视化-*

*深入了解 MSFT 价格运动的复杂本质非常重要，因为这有助于在未来决定最有效的机器学习模型。由于“收盘”功能涵盖了每个交易日结束时股票的现金价值，因此执行了仅与“收盘”相关的某些可视化方法。*

*绘制了一个箱线图和一个补充描述图，作为数据可视化这一阶段的第一步。*

*![](img/fd6d15415fb719cadfea2b22ace37944.png)**![](img/79d47ca2003aec8b9722c75a7889fb03.png)*

*从箱线图可以很容易地看出，似乎没有超出分布数据的最小和最大范围的极端异常值。*

*图表中的高标准差确实揭示了过去五年 MSFT 价格的波动性。更准确地说，在 96.44 美元的平均值周围有 41.91 美元的离差。此外，这个值必须有所保留，因为标准差的项可能与基础证券的价格有关。这意味着标准差的值可以反映股票价格而不是波动性。因此，使用变异系数来评估波动的真实程度，变异系数代表标准差与平均值的比率。这有助于理解样本中数据相对于总体均值的可变性程度。*

*由此得出的变异系数为:41.91 ÷ 96.44 = 0.4346。由于该值远低于 1，因此可以有把握地假设 MSFT 总体上不像之前假设的那样高度不稳定。此外，考虑到微软公司基于其资产负债表的强大财务健康，预计与其股权相关的风险应处于较低水平。*

*为了更好地显示 MSFT 证券的过去趋势，绘制了一个折线图。图表的主要亮点是 2020 年 3 月至 6 月证券价值的大幅下降和快速恢复。这种极端的波动性可以解释为由于最近的 2020 年新冠肺炎疫情对整个股票市场的忧虑态度。*

*![](img/62f1b414cc3ec0820ea3c31bd9de72d7.png)*

## *型号选择-*

*如前所述，选择了单变量时间序列分析，该分析用单个时间相关变量检查一个序列。这是在初步数据可视化阶段之后完成的；在做出这个决定之前，我们考虑了一些因素:*

*   *报告的目的不是要找出多个系列之间的相关关系。更确切地说，它是分析单个价格序列如何在其自身的历史视角上发展的行为。*
*   *数据集中的许多其他特征已被证明对收盘价影响很小甚至没有影响。股息是一个例外；然而，API 并不提供关于它的数据。*
*   *虽然特征高，低，开放和他们的调整后的对应物可能对收盘价有一个小的影响，这可能只是通过引入噪音使模型过于复杂。*

*考虑到这些因素，建立了长短期记忆(LSTM)神经网络作为执行单变量时间序列预测的最佳选择。这些推理属于 LSTM 的架构，它在时间序列的每一步存储信息的能力，以及研究问题的性质。*

*为了理解 LSTM 网络的结构，首先必须理解人工神经网络和递归神经网络(RNNs)的概念。人工神经网络基于互连的数据处理组件的网络，这些组件被称为组织成层的节点。一层中的给定节点通过权重与前面层中的所有其他节点相连，权重影响信号的强度和网络的输出。有三种类型的节点:1)收集数据的输入节点，2)生成值的输出节点，3)修改数据的隐藏节点。*

*RNN 是一种人工神经网络，它以前馈方式工作，允许信息从一个步骤按顺序传播到另一个步骤。然而，与基本前馈神经网络不同，RNNs 在层之间具有递归关系。这意味着他们利用反馈回路，即所谓的时间反向传播，将细节传回网络，这样来自先前输入的信息就可以持续。*

*![](img/ce9ed6a2d5d360bb512d44d26069f785.png)*

*展开的 RNN |来源:https://colah.github.io/posts/2015–08-Understanding-LSTMs/*

*然而，RNNs 也处理消失梯度问题，即在时间段的大间隙之间逐渐丢失信息。当更新神经网络中的权重的梯度由于时间上的反向传播而收缩时，就会发生这种情况。长期短期记忆网络通过实施内部机制(细胞状态和门)来调节信息流，从而改善了这一缺点。细胞状态在整个序列处理过程中传输相关信息，而门是不同的神经网络，它选择哪些信息可以在细胞状态中持续存在。有三种门:1)遗忘门，它选择相关信息来存储和传递，2)输入门，它更新单元状态，3)输出门，它确定随后的隐藏状态。这些操作使得 LSTM 模型善于学习长期依赖关系。*

*![](img/cace770db820b756504598221c2c97d0.png)*

*https://colah.github.io/posts/2015–08-Understanding-LSTMs/ LSTM 网络中的复读模块|来源*

*因此，LSTM 网络被确定为处理报告时间序列问题的最佳选择。有关 rnn 和 LSTMs 的更多信息可在此处查看。*

## *数据-后处理-*

*在本节中，来自预处理阶段的数据被进一步讨论，目的是为 LSTM 模型做准备。形成了一个新的数据帧，该数据帧仅由“Close”组成，并以“Date”作为索引值。由于日终现金价值最能代表某一特定日的证券市场价格，因此重点是“收盘”。*

*LSTM 模型对比例很敏感；因此，来自新数据帧的数据通过从 0 到 1 的缩放进行标准化。这将提高网络收敛的速度。*

```
***#Scaling Data**mms = MinMaxScaler(feature_range = (0,1))scaled_close = mms.fit_transform(df_close).reshape(-1,1)*
```

*新的数据集然后被分成两个部分:80%的训练和 20%的测试。这将允许在最初 80%的数据(训练集)上训练模型，然后在剩余部分(测试集)上重新评估其性能。*

```
***#Splitting Train/Test Data**train_ratio = 0.80rolling = 60train_data = np.array(scaled_close[:int(len(df_close)*train_ratio)])test_data = np.array(scaled_close[int(len(train_data))-rolling:])*
```

*“滚动”变量代表时间步长，即模型分析产生输出的天数。考虑下面的例子:*

*![](img/38c14c3f1d6f170d41ee4b45fadefbfd.png)*

*有一个用于训练机器学习模型的值序列。在每一行中，序列的四个值作为输入，随后的第五个值作为输出；随着行的前进，输入和输出组在序列中向下移动一个值。变量“滚动”代表用于产生输出的输入数量。这个概念进一步应用于将训练集和测试集分成输入集和输出集。*

```
***#Separating x/y Training Sets**x_train = []y_train = []for i in range(rolling, len(train_data)): x_train.append(train_data[i-rolling:i,0]) y_train.append(train_data[i,0])**#Separating x/y Testing Sets**x_test = []y_test = df_close[int(len(train_data)):]for i in range(rolling, len(test_data)): x_test.append(test_data[i-rolling:i])*
```

*LSTM 网络需要三维数组数据作为输入。第一个维度表示批量大小，第二个维度描述时间步长，最后一个维度描述特征。因此，输入被整形以符合这些要求。*

```
***#Reshaping x/y Sets**x_train, y_train = np.array(x_train), np.array(y_train)x_train_shape = x_train.shape[0]x_train = np.reshape(x_train, (x_train_shape, rolling, 1))x_test, y_test = np.array(x_test), np.array(y_test)x_test_shape = x_test.shape[0]x_test = np.reshape(x_test, (x_test_shape, rolling, 1))*
```

## *模型制作-*

*为了构建 LSTM 模型，从 Keras 库中导入了某些模块。*

```
*from keras.models import Sequentialfrom keras.layers import LSTMfrom keras.layers import Densefrom keras.layers import Dropout*
```

*1)顺序将被用作初始化神经网络的构造器，2) LSTM 将协助添加长短期记忆层，3)密集将添加密集连接的神经网络层 4)丢弃将添加丢弃层以防止模型过度拟合。*

*生成的模型如下所示:*

```
***#LSTM Model**lstm = Sequential()lstm.add(LSTM(rolling, return_sequences = True, input_shape =(rolling, 1)))lstm.add(Dropout(0.2))lstm.add(LSTM(rolling, return_sequences = True))lstm.add(Dropout(0.2))lstm.add(LSTM(rolling))lstm.add(Dropout(0.2))lstm.add(Dense(1)) lstm.compile(optimizer = 'adam', loss = 'mse', metrics = ['mean_squared_error'])*
```

*![](img/ca67b4a05577aa2c755814a30ddb6a1e.png)*

*LSTM 模型是用以下参数创建的:*

*   *60 个单位作为 LSTM 层中输出空间(时间步长)的维度。*
*   *Return_sequences = True，返回每个输入时间步长和堆叠的隐藏状态输出*
*   *LSTM 层。(以便随后的 LSTM 层具有三维序列)*
*   *0.2 作为丢弃层的输入，丢弃 20%的层，以防止模型过度拟合。*
*   *密集层中的输入 1 指定一个单位的输出。*
*   *Adam optimizer 是一种更新网络权重的优化算法，用于编译模型。*
*   *均方误差被设置为损失优化器。*

*然后用来自数据训练部分的输入和输出集来拟合该模型。然后用某些特征对其进行训练，例如 epochs，其指示学习算法将在整个数据集中工作的次数，设置为 60，分配给 x_train.shape[0]的批量大小，以及给定测试集的验证集。*

```
***#LSTM Model Fitting**lstm.fit(x_train, y_train, validation_data=(x_test, y_test),epochs = 60, batch_size = 50, verbose = 1)*
```

## *模型评估指标-*

*被选择用来评估 LSTM 模型性能的三个估计量是均方误差(MSE)、均方根误差(RMSE)和 R 平方。MSE 应评估误差平方和返回误差方差的平均值。RMSE 是 MSE 的平方根版本，它将计算残差(预测误差)的标准偏差。最后，R 平方度量将展示回归线与实际数据点的拟合，因为它测量模型所解释的响应变量变化的百分比。*

# *结果*

*使用 Sequential 的 predict()和 MinMaxScaler 的 inverse_transform()函数，从拟合的模型生成预测，并从 0 到 1 的范围内重新缩放。这导致了两套价格预测；一个用于训练部分，一个用于测试部分。*

*模型在训练集上的性能由三个模型评估度量来评估。*

*![](img/47d821f1937fe1093b57161220a7e6fa.png)*

*这些 MSE 和 RMSE 值立即表明在实际历史数据点周围没有太多误差，因为两者都接近于 0。R 平方的值表明 MSFT 99.53%的价格可以用模型解释。虽然这些结果最初可能说明了预测的惊人准确性，但存在严重的过度拟合问题，因为它可能无法很好地转化为看不见的数据。*

*通过绘制实际训练数据集及其预测值，可以更好地可视化这些指标。*

*![](img/4c72e6d555357431b20caea55a116241.png)*

*该模型在测试集上的性能由相同的三个评估指标进一步评估。*

*![](img/a9b6e5f0f6121b018d8005ebadb08ef0.png)*

*与训练集相比，MSE 和 RMSE 更高，R 平方更低。然而，这些 MSE 和 RMSE 仍然被认为非常低。这证明了模型集与实际值相差不大。R 平方值表明 94.2%的 MSFT 价格可以用模型解释。*

*再次，这是可视化的图形实际训练数据集和预测值。*

*![](img/2f80b4e189e1c9cb5031b82498fa2d7c.png)*

*实际值与预测值的图表:*

*![](img/7805cf8dccd7ac2e8a6f3b261441d722.png)*

# *讨论*

*可以有一定把握地说，LSTM 模型在预测股票价格方面证明是成功的。与真实数据点相比，特别是测试部分的预测值相当准确；0.17 的 MSE 和 0.41 的 RMSE 接近于 0 并且非常低，因此表明平均误差偏差很小。这一印象被 0.942 的 R 平方进一步证实，因为该模型能够解释测试集中 94.2%的 MSFT 股票价格。*

*虽然最初有人担心该模型在训练集上过度拟合，因为它在所有三个指标上都非常接近完美，但实际上它最终很好地转变为预测测试集上的未知数据。然而，仍然有一个问题值得关注，即为什么模型表现得如此之好，它涉及单步和多步时间序列预测之间的差异。迄今为止建立的模型是单步时间序列预测模型，因为它使用几个输入来生成一个输出。但是，请考虑已定型的模型是否是多步时间序列投影，在这种情况下，也将接受多个输入并生成多个输出。毫无疑问会有不同的结果。*

*创建多步时间序列投影的一种常用方法是通过递归方法，这种方法包括制作单个投影并将其附加到下一组输入中，以预测后续的时间步。*

*![](img/cf5cf310bf9674c1e178de2a50200fbf.png)*

*然而，它也引入了将早期误差传播到未来预测中的问题；来自时段 t 和 t+1 的预测误差将延续到后续时段。因此，如果单个预测在某种程度上不准确，则下一个值链很有可能变得越来越错误，因为错误的值将被用作未来的输入。因此，由于误差传播，多步时间序列预测模型有可能在更长的预测时间内退化。*

*在本报告中使用的单步模型的情况下，即使一个预测完全偏离实际值，后续的预测也很有可能被实际值带回正轨。*

*话又说回来，这个关注点不应该完全抹黑模型的准确性。如前所述，该模型在捕获 60 个数据点作为输入以生成一个输出方面表现良好，这平均只会在训练数据上偏离 0.19 美元，在未看到的数据上偏离 0.41 美元。因此，即使在递归多步时间序列预测的情况下，基于该模型的预测在未来的某个时期也应该是相当准确的，很可能小于 60 点。*

# *结论*

*预测股市还是有一些好处的，尤其是通过像机器学习这样强大的方法。该报告的研究结果表明，在单步时间序列预测问题的情况下，使用 LSTM 网络可以产生具有良好准确性的结果，更具体地说是 94.2%。对此的解释可能是，尽管有效市场假说可能在很长一段时间内成立，但它并不能完全解释证券价格的短期行为。如前所述，行为金融学展示了这样一个事实:股票市场在很大程度上是由非理性投资者决策和经济趋势驱动的；一个恰当的例子是，2020 年 MSFT 股票价格的大幅下跌是由新冠肺炎引起的。因此，在很大程度上，理解股票市场的行为可以被视为一个序列问题。*

*话说回来，报告的调查结果也有些局限。它只真正调查了一个序列的行为，因此，它不能建立各种其他因素和证券价格之间的关系。因此，如果该模型在未来继续被使用，在面对不可预见的事件时，可能会有许多实例产生完全有缺陷的预测。此外，基于单步时间序列预测模型的研究结果也存在问题。看看多步系列预测模型会产生什么样的结果，以及它将如何回答本报告的研究问题，这将是很有趣的。如果将来要进行任何改进，这两个考虑因素是改进的主要候选因素。*

# *十五天预测(方法到结果)*

*为了全面总结本报告，从最近的交易日期开始，使用递归多步时间序列预测方法对 MSFT 价格进行十五天预测。选定数字 15 是因为它小于“滚动”(时间步长)值的一半。因此，之前构建的 LSTM 模型使用 MSFT 的所有历史数据进行了重新训练，没有拆分。*

```
***#Creating Inputs/ Outputs Using Whole Test Data**df_final = np.array(scaled_close)x = [] y = []train_amt = 60for i in range(train_amt, len(df_final)): x.append(df_final[i-train_amt:i,0]) y.append(df_final[i,0])x, y = np.array(x), np.array(y)x_shape = x.shape[0]x = np.reshape(x, (x_shape, train_amt, 1))y = np.reshape(y, (-1,1))*
```

*训练完模型后，将创建一个循环，以接受新的单个输出作为未来的输入，并生成后续输出。*

```
***#Projection Set**steps = 15df_final = pd.DataFrame(scaled_close)for x in range (0,steps): data = np.array(df_final[len(df_final)-train_amt + x:]) x_project = np.array(data) x_project_shape = x_project.shape[0] x_project = np.reshape(x_project, (1, -1, 1)) projected_data = pd.DataFrame(lstm_final.predict(x_project)) df_final = df_final.append(projected_data)**#Rescaling the Dataset**df_final = mms.inverse_transform(df_final)*
```

*构造了一个新的函数，增加“n”个新的营业日来预测“n”个价格。*

```
***#Creating a Function For Adding Business Days**import datetimedef add_bdays(from_date, add_days): bdays_add = add_days curr_date = from_date while bdays_add > 0: curr_date += datetime.timedelta(days=1) weekday = curr_date.weekday() if weekday >= 5:

            continue bdays_add -= 1return curr_date*
```

*在创建新的远期指数后，将构建一个仅用于价格预测的新数据框架。*

```
***#Creating Date Index With 15 Days Forward**df_date = df[['Date']]start_date = str(df_date['Date'][0])end_date = df_date[‘Date'].iloc[-1]proj_dates = []for x in range(1,steps+1): proj_dates.append(add_bdays(end_date,x))df_proj_dates = pd.DataFrame(proj_dates)df_append_date = df_date['Date'].append(df_proj_dates)date_array = np.array(df_append_date)**#Creating New DataFrame For Forward Dates**df_new = pd.DataFrame(df_final, columns=['Projected Close'])df_new['Date'] = date_arraydf_new.set_index('Date', inplace = True)projected_df = df_new.tail(steps)projected_df*
```

*由此得出的价格预测是:*

*![](img/9946ee5d08fa186fd845002a83f0ce8d.png)**![](img/c0fa7c48ee1db03df721eab01d56410d.png)**![](img/0e54fe298b7b985cab3f093deababde1.png)*

# *资源*

*[http://colah.github.io/posts/2015-08-Understanding-](http://colah.github.io/posts/2015-08-Understanding-LSTMs/)*

*[LSTMs/](http://colah.github.io/posts/2015-08-Understanding-LSTMs/)[https://machine learning mastery . com/gentle-introduction-long-short-term-memory-networks-experts/](https://machinelearningmastery.com/gentle-introduction-long-short-term-memory-networks-experts/)*

*【http://vision.stanford.edu/pdf/KarpathyICLR2016.pdf *

*[https://machine learning mastery . com/multi-step-time-series-forecasting-with-machine-learning-models-for-household-consumption-electricity-consumption/](https://machinelearningmastery.com/multi-step-time-series-forecasting-with-machine-learning-models-for-household-electricity-consumption/)*

*[https://www . research gate . net/publication/262450459 _ Methods _ for _ Multi-Step _ Time _ Series _ Forecasting _ with _ Neural _ Networks](https://www.researchgate.net/publication/262450459_Methods_for_Multi-Step_Time_Series_Forecasting_with_Neural_Networks)*