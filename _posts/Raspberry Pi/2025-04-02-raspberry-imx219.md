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
![](/common/img/raspberryPi/imx219/connect-imx219.jpg)
_Raspberry Pi 5 与 imx219 连接示意图_

**注意**：一定要连接完好，接口处保持水平，不要倾斜。

### 配置

打开文件`/boot/config.txt`
```shell
sudo vim.tiny /boot/config.txt
```
若出现如下内容，则关闭此文件，并使用文件`/boot/firmware/config.txt`进行配置：
![](/common/img/raspberryPi/imx219/config.png)

打开`/boot/firmware/config.txt`：
```shell
sudo vim.tiny /boot/firmware/config.txt
```

修改两处，并保存退出：
1. 禁用摄像头自动检测：将`camera_auto_detect=1`改为`camera_auto_detect=0`。  
![](/common/img/raspberryPi/imx219/camera_auto_detect.png)
2. 文件最后添加，`dtoverlay=imx219,cam0` 或 `dtoverlay=imx219,cam1`。写入 cam0 还是 cam1，要与imx219连接的是 `CAM/DISP 0`（cam0） 还是 `CAM/DISP 1`（cam1）对应。  
如果装了`双摄像头`，`dtoverlay=imx219,cam0`和`dtoverlay=imx219,cam1`两行都要写。  
若使用其他型号摄像头，将imx219替换掉即可，如**dtoverlay=ov5647,cam1**。  
![](/common/img/raspberryPi/imx219/dtoverlay=.png)

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

下面的命令开启5秒左右的 预览窗口(preview window)，然后拍摄一张全像素的jpeg图像，保存为test.jpg：
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

![](/common/img/raspberryPi/imx219/rpicam-fd.png)

可能是权限不足，可使用`sudo`，如:
```shell
sudo rpicam-hello -t 0
sudo libcamera-hello -t 0 --camera 0
```

### MobaXterm打开图片

```shell
xdg-open test.jpg
open test.jpg
```

MobaXterm打开图片`报错`：
![](/common/img/raspberryPi/imx219/Peas-notFound.png)

解决方案是，安装Peas：
```shell
sudo apt install libpeas-dev
# 如果有 安装失败/找不到 的软件，则执行以下命令，然后再次执行以上命令
sudo apt update
```
![](/common/img/raspberryPi/imx219/Peas-install.png)


## MobaXterm X11-Forwarding不显示图像

MobaXterm ssh连接 树莓派5 后，使用`xdg-open` 或 `open`等命令打开图片，不显示图像。

### 检测MobaXterm 是否开启 X11转发

- 右键，选择 Edit session。
![](/common/img/MobaXterm/right-click.png)

- 点击 Advanced SSH settings，查看是否勾选 X11-Forwording，若没勾选，则勾选上。
![](/common/img/MobaXterm/session-settings.png)

- 点击 **OK** / **Cancel** / **X**，以关闭窗口。

### 检查服务器 基础 X11 转发是否正常

1. 验证 DISPLAY 变量  
ssh 连接到服务器（树莓派），执行以下命令：
```shell
echo $DISPLAY
```
- 正常应输出 localhost:10.0 或类似值。
- 如果为空，手动设置：
  ```shell
  export DISPLAY=localhost:10.0
  ```

2. 测试简单GUI程序  
- 对于GUI程序`xclock`、`xeyes`，若能转发其界面，说明 X11 基本功能正常。
- 若未安装上述程序，则使用如下命令安装：
  ```shell
  sudo apt install x11-apps
  ```

### 排查其他 GUI 程序无法显示的原因

1. 缺少 GUI 依赖库  
- 某些程序（如 gedit、firefox）需要额外的 GTK/Qt 库：
   ```shell
   # 安装常见 GUI 依赖，然后重新尝试运行目标程序（如 gedit）
   sudo apt update
   sudo apt install -y libgtk-3-0 libqt5gui5 libgl1-mesa-glx
   ```
- 其他 GUI 程序黑屏	缺少 OpenGL
   ```shell
   sudo apt install libgl1-mesa-glx
   ```

2. xdg-open 调用错误  
如果 `xdg-open test.jpg` 不显示图片，而是触发下载：
- 原因：`xdg-open` 默认调用了浏览器或下载工具，而非图片查看器。
- 解决方案：
  - 安装 feh 或 eog，feh 更轻量。
    ```shell
    sudo apt install feh  # 安装 feh（轻量图片查看器）
    feh test.jpg          # 用 feh 打开图片
    sudo apt install eog  # 安装 eog
    eog test.jpg
    ```
  - 如果 `feh` 能显示，说明 `xdg-open` 配置错误，可以修改默认程序：
    ```shell
    xdg-mime default feh.desktop image/jpeg
    xdg-mime default feh.desktop image/png
    ```

3. 程序自身限制（如 Firefox）  
某些程序（如 `firefox`）可能默认禁用远程 X11：
- 尝试强制允许 X11 转发：
    ```shell
    firefox --no-remote
    ```
- 如果仍然不行，改用 chromium：
    ```shell
    sudo apt install chromium-browser -y
    chromium-browser --no-sandbox
    ```

### 检查 SSH 服务端配置

确保树莓派的 `/etc/ssh/sshd_config` 允许 X11 转发：
```shell
sudo vim /etc/ssh/sshd_config
```
- 检查以下行是否启用：
  ```shell
  X11Forwarding yes
  ```
- 若未启用，则启用，并重启 SSH：
  ```shell
  sudo systemctl restart ssh
  ```

### 检查 X11 权限问题

1. ~/.Xauthority 权限
```shell
ls -la ~/.Xauthority
```
- 确保当前用户有读写权限（`-rw-------`）。
- 如果权限不对，修复：
```
chmod 600 ~/.Xauthority
```

2. 临时放宽 X11 权限（测试用）
```shell
xhost +
```
- **（注意：这会降低安全性，仅用于测试）**
- 然后再尝试运行目标程序。

### 替代方案

1. 使用 VNC 远程桌面
如果 X11 转发不稳定，改用 VNC：
```shell
sudo apt install realvnc-vnc-server -y
sudo raspi-config  # 启用 VNC
```
- 然后在 MobaXterm 中使用 VNC 客户端连接。

2. 直接传输文件到本地
- 使用scp
    ```shell
    scp 树莓派用户名@树莓派IP:/path/to/file.png .
    ```
- 在 MobaXterm 本地文件浏览器中双击打开。

