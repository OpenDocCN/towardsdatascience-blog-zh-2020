# MLflow 第 2 部分:将跟踪服务器部署到 Minikube！

> 原文：<https://towardsdatascience.com/mlflow-part-2-deploying-a-tracking-server-to-minikube-a2d6671e6455?source=collection_archive---------8----------------------->

![](img/f38000f037e55890fe41f5ab65d32cb1.png)

## MLflow 101

## 在 Minikube 上运行的单个服务器中创建一个用于记录和跟踪模型工件的点

*10/15/20 更新:在撰写本系列的下一篇文章时，我发现了几个妨碍我正确部署 Minikube 的错误。为此，我更新了一些内容，让您可以使用一个工作实例！😃*

欢迎回来，朋友们！我们继续关注 MLflow 上的迷你系列。如果你错过了第一部分，一定要在这里查看[。第一篇文章是关于 MLflow 的日志基本参数、度量和工件的超级基础介绍。这只是让我们将这些项目记录到本地机器上的一个位置，这不是一个理想的做法。在公司环境中，理想情况下，您希望将所有这些东西记录到一个集中的、可重用的位置。这就是我们今天要解决的问题！当然，你可以在这个链接](/mlflow-part-1-getting-started-with-mlflow-8b45bfbbb334)找到我在 GitHub [上的所有代码。](https://github.com/dkhundley/ds-quick-tips/tree/master/009_mlflow_tracking_server)

所以要明确的是，我们将涉及一些高级主题，这些主题需要一些关于 Docker 和 Kubernetes 的知识。我个人计划以后再写关于这些的文章，但是现在，如果你想快速开始使用 Docker 和 Kubernetes，我推荐以下资源:

*   [Docker 101 教程](https://www.docker.com/101-tutorial)
*   [学习 Kubernetes 基础知识](https://kubernetes.io/docs/tutorials/kubernetes-basics/)

现在，如果你知道 Kubernetes，你很可能熟悉 Minikube，但如果你不熟悉，Minikube 基本上是一个小虚拟机，你可以在本地机器上运行，启动一个沙盒环境来测试 Kubernetes 的概念。一旦 Minikube 建立并运行，对于那些在合法的 Kubernetes 环境中工作过的人来说，它看起来会非常熟悉。设置 Minikube 的说明在本页中有很好的记录[，但是为了让 Minikube 工作，我们需要在这篇文章的后面添加一些额外的东西。](https://kubernetes.io/docs/tasks/tools/install-minikube/)

在继续之前，我认为一张图片胜过千言万语，所以下面是我们将在这里建造的建筑的一张小图片。

![](img/854abf94e30a7b13e3c6578aa12b61f9.png)

好的，右边是我们的小型环境。同样，Minikube 是合法的 Kubernetes 环境的高度代表，所以 Minikube 中的所有东西都是我们在任何 Kubernetes 工作区中看到的。因此，我们可以看到 MLflow 的跟踪服务器部署在部署内部。该部署通过将服务连接到入口与外部世界进行交互(这就是为什么在我们的图片中入口跨越内部和外部)，然后我们可以在 web 浏览器中查看跟踪服务器界面。很简单，对吧？

好的，第一步是创建一个 Docker 映像，用于构建 MLflow 跟踪服务器。这真的很简单，我个人已经上传了我的公众形象，以防你想跳过这第一步。这是我个人 Docker Hub 中的图片。)Dockerfile 将简单地构建在一个基本的 Python 映像之上，安装 MLflow，并设置适当的 entrypoint 命令。看起来像这样:

```
# Defining base image
FROM python:3.8.2-slim# Installing packages from PyPi
RUN pip install mlflow[extras]==1.9.1 && \
    pip install psycopg2-binary==2.8.5 && \
    pip install boto3==1.15.16# Defining start up command
EXPOSE 5000
ENTRYPOINT ["mlflow", "server"]
```

你知道这里的流程:建造并推进到 Docker Hub！(或者就用我的。)

*我们 10-15-20 更新开始的地方！*

好的，在这篇文章的前一次迭代中，我试图使用一个简单的 PVC 来存储元数据和工件。事实证明这并不容易。相反，我们将不得不做一些额外的跑腿工作来让它在 Minikube 上运行。为此，我们将为后端元数据配置一个 Postgres 存储，并为工件配置一个名为 Minio 的对象存储。(下面有更多关于 Minio 的内容，以防你没听说过。)如果这两件事听起来都让你望而生畏，那没关系！您可以简单地使用我的代码来启动和运行。

好吧，让我们来解决 Postgres 的部署问题。以下是 K8s 清单代码:

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: mlflow-postgres-config
  labels:
    app: mlflow-postgres
data:
  POSTGRES_DB: mlflow_db
  POSTGRES_USER: mlflow_user
  POSTGRES_PASSWORD: mlflow_pwd
  PGDATA: /var/lib/postgresql/mlflow/data
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mlflow-postgres
  labels:
    app: mlflow-postgres
spec:
  selector:
    matchLabels:
      app: mlflow-postgres
  serviceName: "mlflow-postgres-service"
  replicas: 1
  template:
    metadata:
      labels:
        app: mlflow-postgres
    spec:
      containers:
      - name: mlflow-postgres
        image: postgres:11
        ports:
        - containerPort: 5432
          protocol: TCP
        envFrom:
        - configMapRef:
            name: mlflow-postgres-config
        resources:
          requests:
            memory: "1Gi"
            cpu: "500m"
        volumeMounts:
        - name: mlflow-pvc
          mountPath: /var/lib/postgresql/mlflow
  volumeClaimTemplates:
  - metadata:
      name: mlflow-pvc
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 100Mi
---
apiVersion: v1
kind: Service
metadata:
  name: mlflow-postgres-service
  labels:
    svc: mlflow-postgres-service
spec:
  type: NodePort
  ports:
  - port: 5432
    targetPort: 5432
    protocol: TCP
  selector:
    app: mlflow-postgres
```

因此，我不会一行一行地介绍所有内容，但在 1000 英尺的高度上，我将启动一个 Postgres 实例，它具有 100 兆字节的存储空间，并在代码顶部定义了适当的配置信息。如果你愿意，你可以改变这些变量。记住，我们只是在这里学习，所以这些变量显然是暴露的。在现实世界中，这是一个巨大的安全问题，所以如果您打算将它用于合法部署，请不要听从我的建议。

好了，部署好之后，我们就可以开始处理我们的对象存储了:Minio。现在，如果你像我一样对 Minio 完全陌生，它基本上是一个对象存储，你可以部署到 K8s，基本上模仿亚马逊网络服务(AWS)的 S3 服务。其部署语法如下所示:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mlflow-minio
spec:
  selector:
    matchLabels:
      app: mlflow-minio
  template:
    metadata:
      labels:
        app: mlflow-minio
    spec:
      volumes:
      - name: mlflow-pvc
        persistentVolumeClaim:
          claimName: mlflow-pvc
      containers:
      - name: mlflow-minio
        image: minio/minio:latest
        args:
        - server
        - /data
        volumeMounts:
        - name: mlflow-pvc
          mountPath: '/data'
        env:
        - name: MINIO_ACCESS_KEY
          value: "minio"
        - name: MINIO_SECRET_KEY
          value: "minio123"
        ports:
        - containerPort: 9000
---
apiVersion: v1
kind: Service
metadata:
  name: mlflow-minio-service
spec:
  type: NodePort
  ports:
  - port: 9000
    targetPort: 9000
    protocol: TCP
  selector:
    app: mlflow-minio
---
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: mlflow-minio-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.il/add-base-url: "true"
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
spec:
  rules:
  - host: mlflow-minio.local
    http:
      paths:
        - backend:
            serviceName: mlflow-minio-service
            servicePort: 9000
          path: /
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mlflow-pvc
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 100Mi
```

因此，从高层次来看，我们正在部署一个 Minio 对象存储，由一个拥有 100 兆数据的 PVC 提供支持。您还可以在部署的环境变量中看到，我们定义了访问密钥 ID 和秘密访问密钥。这些与 AWS 的 AWS_ACCESS_KEY_ID 和 AWS_SECRET_ACCESS_KEY 非常相关。现在，在进入下一步之前，你必须正确配置你的机器的入口，在我最初的帖子中，我在下面分享了如何做。(我懒得在这里重新输入，所以为 ingress 做一个 CTRL+F，我相信你会找到它！)

好的，如果您正确配置了入口，并在浏览器中导航到 mlflow-minio.local，您应该会看到这个闪屏。

![](img/4fc945de91a8390d6a694e1ae9874de6.png)

在相应的字段中，键入我们在 Minio 部署中定义的访问密钥和秘密密钥。(如果你和我保持一致，那分别是“minio”和“minio123”。)点击回车键进入下一个屏幕。

![](img/df4ce2c68e12329e253e2368a9f8af18.png)

好的，在我的例子中，我已经创建了“桶”,我们将使用它来存储我们的工件。对于您来说，只需单击 UI 右下角的 orangish 加号，选择“创建新桶”，并将您的新桶命名为“mlflow”即可。

唷！好了，我们已经设置好了后端的东西！是时候让实际的服务器开始运转了！

我将主要坚持使用这里的部署清单。大多数语法对您来说都很熟悉。这里唯一需要注意的是我们将传递给建筑 Docker 图像的参数。让我先向您展示一下我的部署清单。

```
# Creating MLflow deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mlflow-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mlflow-deployment
  template:
    metadata:
      labels:
        app: mlflow-deployment
    spec:
      containers:
      - name: mlflow-deployment
        image: dkhundley/mlflow-server:1.0.3
        imagePullPolicy: Always
        args:
        - --host=0.0.0.0
        - --port=5000
        - --backend-store-uri=postgresql://mlflow_user:mlflow_pwd@10.98.160.170:5432/mlflow_db
        - --default-artifact-root=s3://mlflow/
        - --workers=2
        env:
        - name: MLFLOW_S3_ENDPOINT_URL
          value: [http://mlflow-minio.local/](http://mlflow-minio.local/)
        - name: AWS_ACCESS_KEY_ID
          value: "minio"
        - name: AWS_SECRET_ACCESS_KEY
          value: "minio123"
        ports:
        - name: http
          containerPort: 5000
          protocol: TCP
---
apiVersion: v1
kind: Service
metadata:
  name: mlflow-service
spec:
  type: NodePort
  ports:
    - port: 5000
      targetPort: 5000
      protocol: TCP
      name: http
  selector:
    app: mlflow-deployment
---
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: mlflow-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.il/add-base-url: "true"
spec:
  rules:
  - host: mlflow-server.local
    http:
      paths:
        - backend:
            serviceName: mlflow-service
            servicePort: 5000
          path: /
```

这里需要指出几个关键点。首先，Postgres 实例在“args”中用其 IP 地址引用。我通过运行以下命令获得了该 IP 地址:

```
kubectl get services
```

这会给你这个屏幕:

![](img/451632f03e63307f1d1e93c4f4bace21.png)

您会注意到 mlflow-postgres-service 的 CLUSTER-IP 与我的服务器部署清单中的内容直接相关。无论你的服务显示什么，你都需要更新你的 IP，因为它可能和我的不一样。(说实话……我觉得有一种编程方式可以做到这一点，但我真的不知道该怎么做。)还要注意我们是如何将 Minio 称为后端服务器的。对你来说，我们确实在使用类似 AWS 的环境变量可能看起来很奇怪，但是，嘿，这就是它的工作方式！

好了，现在我们已经成功部署了一切，是时候让我们的 Minikube 入口工作了。如果你在一个合法的 Kubernetes 环境中工作，你可能不会有这个问题，但是 Minikube 在这里可能有点棘手。老实说，这最后一部分花了我好几天才弄明白，所以我很高兴终于把这些知识传授给你了！

让我们再看一眼 YAML 入口:

```
# Creating the Minikube ingress
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: mlflow-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.il/add-base-url: "true"
spec:
  rules:
  - host: mlflow-server.local
    http:
      paths:
        - backend:
            serviceName: mlflow-service
            servicePort: 5000
          path: /
```

这其中的大部分你应该很熟悉。在我们的示例中，我们将在 mlflow-server.local 上提供 MLflow 跟踪服务器的 UI。对您来说可能比较陌生的一点是那些注释，它们是绝对必要的。没有它们，您的入口将无法正常工作。我特意把下面的图片贴到 Twitter 上，试图让人们帮我解决黑屏问题。这很令人沮丧。

![](img/079937cec5eb81770ccf8b62613134bd.png)

呸，真是一团糟！经过反复试验，我终于发现上面提供的特定注释配置是有效的。老实说，我不能告诉你为什么。\_(ツ)_/

但是等等，还有呢！默认情况下，Minikube 没有设置为开箱即可处理入口。为了做到这一点，你需要做一些事情。首先，在 Minikube 服务器运行之后，运行以下命令:

```
minikube addons enable ingress
```

很简单。现在，您需要设置您的计算机，以通过我们在入口中设置的 mlflow-server.local 主机引用 Minikube 集群的 IP。要获取 Minikube 的 IP 地址，只需运行以下命令:

```
minikube ip
```

把它复制到你的剪贴板上。现在，下一部分对你来说可能是全新的。(至少，对我来说是这样！)就像您可以为 Linux 创建别名命令一样，您显然也可以创建从 IP 地址到 web 地址的别名关系。这非常有趣，因为我了解到这是您的浏览器将“localhost”翻译为您的本地 IP 地址的地方。

要导航到需要这样做的位置，请运行以下命令:

```
sudo nano /etc/hosts
```

您应该会看到一个类似这样的屏幕:

![](img/8fa2e3d6806ef67e7b8c27d790a921eb.png)

因此，您可以在顶部看到我刚才提到的本地主机。打开这个接口，粘贴您的 Minikube 的 IP 地址(在我的例子中是 192.168.64.4 ),然后粘贴 MLflow 服务器和 Minio 工件存储的主机名，在我们的例子中分别是 mlflow-server.local 和 mlflow-minio.local。

好的，如果你做了所有正确的事情，你应该已经准备好了！导航到您选择的浏览器，打开[http://ml flow-server . local .](http://mlflow-server.local.)如果一切顺利，您应该会看到一个熟悉的屏幕。

![](img/626ef36547ef6d838e07ce13da4015be.png)

这就是这篇文章的全部内容，各位！我不想给你们带来太多负担，所以在我们的下一篇文章中，我们将从这里开始，在这个共享的跟踪服务器上记录一两个实践模型，看看它是否工作。在接下来的两篇文章中，我们将进一步展示如何从这个跟踪服务器部署模型。所以说实话，这篇文章的内容可能没有那么迷人，但我们正在铺设火车轨道，它将在接下来的几篇文章中让一切真正飞起来。

在那之前，感谢你阅读这篇文章！请务必查看我以前关于其他数据科学相关主题的文章，下周我们将看到更多 MLflow 内容！