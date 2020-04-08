---
title: 从 Git 到 GitHub
author: 陈天傲
---

笔者本来想直接写一篇 GitHub 不完全指南，但总觉得要说 GitHub 还是要从 Git 说起。

根据 [Git 官网](https://git-scm.com)的说法，Git 是一个 *free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency*（被设计用于快速高效处理从小型到非常大型的一切项目的自由且开源的分布式版本控制系统）。

这里先不去讨论「什么是 *free and open source*」、「*free* 究竟是自由还是免费」，我们只需要明确下什么是分布式版本控制系统。

## Git 与版本控制

版本控制是工程项目中常用的工程/项目管理手段，主要目的就是控制同一工程在不同时间节点的状态，方便归档、回溯、测试、回滚。这种控制通常通过保存完整的版本副本并记录版本和时间的关系来实现，在软件工程中的知名实现就有曾经大名鼎鼎的 Apache Subversion（常用缩写名 SVN）。而这种传统的版本控制手段（可以认为是中心化版本控制）无法高效处理针对项目不同部分乃至相同部分的异步修改——这对于多人协作的大型软件项目是极为不便的。因此，软件工程中出现了分布式版本控制方法。

分布式版本控制通过日志式、自动化分支管理、冲突处理等核心技术实现多分支、异步迭代等分布式特性，从而方便软件开发项目的协作。时下最为流行的**分布式版本控制系统**就是 Git，同时，最为流行的**版本控制系统**也是 Git。

## Git 的基本概念

Git 使用 repository 存储项目资源，每个项目对应一个 repository，初始化一个 Git 项目的第一步就是为其建立 repository。

Git 使用 pull 从远程 repository 向本地 repository 拉取资源；使用 push 从本地 repository 向远程 repository 推送资源；使用 commit 将对文件的修改提交到目标 branch。

Git 使用 branch 创建平行工作流，在被 merge 前，每个 branch 间都是相互独立的。Master branch 是第一个 branch，也是整个项目的主工作流。理想情况下其他 branch 最终都应 merge into master branch。

Git 使用 Pull Request 请求整个项目的其他协作者（但也可以包括自己）对 commit 进行回应，如果没有异议则可以执行 merge。

Git 使用 merge 合并 branch，如果自动化合并时出现冲突则转入冲突处理流程。

## Git 的应用

Git 作为一个源于软件工程领域的分布式版本控制系统，理论上可以用于任何数字化信息（也就是数据，通常以文件系统中的文件为载体）的版本控制，只是其工作原理决定了其在处理未知二进制信息（比如 PNG 图片）时效率很低。事实上，Git 也确实可以良好应用于软件工程之外的版本控制工作，一个知名的例子就是 GitBook。

Git 本身是一个开源项目，同时也被用来代指这个项目的可执行（二进制）发行版本。Git 的官方发行版是一个命令行程序，像其他命令行应用一样，可以直接使用 git 命令执行。

Git 作为一个分布式的版本控制系统，在工作时并不要求（要求，指必须满足的需求）一个专用的（dedicated）中心化服务器。也就是说，Git 终端之间可以以 peer-to-peer 的形式通过（完全或非完全）星型拓扑实现对等互访，不同终端间独立地、直接地交换版本迭代信息。但这样的局限显而易见——信息交换效率低下、repository 难以被集中管理。而一个中心化的 Git 服务器可以解决这些问题。

因此，使用一台中心化的 Git 服务器是 Git 在多人协作中的典型用法。

## Git 服务器与分布式

也许你会问：使用中心化服务器的 Git 还是分布式的吗，分布式版本控制的意义又何在？

诚然，在数据交换方式上，这已经不是分布式了。但这不妨碍「没有一个权威（且唯一，这里的唯一指的是 unique 而非副本的拷贝唯一）的中心化副本这条分布式版本控制的真理」，因为即使对于 master branch，在最后一条「commit 被 push」或「pull request 被 merge」之前，服务器也不知道真正的 master branch 是怎样的。换言之，Git 服务器无法实时知道 master branch 乃至 repository 的权威状态——因为没有权威——因此使用中心化 Git 服务器的 Git 系统依旧是分布式的。

那分布式版本控制的意义又何在呢？或者说，分布式版本控制相较于传统的中心化版本控制有哪些有些优势？其实如果只是说 Git 的优势，Wikipedia 和其他技术博客上大可找到一堆，且反之亦然。Git 因诸多硬核特性而风靡于大型项目和互联网公司，而 SVN 被认为具有更丰富的权限管理特性，同时相对友好易上手（一定程度上取决于 GUI 实现）。据不可靠消息，华为和 BAT 内部都有在使用 SVN 或魔改 SVN。但如果把这个问题扩大到中心化版本控制和分布式版本控制，笔者觉得自己对他们的理解可能还鞭长莫及。

Anyway，对于 Git 和分布式，笔者的个人看法是：

* 无论单说 SVN 和 Git 还是集中式与分布式版本控制，都很难评价孰优孰劣。

* Git 化的潮流确实势不可挡，但成就 Git 的是 Git 本身，而不是分布式。

对于这个问题，也许本站的另一位作者 Secret 君有兴趣谈一谈，也也许未来某天笔者会回来填坑……

## 基于 Git 的 SaaS

既然使用中心化的 Git 服务器来方便协作是合理的，那么在这个云时代，Git 当然也要上云。云主机和云平台还不够彻底，要云就云应用，因此有了基于 Git 的 SaaS（Software as a Service）——我们管这个叫 Git 托管。（至于什么是 SaaS，其实是个科技以换名为本的故事，以后有机会再聊。）

Git 托管的本质就是全权的 Git repository 托管，同时顺带提供身份验证、访问控制、审计、自动化流程、CI/CD 等必要/增值服务。

基于 Git 的 SaaS 服务商有很多，比如数据库翻车直播恢复的 GitLab、SVN 起家主打私有库的 Bitbucket、曾用名「码云」的 Gitee、Git/SVN 双修的 CODING，但最最最著名的，还是几乎成为了 Git 服务的代名词的 GitHub。

关于 GitHub 及其相关应用，请看 [GitHub 不完全指南](/github-guide)。

{% include ad.html %}
