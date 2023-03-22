# 用 Python 进行 AWS 转录的快速教程

> 原文：<https://towardsdatascience.com/a-quick-tutorial-to-aws-transcribe-with-python-53bbf6605a55?source=collection_archive---------10----------------------->

## 通过 Google Colab 和 Google Drive 使用 AWS 转录的转录服务简介

去年夏天，我开发了一些涉及语音转文本机制的产品，我认为最好使用现有的 API 来实现这些目的。在这篇文章中，我想与那些想尝试这些美妙技术的人分享我使用这些 API 的一点点经验。希望你们觉得有帮助:)

哦！而**这里是我将涉及的快速内容:**
**-设置:通用包和初始化基本功能
-单扬声器文件
-多扬声器文件
-通过 Colab 和 Google Drive 访问文件并上传到 S3 存储器
-创建词汇表以增强转录准确性**

代码链接:Google Colab ( [此处](https://colab.research.google.com/drive/1oaS1dOj5kkzx9Q8YRZd54AGHzrQEqg_9))、Gist ( [此处](https://gist.github.com/viethoangtranduong/28a365e6457f35e206779995f488318a))或 Github( [此处](https://github.com/viethoangtranduong/AWS-Transcribe-Tutorial/blob/master/AWS_Transcribe_Tutorial.ipynb))。

![](img/4021e56f94b36411b5d15a4d00de7ac7.png)

[来源](https://pixabay.com/illustrations/web-network-programming-3706562/)

## 为什么选择语音转文本？

语音转文本是一项很有前途的技术，不是作为一个产品本身，而是更多地基于它对许多其他产品的底层应用。由于我们的阅读速度比听力速度快得多，阅读转录比听类似内容的音频节省更多的时间。谷歌的新旗舰手机:Pixel 4，引入了执行实时转录的录音应用程序！有前途的产品可能是会议记录(Zoom 已经提供了这个功能)、讲座(文本记录)等等。

多年来，语音转文本已经是一个相当成熟的领域。由于这种挑战更多的是横向的，而不是纵向的，因此拥有来自各种输入源的大量数据的公司会胜出。毫无疑问，像亚马逊、谷歌、IBM、微软这样的大公司是在他们的云上提供转录服务的领导者。

每种产品都有其优点和缺点，可能会不同程度地满足您的需求。我强烈建议尝试所有这些服务，并选择最适合您所需用例的服务。在这篇文章中，我将重点放在 Amazon Transcription 服务上，因为它有丰富的输出:一个 JSON 文件，包含所有时间戳和其他信息，这非常有用！

(对我(希望)下一篇帖子的小炒作:Google Cloud(执行单个说话者转录时):输出没有任何标点符号。我希望写一个 RNN 模型来添加标点符号，以丰富输出。但这是在未来。让我们暂时回到 AWS！)

我将通过使用谷歌 Colab 的步骤。完整代码的链接是[这里是](https://colab.research.google.com/drive/1oaS1dOj5kkzx9Q8YRZd54AGHzrQEqg_9)。我们开始吧！

## **设置:通用软件包和初始化基本功能。**

```
!pip install boto3
import pandas as pd
import time
import boto3
```

Boto 是用于 Python 的 AWS 软件开发工具包。更多关于 Boto 3 文档的信息可以在[这里](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)找到。

我们还需要 AWS 帐户的访问密钥。如果你还没有创建一个帐户，请这样做(它是免费创建的，如果你不使用太多，还有免费层)！
当您拥有自己的帐户时，以下是获取个人访问密钥的方法(如果您已经有了访问密钥，请随意使用):

*   步骤 1:转到 AWS 管理控制台页面。
*   第二步:点击右上角的用户名，选择“我的安全凭证”
*   步骤 3:选择“访问密钥(访问密钥 ID 和秘密访问密钥。”
*   第四步:创建新的密钥，并记住保存它！
*   步骤 5:添加到我们的代码中:初始化转录作业。

```
transcribe = boto3.client('transcribe',
aws_access_key_id = #insert your access key ID here,
aws_secret_access_key = # insert your secret access key here
region_name = # region: usually, I put "us-east-2"
```

此外，我们需要**创建/连接我们的亚马逊 S3 存储。**
AWS 转录将从您的 S3 存储中转录文件。这非常方便，因为你可以将文件存储到亚马逊 S3，并直接从云中处理它们。点击阅读如何创建你的 S3 桶[。](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html)

尝试上传一个随机的音频/视频文件到 S3 存储，让我们试试转录服务！这些是我们的价值观

*   job_uri: S3 访问链接，通常为“S3://bucket _ name/”+audio _ file _ name(如“S3://viethoangtranduong/AWS . wav”)
*   job_name:对于每个转录调用，我们需要一个作业名。在这种情况下，我使用音频文件名本身。我们也可以使用散列函数来自动化系统。
    **注意:如果已经存在同名作业，作业将会崩溃。避免这些问题的可能方法是** :
    - **一个哈希函数**来编码音频文件名和作业的时间戳(这将避免重复，即使文件具有相同的名称)
    - **一个密钥生成器数据库**:如果我们使用 base 62(因为我们想避免“/”和“+”)，那么我们可以有 62⁶ = 56.8 B 唯一代码(这应该足够了)。我们可以将未使用的密钥用于每项工作。
    我们可以有两个数据库来存储使用和未使用的密钥。每次使用一个未使用的键时，我们将它移动到另一个数据库。我们必须跟踪文件名和匹配的键，以便将来遍历。使用这种方法，我们可以进一步发展成转录文件的链接缩写。
*   file_format:文件格式。AWS 可以处理大多数文件，如. mp3、.wav，甚至像. mp4 这样的视频

为了简单起见，我创建了一个函数调用 check_job_name 来处理重复的作业名。

```
def check_job_name(job_name):
  job_verification = True # all the transcriptions
  existed_jobs = transcribe.list_transcription_jobs() for job in existed_jobs['TranscriptionJobSummaries']:
    if job_name == job['TranscriptionJobName']:
      job_verification = False
      break if job_verification == False:
    command = input(job_name + " has existed. \nDo you want to override the existed job (Y/N): ")    if command.lower() == "y" or command.lower() == "yes":                transcribe.delete_transcription_job(TranscriptionJobName=job_name)
    elif command.lower() == "n" or command.lower() == "no":      job_name = input("Insert new job name? ")      check_job_name(job_name)
    else:
      print("Input can only be (Y/N)")
      command = input(job_name + " has existed. \nDo you want to override the existed job (Y/N): ")
  return job_name
```

**对于单扬声器文件**

```
def amazon_transcribe(audio_file_name):
  job_uri = # your S3 access link
  # Usually, I put like this to automate the process with the file name
  # "s3://bucket_name" + audio_file_name    # Usually, file names have spaces and have the file extension like .mp3
  # we take only a file name and delete all the space to name the job
  job_name = (audio_file_name.split('.')[0]).replace(" ", "")    # file format  
  file_format = audio_file_name.split('.')[1]

  # check if name is taken or not
  job_name = check_job_name(job_name)
  transcribe.start_transcription_job(
      TranscriptionJobName=job_name,
      Media={'MediaFileUri': job_uri},
      MediaFormat = file_format,
      LanguageCode='en-US')

  while True:
    result = transcribe.get_transcription_job(TranscriptionJobName=job_name)
    if result['TranscriptionJob']['TranscriptionJobStatus'] in ['COMPLETED', 'FAILED']:
        break
    time.sleep(15)
  if result['TranscriptionJob']['TranscriptionJobStatus'] == "COMPLETED":
    data = pd.read_json(result['TranscriptionJob']['Transcript']['TranscriptFileUri'])
  return data['results'][1][0]['transcript']
```

因为转录可能需要时间，所以我们创建了一个 while 循环来等待它完成(每 15 秒重新运行一次)。
最后一个“if”语句从 JSON 文件中提取特定的脚本。我将在文章的最后讨论如何提取时间戳。

**对于多扬声器文件**

对于 AWS 为多个扬声器转录，它可以检测的最大扬声器是 10 个。这次我将接受两个参数作为输入:audio_file_name 和 max_speakers。我强烈建议使用 max_speakers 值来提高 AWS 的准确性。但是，您也可以将其留空。

```
def amazon_transcribe(audio_file_name, max_speakers = -1):

  if max_speakers > 10:
    raise ValueError("Maximum detected speakers is 10.")

  job_uri = "s3 bucket link" + audio_file_name
  job_name = (audio_file_name.split('.')[0]).replace(" ", "")

  # check if name is taken or not
  job_name = check_job_name(job_name)
    if max_speakers != -1:
      transcribe.start_transcription_job(
        TranscriptionJobName=job_name,
        Media={'MediaFileUri': job_uri},
        MediaFormat=audio_file_name.split('.')[1],
        LanguageCode='en-US',
        Settings = {'ShowSpeakerLabels': True,
                  'MaxSpeakerLabels': max_speakers})
    else:
      transcribe.start_transcription_job(
        TranscriptionJobName=job_name,
        Media={'MediaFileUri': job_uri},
        MediaFormat=audio_file_name.split('.')[1],
        LanguageCode='en-US',
        Settings = {'ShowSpeakerLabels': True}) while True:
    result = transcribe.get_transcription_job(TranscriptionJobName=job_name)
    if result['TranscriptionJob']['TranscriptionJobStatus'] in ['COMPLETED', 'FAILED']:
      break
    time.sleep(15) if result['TranscriptionJob']['TranscriptionJobStatus'] == 'COMPLETED':
    data = pd.read_json(result['TranscriptionJob']['Transcript']['TranscriptFileUri'])
  return result
```

这一次，输出不再是文本，而是一个文件结果(在 Python 中，它是一种字典数据类型)。

```
data = pd.read_json(result['TranscriptionJob']['Transcript']['TranscriptFileUri'])
transcript = data['results'][2][0]['transcript']
```

这段代码将为您提供原始的转录(没有扬声器标签):结果将类似于将这些文件输入到单扬声器模型中。

**如何添加扬声器标签？**

现在，我们将读取“TranscriptFileUri”中的 JSON 文件。
由于我们正在使用 Google Colab，我还将演示如何访问特定文件夹中的文件。假设我们已经把它放在一个文件夹中:Colab Notebooks/AWS transcript reader:下面是访问它的方法。

```
from google.colab import drive
import sys
import os drive.mount('/content/drive/')
sys.path.append("/content/drive/My Drive/Colab Notebooks/AWS Transcribe reader")
os.chdir("/content/drive/My Drive/Colab Notebooks/AWS Transcribe reader")
```

现在，我们需要处理来自 AWS 转录的 JSON 输出。下面的代码将提供一个带有[时间戳，演讲者标签，内容]的. txt 文件。
当**输入“filename.json”文件**时，期待完整抄本的“filename.txt”文件。

```
import json
import datetime
import time as ptime def read_output(filename):
  # example filename: audio.json

  # take the input as the filename

  filename = (filename).split('.')[0]

  # Create an output txt file
  print(filename+'.txt')
  with open(filename+'.txt','w') as w:
    with open(filename+'.json') as f:
      data=json.loads(f.read())
      labels = data['results']['speaker_labels']['segments']
      speaker_start_times={}

      for label in labels:
        for item in label['items']:
          speaker_start_times[item['start_time']] = item['speaker_label'] items = data['results']['items']
      lines = []
      line = ''
      time = 0
      speaker = 'null'
      i = 0

      # loop through all elements
      for item in items:
        i = i+1
        content = item['alternatives'][0]['content'] # if it's starting time
        if item.get('start_time'):
          current_speaker = speaker_start_times[item['start_time']] # in AWS output, there are types as punctuation
        elif item['type'] == 'punctuation':
          line = line + content

      # handle different speaker
        if current_speaker != speaker:
          if speaker:
            lines.append({'speaker':speaker, 'line':line, 'time':time})
          line = content
          speaker = current_speaker
          time = item['start_time'] elif item['type'] != 'punctuation':
          line = line + ' ' + content
      lines.append({'speaker': speaker, 'line': line,'time': time})      # sort the results by the time
      sorted_lines = sorted(lines,key=lambda k: float(k['time']))
      # write into the .txt file
      for line_data in sorted_lines:
        line = '[' + str(datetime.timedelta(seconds=int(round(float(line_data['time']))))) + '] ' + line_data.get('speaker') + ': ' + line_data.get('line')
        w.write(line + '\n\n')
```

然后，在同一个文件夹中，会出现“filename.txt”文件，其中包含所有的抄本。

**奖励 1:直接访问和上传文件到 S3 存储器**

将文件上传到 AWS S3 存储肯定会使许多过程自动化。

```
# define AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, and bucket_name
# bucket_name: name of s3 storage folder
s3 = boto3.client('s3', 
  aws_access_key_id = AWS_ACCESS_KEY_ID,
  aws_secret_access_key = AWS_SECRET_ACCESS_KEY,
  region_name = "us-east-2")s3.upload_file(file_name, bucket_name, file_name)
```

**红利二:我们为什么不创建一个词汇表来增强准确性？**

我们可以通过控制台管理将词汇表手动上传到 AWS 转录服务中。接受的文件有。csv 或. txt.
然而，如果我们想用 Python 来自动化这个过程，那就有点棘手了。
下面是我发现的一种方法:AWS 通过 Python 接受特定类型的输入，这是一个有 4 列的 data frame:[' Phrases '，' IPA '，' SoundsLike '，' DisplayAs']转换成。txt 文件。有关各列含义和自定义词汇的更多信息，请阅读此处的。

```
def vocab_name(custom_name):
  vocab = pd.DataFrame([['Los-Angeles', np.nan, np.nan, "Los Angeles"], ["F.B.I.", "ɛ f b i aɪ", np.nan, "FBI"], ["Etienne", np.nan, "eh-tee-en", np.nan]], columns=['Phrase', 'IPA', 'SoundsLike', 'DisplayAs']) vocab.to_csv(custom_name+'.csv', header=True, index=None, sep='\t')
  import csv
  import time
  csv_file = 'custom_name+'.csv
  txt_file = 'custom_name+'.txt with open(txt_file, "w") as my_output_file:
    with open(csv_file, "r") as my_input_file:
      my_output_file.write(" ".join(row)+'\n') for row in csv.reader(my_input_file)]
    my_output_file.close()
  ptime.sleep(30) # wait for the file to finish bucket_name = #name of the S3 bucket
  s3.upload_file(txt_file, bucket_name, txt_file)
  ptime.sleep(60) response = transcribe.create_vocabulary(
    VocabularyName= custom_name,
    LanguageCode='en-US',
    VocabularyFileUri = "your s3 link" + txt_file)
    # the link usually is bucketname.region.amazonaws.com# after running vocab_name, we can check the status through this line# if it's ready, the VocabularyState will be 'READY'
transcribe.list_vocabularies()
```

上传和添加词汇表花费了相当多的时间。这可能不是最佳的方法，但它是有效的(在尝试字符串列表、列表列表中，但目前没有一个有效)。
**如果你找到了进一步自动化的方法，请评论！我很乐意讨论和了解更多。**

## **结论和…下一步是什么？**

这里有一个关于 AWS 转录的快速教程。希望你觉得有用！此外，我希望听到你的想法，并进一步讨论它们。(我可能会写一些关于在 Google Cloud little-punctuations 抄本中添加标点符号的内容，所以我希望我不会偷懒)。

链接到代码:Google Colab ( [这里](https://colab.research.google.com/drive/1oaS1dOj5kkzx9Q8YRZd54AGHzrQEqg_9))，Gist ( [这里](https://gist.github.com/viethoangtranduong/28a365e6457f35e206779995f488318a)，或者 Github( [这里](https://github.com/viethoangtranduong/AWS-Transcribe-Tutorial/blob/master/AWS_Transcribe_Tutorial.ipynb))。
欢迎通过[linkedin.com/in/viethoangtranduong/](https://www.linkedin.com/in/viethoangtranduong/)联系我或在此发表评论，我会尽力尽快回复！

**附:实时转录比转录一个类似的文件要好得多。**

**参考资料** [五分钟概述 AWS 转录](https://medium.com/@labrlearning/a-five-minute-overview-of-aws-transcribe-514b6cfeeddd)
[AWS 转录为 Docx。](https://github.com/kibaffo33/aws_transcribe_to_docx)