# 什么是 Google API 愿景？以及如何使用它

> 原文：<https://towardsdatascience.com/what-is-google-api-vision-and-how-to-use-it-372a83e6d02c?source=collection_archive---------32----------------------->

![](img/e83eee2b3a044f4d75cfa72395ac73f6.png)

来源:[曼努埃尔·盖辛格，](https://www.pexels.com/fr-fr/@artunchained)佩克斯

## *使用服务帐户通过 OCR 从图像中提取文本。*

## 介绍

这篇文章源于一个有趣的知识提取项目。第一步是提取 pdf 文档的文本。我工作的公司是基于 Google 平台的，所以很自然，我想使用 API Vision 的 OCR，但是找不到使用 API 提取文本的简单方法。所以这个帖子。

> 这个帖子的笔记本可以在 [GitHub](https://github.com/Christophe-pere/API_vision_google) 上找到

## 谷歌 API 愿景

谷歌发布了 [API](https://cloud.google.com/vision) 来帮助人们、行业和研究人员使用他们的功能。

> Google Cloud 的 Vision API 拥有强大的机器学习模型，通过 REST 和 RPC APIs 进行预训练。标记图像，并将它们快速组织到数百万个预定义的类别中。您将能够检测物体和人脸，阅读印刷或手写文本，并将有用的元数据集成到您的图像目录中。(来源: [API 视觉](https://cloud.google.com/vision))

我们对这篇文章感兴趣的 API 部分是 OCR 部分。

## 光学字符识别

[光学字符识别](https://cloud.google.com/vision/docs/ocr)或 OCR 是一种在图像内部识别和检测字符的技术。大多数时候，卷积神经网络(CNN)是在一个非常大的不同类型和颜色的字符和数字数据集上训练的。您可以想象在每个像素或像素组上有一个小窗口切片，以检测字符或部分字符、空格、表格、线条等。

## 服务帐户

> 服务帐户是一种特殊类型的 Google 帐户，旨在代表一个非人类用户，该用户需要进行身份验证并被授权访问 Google APIs 中的数据。([来源:IAM 谷歌云](https://cloud.google.com/iam/docs/understanding-service-accounts))

基本上你可以把它想象成一个 [RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem)) 密钥(通过互联网在机器之间进行高安全性通信的加密密钥)，用它你可以连接到谷歌服务(API、GCS、IAM……)。它的基本形式是一个 [json](https://www.json.org/json-en.html) 文件。

## 笔记本

在这里，我将向您展示使用 API 和从图像中自动提取文本的不同函数。

需要安装的库:

```
!pip install google-cloud
!pip install google-cloud-storage
!pip install google-cloud-pubsub
!pip install google-cloud-vision
!pip install pdf2image
!pip install google-api-python-client
!pip install google-auth
```

使用的库:

```
from pdf2image import convert_from_bytes
import glob
from tqdm import tqdm
import base64
import json
import os
from io import BytesIO
import numpy as np
import io
from PIL import Image
from google.cloud import pubsub_v1
from google.cloud import visionfrom google.oauth2 import service_account
import googleapiclient.discovery
*# to see a progress bar*
tqdm().pandas()
```

OCR 可以接受在 API 中使用的 pdf、tiff 和 jpeg 格式。在本帖中，我们将把 pdf 转换成 jpeg 格式，将许多页面连接成一张图片。使用 jpeg 的两种方式:

首先，您可以将您的 *pdf* 转换成 *jpeg* 文件，并将其保存到另一个存储库中:

```
*# Name files where the pdf are and where you want to save the results*
NAME_INPUT_FOLDER = "PDF FOLDER NAME"
NAME_OUTPUT_FOLDER= "RESULT TEXTS FOLDER"list_pdf = glob.glob(NAME_INPUT_FOLDER+"/*.pdf") *# stock the name of the pdf files* *# Loop over all the files*
for i in list_pdf:
        *# convert the pdf into jpeg*
        pages = convert_from_path(i, 500)

        for page in tqdm(enumerate(pages)):
            *# save each page into jpeg* 
            page[1].save(NAME_OUTPUT_FOLDER+"/"+i.split('/')[-1].split('.')[0]+'_'+str(page[0])+'.jpg', 'JPEG') *# keep the name of the document and add increment* 
```

在这里，您可以通过 API 使用您的 jpeg 文档。但是，你可以做得更好，不用保存 *jpeg* 文件，直接在内存中使用它来调用 API。

## 设置凭据

在深入之前，我们需要配置 Vision API 的凭证。你会发现，这很简单:

```
SCOPES = ['[https://www.googleapis.com/auth/cloud-vision'](https://www.googleapis.com/auth/cloud-vision')]
SERVICE_ACCOUNT_FILE = "PUT the PATH of YOUR SERVICE ACCOUNT JSON FILE HERE"*# Configure the google credentials*
credentials = service_account.Credentials.from_service_account_file(
        SERVICE_ACCOUNT_FILE, scopes=SCOPES)
```

## 图片处理

这需要更多的代码，因为我们还要连接 10 页文档来创建一个“大图”并将其提供给 API。一次调用比 10 次调用更划算，因为每次请求 API 时都要付费。

我们走吧:

操作和连接图片以将它们提供给 API 的函数

使用这两个函数，您将能够加载一个 pdf 文件，将其转换为字节，创建一个“大图片”并将其馈送到函数***detect _ text _ document()***(详细信息如下)。

函数***detect _ text _ document***用于输入图片内容和凭证(您的服务帐户信息)。

输出是从图像中提取的文本。这个函数的目标是将单词连接成段落和文档。

## 怎么用？

您可以像这样使用这个函数块:

```
for doc_pdf in tqdm(list_pdf): *# call the function which convert into jpeg, stack 10 images
        # and call the API, save the output into txt file* 
        concat_file_ocr(doc_pdf)
```

输入就是用 *glob* 函数获得的路径。该凭证是在 ***设置凭证*** 部分生成的。该循环将获取输入文件的每个 pdf，使用通过转换 pdf 获得的 jpeg 文件调用 API，并保存包含检测的文本文件。

## 结论

在这里，您将结束关于如何使用视觉 API 并自动生成包含检测的文本文件的教程。您知道如何使用您的服务帐户配置凭证，以及如何将 pdf 转换为 jpeg 文件(每页一个 jpeg)。是全部吗？不，我有一些奖金给你(见下文)。

## 好处 1:每页使用 API

前面的函数允许您使用 API 来连接页面。但是，我们可以在 pdf 文档的每一页使用 API。下面的函数将请求 API 将 pdf 转换成 *jpeg* 格式的每一页。

使用它非常简单，只需用 pdf 文件夹的路径和凭证调用这个函数。像这样:

```
if per_page: *# option True if you want to use per page*
    *# call the API vision per page of the pdf*
    for i in tqdm(list_pdf):
        *# open the pdf and convert it into a PlImage format jpeg*
        call_ocr_save_txt(i, cred=credentials)
```

## 好处 2:使用多重处理库

只是为了好玩，你可以把这个 API 和多重处理一起使用(好吧，这在 python 中不是真正的多重处理，带有全局解释器锁(GIL))。但是，这里的代码:

```
if multi_proc:
    nb_threads = mp.cpu_count() *# return the number of CPU*
    print(f"The number of available CPU is {nb_threads}")
        *# if you want to use the API without stacking the pages*
    if per_page:
        *# create threads corresponding to the number specified*
        pool = mp.Pool(processes=nb_threads)    
        *# map the function with part of the list for each thread*
        result = pool.map(call_ocr_save_txt, list_pdf) 

    if per_document:
        pool = mp.Pool(processes=nb_threads) 
        result = pool.map(concat_file_ocr, list_pdf)
```