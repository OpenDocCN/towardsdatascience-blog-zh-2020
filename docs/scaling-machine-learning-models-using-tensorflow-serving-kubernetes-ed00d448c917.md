# 使用 Tensorflow Serving & Kubernetes 扩展机器学习模型

> 原文：<https://towardsdatascience.com/scaling-machine-learning-models-using-tensorflow-serving-kubernetes-ed00d448c917?source=collection_archive---------28----------------------->

## 将 ML 模型投入生产和规模的开发人员指南

![](img/4f220423adaf9d7c853bc6cdde6527b3.png)

来源:[哈里特](https://unsplash.com/@hharritt)，转自 [Unsplash](https://unsplash.com/photos/Ype9sdOPdYc)

[Tensorflow serving](https://www.tensorflow.org/tfx/guide/serving) 是一个神奇的工具，可以将您的模型投入生产，从处理请求到有效地使用 GPU 处理多个模型。当请求数量增加，系统很难跟上请求时，问题就出现了。这就是 Kubernetes 可以帮助编排和扩展多个 docker 容器的地方。

# 大纲:

1.  设置**对接**
2.  获取**型号**
3.  **集装箱化**模式
4.  设置谷歌云**集群**
5.  **使用 Kubernetes 部署**型号

让我们开始吧:

# 1.设置 Docker

Docker 是什么？— Docker 提供了在松散隔离的环境(称为容器)中打包和运行应用程序的能力。([详情](https://opensource.com/resources/what-docker))

要安装**docker 你可以在这里勾选[](https://docs.docker.com/get-docker/)****它支持多个平台。******

****如果你使用的是 ubuntu，你可以使用:****

```
***# Install docker community edition* curl -fsSL [https://download.docker.com/linux/ubuntu/gpg](https://download.docker.com/linux/ubuntu/gpg) | sudo apt-key add -sudo add-apt-repository "deb [arch=amd64] [https://download.docker.com/linux/ubuntu](https://download.docker.com/linux/ubuntu) $(lsb_release -cs) stable"sudo apt-get update
sudo apt-get install -y docker-ce**
```

****要在 Ubuntu 中使用 docker，我们通常需要添加前缀 sudo，但是如果你不想每次都添加 sudo，你可以这样做:****

```
***# Remove sudo access needed for docker* sudo groupadd docker
sudo gpasswd -a $USER docker**
```

****我们将需要一个 [**DockerHub**](https://hub.docker.com/) 账户，以便以后我们可以推动我们的 docker 形象。如果您没有帐户，请创建一个帐户。****

```
***# Once your Dockerhub account is setup, login to docker* docker login**
```

# ****2.获取模型****

****[TensorFlow serving](https://www.tensorflow.org/tfx/guide/serving) 仅支持 [SavedModel](https://www.tensorflow.org/tutorials/keras/save_and_load#savedmodel_format) 格式，因此我们需要将任何 TensorFlow 模型或 Keras 模型转换为 SavedModel 格式。这里有一个关于如何保存到保存的模型格式的例子[https://www.tensorflow.org/guide/saved_model](https://www.tensorflow.org/guide/saved_model)****

****为简单起见，我们将从 Tensorflow/models 下载一个预先训练好的 ResNet 保存模型。****

```
***# Downloading ResNet saved models* mkdir /tmp/myresnetcurl -s [https://storage.googleapis.com/download.tensorflow.org/models/official/20181001_resnet/savedmodels/resnet_v2_fp32_savedmodel_NHWC_jpg.tar.gz](https://storage.googleapis.com/download.tensorflow.org/models/official/20181001_resnet/savedmodels/resnet_v2_fp32_savedmodel_NHWC_jpg.tar.gz) | tar --strip-components=2 -C /tmp/myresnet -xvz**
```

# ****3.集装箱服务模式****

****![](img/35f47d3ddb8680410053fa94693c03ce.png)****

****原始来源:[期限](https://tenor.com/view/whale-docker-container-gif-12376852)****

****接下来，我们需要制作一个 Tensorflow 服务图像。幸运的是 [Tensorflow 服务图片](https://hub.docker.com/r/tensorflow/serving/tags/)已经在 Dockerhub 中建立并可用。它有 GPU 和 CPU 两个版本。让我们下载它。****

```
***# Downloading the CPU version*
docker pull tensorflow/serving:2.1.0*# To download the GPU version you can just
# docker pull tensorflow/serving:2.1.0-gpu***
```

*****' tensor flow/serving:2 . 1 . 0 '*容器图像或任何其他 TF 服务图像的默认[入口点](https://docs.docker.com/engine/reference/builder/#entrypoint)为'**/usr/bin/TF _ serving _ entry point . sh**'。我们将创建自己的*TF _ serving _ entry point . sh*，我会在下面告诉你为什么:****

****tf_serving_entrypoint.sh****

****上面的脚本运行 Tensorflow 服务，从' **/models/resnet/** '加载模型，为 [**gRPC**](https://www.grpc.io/) **，**打开端口 8500，为 **REST-API** 打开端口 8501。****

```
***# Download the tf_serving_script.sh* curl -s [https://gist.githubusercontent.com/bendangnuksung/67c59cdfb2889e2738abdf60f8290b1d/raw/918cfa09d6efcc200bb2d617859138fd9e7c2eb4/tf_serving_entrypoint.sh](https://gist.githubusercontent.com/bendangnuksung/67c59cdfb2889e2738abdf60f8290b1d/raw/918cfa09d6efcc200bb2d617859138fd9e7c2eb4/tf_serving_entrypoint.sh) --output tf_serving_entrypoint.sh*# Make it executable* chmod +x tf_serving_script.sh**
```

****建议创建我们自己的服务脚本，因为您将可以控制 ***模型名称*** 、**、端口** 和 ***模型路径*** 。如果您有多个模型，默认的“tf_serving_entrypoint.sh”将抛出一个错误。为了服务多个模型，您需要为您的多个模型创建一个[**models . config**](https://www.tensorflow.org/tfx/serving/serving_config#model_server_config_details)并更新您的脚本。您的服务模型看起来有点像这样:****

```
***# Just an example of running TF serving with models.cofig
# tensorflow_model_server --port=8500 --rest_api_port=8501 
# --model_config_file=/path/to/models.config***
```

****要了解更多关于用 docker 上菜的 TF，请参考 [tfx/serving/docker](https://www.tensorflow.org/tfx/serving/docker) 。****

****将 **RestNet 保存的模型**和 **tf_serving_script.sh** 移动到 docker 镜像中并运行:****

```
***# Run the tf/serving containerimage* docker run -d --name=servingbase tensorflow/serving:2.1.0*# copy the saved model* docker cp /tmp/resnet/ servingbase:/models/*# copy tf_serving_script.sh* docker cp tf_serving_entrypoint.sh servingbase:/usr/bin/tf_serving_entrypoint.sh*# commit* docker commit servingbase myresnet:latest*# kill the container* docker kill servingbase*# running new created image* docker run -d --name=myresnet -p 8500:8500 -p 8501:8501 myresnet:latest*# list running container and see whether its running* docker ps**
```

****让我们测试一下码头工人是否响应我们的请求。将下载一个客户端脚本，使用 [**gRPC**](https://www.grpc.io) 和 **RESTAPI** 进行推理。****

```
***# Download the Restnet client script* curl [https://gist.githubusercontent.com/bendangnuksung/8e94434a8c85308c2933e419ec29755a/raw/0a52618cdce47d16f2e71c900f2a1ee92063933f/resnet_client_restapi_grpc.py](https://gist.githubusercontent.com/bendangnuksung/8e94434a8c85308c2933e419ec29755a/raw/0a52618cdce47d16f2e71c900f2a1ee92063933f/resnet_client_restapi_grpc.py) --output [resnet_client_restapi_grpc.py](https://gist.github.com/bendangnuksung/8e94434a8c85308c2933e419ec29755a)**
```

******测试客户端脚本**:****

```
***# Test using GRPC* python [resnet_client_restapi_grpc.py](https://gist.github.com/bendangnuksung/8e94434a8c85308c2933e419ec29755a) -p 8500 -ip localhost*# Test using RESTAPI* python resnet_client_restapi_grpc.py -p 8501 -ip localhost*# We will see that GRPC has faster response time than RESTAPI
# Once its deployed in cloud the difference is much higher**# Stop running container* docker stop myresnet**
```

******将图像推送到**[**docker hub**](https://hub.docker.com)**:******

```
***# Push image to Dockerhub | replace* ***DOCKERHUB_USERNAME*** with yourA/c name
docker tag myresnet:latest **DOCKERHUB_USERNAME**/myresnet:latest
docker push **DOCKERHUB_USERNAME**/myresnet:latest**
```

# ****4.设置 Google 云集群****

****我们将使用[谷歌云平台(GCP)](https://cloud.google.com) ，因为它提供 Kubernetes 引擎，并提供一年 300 美元的免费试用，这对我们来说是一个惊人的资源。您可以在这里 免费试用您的 [**。您需要启用您的帐单来激活您的 300 美元免费试用。**](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwiLsZ27787pAhXazDgGHYj1BhMQFjAAegQICRAC&url=https%3A%2F%2Fcloud.google.com%2Ffree&usg=AOvVaw1o4_byaUt6R1U9GFwHeAd6)****

****GCP 允许你使用 [Gcloud SDK](https://cloud.google.com/sdk/gcloud) 通过 CLI 处理资源。**为[**Ubuntu**](https://cloud.google.com/sdk/docs/quickstart-debian-ubuntu)[**Mac**](https://cloud.google.com/sdk/docs/quickstart-macos)安装** Gcloud SDK。****

**[Kubectl](https://kubernetes.io/docs/reference/kubectl/overview/) 也需要控制 Kubernetes 集群。从[T21 这里](https://kubernetes.io/docs/tasks/tools/install-kubectl/) **安装 Kubectl。****

****设置 GCloud 项目并实例化集群**:**

```
*# Proceed once Gcloud and Kubectl is installed**# Gcloud login* 
gcloud auth login*# Create unique Project name | Replace* ***USERNAME*** *with any unique name*
gcloud projects create **USERNAME**-servingtest-project*# Set project* gcloud config set project **USERNAME**-servingtest-project
```

****激活 Kubernetes 引擎 API** 。[https://console . cloud . Google . com/APIs/API/container . Google APIs . com/overview？project =**USERNAME**-serving test-project](https://console.cloud.google.com/apis/api/container.googleapis.com/overview?project=USERNAME-servingtest-project)(将链接中的 **USERNAME** 替换为您之前提供的唯一名称)**

****创建并连接集群:****

```
*# Creating a cluster with 2 nodes*
gcloud beta container --project "**USERNAME**-servingtest-project" clusters create "cluster-1" --zone "us-central1-c" --disk-size "30" --num-nodes "2"*# You can change the zone and disk size. More Details at* [*https://cloud.google.com/sdk/gcloud/reference/container/clusters/create*](https://cloud.google.com/sdk/gcloud/reference/container/clusters/create)*# Connect to cluster*
gcloud container clusters get-credentials cluster-1 --zone us-central1-c --project **USERNAME**-servingtest-project*# Check whether the 2 Nodes are ready:*
kubectl get nodes*# Sample output:* # *NAME                STATUS   ROLES    AGE   VERSION* # *gke-cluster-1-...   Ready    <none>   74s   v1.14.10-gke.36* # *gke-cluster-1-...   Ready    <none>   75s   v1.14.10-gke.36*
```

# **5.使用 Kubernetes 部署模型**

**什么是 Kubernetes？—它是一个容器编制器。你可以认为 Kubernetes 是一个很好的俄罗斯方块玩家，所以每当不同形状和大小的新容器进来时，Kubernetes 都会找到放置容器的最佳位置。**

**务必查看此[视频](https://youtu.be/u8dW8DrcSmo?t=671)以便更好地理解。**

**分两个阶段使用 Kubernetes 部署容器模型:**

1.  ****部署**:部署负责保持一套[吊舱](https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/)运行。我们为 pod 定义了所需状态的列表，部署控制器将实际状态更改为所需状态。**

**2.**服务**:将运行在一组[pod](https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/)上的应用程序公开为网络服务的抽象方式。我们定义了一个状态列表，比如端口应该监听哪里或者应该监听哪个应用程序。**

**service.yaml**

****让我们下载这两个文件**:**

```
# Download deployment & service yaml files
curl [https://gist.githubusercontent.com/bendangnuksung/f1482aa9da7100bc3050616aaf503a2c/raw/7dc54db4ee1311c2ec54f0f4bd7e8a343d7fe053/deployment.yaml](https://gist.githubusercontent.com/bendangnuksung/f1482aa9da7100bc3050616aaf503a2c/raw/7dc54db4ee1311c2ec54f0f4bd7e8a343d7fe053/deployment.yaml)--output [deployment.yaml](https://gist.githubusercontent.com/bendangnuksung/f1482aa9da7100bc3050616aaf503a2c/raw/5634d69757aad3d392343bfbe15a85badcdf76c9/deployment.yaml)curl [https://gist.githubusercontent.com/bendangnuksung/5f3edd5f16ea5bc4c2bc58a783b562c0/raw/f36856c612ceb1ac0958a88a67ec02da3d437ffe/service.yaml](https://gist.githubusercontent.com/bendangnuksung/5f3edd5f16ea5bc4c2bc58a783b562c0/raw/f36856c612ceb1ac0958a88a67ec02da3d437ffe/service.yaml) --output service.yaml
```

**我们将需要对' **deployment.yaml** '进行更改**

```
*line 16: Change bendang to Your Dockerhub account name
from: image: bendang/myresnet:latest
to  : image:* **DOCKERHUB_USERNAME***/myresnet:latest*
```

****开始部署**:**

```
**kubectl get deployment** *#Output:No resources found in default namespace.***kubectl apply -f deployment.yaml** *#Output: deployment.extensions/myresnet-deployment created**# Wait until the depoyment is ready: 2/2***kubectl get deployment**
*#Output: 
# NAME                  READY   UP-TO-DATE   AVAILABLE   AGE
# myresnet-deployment   2/2     2            2           1m*
```

**这将把“ *myresnet:latest”映像加载到“deployment . YAML”*文件中定义的两个窗格中。**

****启动服务**:**

```
**kubectl get service** *# output:
#NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
#kubernetes   ClusterIP   10.7.240.1   <none>        443/TCP   28h***kubectl apply -f service.yaml** *#output: service/myresnet-service created**# wait until it allocate external IP for LoadBalancer***kubectl get service**
*#output
#NAME         TYPE          CLUSTER-IP   EXTERNAL-IP     PORT(S)                         
#kubernetes   ClusterIP     10.7.240.1   <none>          443/TCP                       
#myresnet-s.  LoadBalancer  10.7.252.203* ***35.192.46.666*** *8501 & 8500*
```

**运行*‘service . YAML’*后，我们将获得一个外部 IP，在本例中，它是 ***35.192.46.666。这个 IP 现在将成为我们的单一访问点，我们可以在这里调用我们的模型，所有的负载平衡都在内部处理。*****

****测试:****

**我们仍将使用相同的脚本'*resnet _ client _ restapi _ grpc . py*'，唯一的变化是提供我们创建的服务的'*外部 IP* '。**

```
*# Test using GRPC* python [resnet_client_restapi_grpc.py](https://gist.github.com/bendangnuksung/8e94434a8c85308c2933e419ec29755a) -p 8500 -ip ***35.192.46.666****# Test using RESTAPI* python [resnet_client_restapi_grpc.py](https://gist.github.com/bendangnuksung/8e94434a8c85308c2933e419ec29755a) -p 8501 -ip ***35.192.46.666***
```

**如果你有任何问题，请在下面的评论中告诉我。**

***敬请关注我下一篇关于* ***在谷歌云功能上部署机器学习模型*** *。***