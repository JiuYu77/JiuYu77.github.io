---
title: Git 简介
description: git 版本控制器。
author: yu
date: 2025-06-18 15:22:32 +0800
categories: [Blogging, git]
tags: [Git]
image:
  path: /common/posts/git/git-scm.png
  alt: git-scm
---

## 前言

**`Git` 是一个分布式版本控制系统。**

<a href="https://git-scm.com/" target="_blank">**Git 官网**</a>

我知道这个定义听起来有点技术化，我们来拆解一下：

1. **版本控制：**
- 想象一下你在写一份重要的文档（比如论文、报告、小说）。你会不断修改它。为了防止改错或者想找回之前的某个版本，你可能会手动保存成“报告_v1.docx”、“报告_v2.docx”、“报告_最终版.docx”、“报告_真的最终版.docx”……
- **Git 就是自动帮你做这件事的超级管家。** 它精确记录你对文件（通常是代码文件）所做的每一次修改（谁改的、什么时候改的、改了哪里、为什么改）。你可以随时回到任何一个历史版本，或者比较不同版本之间的差异。

2. **分布式：**
- 这是 Git 最强大的特点之一。传统的版本控制系统（如 SVN）通常有一个中央服务器存储所有历史版本。每个人的电脑（客户端）只保存当前正在工作的文件。
- **而 Git 是分布式的：** 每个参与项目开发的人的电脑上，都拥有**完整的项目仓库（包括所有历史记录和版本）** 的完整副本。
- **好处：**
  - **离线工作：** 你可以在没有网络连接的情况下提交更改、查看历史、创建分支等，等有网了再同步。
  - **速度快：** 几乎所有操作（查看历史、提交代码、比较差异）都在本地进行，非常快。
  - **健壮性高：** 每个人的电脑都是一个完整的备份。即使中央服务器挂了，也可以用任何一个人的本地仓库恢复。
  - **灵活的协作：** 开发者可以独立工作，然后通过各种方式（如推送到共享仓库如 GitHub, GitLab, Gitee）交换更改。


## Git 的核心价值

**历史追踪：** 完整记录项目从开始到现在的所有变化，随时可以“穿越”回去。

**协作开发：** 让多人同时在同一个项目上高效工作，管理各自和他人代码的合并。

**分支管理：** 这是 Git 的杀手级功能！你可以轻松创建代码的“平行宇宙”（分支），在新分支上开发新功能或修复 Bug，而不会影响主线（`main` 或 `master` 分支）。开发测试完成后，再把分支合并回主线。这极大地支持了现代敏捷开发流程。

**备份与恢复：** 你的代码仓库（尤其是推送到远程仓库后）就是一个强大的备份。误删了文件？改错了代码？轻松回滚到之前的版本。

**责任明晰：** 每一次修改都记录作者和时间，方便追查问题来源。


## Git vs. GitHub / GitLab / Gitee

**Git：** 是核心的版本控制工具软件，运行在你的本地电脑上，管理你的本地仓库。

**GitHub / GitLab / Gitee：** 是基于 Git 的在线代码托管服务平台。它们提供了一个中央服务器来存放 Git 仓库（称为远程仓库），并围绕 Git 添加了非常多的协作功能，比如：
- Web 界面查看代码、历史、差异。
- 问题跟踪（Issue Tracking / Bug 管理）。
- 拉取请求（Pull Request / Merge Request）代码审查流程。
- 项目管理、Wiki、持续集成/持续部署等。

**简单说：Git 是引擎，GitHub/GitLab/Gitee 是带车库、维修站、观众席的赛车场。**

## 总结一下

**Git 是什么？** 一个强大、免费、开源的分布式版本控制系统。

**它做什么？** 跟踪文件（主要是代码）的变化，记录历史，支持高效协作（尤其是通过分支），让你能安全地实验和回滚。

**为什么重要？** 现代软件开发（无论个人项目还是大型团队协作）的基石工具，没有它，高效、安全地管理代码变更几乎是不可能的。


Git 是程序员必备技能，虽然初学可能觉得概念有点多，但掌握基础后，你会发现它极大地提升了你的开发效率和代码管理能力！加油！



