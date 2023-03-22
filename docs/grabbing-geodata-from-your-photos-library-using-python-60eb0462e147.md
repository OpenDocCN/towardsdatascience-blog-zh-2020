# 使用 Python 从照片库中获取地理数据

> 原文：<https://towardsdatascience.com/grabbing-geodata-from-your-photos-library-using-python-60eb0462e147?source=collection_archive---------34----------------------->

![](img/1941a4bea0474b03a20ccd5fa5456dde.png)

凯蒂·莫尔德·www.katymould.com

## 使用 python 自动整理你的照片收藏

所以我女朋友的众多天赋之一就是摄影。然而，她有超过 100，000 张照片，需要按照年份和城市进行分类。她主要从事基于位置的摄影，并发现当她把世界上的某些地方都放在一个地方时，写关于这些地方的文章更容易。

这是一项艰巨的任务，也是她在职业生涯中推迟的事情。然而，现在 COVID 已经让我们处于这样一个位置，这些通常会被推迟的必要任务现在她已经决定解决了。

所以我决定用我的 Python 知识来帮助她，这似乎很适合这个任务。

# 你将学到什么

1.  为了能够从 jpegs 中获取元数据
2.  为了能够使用谷歌地理编码 API 来反向地理编码，以获得基于经度和纬度的地址信息
3.  为了使用该元数据来获得城市和国家信息
4.  能够按照年份和国家对图片进行分类，以便进一步编辑

首先，我们将看看程序的结构和我们将用来创建脚本的包。

# 程序结构

1.  获取要排序的图像列表
2.  获取照片元数据(经度和纬度，照片拍摄日期)
3.  基于经度/纬度的抓取位置
4.  基于年份创建文件夹
5.  基于城市在每年内创建文件夹
6.  根据年份和城市将图像移动到文件夹中

# 脚本中使用的包

1.  GPSPhoto:用于从 jpeg 文件中获取元数据
2.  Selenium:用于访问基于经度和纬度的位置数据
3.  时间:用于减缓 selenium 获取数据的过程
4.  Os:用于用 python 创建文件夹
5.  Shutil:用于将文件移到新创建的文件夹中

现在我们已经有了如何进行的结构，让我们来讨论每一部分。

![](img/3a0898cb4c1ae01e13a3a2c837d8cce2.png)

凯蒂·莫尔德·www.katymould.com

# 制作图像列表

使用 python 的 os 模块很容易创建文件列表。

```
import os
image_list = os.listdir('c:\\users\\Aaron\\photo')
image_list = [a for a in image_list if a.endswith('jpg')]
```

注释
1。os 模块导入
2。定义了`image_list`变量。使用 os 模块的`listdir`方法，我们指定图像的位置。注意`\\`是用来允许我们创建一个文件夹位置的字符串。这叫做转义符。
3。我们创建一个列表，只包含以'结尾的文件。jpg。这里我们使用了一个 list comprehension，并使用 endswith string 方法只包含' jpg。

# 从照片元数据中获取位置信息

有很多软件包可以获得这些数据。我发现`GPSPhoto`是一种简单的方法，可以清晰地获得我所需要的东西。我们使用 getgpsdata 方法，该方法指定图像的文件路径，以获得经度和纬度以及照片拍摄时间的字典。

```
from GPSPhoto import gpsphoto
for a in image_list: 
     data = gpsphoto.getGPSData(os.getcwd() + f'\\{a}')
     print(data) 
```

输出:

```
{'Latitude': 55.85808611111111,
 'Longitude': -4.291505555555555,
 'Altitude': 10.32358870967742,
 'UTC-Time': '17:41:5.29',
 'Date': '08/23/2017'}
```

注释
1。我们遍历 image_list 并使用`getcwd()`方法获得当前的工作目录，并将它与一个文件映像连接起来。在这种情况下，我们使用变量 a。这是因为我们将循环遍历我们的图像列表，因此需要一种通用的方法来获取每个图像的元数据，而不管文件夹中有多少图像。详见下文！

2.注意，我们使用 f 字符串来做这件事，我们可以循环我们的图像列表，并获取每个图像的数据。`{}`中的任何内容都作为字符串传递，所以在这种情况下，它将是我们刚刚创建的列表中的每个图像文件。

3.我们已经打印出了整理文件夹所需的元数据。

# 转换经度和纬度数据

在上面的部分中，我们获取了经度和纬度数据。我们需要转换这个来获得一个位置。为此，我们可以使用 google 提供的地理编码 API。我们每个月可以免费使用 200 美元，每 1000 次请求可以获得 5 美元。这是一个不错的方法来做一些图片。

请按照说明访问地理编码 API [这里](https://developers.google.com/maps/documentation/geocoding/get-api-key)。请记住，在访问地理编码 API 之前，您需要设置一个计费帐户。别担心，钱不会因为你给了他们你的详细资料而被拿走！但是如果你有很多请求，请小心！

为了获得我们需要的信息，我们必须使用下面指定经度和纬度的 url 发出请求。请记住插入您按照上面的指南获得的 API 密钥！

```
import requests
long = data['Longitude']
lat = data['Latitude']response = requests.get(f'[https://maps.googleapis.com/maps/api/geocode/json?latlng={lat},{long}&key=INSERT API KEY'](https://maps.googleapis.com/maps/api/geocode/json?latlng=55.8580861,-4.291505&key=AIzaSyBvJdGu9atxkyfsE_rnLaZDJ-5sy4xj9E8'))print(response.json())
```

输出:

```
{'plus_code': {'compound_code': 'VP55+69 Glasgow, UK',
  'global_code': '9C7QVP55+69'},
 'results': [{'address_components': [{'long_name': '21',
     'short_name': '21',
     'types': ['street_number']},
    {'long_name': 'Pacific Quay',
     'short_name': 'Pacific Quay',
     'types': ['route']},
    {'long_name': 'Glasgow',
     'short_name': 'Glasgow',
     'types': ['postal_town']},
```

当我们发出一个 API 请求时，我们得到这个 json 文件！现在 json 的行为很像一个字典，因此我们可以像 wise 一样访问 json 的一部分。

笔记。
1。我们对 API url 使用 request get 方法。然后我们使用`json()`方法将它转换成我们想要的形式。

```
json_response = response.json()city = json_response['results'][0]['address_components'][2]['long_name']
```

注释
1。我们指定第一个结果键`[results][0]`和其中的`[address_components]`键。我们使用`[address_components]`中的第二个项目，最后我们需要键`long_name`的值

最好是一步一步地迭代，以获得您想要的数据。

# 为每年拍摄的图像创建文件夹

此时，我们有了从单个图像获取元数据的方法，并且有了图像列表。但是我们需要能够根据照片拍摄的年份创建文件夹。我们使用带有一些字符串操作的 os 模块来实现这一点。

```
year = data['Date'].split('/')[-1]
year_path = os.getcwd() + '\\' + str(year)
print(year_path)

if os.path.isdir(f'{year}'):
    return year_path
else:
    os.mkdir(data['Date'].split('/')[-1])
    return year_path
```

注释
1。`data[Date]`是图像拍摄日期的字典值。我们使用拆分列表方法将这些数据拆分为日、月和年的列表。年份总是最后一个列表，因此我们`[-1]`给出了我们需要的年份。我们使用`getcwd`方法获取当前工作目录。

2.我们使用 isdir 方法来确认有一个用于计算年份的文件夹。然后我们返回值 year_path。这段代码将被转换成一个函数，从而返回数据。

3.如果还没有创建 year 文件夹，我们使用`mkdir`并指定文件夹名称，在这种情况下，这是我们刚刚计算的年份。

# 为每个特定城市创建每年的文件夹

我们现在有了来自图像元数据的城市，我们可以创建各自的文件夹。但我们希望创建城市文件夹作为一个子目录的一年。

```
location_path = year_path + '\\' + city
if os.path.isdir(f'{location_path}'):
    print('City Folder already created')
else:
    os.mkdir(f'{year_path}' + '\\' + f'{city}')
    print(f'City: {city} folder created ')
```

注释
1。在这里，我们检查每年的位置文件夹。

2.如果还没有创建位置文件夹，我们像以前一样使用`mkdir`方法，但是这次我们使用上面创建的`year_path`变量，并将它与我们刚刚创建的城市变量连接起来。就这么简单！

# 将图像移动到正确的文件夹

现在我们创建了一个图像文件名的列表，我们说过我们会循环遍历它们，当我们想要将文件移动到一个特定的位置时，这变得很重要。我们使用 shutil 包来移动文件。move 方法有两个参数，一个是文件的绝对路径，另一个是文件所在位置的绝对路径。

```
shutil.move(os.getcwd() + f'\\{a}' ,location_folder + f'\\{a}')
```

注释
1。我们指定变量 a，它将是我们的图像文件名，我们连接它来创建图像的绝对路径。
2。我们使用在上一节中创建的`location_folder`,并将它与图像文件名连接起来，给出我们希望文件所在位置的绝对路径。

# 准备脚本

现在代码的所有部分都写好了。我们还没有解决这样一个事实，即我们将围绕一个图像文件名列表循环。在我的例子中，我女朋友的一些照片没有元数据，因此在这种情况下也有必要编码，简单地继续删减，直到看到有元数据的图像。

```
def address(data):
    long = data['Longitude']
    lat = data['Latitude']
    response = requests.get(f'[https://maps.googleapis.com/maps/api/geocode/json?latlng={lat},{long}&key=AIzaSyA6WyqENNQ0wbkNblLHgWQMCphiFmjmgHc'](https://maps.googleapis.com/maps/api/geocode/json?latlng={lat},{long}&key=AIzaSyA6WyqENNQ0wbkNblLHgWQMCphiFmjmgHc'))
    json_response = response.json()
    city = json_response['results'][0]['address_components'][2]['long_name']
    return citydef year_folder(data): year_path = os.getcwd() + '\\' + str(data['Date'].split('/')[-1])
    print(year_path)
    year = data['Date'].split('/')[-1]
    if os.path.isdir(f'{year}'):
        return year_path
    else:
        os.mkdir(data['Date'].split('/')[-1])
        return year_pathdef location_folder(year_path,city): location_path = year_path + '\\' + city
    if os.path.isdir(f'{location_path}'):
        print('City Folder already created')
    else:
        os.mkdir(f'{year_path}' + '\\' + f'{city}')
        print(f'City: {city} folder created ')def move_images(year_path, city,a):
    location_folder = year_path + f'\\{city}'
    shutil.move(os.getcwd() + f'\\{a}',location_folder + f'\\{a}')
```

注释
1。从我们上面讨论的内容来看,`address`函数没有太大的变化。
2。`year_folder`函数检查是否已经创建了年文件夹。如果是这样，我们不需要创建一个。语句的 else 部分确保如果 year 文件夹不存在，我们创建一个。
3。`location_folder`函数我们为位置文件夹创建绝对路径，并测试它是否已经存在。如果没有，我们就创建它。
4。`move_images`功能我们将文件从原始文件夹移动到特定年份和城市文件夹中的相应位置。

```
if __name__ == "__main__":image_list = os.listdir('c:\\users\\Aaron\\photo')
image_list = [a for a in image_list if a.endswith('jpg')]
 print(image_list)for a in image_list:
        data = gpsphoto.getGPSData(os.getcwd() + f'\\{a}')
        print(data)
        try:
            if data['Latitude']:
                city = address(data)
        except:
            print('Image has no Address')
            continue
        if data['Date']:
            year_path = year_folder(data)
        else:
            print('Image has no Date Attached')
            continue
        location_folder(year_path,city)
        move_images(year_path,city,a)
        print('Image moved succesfully')
```

注释
1。`image_list`变量使用`listdir`方法创建图像目录中所有文件名的列表。
2。我们循环每个图像文件名。我们获取每个文件的元数据，并对其进行两次检查。如果它有位置元数据，我们就获取它，如果它有照片拍摄时间的元数据，我们就获取它。
3。随后，如果我们有照片拍摄的地点和日期，我们就创建年份文件夹和地点文件夹。
4。然后，我们将图像从文件原来所在的文件夹移动到我们创建的文件夹中。

使用几个软件包，我们已经成功地创建了一个脚本，可以将成千上万的照片分类到所需的文件夹中，以便进一步编辑和分类照片！完整代码请见[此处](https://gist.github.com/medic-code/367b11117a1910a838d3800cc850bbd1)。

请在此查看更多关于我在博客和其他帖子上的项目进展的细节。如需更多技术/编码相关内容，请在此注册我的简讯

我将非常感谢任何评论，或者如果你想与 python 合作或需要帮助，请联系我。如果你想和我联系，请在这里或者在推特上联系我。

# 其他文章

[](https://medium.com/swlh/5-python-tricks-you-should-know-d4a8b32e04db) [## 你应该知道的 5 个 Python 技巧

### 如何轻松增强 python 的基础知识

medium.com](https://medium.com/swlh/5-python-tricks-you-should-know-d4a8b32e04db) [](/how-to-download-files-using-python-ffbca63beb5c) [## 如何使用 Python 下载文件

### 了解如何使用 Python 下载 web 抓取项目中的文件

towardsdatascience.com](/how-to-download-files-using-python-ffbca63beb5c)