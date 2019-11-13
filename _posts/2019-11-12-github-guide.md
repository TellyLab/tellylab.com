---
layout: post
title: GitHub 不完全指南
author: Telly Chen 陈天傲
categories: [blog]
tags: [git, github, workflow]
---

我们常说的 GitHub 是指 GitHub, Inc.，一家位于美国的私人公司，于 2018 年被 Microsoft Corporation 收购。其持有 [GitHub.com](https://github.com) 域名（域名是大小写不敏感的），但不以此为唯一对外开展业务的域名。

以下是关于 GitHub 的**不完全**指南。

## GitHub 账户

想要注册 GitHub 账户，需要选择一个 GitHub 账户计划。

根据 [GitHub 的官方说法](https://help.github.com/en/github/getting-started-with-github/githubs-products)，GitHub 有多种产品，其实就是不同的的账户计划。按照用途可以分为个人计划和团队计划，目前个人计划分为免费计划和每月 7 刀的专业计划，免费计划可以几乎无条件但有限制地使用。

除了以上提到的几种账户类型外，GitHub 还有一个组织（organization）的概念。组织像其他用户账户一样，拥有仓库（repositories）、项目（projects）、包（packages），也拥有和其他账户统一的用户名命名空间（也就是说组织名和用户名不能重复），还拥有一个和个人帐户不太一样的门户页（例如本站的[门户页](https://github.com/Tianao)）。但组织不被视为一个账户，组织不能直接注册，亦不能直接使用组织名登录到 GitHub。想要使用组织身份，需要首先拥有一个普通账户，然后通过普通账户新建组织。想要新建一个组织，需要选择一个团队计划，GitHub 对于开源团队使用的的团队计划是免费的。

使用组织有很多好处，即使是在规模不大的团队中，也可以体会到组织在去中心化和权限管理方面的优势，从而让一个团队真正以组织的模式运行——这是 GitHub 基于 Git 而高于 Git 的地方之一，也是笔者在[从 Git 到 GitHub](/from-git-to-github.html) 中提到的基于 Git 的 SaaS 的增值服务部分。

## Hello World

Hello World 是一个经典的计算机编程入门项目。在 GitHub，同样有一篇名为 [Hello World](https://guides.github.com/activities/hello-world/) 的入门指南。

但由于正在阅读的本文是一篇不完全指南，笔者不会逐句翻译这篇官方指南。

## 访问 GitHub 仓库

本章将通过介绍几种用户访问 GitHub 仓库的方式，引出一些 Git 的基本概念，然后讲述 GitHub 的基本操作。

在 Git 中，为一个项目集中存放资源的地方叫作仓库（repository）。

### 使用 Web 访问 GitHub.com

访问 GitHub 仓库最直观的方式就是通过 Web 浏览器访问 https://github.com/<用户名>/<仓库名> ，例如：

[https://github.com/TellyLab/tellylab.com](https://github.com/TellyLab/tellylab.com) 

其中 TellyLab 是本站在 GitHub 的组织名，tellylab.com 是本站的项目仓库名。

或者，你可以从 [GitHub.com](https://github.com) 的任意页面一步一步点进去。

这种方式的特点在于用户访问的仓库副本是直接存储在 GitHub 上的那份（而不是用户终端设备本地的仓库副本），因此对仓库中分支（branch）的任何 commit 都会立即、直接在 GitHub 上生效（这听起来很符合直觉，但下面会解释为什么不是这样）。

#### Git 仓库与仓库副本

用于同一个项目的所有同名/同源 Git 仓库都被视为同一个 Git 仓库，但同一个 Git 仓库在不同时间和/或不同物理位置的不同拷贝（哪怕内容事实上相同）被称为同一个 Git 仓库的不同副本，简称仓库副本。

#### 分支

Git 通过分支机制保证多条工作流在并行、异步推进时的相互独立。

分支就像是平行宇宙，不同的分支对用户展示为不同的工作时空。具体来讲，不同的工作分支对用户展示为工作目录的不同副本，用户在 A 分支下对 d 目录中 f 文件所做的任何修改都不会体现在 B 分支下 d 目录中的 f 文件身上——除非~~时空错乱~~进行分支合并。

Master branch 是新仓库建立时的唯一分支，也是一个新仓库副本的默认工作分支。顾名思义，master branch 是一个项目的主工作流。Master branch 常简称为 master。

#### Commit

在 Git 中，对分支提交修改的行为/动作被称作 commit，因此 commit 既是一个名词也是一个动词。我们可以说：commit 一下、这次 commit、那些 commits。

Commit 是对工作分支进行的，因此任何 commit 都属于一个 branch。

### 使用 GitHub Desktop

[GitHub Desktop](https://desktop.github.com), GitHub 官方桌面客户端，~~安全 稳定 声誉，~~目前支持 macOS 和 Windows，Linux 用户也懒得用对吧。

作为桌面客户端，用户无需使用命令行就可以使用 Git 的基本功能，对于不想安装 git 环境、不想使用 git 命令的用户是个简单而不强大的选择。支持通过应用内登录 GitHub.com 账户、应用内登录 GitHub 企业服务器、直接输入 URL 三种方式克隆远程仓库。

#### Clone

在 Git 中，访问远程仓库并下载远程仓库副本，然后为仓库创建本地副本的操作被称为克隆（clone）。

克隆的意义在于通过对本地文件系统的直接读写实现对仓库副本中分支的读写，从而间接实现对远程仓库中分支的读写。但是用户本地文件系统中的目录（也就是用户在操作系统中可以直接查看的那个目录）比仓库中的目录要低一个维度，因为仓库中的目录同时具有目录位置和当前分支两个属性。

为了完成仓库目录这个高维度目录到本地文件系统目录这个低维度目录的映射，在建立本地仓库副本时，Git 程序会首先将本地仓库副本的工作分支设为 master 分支，确定的工作分支可以将仓库目录降维打击成适用于本地文件系统的工作目录，当用户切换当前工作分支时，Git 将使用新的工作分支-仓库目录组合刷新工作目录。

因此，克隆后的本地仓库副本默认工作在 master 分支。

本地工作目录就是本地仓库副本在当前工作分支分支下的目录。不同的分支决定了不同的工作目录，因此用户在访问工作目录前需要选择正确的工作分支。

在本地使用任何方式（无论是文本编辑器、IDE，还是二进制、十六进制文件编辑器，甚至 Photoshop、数字照相机）对工作目录进行修改（对工作目录内的目录/文件进行增删改操作都属于对工作目录的修改），即可对本地仓库副本的当前工作分支实现修改。

接下来就可以在 GitHub Desktop 上执行 commit 操作，向 Git 程序（或者说向记录 commit 历史的数据库）提交这次修改了。事实上，GitHub Desktop 一直在（准实时地）监视工作目录的修改，但只有在用户执行 commit 后，Git 程序才认为用户证实了这是一次对分支的修改，从而在 commit 历史中将其记录。

Git 允许同一个仓库在不同终端上的不同副本异步地执行 commit，为了在 commit 之后令不同仓库副本保持同步，用户需要手动执行一些操作。

#### Push

对于 GitHub，任何用户终端上的仓库都只和 GitHub 服务器上的仓库进行同步。因此，在用户终端（比如 GitHub Desktop）上进行 commit 后，需要执行 push 操作将 commit 记录同步至 GitHub。

#### Fetch

在 Git 终端上，如果用户想要知道当前的本地仓库副本与远程仓库副本有哪些不同，可以执行 fetch 操作。这一操作将从远程仓库下载所有 commit 记录，但是无论是否有新的 commit，fetch 都不会对本地仓库副本进行任何修改。因此，用户可以随时随便随意 fetch。事实上，GitHub Desktop 也会每隔一段时间自动执行 fetch，但这不妨碍用户在进行任何 Git 操作前手动执行一次（或多次）fetch 操作，以确保进行接下来的操作的决定是正确的。

在任何时候，GitHub Desktop 都将准实时地显示当前工作目录与本地仓库副本中最新一次 commit 记录的差异。

差异以两者中较新的状态相较于较老的状态的变化为正方向，绿色代表增加、红色代表删除、黄色代表修改。

#### Merge

一旦出现异步修改不同分支副本导致的分支分歧，用户便可以选择合并这些分支副本。如果用户希望将远程分支合并进本地分支，则可以执行 merge。

#### Pull

如果没有分支分歧，但远程分支副本确有更新，则可以执行 pull，使用远程分支副本刷新本地分支副本。Pull 相当于先 fetch 再 merge 的组合。

### 使用 Git 接口

Git 是 GitHub 的基础，GitHub 允许用户通过标准的 Git 接口访问 GitHub 中的仓库（更多关于 Git 请看[从 Git 到 GitHub](/from-git-to-github.html)），因此用户可以使用任何 Git 兼容程序——包括 标准的 CLI、GUI 发行版、内建 Git 支持的 IDE 等访问 GitHub 仓库。

（未完待续）
