# 使用 Google Colab 自动准备数据:读取和处理 Zip 文件中的 Citi Bike 数据

> 原文：<https://towardsdatascience.com/automate-data-preparation-using-google-colab-read-and-process-citi-bike-data-in-zip-file-2ab34dd33205?source=collection_archive---------49----------------------->

![](img/535ca6983c73c870d52a1819b45024d4.png)

安东尼·福明在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

我不时会收到同事的请求，要求我处理一些大型数据文件，并从数据中报告一些统计数据。由于他们依靠 **Excel** 作为主要的数据处理/分析工具，而不使用 **Python** 、 **R** 或 **SQL** ，读取和处理超过 1048576 行的数据文件(Excel**的行容量**)成为一个问题。

为了处理 zip 文件中的数据，通常我会下载 zip 文件，解压缩得到 CSV 文件，然后将 CSV 文件导入 SQL、R 或 Python 进行处理。得到结果后，我需要将结果输出到 CSV 文件中，并将文件发送给请求这项工作的同事。当我每个月都有新数据进来时，我必须重复这个过程，这整个过程可能会变得很麻烦。

# **谷歌 Colab 让自动化成为可能**

**Google Colab** (即 Google Colaboratory)提供了一个共享 Python 代码并在 web 浏览器中运行的平台。与非 Python 用户分享您的 Python 解决方案尤其有用。

这里有一个 Python 程序的例子，它使用 **Google Colab** 来自动准备每月**花旗自行车**的数据。该程序可以直接从 Citi Bike online 门户网站读取 zip 文件，并处理数据以报告每个日历日的 Citi Bike 行程总数。用户只需要提供他们想要处理的数据文件的 URL，并最终将结果下载/保存到他们的本地计算机。

# 花旗自行车数据

Citi Bike 网站每月都会发布用户使用数据，并维护回溯至 2013 年 6 月的历史数据。这是分析纽约大都市区微观流动趋势的良好数据源。

花旗自行车数据文件中的记录数量可能非常大。例如，2020 年 8 月的数据包含超过 200 万条记录，超过了 **Excel** 的行容量。

# **用于处理花旗自行车数据的 Python 程序**

首先进入**花旗自行车旅行数据门户**并复制您想要处理的文件的**链接地址**:

[https://s3.amazonaws.com/tripdata/index.html](https://s3.amazonaws.com/tripdata/index.html)

要运行代码，你需要**登录你的谷歌账户**。一旦登录，你可以通过点击代码窗口左上角的箭头**来运行 Python 代码。**

将数据文件链接地址粘贴到**输入框“数据 URL:”**，点击“回车”。代码将从文件地址下载 zip 文件，并逐行读入数据，并将数据保存到一个 **DataFrame** 中。

```
data_address = input('Data url: ')
url = urllib.request.urlopen(data_address)
data = []
df = pd.DataFrame()
**with** ZipFile(BytesIO(url.read())) **as** my_zip_file:
    **for** contained_file **in** my_zip_file.namelist():
        **for** line **in** my_zip_file.open(contained_file).readlines():
            s=str(line,'utf-8')
            s = re.sub(r"\n", "", s)
            s = re.sub(r"**\"**", "", s)
            line_s = s.split(",")
            data.append(line_s)

df = pd.DataFrame(data)
```

一旦我们将数据存储到一个数据框架中，事情就变得简单有趣了。原始数据中的第一行包含变量名(或列名)。使用第一行中的数据作为数据帧的列名，然后从数据帧中删除第一行。然后根据每次旅行的“开始时间”合计数据帧，以计算每个日历日的旅行次数，并将结果打印到输出控制台。

```
col_name = df.iloc[0].astype(str)
df.columns = col_name
df = df.drop([0])
df['startdate']=df['starttime'].astype('datetime64[ns]').dt.date
date_count = df.groupby('startdate').count()[['bikeid']]
date_count.columns = ['count']
print(date_count)
```

一旦结果输出到控制台，**通过在控制台底部的**输入框“输出文件名:”**中指定输出文件名，将结果**下载到您的本地计算机。

```
output_file_name = input('Output File Name: ')
date_count.to_csv(output_file_name+'.csv') 
files.download(output_file_name+'.csv')
```

完整的代码如下:

```
**from** **io** **import** BytesIO
**from** **zipfile** **import** ZipFile
**import** **pandas** **as** **pd**
**import** **urllib.request**
**import** **re**
**from** **google.colab** **import** files

data_address = input('Data url: ')
url = urllib.request.urlopen(data_address)
data = []
df = pd.DataFrame()
**with** ZipFile(BytesIO(url.read())) **as** my_zip_file:
    **for** contained_file **in** my_zip_file.namelist():
        **for** line **in** my_zip_file.open(contained_file).readlines():
            s=str(line,'utf-8')
            s = re.sub(r"\n", "", s)
            s = re.sub(r"**\"**", "", s)
            line_s = s.split(",")
            data.append(line_s)

df = pd.DataFrame(data)
col_name = df.iloc[0].astype(str)
df.columns = col_name
df = df.drop([0])
df['startdate']=df['starttime'].astype('datetime64[ns]').dt.date
date_count = df.groupby('startdate').count()[['bikeid']]
date_count.columns = ['count']
print(date_count)

output_file_name = input('Output File Name: ')
date_count.to_csv(output_file_name+'.csv') 
files.download(output_file_name+'.csv')
```

[](https://huajing-shi.medium.com/membership) [## 用我的推荐链接-华景石加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

huajing-shi.medium.com](https://huajing-shi.medium.com/membership) 

链接到 Google Colab 笔记本:

[https://colab . research . Google . com/drive/1 sptto E6 sl 9 on b5-lhappowgcipe 1 mihw # scroll to = krmffxaw 30 RC](https://colab.research.google.com/drive/1SpTToe6sL9OnB5-LhAPpOwGCIPe1miHw#scrollTo=kRMFFxaW30Rc)

GitHub 链接:

[https://github . com/Shi 093/Citi bike-Monthly-Report/blob/master/Citi bike _ daily count . ipynb](https://github.com/shi093/CitiBike-Monthly-Report/blob/master/citiBike_dailyCount.ipynb)