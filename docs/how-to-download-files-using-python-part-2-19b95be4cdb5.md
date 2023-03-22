# 超越 python 中请求包的基础

> 原文：<https://towardsdatascience.com/how-to-download-files-using-python-part-2-19b95be4cdb5?source=collection_archive---------6----------------------->

![](img/911cc4cb9347b03ff50ffb4555ab2d0a.png)

émile PerronUnsplash 图像

## 了解如何在 python 中使用进度条、恢复部分下载的文件和验证文件

当您习惯了 requests python 包后，在命令行应用程序中考虑验证文件、恢复未完成的 get 请求和使用进度条的方法会很有用。这些超出了请求包的基本用途。我们将通过简单的方法使用请求包来实现这一点。

# 在本文中，您将了解到

1.  如何恢复下载不完整的二进制文件
2.  如何创建一个简单的下载验证程序来传输文件/备份数据。
3.  如何显示简单的命令行进度条？

# 恢复下载

下载大文件时，可能会因为各种原因而中断。我们有时需要一种能够在最后一个字节恢复的方法来重新建立连接。

作为来自服务器的 HTTP get 请求的一部分，我们将获得数据头和数据体。二进制文件的 HTTP 头返回了我们请求的文件的大量信息！根据请求被发送到的服务器，我们有时会得到的部分之一是`accept-ranges`头。这允许客户端下载部分下载的数据。

下面是一个接受下载部分文件的二进制文件头的例子。

```
{'Server': 'VK', 
'Date': 'Wed, 29 Jan 2020 11:47:20 GMT', 
'Content-Type': 'application/pdf', 
'**Content-Length**': '9713036', 
'Connection': 'keep-alive', 
'Last-Modified': 'Mon, 20 Jan 2020 13:01:17 GMT', 
'ETag': '"5e25a49d-94358c"', 
**'Accept-Ranges': 'bytes',** 
'Expires': 'Wed, 05 Feb 2020 11:47:20 GMT', 
'Cache-Control': 'max-age=604800', 
'X-Frontend': 'front632904', 
'Access-Control-Expose-Headers': 'X-Frontend',
 'Access-Control-Allow-Methods': 'GET, HEAD, OPTIONS',
 'Access-Control-Allow-Origin': '*', 
'Strict-Transport-Security': 'max-age=15768000'}
```

有了这种能力，我们还可以向服务器指定我们请求下载的文件的位置。然后，我们可以在该特定位置开始请求二进制数据，并从那里继续下载。

现在，要下载二进制文件的一小部分，我们必须能够用 request get HTTP 方法发送文件头。请求包使我们能够轻松地做到这一点。我们只想获得一定量的字节，我们通过发送一个`range`头来指定要接收多少字节。然后，我们可以将它放入一个变量，并通过 get 请求传递它。

```
resume_headers = {'Range':'bytes=0-2000000'}
r = request.get(url, stream=True, headers=resume_header)
with open('filename.zip','wb') as f:
  for chunk in r.iter_content(chunk-size=1024)
      f.write(chunk)
```

笔记

1.我们在请求获取方法中指定了`stream = True`。这允许我们控制何时下载二进制响应的主体。

2.我们在`requests.get()`方法中使用 headers 参数来定义从 0 到 2000000 的字节位置。范围标题的边界是包含的。这意味着将下载字节位置 0 到 2000000。

4.我们使用一个 with 语句来写文件 filename . zip。`r.iter_content` 方法允许我们通过以字节为单位定义`chunk-size`来指定要下载的数据的大小。在这种情况下，它被设置为 1024 字节。

我们下载了部分文件。这对我们恢复部分下载文件的实际目标有什么帮助？

当我们想要恢复部分下载的文件时，我们在文件头中指定文件大小。这是需要下载的下一个字节。这就是 python 中能够恢复下载的症结所在。

我们需要知道部分下载的文件大小。python 中有一系列的包可以做到这一点，pathlib 是这个用例的一个很好的包。请参见[这里的](https://docs.python.org/3/library/pathlib.html)获取使用 pathlib 的指导。

我们首先要导入 pathlib 包

```
import pathlib
```

方法`path.stat()`返回关于路径的信息(类似于 os.stat，如果你熟悉 os 包的话)。现在我们调用`path.stat()`方法的`st_size`属性来获取文件的大小(也类似于 OS 包)。

现在我们准备将它投入使用

```
resume_header = {'Range':f'bytes= {path('filename.zip').stat().st_size}**-**'}
```

现在，这个需要打开包装。字符串前的 f 是一个 f 字符串，这是一个格式化字符串的好方法。作为一个用例，这里花括号中的`{path.stat().st_size}`是一个表达式。我们正在创建的字符串被花括号中表达式的含义所修改。这可能是一个变量，但在这种情况下，我们得到的是我们下载的部分文件的大小。f 字符串解释{}中的任何内容，然后将结果显示为字符串。在这种情况下，它为我们打印文件大小。

字符串中`{path.stat().st_size}`后面的粗体连字符表示我们从部分文件大小的字节开始抓取数据，直到文件的末尾。

既然我们理解了这一点，我们就可以将所有这些放在下面的代码中

```
resume_header = {'Range':f'bytes={path('filename.zip').stat().st_size}-'}
r = requests.get(url,stream=True, headers=resume_header)with open ('filename.zip','ab') as f:
   for chunk in r.iter_content(chunk-size=1024):
     f.write(chunk)
```

笔记

1.  “打开”功能中的“ab”模式将新内容添加到文件中。我们不想覆盖现有的内容，这是代替' wb '

# 验证文件

我们有时需要能够验证下载的文件。如果您恢复了文件下载，或者如果这是您想要与其他人共享的重要研究数据。有一个 python 模块叫做 hashlib 模块，它创建一个散列函数。哈希函数获取数据，并将其转换为唯一的数字和字母字符串。我们称之为哈希对象。这个字符串的计算是通过我们可以指定为模块的一部分的算法来完成的。

让我们开始讨论如何验证下载的文件。我们需要知道哈希值应该是什么才能验证它。我们将读取二进制文件并生成散列，然后可以将它与文件的已知散列值进行比较。由于散列文件是唯一的，我们将能够确认它们是相同的文件。

要创建哈希值，我们需要指定创建它的算法。有很多选择，但在这个例子中，我们使用`sha256()`。现在，为了创建一个哈希值，我们使用 hashlib `update()`方法，该方法只接受“类似字节”的数据，比如字节。为了访问哈希值，我们调用了`hexdigest()`方法。`hexdigest()`接受 hash 对象，并为我们提供一串仅十六进制的数字。这个字符串由我们之前指定的算法定义。

一旦创建了哈希值，就不能反向获得原始的二进制数据。它只有一种方式。我们可以通过唯一的哈希值来比较两个文件，并使其在传输给其他人时更加安全。

```
import hashlibwith open('research.zip', 'rb') **as** f:
    content = f.read()
sha = hashlib.sha256()
sha.update(content)
print(sha.hexdigest())
```

输出:

```
42e53ea0f2fdc03e035eb2e967998da5cb8e2b1235dd2630ffc59e31af866372
```

笔记

1.  hashlib 模块被导入
2.  我们使用 with 语句读取文件:这确保了我们不必使用 close 语句。
3.  我们调用`read()`方法来读取二进制数据的所有内容
4.  创建变量 sha，并使用 SHA 256 算法创建散列对象来创建散列值。
5.  使用`update()`方法，我们将二进制数据传递给散列对象。通过这样做，我们得到一个哈希值。
6.  使用`hexdigest()`方法，我们可以打印出哈希值，即二进制文件特有的固定字符串。

所以现在无论何时你想验证一个文件，我们都有一个哈希值。如果你不得不再次下载或者把数据传输给同事。你只需要比较你创建的哈希值。如果恢复的下载完成并且具有正确的哈希值，那么我们知道它是相同的数据。

让我们创建一个小脚本来确认您的朋友传输给您的一个文件。例如，您需要输入哈希值。

```
user_hash = input('Please input hash please: ')sha = hashlib.sha256()with open('file.zip' as 'rb') as f:
   chunk = f.read()
   if not chunk:
       break
   sha.update(chunk)
try:
    assert sha.hexdigest() == user_hashexcept AssertionError:
    print('File is corrupt, delete it and restart program'else: 
   print('File is validated')
```

笔记

1.  我们要求用户输入一个散列值，该值被赋予变量 user-hash。
2.  当我们指定算法时，会创建变量 sha 和散列对象
3.  我们使用 with 语句打开想要验证的文件。我们定义变量 chunk，并使用 read 方法为它分配二进制数据。

4.我们使用 hashlib `update()`方法为这个块创建一个散列对象。

5.我们使用`sha.hexdigest()`为这个块创建一个哈希值。

6.我们使用 assert 关键字，它计算表达式的真值。在这种情况下，根据输入的哈希值评估下载数据的哈希值。

7.我们指定一个异常`AssertionError`。当 assert 语句为 false 并指定错误消息时，将调用此函数。

8.在 else 语句中，如果`user_hash`变量与文件的哈希值相同，我们打印出该文件已经过验证。

所以这里我们为下载的文件创建了一个非常简单的验证工具。

# 进度条

有许多软件包可以显示您的编程代码的进度。这里我们就来说一个下载文件时添加进度条的简单方法！如果您正在批量下载，并希望在下载过程中看到进度，这可能会很有用。进度条在很多方面都很有用，不仅仅是下载文件。

Tqdm 是一个第三方 python 包，可以处理进度条。对我来说，这是思考 python 的最好方式，从尽可能少的代码开始，得到你想要的。

首先，您需要使用 pip 安装 tqdm。

```
pip install tqdm
```

然后我们会想要导入 tqdm。现在是我们要用 tqdm 方法来显示数据的进度。tqdm 模块可以解释每个块并显示文件的进度。

将进度条合并到下载文件中。首先，我们必须创建一些变量，最重要的是文件的大小。我们可以使用请求包来做到这一点。我们获取二进制头和“内容长度”。向上滚动到另一部分的二进制文件头。它的相关值是我们从服务器请求了多少字节的数据。响应是一个字符串，当在进度条中使用它时，我们必须将它转换成数字格式

另一个重要的部分是文件名。我们可以将 URL 拆分成一个列表，然后非常简单地选择最后一项。

一旦设置好所有变量，我们就可以指定 tqdm 模块。

with 语句意味着一旦操作完成并且 open 函数写入数据，它就关闭。然后，我们使用 tqdm 方法并指定参数来显示正在下载的数据。请注意，论点是非常详细的！让我们一个一个地看。

1.  `total`参数是我们定义的文件大小。
2.  `unit`参数是我们指定的字符串，用于定义数据块的每次迭代的单元。在这种情况下，我们指定 B 为字节。
3.  `desc`参数显示文件名
4.  `initial`参数指定了进度条从哪里开始，在本例中为 0。
5.  `ascii`参数是指定我们用什么来填充进度条。如果设置为 false，它将采用 unicode 来填充进度条。

既然我们已经解释了我们在做什么，现在让我们来看看代码:

```
from tqdm import tqdmurl = "insert here"
file = url.split('/')[-1]r = requests.get(url, stream=True, allow_redirects=True)
total_size = int(r.headers.get('content-length'))
initial_pos = 0with open(file,'wb') as f: 
    with tqdm(total=total_size, unit=B, 
               unit_scale=True,                      desc=file,initial=initial_pos, ascii=True) as pbar: for ch in r.iter_content(chunk_size=1024),                             
              if ch:
                  f.write(ch) 
                  pbar.update(len(ch))
```

输出:

```
filename.zip 100%|#################################################| 3.71M/3.71M [00:26<00:00, 254kB/s]
```

笔记

1.  我们从 tqdm 模块中导入 tqdm 方法。
2.  定义了变量 url。
3.  定义了文件变量，我们使用拆分字符串的方法将 url 拆分成一个列表。('/')参数告诉 split 方法在 url 的/'之间拆分字符串。这里，我们想要得到列表的最后一个索引，因为这将是我们想要的文件名。
4.  变量 r 用于指定一个 HTTP get 请求，我们允许它有一个开放的数据流并允许重定向。
5.  定义了`total_size`变量，并使用请求包获取二进制头。使用 get 方法，我们得到了二进制文件大小的`‘content-length’`值。现在，它返回一个字符串，我们用 int()把它变成一个数字。
6.  变量`initial_pos`被赋值为 0，这对于 tqdm 方法的指定很重要。
7.  我们使用 with 语句访问 tqdm 方法。我们在参数中指定了几项。
8.  `r.iter_content`将数据分割成块。我们将 ch 定义为一个 1024 字节的块，如果该块可用，我们将该块写入文件。
9.  我们调用 tqdm 方法的 update 属性将该块更新到进度条并显示出来。

所以在那之后，你应该对如何处理进度条、验证文件和超越请求包的基础有了更好的了解。

感谢阅读！

**关于作者**

我是一名医学博士，对教学、python、技术和医疗保健有浓厚的兴趣。我在英国，我教在线临床教育以及运行网站[www.coding-medics.com。](http://www.coding-medics.com./)

您可以通过 asmith53@ed.ac.uk 或 twitter [here](https://twitter.com/AaronSm46722627) 联系我，欢迎所有意见和建议！如果你想谈论任何项目或合作，这将是伟大的。

如需更多技术/编码相关内容，请在此注册我的简讯[。](https://aaronsmith.substack.com/p/coming-soon?r=6yuie&utm_campaign=post&utm_medium=web&utm_source=copy)