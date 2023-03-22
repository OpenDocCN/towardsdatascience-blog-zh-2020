# 用 Python & Stan 用一个不切实际的模型选出超级碗冠军

> 原文：<https://towardsdatascience.com/using-python-stan-to-pick-the-superbowl-winner-with-an-unrealistic-model-e68c84c3e95a?source=collection_archive---------25----------------------->

## 贝叶斯逻辑回归的一个简单例子

![](img/42da90732c8ee993de5afb22d6d06d13.png)

戴夫·阿达姆松在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

本文将说明一种使用 Stan 构建简单、不切实际但有用的概率模型的方法，以:

*   根据球队进攻、对手防守以及球队是否在主场比赛，预测每支 NFL 球队在一场比赛中的得分。
*   使用该模型模拟一个时间表，其中每支球队与所有 31 支球队进行 100 次主客场比赛。
*   使用结果为所有 32 个团队生成一个实力排名。
*   使用该模型预测季后赛的结果&最终是超级碗。

# **原定目标**

我本来是想找一个小问题，学习如何使用 Stan 进行贝叶斯建模。NFL 权力排名模型似乎是一个有趣的尝试。某一年的数据样本量很小，因此很有吸引力:

*   32 支队伍
*   16 场比赛
*   512 种结果——每场比赛每队一种
*   随着新分数的出现，该模型可以每周更新。

从斯坦的主页——https://mc-stan.org/——可以下载该软件。

# 关于斯坦

“Stan 是用于统计建模和高性能统计计算的一流平台。成千上万的用户依赖 Stan 进行社会、生物、物理科学、工程和商业领域的统计建模、数据分析和预测。

用户在 Stan 的概率编程语言中指定对数密度函数，得到:

*   使用 MCMC 抽样的完全贝叶斯统计推断(NUTS，HMC)
*   带有变分推理的近似贝叶斯推理
*   优化的惩罚最大似然估计(L-BFGS)”

# **数据**

数据很容易从 https://www.pro-football-reference.com/years/2019/games.htm 获得。只需单击共享箭头选项卡，即可获得要复制的数据的 csv 布局。

为了保持模型简单，模型需要的字段是:

*   获胜队
*   失败的队伍
*   获胜团队得分
*   失去团队分数
*   主/从指示器—>@

# **不切实际的分数概率模型**

在实际比赛中，得分事件产生 1、2、3 或 6 分，并且是 3 个以上因素的结果。

将被评估的模型将把分数仅仅作为球队进攻、对手防守以及球队是否在主场比赛的函数。

用来预测球队得分的不切实际的模型是:

*   一个队可以得 0 到 60 分
*   团队得分的点数被建模为具有 60 次试验的二项式分布，并且成功概率 p 如上所述被估计。

将使用 Pystan 对模型进行估计，并对模型中的特征进行先验分布，如下所述。

# **数据处理**

抱歉——为读到这篇文章的任何数据科学家争论——我道歉。

进口:

```
import pandas as pd
import numpy as np
import pystan
from   pystan import StanModel
```

(1)读入保存的结果和重命名列的 csv 文件:

```
df         = pd.read_csv("~/ml/Data/NFL2019.csv")df.columns = ['Week', 'Day', 'Date', 'Time', 'Winner',    'away','Loser','Unnamed: 7', 'PtsW', 'PtsL', 'YdsW', 'TOW', 'YdsL', 'TOL']
```

(2)建立将城市队名称映射到队名称和队 id 的字典。最初，我认为我必须构建一个 256 X 65 X 的设计矩阵，因此团队 id 在下面的代码中是 col_id。稍后会详细介绍。

如果 col_id 没有从 1 开始，问题可能会在以后出现，因为 Stan 默认从 1 而不是 0 开始。

```
team_dict = {}
col_id    = 1for i in range(df.shape[0]):
    team_ = df.Winner[i]
    temp  = team_.split()
    temp  = temp[-1]
    if team_ not in team_dict:
        team_dict[team_] = dict(team=temp,k= col_id,)
        col_id += 1
```

带有 k 的前几个 team_dict 条目是 team_id:

```
{'Green Bay Packers': {'team': 'Packers', 'k': 1},
 'Minnesota Vikings': {'team': 'Vikings', 'k': 2},
 'Los Angeles Rams': {'team': 'Rams', 'k': 3},
 'Baltimore Ravens': {'team': 'Ravens', 'k': 4},
 'Philadelphia Eagles': {'team': 'Eagles', 'k': 5},
```

(3)构建反向字典，将列 id 映射回团队

```
X_dict = {}
for key in team_dict:
    x_key = team_dict[key]['team']
    X_dict[x_key] = team_dict[key]['k']
```

前几个 X_dict 条目:

```
{'Packers': 1,
 'Vikings': 2,
 'Rams': 3,
 'Ravens': 4,
 'Eagles': 5,
```

(4)构建 X 矩阵以馈送给 Stan。它将有 3 列:

*   我们预测其得分的团队的 id
*   它正在玩的队的 id(防御)
*   基于数据中@符号的 home 0/1 指示器

此外，存储分数。

对于数据中的每一行，在 X 矩阵中创建两行，一行用于获胜的团队，一行用于失败的团队。idx_o & idx_d 是 team_dict 中攻防队的指数。

这导致在 3 列向量中存储可能非常稀疏的矩阵(在 65 列的每行中只有 3 个非零元素)。

```
X      = np.zeros((2*df.shape[0],3),int)
score  = np.zeros(2*df.shape[0],int)
row    = 0for i in range(df.shape[0]):
    idx_o    = team_dict[df.Winner[i]]['k']
    idx_d    = team_dict[df.Loser[i]]['k']
    X[row,0] = idx_o                         
    X[row,1] = idx_d
    score[row]     = df.PtsW.values[i]
    if df.away[i] != '@':
        X[row,2] = 1 
    row += 1

    idx_o = team_dict[df.Loser[i]]['k']
    idx_d = team_dict[df.Winner[i]]['k']
    X[row,0] = idx_o
    X[row,1] = idx_d
    score[row]     = df.PtsL.values[i]
    if df.away[i] == '@':
        X[row,2] = 1
```

x 矩阵的前几行:

```
array([[ 1, 20,  0],
       [20,  1,  1],
       [ 2, 21,  1],
       [21,  2,  0],
       [ 3, 25,  0],
       [25,  3,  1],
```

# **使用 Stan 评估模型**

(1)Stan 的数据字典定义—相当简单，告诉 Stan 数据是什么以及有多少行(N)。

```
datadict  = { 
    'offense'  : X[:,0] ,
    'defense'  : X[:,1],
    'home'     : X[:,2] ,
    'score'    : score,
    'N'        : X.shape[0]
}
```

(2)定义 Stan 模型——Python 多行字符串。

组件:

*   ***数据*** —如上面的 datadict 中所定义。
*   ***参数*** —我们希望 Stan 估计的参数列表。一个用于每个队的进攻和防守，一个用于主场比赛和拦截。主队系数有一个上限。
*   ***转换后的参数*** —该模型估计 **p 的对数，**我们不切实际的二项式模型中的概率。函数 ***inv_logit*** 将登录次数转换成成功的概率(在我们的例子中是 60 次试验中每一次的分数)。
*   ***模型—*** 在模型部分，先验分布与我们正在估计的概率模型(得分)一起指定。

```
stanCode = """
data {
int N;
int score [N];
int offense [N];
int defense [N];
int home    [N];
}
parameters {
vector      [32]       b_offense;
vector      [32]       b_defense;
real <upper = 0.05>    b_home;
real                   alpha;
}
transformed parameters{
  real mu [N];
  for (n in 1:N){
    mu[n]  <- inv_logit(alpha + b_offense[offense[n]] 
                              + b_defense[defense[n]]
                              + b_home * home[n]);
  }
}model {
alpha      ~ normal(log(23),1);
b_offense  ~ cauchy(0,0.5);
b_defense  ~ cauchy(0,0.5);
b_home     ~ double_exponential(0,5);
score      ~ binomial(60,mu);}
"""
```

(3)编译 Stan 模型——这需要一些时间。将打印出一条信息消息。

```
from pystan import StanModel
sm = StanModel(model_code=stanCode)
```

(4)估计模型系数——op 将是一个字典，包含每个队的进攻和防守系数、主客场系数、截距和预测 mu。

```
op = sm.optimizing(data=datadict)
```

# **实力排名**

在这一点上，我们现在已经有了模型权重，它使我们能够通过在游戏中运行 sigmoid 变换，根据任何两个队的进攻和防守系数以及该队是否在主场，来预测他们在游戏中的得分。

我们可以模拟任何球队和其他球队比赛的结果…不限于实际比赛。那么为什么不去做呢？我们模拟了一个时间表，其中每支球队与其他球队“比赛”100 次，50 次在主场，50 次在客场。我们记录赢、输和平局。

首先，我们需要调整索引(team_ids ),因为模拟将在 Python 中进行，并构建一个团队列表来运行模拟。

```
team_list = []
for team in X_dict:
    team_list.append(team)
    X_dict[team] = X_dict[team] - 1
    print("{a:>20s}  {b:6.3f}  {c:6.3f}".format(
        a=team, 
        b=op['b_offense'][X_dict[team]],
        c=op['b_defense'][X_dict[team]]))
```

检查前几行——乌鸦和酋长队的进攻系数非常正，防守系数为负，而老鹰队接近平均水平。

```
 offense  defense
Packers   0.037   -0.203              
Vikings   0.167   -0.226                 
Rams      0.147   -0.131               
Ravens    0.752   -0.358               
Eagles    0.047    0.018                
Bills    -0.252   -0.450               
Chiefs    0.427   -0.226
```

模拟所有球队之间的 100 场比赛赛季:

```
wins   = np.zeros(len(team_list),int)
losses = np.zeros(len(team_list),int)
ties   = np.zeros(len(team_list),int)
for idx_1 in range(31):
    for idx_2 in range(idx_1+1,32):
        team_1 = team_list[idx_1]
        team_2 = team_list[idx_2]

        for game in range(100):
            at_home = game % 2 u      = op['alpha'] 
            u     += op['b_offense'][X_dict[team_1]] 
            u     += op['b_defense'][X_dict[team_2]] 
            u     += op['b_home'] * at_home
            prob   = 1\. / (1\. + np.exp(-u))
            pts_1  = np.random.binomial(60,prob) u      = op['alpha'] 
            u     += op['b_offense'][X_dict[team_2]] 
            u     += op['b_defense'][X_dict[team_1]] 
            u     += op['b_home'] * (1 - at_home)
            prob   = 1\. / (1\. + np.exp(-u))
            pts_2  = np.random.binomial(60,prob) if pts_1 > pts_2:
                wins[idx_1]   += 1
                losses[idx_2] += 1
            elif pts_1 < pts_2:
                wins[idx_2]   += 1
                losses[idx_1] += 1
            else:
                ties[idx_1]   += 1
                ties[idx_2]   += 1
```

结果——按获胜百分比排列的实力等级(平局算作 1/2 的胜负)。

```
report = pd.DataFrame(dict(
    team = team_list,
    won  = wins,
    lost = losses,
    ties = ties
))
report['winpct'] = np.round((report.won + 0.5 * report.ties) / (report.won + report.lost + report.ties),3)report.sort_values('winpct',ascending=False,inplace=True)
report.reset_index(inplace=True)
report = report[['team','won','lost','ties','winpct']]
print(report)
```

【2019 赛季模拟实力排名:

这些排名通过了一个意义上的检验——强队在顶部，弱队在底部。大部分球队都在中心，公羊队和牛仔队在排名上表现不佳，而海鹰队和老鹰队表现出色，进入了季后赛。

```
 team    won  lost  ties  winpct
0       Ravens  3025    53    22   0.979
1     Patriots  2823   230    47   0.918
2        49ers  2795   253    52   0.910
3       Chiefs  2653   379    68   0.867
4       Saints  2460   558    82   0.807
5      Cowboys  2269   726   105   0.749
6      Vikings  2270   735    95   0.748
7         Rams  2104   867   129   0.700
8       Titans  2014   942   144   0.673
9      Packers  1997   979   124   0.664
10    Seahawks  1882  1073   145   0.630
11       Bills  1882  1081   137   0.629
12      Texans  1589  1371   140   0.535
13  Buccaneers  1593  1377   130   0.535
14      Eagles  1585  1366   149   0.535
15    Steelers  1545  1391   164   0.525
16     Falcons  1487  1469   144   0.503
17       Bears  1342  1594   164   0.459
18    Chargers  1297  1666   137   0.440
19       Colts  1237  1716   147   0.423
20      Browns  1213  1739   148   0.415
21     Broncos  1212  1754   134   0.413
22   Cardinals  1032  1922   146   0.356
23       Lions   750  2199   151   0.266
24        Jets   603  2369   128   0.215
25     Raiders   586  2403   111   0.207
26     Jaguars   587  2415    98   0.205
27    Panthers   552  2441   107   0.195
28     Bengals   511  2479   110   0.183
29      Giants   473  2529    98   0.168
30    Redskins   209  2826    65   0.078
31    Dolphins   178  2853    69   0.069
```

**模拟季后赛**

用于获取获胜概率的 1 场比赛的函数:

```
def sim_games(team_1,team_2,row_id,report,s_round,add_home=True):
    wins   = np.zeros(2,int)
    losses = np.zeros(2,int)
    ties   = np.zeros(2,int)
    score  = np.zeros((1000,2),int) for game in range(1000):        
        u      = op['alpha'] 
        u     += op['b_offense'][X_dict[team_1]]   
        u     += op['b_defense'][X_dict[team_2]]  
        u     += op['b_home'] * add_home
        prob   = 1\. / (1\. + np.exp(-u))
        pts_1  = np.random.binomial(60,prob)

        score[game,0] = pts_1 u      = op['alpha'] 
        u     += op['b_offense'][X_dict[team_2]]   
        u     += op['b_defense'][X_dict[team_1]]  
        u     += op['b_home'] * add_home
        prob   = 1\. / (1\. + np.exp(-u))
        pts_2  = np.random.binomial(60,prob)

        score[game,1] = pts_2 if pts_1 > pts_2:
            wins[0]   += 1
            losses[1] += 1
        elif pts_1 < pts_2:
            wins[1]   += 1
            losses[0] += 1
        else:
            ties[0]   += 1
            ties[1]   += 1new_row = pd.DataFrame(dict(
        Round        = s_round,
        Visitor      = team_1,
        V_Wins       = int(wins[0]+ ties[0]/2),
        V_Score      = np.round(np.mean(score[:,0]),1),
        Home         = team_2,
        H_Wins       = int(wins[1]+ ties[1]/2),
        H_Score      = np.round(np.mean(score[:,1]),1)
),index=[row_id])report = pd.concat((report,new_row))
return(report) 
```

**模拟季后赛的函数调用:**

进行第一轮比赛，并将预计的获胜者传送到第二轮比赛。

```
report = pd.DataFrame()
row_id = 0
report = sim_games("Bills","Texans",
row_id,report,s_round='Round 1',add_home=True)
row_id = 1
report = sim_games("Titans","Patriots",
row_id,report,s_round='Round 1',add_home=True)
row_id = 2
report = sim_games("Seahawks","Eagles",
row_id,report,s_round='Round 1',add_home=True)
row_id = 4
report = sim_games("Vikings","Saints",
row_id,report,s_round='Round 1',add_home=True)
row_id = 5
report = sim_games("Bills","Ravens",
row_id,report,s_round='Round 2',add_home=True)
row_id = 6
report = sim_games("Patriots","Chiefs",
row_id,report,s_round='Round 2',add_home=True)
row_id = 7
report = sim_games("Seahawks","49ers",
row_id,report,s_round='Round 2',add_home=True)
row_id = 8
report = sim_games("Saints","Packers",
row_id,report,s_round='Round 2',add_home=True)
row_id = 9
report = sim_games("Saints","49ers",
row_id,report,s_round='Round 3',add_home=True)
row_id = 10
report = sim_games("Patriots","Ravens",
row_id,report,s_round='Round 3',add_home=True)
row_id = 11
report = sim_games("49ers","Ravens",
row_id,report,s_round='superbowl',add_home=False)
```

# **结果**

```
 Round   Visitor  V_Wins  V_Score      Home  H_Wins  H_Score
0     Round 1     Bills     648     19.9    Texans     352     17.9
1     Round 1    Titans      77     16.6  Patriots     922     23.9
2     Round 1  Seahawks     685     26.3    Eagles     315     23.5
4     Round 1   Vikings     392     22.9    Saints     607     24.3
5     Round 2     Bills       9     15.4    Ravens     990     27.2
6     Round 2  Patriots     665     22.5    Chiefs     335     20.0
7     Round 2  Seahawks      63     21.3     49ers     936     29.5
8     Round 2    Saints     792     25.1   Packers     207     20.9
9     Round 3    Saints     253     23.2     49ers     746     26.6
10    Round 3  Patriots     233     20.5    Ravens     766     24.3
11  superbowl     49ers     166     24.3    Ravens     834     29.6
```

# **总结**

2019 年的 NFL 结果被用来建立一个使用 PyStan 预测分数的模型，更重要的是作为一个如何使用 Stan 估计贝叶斯模型的例子。

随着数据的可用和赛季的进展，该模型可用于生成每周电力排名。

一些观察结果:

*   尽管该模型在权力排名上做得相当好，但预测的分数似乎受到了先验的影响。
*   具有更大分布的先验可能导致更宽的预测得分范围。
*   模型的弱点包括模型是基于整个赛季到目前为止，没有额外的重量给予最近的分数，关键的伤病或球员回归。

**问题&建议**

如果您有任何问题、建议或需要预测模型，请随时发送电子邮件至:

bill.fite@miner3.com。