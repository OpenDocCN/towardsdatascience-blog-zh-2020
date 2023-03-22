# 为您的 SQL 数据库构建回归测试框架

> 原文：<https://towardsdatascience.com/how-to-regression-test-your-sql-database-c6c8d66848c5?source=collection_archive---------12----------------------->

## 了解如何使用 Python 构建 SQL 回归框架

![](img/f0fafec5ede8f1c1bf1059b89acb59e5.png)

卡斯帕·卡米尔·鲁宾在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

当谈到测试软件时，有无数种方法可以做。回归测试就是其中的一种方法，它本质上是一种比较软件的两个不同版本的输出的方法。

比较软件的两个版本的输出将很快突出相同点和不同点。如果软件中的一个变化导致了一个意想不到的不同，它会立即被突出显示。这同样适用于预期有差异但没有发现的情况。

随着解决方案规模的增长，以及它们的输出在数量和复杂性上的增加，必须建立一个框架来允许这种类型的测试快速、轻松地执行。手动测试容易出错且耗时。

# 这个想法

![](img/42b9cad826e1aac1d017b938c6c2290a.png)

由[活动创作者](https://unsplash.com/@campaign_creators?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

这个想法非常简单。构建一个 python 脚本，允许我们快速有效地比较两个 SQL 数据库。该脚本需要尽可能可扩展和可配置。

更具体地说，该脚本应该允许多环境设置和多数据集比较。换句话说，用户应该能够设置不同的环境和要执行的 SQL 命令，然后通过几个配置更改就可以运行它。

事不宜迟，让我们进入细节。

# 魔力

## 连接到 SQL Server

连接到 SQL server 很简单，但是根据 SQL server 的类型，您需要使用不同的库。对于这个例子，我使用的是 Microsoft SQL server，因此使用的是 pyodbc 库。

```
import pyodbc
import pandas as pd
Env1 = pyodbc.connect('Driver = {SQL Server}; Server = ServerName; Database = DatabaseName; Trusted_Connection = yes;')
SQL = 'SELECT * FROM Table1'
print(pd.read_sql_query(SQL, con = Env1)
```

## 比较两个数据帧

这里没有必要重新发明轮子；我们将使用现有的库来比较数据集。

```
import datacompy
import pandas as pd
df1 = pd.read_csv('FL_insurance_sample.csv')
df2 = pd.read_csv('FL_insurance_sample - Copy.csv')
compare = datacompy.Compare(
    df1,
    df2,
    join_columns='policyID',  #You can also specify a list of columns eg ['policyID','statecode']
    abs_tol=0, #Optional, defaults to 0
    rel_tol=0, #Optional, defaults to 0
    df1_name='Original', #Optional, defaults to 'df1'
    df2_name='New' #Optional, defaults to 'df2'
)
print(compare.report())
```

## 有趣的是

这个练习有趣的部分是把所有的东西放在一起。本质上，我们需要创建一个要执行的 SQL 语句列表，以及用于比较的结果的相关数据键。

一旦定义好，我们就可以遍历 SQL 语句列表，在我们的两个环境中执行每个语句。一旦我们得到结果，使用我们的比较库，我们可以评估是否有任何差异。为了提高效率和减少返回的数据，我们将在尝试获得更细粒度的反馈之前，首先进行“哑”的同类比较。

# 代码

```
import pyodbc, datacompy
import pandas as pd###############Set up your Environment connections#########################
Env1 = pyodbc.connect('Driver = {SQL Server}; Server = ServerName; Database = DatabaseName; Trusted_Connection = yes;')
Env2 = pyodbc.connect('Driver = {SQL Server}; Server = ServerName2; Database = DatabaseName2; Trusted_Connection = yes;')########Environment Set Up###################################
Original = Env1
Target = Env2######SQL Statements to Execute#############################
SQLStatement = [
    #[ReportName, Stored Proc, [data keys]]
    ['Report1','EXEC StoredProc1 [@Var1](http://twitter.com/Var1) = "Woop" , [@Var2](http://twitter.com/Var2) = "Woop2"', ['Key1', 'Key2']],
    ['Report2','EXEC StoredProc2 [@Var1](http://twitter.com/Var1) = "Woop", [@Var2](http://twitter.com/Var2) = "Woop2"', ['Key3','Key4']]
]###############################
def compareResults(reportName, df1, df2, joinOnKeys):
    if df1.equals(df2) == True:
        print('###########################################################')
        print(reportName, 'Success')
    else:
        print('###########################################################')
        print(reportName, 'Fail')
        compare = datacompy.Compare(
            df1,
            df2,
            join_columns= joinOnKeys,
            abs_tol = 0, 
            rel_tol = 0,
            df1_name= 'Original',
            df2_name= 'Target'
        )
        print(compare.report(sample_count=20))for i in SQLStatement:
    results1 = pd.read_sql_query(i[1], con = Original)
    results2 = pd.read_sql_query(i[1], con = Target)
    compareResults(i[0], results1, results2, i[2])
```

## 示例输出

```
###########################################################
Report1 Success###########################################################
Report2 FailDataComPy Comparison
--------------------
DataFrame Summary
-----------------
DataFrame  Columns   Rows
0  Original       18  36634
1       New       18  36634Column Summary--------------
Number of columns in common: 18
Number of columns in Original but not in New: 0
Number of columns in New but not in Original: 0Row Summary
-----------
Matched on: policyid
Any duplicates on match values: No
Absolute Tolerance: 0
Relative Tolerance: 0
Number of rows in common: 36,634
Number of rows in Original but not in New: 0
Number of rows in New but not in Original: 0
Number of rows with some compared columns unequal: 2
Number of rows with all compared columns equal: 36,632Column Comparison
-----------------
Number of columns compared with some values unequal: 2
Number of columns compared with all values equal: 16
Total number of values which compare unequal: 2Columns with Unequal Values or Types
------------------------------------
Column Original dtype New dtype  # Unequal   Max Diff  # Null Diff
0  eq_site_limit        float64   float64          1  190601.40            0
1  hu_site_limit        float64   float64          1   79375.76            0Sample Rows with Unequal Values
-------------------------------
policyid  eq_site_limit (Original)  eq_site_limit (New)
2    206893                  190724.4                123.0
policyid  hu_site_limit (Original)  hu_site_limit (New)
3    333743                  79520.76                145.0
```

# 结论

好了，伙计们！用不到 50 行代码，您就有了一个完整的 SQL 回归测试框架来帮助您确定下一个候选版本的有效性。

您所需要做的就是选择正确的两个环境，并定义您想要执行的存储过程的列表。

你会用不同的方式做事情吗，或者对改进这段代码有什么建议吗？让我知道！

如果你觉得这篇博客有用，你可能也会喜欢:

[](/building-a-python-ui-for-comparing-data-13c10693d9e4) [## 构建用于比较数据的 Python UI

### 如何快速让您的非技术团队能够比较数据

towardsdatascience.com](/building-a-python-ui-for-comparing-data-13c10693d9e4) [](/automate-ui-testing-with-pyautogui-in-python-4a3762121973) [## 用 Python 中的 PyAutoGUI 实现自动化 UI 测试

### 回归测试你的用户界面的一个快速简单的方法

towardsdatascience.com](/automate-ui-testing-with-pyautogui-in-python-4a3762121973) [](https://medium.com/financeexplained/from-excel-and-pandas-dataframes-to-sql-f6e9c6b4a36a) [## 从 Excel 和 Pandas 数据框架到 SQL

### 如何使用您的 SQL 知识来利用 Python 的熊猫

medium.com](https://medium.com/financeexplained/from-excel-and-pandas-dataframes-to-sql-f6e9c6b4a36a)