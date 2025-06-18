---
title: Git 安装
description: 不同平台安装 git。
author: yu
date: 2025-06-18 16:06:29 +0800
categories: [Blogging, git]
tags: [Git]
---


## Linux/Unix 安装 git

如果你想在Linux上⽤⼆进制安装程序（一种命令、软件）来安装基本的Git⼯具，可以使⽤发⾏版包含的基础软件包管理⼯具来安装。

基于Debian的发⾏版上，如Ubuntu，请使⽤ `apt`：

git (标准 Git 包)
```shell
sudo apt install git
```
git-all (Git 全家桶包)
```shell
sudo apt install git-all
```
通常，安装 `git` 就足够了。

**更详细内容，请移步：**
<a href="https://git-scm.com/downloads/linux" target="_blank">Git 下载地址（Linux/Unix）</a>

**`git` 和 `git-all`:**
1. **`git` (标准 Git 包)**
- **这是最常用、最核心的包。**
- 它包含了 Git 的**基本命令行工具和核心功能**，足以满足绝大多数日常开发需求。
- 安装这个包后，你就可以使用所有基础的 Git 命令：
  - git init, git clone, git add, git commit, git push, git pull, git branch, git checkout, git merge, git log, git diff, git status 等等。
  - 进行本地版本管理。
  - 与远程仓库（如 GitHub, GitLab, Gitee）进行交互（推送、拉取）。
  - 分支管理、合并、解决冲突等核心协作功能。

2. **`git-all` (Git 全家桶包)**
- 这个包是一个**元包**或**虚拟包**。它本身并不包含具体的软件文件，而是**依赖于其他一系列包含 Git 附加工具和功能的子包**。
- 安装 `git-all` 相当于一次性安装了 **`git` 核心包** + **所有可选的 Git 相关工具**。它包含的内容通常有：
  - `git` (核心命令行工具)
  - `gitk` 和 `git-gui`：图形化的仓库浏览器和提交工具。gitk 主要用于查看历史记录，git gui 提供了一个图形界面进行提交等操作。
  - `git-email`：提供通过电子邮件发送和接收补丁的功能。
  - `git-svn`：允许你与 Subversion (SVN) 仓库交互（作为 Git 客户端访问 SVN 服务器）。
  - `git-cvs`：允许你与 CVS 仓库交互（较少用）。
  - `git-mediawiki`：与 MediaWiki 交互。
  - 其他可能的子模块或辅助工具（具体取决于发行版的打包方式）。

## Windows 安装 git

Windows系统安装Git，可以 下载 `.exe` 安装包 或 `命令行`安装。

请移步 Git官网 Windows下载页面：
<a href="https://git-scm.com/downloads/win" target="_blank">Git 下载地址（Windows）</a>
，要注意这是⼀个名为 `Git for Windows` 的项⽬（也叫做msysGit），和Git是分别独⽴的项⽬。

<a href="https://git-for-windows.github.io/" target="_blank">Git for Windows 的 GitHub Pages</a>

<a href="https://github.com/git-for-windows/git" target="_blank">Git for Windows 的 git仓库</a>

## macOS 安装 git

**更详细内容，请移步：**
<a href="https://git-scm.com/downloads/mac" target="_blank">Git 下载地址（macOS）</a>

## 查看git版本

```shell
git --version
```

