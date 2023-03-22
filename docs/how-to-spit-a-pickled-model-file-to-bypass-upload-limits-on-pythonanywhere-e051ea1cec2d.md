# 如何分割一个 Pickled 模型文件以绕过 PythonAnywhere 上的上传限制

> 原文：<https://towardsdatascience.com/how-to-spit-a-pickled-model-file-to-bypass-upload-limits-on-pythonanywhere-e051ea1cec2d?source=collection_archive---------38----------------------->

## 在这篇短文中，我将分享如何使用 python 和 os 库分割和连接一个 pickled 模型文件，以绕过 PythonAnywhere 上的上传限制。

![](img/625cee305a8460cb8fe888d7142ee90e.png)

凯文·Ku 拍摄的图片

在我的上一篇文章“建立一个卷积神经网络来识别剃过脸和没剃过脸的人”中，我分享了我用 Pickle 保存最终训练好的模型的方法。

> “pickle”是将 Python 对象层次结构转换成字节流的过程，“unpickling”是相反的操作，将字节流(来自二进制文件或类似字节的对象)转换回对象层次结构。— **源代码:** [Lib/pickle.py](https://github.com/python/cpython/tree/3.8/Lib/pickle.py)

作为一个复习，这里有一行代码来处理你的最终模型，保存所有的权重，没有将结构保存为。json 文件，并作为. h5 文件单独加权。

```
import picklepickle.dump(model, open(“Your_Saved_Model.p”, ‘wb’))
```

因为我有一个模型，所以我的目标是构建一个 Flask 应用程序，并部署它供公众使用。我选择 [PythonAnywhere](https://www.pythonanywhere.com/?affiliate_id=0077ae73) 来托管[我的应用程序](http://jtadesse.pythonanywhere.com/)，因为我需要访问 Bash 控制台，还需要设置一个虚拟环境，但是我意识到文件上传是有限制的，我的模型比允许的大了三倍。

我做了一些研究，发现了一篇非常有帮助的[文章](https://stonesoupprogramming.com/2017/09/16/python-split-and-join-file/)，在理解如何用 Python 拆分和合并文件方面。使用`os`库和下面的函数，我能够将 pickled 模型文件分割成三个更小的文件:

```
def split(source, dest_folder, write_size):
    # Make a destination folder if it doesn't exist yet
    if not os.path.exists(dest_folder):
        os.mkdir(dest_folder)
    else:
        # Otherwise clean out all files in the destination folder
        for file in os.listdir(dest_folder):
            os.remove(os.path.join(dest_folder, file)) partnum = 0

    # Open the source file in binary mode
    input_file = open(source, 'rb') while True:
        # Read a portion of the input file
        chunk = input_file.read(write_size)

        # End the loop if we have hit EOF
        if not chunk:
            break

        # Increment partnum
        partnum += 1

        # Create a new file name
        filename = '/Model_Files/final_model' + str(partnum)

        # Create a destination file
        dest_file = open(filename, 'wb')

        # Write to this portion of the destination file
        dest_file.write(chunk) # Explicitly close 
        dest_file.close() # Explicitly close
    input_file.close() # Return the number of files created by the split
    return partnum
```

该函数需要 source、write_size 和 dest_folder。source 是保存整个模型的位置，write_size 是每个文件的字节数，dest_folder 是保存每个分割文件的目标文件夹。

这里有一个例子:

```
split(source='Your_Saved_Model.p', write_size=20000000, dest_folder='/Model_Files/')
```

因为我现在有三个可接受的上传文件，所以一旦应用程序在浏览器中启动，我需要更新我的 flask 应用程序来处理服务器上的文件连接。这里有一个到我的 [GitHub repo](https://github.com/cousinskeeta) 的链接，可以在部署到 [PythonAnywhere](https://www.pythonanywhere.com/?affiliate_id=0077ae73) 之前查看完整的代码。

我使用下面的函数来连接文件，并将合并后的模型保存在应用程序的服务器上:

```
def join(source_dir, dest_file, read_size):
    # Create a new destination file
    output_file = open(dest_file, 'wb')

    # Get a list of the file parts
    parts = ['final_model1','final_model2','final_model3']

    # Go through each portion one by one
    for file in parts:

        # Assemble the full path to the file
        path = file

        # Open the part
        input_file = open(path, 'rb')

        while True:
            # Read all bytes of the part
            bytes = input_file.read(read_size)

            # Break out of loop if we are at end of file
            if not bytes:
                break

            # Write the bytes to the output file
            output_file.write(bytes)

        # Close the input file
        input_file.close()

    # Close the output file
    output_file.close()join(source_dir='', dest_file="Combined_Model.p", read_size = 50000000)
```

这个函数将单个文件字节合并成一个单独的 pickled 对象。看看我如何使用 jQuery 让用户以一种交互的方式上传图像，并通过向后端发送请求来进行预测，使用新的组合模型来服务于预测。查看我的 [GitHub repo](https://github.com/cousinskeeta) 应用程序。

***资源*** :

[](https://stonesoupprogramming.com/2017/09/16/python-split-and-join-file/) [## Python 拆分和连接文件

### 《编程 Python:强大的面向对象编程》这本书有一个示例程序，展示了如何拆分和…

stonesoupprogramming.com](https://stonesoupprogramming.com/2017/09/16/python-split-and-join-file/)