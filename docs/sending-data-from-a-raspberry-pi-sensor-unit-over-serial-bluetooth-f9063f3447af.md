# 通过串行蓝牙从 Raspberry Pi 传感器单元发送数据

> 原文：<https://towardsdatascience.com/sending-data-from-a-raspberry-pi-sensor-unit-over-serial-bluetooth-f9063f3447af?source=collection_archive---------6----------------------->

## 一个关于如何使用蓝牙将信息从树莓派发送到手机、平板电脑或便携式笔记本电脑的教程。

![](img/eb76ce8d99e707ded56b2629b3c82a07.png)

塞缪尔·切纳德在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 介绍

在构建便携式传感器时，我们通常希望在允许它们远程记录数据之前校准并仔细检查它们的读数。在开发它们的时候，我们可以很容易地 SSH 到它们，并把任何结果写到屏幕上。然而，当我们在世界上一个非常偏远的地方，没有笔记本电脑，无线网络或信号，会发生什么？

在本教程中，我们将探讨如何利用 Raspberry Pi Zero(无 WiFi)的蓝牙功能将初始结果传输到我们选择的手持设备。在我们的情况下，将通过使用手机或 android 平板电脑，这样我们就可以比较传感器和 GPS 读数。

# 安装和设置

在我们开始之前，蓝牙需要做一些改变。这些概述如下。

## 配置设备蓝牙

我们从更改已安装的蓝牙库的配置开始:

```
sudo nano /etc/systemd/system/dbus-org.bluez.service
```

在这里，我们找到从`ExecStart`开始的行，并用以下内容替换它:

```
ExecStart=/usr/lib/bluetooth/bluetoothd --compat --noplugin=sap
ExecStartPost=/usr/bin/sdptool add SP
```

添加了“兼容性”标志后，我们现在必须重新启动 Pi 上的蓝牙服务:

```
sudo systemctl daemon-reload;
sudo systemctl restart bluetooth.service;
```

## 配对我们的监控设备

为了防止在野外配对蓝牙设备时出现问题，预先配对设备总是一个好主意——保存它们的配置。

为此，我们按照下面链接中描述的过程使用`bluetoothctl`:

[](https://medium.com/cemac/pairing-a-bluetooth-device-using-a-terminal-1bfe267db35) [## 使用终端配对蓝牙设备

### 在使用 raspberry pi zero 时，您会受到 USB 端口的限制。而不是总是有一个 USB 集线器连接到…

medium.com](https://medium.com/cemac/pairing-a-bluetooth-device-using-a-terminal-1bfe267db35) 

1.  找到我们的主机 MAC 地址

```
hcitool scan
```

这将产生以下格式的结果:

```
Scanning ...XX:XX:XX:XX:XX:XX device1XX:XX:XX:XX:XX:XX device2
```

2.选择我们想要的设备并复制其地址。

3.执行以下操作:

```
sudo bluetoothctl
```

4.在蓝牙控制台中运行以下 3 个命令(替换您复制的地址):

```
discoverable on# thenpair XX:XX:XX:XX:XX:XX# and trust XX:XX:XX:XX:XX:XX# where XX corresponds to the address copied from above
```

配对时，可能会要求您确认两台设备上的 pin。`trust`将设备地址保存到信任列表。

要使 PI 在启动时可被发现，您可以查看下面的代码:

[](https://medium.com/cemac/keep-bluetooth-discoverable-rpi-unix-bbe1c9ecbdb6) [## 保持蓝牙可被发现(RPI / Unix)

### 如何从开机就启用蓝牙可见性和配对？

medium.com](https://medium.com/cemac/keep-bluetooth-discoverable-rpi-unix-bbe1c9ecbdb6) 

## 启动时启用通信

最后，我们希望告诉设备在启动时注意蓝牙连接。为此，我们可以将以下文件添加到`/etc/rc.local`(在`exit`命令之前)。

```
sudo rfcomm watch hci0 &
```

注意在末尾加上&符号，否则，它会停止设备的启动过程。此外，如果您正在通过串行读取另一个设备，例如 GPS 接收器，您可能希望使用`rfcomm1`而不是`hci0 (rfcomm0)`。

# 从另一台设备连接到蓝牙串行接口

根据您使用的设备，读取串行监视器的方法会有所不同。在 android 设备上，您可以采用 node/javascript 方法(这应该适用于所有操作系统！).出于演示的目的，我将描述一种使用 python 来检查 MacBook Pro 上的工作情况的方法。

## 确定端口名称

如果你有一个终端，最简单的方法就是输入

```
ls /dev/tty.
```

然后点击 tab(自动完成)按钮。

假设你没有改变这一点，这应该是你的设备`hostname`后跟串行端口。新安装的 raspberry pi 的默认串行端口路径应该是

```
/dev/tty.raspberrypi-SerialPort
```

## 读取接收的数据

为了读取接收到的任何数据，我们可以使用 python `serial`库和下面的代码片段。

```
import serialser = serial.Serial('/dev/tty.raspberrypi-SerialPort', timeout=1, baudrate=115000)serial.flushInput();serial.flushOutput()

while True:
    out = serial.readline().decode()
    if out!='' : print (out)
```

请注意，这是一个无限循环，不断打印它接收到的任何内容。要在收到“退出”消息时取消它，我们可以使用:

```
if out == 'exit': break
```

# 从传感器发送数据

## 从壳里

测试时，发送数据最简单的方法是从 raspberry pi shell 将数据回显到`/dev/rgcomm0`。这允许我们在编写更复杂的东西之前，手动测试端口上的通信。

```
echo "hello!" > /dev/rfcomm0
```

## 来自 python 脚本

如果从 raspberry pi 读取数据并对其进行预处理，我们很可能会使用 python 来完成繁重的工作。从这里我们可以将`rfcomm0` 通道视为一个文件，并按如下方式写入:

```
with open(‘/dev/rfcomm0’,’w’,1) as f:
     f.write(‘hello from python!’)
```

# 结论

如果我们想快速检查传感器在野外的表现，我们可以利用 Raspberry Pi 的蓝牙功能。这是通过创建一个蓝牙串行端口并通过它发送数据来实现的。如果我们不想携带笨重的笔记本电脑，或者在 WiFi 网络被占用或不可用的情况下，这些方法特别有用。

更复杂的任务，比如向 Raspberry Pi 发送命令，甚至通过蓝牙进入它也是可能的，但是超出了本教程的范围。