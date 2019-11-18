---
layout: post
title: GitHub 不完全指南
author: Telly Chen 陈天傲
categories: [blog]
tags: [git, github, workflow]
---

我们常说的 GitHub 是指 GitHub, Inc.，一家位于美国的私人公司，于 2018 年被 Microsoft Corporation 收购。其持有 [GitHub.com](https://GitHub.com) 域名（域名是大小写不敏感的），但不以此为唯一对外开展业务的域名。

GitHub 的主要业务是提供 Git 相关的 SaaS（软件即服务），其中最为大众所熟知且常用的就是位于 GitHub.com 的完全 Git 托管服务。因此，我们常说的 GitHub 特指 GitHub.com。

以下是关于 GitHub 的**不完全**指南。

## GitHub 账户

在注册 GitHub 账户时，需要选择一个 GitHub 账户计划。

根据 [GitHub 官方说法](https://help.github.com/en/github/getting-started-with-github/githubs-products)，GitHub（特指 GitHub.com）有多种产品，其实就是 GitHub.com 的不同账户计划。按照用途可以分为个人计划和团队计划，目前个人计划分为免费计划和每月 7 刀的专业计划，免费计划可以几乎无条件但有限制地使用。

除了以上提到的几种账户类型外，GitHub 还有一个组织（organization）的概念。组织像其他用户账户一样，拥有仓库（repositories）、项目（projects）、包（packages），也拥有和其他账户统一的用户名命名空间（也就是说组织名和用户名不能重复），此外还拥有一个和个人帐户不太一样的门户页（例如[本站的门户页](https://github.com/Tianao)）。但组织不被视为一个账户，组织不能直接注册，亦不能直接使用组织名登录到 GitHub。想要使用组织身份，需要首先拥有一个普通账户，然后通过普通账户新建组织。在新建组织时，需要选择一个团队计划，GitHub 对于开源团队使用的团队计划是免费的。

使用组织有很多好处，即使是在规模不大的团队中，也可以体会到组织在去中心化和权限管理方面的优势，从而让一个团队真正以组织的模式运行——这是 GitHub 基于 Git 而高于 Git 的地方之一，也是笔者在[从 Git 到 GitHub](/from-git-to-github.html) 中提到的基于 Git 的 SaaS 的增值服务部分。

## Hello World

Hello World 是一个计算机编程的经典入门项目。在 GitHub，同样有一篇名为 [Hello World](https://guides.github.com/activities/hello-world/) 的入门指南。

但由于正在阅读的本文是一篇不完全指南，笔者不会逐句翻译这篇官方指南。

## 访问 GitHub 仓库与执行基本操作

本章将通过介绍几种用户访问 GitHub 仓库的方式，引出一些 Git 的基本概念，然后讲述一些相关的基础操作。

在 Git 中，为一个项目集中存放资源的地方叫作仓库（repository）。

### 使用 Web 访问 GitHub.com

访问 GitHub 仓库最直观的方式就是通过 Web 浏览器访问 https://github.com/<用户名>/<仓库名> ，例如：

[https://github.com/TellyLab/tellylab.com](https://github.com/TellyLab/tellylab.com) 

其中 TellyLab 是本站在 GitHub 的组织名，tellylab.com 是本站的项目仓库名。

或者，你可以从 [GitHub.com](https://github.com) 的任意页面一步一步点进去。

这种方式的特点在于用户访问的仓库副本是直接存储在 GitHub 上的那份（而不是用户终端设备本地的仓库副本），因此对仓库中分支（branch）的任何 commit 都会立即、直接在 GitHub 上生效（这听起来很符合直觉，但下面会解释为什么不是这样）。

#### 仓库与仓库副本

用于同一个项目的所有同名/同源 Git 仓库都被视为同一个 Git 仓库，但同一个 Git 仓库在不同时间和/或不同物理位置的不同拷贝（哪怕内容事实上相同）被视为同一个 Git 仓库的不同副本，简称仓库副本。

#### Branch

Git 通过 branch（分支）机制保证多条工作流在并行、异步推进时的相互独立。

分支就像是平行宇宙，不同的分支对用户展示为不同的工作时空。具体来讲，不同的工作分支对用户展示为仓库目录的不同副本，用户在 A 分支下对 d 目录中 f 文件所做的任何修改都不会体现在 B 分支下 d 目录中的 f 文件身上——除非~~时空错乱~~进行分支合并（merge）。

Master branch 是新仓库建立时的唯一分支，也是一个新仓库副本的默认工作分支。顾名思义，master branch 是一个项目的主工作流。Master branch 常简称为 master。

#### Commit

在 Git 中，对分支提交修改的行为/动作被称作 commit（一说 commit 被译作「提交」，但笔者选择不译），因此 commit 既是一个名词也是一个动词。我们可以说：commit 一下、这次 commit、那些 commits。

Commit 是对分支进行的，因此 commit 的实质是 commit to a branch。

Commit 操作本身只会对执行 commit 时的工作分支产生影响，但是如果因为其他操作导致这某次 commit 对其他分支产生了影响，这次 commit 也会出现在其他分支的 commit 历史中。

Commit 记录既可以指单次 commit 本身（也就是「一条 commit 记录」），也可以指 commit 记录的集合（也就是多次 commit 操作的历史记录）。

### 使用 GitHub Desktop

[GitHub Desktop](https://desktop.github.com) 是 GitHub 的官方桌面客户端，~~安全 稳定 声誉，~~目前支持 macOS 和 Windows，Linux 用户也懒得用对吧。

作为桌面客户端，用户无需操作命令行就可以访问 GitHub 仓库并执行大部分 Git 操作，对于不想安装 git 环境、不想使用 git 命令的用户是个简单而不算太强大的选择。支持通过应用内登录 GitHub.com 账户、应用内登录 GitHub 企业服务器、直接输入 URL 三种方式克隆（clone）远程仓库；支持在本地初始化新仓库；支持添加本地已有仓库。

#### Clone

在 Git 中，访问远程仓库并下载远程仓库副本，然后为仓库创建本地副本的操作被称为克隆（clone）。

克隆的意义在于通过对本地文件目录的直接读写实现对仓库副本中分支的读写，从而间接实现对远程仓库中分支的读写。

但是用户本地文件系统中的目录（也就是用户在操作系统中可以直接查看的那个目录）比仓库中的目录要低一个维度——因为仓库中一个确定的目录需要同时由目录位置（等同于文件系统中的目录位置）和指定的分支（specified branch）两个属性描述。

为了完成仓库目录到本地文件系统目录这个高维度到低维度的映射，在建立本地仓库副本时，Git 程序会首先将本地仓库副本的工作分支设为 master 分支（其实设为哪个分支并不重要，只是 Git 程序的设计就是如此）。此时，确定的工作分支就可以将仓库目录降维打击成适用于本地文件系统的工作目录，然后用户也就可以通过直接访问位于本地文件系统中的工作目录来访问仓库副本的分支了。

因此，克隆得到的本地仓库副本默认工作在 master 分支。

#### 工作目录

工作目录在路径上等同于本地仓库副本在本地文件系统中的目录，但工作目录（或工作目录的内容）并不等同于本地仓库副本本身。在这个路径下，除了工作目录的内容外，还有一些 Git 仓库本身的工作文件。这些文件定义了 Git 仓库的一些属性、记录了分支和 commit 信息。依靠这些信息，Git 便可以（有能力）在用户切换分支时刷新工作目录的内容。

#### 切换工作分支

在 Git 中，「将仓库副本的工作分支切换到指定的分支并刷新工作目录」的操作/行为叫做 checkout。

在 GitHub Desktop 中，没有明确的 checkout 命令或者 checkout 按钮，取而代之的是「Current Branch（当前分支）」下拉菜单和可以直接点击切换的分支名按钮。当用户点击分支名切换当前工作分支时，Git 程序将使用新的「工作分支-仓库目录」组合刷新工作目录。

由于位于本地的工作目录就是本地仓库副本在当前工作分支下的目录的映射——不同的分支决定了不同的工作目录内容，因此用户在访问工作目录前需要确保选择了正确的工作分支。

在本地使用任何方式（无论是文本编辑器、IDE，还是二进制、十六进制文件编辑器，甚至 Photoshop、数字照相机）对工作目录的内容进行修改（对工作目录内的目录/文件进行增删改操作都属于对工作目录内容的修改），即可对本地仓库副本的当前工作分支实现修改。

修改完成，就可以在 GitHub Desktop 上执行 commit 操作，向 Git 程序（或者说向记录 commit 历史的数据库）提交这次修改了。事实上，GitHub Desktop 一直在（准实时地）监视工作目录的修改，但只有在用户执行 commit 后，Git 程序才认为用户证实了这是一次对分支的修改，从而在 commit 历史中将其记录。

Git 允许同一个仓库在不同终端上的不同副本异步地执行 commit，为了在 commit 之后令不同仓库副本保持同步，用户需要手动执行一些操作。

#### Push

对于 GitHub，任何用户终端上的仓库副本都只和 GitHub 服务器上的仓库副本进行同步。因此，在用户终端上进行 commit 后，需要执行 push 操作将 commit 记录同步至 GitHub。

#### Fetch

在 Git 终端上，执行 fetch 操作将从远程仓库下载所有 commit 记录。

如果用户想要知道当前的本地仓库副本与远程仓库副本有哪些不同，则可以执行 fetch 操作，但是无论是否有新的 commit，fetch 都不会对本地工作目录的内容进行任何修改。因此，用户可以随时随便随意 fetch。事实上，GitHub Desktop 也会每隔一段时间自动执行 fetch，但这不妨碍用户在进行任何 Git 操作前手动执行一次（或多次）fetch 操作，以确保进行接下来的操作的决定是正确的。

在任何时候，GitHub Desktop 都将准实时地显示当前工作目录与本地仓库副本中最新一次 commit 记录的差异。

差异以两者中较新的状态相较于较老的状态的变化为正方向，绿色代表增加、红色代表删除、黄色代表修改。

#### Merge

在 Git 中，合并两条已有分支的操作叫作 merge。

Merge 既可以用来合并一条相同分支的两个不同副本，也可以用来合并两条不同的分支。（分支和分支副本的关系类似于仓库和仓库副本的关系。）

一旦出现异步修改不同仓库副本中相同分支导致的分支分歧，用户可以选择合并这些相同分支的不同分支副本。如果用户确定希望将远程分支副本合并进本地分支副本，则可以直接执行 merge ——或者说，没有任何解释说明的 merge 特指将同一分支的远程分支副本合并进本地分支副本。

在 GitHub Desktop 中，如果希望将一个分支合并进当前工作分支，则可以选择 Branch 菜单的 Merge into Current Branch，然后选择源分支，点击合并。

#### Pull

如果没有分支分歧，但当前工作分支的远程分支副本确有更新，则可以执行 pull，使用远程分支副本刷新本地分支副本。Pull 相当于先 fetch 再 merge 的组合。

由于上文所述的原因（应当及时 fetch），GitHub Desktop 永远优先显示 pull 按钮而不是 merge 按钮。

### 使用 Git 接口

Git 是 GitHub 的基础，因此 GitHub 允许用户通过标准的/通用的 Git 接口访问 GitHub 仓库。

GitHub 用户可以使用任何 Git 兼容程序——包括标准的/通用的 CLI/GUI 发行版、内建 Git 支持的 IDE 等访问 GitHub 仓库。

#### 使用 HTTPS

顾名思义，Git 兼容程序可以使用 HTTPS 协议与 GitHub.com 服务器进行通信。由于 Root CA 的存在，用户无需进行任何额外配置即可与 GitHub.com 服务器建立安全连接。在 Git 兼容程序上，输入远程仓库 URL 即可访问 GitHub 仓库。GitHub 仓库的 URL 格式为：

https://github.com/<用户名>/<仓库名>.git

例如：

https://github.com/TellyLab/tellylab.com.git

其中 TellyLab 是本站在 GitHub 的组织名，tellylab.com 是本站的项目仓库名。

#### 使用 SSH

同理，Git 兼容程序可以使用 SSH 协议与 GitHub.com 服务器进行通信。由于全球尚无完善的针对 SSH 的 PKI（公钥基础设施），用户通过 SSH 与 GitHub.com 服务器建立安全连接前，需要首先在 GitHub 账户设定页面为 GitHub 账户添加 SSH 公钥。GitHub 仓库的 SSH 地址格式为：

git@github.com:<用户名>/<仓库名>.git

例如：

git@github.com:TellyLab/tellylab.com.git

其中 TellyLab 是本站在 GitHub 的组织名，tellylab.com 是本站的项目仓库名。

#### Git 命令

在通过标准 Git CLI 访问包括 GitHub 仓库在内的任何 Git 仓库时，都需要使用 git 命令。在这里，将 git 的首字母小写的原因在于 Git 的 CLI 命令正是「git」（并不是所有 CLI 应用的命令都是如此）。

GitHub Training 有一份 Git 速查 PDF，名为 [Git Cheat Sheet](https://github.github.com/training-kit/downloads/github-git-cheat-sheet.pdf)，其中简单介绍了常用的 git 命令。同样，笔者不会逐句翻译这份 PDF。

需要注意的是，如果你觉得接下来的内容是笔者在逐词翻译这份 PDF 中的某句话，那么你可能感觉错了。笔者对于某些命令的介绍和描述将和这份 PDF 中的有所出入——这正是笔者要写这篇文章的原因之一——如果你更希望阅读英文原版，请移步 [Git Cheat Sheet](https://github.github.com/training-kit/downloads/github-git-cheat-sheet.pdf)。

在介绍下列命令时，假设读者已经具有基础的 *nix/POSIX CLI/shell 操作知识。

$ git config --global user.name "[name]"  
全局设定你希望在 commit 记录中显示的用户名字

$ git config --global user.email "[email address]"  
全局设定你希望在 commit 记录中显示的 email 地址

$ git config --global color.ui auto  
全局设定 CLI 输出色彩为自动（也就是彩色，且配色方案为默认）

$ git branch [branch-name]  
创建新分支

$ git checkout [branch-name]  
切换当前工作分支并用新分支的仓库目录刷新工作目录

$ git merge [branch]  
将指定分支的 commit 记录合并进当前分支（这既意味着当前分支副本内容的刷新也意味着 commit 记录的继承）

$ git branch -d [branch-name]  
删除指定分支

$ git init  
将当前工作目录（这里指 pwd 打印出的那个目录，无关于 Git 的工作目录）初始化为一个 Git 仓库（虽然初始化后的这个目录也确实在路径上等同于这个 Git 仓库的工作目录）

$ git clone [url]  
从指定地址克隆一个 Git 仓库到本地（URL 的存在意味着指定地址并不一定是网络上的远程地址）

$ git fetch  
执行 fetch 操作

$ git merge  
执行 merge 操作

$ git push  
执行 push 操作

$ git pull  
执行 pull 操作

$ git log  
列出当前分支的 commit 记录

$ git log --follow [file]  
列出指定文件的版本历史，包括对其产生影响的 commit 记录和重命名记录

$ git diff [first-branch]...[second-branch]  
展示两个分支的内容差异

$ git show [commit]  
展示指定 commit 记录的元数据（metadata）和内容变化

$ git add [file]  
将指定文件添加到 Git 仓库的版本控制范围内（官方中文说法好像是建立跟踪）

$ git commit -m "[descriptive message]"  
执行 commit 操作并记录描述性信息（Commit 摘要）

$ git reset [commit]  
将 commit 历史撤销到指定 commit 后的状态（也就是撤销指定 commit 后的所有 commit 记录，不包括被指定的 commit 本身），但不对本地工作目录的内容进行回滚

$ git reset --hard [commit]  
将 commit 历史撤销到指定 commit 后的状态，同时对本地工作目录的内容进行回滚（请注意，这是一条危险命令，一旦误操作，git 本身不提供任何反回滚能力；如有必要，则需考虑使用文本编辑器/IDE 的撤销功能或独立于 Git 的文件备份/快照/版本管理功能进行挽回；如果针对整个本地仓库副本所在的目录进行回滚，则可能会因 commit 记录随之回滚而令问题变得更加复杂，在执行此类操作前请确保你对 Git 的工作原理足够熟悉）

## 理解 GitHub 工作流

Git 尤其是 GitHub 的核心在于协作，在了解基本操作后，理解 GitHub 的工作流是快速上手 GitHub 多人协作项目的极佳方式。在 GitHub，有一篇名为 [Understanding the GitHub flow](https://guides.github.com/introduction/flow/) 的官方指南，同样，笔者不会翻译。

### 协作模式

通过 GitHub 进行多用户协作时有两种主流的协作模式（collaborative models，实为协作模型，但笔者愿意译作协作模式），但无论是协作模式还是整个工作流都不是固有标准，因此用户可以任意选择、演绎甚至创建协作模式和/或工作流。[GitHub 的官方文档](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-collaborative-development-models)简单介绍了这两种协作模式，同样，笔者不会翻译。

#### 共享仓库模式

共享仓库模式（shared repository model）应该是最传统的 Git 协作模式，顾名思义，所有协作者在执行 commit 操作时均共享同一个 Git 仓库。在 GitHub 上，这里的同一个 Git 仓库是指一个特定用户账户/组织名下的特定仓库（比如 TellyLab/tellylab.com 就是一个特定的仓库），不同协作者的实际 commit 对象是同一个仓库的不同副本，然后通过 push 操作将自己的 commit 记录推送至位于 GitHub 服务器的中央仓库副本。

在共享仓库模式下，所有的协作者都拥有向中央仓库 push 的权限，因此，从安全的角度讲，共享仓库模式仅适用于没有非信任贡献者参与的自治项目——所有协作者共商码是、求同除异，任何人都可以是项目的主宰。

在 GitHub.com 上，向仓库添加协作者的路径是：仓库页 - Settings - Collaborators & teams.

#### Fork 拉取模式

Fork 拉取模式（fork and pull model）一说为复刻拉取模式，但笔者认为这种说法/翻译并不妥当，一来复刻给人以一种克隆的感觉，但 fork 的本质是一种保留对上游依赖关系的跨用户分支，复刻没有体现出他和克隆的区别；二来 fork 一词本身并无任何复刻之意；三来唱片/出版/印刷业中的复刻均为 re- 开头的单词，明显有「复」字之义，有别于 fork。对于 fork 的另一种理解是使用叉子从别人那里叉一片肉过来，这种解释虽没有体现出对上游的依赖关系，但贵在生动形象，无伤大雅。

在采用 fork 拉取模式的项目中，fork 拉取和共享仓库两种模式往往同时存在。Fork 拉取模式的特别之处就在于 fork 和拉取。对于项目仓库的所有者和受信任的协作者，他们采用共享仓库的模式进行协作。而对于外部贡献者，则需要首先 fork 源仓库（上游仓库）到自己名下，然后对自己名下的 fork 仓库执行 commit，在完成阶段性贡献后，外部贡献者如果希望将自己的成果并入源仓库，则需要由源仓库的协作者（确切地讲是有写仓库权限的用户）执行 pull 操作将下游 fork 仓库的 commit pull 入上游源仓库。在这种情况下，下游用户需要发起 pull request 来请求上游仓库维护者发起 pull 操作。

Fork 拉取模式在 GitHub 上非常常见，因为几乎所有的知名开源项目都是采用 fork 拉取模式吸纳外部贡献的。这种模式的工作原理决定了只有受信任的协作者才能最终决定源仓库的内容，因此可以避免广大想法/立场/目的不一的外部贡献者彼此间的分歧导致的分支冲突甚至破坏性编辑，同时也可以通过源仓库所有者的声誉为项目质量背书（但这里的背书不意味着担保和/或负责，具体权利与义务需要参见项目 license）。

在 GitHub.com 上，fork 一个仓库的方式是点击仓库页右上角的 Fork 按钮。

### 创建分支

多人协作项目的一个常见分工原则就是不同的团队负责不同的并行推进路线，每条推进路线对应一个分支。比如分支 A 用于新功能开发、分支 B 用于已知 bug 修复、分支 C 用于集成测试，前两个分支均基于 master，而第三个分支则基于前两个，同时，对这三个分支所做的任何 commit 都不会影响 master——直到至少一者被合并入 master。

Master 分支的使用原则在于其在任何时候都应当是可发布（亦或者是部署/交付）的，因此，在通过最终测试前，不应将其他分支并入 master 分支。分支的命名应当是具有描述性的，比如 two-factor-sec, login-bug-fix, final-test.

要开始一条工作路线，从创建一个分支开始。要加入一条现有的工作路线，在成功访问到目标仓库后，直接切换当前工作分支到目标工作路线所对应的分支即可。

### 执行 Commit

分支一旦被创建，用户便可开始正式工作，并随时执行 commit。在执行 commit 时，应当注意附上有意义的 commit 信息，这既是给协作者的参考，也是给自己的备忘，以便在未来出现问题时可以快速、准确地溯源、回滚。

### 发起 Pull Request

Pull request 是 GitHub 的杀手功能之一，单从 Git 的角度来讲，pull request 允许 commit 者向其他协作者发起一个让对方 pull 自己所 push 的 commit(s) 的请求~~（这段好 4A，冇办法）~~。无论你是不是具有直接 push 权限（写仓库权限）的协作者，都可以发起一个 pull request 来请求其他协作者向自己的仓库（或你们共享的仓库）拉取你所做的 commit(s)。

除此之外，pull request 机制本身提供了强大的协作沟通、自动化流程追踪和合并策略选择能力。用户对于一个 pull request 的响应并不是要么接受要么拒绝的布尔型，也不是一旦接到请求就必须立即作出决定。实际上，在一个分支刚建立起时就发起 pull request 请求，然后令这个请求在整个分支的生命周期内保持待决状态是很常见的做法，此时的 pull request 就是一个自动化流程追踪系统。在一个 pull request 中，用户可以使用 「@提及系统」来提及一个希望收到明确请求的对象（这个对象既可以是用户也可以是组织中的团队）；还可以在这个 pull request 下与其他协作者沟通，甚至附上 emoji 和图片；也可以选择继续提交新的 commit，直到大家达成共识（或者做出妥协）；最后，具有写仓库权限的协作者可以 merge 这个 pull request，通过依次执行 merge pull request 和 confirm merge 操作将其并入目标分支。

在共享仓库模式中，pull request 通常用于发起 review 请求，然后将一个分支并入另一个分支——虽然发起者本身就具有直接 push 和 merge 的权限，但作为协作流程的一部分，在合并分支之前通过 pull request 知会他人、接受 review 并征求他人意见，往小了说是对其他团队成员的尊重，往大了说这关系到生产安全。

在 fork 拉取模式中，pull request 通常用于请求将一个下游 fork 仓库的分支并入上游源仓库的分支，由于这种模式下位于下游的、源仓库外部的贡献者不具有对源仓库的直接写权限，因此只能通过 pull request 方式来请求上游源仓库的维护者对其希望贡献 commit 的意愿作出回应（视情况作出 merge 决定或给出驳回原因）。

### 合并分支

在大型或关键生产项目中，通过单元测试的分支往往还要被并入集成测试/系统测试/验收测试等分支进行最终测试，因此在通过最终测试前，不应将其他分支并入 master 分支（这一点在刚才已经说过了）。而如果某个分支没有通过某次测试，则应当根据 commit 记录进行回滚，然后修改持续分支、并入测试分支，直到通过测试。对于已经被并入测试分支且通过测试的分支、已经通过最终测试并被并入 master 的分支，他们的生命周期便可以（但不必须）结束了。除非要进行持续交付，否则可以安全地删除已被合并入其他分支的分支——这种状态下的分支被称为头分支，GitHub 提供了自动删除头分支的功能，这个功能默认关闭，针对特定仓库开启这个功能的路径是：仓库页 - Settings - Options - Automatically delete head branches. 被 GitHub 自动删除的分支是可以被恢复的，恢复入口之一在：仓库页 - Pull requests.

## GitHub 进阶使用

### 排除文件

#### 针对仓库排除

#### 针对本地仓库副本排除

#### 本地全局排除

### 与同步网盘配合使用

（未完待续，在现有标题的内容被补全后，本文即转入随缘更新状态，笔者将看心情更新 GitHub 的进阶使用方法）

（最后更新于 2019-11-19）
