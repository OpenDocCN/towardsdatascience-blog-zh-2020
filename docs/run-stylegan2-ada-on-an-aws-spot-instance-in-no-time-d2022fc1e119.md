# 立即在 AWS Spot 实例上运行 StyleGAN2 ADA

> 原文：<https://towardsdatascience.com/run-stylegan2-ada-on-an-aws-spot-instance-in-no-time-d2022fc1e119?source=collection_archive---------33----------------------->

## 在 AWS 上运行 StyleGAN2，与预训练的模型一起玩，或者从头开始训练一个模型。

![](img/e6a214bbf038ebcc7feb5be9f4622d8f.png)

示例使用有限数量的训练数据为几个数据集生成图像，使用 StyleGAN2 ADA 进行训练(图像来自“[使用有限数据训练生成式对抗网络](https://arxiv.org/abs/2006.06676)”论文)。

# 介绍

最近 NVIDIA 发表了一篇名为“[用有限数据训练生成对抗网络](https://arxiv.org/abs/2006.06676)的论文，并发布了[代码](https://github.com/NVlabs/stylegan2-ada)。他们提出了一种自适应鉴别器增强(ADA)机制，可以稳定 StyleGAN2 训练，并在小数据集上取得明显更好的结果。

在本文中，我们将展示如何在 AWS Spot 实例上快速运行这段代码。

# 启动 AWS Spot 实例

> “Spot 实例是未使用的 EC2 实例，其可用价格低于按需价格。因为 Spot 实例使您能够以大幅折扣请求未使用的 EC2 实例，所以您可以显著降低 Amazon EC2 的成本。”
> 
> — [现场实例](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-spot-instances.html)，AWS 文档

为了启动一个 Spot 实例并在环境中运行 Docker 容器，我们将使用 [Spotty](https://spotty.cloud) 。Spotty 是一个开源工具，旨在简化云中深度学习模型的开发。

使用 pip 安装最新版本:

```
pip install -U spotty
```

Spotty 只需要在项目的根目录中有一个描述实例和容器参数的`spotty.yaml`配置文件。而且我们已经在原回购的[这个](https://github.com/spotty-playground/stylegan2-ada)叉里给你准备好了。

使用以下命令克隆项目:

```
git clone [https://github.com/spotty-playground/stylegan2-ada](https://github.com/spotty-playground/stylegan2-ada)
```

默认情况下，Spotty 将在`us-east-1`区域启动一个`p2.xlarge` Spot 实例，但是，如果需要，您总是可以在`spotty.yaml`文件中更改这些参数。

在启动实例之前，请确保您已经安装并配置了 AWS CLI。更多信息，请参见[安装 AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) 和[配置基础知识](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html)。

要启动实例，请从项目根目录运行以下命令:

```
spotty start aws-1
```

等待实例启动:

```
Bucket "spotty-stylegan2-ada-la4gun6uy92a-eu-central-1" was created.
Syncing the project with the S3 bucket...
Creating IAM role for the instance...
Preparing CloudFormation template...
  - volume "stylegan2-ada-aws-1-workspace" will be created
  - availability zone: auto
  - on-demand instance
  - AMI: "Deep Learning AMI (Ubuntu 16.04) Version 35.0" (ami-01955a821cfdfaf73)Volumes:
+-----------+------------+------------+-----------------+
| Name      | Mount Path | Type       | Deletion Policy |
+===========+============+============+=================+
| workspace | /workspace | EBS volume | Retain Volume   |
+-----------+------------+------------+-----------------+Waiting for the stack to be created...
  - launching the instance... DONE
  - preparing the instance... DONE
  - mounting volumes... DONE
  - syncing project files... DONE
  - building Docker image... DONE
  - starting container... DONE+---------------------+---------------+
| Instance State      | running       |
+---------------------+---------------+
| Instance Type       | p2.xlarge     |
+---------------------+---------------+
| Availability Zone   | us-east-1e    |
+---------------------+---------------+
| Public IP Address   | 100.26.50.220 |
+---------------------+---------------+
| Purchasing Option   | Spot Instance |
+---------------------+---------------+
| Spot Instance Price | $0.2700       |
+---------------------+---------------+Use the "spotty sh" command to connect to the container.
```

就是这样！只需连接到容器并使用代码:

```
spotty sh aws-1
```

Spotty 使用 [tmux](https://github.com/tmux/tmux/wiki/Getting-Started) ，所以如果你的互联网连接断开了，你也不会丢失你的进度。有关 tmux 的更多信息，请阅读[官方文档](https://github.com/tmux/tmux/wiki/Getting-Started)或 [Tmux 备忘单](https://tmuxcheatsheet.com/)。

# 例子

## 生成 MetFaces 图像

![](img/fba17032529619e0895ea76b4d6d614d.png)

*met faces 模型生成的图像。*

连接到容器后，使用以下命令通过预先训练的模型生成图像:

```
python generate.py — outdir=out/metfaces — trunc=1 — seeds=85,265,297,849 \
 — network=[https://nvlabs-fi-cdn.nvidia.com/stylegan2-ada/pretrained/metfaces.pkl](https://nvlabs-fi-cdn.nvidia.com/stylegan2-ada/pretrained/metfaces.pkl)
```

该命令将在`out/metfaces/`目录中生成 4 幅图像。您可以使用以下命令将它们下载到您的本地计算机上:

```
spotty download aws-1 -i 'out/*'
```

## 使用 FFHQ 模型将图像投影到潜在空间

在这个例子中，我们将展示如何使用自定义的 Spotty 脚本。

要找到一个潜在向量并生成一个进展视频，将一个目标图像`target.jpg`放到本地机器上的`data/`目录中，并运行以下命令:

```
spotty run aws-1 projector-ffhq -p TARGET=data/target.jpg -p OUTPUT_DIR=out/projection
```

该脚本将在一个 [tmux](https://github.com/tmux/tmux/wiki/Getting-Started) 会话中运行。因此，即使您的互联网连接断开，该进程仍将运行，您可以在以后的任何时间重新连接它。

脚本完成后，使用`Ctrl+b, x`组合键关闭 tmux 窗口，并将结果下载到您的本地机器:

```
spotty download aws-1 -i 'out/*'
```

完成后，不要忘记停止实例！使用`spotty stop`命令。如果不再需要这些数据，请删除 EBS 卷。