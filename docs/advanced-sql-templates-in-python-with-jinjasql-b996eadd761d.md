# 带 JinjaSql 的 Python 中的高级 SQL 模板

> 原文：<https://towardsdatascience.com/advanced-sql-templates-in-python-with-jinjasql-b996eadd761d?source=collection_archive---------4----------------------->

## 使用 Python 函数、循环和可变预设增强您的 SQL 模板。

![](img/b47fd11380a1098a7422b720cf71eecc.png)

在[Python 中模板化 SQL 查询的简单方法](/a-simple-approach-to-templated-sql-queries-in-python-adc4f0dc511)中，我使用 [JinjaSql](https://github.com/hashedin/jinjasql) 介绍了 Python 中 SQL 模板的基础知识。这篇文章使用预置、循环和自定义函数进一步展示了 JinjaSql 模板中的 [Jinja2](http://jinja.pocoo.org/) 的强大功能。让我们考虑一个日常用例，当我们有一个包含一些维度和一些数值的表，并且我们想要找到给定维度或维度组合的一些指标。下面的示例数据很小，而且完全是虚构的，但它足以演示高级功能。首先，我介绍数据集和 SQL 查询要回答的问题。然后，我将构建没有模板的查询，最后，展示如何使用 SQL 模板来参数化和生成这些查询。

# 示例数据集

让我们考虑一个包含一些商店购物记录的表`transactions`。购买可以通过现金、信用卡或借记卡进行，这为数据增加了额外的维度。下面是创建和填充该表的代码。

```
**create** **table** transactions (
    transaction_id **int**,
    user_id **int**,
    transaction_date **date**,
    store_id **int**,
    payment_method **varchar**(10),
    amount **float**
)
;**insert** **into** transactions
(transaction_id, user_id, transaction_date, store_id, payment_method, amount)
**values**
    (1, 1234, ‘2019–03–02’, 1, 'cash', 5.25),
    (1, 1234, ‘2019–03–01’, 1, 'credit', 10.75),
    (1, 1234, ‘2019–03–02’, 2, 'cash', 25.50),
    (1, 1234, ‘2019–03–03’, 2, 'credit', 17.00),
    (1, 4321, ‘2019–03–01’, 2, 'cash', 20.00),
    (1, 4321, ‘2019–03–02’, 2, 'debit', 30.00),
    (1, 4321, ‘2019–03–03’, 1, 'cash', 3.00)
;
```

# 要计算的指标

在研究一个数据集时，通常会查看所有维度的主要性能指标。在本例中，我们希望计算以下指标:

*   交易数量
*   平均交易金额
*   交易总额

我们需要每个用户、商店和支付方式的这些指标。我们还想通过商店和支付方式来了解这些指标。

# 单一维度的模板

获取每个商店指标的查询是:

```
**select**
    store_id
    , count(*) **as** num_transactions
    , sum(amount) **as** total_amount
    , avg(amount) **as** avg_amount
**from**
    transactions
**group** **by**
    store_id
**order** **by** total_amount **desc**
```

为了获得其他维度的相同指标，我们只需将`select`和`group by`子句中的`store_id`改为`user_id`或`payment_method`。因此 JinjaSql 模板可能看起来像

```
_BASIC_STATS_TEMPLATE = '''
**select**
    {{ dim | sqlsafe }}
    , count(*) **as** num_transactions
    , sum(amount) **as** total_amount
    , avg(amount) **as** avg_amount
**from**
    transactions
**group** **by**
    {{ dim | sqlsafe }}
**order** **by** total_amount **desc**
'''
```

为了从 JinjaSql 模板生成 SQL，我们将使用 GitHub 上的 [sql_tempates_base.py](https://github.com/sizrailev/life-around-data-code/blob/master/pylad/sql_templates_base.py) 中可用的`apply_sql_template`函数，这里介绍的[是](/a-simple-approach-to-templated-sql-queries-in-python-adc4f0dc511)。为了生成带有`payment_method`维度参数的 SQL 查询，我们按如下方式调用这个函数。

```
params = {
    'dim': 'payment_method'
}
sql = apply_sql_template(_BASIC_STATS_TEMPLATE, params)
```

上面的模板适用于单个维度，但是如果我们有多个维度呢？要生成适用于任意数量维度的通用查询，让我们创建一个函数的框架，该框架将维度列名列表作为参数，并返回 SQL。

```
**def** get_basic_stats_sql(dimensions):
    '''
    Returns the sql computing the number of transactions,
    as well as the total and the average transaction amounts
    for the provided list of column names as dimensions.
    '''
    # TODO: construct params
    **return** apply_sql_template(_BASIC_STATS_TEMPLATE, params)
```

本质上，我们希望将列名列表(如`['payment_method']`或`['store_id', 'payment_method']`)转换成包含列名的单个字符串，以逗号分隔的列表形式。这里我们有一些选项，因为它既可以在 Python 中完成，也可以在模板中完成。

# 传递在模板外部生成的字符串

第一个选项是在将字符串传递给模板之前生成逗号分隔的字符串。我们可以简单地将列表中的成员连接在一起:

```
**def** get_basic_stats_sql(dimensions):
    '''
    Returns the sql computing the number of transactions,
    as well as the total and the average transaction amounts
    for the provided list of column names as dimensions.
    '''
    params = {
       'dim': '\n    , '.join(dimensions)
    }
    **return** apply_sql_template(_BASIC_STATS_TEMPLATE, params)
```

碰巧模板参数`dim`在正确的位置，所以结果查询是

```
>>> print(get_basic_stats_sql(['store_id', 'payment_method']))
**select**
    store_id
    , payment_method
    , count(*) **as** num_transactions
    , sum(amount) **as** total_amount
    , avg(amount) **as** avg_amount
**from**
    transactions
**group** **by**
    store_id
    , payment_method
**order** **by** total_amount **desc**
```

现在，我们可以使用

```
dimension_lists = [
    ['user_id'],
    ['store_id'],
    ['payment_method'],
    ['store_id', 'payment_method'],
]dimension_queries = [get_basic_stats_sql(dims) **for** dims **in** dimension_lists]
```

# 模板中的预设变量

除了将预构建的字符串作为模板参数传递之外，还有一种方法是通过在顶部设置一个新变量，将列列表 SQL 生成移到模板本身中:

```
_PRESET_VAR_STATS_TEMPLATE = '''
{% **set** dims = '\n    , '.join(dimensions) %}
**select**
    {{ dims | sqlsafe }}
    , count(*) **as** num_transactions
    , sum(amount) **as** total_amount
    , avg(amount) **as** avg_amount
**from**
    transactions
**group** **by**
    {{ dims | sqlsafe }}
**order** **by** total_amount **desc**
'''
```

这个模板比以前的版本可读性更好，因为所有的转换都发生在模板中的一个地方，同时也没有混乱。该功能应更改为

```
**def** get_stats_sql(dimensions):
    '''
    Returns the sql computing the number of transactions,
    as well as the total and the average transaction amounts
    for the provided list of column names as dimensions.
    '''
    params = {
        'dimensions': dimensions
    }
    **return** apply_sql_template(_PRESET_VAR_STATS_TEMPLATE, params)
```

# 模板内的循环

我们也可以在模板中使用循环来生成列。

```
_LOOPS_STATS_TEMPLATE = '''
**select**
    {{ dimensions[0] | sqlsafe }}\
    {% **for** dim **in** dimensions[1:] %}
    , {{ dim | sqlsafe }}{% **endfor** %}
    , count(*) **as** num_transactions
    , sum(amount) **as** total_amount
    , avg(amount) **as** avg_amount
**from**
    transactions
**group** **by**
    {{ dimensions[0] | sqlsafe }}\
    {% **for** dim **in** dimensions[1:] %}
    , {{ dim | sqlsafe }}{% **endfor** %}
**order** **by** total_amount **desc**
'''
```

这个例子可能不是循环的最佳用途，因为一个预置变量可以很好地完成工作，而没有额外的复杂性。然而，循环是一个强大的构造，特别是当循环中有额外的逻辑时，比如条件(`{% if ... %} — {% endif %}`)或嵌套循环。

那么上面的模板中发生了什么呢？列表的第一个元素`dimensions[0]`是独立的，因为它不需要在列名前面加逗号。如果查询中有一个定义好的第一列，我们就不需要它了，for 循环看起来就像

```
{% **for** dim **in** dimensions %}
, {{ dim | sqlsafe }}
{% **endfor** %}
```

然后，for 循环结构遍历剩余的元素`dimensions[1:]`。同样的代码出现在`group by`子句中，这也是不理想的，只是为了显示循环功能。

有人可能想知道为什么循环的格式如此奇怪。原因是 SQL 模板的流元素，比如`{% endfor %}`，如果它们出现在单独的行上，就会生成一个空行。为了避免这种情况，在上面的模板中，`{% for ... %}`和`{% endfor %}`在技术上与前面的代码在同一行上(因此在第一个列名后面有反斜杠`\`)。当然，SQL 不关心空白，但是阅读 SQL 的人可能(也应该)关心空白。除了与模板中的格式进行斗争之外，还可以在打印或记录查询之前去掉生成的查询中的空行。为此，一个有用的功能是

```
**import** os**def** strip_blank_lines(text):
    '''
    Removes blank lines from the text, including those containing only spaces.
    [https://stackoverflow.com/questions/1140958/whats-a-quick-one-liner-to-remove-empty-lines-from-a-python-string](https://stackoverflow.com/questions/1140958/whats-a-quick-one-liner-to-remove-empty-lines-from-a-python-string)
    '''
    **return** os.linesep.join([s **for** s **in** text.splitlines() **if** s.strip()])
```

格式更好的模板就变成了

```
_LOOPS_STATS_TEMPLATE = '''
**select**
    {{ dimensions[0] | sqlsafe }}
    {% **for** dim **in** dimensions[1:] %}
    , {{ dim | sqlsafe }}
    {% **endfor** %}
    , count(*) **as** num_transactions
    , sum(amount) **as** total_amount
    , avg(amount) **as** avg_amount
**from**
    transactions
**group** **by**
    {{ dimensions[0] | sqlsafe }}
    {% **for** dim **in** dimensions[1:] %}
    , {{ dim | sqlsafe }}
    {% **endfor** %}
**order** **by** total_amount **desc**
'''
```

打印查询的调用是:

```
print(strip_blank_lines(get_loops_stats_sql(['store_id', 'payment_method'])))
```

到目前为止，所有的 SQL 模板都使用维度列表来产生完全相同的查询。

# 在字典上循环的自定义维度

在上面的循环例子中，我们看到了如何遍历一个列表。也可以迭代一个字典。这很方便，例如，当我们想要别名或转换形成维度的部分或全部列时。假设我们想将借记卡和信用卡合并为一个值，并将其与现金交易进行比较。我们可以通过首先创建一个字典来定义`payment_method` 的转换并保持`store_id`不变来实现。

```
custom_dimensions = {
    'store_id': 'store_id',
    'card_or_cash': "case when payment_method = 'cash' then 'cash' else 'card' end",
}
```

这里，`credit`和`debit`值都被替换为`card`。然后，该模板可能如下所示:

```
_CUSTOM_STATS_TEMPLATE = '''
{% **set** dims = '\n    , '.join(dimensions.keys()) %}
**select**
    sum(amount) **as** total_amount
    {% **for** dim, def **in** dimensions.items() %}
    , {{ def | sqlsafe }} **as** {{ dim | sqlsafe }}
    {% endfor %}
    , count(*) **as** num_transactions
    , avg(amount) **as** avg_amount
**from**
    transactions
**group** **by**
    {{ dims | sqlsafe }}
**order** **by** total_amount **desc**
'''
```

注意，我将`total_amount`作为第一列，只是为了简化这个例子，避免单独处理循环中的第一个元素。另外，请注意,`group by`子句使用了一个预置变量，并且不同于 select 查询中的代码，因为它只列出了生成的列的名称。产生的 SQL 查询是:

```
>>> print(strip_blank_lines(
...     apply_sql_template(template=_CUSTOM_STATS_TEMPLATE,
...                        parameters={'dimensions': custom_dimensions})))**select**
    sum(amount) **as** total_amount
    , store_id **as** store_id
    , **case** **when** payment_method = 'cash' **then** 'cash' **else** 'card' **end** **as** card_or_cash
    , count(*) **as** num_transactions
    , avg(amount) **as** avg_amount
**from**
    transactions
**group** **by**
    store_id
    , card_or_cash
**order** **by** total_amount **desc**
```

# 从 JinjaSql 模板中调用定制 Python 函数

如果我们想用 Python 函数生成一部分代码呢？Jinja2 允许用户注册自定义函数和其他包中的函数，以便在 SQL 模板中使用。让我们从定义一个函数开始，该函数生成我们插入到 SQL 中的字符串，用于转换定制维度。

```
**def** transform_dimensions(dimensions: dict) -> str:
    '''
    Generate SQL for aliasing or transforming the dimension columns.
    '''
    **return** '\n    , '.join([
        '{val} as {key}'.format(val=val, key=key)
        **for** key, val **in** dimensions.items()
    ])
```

这个函数的输出是我们期望出现在`select`子句中的内容:

```
>>> print(transform_dimensions(custom_dimensions))
store_id **as** store_id
 , **case** **when** payment_method = 'cash' **then** 'cash' **else** 'card' **end** **as** card_or_cash
```

现在我们需要用 Jinja2 注册这个函数。为此，我们将修改前面博客中的[函数，如下所示。](http://www.lifearounddata.com/templated-sql-queries-in-python/)

```
**from** jinjasql **import** JinjaSql**def** apply_sql_template(template, parameters, func_list=None):
    '''
    Apply a JinjaSql template (string) substituting parameters (dict) and return
    the final SQL. Use the func_list to pass any functions called from the template.
    '''
    j = JinjaSql(param_style='pyformat')
    **if** func_list:
        **for** func **in** func_list:
        j.env.globals[func.__name__] = func
    query, bind_params = j.prepare_query(template, parameters)
    **return** get_sql_from_template(query, bind_params)
```

这个版本有一个额外的可选参数`func_list`，它需要是函数的一个`list`。

让我们改变模板来利用`transform_dimensions`功能。

```
_FUNCTION_STATS_TEMPLATE = '''
{% **set** dims = ‘\n , ‘.join(dimensions.keys()) %}
**select**
    {{ transform_dimensions(dimensions) | sqlsafe }}
    , sum(amount) **as** total_amount
    , count(*) **as** num_transactions
    , avg(amount) **as** avg_amount
**from**
    transactions
**group** **by**
    {{ dims | sqlsafe }}
**order** **by** total_amount **desc**
'''
```

现在我们也不需要担心第一列没有逗号。下面的调用产生一个类似于上一节的 SQL 查询。

```
>>> print(strip_blank_lines(
...     apply_sql_template(template=_FUNCTION_STATS_TEMPLATE,
...                        parameters={‘dimensions’: custom_dimensions},
...                        func_list=[transform_dimensions])))**select**
    store_id **as** store_id
    , **case** **when** payment_method = 'cash' **then** 'cash' **else** 'card' **end** **as** card_or_cash
    , sum(amount) **as** total_amount
    , count(*) **as** num_transactions
    , avg(amount) **as** avg_amount
**from**
    transactions
**group** **by**
    store_id
    , card_or_cash
**order** **by** total_amount **desc**
```

注意我们如何将`transform_dimensions`作为一个列表`[transform_dimensions]`传递给`apply_sql_template`。可以将多个函数作为函数列表传递给 SQL 模板，例如`[func1, func2]`。

# 结论

本教程详细介绍了 JinjaSql 的[基本用法](/a-simple-approach-to-templated-sql-queries-in-python-adc4f0dc511)。它演示了在 JinjaSql 模板中使用预置变量、列表和字典循环以及定制 Python 函数来生成高级 Sql 代码。特别是在`apply_sql_template`函数中添加了自定义函数注册，使得模板更加强大和通用。参数化 SQL 查询对于自动生成报告和减少需要维护的 SQL 代码量仍然是不可或缺的。一个额外的好处是，使用可重用的 SQL 代码片段，使用标准的 Python 单元测试技术来验证生成的 SQL 是正确的变得更加容易。

本帖中的[代码是在](https://github.com/sizrailev/life-around-data-code/blob/master/pylad/advanced_sql_templates.py) [MIT 许可](https://opensource.org/licenses/MIT)下授权的。这篇文章最初出现在 [Life Around Data](http://www.lifearounddata.com/advanced-sql-templates-in-python-with-jinjasql/) 博客上。

谢尔盖·伊兹拉列夫拍摄的照片