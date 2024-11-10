--最大的同性交友平台

‍

### Header

Github 相关的一些内容, 包括衍生工具

Git 指令以及底层见 Git 笔记本

‍

‍

‍

# 知识

‍

## Readme 美化

‍

#### 小徽章

[Shields.io | Shields.io](https://shields.io/)

‍

‍

‍

‍

## 选择开源协议

[概括介绍](https://www.cnblogs.com/bronya0/p/14551683.html)

‍

1. **None / No License**  
   默认协议，不允许他人复制、分发、修改、使用，只能 fork 下来看
2. **Apache License 2.0**  
   允许个人使用、商业使用、复制、修改、分发，但是出了事作者免责，版权信息要保留. 做了修改要说明.
3. **MIT License**  
   允许个人使用、商业使用、复制、修改、分发，但是必须保留作者信息，比较宽松.
4. **GNU GPLv3**  
   它允许个人使用、商业使用、专利授权，允许复制、分发、修改，作者不承担用户使用的一切后果. 但是它有很多限制：  
   你必须开源，无论有没有修改.  
   协议和版权信息要保留说明  
   协议不能私自更改，与原版本一致.  
   你修改的地方要说清楚.
5. **BSD 2-Clause “Simplified” License**  
   允许许任何人进行个人使用、商业使用、复制、分发、修改，加上作者的版权信息，还必须保留免责声明，免去作者的一些责任（比如使用后果）
6. **BSD 3-Clause “New” or “Revised” License**  
   在 BSD 2-Clause “Simplified” License 协议的基础上，还不得追加使用作者的信息做商业宣传. 例如，你对外说是作者某某某的作品，利用人家的名气. 但是你自己做了不当的修改.
7. **Eclipse Public License 2.0**  
   允许个人使用、商业使用、专利授权、复制、分发和修改，作者免责，需要保留版权信息、必须开源、不允许更换协议, 特点在于可以对软件进行商业使用，对专利授权免去版税
8. **GNU Affero General Public License v3.0**  
   允许个人使用、商业使用、专利授权、复制、分发和修改，作者免责，贡献者可以快速专利授予，需要保留版权信息、必须开源、不允许更换协议、声明变更. 和 GPL 类似，不同点在于，如果你修改了源码并在放到网上提供服务，那么你必须公开这个修改版本的完整的源代码.
9. **GNU General Public License v2.0**  
   相比于 GNU GPLv3，不能进行专利授予.
10. **Mozilla Public License 2.0**  
     许个人使用、商业使用、专利授权、复制、分发和修改，作者免责，需要保留版权信息、必须开源，不允许更换协议（但允许更换成某些 GNU 协议），不允许使用商标.
11. **The Unlicense**  
     完全免费，无约束. 出了事情作者免责.

‍

‍

### 选择建议

‍

1、普通开发者(我自己)

如果你是**信仰开源大法**的普通开发者，使用 **MIT License** 协议即可，它会保留你的版权信息，又允许他人进行修改.

‍

2、**用到了 GNU 的开发者**

如果你用到了 GNU 的库，由于“传染性”，不允许更换协议，必须选择 GNU 相关的协议.

‍

3、**开源库**开发者

推荐使用 GNU LGPL 相关协议.

‍

4、无私奉献的雷锋

感谢你为世界作出的贡献，必选 The Unlicense.

‍

5、不知道该选什么

选择默认的 None 即可，**保留你的全部权利**，后续再去决定要不要更换协议.

‍

‍

# 搜索

‍

搜 <kbd>awesome</kbd>​​, 找精选优选

‍

[开源软件知乎介绍](https://www.zhihu.com/question/56766597/answer/3282894989)

‍

‍

# 项目

‍

## 规范提交 PR

[Link](https://zhuanlan.zhihu.com/p/584834288)(知乎大佬整合版)

> Github 官网对协作流程方面写很清楚
>
> [GitHub Docs](https://github.com/github/docs) 其他语言的官方翻译会滞后于英文版，因为其他语言版本不接受内容贡献（这是 Github 官方在准确度和效率之间做的一个平衡）.
>
> > **避坑提醒：** 1. 给 [GitHub Docs](https://github.com/github/docs) 提交内容翻译 PR 通常是不会通过的. 这里官方说的很清楚了，不接受翻译内容贡献. 2. 如果发现英文原文内容有问题（语法，语义，词法），这个是可以提交修改意见 (Issue) 和合并请求 (Pull Request) 的（其他语言的版本也都是通过英文版本翻译过来的）. 但也要基于原文内容修改，自己独立创作的内容合并请求很难被 Github 官方通过.
>
> 下面是参考的相关 Github 官方文档：
>
> - [GitHub flow](https://docs.github.com/en/get-started/quickstart/github-flow)
> - [Contributing to projects](https://docs.github.com/en/get-started/quickstart/contributing-to-projects)
> - [Basic writing and formatting syntax](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
> - [Linking a pull request to an issue](https://docs.github.com/en/issues/tracking-your-work-with-issues/linking-a-pull-request-to-an-issue)

‍

‍

### 找项目

如果是一些小问题，而且自己时间有限，可以先提一个 issue，提醒作者本人或者其他人解决，如果没有人回应，再尝试自己提交 PR

‍

### 派生一个存储库 (Forking a repository)

​**​`Fork`​**​ 按钮，创建一个新的派生项目（**Create a new fork**）

‍

### 克隆一个派生 (Cloning a fork)

进入自己的 Github 工作区，将派生项目克隆到本地

‍

### 创建一个分支 (Creating a branch)

```bash
# 创建并切换到本地新分支，分支的命名尽量简洁，并与解决的问题相关
git checkout -b delete-unused-link
```

‍

### 做出修改 (Make changes)

‍

### 提交修改 (Pushing changes)

这步需要注意一下，有些项目更新的会比较频繁. 当你做出修改和提交 PR 之前，可能有作者新的提交和 PR 被合并到原项目. 如果有这种情况发生，在你工作区的派生项目会显示原项目有更新

‍

> 推到远程仓库之前要先拉下来一次, 免得远程又有更改而导致本地和远程出现更多差异(我的直观理解) 当然要拿一个专门同步其修改的分支去同步

点击 **​`Update branch`​**​ 之后，将原项目更新同步到派生项目

‍

再进入本地项目文件夹

```bash
# 保存本地修改并将工作目录还原到当前HEAD提交状态
git stash

# 从远程拉取最新的项目代码，将派生项目更新同步到本地
git pull

# 将保存的修改还原回当前工作目录
git stash pop
```

‍

查看更新后的内容是否和本地修改有冲突，有冲突就解决冲突，完成后就可以提交修改了

```bash
# 当前文件夹位置 leerob.io
# 保存本地修改并将工作目录还原到当前HEAD提交状态
git commit -am 'Delete unused Link declaration'

# 推到派生项目远端仓库，因为之前项目分支是在本地创建的，需要带上 '--set-upstream'
git push --set-upstream origin delete-unused-link
```

‍

### 创建合并请求 (Create a pull request)

‍

回到线上派生项目的工作区，会看到新分支和修改的合并提交信息，点击**​`Compare & pull request`​**​

‍

选择你想并入的原项目分支，标题和描述信息. 如果有对应的 issue，就通过键入 `#`​ 添加(Github 会自动展示 issues 列表)

‍

点击 **​`Create pull request`​**​ ，就行了. 不出意外，你提的 PR 就应该躺在下面了

‍

### 发表评论 (Address review comments)

这部分是原项目作者需要遵循的规范

‍

### 合并你的请求 (Merge your pull request)

这是原项目作者要做的

‍

### 删除你的分支 (Delete your branch)

最后一步不是必须的，只是保持一个规范的开源协作习惯，减少意外提交错误项目分支的情况发生.

来到原项目 Github 主页，找到之前已经合并的提交请求（在关闭的 PR 列表中），点击 Delete branch

‍

```cmd
# 删除本地分支
git branch -d delete-unused-link
```

注意：下次在已有的派生项目创建新分支前，要先将原项目的更新同步到派生项目，并将更新后的派生项目拉到（`git pull`​）本地，再重新建立分支（`git checkout -b new-branch-name`​ ），再重复上述过程即可.

‍

‍

## 账户

‍

### Github 公钥

> 一般展示的报错是:"publicKey"缺失

[Thanks](https://zhuanlan.zhihu.com/p/454666519)

‍

**创建 SSH Key pair**

> 简易操作
>
> 打开 git bash
>
> ```java
> ssh-keygen
> ```
>
> 方便起见都不填,秘钥生成在`C:\Users\xxxx/.ssh/`​ 这个地方，文件名为`id_rsa.pub`​, 把文件地址`C:\Users\xxxx/.ssh/id_rsa.pub`​ 复制一下，通过`cat`​ 命令查看其内容
>
> ```java
> cat "C:\Users\spade/.ssh/id_rsa.pub"
> ```
>
> 我的内容如下, **将 SSH key 添加到 Github 账户, 设置-公钥-添加 PCPro-SSH-Key**
>
> ```java
> ssh-rsa /A/k/+FbNsssssss9h+YU= SK@LAPTOP-FHJM8IEB
> ```

‍

‍

1. 本地机器上配置 SSH

   ```cmd
   ssh-keygen -t rsa -C "spadekxcwxtlsg@gmail.com"
   ```

2. 将公钥复制粘贴到 Github 的个人 SSH Keys 配置里. 产生的公钥一般为 ~/.ssh/id_rsa.pub

   ```
   cat ~/.ssh/id_rsa.pub
   ```

   选中公钥复制粘贴到 Github [SSH Keys 设置](https://github.com/settings/keys) 的 NEW SSH key 的 key 里即可(Title 自己命名).

3. 启动 SSH 代理并添加密钥

   ```
   eval `ssh-agent -s`
   ssh-add
   ```

‍

# 特殊

‍

‍

## 桌面端 Github

‍

基础创建分支无需赘述

‍

## GithubCodeSpace

浏览器 VSCode, 可以租用但是有每月限制(学生包可以有额度)

但是在线环境需要重新配

‍

‍

# 实操

见版本控制个人使用记录 +Github 仓库

‍

‍

# 建议

‍

## 参与开源项目 Guide

‍

首先我们需要 fork 该项目. 然后就可以在自己的仓库中看到 chunjun 项目了

‍

clone 我们仓库中的 chunjun 项目

```bash
git clone https://github.com/your-github-name/chunjun.git
```

‍

添加远程分支

```bash
git remote add upstream https://github.com/DTStack/chunjun.git
```

‍

添加了之后可以查看远程仓库

```bash
git remote -v
origin  https://github.com/your-github-name/chunjun.git (fetch)
origin  https://github.com/your-github-name/chunjun.git (push)
upstream    https://github.com/DTStack/chunjun.git (fetch)
upstream    https://github.com/DTStack/chunjun.git (push)
```

‍

不论是准备开发一个新功能，还是准备提交一个 pr，都需要优先更新远程分支到本地, 例如, 现在你需要基于 master 开发一个新的功能，你可以做如下操作

```bash
# 可以使用
git pull
# 或者使用
git fetch upstream -p
git rebase upstream/master

# 然后基于 master 创建一个 feature 分支(一般新功能需要先写issue和社区同学讨论该功能，比如和作者讨论你的想法是否能带来好的效果、以及该功能是否可行)
git checkout -b feature_your-issueid
```

‍

等你开发完功能，并且完成测试之后，可以提交代码, 注意这里先不要直接 push

```bash
git add .
git commit -m "your-commit-message"
```

此时你开发一个功能可能耗时 1h，期间已经有其他同学提交了代码，所以你还需要保持最新代码,

```bash
git fetch upstream
git rebase upstream/feature_your-issueid
```

rebase 之后可能会有文件冲突，需要按需解决冲突，将所有冲突都解决之后再执行

```bash
git add .
git rebase --continue
```

看到提示 `rebase successful`​ 之类的就表示冲突解决完成了，然后就提交到你的 github 仓库中(注意不是 upstream), rebase 之后可能无法正常推送, 需要 `git push -f`​ 强制推送，这个操作有风险, 操作前请仔细检查以避免出现无关代码被强制覆盖的问题, 具体风险可以看 [5.6 rebase](file:///C:/Users/SPADEK/Desktop/git%E5%B8%B8%E7%94%A8%E6%93%8D%E4%BD%9C.md#56-git-rebase) 相关的解释.

```bash
git push origin feature_your-issueid
```

然后按页面提示，提交 pr

‍

## 给开源项目贡献代码全流程

[Link](https://mp.weixin.qq.com/s/JMNQi3BSTmKpF9vXMEdKHw)

使用 GitHub Flow

‍

### **克隆(Clone)副本仓库到本地**

把 Fork 后的副本仓库 Clone 到本地.

```git
git clone https
:
//github.com/yourname/go go   # clone 到本地的 go 目录
```

‍

### **使用分支 (branch)**

进入仓库目录后，可以使用如下命令创建并切换到 test 分支.

```git
git checkout
-
b test
# 创建并切换到 test 分支
```

‍

### **在本地仓库提交 (commit)**

‍

在这个 `test`​ 分支下经过一些修改后，你需要提交这些修改到本地仓库.

```git
git addA #添加所有文件
git commitm'Add test' #提交 commit
```

‍

### **跟原始仓库 (upstream)合并**

‍

前面已经说过，在副本仓库做的修改是不会影响到原始仓库的. 同样，在原始仓库的更新也不会反映到副本仓库来.

在 GitHub，如果你副本仓库的**进度落后于原始仓库还坚持发起 PullRequest，后果只会是被拒绝**.

‍

那么问题来了，我们应该如何同步原始仓库的更新呢？  
答案是：区别于 origin，它是用来向副本仓库提交更新的远程仓库；我们添加一个 upstream，也被称为 **上游** 是专门用来**同步原始仓库更新的远程仓库**.

在默认情况下，在你 clone 后的仓库目录里，git 已经自动将 origin 和你的副本仓库关联在一起了

‍

你可以通过如下命令查看.

```git
git remotevv
两个 verbose 参数查看远程仓库
```

‍

然后，通过如下命令添加这个 upstream，使用这个名字只是约定俗成，你可以用你觉得更好的名字来替换它.

```git
git remote add upstream git@github.comgolang/go.git
添加 upstream 远程仓库
```

‍

现在，我们假设在做出修改后，上游（upstream）已经更新了很多提交.  
此时如果对上游的变化视而不见，强行 push 并发起 PullRequest 还是会被拒绝.  
可以通过如下命令**拉取并合并上游的更新**：

```git
git checkout master
切换到默认存在的 master 分支
git pullrebase upstream master:master
使用 rebase 模式拉取 upstream/master 上的更新
且与本地的 master 合并。第一个 master 是远程分支，第二个是本地分支。
git checkout test
切换到前面建立的 test 分支
git rebase master
使用 rebase 模式合并本地的 test 和 master 分支
```

也可以通过另一种方式：

```git
git checkout master
切换到 master 分支
git fetch upstream master
获取 upstream 上的 master 分支
git checkout test
git rebase upstream/master
使用 rebase 模式合并本地的 test 和 upstream/master 分支
```

‍

总的来说，可以把本地的 master 分支当作一个只负责从上游获取更新的分支，所有本地的改动都不会直接在 master 上面进行.

而是先将上游的 master 和本地的 master 合并，此时，本地的 master 是上游的最新版本；
再通过合并 test 和本地的 master 来完成本地改动的更新. 整个过程在未开始合并之前，你的代码更新应该只会出现在 test 分支上.

> 注意：在使用 git rebase 相关的命令时，需要谨慎应用在已经提交的更新或远程仓库上.

‍

### **推送 (push)到副本仓库**

现在，我们已经完成代码的修改、上游的同步更新并且完成了合并.

接下来应该将代码 push 到副本仓库.

```git
git push origin test
# 将本地 test 分支的代码 push 到 origin 的 test 分支

# 如果该分支不存在则会创建
```

‍

这个 `push`​ 只会更新副本仓库，并不会影响到原始仓库.

要将代码贡献到原始仓库，还要发起 PullRequest

‍

### 发起 PullRequest

直接图形化操作

‍

‍

## 新手上路提交 PR

_大佬建议_

‍

‍

### 第一次提交 PR 的时候

遵守 fork-commit-push-PR 流程

需要注意的是，任何工作最好都不要在已经有的分支上进行. 默认应该是**每次工作开一个新的分支**

理由是如果是在已有的分支上进行 commit（熟练实习生一般是偷懒或者习惯没有养成，新手实习生往往是不知道可以 checkout -b）， 那么基本上是「单线程」的工作模式：在 master 分支上 commit 之后， push 到自己的 GitHub remote， 发起 PR，然后就什么都做不了了，等我来处理.

‍

正确做法是每次工作都开一个新的分支来做

1. Clone 下来 repo，切换到 master 或者 develop 分支（具体根据项目约定）.
2. 从项目工作约定的分支 checkout -b 一个 local branch，分支名字可以是 issueNNN，对应任务的 id.
3. work，commit，work，commit，得到一个 commit list （有时候对应的 patches 叫 patch set）
4. push 到你自己的 GitHub remote 的新分支，注意是新分支，一般是跟本地分支名字相同（to save your time and life）.
5. 登录进入 GitHub 网页，看到你的分支，按照提示进行 Pull Request 操作. 注意一定要选对目标分支，如果不明白，跟你的 mentor 询问.
6. Reviewer （一般是你的 mentor）进行 code review. 一般会提一些意见，要求修改.
7. 如果要求修改，跳转到 3. 新的修改 push 到已经发起 PR 的分支之后不需要重复发起 PR.
8. 如果被 merge，那么恭喜,同时要注意，现在开始你的工作流程就不一样了. 继续往下看.

‍

‍

### 第 N+1 次提交 PR 的时候

OK，现在恭喜你，已经有了被 merge 的 PR. 同时有一个坏消息告诉你： 从现在开始你 fork 出来的 repo 已经跟上游的 repo 不一样了

‍

1. 进入已经 clone 的 repo. 运行 git remote -v. 预期看到一个 remote，名字是 origin， URL 是你的 GitHub repo，forked from repo AAA.
2. 假设你的 repo 是从 repo AAA fork 出来的. 例如 [https://github.com/lazyparser/becoming-a-compiler-engineer-codes](https://github.com/lazyparser/becoming-a-compiler-engineer-codes) 运行 git remote add lazyparser URL. 这一步的作用是添加了一个新的 remote. remote 的名字可以自己取一个，没有什么需要遵守的规律.
3. 运行 git fetch lazyparser 将上游仓库的代码也 clone 一份下来到你的本地机器（跟 origin 同样保存在 .git 目录下）
4. 本地创建一个分支，从你计划 push 的 branch. 具体 git help branch or git help checkout. 你会发现原来这些命令都可以带两个以上参数的. 一条命令搞定.
5. work，commit，work，commit，得到一个 commit list （有时候对应的 patches 叫 patch set）
6. 【注意】 push 到 **你自己的** GitHub remote 的新分支，注意是新分支，一般是跟本地分支名字相同（to save your time and life）.
7. 其他都跟第一次类似了.
8. 以后，如果 PR 遇到冲突，表示上游分支已经更新了，你需要更新自己的本地分支已解决冲突. 解决方法是 git fetch 然后 git rebase. 遇到冲突的时候，用 git status 查看哪些是冲突的，打开，一个个修改. 搜索类似 <<<<HEAD 这样的字样.
9. 修改（fix）了所有的 con'flicts 之后，用 git rebase --continue 继续 rebase.
10. rebase 完成之后，push 到你自己的 remote（一般是 origin）. 如果已经发起了 PR，那么 PR 应该自动更新了.

‍

‍

‍

‍
