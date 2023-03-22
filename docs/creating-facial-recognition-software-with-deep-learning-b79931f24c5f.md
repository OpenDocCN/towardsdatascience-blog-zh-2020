# 使用机器学习创建面部识别软件

> 原文：<https://towardsdatascience.com/creating-facial-recognition-software-with-deep-learning-b79931f24c5f?source=collection_archive---------56----------------------->

## *使用 Keras 和 OpenCV*

![](img/b2e1872b610e1489863213926063a78e.png)

照片由 [pixabay](https://www.pexels.com/@pixabay) 在[像素](https://www.pexels.com/photo/abstract-art-blur-bright-373543/)上拍摄

虽然用机器学习来进行面部识别的想法很早就被人知道了，但是自己创作这个软件还是挺有用的。

# 概念:

本文的目标是创建一个二元分类面部识别网络，允许一个人访问，而不允许另一个人访问。

我将使用 open-cv 访问计算机中的内置摄像头，并使用 haar-cascade 分类器来检测图像中的人脸。收集数据后，我会使用卷积神经网络对数据进行模型训练。

之后，当 OpenCV 检测到被授予访问权限的人时，它会在他/她的脸部周围画一个正方形，正方形上有“访问已验证”的字样。

# 代码:

```
from numpy import unique
from numpy import argmax
import os
import cv2
from PIL import Image
import numpy as np
from tensorflow.keras import Sequential
from tensorflow.keras import optimizers
from tensorflow.keras.layers import Dense
from tensorflow.keras.layers import Conv2D
from tensorflow.keras.layers import MaxPooling2D
from tensorflow.keras.layers import Flatten
from tensorflow.keras.layers import Dropout
from tensorflow.keras.layers import BatchNormalization
from tensorflow.keras.layers import Dropout
```

除了标准的 numpy 和 os 库用于数据访问和数据操作，我还使用 open-cv 和 PIL 进行图像处理。

```
def data_path():
    file = 'XXXXXXXX'
    os.chdir(file)
    files = os.listdir()
    files.remove('.DS_Store')
    return file,files
```

这是访问你计算机中数据的功能。要使此函数工作，请将变量文件更改为数据所在的相关目录。

```
def data_setup(files,file):
    pixels = [0]*len(files)
    answers = list()
    print(len(files))
    for i in range(len(files)):
        image = Image.open(files[i])
        pixels[i]= np.asarray(image)
        pixels[i] = pixels[i].astype('float32')
        pixels[i] /= 210.0
        if files[i][0] == 'm':
            answers.append(1)
        elif files[i][0] == 'n':
            answers.append(0)
    dataset = np.array(pixels)
    for i in range(len(dataset)):
        dataset[i] = dataset[i].reshape(320,320)
    return np.asarray(dataset),np.asarray(answers)
```

该函数访问数据，并将所有照片重新整形为 320 x 320 的图片。然后，它返回 X 和 y 值，即数据和真值。

```
def train(data,answers):
    x_train = data
    y_train = answers
    x_train = np.array(x_train)
    x_train = x_train.reshape((x_train.shape[0], x_train.shape[1], x_train.shape[2], 1))
    print(x_train.shape)
    in_shape = x_train.shape[1:]
    print(len(data),len(answers))
    model = Sequential()
    model.add(Conv2D(10, (3,3), activation='relu', kernel_initializer='he_uniform', input_shape=in_shape))
    model.add(MaxPooling2D((2, 2)))
    model.add(Conv2D(10, (3,3), activation='relu', kernel_initializer='he_uniform', input_shape=in_shape))
    model.add(MaxPooling2D((2, 2)))
    model.add(Flatten())
    model.add(Dense(1,activation ='sigmoid'))
    model.compile(optimizer='adam', loss='binary_crossentropy',metrics = ['accuracy'])
    model.fit(x_train, y_train, epochs=100, batch_size=100, verbose = 2, validation_split = 0.33)
    return model
```

这个脚本创建、编译和训练卷积网络。使用的损失是二进制交叉熵，度量是准确度，因为我们希望网络以高准确度预测正确的人脸。

```
def face_recognition(model):
    dirx = 'XXXXXXXXXXXXXXXXXXXXXXXXXX'
    os.chdir(dirx)
    face_cascade = cv2.CascadeClassifier('cascades/data/haarcascade_frontalface_alt.xml')
    cap = cv2.VideoCapture(0)
    while True:
        ret,frame = cap.read()
        gray = cv2.cvtColor(frame,cv2.COLOR_BGR2GRAY)
        faces = face_cascade.detectMultiScale(gray,scaleFactor = 1.05, minNeighbors = 5)
        for (x,y,w,h) in faces:
            print('Face Detected')
            roi_gray = gray[y:y+h,x:x+w]
            roi_color = frame[y:y+h,x:x+w]
            roi_gray = roi_gray.astype('float32')
            roi_gray /= 210.0
            classify = cv2.resize(roi_gray,(320,320))
            if classify.shape == (320,320):
                classify = classify.reshape((1, classify.shape[0], classify.shape[1], 1))
                color = (255,0,0)
                stroke = 2
                end_cord_x = x+w
                end_cord_y = y+h
                pred = model.predict(classify)
                print(pred)
                if pred == 1:
                    cv2.rectangle(frame,(x,y),(end_cord_x,end_cord_y),color,stroke)
                    cv2.putText(frame, 'Access', (x, y-10), cv2.FONT_HERSHEY_SIMPLEX, 0.9, (36,255,12), 2)
                elif pred == 0:
                    cv2.rectangle(frame,(x,y),(end_cord_x,end_cord_y),color,stroke)
                    cv2.putText(frame, 'Denied Access', (x, y-10), cv2.FONT_HERSHEY_SIMPLEX, 0.9, (36,255,12), 2)
        if cv2.waitKey(20) & 0xFF == ord('q'):
            break
        cv2.imshow('frame',frame)
```

该脚本应用模型并实时进行预测，创建并标记有面的盒子。要使这个函数工作，您必须再次确定 haar-cascade 在哪个目录中。您可能需要将文件夹从外部位置移到正确的目录。

```
file,files=data_path()
data,answers = data_setup(files,file)
model = train(data,answers)
face_recognition(model)
```

程序的最后一部分操作所有功能，并启动实时面部识别。

# 如何改进我的计划:

当我写程序时，我总是分享一个强大的框架，在那里可以添加更复杂的功能。以下是一些可以增加程序功能的方法:

*   多类分类

当给定每个人的一组平衡的照片时，尝试训练模型来预测这个人是谁，而不是二进制分类。

*   添加更多的哈尔级联分类器

我使用的 haar-cascade 是正面人脸 cascade，只能检测正面人脸照片。你可以添加更多的分类器，这将使它工作，不管相机的角度。

# 我的链接:

如果你想看更多我的内容，点击这个 [**链接**](https://linktr.ee/victorsi) 。