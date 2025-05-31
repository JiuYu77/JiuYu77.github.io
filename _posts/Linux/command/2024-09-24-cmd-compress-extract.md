---
title: 压缩解压
description: 压缩解压，tar、gzip、bzip2、zip、unzip。
author: yu
date: 2024-09-24 14:03:34 +0800
categories: [Linux, 命令]
tags: [Linux, command]
---

## tar

### tar

tar常用参数：
|        参数       |             参数说明              |
|:-----------------:|:---------------------------------:|
|  z                |  通过gzip压缩或解压               |
|  x                |  解开tar包                        |
|  v                |  显示详细的tar命令执行过程        |
|  f                |  指定压缩文件名字                 |
|  C                |  指定解压的目录路径               |
|  c                |  创建新的tar包                    |
|  j                |  通过bzip2命令压缩或解压          |
|  t                |  不解压查看tar包的内容            |
|  p                |  保持文件的原有属性               |
| --exclude=PATTERN |  打包时排除不需要处理的文件或目录 |
| -h                |  打包软连接文件指向的真实源文件   |
| -hard-dereference |  打包硬链接文件                   |

查看压缩包的内容，不解压：
```bash
tar -tf hello.tar.gz
```
压缩，打包为.tar：
```shell
tar -cf all.tar *.jpg
```
更新：
```shell
# 更新原来 tar 包 all.tar 中的 logo.gif 文件
tar -uf all.tar logo.gif
```

**解压**：tar`自动检测`压缩文件使用的算法并自动解压文件。可以使用`tar -xf`命令来解压使用其他压缩算法的tar存档文件。例如.tar.gz文件、.tar.bz文件、tar.xz文件等。
```shell
# 自动检测压缩算法
tar -xf 压缩包
tar -xvf 压缩包
tar -xvf 压缩包 -C 指定目录  # 解压到指定目录

# 提取tar.xz
tar -xvJf test.tar.xz
```

### tar调用gzip

解压tar.gz：
```shell
# 解压到当前目录
tar -xzf hello.tar.gz
tar -xzvf hello.tar.gz
# 解压到指定目录
tar -zxvf hello.tar.gz -C /home/test
```
压缩：
```shell
# 当前目录
tar -czf all.tar.gz 要打包的目录或文件
示例：tar -czf all.tar.gz *.jpg aa/
# 指定目录
tar -czf all.tar.gz -C path/ 要打包的目录或文件
示例：tar -czf all.tar.gz -C ~/project my_code/
```

### tar调用bzip2

解压tar.bz2
```shell
tar -xjf all.tar.bz2
```
tar调用bzip2打包
```shell
tar -cjf all.tar.bz2 *.jpg
```

## zip

查看压缩包内容
```shell
zip -sf a.zip
```
压缩.zip文件
```shell
# 打包：当前路径
zip 压缩包名.zip 文件1 文件夹1

# 打包：其他路径；-r 递归处理，将指定目录下的所有文件和子目录一并处理
zip 压缩包名.zip 路径/文件
zip -r 压缩包名.zip 路径/文件夹 路径/文件
```

## unzip

unzip命令用于解压`.zip`压缩包。

安装unzip
```shell
sudo apt-get install unzip
```
`-l`：仅查看压缩文件包里的内容，而不进行解压缩
```shell
unzip -l hello.zip
```

解压
```shell
# 解压文件到当前目录
unzip hello.zip

# -d：解压文件到指定目录
unzip hello.zip -d 目标目录

# -q：静默模式，即解压时不显示详细解压过程。
# -t：检查压缩文件是否正确。使用该选项可以验证压缩文件的完整性，但不会显示详细解压过程。
```


