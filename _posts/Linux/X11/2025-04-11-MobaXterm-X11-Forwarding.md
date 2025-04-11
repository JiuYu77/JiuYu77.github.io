---
title: MobaXterm X11-Forwarding 不显示图像
description: 针对MobaXterm不显示X11转发的图像，本文提供了几种可能的解决方案。
author: yu
date: 2025-04-11 12:55:51 +0800
categories: [Linux, X11]
tags: [Linux]
---


## MobaXterm X11-Forwarding不显示图像

若您遇到 MobaXterm的X11-Forwarding（X11转发）不显示图像的问题，可查看本文针对不同情况的解决方案，可优先尝试最有可能的解决方法。

例如，MobaXterm ssh连接 树莓派5 后，**MobaXterm显示X11-Forwarding配置正常**，但使用`xdg-open` 或 `open`等命令打开图片，却不显示图像，可优先尝试**xdg-open 调用错误**相关内容。

可以尝试的方法，请见下文。

## 检测MobaXterm 是否开启 X11转发

- 右键，选择 Edit session。
![](/common/img/MobaXterm/right-click.png)

- 点击 Advanced SSH settings，查看是否勾选 X11-Forwording，若没勾选，则勾选上。
![](/common/img/MobaXterm/session-settings.png)

- 点击 **OK** / **Cancel** / **X**，以关闭窗口。

## 检查服务器 基础 X11 转发是否正常

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

## 排查其他 GUI 程序无法显示的原因

### 缺少 GUI 依赖库

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

### xdg-open 调用错误

如果 `xdg-open test.jpg`、`open test.jpg`等命令 不显示图片，而是触发下载：
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

### 程序自身限制（如 Firefox）

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

## 检查 SSH 服务端配置

确保服务端的 `/etc/ssh/sshd_config`文件 允许 X11 转发：
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

## 检查 X11 权限问题

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

3. `xauth` 主要用于生成、添加、删除和列出 X 服务器的授权文件（通常是 `~/.Xauthority`），这些文件包含了访问 X 服务器所需的授权信息。
- 列出授权：使用 `xauth list` 命令列出授权文件中的所有授权信息。
- 生成授权文件：使用 `xauth generate` 命令生成新的授权文件。
- 添加授权：使用 `xauth add` 命令向授权文件中添加新的授权信息。
- 删除授权：使用 `xauth remove` 命令从授权文件中删除授权信息。

## 替代方案

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

