# 机器学习作业不会因为你盯着它们而跑得更快

> 原文：<https://towardsdatascience.com/machine-learning-jobs-dont-run-faster-because-you-stare-at-them-ef8b8fe07349?source=collection_archive---------31----------------------->

## 在您做更多令人兴奋的事情时，让您的 python 脚本文本状态更新和警告消息。

![](img/66843f384385359c9728d0ee7e2fc453.png)

图片由 [Free-Photos](https://pixabay.com/photos/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1149544) 来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1149544)

尽管很吸引人，但我没有时间坐下来看长时间运行的数据管道时钟、模型训练和 python 脚本来处理我的任务。通常，我们希望确保没有需要关注的失败。这个问题的一个解决方案是发送一封电子邮件或文本来获取你想要的任何状态更新。你可以放心地去做其他的事情。

关于如何从 python 脚本发送[电子邮件](https://realpython.com/python-send-email/)和[文本](https://www.twilio.com/docs/sms/send-messages#send-an-sms-with-twilios-api)有几个很好的教程。根据您是在组织内工作还是在项目中工作，设置会有所不同。对于本例，我将设置一个 Twilio 电话号码，并在 python 脚本运行期间使用该帐户向自己发送文本更新。

设置 Twilio 帐户后，您可以访问您的帐户 id 和 API 令牌。要通过 API 发送文本，您需要请求一个 Twilio 电话号码。整个过程大约需要 2 分钟。非常快！

下面的脚本是典型的机器学习类型的脚本。在这种情况下，autoML 脚本使用 [mljar 包](https://github.com/mljar/mljar-supervised)运行。我不仅想知道脚本何时完成运行，还想在出现故障时得到提醒。

```
# Example of sending text status updates while running an autoML script.import pandas as pd 
# scikit learn utilites
from sklearn.metrics import accuracy_score
from sklearn.model_selection import train_test_split# mljar-supervised package
from supervised.automl import AutoML######################################################
# [https://www.twilio.com/docs/python/install](https://www.twilio.com/docs/python/install)
from twilio.rest import Client# Your Account Sid and Auth Token from twilio.com/console
# DANGER! This is insecure. See [http://twil.io/secure](http://twil.io/secure)
account_sid = 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
auth_token = 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
client = Client(account_sid, auth_token)
#######################################################try:
    input_df  = pd.read_csv('train.csv')
    input_df.fillna(0)
    print('Input file columns: ' + str(input_df.columns))train_data  = input_df[['id', 'keyword', 'location', 'text']]train_label = input_df[['target']]print('train data : ' + str(train_data.head(1)))
    print('train label: ' + str(train_label.head(1)))X_train, X_test, y_train, y_test = train_test_split(
        pd.DataFrame(train_data), train_label, stratify=train_label, test_size=0.25,
        random_state=27
        )
except:
    message = client.messages.create(
                              body='mljar data load and split failed',
                              from_='+1xxxxxxxxxx', #twilio #
                              to='+1xxxxxxxxx'
                          )
    print(message.sid)try:
    automl = AutoML(mode="Explain")
    automl.fit(X_train, y_train)
    message = client.messages.create(
                              body='mljar Explain completed',
                              from_='+xxxxxxxxxx',
                              to='+1xxxxxxxx'
                          )
except:
    except:
    message = client.messages.create(
                              body='mljar Explain failed',
                              from_='+xxxxxxxxx',
                              to='+1xxxxxxxxxx'Nothing is more disappointing than logging onto your computer after waiting hours for a model to train and finding that it failed hours ago! Prevent this by setting up text or email alerts. It’s simple, fast, and is satisfying when you get the ‘successful completion’ text.
```

没有什么比提交一个脚本运行后登录到您的计算机上，发现它几个小时前就失败了更令人失望的了！通过设置文本或电子邮件提醒来防止这种情况。当你收到“成功完成”的信息时，这是简单、快速和令人满意的。