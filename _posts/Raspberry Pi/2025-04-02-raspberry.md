---
title: 树莓派5 使用 imx219摄像头
description: 树莓派5，77° imx219摄像头。
author: yu
date: 2025-04-02 11:32:48 +0800
categories: [Blogging, 树莓派]
tags: [Raspberry Pi, apt]
---

## 安装imx219

### 连接

树莓派5 有两个`CSI`接口（摄像头接口，相机串行接口，Camera Serial Interface），分别为`CAM/DISP 0` 和 `CAM/DISP 1`，这两个接口的位置是挨在一起的。

下图将`imx219`接到了`CAM/DISP 1`。
![](/common/img/raspberryPi/connect-imx219.jpg)
_Raspberry Pi 5 与 imx219 连接示意图_

### 配置

打开文件`/boot/config.txt`
```shell
sudo vim.tiny /boot/config.txt
```
若出现如下内容，则关闭此文件，并使用文件`/boot/firmware/config.txt`进行配置：
![](/common/img/raspberryPi/config.png)

打开`/boot/firmware/config.txt`：
```shell
sudo vim.tiny /boot/firmware/config.txt
```

修改两处，并保存退出：
1. 禁用摄像头自动检测：将`camera_auto_detect=1`改为`camera_auto_detect=0`。  
![](/common/img/raspberryPi/camera_auto_detect.png)
2. 文件最后添加，`dtoverlay=imx219,cam0` 或 `dtoverlay=imx219,cam1`。写入 cam0 还是 cam1，要与imx219连接的是 `CAM/DISP 0`（cam0） 还是 `CAM/DISP 1`（cam1）对应。  
如果装了`双摄像头`，`dtoverlay=imx219,cam0`和`dtoverlay=imx219,cam1`两行都要写。  
若使用其他型号摄像头，将imx219替换掉即可，如**dtoverlay=ov5647,cam1**。  
![](/common/img/raspberryPi/dtoverlay=.png)

重启树莓派：`sudo reboot`。

## 测试

### 使用摄像头

```shell
rpicam-hello  # 约5秒的预览窗口
rpicam-hello -t 0  # 一直开启预览窗口（无限期运行进行预览）
rpicam-hello -t 1000  # 开启1000毫秒
rpicam-hello -t 10s   # 开启10秒
rpicam-hello -t 10s --camera 0  # 指定使用的摄像头，从0开始编号。若只有一个摄像头，那它的编号就为0

rpicam-vid  # 约5秒的预览窗口

rpicam-raw  # 约5秒 不显示预览窗口

rpicam-still  # 约5秒的预览窗口
rpicam-still -t 0

libcamera-hello -t 0 --camera 0  # libcamera-hello命令也可以
```

下面的命令开启5秒左右的预览窗口，然后拍摄一张全像素的jpeg图像，保存为test.jpg：
```shell
rpicam-jpeg -o test.jpg

libcamera-jpeg -o test.jpg
```

设置图像分辨率 和 浏览显示时间：
```shell
rpicam-jpeg -o test.jpg -t 2000 --width 640 --height 480
```

捕获（拍摄）视频：
```shell
rpicam-vid -t 10s -o test.h264  # 拍摄10秒视频，保存为文件test.h264
rpicam-vid -t 10s -o test.mp4   # 拍摄10秒视频，保存为文件test.mp4
```

rpicam-raw：
```shell
# 不显示预览窗口，直接从传感器将视频记录为原始的 Bayer帧
rpicam-raw -t 3000 -o test.raw  # 将三秒（3000毫秒）的原始剪辑记录到一个名为test.raw的文件中
```

rpicam-still：
```shell
# rpicam-still允许文件以多种不同的格式保存。
# 它支持png和bmp编码。它还允许将文件保存为 RGB 或 YUV 像素的二进制转储，没有编码或文件格式。
# 在后一种情况下，读取文件的应用程序必须了解其自身的像素排列。
rpicam-still -e png -o test.png
rpicam-still -e bmp -o test.bmp
rpicam-still -e rgb -o test.data
rpicam-still -e yuv420 -o test.data
```

运行命令时，若出现错误:
> what():  failed to import fd 29
{: .prompt-danger }

![](/common/img/raspberryPi/rpicam-fd.png)

可能是权限不足，可使用sudo，如:
```shell
sudo rpicam-hello -t 0
sudo libcamera-hello -t 0 --camera 0
```

### MobaXterm打开图片

MobaXterm打开图片报错：
![](/common/img/raspberryPi/Peas-notFound.png)

安装Peas：
```shell
sudo apt install libpeas-dev
```
![](/common/img/raspberryPi/Peas-install.png)

MobaXterm打开图片，不显示。
