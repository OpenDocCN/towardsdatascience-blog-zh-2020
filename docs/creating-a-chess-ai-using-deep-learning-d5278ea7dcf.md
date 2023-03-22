# 使用深度学习创建国际象棋人工智能

> 原文：<https://towardsdatascience.com/creating-a-chess-ai-using-deep-learning-d5278ea7dcf?source=collection_archive---------0----------------------->

## 使用神经网络解码世界上最古老的游戏…

![](img/503fe09dc7c6090b5b72ee346651bded.png)

哈桑·帕夏在 [Unsplash](https://unsplash.com/s/photos/chess?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

当加里·卡斯帕罗夫被 IBM 的深蓝国际象棋算法废黜时，该算法没有使用机器学习，或者至少以我们今天定义机器学习的方式使用。

本文旨在通过使用神经网络，一种更新形式的机器学习算法，使用神经网络来创建一个成功的国际象棋人工智能。

# 概念:

使用超过 20，000 个实例的国际象棋数据集(联系 victorwtsim@gmail.com 获取数据集)，当给定一个棋盘时，神经网络应该输出一个移动。

下面是代码的 [github](https://github.com/victorsimrbt) 回购:

# 代码:

## 第一步|准备:

```
import os
import chess
import numpy as np
import pandas as pd
from tensorflow import keras
from tensorflow.keras import layers
```

这些库是创建程序的先决条件:操作系统和 pandas 将访问数据集，python-chess 是测试神经网络的“即时”棋盘。Numpy 是执行矩阵操作所必需的。Keras 的任务是创建神经网络。

## 第 2 步|访问数据:

```
os.chdir('XXXXXXXXXXX')
df = pd.read_csv('chess_normalized.csv')
data = df['moves'].tolist()[:500]
split_data = []
indice = 500
```

对于这个项目，我们需要的只是数据集中每个棋局的 pgn。请更改 os.chdir 函数的目录路径，以访问数据集所在的目录。

## 第三步|热门词典:

```
chess_dict = {
    'p' : [1,0,0,0,0,0,0,0,0,0,0,0],
    'P' : [0,0,0,0,0,0,1,0,0,0,0,0],
    'n' : [0,1,0,0,0,0,0,0,0,0,0,0],
    'N' : [0,0,0,0,0,0,0,1,0,0,0,0],
    'b' : [0,0,1,0,0,0,0,0,0,0,0,0],
    'B' : [0,0,0,0,0,0,0,0,1,0,0,0],
    'r' : [0,0,0,1,0,0,0,0,0,0,0,0],
    'R' : [0,0,0,0,0,0,0,0,0,1,0,0],
    'q' : [0,0,0,0,1,0,0,0,0,0,0,0],
    'Q' : [0,0,0,0,0,0,0,0,0,0,1,0],
    'k' : [0,0,0,0,0,1,0,0,0,0,0,0],
    'K' : [0,0,0,0,0,0,0,0,0,0,0,1],
    '.' : [0,0,0,0,0,0,0,0,0,0,0,0],
}alpha_dict = {
    'a' : [0,0,0,0,0,0,0],
    'b' : [1,0,0,0,0,0,0],
    'c' : [0,1,0,0,0,0,0],
    'd' : [0,0,1,0,0,0,0],
    'e' : [0,0,0,1,0,0,0],
    'f' : [0,0,0,0,1,0,0],
    'g' : [0,0,0,0,0,1,0],
    'h' : [0,0,0,0,0,0,1],
}number_dict = {
    1 : [0,0,0,0,0,0,0],
    2 : [1,0,0,0,0,0,0],
    3 : [0,1,0,0,0,0,0],
    4 : [0,0,1,0,0,0,0],
    5 : [0,0,0,1,0,0,0],
    6 : [0,0,0,0,1,0,0],
    7 : [0,0,0,0,0,1,0],
    8 : [0,0,0,0,0,0,1],
}
```

一键编码是必要的，以确保没有特征或某些实例的权重高于其他特征或实例，从而在数据中产生偏差并阻碍网络的学习。pgn 值中的每个移动和配置都被改变成矩阵，在适当的列中具有 1。

## 步骤 4|准备数据的初步功能:

```
def make_matrix(board): 
    pgn = board.epd()
    foo = []  
    pieces = pgn.split(" ", 1)[0]
    rows = pieces.split("/")
    for row in rows:
        foo2 = []  
        for thing in row:
            if thing.isdigit():
                for i in range(0, int(thing)):
                    foo2.append('.')
            else:
                foo2.append(thing)
        foo.append(foo2)
    return foodef translate(matrix,chess_dict):
    rows = []
    for row in matrix:
        terms = []
        for term in row:
            terms.append(chess_dict[term])
        rows.append(terms)
    return rows
```

这两个函数是将板翻译成 ascii 形式然后翻译成矩阵的两个初步函数。

## 第 5 步|创建数据:

```
for point in data[:indice]:
    point = point.split()
    split_data.append(point)

data = []
for game in split_data:
    board = chess.Board()
    for move in game:
        board_ready = board.copy()
        data.append(board.copy())
        board.push_san(move)
trans_data = []
for board in data:
    matrix = make_matrix(board)
    trans = translate(matrix,chess_dict)
    trans_data.append(trans)
pieces = []
alphas = []
numbers = []
```

对于神经网络的输入，板本身就足够了:板本身保存时态数据，尽管顺序不正确。添加多个板来形成临时数据对我可怜的 8GB 内存来说计算量太大了。

将原始输入作为移动将移除时态数据，并阻止卷积层从数据中提取特征。在第一行中，变量 indice 是可选的。我添加了这个变量来减少数据大小，以便在向上扩展之前测试网络和数据是否正常工作。

## 第 6 步|转换数据:

```
true_data = flatten(split_data)
for i in range(len(true_data)):
    try:
        term = flatten(split_data)[i]
        original = term[:]
        term = term.replace('x','')
        term = term.replace('#','')
        term = term.replace('+','')
        if len(term) == 2:
            piece = 'p' 
        else:
            piece = term[0]
        alpha = term[-2]
        number = term[-1]
        pieces.append(chess_dict[piece])
        alphas.append(alpha_dict[alpha])
        numbers.append(number_dict[int(number)])
    except:
        pass
```

这段代码通过使用 try except 函数并删除 check 或 checkmate 的所有额外符号，删除了所有不能一键编码的实例。

不幸的是，这意味着程序永远也学不会城堡。

## 步骤 7|创建神经网络:

```
board_inputs = keras.Input(shape=(8, 8, 12))conv1= layers.Conv2D(10, 3, activation='relu')
conv2 = layers.Conv2D(10, 3, activation='relu')
pooling1 = layers.MaxPooling2D(pool_size=(2, 2), strides=None, padding="valid", data_format=None,)
pooling2 = layers.MaxPooling2D(pool_size=(2, 2), strides=None, padding="valid", data_format=None,)
flatten = keras.layers.Flatten(data_format=None)x = conv1(board_inputs)
x = pooling1(x)
x = conv2(x)
x = flatten(x)
piece_output = layers.Dense(12,name = 'piece')(x)model_pieces = keras.Model(inputs=board_inputs, outputs=piece_output, name="chess_ai_v3")
earlystop = keras.callbacks.EarlyStopping(monitor='loss', min_delta=0, patience=250, verbose=0, mode='auto', baseline=None, restore_best_weights=True)
model_pieces.compile(
    loss=keras.losses.mse,
    optimizer=keras.optimizers.Adam(),
    metrics=None,
)
model_pieces.fit(trans_data[:len(pieces)],pieces[:len(pieces)],batch_size=64, epochs=100,callbacks = [earlystop])
clear_output()board_inputs = keras.Input(shape=(8, 8, 12))conv1= layers.Conv2D(10, 3, activation='relu')
conv2 = layers.Conv2D(10, 3, activation='relu')
pooling1 = layers.MaxPooling2D(pool_size=(2, 2), strides=None, padding="valid", data_format=None,)
pooling2 = layers.MaxPooling2D(pool_size=(2, 2), strides=None, padding="valid", data_format=None,)
flatten = keras.layers.Flatten(data_format=None)x = conv1(board_inputs)
x = pooling1(x)
x = conv2(x)
x = flatten(x)
alpha_output = layers.Dense(7,name = 'alpha')(x)model_alpha = keras.Model(inputs=board_inputs, outputs=alpha_output, name="chess_ai_v3")
earlystop = keras.callbacks.EarlyStopping(monitor='loss', min_delta=0, patience=250, verbose=0, mode='auto', baseline=None, restore_best_weights=True)
model_alpha.compile(
    loss=keras.losses.mse,
    optimizer=keras.optimizers.Adam(),
    metrics=None,
)
model_alpha.fit(trans_data[:len(alphas)],alphas[:len(alphas)],batch_size=64, epochs=100,callbacks = [earlystop])
clear_output()board_inputs = keras.Input(shape=(8, 8, 12))conv1= layers.Conv2D(10, 3, activation='relu')
conv2 = layers.Conv2D(10, 3, activation='relu')
pooling1 = layers.MaxPooling2D(pool_size=(2, 2), strides=None, padding="valid", data_format=None,)
pooling2 = layers.MaxPooling2D(pool_size=(2, 2), strides=None, padding="valid", data_format=None,)
flatten = keras.layers.Flatten(data_format=None)x = conv1(board_inputs)
x = pooling1(x)
x = conv2(x)
x = flatten(x)
numbers_output = layers.Dense(7,name = 'number')(x)model_number = keras.Model(inputs=board_inputs, outputs=numbers_output, name="chess_ai_v3")
earlystop = keras.callbacks.EarlyStopping(monitor='loss', min_delta=0, patience=250, verbose=0, mode='auto', baseline=None, restore_best_weights=True)
model_number.compile(
    loss=keras.losses.mse,
    optimizer=keras.optimizers.Adam(),
    metrics=None,
)model_number.fit(trans_data[:len(numbers)],numbers[:len(numbers)],batch_size=64, epochs=100,callbacks = [earlystop])
clear_output()
```

该神经网络是一个卷积神经网络，具有从数据中提取特征的最大池。这种神经网络结构对于要预测的三个变量中的每一个都是重叠的:棋子、alpha(列)和 number(行)。

有一些我在设计神经网络时无法避免的致命缺点:

*   神经网络可能会预测不合法的举动
*   神经网络的拓扑为预测每个特征创建了一个断开点

如果你认为你能解决它，请随意使用这段代码来改进我的程序！

## 第八步|做预测:

```
new_chess_dict = {}
new_alpha_dict = {}
new_number_dict = {}
for term in chess_dict:
    definition = tuple(chess_dict[term])
    new_chess_dict[definition] = term
    new_chess_dict[term] = definition

for term in alpha_dict:
    definition = tuple(alpha_dict[term])
    new_alpha_dict[definition] = term
    new_alpha_dict[term] = definition

for term in number_dict:
    definition = tuple(number_dict[term])
    new_number_dict[definition] = term
    new_number_dict[term] = definitiondata = np.reshape(trans_data[0],(1,8,8,12))
pred = model_pieces.predict(data)
def translate_pred(pred):
    translation = np.zeros(pred.shape)
    index = pred[0].tolist().index(max(pred[0]))
    translation[0][index] = 1
    return translation[0]
piece = translate_pred(model_pieces.predict(data))
alpha = translate_pred(model_alpha.predict(data))
number = translate_pred(model_alpha.predict(data))
piece_pred = new_chess_dict[tuple(piece)]
alpha_pred = new_alpha_dict[tuple(alpha)]
number_pred = new_number_dict[tuple(number)]
move =str(piece_pred)+str(alpha_pred)+str(number_pred)
```

为了解码来自各个神经网络的预测，必须创建反向字典:这意味着采用一次性编码并将其翻译成字符串。这在通过颠倒术语和定义而创建的 new_chess_dict、new_alpha_dict 和 new_number_dict 字典中有详细描述。

有了这最后一点代码，程序就完成了！

# 结论:

虽然神经网络在进行预测方面起作用，但它经常预测非法移动，因为移动范围对于非法移动范围是连续的。我不能为此创造一个新的解决方案，但我想到了一个新的方法来实现一个具有不同算法的象棋人工智能:遗传算法！敬请关注！

# 我的链接:

如果你想看更多我的内容，点击这个 [**链接**](https://linktr.ee/victorsi) 。