---
title: Linux 环境变量
description: Linux，环境变量。
author: yu
date: 2025-06-01 15:16:40 +0800
categories: [Linux, Linux系统-进阶]
tags: [Linux, command]
---


## 环境变量的含义

`环境变量`（environment variables）一般是指在操作系统中用来指定操作系统命令和应用程序运行环境的一些参数，如：临时文件夹位置和系统文件夹位置等。

环境变量的**本质**是操作系统或特定程序（如Shell）维护的**键值对**。

程序（操作系统命令和应用程序）的执行都需要运行环境，这个环境是由多个环境变量组成的。

因此，环境变量的**作用** 是为**正在运行的程序**（包括操作系统命令、应用程序、脚本）提供关于**当前运行环境**的关键配置信息。
使得程序能够：
- **动态适应环境**：程序无需硬编码特定路径或设置，通过读取环境变量即可知道在哪里找文件（如`PATH`）、使用什么语言（如`LANG`）、用户主目录在哪（如`HOME`）、临时文件放哪（如`TMPDIR`或`TEMP`）等。
- **配置程序行为**：开发者可以通过设置特定的环境变量来改变程序的功能或输出（例如，`DEBUG=1`让程序输出调试信息，`EDITOR=vim`指定默认文本编辑器）。
- **进程间传递信息**：父进程（如Shell）设置的环境变量会被其启动的子进程`继承`。

`Linux环境变量`也称`Shell环境变量`，由下划线、大写字母、小写字母和数字组成，以下划线、大写字母或小写字母打头，习惯上使用大写字母，例如PATH、HOSTNAME、LANG等，**也可以**使用小写字母。

## 环境变量的分类

### 按生效的范围分类

1. **系统环境变量**
- 对整个操作系统中**所有用户**都生效。
- 通常由系统管理员设置，存储在系统级别的配置文件（如Linux的`/etc/environment`, `/etc/profile`, `/etc/profile.d/*`；Windows的系统属性）。
- 影响范围最广。
2. **用户环境变量**
- 仅对**特定用户**生效。
- 存储在用户主目录下的配置文件（如Linux的`~/.bashrc`, `~/.bash_profile`, `~/.profile`；Windows的用户环境变量设置）。
- 允许用户自定义个人环境（如设置个人`PATH`添加私有工具目录）。

### 按生存周期分类

1. **永久环境变量**
- 通过修改**配置文件**（如上所述的系统或用户配置文件）来设置。
- 在**系统重启**或**用户重新登录**后依然有效。
- 是配置环境变量的主要方式。
2. **临时环境变量**
- 仅在**当前Shell会话**或**当前命令行窗口**中有效。
- 通过命令行直接设置（如Linux/Unix: `export VARIABLE=value`；Windows CMD: `set VARIABLE=value`；Windows PowerShell: `$env:VARIABLE = "value"`）。
- 关闭终端窗口或退出Shell会话后，该变量即失效。
- 常用于临时测试或运行一次性脚本。

## Linux常用环境变量

### PATH

`PATH`定义Shell搜索可执行文件的目录列表。是**最核心**的环境变量。

`PATH`是可执行程序（Linux系统命令和用户的应用程序）的搜索目录。

如果可执行程序的目录**不在**PATH指定的目录中，执行时需要**指定目录**。

1. PATH环境变量存放的是目录列表，目录之间用冒号`:`分隔，最后的点表示当前目录。示例：
```shell
export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin
export PATH=$PATH:/usr/bin
```
如果PATH变量中没有包含点`.`，那么执行当前目录下的程序需要加`./`或使用`绝对路径`。<br/>
**注意**：要不要加点视系统而定。我在Ubuntu20.04试了一下，不用加点也可以直接执行程序（不需要./或绝对路径）
2. PATH缺省(默认)包含了**Linux系统命令所在的目录**，如果不包含这些目录，Linux的常用命令将无法执行(除非使用绝对路径)。
3. 在用户的`.bashrc`、`.bash_profile` 或 `.profile` 文件，可以对`PATH`进行扩充（配置、修改）。

### HOME

`HOME`是当前用户的主目录路径。

### USER / LOGNAME

`USER` 和 `LOGNAME` 表示当前用户名。

### HOSTNAME

服务器的主机名。

### PWD

当前工作目录。

### HISTSIZE

保存历史命令的数目。

### SHELL

用户当前使用的shell解析器。

### LANG 和 LC_\*

定义系统语言和本地化设置（字符集、日期格式等）。

### LD_LIBRARY_PATH

C/C++语言**动态链接库**文件的搜索目录，它不是Linux缺省的环境变量(系统默认没有设置LD_LIBRARY_PATH变量的值)，对C/C++程序员来说非常重要。

LD_LIBRARY_PATH环境变量存放的是目录列表，与`PATH`变量格式相同。

### TERM

终端类型。

### PS1

定义Shell命令提示符的格式。

### EDITOR / VISUAL

默认的文本编辑器（如vim, nano）。

### TMPDIR

临时文件目录（通常默认为`/tmp`）。

### CLASSPATH

JAVA语言库文件的搜索目录，它不是Linux缺省的环境变量(系统默认没有设置CLASSPATH变量的值)，对Java程序员来说非常重要。

CLASSPATH环境变量存放的是目录列表，与PATH变量格式相同。

## 查看Linux环境变量

查看所有环境变量
```shell
env
printenv

# 匹配部分环境变量
env | grep 环境变量名
```

查看特定变量
```shell
echo $VARIABLE_NAME
echo ${环境变量名}
```

## 设置环境变量

格式：
```shell
# 格式1
变量名='变量值'
export 变量名

# 格式2
export 变量名='变量值'

# 格式3：增加变量值，以 PATH 为例
export PATH=$PATH:~/
```
如果环境变量的值**没有**空格等**特殊符号**，**可以省略**单引号/双引号。

### 临时生效

直接在terminal使用`export`设置环境变量，退出Shell就会失效，示例：
```shell
export Var=JiuYu.123
export PATH=/home/jiuyu/myDocuments:$PATH
```

### 永久生效

要使配置的环境变量`永久生效`，应该在`脚本文件`中配置。

#### 环境变量脚本文件的执行顺序

`/etc/profile` &rightarrow; `/etc/profile.d/`目录下的脚本文件 -> `/etc/bash.bashrc` -> 用户的`.bash_profile`、`.bash_login` 或 `.profile`文件其中一个 -> 用户的`.bashrc`。

#### 用户环境变量

用户环境变量只对当前用户生效。

默认情况下，用户主目录不一定存在`~/.bash_profile`文件；存在.bash_profile 或 `~/.bash_login`文件时，bash**不会读取**`~/.profile`文件。

Linux启动时会执行，`~/.bash_profile`、`~/.bash_login`、`~/.profile`，三者中的一个。

执行优先级：
```shell
~/.bash_profile  >  ~/.bash_login  >  ~/.profile
# 这三个文件，不一定都有，有的系统只有~/.profile文件
```

每次打开新的 terminal/shell 时，会被读取的文件：
```shell
~/.bashrc
~/.profile
```

`~/.bash_logout`：每次退出系统（退出bash shell）时执行该文件。

`~/.bash_history`：保存了当前用户使用过的历史命令。


**配置用户环境变量**

1. 在`.bash_profile`或`.profile`中配置环境变量（**推荐**）<br/>
若存在.bash_profile文件，则优先在此文件中设置环境变量；否则就在.profile文件中设置环境变量
2. `~/.bashrc`中配置<br/>
当用户登录时以及每次打开新的terminal/shell时该文件被读取，不推荐在.bashrc文件配置用户环境变量，因为每打开一个terminal，该文件都会被读取一次，效率肯定受影响。
3. 以 ~/.bashrc 为例
- 打开文件 ~/.bashrc
```shell
vim ~/.bashrc
```
- 在文件最下面，输入配置信息
```shell
export PATH=$PATH:/usr/local/cuda-11.6/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-11.6/lib64
```
- 也可以定义中间变量
```shell
CUDA_HOME=/usr/local/cuda-11.6
export PATH=$PATH:$CUDA_HOME/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CUDA_HOME/lib64
```
- 编辑完成后，保存退出。
- 使环境变量生效
```shell
source ~/.bashrc
```
4. 在 ~/.profile 中配置
- 同 ~/.bashrc 配置步骤
- 只有最后一步不同：
```shell
source ~/.profile
```

#### 系统环境变量

1. 在/etc/profile文件中设置<br/>
用户登录时系统会执行`/etc/profile`文件设置系统的环境变量。但是，Linux**不建议**在/etc/profile文件中设置系统环境变量。
2. 在/etc/profile.d目录中设置（**推荐**）<br/>
/etc/profile在每次启动时会执行`/etc/profile.d/`目录下的全部脚本文件。/etc/profile.d比/etc/profile好维护，不想要什么环境变量**直接删除**/etc/profile.d目录下对应的**脚本(.sh)文件**即可。
3. 在/etc/bash.bashrc文件中设置<br/>
有的系统是/etc/bashrc文件。<br/>
/etc/profile在每次启动时会执行`/etc/bash.bashrc`文件。<br/>
该文件配置的环境变量将会影响**全部用户**使用的bash shell，Linux不建议在/etc/bash.bashrc。
4. 以在 **/etc/profile.d/** 中配置，为例
- 新建一个.sh文件，以 `clangd` 为例
```shell
sudo vim /etc/profile.d/clangd.sh
```
- 输入配置信息
```shell
export Clangd_PATH=/home/nk/my_install/clangd_16.0.2/bin
export PATH=$Clangd_PATH:$PATH
```
- 编辑完成后，保存退出。
- 使环境变量生效
```shell
source /etc/profile
```
- 使环境变量永久生效：重启计算机


## 环境变量的生效

1. 在Shell下
用`export`设置的环境变量对当前shell **立即生效**，退出终端后失效。
2. 在脚本文件中
- 方法1：`source`
  - 用source命令让新设置的环境变量立即生效，如`source /etc/profile`
  - source只能让新设置的环境变量在当前terminal下生效，其他的terminal要想生效，也要执行source命令
  - source命令：设置新的环境变量并source后，再将该变量删除，再source，这个新的环境变量**仍然存在**，并没有被删除
- 方法2：对于`~/.bashrc`中配置环境变量，打开一个新的Shell
- 方法3：重启计算机。
设置的永久生效环境变量，重启计算机后再任何终端都有效。
重启计算机是最有效的方法。
- 方法4：注销，重新登陆。
重新登陆后再任何终端都生效。

