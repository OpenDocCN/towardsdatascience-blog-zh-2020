# 在 CouchDB 中存储 Hyperledger 结构证书和密钥

> 原文：<https://towardsdatascience.com/store-hyperledger-fabric-certificates-and-keys-in-couchdb-43c8f91a61b0?source=collection_archive---------20----------------------->

## Hyperledger Fabric 是关于权限的。这些权限以证书和密钥的形式提供。从广义上讲，它被称为身份。

![](img/4477d41b372b639770bc461d1d1d1c4f.png)

照片由[迈卡·威廉姆斯](https://unsplash.com/@mr_williams_photography?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/safe?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

当应用程序与 Hyperledger Fabric 网络交互时，它使用此身份来验证自己。结构网络验证身份并授权应用程序进行交互。

简而言之，身份是非常重要的，如果你不妥善保存，那么它可能会变成一个令人头痛的问题。😩

> *最初发布于*[*https://schadokar . dev*](https://schadokar.dev/posts/store-hyperledger-fabric-certificates-and-keys-in-couchdb/)

# 我可以在哪里存储身份？💼

在 Hyperledger 结构中，这种存储被称为 [***钱包***](https://hyperledger-fabric.readthedocs.io/en/latest/developapps/wallet.html) 。

钱包有三种类型:

## *文件系统*

这是一个简单的文件夹。本地存储钱包。对于钱包来说，这是一个不错的默认选择。在`fabric-samples/balance-transfer`中，`file system`是默认的钱包。当你运行`balance-transfer`时，它会创建一个`fabric-client-kv-orgName`文件夹并保存所有的身份。这个配置是在`orgname.yaml`的[链接](https://github.com/hyperledger/fabric-samples/blob/release-1.4/balance-transfer/artifacts/org1.yaml)中定义的。

## 内存中

应用存储中的钱包。当您的应用程序在不能访问文件系统的受限环境中运行时，请使用这种类型的 wallet 通常是网络浏览器。值得记住的是，这种类型的钱包是不稳定的；应用程序正常结束或崩溃后，身份将会丢失。— [文档](https://hyperledger-fabric.readthedocs.io/en/latest/developapps/wallet.html#types)

## CouchDB

用 couchdb 当钱包。此选项最适合生产。

在本教程中，我们将把`CouchDB`配置为钱包。👨🏻‍💻

> 为了演示，我使用了`Fabric Node SDK`和`fabric/samples/balance-transfer`。

钱包使用 2 个存储来保存证书和密钥:

## 1.州商店:

状态存储用于存储注册身份的证书。它存储了身份的基本信息:

```
{
  "name": "test",
  "mspid": "org1",
  "roles": null,
  "affiliation": "",
  "enrollmentSecret": "<ENROLLMENT_SECRET>",
  "enrollment": {
    "signingIdentity": "<PRIVATE_KEY_NAME>",
    "identity": {
      "certificate": "<SIGN_CERT>"
    }
  }
}
```

> *❗️* 注意:签名标识是存储在加密存储中的私钥和公钥的指针或地址。

## 2.加密存储:

加密存储用于存储身份的公钥和私钥。

要将 couchdb 配置为 wallet:

## 第一步

导入由`Node SDK`提供的`CouchDBKeyValueStore`库。

```
**const CDBKVS = require("fabric-client/lib/impl/CouchDBKeyValueStore.js");**
```

> 请务必阅读《T4 》,它值得一读。

## 第二步

设置`state store`。

```
**let stateStore = await new CDBKVS({
  url: "https://<USERNAME>:<PASSWORD>@<URL>",
  name: "<DB_NAME>"
});

const Client = require("fabric-client");

const client = Client.loadFromConfig("path of network.yaml");

client.setStateStore(stateStore);**
```

*   `<USERNAME>`是 couchdb 的用户名。
*   `<PASSWORD>`是 couchdb 的密码。
*   `<URL>`是 couchdb 的 URL。
*   `<DB_NAME>`(可选)是用作状态存储的数据库名。默认的 dbname 是`userdb`。如果数据库不存在，它会创建它。

> [**客户端**](https://hyperledger.github.io/fabric-sdk-node/release-1.4/Client.html) **是用户和光纤网络之间的接口。**

## 第三步

设置`crypto store`。

```
**const cryptoSuite = Client.newCryptoSuite();

let cryptoKS = Client.newCryptoKeyStore(CDBKVS, {
  url: "https://<USERNAME>:<PASSWORD>@<URL>",
  name: "<DB_NAME>"
});

cryptoSuite.setCryptoKeyStore(cryptoKS);

client.setCryptoSuite(cryptoSuite);**
```

您必须根据上述步骤更新客户端，使其使用 couchdb。
在下一节中，我们将在`balance-transfer` fabric 示例中实现上述步骤。

# CouchDB 在平衡传递中的实现

我使用[平衡转移](https://github.com/hyperledger/fabric-samples/blob/release-1.4/balance-transfer)织物样品作为参考。

> 我假设你知道如何进行余额转移。

## 启动余额转移网络

按照余额转移说明启动网络。

它将通过以下方式启动网络:

*   2 CAs
*   单独订购者
*   4 个同行(每个组织 2 个同行)

## 为钱包启动 couchdb

如果您使用基于云的 couchdb，这一步是可选的。

**基于 Docker 的 Couchdb**

```
**docker run --name couch-userdb -e COUCHDB_USER=admin -e COUCHDB_PASSWORD=password -p 5984:5984 -d couchdb**
```

如果不存在，上述命令将从 docker hub 中提取`couchdb`的 docker 图像。

**CouchDB 详细信息:**

*   容器名称:`couch-userdb`
*   CouchDB 用户名:`admin`
*   CouchDB 密码:`password`
*   网址:`localhost:5984`

CouchDB 连接 URL 是

```
https://<USERNAME>:<PASSWORD>@<URL> [**https://admin:password@localhost:5984**](https://admin:password@localhost:5984)
```

## 在余额转移中更新客户

打开`app/helper.js`，更新`getClientForOrg`。

在下面的代码中，我们只是用上面的 couchdb 配置步骤替换了`await client.initCredentialStores();`。

我们所做的改变，

*   ***第 13 行:*** 导入`CouchDBKeyValueStore`。从上面开始。
*   ***第 31–52 行:*** 设置状态存储和加密存储。第二步& 3。

上面的代码有一个小的变化。

```
**// Client variable is used as hfc
var hfc = require("fabric-client");

// Instead of Client
const Client = require("fabric-client");**
```

> ***状态存储和加密存储的 db(dbname)不必相同。两家商店都可以有各自独立的数据库。这取决于要求。您可以将状态存储和加密存储 db 分别命名为*** `***orgName-state-store***` ***和*** `***orgName-crypto-store***` ***。*** ***每个组织必须有自己的状态存储和加密存储 db，否则会抛出认证错误。***

```
**Error: fabric-ca request register failed with errors [[{"code":20,"message":"Authentication failure"}]]**
```

## 在余额转移中注册新用户

一旦注册了用户，就可以使用 couchdb apis 检查状态存储和加密存储。

**例如:注册一个用户**

我使用下面的参数来注册一个用户。对于`org1`，我对`state-store`和`crypto-store`使用了相同的 db `org1db`。

*   名称:`alice`
*   组织:`org1`
*   数据库名:`org1db`
*   CouchDB URL: `[**http://admin:password@localhost:5369**](http://admin:password@localhost:5369)`

打开浏览器，转到`**http://localhost:5369/org1db/_all_docs**`。返回保存在`org1db`中的所有文档。

![](img/7ab56145708525017bb7e2e7e71ad5b5.png)

指标`0, 1, 2`是`admin`的证书。

索引`3`是存储在`state-store`中的`alice`证书。
索引`4-5`是`crypto-store`中存储的`alice`的公钥和私钥。

转到`http://localhost:5369/org1db/alice`。它返回存储在状态存储中的`alice`的所有细节。

![](img/63abb6d546b4c2d1f33198bc8185d995.png)

检查`signingIdentity`。

```
**"signingIdentity":"d37a97a8c2377c21537801ec1a929d81905ae57963a2f6c8ba0308931a7fc791"**
```

现在，检查上图中索引`4 & 5`的 id。两者是一样的。

如果您还记得，`signingIdentity`字段是存储在加密存储中的身份的私钥和公钥的引用。

# 结论

CouchDB wallet 是生产用例的绝佳选择。您可以尝试使用其他数据库，但是在这种情况下，您必须像`CouchDBKeyValueStore.js`一样编写相应的库。

以下是我觉得有用的参考资料。如果您发现任何您认为可以添加到此处的资源，请不要羞于分享。😉

# 参考📌

*   [https://developer . IBM . com/tutorials/store-fabric-certificates-keys-IBM-cloud ant-fabric-node-SDK/](https://developer.ibm.com/tutorials/store-fabric-certificates-keys-ibm-cloudant-fabric-node-sdk/)
*   [https://stack overflow . com/questions/53639061/hyperledger-fabric-state-store 和 crypto-store 之间的区别是什么](https://stackoverflow.com/questions/53639061/hyperledger-fabric-what-is-the-difference-between-state-store-and-crypto-store)
*   [https://stack overflow . com/questions/54305378/hyperledger-fabric-client-credential-store-using-couch db](https://stackoverflow.com/questions/54305378/hyperledger-fabric-client-credential-store-using-couchdb)
*   [https://stack overflow . com/questions/58371858/hyperledger-fabric-client-credential-store-using-couchdbcouchdbkeyvaluestore](https://stackoverflow.com/questions/58371858/hyperledger-fabric-client-credential-store-using-couchdbcouchdbkeyvaluestore)

*原载于*[*https://schadokar . dev*](https://schadokar.dev/posts/couchdb-keyvalue-store-hlf/)*。*