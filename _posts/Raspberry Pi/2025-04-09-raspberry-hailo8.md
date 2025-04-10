---
title: 树莓派5 使用 Hailo-8
description: 树莓派5，Hailo-8，NPU。
author: yu
date: 2025-04-09 22:01:48 +0800
categories: [Blogging, 树莓派]
tags: [Raspberry Pi, apt, Hailo-8]
---

## 简介

Hailo-8，是由以色列公司 Hailo 开发的一款高性能边缘 AI 处理器(NPU，Neural network Processing Unit)，是一款AI加速器(AI Accelerator)，它专为低功耗、高效率的深度学习推理任务设计。

- `Hailo-8 AI Accelerator`，官网描述摘录（截至2025-04-09）：The `Hailo-8 edge AI processor`, featuring up to 26 tera-operations per second (TOPS), significantly outperforms all other edge processors. 
- `Hailo-8L Entry-Level AI Accelerator`，官网描述摘录（截至2025-04-09）：The `Hailo-8L Entry-Level AI Accelerator`, featuring up to 13 tera-operations per second (TOPS), is designed to support entry level products requiring limited AI capacity or lower performance.

[hailo-ai GitHub](https://github.com/hailo-ai)

[hailo.ai 官网入口](https://hailo.ai/)  
[Hailo-8L Entry-Level AI Accelerator 官网入口](https://hailo.ai/products/ai-accelerators/hailo-8l-ai-accelerator-for-ai-light-applications/)  
[Hailo-8 AI Accelerator 官网入口](https://hailo.ai/products/ai-accelerators/hailo-8-ai-accelerator/)

>CPU，Central Processing Unit，中央处理单元（Unit 可翻译为 器 或 单元）  
 GPU，Graphics Processing Unit，图形处理单元  
 NPU，Neural Processing Unit，神经（网络）处理单元  
{: .prompt-tip }

[**树莓派-hailo官方文档 AI Kit and AI HAT+ software**](https://www.raspberrypi.com/documentation/computers/ai.html)

本文`树莓派5`使用的系统：**`Raspberry Pi OS (64-bit)`**。默认软件源：
![](/common/img/raspberryPi/Hailo-8/sources.list.png)
*/etc/apt/sources.list*
![](/common/img/raspberryPi/Hailo-8/sources.list.d-raspi.list.png)
*/etc/apt/sources.list.d/raspi.list*

是否修改了软件源：**否**。

## 安装摄像头

安装摄像头用于测试 `Hailo-8` NPU，可参考[树莓派5使用imx219摄像头](https://jiuyu77.github.io/posts/raspberry-imx219/)（也许会有帮助）。

## 硬件连接

1. 首先，确保您的Raspberry Pi运行最新的软件。运行以下命令进行更新：
    ```shell
    sudo apt update && sudo apt full-upgrade
    ```
    下图最后一行，选择`Y`：
    ![](/common/img/raspberryPi/Hailo-8/apt-update-upgrade.png)
2. 接下来，确保您的树莓派固件(Raspberry Pi firmware)是最新的。
- 运行以下命令以查看您正在运行的固件：
    ```shell
    sudo rpi-eeprom-update
    ```
    ![](/common/img/raspberryPi/Hailo-8/rpi-eeprom-update.png)
- 如果您看到2023年12月6日或更晚的日期，[请继续下一步](#section1)，上图时间为`2025-03-10`直接跳过此步。  
如果您看到的日期早于2023年12月6日，请运行以下命令以打开Raspberry Pi Configuration CLI：
    ```shell
    sudo raspi-config
    ```
    依次选择 `Advanced Options` > `Bootloader Version`，选择 `Latest`。然后， 通过 `Finish` 或 `Escape 按键` 退出`raspi-config`。
- 运行以下命令将固件更新到最新版本<a id="section1"></a>：
    ```shell
    sudo rpi-eeprom-update -a
    ```
- 重启`sudo reboot`或者直接 关闭`sudo poweroff`。
3. 断开树莓派电源。
4. 将 hailo-8 安装到树莓派上。
   - 注意 pcie 带状电缆(ribbon cable) 是`分正反`的，下面如果出现问题，可以尝试将带状电缆两头对调。
5. 插上电源，开机。

## 驱动&固件安装

0. 开启 PCIe Gen 3.0（可选）  
要启用PCIe Gen 3.0速度，请将以下行添加到`/boot/firmware/config.txt`的最下面：
    ```
    dtparam=pciex1_gen=3
    ```
    使用`sudo reboot`重新启动Raspberry Pi以使这些设置生效。
1. 安装使用NPU所需的依赖项。从终端窗口运行以下命令：
    ```shell
    sudo apt install hailo-all
    ```
    这将安装以下依赖项：
    - Hailo kernel device driver and firmware

    - HailoRT middleware software

    - Hailo Tappas core post-processing libraries

    - The rpicam-apps Hailo post-processing software demo stages
2. 使用`sudo reboot`重新启动Raspberry Pi，以使这些设置生效。
3. 要确保一切正常运行，请运行以下命令：
    ```shell
    hailortcli fw-control identify
    ```
    如果您看到类似以下的输出，则您已成功安装NPU及其软件依赖项：
    ```
    Executing on device: 0001:01:00.0
    Identifying board
    Control Protocol Version: 2
    Firmware Version: 4.20.0 (release,app,extended context switch buffer)
    Logger Version: 0
    Board Name: Hailo-8
    Device Architecture: HAILO8
    Serial Number: HLLWM2A230600778
    Part Number: HM218B1C2FAE
    Product Name: HAILO-8 AI ACC M.2 M KEY MODULE EXT TEMP
    ```
    此外，您还可以检查内核日志，确定是否成功：
    ```shell
    dmesg | grep -i hailo
    ```
    正常情况下，这应该会产生类似于以下内容的输出：
    ```
    [    2.349431] hailo: Init module. driver version 4.20.0
    [    2.349531] hailo 0001:01:00.0: Probing on: 1e60:2864...
    [    2.349536] hailo 0001:01:00.0: Probing: Allocate memory for device extension, 13184
    [    2.349553] hailo 0001:01:00.0: enabling device (0000 -> 0002)
    [    2.349557] hailo 0001:01:00.0: Probing: Device enabled
    [    2.349591] hailo 0001:01:00.0: Probing: mapped bar 0 - 00000000e8f521b9 16384
    [    2.349601] hailo 0001:01:00.0: Probing: mapped bar 2 - 000000001b1af6ce 4096
    [    2.349604] hailo 0001:01:00.0: Probing: mapped bar 4 - 00000000a2dd0d14 16384
    [    2.349607] hailo 0001:01:00.0: Probing: Force setting max_desc_page_size to 4096 (recommended value is 16384)
    [    2.349615] hailo 0001:01:00.0: Probing: Enabled 64 bit dma
    [    2.349618] hailo 0001:01:00.0: Probing: Using userspace allocated vdma buffers
    [    2.349620] hailo 0001:01:00.0: Disabling ASPM L0s
    [    2.349623] hailo 0001:01:00.0: Successfully disabled ASPM L0s
    [    2.349962] hailo 0001:01:00.0: Writing file hailo/hailo8_fw.bin
    [    2.438985] hailo 0001:01:00.0: File hailo/hailo8_fw.bin written successfully
    [    2.438992] hailo 0001:01:00.0: Writing file hailo/hailo8_board_cfg.bin
    [    2.439015] Failed to write file hailo/hailo8_board_cfg.bin
    [    2.439017] hailo 0001:01:00.0: File hailo/hailo8_board_cfg.bin written successfully
    [    2.439019] hailo 0001:01:00.0: Writing file hailo/hailo8_fw_cfg.bin
    [    2.439026] Failed to write file hailo/hailo8_fw_cfg.bin
    [    2.439027] hailo 0001:01:00.0: File hailo/hailo8_fw_cfg.bin written successfully
    [    2.538289] hailo 0001:01:00.0: NNC Firmware loaded successfully
    [    2.538302] hailo 0001:01:00.0: FW loaded, took 188 ms
    [    2.549775] hailo 0001:01:00.0: Probing: Added board 1e60-2864, /dev/hailo0
    ```

## 可能遇到的问题

运行命令`hailortcli fw-control identify`出现以下错误：
```
[HailoRT] [error] Can't find hailort driver class. Can happen if the driver is not installed, if the kernel was updated or on some driver failure (then read driver dmesg log)
[HailoRT] [error] CHECK_SUCCESS failed with status=HAILO_DRIVER_NOT_INSTALLED(64) - Failed listing hailo devices
[HailoRT] [error] CHECK_SUCCESS failed with status=HAILO_DRIVER_NOT_INSTALLED(64)
[HailoRT] [error] CHECK_SUCCESS failed with status=HAILO_DRIVER_NOT_INSTALLED(64)
[HailoRT] [error] CHECK_SUCCESS failed with status=HAILO_DRIVER_NOT_INSTALLED(64)
[HailoRT] [error] CHECK_SUCCESS failed with status=HAILO_DRIVER_NOT_INSTALLED(64)
[HailoRT CLI] [error] CHECK_SUCCESS failed with status=HAILO_DRIVER_NOT_INSTALLED(64)
```
通常是 `Hailo-8` 和 `PCIe TO M.2 HAT+`外设 未连接完好，建议重点检查`带状电缆(ribbon cable)`。

## 查看设备识别

查看是否识别 `hailo-8`设备：
```shell
lspci -v | grep -i hailo
```
确认 PCIe 设备已被系统识别（**需 Hailo 硬件已正确插入**，尤其注意`带状电缆`）。

## 关于 hailo PCIe 驱动（若有需要可使用）

本文PCIe即pcie。

### 驱动 加载&卸载

- 加载 hailo PCIe 设备驱动
  ```shell
  sudo modprobe hailo_pci
  sudo modprobe hailo_pci debug=1 # 加载驱动并输出调试信息
  ```
  日志可通过 `dmesg` 查看。
- 卸载驱动（例如需要重新加载）
  ```shell
  sudo modprobe -r hailo_pci
  ```
- 开机自动加载：  
将模块名添加到 `/etc/modules-load.d/hailo.conf`（需 root 权限）。

### 验证PCIe驱动是否加载成功

检查内核模块：
```shell
lsmod | grep hailo_pci
```
若输出中包含 hailo_pci，则驱动已加载。

## Demo

相机应用程序的`rpicam-apps`套件实现了一个后处理框架。rpicam-apps包括多个应用程序：rpicam-hello、rpicam-jpeg、rpicam-raw、rpicam-still、rpicam-vid。

通过 `rpicam-apps`自带的demo(示例) 测试 `Hailo-8人工智能加速器` 是否真的可用。

运行以下命令以安装最新的rpicam-apps软件包：
```shell
sudo apt update && sudo apt install rpicam-apps
```

