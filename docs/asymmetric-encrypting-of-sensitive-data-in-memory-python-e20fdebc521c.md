# 内存中敏感数据的非对称加密(Python)

> 原文：<https://towardsdatascience.com/asymmetric-encrypting-of-sensitive-data-in-memory-python-e20fdebc521c?source=collection_archive---------22----------------------->

有时需要在读取和写入周期之间加密数据，例如，我们有一个设备可以获取敏感数据并记录下来进行处理。但是，由于数据存储在写入数据的同一设备上，我们不希望使用用于加密数据的同一密钥来解密数据。这就是我们使用非对称加密的原因。

![](img/7948ffe48a9b69b7fff6cb2061cea7a9.png)

照片由[弗洛里安·奥利佛](https://unsplash.com/@rxspawn?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## 不对称加密

不对称加密对数据使用两个密钥(一个私钥和一个公钥)。这里，公钥用于每个单独的(易受攻击的)设备，仅用于加密数据。一旦加密，这些*就不能被*用来解密。然而，私钥是一个只提供给所有者的密钥，用于读取加密数据。也可以用私钥加密数据，这样就只能用公钥来读取数据，但这是一种不好的做法，会导致比它所解决的问题更多的问题。

## 密码学 python 包

我们将要使用的 python 包叫做`cryptography`，可以使用`pip install cryptography`来安装。

## 密钥生成

我们从导入所需的包开始:

```
**import** cryptography
**from** cryptography.hazmat.backends **import** default_backend
**from** cryptography.hazmat.primitives.asymmetric **import** rsa
**from** cryptography.hazmat.primitives **import** serialization
```

接下来，我们生成公钥和私钥。这些有两个参数—公共指数和密钥大小。公共指数是一个正质数(通常是`65537`)，密钥大小是模数的长度，以比特为单位(对于 2015 年的密钥，建议这些是`>2048`比特)

```
private_key = rsa.generate_private_key(
    public_exponent=65537,
    key_size=2048,
    backend=default_backend()
)
public_key = private_key.public_key()
```

## 保存生成的密钥

要保存生成的密钥，我们必须首先对它们进行序列化，然后将它们写入文件:

```
# private key
serial_private = private_key.private_bytes(
    encoding=serialization.Encoding.PEM,
    format=serialization.PrivateFormat.PKCS8,
    encryption_algorithm=serialization.NoEncryption()
)
with open('private_noshare.pem', 'wb') as f: f.write(serial_private) # public key
serial_pub = public_key.public_bytes(
    encoding=serialization.Encoding.PEM,
    format=serialization.PublicFormat.SubjectPublicKeyInfo
)
with open('public_shared.pem', 'wb') as f: f.write(serial_pub)
```

## 读取加密密钥

```
# make sure the following are imported
# from cryptography.hazmat.backends import default_backend
# from cryptography.hazmat.primitives import serialization#########      Private device only    ##########
def read_private (filename = "private_noshare.pem):
    with open(filename, "rb") as key_file:
        private_key = serialization.load_pem_private_key(
            key_file.read(),
            password=None,
            backend=default_backend()
        )
    return private_key

######### Public (shared) device only ##########
def read_public (filename = "public_shared.pem"):
    with open("public_shared.pem", "rb") as key_file:
        public_key = serialization.load_pem_public_key(
            key_file.read(),
            backend=default_backend()
        )
    return public_key
```

## 加密

为了存储一些敏感信息(例如一个人的体重),我们可以使用以下方法对其进行加密:

```
# make sure the following are imported
# from cryptography.hazmat.primitives import hashes
# from cryptography.hazmat.primitives.asymmetric import padding######### Public (shared) device only #########data = [b'My secret weight', b'My secret id']
public_key = read_public()open('test.txt', "wb").close() # clear file
for encode in data:
    encrypted = public_key.encrypt(
        encode,
        padding.OAEP(
            mgf=padding.MGF1(algorithm=hashes.SHA256()),
            algorithm=hashes.SHA256(),
            label=None
        )
    )

    with open('test.txt', "ab") as f: f.write(encrypted)
```

## [通信]解密

由于每个数据段都编码在相同数量的字节中，因此可以分块读取文件。这允许我们读取和解码每个数据块，重新创建原始数据集。

```
# make sure the following are imported
# from cryptography.hazmat.primitives import hashes
# from cryptography.hazmat.primitives.asymmetric import padding#########      Private device only    ##########read_data = []
private_key = read_private()with open('test.txt', "rb") as f:
    for encrypted in f:
        read_data.append(
            private_key.decrypt(
                encrypted,
            padding.OAEP(
                mgf=padding.MGF1(algorithm=hashes.SHA256()),
                algorithm=hashes.SHA256(),
                label=None
            )))# >>> read_data = [b'My secret weight', b'My secret id']
```

## 结论

所以你有它，一个简单的方法来加密数据(在内存中)，面向公众的设备，这样你就需要一个单独的密钥来解码它了。