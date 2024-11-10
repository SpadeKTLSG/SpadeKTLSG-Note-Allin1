--开源分布版本控制工具

‍

‍

### Header

作为版本控制一哥, 同样记录版本控制内容

开源免费的分布式版本控制系统，可以快速高效地处理从小型或大型的各种项目. 易于学习，占用空间小，性能快得惊人. 

Git软件比Subversion、CVS、Perforce和ClearCase等SCM（Software Configuration Management软件配置管理）工具具有性价比更高的本地分支、方便的暂存区域和多个工作流等功能. 

‍

#### 知识库

[Pro Git](https://www.progit.cn/#_pro_git)

‍

# 知识

‍

## 名词解释

工作目录  Git Work Dir

* blob, 就是单个的文件；
* tree, 就是一个文件夹.

快照(snapshot)则是被追踪的最顶层的树

‍

‍

‍

‍

## 版本控制

‍

一般情况下，一份文件，无论是DOC办公文档，还是编程源码文件，我们都会对文件进行大量的修改和变更. 但是我们无法保证每一次的修改和变更都是正确并有效的，往往有的时候需要追溯历史操作，而版本控制（Revision control）是一种在开发的过程中用于管理我们对文件、目录或工程等内容的修改历史，方便查看更改历史记录，备份以便恢复以前的版本的软件工程技术. 

没有进行版本控制或者版本控制本身缺乏正确的流程管理，在软件开发过程中将会引入很多问题，如软件代码的一致性、软件内容的冗余、软件过程的事物性、软件开发过程中的并发性、软件源代码的安全性，以及软件的整合等问题. 

‍

### 基础功能

1. 保存和管理文件
2. 提供客户端工具进行访问
3. 提供不同版本文件的比对功能

‍

### 主流版本控制系统

‍

#### 集中化版本控制系统

Centralized Version Control Systems (CVCS)

比如：CVS, Subversion, Perforce, etc.

‍

这种版本控制系统有一个单一的集中管理的服务器，保存所有文件的最新版本，大家可以通过连接到这台服务器上来获取或者提交文件. 

‍

这种模式相对本地版本控制系统是有所改进的，但是缺点也很明显，如果服务器宕机，那么轻则耽误工作、重则数据丢失. 于是分布式版本控制系统应运而生. 

‍

#### 分布式版本控制系统

Distributed Version Control Systems (DVCS)

比如：Git, Mercurial, Bazaar, etc.

‍

分布式的版本控制系统会把**代码仓库完整地镜像**下来，这样任何一个服务器发生故障，都可以用其他的仓库来修复. 

更进一步，这种模式可以更方便的和不同公司的人进行同一项目的开发，因为两个远程代码仓库可以交互，这在之前的集中式系统中是无法做到的. 

‍

“把代码仓库完整地镜像下来”

CVCS 每个版本存放的是当前版本与前一个版本的差异，因此也被称作基于差异的版本控制 (delta-based)；

Git 存储的是所有文件的一个快照 (snapshot)，如果有的文件没有修改，那就只保留一个 reference 指向之前存储的文件. 

‍

在Git中，每个版本库都是一样重要的. 所以就不存在像集中式版本控制软件中以谁为主的问题. 任何一个库都可以当成主库. 

这种方式可以更大限度地保证项目资源得安全. 

‍

‍

#### 版本问题解决方法

‍

文件冲突问题(类似数据库的幻读等错误)

‍

**集中式**

解决1: 加不可修改锁

解决2: 有约束, 进行比对(同一行冲突仍然不行)

‍

**分布式**

服务器拷贝到本地一份, 访问本地操作.

中央服务器出问题没事.

‍

‍

# 原理

‍

‍

## HEAD文件

作用

1. 定位文件
2. 避免冲突

‍

‍

## 数据模型

‍

### 快照

Git 记录了每个快照的 parent，也就是当前这个文件夹的上一个版本

那么快照的迭代更新的过程就可以表示为一个**有向无环图(平时见到的commit形成的树状结构),**  每个快照其实都对应了一次 `commit`​

​![1460000022953312](assets/net-img-1460000022953312-20240930223356-tso2ixy.png)​

‍

‍

### 快照查看

> ​`git cat-file -t`​: 查看每个 SHA-1 的类型;  
> ​`git cat-file -p`​: 查看每个对象的内容和简单的数据结构.

但是通过这个哈希值来搜索也太不方便了，毕竟这是一串 40 位的十六进制字符，就是第二部分 `git log`​ 里输出的那个`编码`​

‍

因此，Git 还给了一个引用 `reference`​

比如，我们常见的 `HEAD`​ 就是一个特殊的引用

本地库就是由 `对象`​ 和 `引用`​ 构成的，或者叫 `Repositories`​

在硬盘上，Git 只存储 `对象`​ 和 `引用`​，所有的 Git 命令都对应提交一个快照

‍

‍

## 分区

‍

用户-> 仓库[操作文件] <-> 比对文件 <-> .git[存储文件]

‍

**存储区**    Git软件用于存储资源得区域. 一般指得就是.git文件夹

          对应的文件状态是：`committed`​​​，文件已经安全的保存在本地数据库中. 

**工作区**    Git软件对外提供资源得区域，此区域可人工对资源进行处理.   
          对应的文件状态是：`modified`​​​，已修改，但还没保存到数据库中. 

**暂存区**    Git用于比对存储区域和工作区域得区域. Git根据对比得结果，可以对不同状态得文件执行操作.   
          对应的文件状态是：`staged`​​​，Git 已经对该文件做了标记，下次提交知道要包含它. 

‍

换种说法, git四个区域

3个本地区域

* 工作区(Workspace): 存放项目代码的地方.
* 暂存区(Stage): 存放临时的改动, 事实上它只是一个文件, 保存即将提交的文件列表信息.
* 资源库(Repository): 安全存放数据的位置, 这里面有提交到所有版本的数据. 其中 HEAD 指向最新放入仓库的版本.

1个远程区域

* 远程库(Remote): 托管代码的服务器.

‍

‍

## 分支

‍

### 主干分支

默认情况下，Git软件就存在分支的概念，而且就是一个分支，称之为master分支，也称之为主干分支. 

‍

这就意味着，所有文件的版本管理操作都是在master这一个分支路线上进行完成的. 

不过奇怪的是，为什么之前的操作没有体现这个概念呢，那是因为，默认的所有操作本身就都是基于master分支完成的. 而master主干分支在创建版本库时，也就是git init时默认就会创建. 

‍

‍

### 其他分支

就像之前说的，如果仅仅是一个分支，在某些情况并不能满足实际的需求，那么就需要创建多个不同的分支. 

‍

‍

# 基础

‍

‍

## 配置

‍

Git软件的配置文件位置为：**Git安装路径/etc/gitconfig**

‍

获取配置信息

```java
git config -l
```

修改 git 配置文件

```
$ git config -e            # 针对当前仓库
$ git config -e --global   # 针对系统上的所有仓库
```

设置提交代码时的用户信息

```
$ git config --global user.name "yourUserName"     # 去掉 --global 就只对当前仓库生效
$ git config --gloabl user.email "yourEmail"       # 去掉 --global 就只对当前仓库生效
```

‍

‍

‍

## 规范

‍

### 提交Commit

‍

暂时不需要, 建议

[优雅的提交你的 Git Commit Message - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/34223150)

‍

‍

### 分支命名

‍

|分支|命名|说明|
| ----------| -------------| ------------------------------------------------------------|
|主分支|master/main|主分支，所有提供给用户使用的正式版本，都在这个主分支上发布|
|开发分支|dev|开发分支，永远是功能最新最全的分支|
|功能分支|feature-*|新功能分支，某个功能点正在开发阶段|
|发布版本|release-*|发布定期要上线的功能|
|修复分支|bug-*|修复线上代码的 bug|
|临时分支|hotfix-*|紧急修复线上代码的 bug|

‍

‍

### 提交用户名

git终端,查看git用户名和邮箱地址命令：

```java
git config user.name
git config user.email
```

‍

修改全局用户名和邮箱地址：

```java
git config --global user.name "WenXing Cao(Intern)"
git config --global user.email "cwx02040673@alibaba-inc.com"
```

‍

‍

‍

# 指令

‍

‍

## 常用命令

‍

### 概要

‍

|命令名称|作用|
| ----------| --------------------|
|**git init**|初始化本地库|
|**git status**|查看本地库状态|
|**git add 文件名**|添加到暂存区|
|**git commit -m &quot;日志信息&quot; 文件名**|提交到本地库|
|**git reflog**|查看历史记录（简）|
|**git log**|查看历史记录（详）|
|**git reset --hard 版本号**|版本穿梭|

‍

分支操作

|命令名称|作用|
| ---------------------| ------------------------------|
|git branch 分支名|创建分支|
|git branch -v|查看分支|
|git checkout 分支名|切换分支|
|git merge 分支名|把指定的分支合并到当前分支上|

‍

远程仓库

|命令名称|作用|
| ------------------------------------| ----------------------------------------------------------|
|git remote -v|查看当前所有远程地址别名|
|git remote add 别名 远程地址|起别名|
|git push 别名 分支|推送本地分支上的内容到远程仓库|
|git clone 远程地址|将远程仓库的内容克隆到本地|
|git pull 远程库地址别名 远程分支名|将远程仓库对于分支最新内容拉下来后与当前本地分支直接合并|

‍

‍

### 本地

‍

#### git tag

创建标签(查看所有标签)

tagname 对当前版本建立标签

tagname commit_id 对历史版本建立标签

-a tagname -m “描述…” commit_id 添加说明

git show tagname 查看某个标签具体信息

-d tagname 删除本地标签

‍

git push [remotename] [tagname]：推送到远程仓库

git push [remotename] --tags：推送所有的标签

‍

git checkout tag-name：切换标签

git tag -d tag-name：删除本地标签

git push origin :refs/tags/ tag-name：删除远程标签

‍

#### git push origin tagname

推送本地的某个标签到远程

git push origin –tags 一次性推送所有分支

git push origin :refs/tags/tagname 删除远程标签

‍

‍

#### git add

文件提交到暂存区

```bash
# 1. 该命令可以将文件添加到暂存区
git add [file1] [file2] ...

# 2. 添加指定目录到暂存区
git add [dir]

# 3. 添加当前目录下所有文件进入暂存区
git add .
```

这里如果文件改动的比较多，但又不是每个都需要提交可设置 `git ignore file`​，就表示这些文件不要提交，比如在 build project 的时候会自动生成的那些文件等等. (排除列表)

‍

‍

#### git commit

提交暂存区(指定文件)到本地仓库

```
git commit [file1] [file2] ... -m [message]
```

一般后面都会跟个 `-m`​ 加句 `comment`​，简单说下改动的内容或者原因

那如果想再改，再重新 `git add`​​ 即可，但是 `commit`​​ 这句需要改成

```
git commit --amend
```

‍

‍

#### git log

​​git log​​ 可以查看到提交过的信息，从近到远显示每次 commit 的 comment 还有作者、日期等信息

```
commit 5abcd17dggs9s0a7a91nfsagd8ay76875afs7d6
Author: Xiaoqi<xxx@163.com>
Date: xxx xxx xxx
改了 Test 文件
```

commit 后面的这个`编号`​，是每次历史记录的一个`索引`​. 比如如果需要对版本进行前进或者后退的时候，就需要用到它. 

‍

简洁打印

```
git log --oneline
```

更常用

```
git reflog
```

‍

‍

#### git reset

前进或退回到某个版本

```
git reset --hard <编号>
```

‍

参考流程

> 工作区  **-git add-&gt;**  暂存区  **-git commit-&gt;**  本地库  
> **本地库的代码跳到那个版本之后，工作区和暂存区的代码就和本地库的不同步了呀！**

‍

那这些参数就是用来控制这些是否**同步**的

3 个参数：`hard`​, `soft`​, `mixed`​

$ git reset --hard xxx

**三个区都同步，都跳到这个 xxx 的版本上. **

‍

$ git reset --soft xxx

前面两个区不同步，就只有本地库跳到这个版本. 

‍

$ git reset --mixed xxx

暂存区同步，工作区不动. 

‍

所以呢，用的多的就是 hard.

‍

‍

#### git status

查看在你上次提交之后**是否有对文件进行再次修改**

列表显示代号

* A 表示新提交
* M 表示提交过，并且本地又修改了
* AM 表示有改动

‍

‍

#### git diff

显示已经**写入暂存区**和已经被修改但尚未写入暂存区文件的区别

‍

1. 尚未缓存的改动: git diff
2. 查看已经缓存的改动: git diff --cached
3. 查看缓存成功和未缓存的所有改动: git diff HEAD
4. 显示摘要而非整个diff: git diff --stat

‍

#### git ls-files

查看暂存区的文件

```
git ls-files
```

* -c: 默认
* -d: 显示删除的文件
* -m: 显示被修改过的文件
* -o: 显示没有被 git 跟踪过的文件
* -s: 显示 mode 以及对应的 Blog对象, 进而可以获取暂存区中对应文件的内容

‍

#### git cat-file -p

查看暂存区文件中的内容

‍

‍

#### git rm

暂存区和工作区中删除文件

强制删除    -f

从暂存区删除，在工作区保留    --cached

‍

‍

#### git stash

将现在的 **工作区**全部的修改、新增、删除等操作，全部保存起来

‍

参数

save 'save message'    执行存储时, 添加备注, 方便查找, 当然只执行 `git stash`​ 也是可以的, 但查找时不方便. 

list    查看 stash 了哪些存储. 

‍

show    显示做了哪些改动, 默认 show 第一个存储, 如果要显示其他的存储, 后面加 `stash@{$num}`​, 比如第二个: `git stash show stash@{1}`​

‍

show -p    显示第一个存储的改动, 如果想显示其他存储, 则: `git stash show stash@{$num} -p`​, 比如第二个: `git stash show stash@{1} -p`​

‍

apply    应用某个存储, 但不会把存储从存储列表中删除, 默认使用第一个存储, 即 `stash@{0}`​, 如果要是用其他, 则: `git stash apply stash@{$num}`​, 比如第二个: `git stash apply stash@{1}`​

‍

pop    恢复之前缓存的工作目录, 将缓存列表中对应的 stash 删除, 并将对应修改应用到当前的工作目录下, 默认为第一个 stash, 即 `stash@{0}`​, 如果要应用并删除其他 stash, 则: `git stash pop stash@{$num}`​, 比如应用并删除第二个: `git stash pop stash@{1}`​

‍

drop stash@{num}: 丢弃 `stash@{num}`​ 存储, 从列表中删除这个存储

​​clear​    删除所有缓存的 stash

‍

操作的文件范围

* 会贮存:

  * 添加到暂存区的修改（staged changes）
  * git跟踪的但并未添加到暂存区的修改（unstaged changes）
* 不会贮存:

  * 在工作目录中新的文件（untracked files）
  * 被忽略的文件（ignored files）

‍

‍

### 远程

和远程库的交互主要是`推`​、`拉`​，也就是写入和读取

‍

远程

git branch   

‍

#### git remote

查看远程库信息

-v    详细信息

‍

‍

#### git push

推到远程（若远程无对应分支，则推送无效）

‍

#### git clone

clone 整个项目到本地

> 当然了实际工作中也没人这么用. . 因为每家公司都会有自己包装的工具. 不过如果是做 Github 上的开源项目，就用得上了.

```git
$ git clone https://github.com/goto456/leetcode.git // 通过https方式克隆
$ git clone git@github.com:goto456/leetcode.git // 通过ssh方式克隆
```

以上2种方式有如下区别：

> **1. https方式**：不管是谁，只要拿到该项目的 url 可以随便 clone，但是在 push 到远程的时候需要验证用户名和密码；  
> **2. ssh方式**：需要现将你电脑的SSH key（SSH公钥）添加到GitHub（或者其他代码托管网站）上，这样在 clone 项目和 push 项目到远程时都不需要输入用户名和密码.

‍

‍

#### git pull

远程直接拉到本地

​​git pull = fetch + merge​​

> 如果当前本地仓库不是从远程仓库克隆，而是本地创建的仓库，并且**仓库中存在文件**，此时再从远程仓库拉取文件的时候会报错（fatal: refusing to merge unrelated histories ），解决此问题可以在 git pull 命令后加入参数 --allow-unrelated-histories

‍

#### git fetch

数据下载到本地库，但是工作区中的文件没有更新

‍

#### git merge

merge 是 git pull 默认的选项，能保留原有的两条分支和其树上的节点信息

‍

#### git rebase

rebase 是 git pull 另外的选项, 作用更多的是来整合分叉的历史，可以将某个分支上的所有修改都移到另一分支上，就像是变了基底

‍

‍

### 分支

‍

#### git branch

查看分支

‍

​​-v​     显示更多

-a    所有

-r    远程

‍

‍

git branch -d XXX        删除分支

–set-upstream branch-name origin/branch-name 建立本地分支和远程分支的关联

‍

git push origin –d branch-name：删除远程仓库中的分支 （origin 是引用名）

> 如果要删除的分支中进行了开发动作，此时执行删除命令并不会删除分支，如果坚持要删除此分支，可以将命令中的 -d 参数改为 -D：git branch -D branch-name

‍

#### git checkout <branchName>

切换分支

git checkout -b XXX master    基于当前分支创建一个新分支, 并切换到该分支(-b)

‍

#### git merge <branchName>

合并指定分支到当前分支

> 对同一个文件的同一个部分进行了不同的修改，Git 就没办法合并它们，同时会提示文件冲突。此时需要我们打开冲突的文件并修复冲突内容，最后执行 git add 命令来标识冲突已解决

‍

‍

‍

## 高级指令

‍

### 撤销操作

‍

**disk**

|command|description|
| --------------| -----------------------|
|查看修改|​`git diff`​|
|查看状态|​`git status`​ -> `Changes not staged for comit`​|
|撤销文件修改|​`git checkout <change_file> or git restore <change_file>`​|
|提交暂存区|git add <change_file>|

‍

**暂存区**

|command|description|
| ----------------------------------| -------------|
|查看状态|​`git status`​ -> `Changes to be committed(绿色)`​|
|从暂存区移除，但保留硬盘上的修改|​`git reset <change_file>`​ or `git restore --staged <change_file>`​|
|从暂存区移除，不保留硬盘上的修改|​`git checkout HEAD <change_file>`​|
|提交本地git|​`git commit`​|

‍

**local**

|command|description|
| ------------------------------------------------| -------------|
|撤销commit(保留磁盘上的修改和暂存区记录)|​`git reset --soft HEAD~1`​|
|撤销commit(清除暂存区记录, 只保留磁盘上的修改)|​`git reset HEAD~1`​ == `git reset --mixed HEAS~1`​|
|撤销commit(清除暂存区记录, 清除磁盘上的修改)|​`git reset --hard HEAD~1`​|
|生成新的`commitId`​,将上一个`commit+`​的内容变成`commit-`​|​`git revert HEAD`​|
|提交远端git|​`git push`​|

‍

git reset & git revert

1. ​`git reset`​: 只能回到之前某一个commit的状态.
2. ​`git revert`​:撤销中间任意一个commit. `git revert 70a0;(git revert HEAD~1)`​

‍

如果操作项目的分支是公共分支，只能通过 `git revert`​ 生成一个新的 commitId，从这个结果上撤销我们之前的修改. 

1. ​`git revert HEAD`​
2. ​`git push`​

‍

如果操作项目的分支是个人分支，可以通过`git reset`​撤销我们之前的修改

1. ​`git reset --hard HEAD~1`​
2. ​`git push -f`​

‍

‍

### 找回文件

‍

#### 找回commit了的丢失文件

‍

恢复因为执行 `git reset --hard COMMITID`​ 丢失的文件

```bash
# 重新创建一个项目
$ cd .. && rm -rf git-study && mkdir git-study && cd git-study && git init
$ echo 'master message 1' >> master_1.txt
$ git add master_1.txt
$ git commit -m 'first commit'
$ echo 'master message 2' >> master_2.txt
$ git add master_2.txt
$ git commit -m 'No.2 commit'

# 在这两次commit的基础上, reset 到第一次(first commit)上
$ git log   # 获取第一次commitid
$ git reset --hard 4a9bcb880db85a1ca77807dea9b3adce29dc4fda
# 再次查看 log 信息, 此时可以看见只有一次commit了, 第二次 commit(No.2 commit) 已经丢失
$ git log -n 2
```

‍

git 提供了 `git reflog`​ 用来记录你的每一次改变目录树的命令，使用好他就可以很方便的恢复你的提交：

```
4a9bcb8 (HEAD -> master) HEAD@{0}: reset: moving to 4a9bcb880db85a1ca77807dea9b3adce29dc4fda
80258ce HEAD@{1}: commit: No.2 commit
4a9bcb8 (HEAD -> master) HEAD@{2}: commit (initial): first commit
```

‍

可以看到最上面一条记录是将 HEAD 重新指向第一次的commit了, 同时也有显示第二次 commit 的 commitid, 有了这个 commitid, 就可以回滚了. 

```
$ git reset --hard 80258ce
HEAD is now at 80258ce No.2 commit
$ git log
commit 80258ce0146f373d15a1991d61af4061687782bc (HEAD -> master)
Author: kino <kino@gmail.com>
Date:   Thu Nov 24 02:26:10 2022 +0800

    No.2 commit

commit 4a9bcb880db85a1ca77807dea9b3adce29dc4fda
Author: kino <kino@gmail.com>
Date:   Thu Nov 24 02:25:06 2022 +0800

    first commit
```

‍

可以看到, commit 已被找回.

‍

但是通常情况下, 可能会出现在 `git reset`​ 之后, 还有新的 commit, 如果直接 `reset`​ 恢复的 commit, 肯定会造成新的 commit 又丢失, 所以如果我们只是想恢复这个一个 commit, 可以使用 `git cherry-pick commitid`​ 来单独将这个 commitid 恢复到当前分支或者用 `git merge`​ 来做合并

```
$ git cherry-pick 04b0396
[master fbf401a] No.2 commit
 Date: Thu Nov 24 02:38:14 2022 +0800
 1 file changed, 1 insertion(+)
 create mode 100644 master_2.txt
 
$ git log
commit fbf401a96bd9831c18ed02e9ee852cef8111ccb1 (HEAD -> master)
Author: kino <kino@gmail.com>
Date:   Thu Nov 24 02:38:14 2022 +0800

    No.2 commit

commit 1b5bfdb36ad01fb86d94b76654347f5de5475f37
Author: kino <kino@gmail.com>
Date:   Thu Nov 24 02:38:05 2022 +0800

    first commit
```

‍

‍

#### 找回未commit,但添加暂存区的丢失文件

‍

如果只 `git add`​ 了没有 `git commit`​(如果连 `git add`​都没有, 那只能找磁盘数据恢复(回收站)的方式了), 这就不是仅仅一个 `git reflog`​ 就能找回的了. 

‍

```
$ cd .. && rm -rf git-study && mkdir git-study && cd git-study && git init
$ echo 'master message 1' >> master_1.txt
$ git add master_1.txt
$ git commit -m 'first commit'
$ echo 'master message 2' >> master_2.txt
$ git add master_2.txt
$ git commit -m 'No.2 commit'
$ echo 'master message 3' >> master_3.txt
$ git add .

# 查看 log
$ git log -n 2
# 取最新的一次 commit id
$ git reset --hard ee614a48f753479a111723ae7ad926e0750ffa6c
# 查看 status
$ git status 
On branch master
nothing to commit, working tree clean
# 查看本地文件
total 16
-rw-r--r--  1 kino  staff    17B 11 24 02:43 master_1.txt
-rw-r--r--  1 kino  staff    17B 11 24 02:43 master_2.txt
# 可以看见文件已经丢了
```

‍

git 提供了 `git fsck --lost-found`​ 命令, 他会通过一些神奇的方式把历史操作过的文件以某种算法算出来加到.git/lost-found文件夹里，输出的记录就像下面这个样子. 

```
❯ git fsck --lost-found
Checking object directories: 100% (256/256), done.
dangling blob adbd4c8bf64367fb685336a67f02c5716dc47d73
```

‍

这里返回的第一行带有 `blob`​ 的信息，我们可以用 `git show`​来查看里面的内容

```
$ git show adbd4c8bf64367fb685336a67f02c5716dc47d73
master message 3

# 比如可以将内容追加到新文件中 
$ git show adbd4c8bf64367fb685336a67f02c5716dc47d73 > master_3.txt
```

‍

小记: 如果你的提交记录多的话, `git fsck --lost-found`​ 可以看见很多内容, 如下

```
$ git fsck --lost-found
Checking object directories: 100% (256/256), done.
Checking objects: 100% (35559/35559), done.
dangling blob 601e8abff177a0b2f8a31944654c0cdf0dd1f197
......
```

‍

‍

可以看到这里有`blob`​、`commit`​、`tree`​类型的数据，其实还有`tag`​等类型的, 这里需要了解下 git 的底层存储

* ​`commit`​ 数据结构在每次提交之后都会生成一个, 当我们进行 `commit`​ 之后, 首先会创建一个 `commit`​ 组件, 之后创建一个 `tree`​ 组件, 把所有的文件信息都存在里面, 每个 `blob`​ 都代表一个文件, 都可以在 `tree`​ 里面找到.
* ​`blob`​ 组件并不会对文件信息进行存储, 而是只对文件的内容进行记录, 文件信息存储在 `tree`​ 里.

‍

‍

#### 找回文件终极大招

‍

去看看最近修改的文件

```
$ find .git/objects -type f | xargs ls -lt | sed 3q
-r--r--r--  1 kino  staff   33 11 24 02:43 .git/objects/ad/bd4c8bf64367fb685336a67f02c5716dc47d73
-r--r--r--  1 kino  staff   33 11 24 02:43 .git/objects/cc/6e4eeea4f70e784fade7a18bdba6c28f7642e8
-r--r--r--  1 kino  staff   33 11 24 02:43 .git/objects/24/b6cb352efeff7a2b24b99e8ff814ab1fc2a2fd
```

‍

使用 `git cat-file -t commitid`​ 可以看见是什么类型的

```
$ git cat-file -t adbd4c8bf64367fb685336a67f02c5716dc47d73
blob

$ git cat-file -t cc6e4eeea4f70e784fade7a18bdba6c28f7642e8
blob

$ git cat-file -t 24b6cb352efeff7a2b24b99e8ff814ab1fc2a2fd
blob
```

‍

再使用 `git cat-file -p commitid`​ 查看内容

```
$ git cat-file -p adbd4c8bf64367fb685336a67f02c5716dc47d73
master message 3

$ git cat-file -p cc6e4eeea4f70e784fade7a18bdba6c28f7642e8
master message 2

$ git cat-file -p 24b6cb352efeff7a2b24b99e8ff814ab1fc2a2fd
master message 1
```

‍

‍

‍

# 高级

‍

‍

## 代理配置

注意端口配置, 查看计算机网络配置区域

```java
git config --global http.proxy http://127.0.0.1:7890
```

取消代理： git config --global --unset http.proxy

查询是否使用：git config --global http.proxy

‍

## Git迁移Gitlab

(保留 commit )

‍

### clone 原来的项目

```
git clone --bare git://github.com/username/project.git
```

‍

### 推送到新的gitlab

```
cd project
git push --mirror git@example.com/username/newproject.git
```

若提示没有权限, 在gitlab中把项目的权限保护关掉就好了

‍

### 本地代码更换gitlab地址

```
$ git remote set-url origin git@example.com/username/newproject.git
```

‍

## Rebase特性

[Link](https://blog.csdn.net/weixin_42310154/article/details/119004977), 有删节

‍

rebase 命令行操作

```bash
git rebase -i your_commit_id
```

‍

Rebase应用时刻

1.当我们在一个**过时的分支**上面开发的时候，执行 `rebase`​ 以此同步 `master`​ 分支最新变动；

2.假如我们要启动一个放置了很久的并行工作，现在有时间来继续这件事情，很显然这个分支已经落后了. 这时候需要在**最新的基准**上面开始工作，所以 `rebase`​ 是最合适的选择. 

‍

rebase 是将 被rebase 分支的commit 放到最前面. rebase 的时候, 找到**当前分支**和**被rebase分支**的父commit, 然后找到当前分支在父commit之后所有的commit记录, 把这些 commit 记录移动到被 rebase 分支上去. 如此一来, 这些 commit 记录已经不是原来的 commit 了(因为 commit id 已经改变了)

‍

**优缺点**

在后续开发中, 如果 feature 分支需要回退版本, 那么这将很好追溯代码; 如果我们使用 merge 将 master 提交的代码合并到 feature 分支, 那回退版本就得别人提交的代码也删掉了.

同样的, 因为 rebase 会让当前**分支的 commit 重新生成, 这会改变分支的历史**. 在 push 到远程分支的时候, 会提示你的代码和远程分支不一致, 这就需要强制 push ==(强推)==了(`git push --force-with-lease origin mybranch`​)

所以, **千万不要在公共分支上使用 rebase, 历史被打乱是一件很严重的事情!!!**

‍

‍

**建议**

1. 在公共分支上不要使用 rebase, 应该用 merge保留两个分枝树的原有状态
2. 功能分支上, 可以选择 rebase(不介意时间顺序, 把自己的 commit 顶到最后).

‍

‍

## 彻底删除Git上的文件

[参考](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository)

Github for Windows/Mac桌面应用以及网页版都没有提供**清除某个文件操作记录**的功能，就是说即便你删了这个文件重新Push，那么别人依然可以查看你上一个版本.   
所以我们需要的是把和这个文件有关的所有Commit等记录全部删掉当然也包括文件本身. 首先在Git Bash或者CMD或者PowerShell中cd进入到你的本地项目文件夹，然后依次执行下面6行命令即可：

‍

​`FILE_PATH：你要删除的文件的路径`​

```java
git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch FILE_PATH' --prune-empty --tag-name-filter cat -- --all
git push origin main --force
rm -rf .git/refs/original/
git reflog expire --expire=now --all
git gc --prune=now
git gc --aggressive --prune=now
```

‍

‍

## 镜像

‍

[官网下载](https://git-scm.com/downloads)

[Git下载国内镜像地址](https://npm.taobao.org/mirrors/git-for-windows/)

‍

‍

## 乱码解决

‍

### 乱码情景1

> 在cygwin中，使用git add添加要提交的文件的时候，如果文件名是中文，会显示形如 `274\232\350\256\256\346\200\273\347\273\223.png`​ 的乱码.

‍

解决方案：

在bash提示符下输入：

```bash
git config --global core.quotepath false
```

core.quotepath设为false的话，就不会对0x80以上的字符进行quote. 中文显示正常. 

‍

‍

### 乱码情景2

> 在MsysGit中，使用git log显示提交的中文log乱码.

‍

解决方案：

设置git gui的界面编码  
​`git config --global gui.encoding utf-8`​

设置 commit log 提交时使用 utf-8 编码，可避免服务器上乱码，同时与linux上的提交保持一致！

```bash
git config --global i18n.commitencoding utf-8
```

使得在 $ git log 时将 utf-8 编码转换成 gbk 编码，解决Msys bash中git log 乱码. 

```bash
git config --global i18n.logoutputencoding gbk
```

使得 git log 可以正常显示中文（配合i18n.logoutputencoding = gbk)，在 `/etc/profile`​ 中添加：

```bash
export LESSCHARSET=utf-8
```

‍

### 乱码情景3

> 在MsysGit自带的bash中，使用ls命令查看中文文件名乱码. cygwin没有这个问题.

‍

解决方案：

使用 `ls --show-control-chars`​ 命令来强制使用控制台字符编码显示文件名，即可查看中文文件名. 

为了方便使用，可以编辑 `/etc/git-completion.bash`​ ，新增一行 `alias ls="ls --show-control-chars"`​

参考： [git乱码解决方案汇总](https://blog.zengrong.net/post/git-codec-issues/)

‍

‍

## 放弃本地修改强制拉取更新

‍

对于本地已修改但又不想保留的代码（比如你代码改崩了），可以用如下两种方法来重置代码

‍

### restore 重置

如果你修改了代码，但是并未执行 `git add`​ 操作，可直接执行：

```bash
git restore .
```

. 表示所有文件，如果想重置个别文件，指定文件路径即可

```bash
git restore [文件]...
```

> 注意⚠️：如果你已经执行了 `git add`​ 操作，此时代码已保存至暂存区，需要先取消暂存区变更：

```bash
git restore --staged .
```

或者

```bash
git reset .
```

然后，再执行 `git pull`​ 拉取远程代码同步即可. 

‍

‍

### reset 回退 ★

​`reset`​ 比较暴力，相当于 可适用于 代码在工作区、暂存区、仓库区等任何场景，重置后不可恢复🙅‍♂️，对于新手有一定的安全隐患. 

```bash
git fetch --all
git reset --hard origin/master  # 根据实际情况修改分支名
git pull  # 这一步为了同步远程代码，不需要的话可不执行
```

* git fetch 指令是下载远程仓库最新内容，不做合并.
* git reset 指令把HEAD指向master最新版本.

> reset --hard：重置后不保留暂存区和工作区  
> reset --soft：保留工作区，并把重置 HEAD 所带来的新的差异放进暂存区（此时代码的变更状态相当于执行完 `git add`​命令）  
> reset --mixed：reset的默认参数，保留工作目录，并重置暂存区（此时代码的变更状态相当于执行 `git add`​命令之前）

‍

‍

### stash 暂存（推荐）

我比较喜欢的方法，是用stash，暂存代码再同步. 

首先，将所有代码添加至暂存区：

```bash
git add .
```

然后，将代码临时保存：

```bash
git stash
```

此时代码会重置到修改前的状态，可以同步远程仓库区，完事儿. 

```bash
git pull
```

同步后，**如果**还想继续修改原来的代码，可将临时代码恢复至工作区：

```bash
git stash pop
```

> 注意⚠️，stash 用法有很多，比如save，push，pop，clear等，需要使用可以查阅[stash 命令](https://git-scm.com/docs/git-stash)

参考

* [CSDN-git 放弃本地修改，强制拉取更新](https://blog.csdn.net/haoaiqian/article/details/78284337)
* [git官方文档](https://git-scm.com/docs/)
* [菜鸟教程-Git教程](https://www.runoob.com/git/git-tutorial.html)

‍

‍

# 联动

‍

‍

## IDEA项目整合

IDEA项目中可以选择上传至仓库, 需要设置上传内容, 获得更改, 需要Fetch/更新

同样可以选择克隆github仓库等 便捷图形化操作, 需要开启Git和Github插件

‍

### 设置gitignore

gitignore列表可以从Github获取

‍

## Git远程服务器

在Linux服务器中搭建Git服务器

‍

‍

# 实操

‍

整理>>版本控制个人使用记录

‍

‍

提交旁搁置功能: 能够存储因为签出回滚失败的提交, 防呆
