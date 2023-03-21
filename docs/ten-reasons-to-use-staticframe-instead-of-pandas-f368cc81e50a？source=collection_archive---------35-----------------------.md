# 用 StaticFrame 代替熊猫的十大理由

> 原文：<https://towardsdatascience.com/ten-reasons-to-use-staticframe-instead-of-pandas-f368cc81e50a?source=collection_archive---------35----------------------->

## 在处理数据帧时，创建更易维护、更不易出错的 Python

![](img/18750677a1666a6834c7164299d64832.png)

作者图片

如果你用 Python 处理数据，你可能会用熊猫。Pandas 提供了近乎即时的满足:复杂的数据处理例程可以用几行代码实现。然而，如果你在大型项目中使用熊猫多年，你可能会遇到一些挑战。复杂的 Pandas 应用程序会产生难以维护且容易出错的 Python 代码。发生这种情况是因为 Pandas 提供了许多方法来做同样的事情，具有不一致的接口，并且广泛支持就地突变。对于那些来自熊猫的人来说，StaticFrame 提供了一个更加一致的界面，减少了出错的机会。这篇文章展示了使用 StaticFrame 代替 Pandas 的十个理由。

# 为什么是静态框架

在使用 Pandas 开发后端财务系统多年后，我清楚地认识到 Pandas 并不是这项工作的合适工具。Pandas 对标记数据和缺失值的处理，性能接近 NumPy，确实提高了我的生产率。然而，熊猫 API 中的大量不一致导致代码难以维护。此外，熊猫对原位突变的支持导致了严重的出错机会。因此，在 2017 年 5 月，我开始实施一个更适合关键生产系统的库。

现在，经过多年的发展和完善，我们看到了用 StaticFrame 代替 Pandas 在我们的生产系统中取得的优异成绩。用 StaticFrame 编写的库和应用程序更容易维护和测试。我们经常看到 StaticFrame 在大规模、真实世界的用例中表现优于 panda，尽管对于许多独立的操作，StaticFrame 还没有 panda 快。

以下是支持使用 StaticFrame 而不是 Pandas 的十个理由。作为 StaticFrame 的第一作者，我当然对这个演示有偏见。然而，从 2013 年开始与熊猫一起工作，我希望有一些观点可以分享。

所有示例都使用 Pandas 1.0.3 和 StaticFrame 0.6.20。导入使用以下惯例:

```
>>> import pandas as pd
>>> import static_frame as sf
```

# № 1:一致且可发现的界面

应用程序编程接口(API)可以在函数的位置、函数的命名方式以及这些函数接受的参数的名称和类型方面保持一致。StaticFrame 偏离了 Pandas 的 API，以在所有这些领域支持更大的一致性。

要创建一个`sf.Series`或`sf.Frame`，你需要构造函数。Pandas 将其`pd.DataFrame`构造函数放在两个地方:根名称空间(`pd`，通常是导入的)和`pd.DataFrame`类。

例如，JSON 数据是从`pd`名称空间上的函数加载的，而记录数据(Python 序列的一个 iterable)是从`pd.DataFrame`类加载的。

```
>>> pd.read_json('[{"name":"muon", "mass":0.106},{"name":"tau", "mass":1.777}]')
   name   mass
0  muon  0.106
1   tau  1.777>>> pd.DataFrame.from_records([{"name":"muon", "mass":0.106}, {"name":"tau", "mass":1.777}])
   name   mass
0  muon  0.106
1   tau  1.777
```

尽管 Pandas 有专门的构造函数，默认的`pd.DataFrame`构造函数接受多种多样的输入，包括许多与`pd.DataFrame.from_records()`相同的输入。

```
>>> pd.DataFrame([{"name":"muon", "mass":0.106}, {"name":"tau", "mass":1.777}])
   name   mass
0  muon  0.106
1   tau  1.777
```

对于用户来说，这种多样性和冗余性没有什么好处。StaticFrame 将所有的构造函数放在它们构造的类上，并尽可能地集中它们的功能。因为显式的、专用的构造函数更容易维护，所以它们在 StaticFrame 中很常见。比如`sf.Frame.from_json()`和`sf.Frame.from_dict_records()`:

```
>>> sf.Frame.from_json('[{"name":"muon", "mass":0.106}, {"name":"tau", "mass":1.777}]')
<Frame>
<Index> name  mass      <<U4>
<Index>
0       muon  0.106
1       tau   1.777
<int64> <<U4> <float64>>>> sf.Frame.from_dict_records([{"name":"muon", "mass":0.106}, {"name":"tau", "mass":1.777}])
<Frame>
<Index> name  mass      <<U4>
<Index>
0       muon  0.106
1       tau   1.777
<int64> <<U4> <float64>
```

显式导致大量的构造函数。为了帮助您找到您正在寻找的东西，StaticFrame 容器公开了一个`interface`属性，该属性将调用类或实例的整个公共接口作为一个`sf.Frame`提供。我们可以通过使用一个`sf.Frame.loc[]`选择来过滤这个表，只显示构造函数。

```
>>> sf.Frame.interface.loc[sf.Frame.interface['group'] == 'Constructor', 'doc']
<Series: doc>
<Index: signature>
__init__(data, *, index, columns,... Initializer. Args...
from_arrow(value, *, index_depth,... Realize a Frame f...
from_clipboard(*, delimiter, inde... Create a Frame fr...
from_concat(frames, *, axis, unio... Concatenate multi...
from_concat_items(items, *, axis,... Produce a Frame w...
from_csv(fp, *, index_depth, inde... Specialized versi...
from_delimited(fp, *, delimiter, ... Create a Frame fr...
from_dict(mapping, *, index, fill... Create a Frame fr...
from_dict_records(records, *, ind... Frame constructor...
from_dict_records_items(items, *,... Frame constructor...
from_element(element, *, index, c... Create a Frame fr...
from_element_iloc_items(items, *,... Given an iterable...
from_element_loc_items(items, *, ... This function is ...
from_elements(elements, *, index,... Create a Frame fr...
from_hdf5(fp, *, label, index_dep... Load Frame from t...
from_items(pairs, *, index, fill_... Frame constructor...
from_json(json_data, *, dtypes, n... Frame constructor...
from_json_url(url, *, dtypes, nam... Frame constructor...
from_overlay(containers, *, union...
from_pandas(value, *, index_const... Given a Pandas Da...
from_parquet(fp, *, index_depth, ... Realize a Frame f...
from_records(records, *, index, c... Construct a Frame...
from_records_items(items, *, colu... Frame constructor...
from_series(series, *, name, colu... Frame constructor...
from_sql(query, *, connection, in... Frame constructor...
from_sqlite(fp, *, label, index_d... Load Frame from t...
from_structured_array(array, *, i... Convert a NumPy s...
from_tsv(fp, *, index_depth, inde... Specialized versi...
from_xlsx(fp, *, label, index_dep... Load Frame from t...
<<U94>                               <<U83>
```

# № 2:一致且丰富多彩的显示屏

熊猫以不同的方式展示它的容器。例如，`pd.Series`显示了它的名称和类型，而`pd.DataFrame`没有显示这两个属性。如果你显示一个`pd.Index`或`pd.MultiIndex`，你会得到第三种方法:一个适合`eval()`的字符串，当它很大时是不可理解的。

```
>>> df = pd.DataFrame.from_records([{'symbol':'c', 'mass':1.3}, {'symbol':'s', 'mass':0.1}], index=('charm', 'strange'))>>> df
        symbol  mass
charm        c   1.3
strange      s   0.1>>> df['mass']
charm      1.3
strange    0.1
Name: mass, dtype: float64>>> df.index
Index(['charm', 'strange'], dtype='object')
```

StaticFrame 为所有容器提供了一致的、可配置的显示。`sf.Series`、`sf.Frame`、`sf.Index`和`sf.IndexHierarchy`的显示都共享一个公共的实现和设计。这种设计的一个优先考虑的问题是总是显式的容器类和底层数组类型。

```
>>> f = sf.Frame.from_dict_records_items((('charm', {'symbol':'c', 'mass':1.3}), ('strange', {'symbol':'s', 'mass':0.1})))>>> f
<Frame>
<Index> symbol mass      <<U6>
<Index>
charm   c      1.3
strange s      0.1
<<U7>   <<U1>  <float64>>>> f['mass']
<Series: mass>
<Index>
charm          1.3
strange        0.1
<<U7>          <float64>>>> f.columns
<Index>
symbol
mass
<<U6>
```

由于大量的时间花费在可视化地探索这些容器的内容上，StaticFrame 提供了许多显示配置选项，所有这些都通过`sf.DisplayConfig`类公开。对于持久的变更，`sf.DisplayConfig`实例可以传递给`sf.DisplayActive.set()`；对于一次性的更改，`sf.DisplayConfig`实例可以传递给容器的`display()`方法。

虽然`pd.set_option()`可以类似地用于设置熊猫显示特征，但 StaticFrame 提供了更广泛的选项来使类型可被发现。如这个[终端动画](https://raw.githubusercontent.com/InvestmentSystems/static-frame/master/doc/images/animate-display-config.svg)所示，特定类型可以被着色或者类型注释可以被完全移除。

# № 3:不可变数据:无需防御性副本的高效内存管理

Pandas 在数据输入和从容器中公开的数据的所有权方面表现出不一致的行为。在某些情况下，有可能在熊猫的“背后”变异 NumPy 阵列，暴露出不良副作用和编码错误的机会。

例如，如果我们向一个`pd.DataFrame`提供一个 2D 数组，数组的原始引用可以用来“远程”改变`pd.DataFrame`中的值。在这种情况下，`pd.DataFrame`不保护对其数据的访问，只作为一个共享的可变数组的包装器。

```
>>> a1 = np.array([[0.106, -1], [1.777, -1]])>>> df = pd.DataFrame(a1, index=('muon', 'tau'), columns=('mass', 'charge'))>>> df
       mass  charge
muon  0.106    -1.0
tau   1.777    -1.0>>> a1[0, 0] = np.nan *# Mutating the original array.*>>> df *# Mutation reflected in the DataFrame created from that array.*
       mass  charge
muon    NaN    -1.0
tau   1.777    -1.0
```

类似地，有时从`pd.Series`或`pd.DataFrame`的`values`属性中暴露出来的 NumPy 数组可能会发生变异，从而改变`pd.DataFrame`中的值。

```
>>> a2 = df['charge'].values>>> a2
array([-1., -1.])>>> a2[1] = np.nan *# Mutating the array from .values.*>>> df *# Mutation is reflected in the DataFrame.*
       mass  charge
muon    NaN    -1.0
tau   1.777     NaN
```

有了 StaticFrame，就没有了“幕后”变异的漏洞:因为 StaticFrame 管理不可变的 NumPy 数组，所以引用只保存到不可变的数组。如果在初始化时给定了一个可变数组，将会产生一个不可变的副本。不可变数组不能从容器或对底层数组的直接访问中变异。

```
>>> a1 = np.array([[0.106, -1], [1.777, -1]])>>> f = sf.Frame(a1, index=('muon', 'tau'), columns=('mass', 'charge'))>>> a1[0, 0] = np.nan *# Mutating the original array has no affect on the Frame*>>> f
<Frame>
<Index> mass      charge    <<U6>
<Index>
muon    0.106     -1.0
tau     1.777     -1.0
<<U4>   <float64> <float64>>>> f['charge'].values[1] = np.nan *# An immutable array cannot be mutated*
Traceback (most recent call last):
  File "<console>", line 1, in <module>
ValueError: assignment destination is read-only
```

虽然不可变数据减少了出错的机会，但它也提供了性能优势。例如，当用`sf.Frame.relabel()`替换列标签时，底层数据不会被复制。相反，对相同不可变数组的引用在新旧容器之间共享。这样的“无拷贝”操作因此是快速和轻量级的。这与在 Pandas 中做同样的事情时发生的情况形成了对比:相应的 Pandas 方法`df.DataFrame.rename()`被强制对所有底层数据进行防御性复制。

```
>>> f.relabel(columns=lambda x: x.upper()) *# Underlying arrays are not copied*
<Frame>
<Index> MASS      CHARGE    <<U6>
<Index>
muon    0.106     -1.0
tau     1.777     -1.0
<<U4>   <float64> <float64>
```

# № 4:赋值是一个函数

虽然 Pandas 允许就地赋值，但有时这种操作不能提供适当的派生类型，从而导致不良行为。例如，一个赋给整数`pd.Series`的浮点数将在没有警告或错误的情况下截断它的浮点部分。

```
>>> s = pd.Series((-1, -1), index=('tau', 'down'))
>>> s
tau    -1
down   -1
dtype: int64>>> s['down'] = -0.333 *# Assigning a float.*>>> s *# The -0.333 value was truncated to 0*
tau    -1
down    0
dtype: int64
```

使用 StaticFrame 的不可变数据模型，赋值是一个返回新容器的函数。这允许评估类型以确保结果数组可以完全包含赋值。

```
>>> s = sf.Series((-1, -1), index=('tau', 'down'))
>>> s
<Series>
<Index>
tau      -1
down     -1
<<U4>    <int64>>>> s.assign['down'](-0.333) *# The float is assigned without truncation*
<Series>
<Index>
tau      -1.0
down     -0.333
<<U4>    <float64>
```

StaticFrame 使用一个特殊的`assign`接口来执行赋值函数调用。在一个`sf.Frame`上，这个接口公开了一个`sf.Frame.assign.loc[]`接口，可以用来选择赋值的目标。选择之后，要分配的值通过函数调用传递。

```
>>> f = sf.Frame.from_dict_records_items((('charm', {'charge':0.666, 'mass':1.3}), ('strange', {'charge':-0.333, 'mass':0.1})))>>> f
<Frame>
<Index> charge    mass      <<U6>
<Index>
charm   0.666     1.3
strange -0.333    0.1
<<U7>   <float64> <float64>>>> f.assign.loc['charm', 'charge'](Fraction(2, 3)) *# Assigning to a loc-style selection*
<Frame>
<Index> charge   mass      <<U6>
<Index>
charm   2/3      1.3
strange -0.333   0.1
<<U7>   <object> <float64>
```

# № 5:迭代器用于迭代和函数应用

Pandas 具有独立的迭代和函数应用功能。对于`pd.DataFrame`上的迭代，有`pd.DataFrame.iteritems()`、`pd.DataFrame.iterrows()`、`pd.DataFrame.itertuples()`和`pd.DataFrame.groupby()`；对于`pd.DataFrame`上的功能应用，有`pd.DataFrame.apply()`和`pd.DataFrame.applymap()`。

但是由于函数应用需要迭代，因此将函数应用建立在迭代之上是明智的。StaticFrame 通过提供一系列迭代器(如`Frame.iter_array()`或`Frame.iter_group_items()`)来组织迭代和函数应用，通过对`apply()`的链式调用，这些迭代器也可用于函数应用。迭代器上也有应用映射类型的函数(比如`map_any()`和`map_fill()`)。这意味着一旦你知道你想如何迭代，函数应用只是一个方法。

例如，我们可以用`sf.Frame.from_records()`创建一个`sf.Frame`:

```
>>> f = sf.Frame.from_records(((0.106, -1.0, 'lepton'), (1.777, -1.0, 'lepton'), (1.3, 0.666, 'quark'), (0.1, -0.333, 'quark')), columns=('mass', 'charge', 'type'), index=('muon', 'tau', 'charm', 'strange'))>>> f
<Frame>
<Index> mass      charge    type   <<U6>
<Index>
muon    0.106     -1.0      lepton
tau     1.777     -1.0      lepton
charm   1.3       0.666     quark
strange 0.1       -0.333    quark
```

我们可以用`sf.Series.iter_element()`遍历一列值。我们可以通过使用在从`sf.Series.iter_element()`返回的对象上找到的`apply()`方法，使用同一个迭代器做函数应用。在`sf.Series`和`sf.Frame`上都可以找到相同的界面。

```
>>> tuple(f['type'].iter_element())
('lepton', 'lepton', 'quark', 'quark')>>> f['type'].iter_element().apply(lambda e: e.upper())
<Series>
<Index>
muon     LEPTON
tau      LEPTON
charm    QUARK
strange  QUARK
<<U7>    <<U6>>>> f[['mass', 'charge']].iter_element().apply(lambda e: format(e, '.2e'))
<Frame>
<Index> mass     charge    <<U6>
<Index>
muon    1.06e-01 -1.00e+00
tau     1.78e+00 -1.00e+00
charm   1.30e+00 6.66e-01
strange 1.00e-01 -3.33e-01
<<U7>   <object> <object>
```

对于`sf.Frame`上的行或列迭代，一系列方法允许指定用于迭代的行或列的容器类型，即，用数组、用`NamedTuple`或用`sf.Series`(分别为`iter_array()`、`iter_tuple()`、`iter_series()`)。这些方法采用一个轴参数来确定迭代是按行还是按列，并且类似地为函数应用程序公开一个`apply()`方法。要对列应用函数，我们可以执行以下操作。

```
>>> f[['mass', 'charge']].iter_array(axis=0).apply(np.sum)
<Series>
<Index>
mass     3.283
charge   -1.667
<<U6>    <float64>
```

将函数应用于行而不是列只需要更改轴参数。

```
>>> f.iter_series(axis=1).apply(lambda s: s['mass'] > 1 and s['type'] == 'quark')
<Series>
<Index>
muon     False
tau      False
charm    True
strange  False
<<U7>    <bool>
```

Group-by 操作只是另一种形式的迭代，具有相同的迭代和函数应用接口。

```
>>> f.iter_group('type').apply(lambda f: f['mass'].mean())
<Series>
<Index>
lepton   0.9415
quark    0.7000000000000001
<<U6>    <float64>
```

# № 6:严格的仅增长框架

`pd.DataFrame`的一个有效用途是加载初始数据，然后通过添加额外的列来生成派生数据。这种方法利用了类型和基础数组的列组织:添加新列不需要重新分配旧列。

StaticFrame 通过提供一个称为`sf.FrameGO`的`sf.Frame`的严格的、只增长的版本，使得这种方法不容易出错。例如，一旦创建了`sf.FrameGO`，就可以添加新的列，而现有的列不能被覆盖或就地改变。

```
>>> f = sf.FrameGO.from_records(((0.106, -1.0, 'lepton'), (1.777, -1.0, 'lepton'), (1.3, 0.666, 'quark'), (0.1, -0.333, 'quark')), columns=('mass', 'charge', 'type'), index=('muon', 'tau', 'charm', 'strange'))>>> f['positive'] = f['charge'] > 0>>> f
<FrameGO>
<IndexGO> mass      charge    type   positive <<U8>
<Index>
muon      0.106     -1.0      lepton False
tau       1.777     -1.0      lepton False
charm     1.3       0.666     quark  True
strange   0.1       -0.333    quark  False
```

这种有限形式的突变满足了实际需要。此外，从一个`sf.Frame`到一个`sf.FrameGO`的来回转换(使用`Frame.to_frame_go()`和`FrameGO.to_frame()`)是一个非复制操作:底层的不可变数组可以在两个容器之间共享。

# № 7:日期不是纳秒

Pandas 将所有日期或时间戳值建模为 NumPy `datetime64[ns]`(纳秒)数组，而不管纳秒级的分辨率是否实用或合适。这给熊猫制造了一个“Y2262 问题”:超过 2262-04-11 的日期不能被表达。虽然我可以创建一个最长为 2262–04–11 的`pd.DatetimeIndex`,但再过一天，Pandas 就会引发一个错误。

```
>>> pd.date_range('1980', '2262-04-11')
DatetimeIndex(['1980-01-01', '1980-01-02', '1980-01-03', '1980-01-04',
               '1980-01-05', '1980-01-06', '1980-01-07', '1980-01-08',
               '1980-01-09', '1980-01-10',
               ...
               '2262-04-02', '2262-04-03', '2262-04-04', '2262-04-05',
               '2262-04-06', '2262-04-07', '2262-04-08', '2262-04-09',
               '2262-04-10', '2262-04-11'],
              dtype='datetime64[ns]', length=103100, freq='D')>>> pd.date_range('1980', '2262-04-12')
Traceback (most recent call last):
pandas._libs.tslibs.np_datetime.OutOfBoundsDatetime: Out of bounds nanosecond timestamp: 2262-04-12 00:00:00
```

由于索引通常用于粒度远小于纳秒的日期时间值(如日期、月份或年份)，StaticFrame 提供了所有 NumPy 类型的`datetime64`索引。这允许精确的日期-时间类型规范，并避免了基于纳秒的单位的限制。

虽然用 Pandas 不可能，但用 StaticFrame 创建一个扩展到 3000 年的年份或日期的索引是很简单的。

```
>>> sf.IndexYear.from_year_range(1980, 3000).tail()
<IndexYear>
2996
2997
2998
2999
3000
<datetime64[Y]>>>> sf.IndexDate.from_year_range(1980, 3000).tail()
<IndexDate>
3000-12-27
3000-12-28
3000-12-29
3000-12-30
3000-12-31
<datetime64[D]>
```

# № 8:层次索引的一致接口

分级索引允许将多个维度放入一个维度中。使用分级索引， *n* 维数据可以被编码到单个`sf.Series`或`sf.Frame`中。

分级索引的一个关键特征是在任意深度的部分选择，由此选择可以由每个深度级别的选择的交集组成。熊猫提供了许多方式来表达那些内在的深度选择。

一种方法是超载`pd.DataFrame.loc[]`。当使用 Pandas 的层次索引(`pd.MultiIndex`)时，`pd.DataFrame.loc[]`选择中的位置参数的含义变成了动态的。正是这一点使得 Pandas 代码很难使用层次索引来维护。我们可以通过创建一个`pd.DataFrame`并设置一个`pd.MultiIndex`来看到这一点。

```
>>> df = pd.DataFrame.from_records([('muon', 0.106, -1.0, 'lepton'), ('tau', 1.777, -1.0, 'lepton'), ('charm', 1.3, 0.666, 'quark'), ('strange', 0.1, -0.333, 'quark')], columns=('name', 'mass', 'charge', 'type'))>>> df.set_index(['type', 'name'], inplace=True)>>> df
                 mass  charge
type   name
lepton muon     0.106  -1.000
       tau      1.777  -1.000
quark  charm    1.300   0.666
       strange  0.100  -0.333
```

类似于 NumPy 中的 2D 数组，当给`pd.DataFrame.loc[]`两个参数时，第一个参数是行选择器，第二个参数是列选择器。

```
>>> df.loc['lepton', 'mass'] *# Selects "lepton" from row, "mass" from columns*
name
muon    0.106
tau     1.777
Name: mass, dtype: float64
```

然而，与这种期望相反，有时 Pandas 不会将第二个参数用作列选择，而是用作`pd.MultiIndex`内部深度的行选择。

```
>>> df.loc['lepton', 'tau'] *# Selects lepton and tau from rows*
mass      1.777
charge   -1.000
Name: (lepton, tau), dtype: float64
```

为了解决这种不确定性，Pandas 提供了两种选择。如果需要行和列选择，可以通过将分层行选择包装在`pd.IndexSlice[]`选择修饰符中来恢复预期的行为。或者，如果不使用`pd.IndexSlice[]`需要内部深度选择，可以使用`pd.DataFrame.xs()`方法。

```
>>> df.loc[pd.IndexSlice['lepton', 'tau'], 'charge']
-1.0>>> df.xs(level=1, key='tau')
         mass  charge
type
lepton  1.777    -1.0
```

给予`pd.DataFrame.loc[]`的位置参数的含义不一致是不必要的，这使得熊猫代码更难维护:使用`pd.DataFrame.loc[]`的意图在没有`pd.IndexSlice[]`的情况下变得不明确。此外，提供多种方法来解决这个问题也是一个缺点，因为在 Python 中最好有一种显而易见的方法来做事。

StaticFrame 的`sf.IndexHierarchy`提供了更加一致的行为。我们将创建一个等价的`sf.Frame`并设置一个`sf.IndexHierarchy`。

```
>>> f = sf.Frame.from_records((('muon', 0.106, -1.0, 'lepton'), ('tau', 1.777, -1.0, 'lepton'), ('charm', 1.3, 0.666, 'quark'), ('strange', 0.1, -0.333, 'quark')), columns=('name', 'mass', 'charge', 'type'))>>> f = f.set_index_hierarchy(('type', 'name'), drop=True)>>> f
<Frame>
<Index>                                    mass      charge    <<U6>
<IndexHierarchy: ('type', 'name')>
lepton                             muon    0.106     -1.0
lepton                             tau     1.777     -1.0
quark                              charm   1.3       0.666
quark                              strange 0.1       -0.333
<<U6>                              <<U7>   <float64> <float64>
```

与 Pandas 不同，StaticFrame 在位置参数的含义上是一致的:第一个参数总是行选择器，第二个参数总是列选择器。对于在`sf.IndexHierarchy`中的选择，需要`sf.HLoc[]`选择修改器来指定层次中任意深度的选择。有一个显而易见的方法来选择内心深处。这种方法使得 StaticFrame 代码更容易理解和维护。

```
>>> f.loc[sf.HLoc['lepton']]
<Frame>
<Index>                                  mass      charge    <<U6>
<IndexHierarchy: ('type', 'name')>
lepton                             muon  0.106     -1.0
lepton                             tau   1.777     -1.0
<<U6>                              <<U4> <float64> <float64>>>> f.loc[sf.HLoc[:, ['muon', 'strange']], 'mass']
<Series: mass>
<IndexHierarchy: ('type', 'name')>
lepton                             muon    0.106
quark                              strange 0.1
<<U6>                              <<U7>   <float64>
```

# № 9:索引总是唯一的

很自然地认为`pd.DataFrame`上的索引和列标签是惟一标识符:它们的接口表明它们就像 Python 字典，其中的键总是惟一的。然而，熊猫指数并不局限于唯一值。在带有重复项的`pd.DataFrame`上创建索引意味着，对于一些单标签选择，将返回一个`pd.Series`，但是对于其他单标签选择，将返回一个`pd.DataFrame`。

```
>>> df = pd.DataFrame.from_records([('muon', 0.106, -1.0, 'lepton'), ('tau', 1.777, -1.0, 'lepton'), ('charm', 1.3, 0.666, 'quark'), ('strange', 0.1, -0.333, 'quark')], columns=('name', 'mass', 'charge', 'type'))>>> df.set_index('charge', inplace=True) # Creating an index with duplicated labels>>> df
           name   mass    type
charge
-1.000     muon  0.106  lepton
-1.000      tau  1.777  lepton
 0.666    charm  1.300   quark
-0.333  strange  0.100   quark>>> df.loc[-1.0] # Selecting a non-unique label results in a pd.DataFrame
        name   mass    type
charge
-1.0    muon  0.106  lepton
-1.0     tau  1.777  lepton>>> df.loc[0.666] # Selecting a unique label results in a pd.Series
name    charm
mass      1.3
type    quark
Name: 0.666, dtype: object
```

Pandas 对非唯一索引的支持使得客户端代码变得更加复杂，因为它必须处理有时返回一个`pd.Series`而有时返回一个`pd.DataFrame`的选择。此外，索引的唯一性通常是对数据一致性的简单而有效的检查。

一些 Pandas 接口，比如`pd.concat()`和`pd.DataFrame.set_index()`，提供了一个名为`verify_integrity`的可选惟一性检查参数。令人惊讶的是，熊猫默认禁用了`verify_integrity`。

```
>>> df.set_index('type', verify_integrity=True)
Traceback (most recent call last):
ValueError: Index has duplicate keys: Index(['lepton', 'quark'], dtype='object', name='type')
```

在 StaticFrame 中，索引总是唯一的。试图设置非唯一索引将引发异常。这个约束消除了在索引中错误引入重复的机会。

```
>>> f = sf.Frame.from_records((('muon', 0.106, -1.0, 'lepton'), ('tau', 1.777, -1.0, 'lepton'), ('charm', 1.3, 0.666, 'quark'), ('strange', 0.1, -0.333, 'quark')), columns=('name', 'mass', 'charge', 'type'))>>> f
<Frame>
<Index> name    mass      charge    type   <<U6>
<Index>
0       muon    0.106     -1.0      lepton
1       tau     1.777     -1.0      lepton
2       charm   1.3       0.666     quark
3       strange 0.1       -0.333    quark
<int64> <<U7>   <float64> <float64> <<U6>>>> f.set_index('type')
Traceback (most recent call last):
static_frame.core.exception.ErrorInitIndex: labels (4) have non-unique values (2)
```

# № 10:去了又回来看熊猫

StaticFrame 旨在与熊猫并肩工作。通过专门的构造器和导出器，例如`Frame.from_pandas()`或`Series.to_pandas()`，来回切换是可能的。

```
>>> df = pd.DataFrame.from_records([('muon', 0.106, -1.0, 'lepton'), ('tau', 1.777, -1.0, 'lepton'), ('charm', 1.3, 0.666, 'quark'), ('strange', 0.1, -0.333, 'quark')], columns=('name', 'mass', 'charge', 'type'))>>> df
      name   mass  charge    type
0     muon  0.106  -1.000  lepton
1      tau  1.777  -1.000  lepton
2    charm  1.300   0.666   quark
3  strange  0.100  -0.333   quark>>> sf.Frame.from_pandas(df)
<Frame>
<Index> name     mass      charge    type     <object>
<Index>
0       muon     0.106     -1.0      lepton
1       tau      1.777     -1.0      lepton
2       charm    1.3       0.666     quark
3       strange  0.1       -0.333    quark
<int64> <object> <float64> <float64> <object>
```

# 结论

“数据框”对象的概念在 2009 年 Pandas 0.1 发布之前很久就出现了:数据框的第一个实现可能早在 1991 年就出现在 r 的前身 S 语言中。今天，数据框在各种语言和实现中都有实现。熊猫将继续为广大用户提供优秀的资源。然而，对于正确性和代码可维护性至关重要的情况，StaticFrame 提供了一种替代方案，旨在更加一致并减少出错的机会。

有关 StaticFrame 的更多信息，请参见[文档](http://static-frame.readthedocs.io)或[项目现场](https://github.com/InvestmentSystems/static-frame)。