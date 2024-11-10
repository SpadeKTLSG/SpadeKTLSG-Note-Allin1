--Linux傻瓜式安装软件

‍

### Header

Docker是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可抑制的容器中，然后发布到任何流行的Linux机器上，也可以实现虚拟化. 容器完全使用沙盒机制，相互之间不会存在任何接口. 几乎没有性能开销，可以很容易的在机器和数据中心运行. 最重要的是，他们不依赖于任何语言、框架或者包装系统. 

主要参考[bili链接](https://www.bilibili.com/video/BV1HP4118797/?spm_id_from=333.999.top_right_bar_window_history.content.click&vd_source=988afe11dce25cad2e9145dda6aca188) [云文档链接](https://b11et3un53m.feishu.cn/wiki/MWQIw4Zvhil0I5ktPHwcoqZdnec)

就是会用即可, 拿来别人的镜像开启来用就可以

[参考Docker官方文档](https://docs.docker.com/)

镜像

[Docker/DockerHub 国内镜像源/加速列表（长期维护 10 月 6 日更新） - 轩源的网络日志 - Xuanyuan&apos;s Blog](https://xuanyuan.me/blog/archives/1154#:~:text=%E4%B8%BA%E4%BA%86%E5%8A%A0%E9%80%9F%E9%95%9C%E5%83%8F%E6%8B%89%E5%8F%96%EF%BC%8C%E4%BD%BF)

‍

**能力要求**

* 能利用Docker部署常见软件
* 能利用Docker打包并部署Java应用
* 理解Docker数据卷的基本作用
* 能看懂DockerCompose文件

企业开发就是写docker然后运维给你部署了

‍

‍

‍

#### 环境

‍

##### GFW

docker现在国服是禁止了，如果需要拉镜像的话，需要开一个私有的docker库，把国外的镜像拉下来放到这个库里，用这个库去拉镜像，才能成功

‍

‍

‍

# 知识

‍

## 工作流程

使用基础指令获取镜像后, Docker会自动搜索并下载MySQL. 镜像中不仅包含了MySQL本身，还包含了其运行所需要的环境、配置、系统级函数库. 因此它在运行时就有自己独立的环境，就可以跨系统运行，也不需要手动再次配置环境了. 这套独立运行的隔离环境我们称为**容器**. 

‍

* 镜像image
* 容器container

‍

因此，Docker安装软件的过程，就是自动搜索下载镜像，然后创建并运行容器的过程. 

‍

‍

‍

## 镜像管理服务器

Docker官方提供了一个专门管理、存储镜像的网站，并对外开放了镜像上传、下载的权利. Docker官方提供了一些基础镜像，然后各大软件公司又在基础镜像基础上，制作了自家软件的镜像，全部都存放在这个网站. 这个网站就成了Docker镜像交流的[社区](https://hub.docker.com/)

‍

提供存储、管理Docker镜像的服务器，被称为DockerRegistry镜像仓库

DockerHub网站是官方仓库，阿里云、华为云会提供一些第三方仓库，我们也可以自己搭建私有的镜像仓库

‍

Docker本身包含一个后台服务，我们可以利用Docker命令告诉Docker服务，帮助我们快速部署指定的应用. Docker服务部署应用时，首先要去搜索并下载应用对应的镜像，然后根据镜像创建并允许容器，应用就部署完成了. 

‍

‍

## 用途

（1）提供一次性的环境。比如，本地测试他人的软件、持续集成的时候提供单元测试和构建的环境。DevOps & CI/CD

（2）提供弹性的云服务。因为 Docker 容器可以随开随关，很适合动态扩容和缩容。

（3）组建微服务架构。通过多个容器，一台机器可以跑多个服务，因此在本机就可以模拟出微服务架构。

> * 传统：开发jar，运维部署。
> * 现在：开发jar部署上线，运维负责后续问题处理

‍

‍

‍

## 概念

‍

### 虚拟化技术

‍

**虚拟机和docker两种虚拟化技术**。

* 虚拟机，虚拟出一条硬件，运行一个完整的操作系统，然后在系统上安装运行软件
* 容器，直接运行在宿主机的内核中，容器没有自己的内核，也没有虚拟硬件。轻便很多。
* 每个容器是相互隔离的，每个容器内都有一个属于自己的文件系统

‍

### 虚拟机

虚拟机（virtual machine）就是带环境安装的一种解决方案。它可以在一种操作系统里面运行另一种操作系统，比如在 Windows 系统里面运行 Linux 系统。应用程序对此毫无感知，因为虚拟机看上去跟真实系统一模一样，而对于底层系统来说，虚拟机就是一个普通文件，不需要了就删掉，对其他部分毫无影响。

虽然用户可以通过虚拟机还原软件的原始环境。但是，这个方案有几个缺点。

* （1）资源占用多

虚拟机会独占一部分内存和硬盘空间。它运行的时候，其他程序就不能使用这些资源了。哪怕虚拟机里面的应用程序，真正使用的内存只有 1MB，虚拟机依然需要几百 MB 的内存才能运行。

* （2）冗余步骤多

虚拟机是完整的操作系统，一些系统级别的操作步骤，往往无法跳过，比如用户登录。

* （3）启动慢

启动操作系统需要多久，启动虚拟机就需要多久。可能要等几分钟，应用程序才能真正运行。

‍

‍

### 镜像-容器

‍

**镜像（Image）** ：Docker将应用程序及其所需的依赖、函数库、环境、配置等文件打包在一起，称为镜像。

**容器（Container）** ：镜像中的应用程序运行后形成的进程就是**容器**，只是Docker会给容器进程做隔离，对外不可见。

‍

一切应用最终都是代码组成，都是硬盘中的一个个的字节形成的**文件**。只有运行时，才会加载到内存，形成进程。

而**镜像**，就是把一个应用在硬盘上的文件、及其运行环境、部分系统函数库文件一起打包形成的文件包。这个文件包是只读的。

**容器**呢，就是将这些文件中编写的程序、函数加载到内存中运行，形成进程，只不过要隔离起来。因此一个镜像可以启动多次，形成多个容器进程。

‍

‍

## 评价

‍

### 与虚拟机对比

‍

容器：

* 容器是将软件打包成标准化单元，以用于开发、交付和部署
* 容器镜像是轻量的、可执行的独立软件包 ，包含软件运行所需的所有内容：代码、运行时环境、系统工具、系统库和设置
* 容器化软件在任何环境中都能够始终如一地运行。
* 容器赋予了软件独立性，使其免受外在环境差异的影响，从而有助于减少团队间在相同基础设施上运行不同软件时的冲突

容器和虚拟机对比：

* 相同：容器和虚拟机具有相似的资源隔离和分配优势
* 不同：

  * 容器虚拟化的是操作系统，虚拟机虚拟化的是硬件。
  * 传统虚拟机可以运行不同的操作系统，容器只能运行同一类型操作系统

|特性|容器|虚拟机|
| ------------| --------------------| ------------|
|启动|秒级|分钟|
|硬盘使用|一般为MB|一般为GB|
|性能|接近原生|弱于原生|
|系统支持量|单机支持上千个容器|一般几十个|

‍

为什么比VM快？

* Docker有着比虚拟机更少的抽象层
* docker主要用的是宿主机的内核，vm需要Guest OS

‍

# 基础

‍

‍

## 命令

‍

参考[官方文档](https://docs.docker.com/engine/reference/commandline/cli/)

‍

### 进程相关

* 启动docker服务：

  ```bash
  systemctl start docker
  ```
* 停止docker服务：

  ```bash
  systemctl stop docker
  ```
* 重启doker服务：

  ```bash
  systemctl restart docker
  ```
* 查看doker服务状态：

  ```bash
  systemctl status docker
  ```
* 设置开机启动docker服务：

  ```bash
  systemctl enable docker
  ```

‍

### 镜像相关

* 查看镜像：查看本地所有的镜像

  ```bash
  docker images
  docker images –q # 查看所用镜像的id
  ```
* 搜索镜像：从网络中查找需要的镜像

  ```bash
  docker search 镜像名称
  ```
* 拉取镜像：从Docker仓库下载镜像到本地，镜像名称格式为 名称:版本号，如果版本号不指定则是最新的版本。如果不知道镜像版本，可以去docker hub 搜索对应镜像查看

  ```bash
  docker pull 镜像名称
  ```
* 删除镜像：删除本地镜像

  ```bash
  docker rmi 镜像id # 删除指定本地镜像
  docker rmi `docker images -q`  # 删除所有本地镜像 tab上面的键
  ```

‍

‍

### 容器相关

* 查看容器：

  ```bash
  docker ps # 查看正在运行的容器
  docker ps –a # 查看所有容器
  ```
* 创建并启动容器：

  ```bash
  docker run 参数  --name=... /bin/bash
  ```

  参数说明：

  * -i：保持容器运行，通常与 -t 同时使用，加入it这两个参数后，容器创建后自动进入容器中，退出容器后，容器自动关闭
  * -t：为容器重新分配一个伪输入终端，通常与 -i 同时使用
  * -d：以守护（后台）模式运行容器。创建一个容器在后台运行，需要使用docker exec 进入容器。退出后，容器不会关闭
  *  **-it 创建的容器一般称为交互式容器，-id 创建的容器一般称为守护式容器**
  *  **--name：为创建的容器命名**
* 进入容器：

  ```bash
  docker exec 参数 # 退出容器，容器不会关闭
  ```
* 停止容器：

  ```bash
  docker stop 容器名称
  ```
* 启动容器：

  ```bash
  docker start 容器名称
  ```
* 删除容器：如果容器是运行状态则删除失败，需要停止容器才能删除

  ```bash
  docker rm 容器名称
  ```
* 查看容器信息：

  ```bash
  docker inspect 容器名称
  ```

‍

‍

## 搭建

‍

‍

|**命令**|**说明**|**文档地址**|
| ----------------| --------------------------------| --|
|docker pull|拉取镜像|[docker pull](https://docs.docker.com/engine/reference/commandline/pull/)|
|docker push|推送镜像到DockerRegistry|[docker push](https://docs.docker.com/engine/reference/commandline/push/)|
|docker images|查看本地镜像|[docker images](https://docs.docker.com/engine/reference/commandline/images/)|
|docker rmi|删除本地镜像|[docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/)|
|docker run|创建并运行容器（不能重复创建）|[docker run](https://docs.docker.com/engine/reference/commandline/run/)|
|docker stop|停止指定容器|[docker stop](https://docs.docker.com/engine/reference/commandline/stop/)|
|docker start|启动指定容器|[docker start](https://docs.docker.com/engine/reference/commandline/start/)|
|docker restart|重新启动容器|[docker restart](https://docs.docker.com/engine/reference/commandline/restart/)|
|docker rm|删除指定容器|[docs.docker.com](https://docs.docker.com/engine/reference/commandline/rm/)|
|docker ps|查看容器|[docker ps](https://docs.docker.com/engine/reference/commandline/ps/)|
|docker logs|查看容器运行日志|[docker logs](https://docs.docker.com/engine/reference/commandline/logs/)|
|docker exec|进入容器|[docker exec](https://docs.docker.com/engine/reference/commandline/exec/)|
|docker save|保存镜像到本地压缩文件|[docker save](https://docs.docker.com/engine/reference/commandline/save/)|
|docker load|加载本地压缩文件到镜像|[docker load](https://docs.docker.com/engine/reference/commandline/load/)|
|docker inspect|查看容器详细信息|[docker inspect](https://docs.docker.com/engine/reference/commandline/inspect/)|

‍

‍

这些命令的关系

​![](https://pic1.zhimg.com/v2-46b7f8ac9a4c23ef14f79bf34be368d4_r.jpg)​

‍

‍

### 安装

‍

> 这个默认是CentOS的yum管理, [Ubantu](https://zhuanlan.zhihu.com/p/651148141)的参考链接

**旧的Docker先卸载**

```Shell
yum remove docker \
    docker-client \
    docker-client-latest \
    docker-common \
    docker-latest \
    docker-latest-logrotate \
    docker-logrotate \
    docker-engine
```

‍

**配置Docker的yum库**

首先要安装一个yum工具

```Bash
yum install -y yum-utils
```

安装成功后，执行命令，配置Docker的yum源

```Bash
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

‍

**安装Docker**

```Bash
yum install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

‍

**启动和校验**

```Bash
# 启动Docker
systemctl start docker

# 停止Docker
systemctl stop docker

# 重启
systemctl restart docker

# 设置开机自启
systemctl enable docker

# 执行docker ps命令，如果不报错，说明安装启动成功
docker ps
```

‍

**配置镜像加速**

‍

‍

‍

‍

### 开机自启

```PowerShell
# Docker开机自启
systemctl enable docker

# Docker容器开机自启
docker update --restart=always [容器名/容器id]
```

‍

### 配置镜像加速

‍

阿里云镜像加速

官方仓库在国外，下载速度较慢，一般我们都会使用第三方仓库提供的镜像加速功能，提高下载速度. 而企业内部的机密项目，往往会采用私有镜像仓库. 

‍

**开通镜像服务**

在首页的产品中，找到阿里云的**容器镜像服务,** 点击后进入[控制台](https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors)

‍

**配置镜像加速**

找到**镜像工具**下的**镜像加速器**：

[地址](https://yogu36eo.mirror.aliyuncs.com)

‍

```Bash
# 创建目录
mkdir -p /etc/docker

# 复制内容，注意把其中的镜像加速地址改成你自己的
tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://yogu36eo.mirror.aliyuncs.com"]
}
EOF

# 重新加载配置
systemctl daemon-reload

# 重启Docker
systemctl restart docker
```

在/etc/docker的里面找到了js文件即可

‍

‍

### 命令别名

‍

给常用Docker命令起别名，方便我们访问

```PowerShell
# 修改/root/.bashrc文件
vi /root/.bashrc
内容如下：
# .bashrc

# User specific aliases and functions

alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'
alias dps='docker ps --format "table {{.ID}}\t{{.Image}}\t{{.Ports}}\t{{.Status}}\t{{.Names}}"'
alias dis='docker images'

# Source global definitions
if [ -f /etc/bashrc ]; then
        . /etc/bashrc
fi
```

‍

执行命令使别名生效

```PowerShell
source /root/.bashrc
```

‍

## 实例

‍

‍

执行命令后，Docker做的第一件事情，是去自动搜索并下载了MySQL，然后会自动运行MySQL

这种安装方式你完全不用考虑运行的操作系统环境，它不仅仅在CentOS系统是这样，在Ubuntu系统、macOS系统、甚至是装了WSL的Windows下，都可以使用这条命令来安装MySQL

要知道，**不同操作系统下其安装包、运行环境是都不相同的**！如果是**手动安装，必须手动解决安装包不同、环境不同的、配置不同的问题**！

而使用Docker，这些完全不用考虑. 就是因为Docker会自动搜索并下载MySQL. 注意：这里下载的不是安装包，而是**镜像. ** 镜像中不仅包含了MySQL本身，还包含了其运行所需要的环境、配置、系统级函数库. 因此它在运行时就有自己独立的环境，就可以跨系统运行，也不需要手动再次配置环境了. 这套独立运行的隔离环境我们称为**容器**. 

‍

说明：

* 镜像：英文是image
* 容器：英文是container

> 因此，Docker安装软件的过程，就是自动搜索下载镜像，然后创建并运行容器的过程.

Docker会根据命令中的镜像名称自动搜索并下载镜像，那么问题来了，它是去哪里搜索和下载镜像的呢？这些镜像又是谁制作的呢？

Docker官方提供了一个专门管理、存储镜像的网站，并对外开放了镜像上传、下载的权利. Docker官方提供了一些基础镜像，然后各大软件公司又在基础镜像基础上，制作了自家软件的镜像，全部都存放在这个网站. 这个网站就成了Docker镜像交流的社区：

**https://hub.docker.com/**

基本上我们常用的各种软件都能在这个网站上找到，我们甚至可以自己制作镜像上传上去. 

像这种提供存储、管理Docker镜像的服务器，被称为DockerRegistry，可以翻译为镜像仓库. DockerHub网站是官方仓库，阿里云、华为云会提供一些第三方仓库，我们也可以自己搭建私有的镜像仓库. 

官方仓库在国外，下载速度较慢，一般我们都会使用第三方仓库提供的镜像加速功能，提高下载速度. 而企业内部的机密项目，往往会采用私有镜像仓库. 

总之，镜像的来源有两种：

* 基于官方基础镜像自己制作
* 直接去DockerRegistry下载

**总结一下**：

Docker本身包含一个后台服务，我们可以利用Docker命令告诉Docker服务，帮助我们快速部署指定的应用. Docker服务部署应用时，首先要去搜索并下载应用对应的镜像，然后根据镜像创建并允许容器，应用就部署完成了. 

‍

‍

### 部署

‍

‍

#### MySQL

利用Docker来安装一个MySQL软件

‍

传统方式部署MySQL，大概的步骤

* 搜索并下载MySQL安装包
* 上传至Linux环境
* 编译和配置环境
* 安装

‍

而使用Docker安装，仅仅需要一步即可，在命令行输入下面的命令

```PowerShell
docker run -d \
  --name mysql \
  -p 3306:3306 \
  -e TZ=Asia/Shanghai \
  -e MYSQL_ROOT_PASSWORD=2333 \
  mysql
```

‍

利用Docker快速的安装了MySQL，非常的方便，不过我们执行的命令到底是什么意思呢？

快速安装MySQL解读

```PowerShell
docker run -d \
  --name mysql \
  -p 3306:3306 \
  -e TZ=Asia/Shanghai \
  -e MYSQL_ROOT_PASSWORD=123 \
  mysql
```

‍

* ​`docker run -d`​ ：创建并运行一个容器，`-d`​则是让容器以后台进程运行
* ​`--name mysql `​ : 给容器起个名字叫`mysql`​，你可以叫别的
* ​`-p 3306:3306`​ : 设置端口映射.

  * **容器是隔离环境**，外界不可访问. 但是可以**将宿主机端口映射容器内到端口**，当访问宿主机指定端口时，就是在访问容器内的端口了.
  * 容器内端口往往是由容器内的进程决定，例如MySQL进程默认端口是3306，因此容器内端口一定是3306；而宿主机端口则可以任意指定，一般与容器内保持一致.
  * 格式： `-p 宿主机端口:容器内端口`​，示例中就是将宿主机的3306映射到容器内的3306端口
* ​`-e TZ=Asia/Shanghai`​ : 配置容器内进程运行时的一些参数

  * 格式：`-e KEY=VALUE`​，KEY和VALUE都由容器内进程决定
  * 案例中，`TZ=Asia/Shanghai`​是设置时区；`MYSQL_ROOT_PASSWORD=123`​是设置MySQL默认密码
* ​`mysql`​ : 设置**镜像**名称，Docker会根据这个名字搜索并下载镜像

  * 格式：`REPOSITORY:TAG`​，例如`mysql:8.0`​，其中`REPOSITORY`​可以理解为镜像名，`TAG`​是版本号
  * 在未指定`TAG`​的情况下，默认是最新版本，也就是`mysql:latest`​

‍

镜像的名称不是随意的，而是要到DockerRegistry中寻找，镜像运行时的配置也不是随意的，要参考镜像的帮助文档，这些在DockerHub网站或者软件的官方网站中都能找到.

如果我们要安装其它软件，也可以到DockerRegistry中寻找对应的镜像名称和版本，阅读相关配置即可.

‍

‍

#### Nginx

```PowerShell
# 第1步，去DockerHub查看nginx镜像仓库及相关信息

# 第2步，拉取Nginx镜像
docker pull nginx

# 第3步，查看镜像
docker images
# 结果如下：
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
nginx        latest    605c77e624dd   16 months ago   141MB
mysql        latest    3218b38490ce   17 months ago   516MB

# 第4步，创建并允许Nginx容器
docker run -d --name nginx-my-new -p 81:81 nginx

# 第5步，查看运行中容器
docker ps
# 也可以加格式化方式访问，格式会更加清爽
docker ps --format "table {{.ID}}\t{{.Image}}\t{{.Ports}}\t{{.Status}}\t{{.Names}}"

# 第6步，访问网页，地址：http://虚拟机地址

# 第7步，停止容器
docker stop nginx

# 第8步，查看所有容器
docker ps -a --format "table {{.ID}}\t{{.Image}}\t{{.Ports}}\t{{.Status}}\t{{.Names}}"

# 第9步，再次启动nginx容器
docker start nginx

# 第10步，再次查看容器
docker ps --format "table {{.ID}}\t{{.Image}}\t{{.Ports}}\t{{.Status}}\t{{.Names}}"

# 第11步，查看容器详细信息
docker inspect nginx-my-new

# 第12步，进入容器,使用bash操作, 查看容器内目录
docker exec -it nginx bash
# 或者，可以进入MySQL
docker exec -it mysql mysql -uroot -p

# 第13步，删除容器
docker rm nginx
# 发现无法删除，因为容器运行中，强制删除容器
docker rm -f nginx
```

‍

‍

## 数据卷

‍

> 在容器内修改资源非常困难(没有cd,vi这样的命令), 需要使用数据卷操作

容器提供程序的运行环境，但是**程序运行产生的数据、程序运行依赖的配置都应该与容器解耦**. 

‍

**数据卷（volume）** 是一个虚拟目录(逻辑上的)，是**容器内目录**与**宿主机目录**之间映射的桥梁. 

‍

以Nginx为例，我们知道Nginx中有两个关键的目录：

* ​`html`​：放置一些静态资源
* ​`conf`​：放置配置文件

如果我们要让Nginx代理我们的静态资源，最好是放到`html`​目录；如果我们要修改Nginx的配置，最好是找到`conf`​下的`nginx.conf`​文件. 

但遗憾的是，容器运行的Nginx所有的文件都在容器内部. 所以我们必须利用数据卷将两个目录与宿主机目录关联，方便我们操作

​​

容器内的`conf`​和`html`​目录就 与宿主机的`conf`​和`html`​目录关联起来，我们称为**挂载**. 

此时，我们操作宿主机的`/var/lib/docker/volumes/html/_data`​就是在操作容器内的`/usr/share/nginx/html/_data`​目录. 只要我们将静态资源放入宿主机对应目录，就可以被Nginx代理了. 

‍

​`/var/lib/docker/volumes`​​这个目录就是默认的存放所有容器数据卷的目录，其下再根据数据卷名称创建新目录，格式为`/数据卷名/_data`​​. 

‍

**为什么不让容器目录直接指向宿主机目录呢**？

> * 因为直接指向宿主机目录就与宿主机强耦合了，如果切换了环境，宿主机目录就可能发生改变了. 由于容器一旦创建，目录挂载就无法修改，这样容器就无法正常工作了.
> * 但是容器指向数据卷，一个逻辑名称，而数据卷再指向宿主机目录，就不存在强耦合. 如果宿主机目录发生改变，只要改变数据卷与宿主机目录之间的映射关系即可.

由于数据卷目录比较深，不好寻找，通常我们也**允许让容器直接与宿主机目录挂载而不使用数据卷**

容器与数据卷的挂载要在创建容器时配置，对于创建好的容器，是不能设置数据卷的. 而且**创建容器的过程中，数据卷会自动创建**

每一个不同的镜像，将来创建容器后内部有哪些目录可以挂载，可以参考DockerHub对应的页面

‍

‍

### 作用

* 容器数据持久化
* 外部机器和容器间接通信
* 容器之间数据交换

‍

‍

### 数据卷

‍

创建容器时，可以通过 -v 参数来挂载一个数据卷到某个容器内目录

```bash
docker run \
  --name mn \
  -v html:/root/html \
  -p 8080:80
  nginx \
```

这里的-v就是挂载数据卷的命令：

* ​`-v html:/root/htm`​ ：把html数据卷挂载到容器内的/root/html这个目录中

‍

‍

#### nginx的html目录挂载e.g.

```PowerShell
# 1.首先创建容器并指定数据卷，注意通过 -v 参数来指定数据卷
docker run -d --name nginx -p 81:81 -v html:/usr/share/nginx/html nginx

# 2.查看数据卷以及详情
docker volume ls
docker volume inspect html

# 4.查看/var/lib/docker/volumes/html/_data目录
ll /var/lib/docker/volumes/html/_data

# 5.进入该目录，并修改index.html内容
cd /var/lib/docker/volumes/html/_data
vi index.html

# 6.打开页面，查看效果

# 7.进入容器内部，查看/usr/share/nginx/html目录内的文件是否变化
docker exec -it nginx bash
```

‍

‍

#### MySQL的匿名数据卷

```PowerShell
# 1.查看MySQL容器详细信息
docker inspect mysql
# 关注其中.Config.Volumes部分和.Mounts部分
```

我们关注两部分内容，第一是`.Config.Volumes`​部分：

```JSON
{
  "Config": {
    // ... 略
    "Volumes": {
      "/var/lib/mysql": {}
    }
    // ... 略
  }
}
```

‍

可以发现这个容器声明了一个本地目录，需要挂载数据卷，但是**数据卷未定义**. 这就是匿名卷.

然后，我们再看结果中的`.Mounts`​部分：

‍

```JSON
{
  "Mounts": [
    {
      "Type": "volume",
      "Name": "29524ff09715d3688eae3f99803a2796558dbd00ca584a25a4bbc193ca82459f",
      "Source": "/var/lib/docker/volumes/29524ff09715d3688eae3f99803a2796558dbd00ca584a25a4bbc193ca82459f/_data",
      "Destination": "/var/lib/mysql",
      "Driver": "local",
    }
  ]
}
```

‍

可以发现，其中有几个关键属性：

* Name：数据卷名称. 由于定义容器未设置容器名，这里的就是匿名卷自动生成的名字，一串hash值.
* Source：宿主机目录
* Destination : 容器内的目录

‍

上述配置是将容器内的`/var/lib/mysql`​这个目录，与数据卷`29524ff09715d3688eae3f99803a2796558dbd00ca584a25a4bbc193ca82459f`​挂载. 于是在宿主机中就有了`/var/lib/docker/volumes/29524ff09715d3688eae3f99803a2796558dbd00ca584a25a4bbc193ca82459f/_data`​这个目录. 这就是匿名数据卷对应的目录，其使用方式与普通数据卷没有差别.

‍

接下来，可以查看该目录下的MySQL的data文件

```Bash
ls -l /var/lib/docker/volumes/29524ff09715d3688eae3f99803a2796558dbd00ca584a25a4bbc193ca82459f/_data
```

‍

#### MySQL本地目录挂载

‍

mysql本地目录挂载

* 挂载`/root/mysql/data`​到容器内的`/var/lib/mysql`​目录
* 挂载`/root/mysql/init`​到容器内的`/docker-entrypoint-initdb.d`​目录（初始化的SQL脚本目录）
* 挂载`/root/mysql/conf`​到容器内的`/etc/mysql/conf.d`​目录（这个是MySQL配置文件目录）

‍

其中，hm.cnf主要是配置了MySQL的默认编码，改为utf8mb4；而hmall.sql则是后面我们要用到的黑马商城项目的初始化SQL脚本

‍

我们直接将整个mysql目录上传至虚拟机的`/root`​目录下

开始挂载

```Bash
# 1.删除原来的MySQL容器
docker rm -f mysql

# 2.进入root目录
cd ~

# 3.创建并运行新mysql容器，挂载本地目录
docker run -d \
  --name mysql \
  -p 3306:3306 \
  -e TZ=Asia/Shanghai \
  -e MYSQL_ROOT_PASSWORD=123 \
  -v ./mysql/data:/var/lib/mysql \
  -v ./mysql/conf:/etc/mysql/conf.d \
  -v ./mysql/init:/docker-entrypoint-initdb.d \
  mysql

# 4.查看root目录，可以发现~/mysql/data目录已经自动创建好了
ls -l mysql
# 结果：
总用量 4
drwxr-xr-x. 2 root    root   20 5月  19 15:11 conf
drwxr-xr-x. 7 polkitd root 4096 5月  19 15:11 data
drwxr-xr-x. 2 root    root   23 5月  19 15:11 init

# 查看data目录，会发现里面有大量数据库数据，说明数据库完成了初始化
ls -l data

# 5.查看MySQL容器内数据
# 5.1.进入MySQL
docker exec -it mysql mysql -uroot -p123
# 5.2.查看编码表
show variables like "%char%";
# 5.3.结果，发现编码是utf8mb4没有问题
+--------------------------+--------------------------------+
| Variable_name            | Value                          |
+--------------------------+--------------------------------+
| character_set_client     | utf8mb4                        |
| character_set_connection | utf8mb4                        |
| character_set_database   | utf8mb4                        |
| character_set_filesystem | binary                         |
| character_set_results    | utf8mb4                        |
| character_set_server     | utf8mb4                        |
| character_set_system     | utf8mb3                        |
| character_sets_dir       | /usr/share/mysql-8.0/charsets/ |
+--------------------------+--------------------------------+

# 6.查看数据
# 6.1.查看数据库
show databases;
# 结果，hmall是黑马商城数据库
+--------------------+
| Database           |
+--------------------+
| hmall              |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.00 sec)
# 6.2.切换到hmall数据库
use hmall;
# 6.3.查看表
show tables;
# 结果：
+-----------------+
| Tables_in_hmall |
+-----------------+
| address         |
| cart            |
| item            |
| order           |
| order_detail    |
| order_logistics |
| pay_order       |
| user            |
+-----------------+
# 6.4.查看address表数据
+----+---------+----------+--------+----------+-------------+---------------+-----------+------------+-------+
| id | user_id | province | city   | town     | mobile      | street        | contact   | is_default | notes |
+----+---------+----------+--------+----------+-------------+---------------+-----------+------------+-------+
| 59 |       1 | 北京     | 北京   | 朝阳区    | 13900112222 | 金燕龙办公楼   | 李佳诚    | 0          | NULL  |
| 60 |       1 | 北京     | 北京   | 朝阳区    | 13700221122 | 修正大厦       | 李佳红    | 0          | NULL  |
| 61 |       1 | 上海     | 上海   | 浦东新区  | 13301212233 | 航头镇航头路   | 李佳星    | 1          | NULL  |
| 63 |       1 | 广东     | 佛山   | 永春      | 13301212233 | 永春武馆       | 李晓龙    | 0          | NULL  |
+----+---------+----------+--------+----------+-------------+---------------+-----------+------------+-------+
4 rows in set (0.00 sec)
```

‍

‍

‍

### 命令

‍

‍

|**命令**|**说明**|**文档地址**|
| -----------------------| ----------------------| --|
|docker volume create|创建数据卷|[docker volume create](https://docs.docker.com/engine/reference/commandline/volume_create/)|
|docker volume ls|查看所有数据卷|[docs.docker.com](https://docs.docker.com/engine/reference/commandline/volume_ls/)|
|docker volume rm|删除指定数据卷|[docs.docker.com](https://docs.docker.com/engine/reference/commandline/volume_prune/)|
|docker volume inspect|查看某个数据卷的详情|[docs.docker.com](https://docs.docker.com/engine/reference/commandline/volume_inspect/)|
|docker volume prune|清除数据卷|[docker volume prune](https://docs.docker.com/engine/reference/commandline/volume_prune/)|

‍

‍

## 本地目录

‍

### 挂载本地目录或文件

‍

可以发现，数据卷的目录结构较深，如果我们去操作数据卷目录会不太方便.

而容器不仅仅可以挂载数据卷，也可以直接挂载到宿主机目录上。关联关系如下：

* 带数据卷模式：宿主机目录 --> 数据卷 ---> 容器内目录
* 直接挂载模式：宿主机目录 ---> 容器内目录

‍

很多情况下，我们会直接将容器目录与宿主机指定目录挂载. 挂载语法与数据卷类似

```Bash
# 挂载本地目录
-v 本地目录:容器内目录
# 挂载本地文件
-v 本地文件:容器内文件
```

**==注意==**：本地目录或文件必须以 `/`​​ 或 `./`​​开头，如果直接以名字开头，会被识别为数据卷名而非本地目录名. 

‍

‍

例如：

```Bash
-v mysql:/var/lib/mysql # 会被识别为一个数据卷叫mysql，运行时会自动创建这个数据卷
-v ./mysql:/var/lib/mysql # 会被识别为当前目录下的mysql目录，运行时如果不存在会创建目录
```

‍

‍

### 多容器进行数据交换

* 多个容器挂载同一个数据卷
* 数据卷容器

* 创建启动c3数据卷容器，使用 –v 参数设置数据卷

  ```bash
  docker run –it --name=c3 –v /volume centos:7 /bin/bash 
  ```
* 创建启动 c1 c2 容器，使用 –-volumes-from 参数设置数据卷

  ```bash
  docker run –it --name=c1 --volumes-from c3 centos:7 /bin/bash
  docker run –it --name=c2 --volumes-from c3 centos:7 /bin/bash  
  ```

‍

‍

### 数据卷挂载与目录直接挂载的比较

* 数据卷挂载耦合度低，由docker来管理目录，但是目录较深，不好找
* 目录挂载耦合度高，需要我们自己管理目录，不过目录容易寻找查看

‍

## 镜像

镜像的来源

* 基于官方基础镜像自己制作
* 直接去DockerRegistry下载

‍

镜像的名称不是随意的，而是要到DockerRegistry中寻找，镜像运行时的配置也不是随意的，要参考镜像的帮助文档，这些在DockerHub网站或者软件的官方网站中都能找到. 

如果我们要安装其它软件，也可以到DockerRegistry中寻找对应的镜像名称和版本，阅读相关配置即可. 

部署一个Java项目并把它打包为一个镜像

‍

‍

#### 手动构建镜像

当Dockerfile文件写好以后，就可以利用命令来构建镜像了.

‍

首先将课前资料提供的`docker-demo.jar`​包以及`Dockerfile`​拷贝到虚拟机的`/root/demo`​目录

执行命令构建镜像

```Bash
# 进入镜像目录
cd /root/demo
# 开始构建
docker build -t docker-demo:1.0 .
```

‍

命令说明

* ​`docker build `​: 就是构建一个docker镜像
* ​`-t docker-demo:1.0`​ ：`-t`​参数是指定镜像的名称（`repository`​和`tag`​）
* ​`.`​ : 最后的点是指构建时Dockerfile所在路径，由于我们进入了demo目录，所以指定的是`.`​代表当前目录，也可以直接指定Dockerfile目录：

  ```Bash
  # 直接指定Dockerfile目录
  docker build -t docker-demo:1.0 /root/demo
  ```

‍

查看镜像列表

```Bash
# 查看镜像列表：
docker images
# 结果
REPOSITORY    TAG       IMAGE ID       CREATED          SIZE
docker-demo   1.0       d6ab0b9e64b9   27 minutes ago   327MB
nginx         latest    605c77e624dd   16 months ago    141MB
mysql         latest    3218b38490ce   17 months ago    516MB
```

‍

然后尝试运行该镜像

```Bash
# 1.创建并运行容器
docker run -d --name dd -p 8090:8090 docker-demo:1.0
# 2.查看容器
dps
# 结果
CONTAINER ID   IMAGE             PORTS                                                  STATUS         NAMES
78a000447b49   docker-demo:1.0   0.0.0.0:8080->8080/tcp, :::8090->8090/tcp              Up 2 seconds   dd
f63cfead8502   mysql             0.0.0.0:3306->3306/tcp, :::3306->3306/tcp, 33060/tcp   Up 2 hours     mysql

# 3.访问
curl localhost:8080/hello/count
# 结果：
<h5>欢迎访问黑马商城, 这是您第1次访问<h5>
```

‍

### 镜像结构

‍

> 镜像之所以能让我们快速跨操作系统部署应用而忽略其运行环境、配置，就是因为镜像中包含了程序运行需要的系统函数库、环境、配置、依赖.

自定义镜像本质就是依次准备好程序运行的基础环境、依赖、应用本身、运行配置等文件，并且打包而成

‍

> 举个例子，我们要从0部署一个Java应用，大概流程是这样：
>
> * 准备一个linux服务（CentOS或者Ubuntu均可）
> * 安装并配置JDK
> * 上传Jar包
> * 运行jar包
>
> 那因此，我们打包镜像也是分成这么几步：
>
> * 准备Linux运行环境（java项目并不需要完整的操作系统，仅仅是基础运行环境即可）
> * 安装并配置JDK
> * 拷贝jar包
> * 配置启动脚本

上述步骤中的每一次操作其实都是在生产一些文件（系统运行环境、函数库、配置最终都是磁盘文件），所以**镜像就是一堆文件的集合**. 

‍

但需要注意的是，镜像文件不是随意堆放的，而是按照操作的步骤分层叠加而成，每一层形成的文件都会单独打包并标记一个唯一id，称为**Layer**（**层**）. 这样，如果我们构建时用到的某些层其他人已经制作过，就可以直接拷贝使用这些层，而不用重复制作. 

‍

例如，第一步中需要的Linux运行环境，通用性就很强，所以Docker官方就制作了这样的只包含Linux运行环境的镜像. 我们在制作java镜像时，就无需重复制作，直接使用Docker官方提供的CentOS或Ubuntu镜像作为基础镜像. 然后再搭建其它层即可，这样逐层搭建，最终整个Java项目的镜像结构如图所示：

* 入口(Entrypoint)  
  镜像运行入口，一般是程序启动的脚本和参数
* 层( Layer )  
  在Baselmage基础上添加安装包、依赖、配置等，每次操作都形成新的一层.
* 基础镜像（Baselmage)  
  应用依赖的系统函数库、环境、配置、文件等

​​

‍

### Dockerfile初见

由于制作镜像的过程中，需要逐层处理和打包，比较复杂，所以Docker就提供了自动打包镜像的功能. 我们只需要将打包的过程，每一层要做的事情用固定的语法写下来，交给Docker去执行即可

而这种记录镜像结构的文件就称为**Dockerfile**，其对应的语法可以参考[官方文档](https://docs.docker.com/engine/reference/builder/)

‍

常用

|**指令**|**说明**|**示例**|
| --| ----------------------------------------------| -----------------------------|
|**FROM**|指定基础镜像|​`FROM centos:6`​|
|**ENV**|设置环境变量，可在后面指令使用|​`ENV key value`​|
|**COPY**|拷贝本地文件到镜像的指定目录|​`COPY ./xx.jar /tmp/app.jar`​|
|**RUN**|执行Linux的shell命令，一般是安装过程的命令|​`RUN yum install gcc`​|
|**EXPOSE**|指定容器运行时监听的端口，是给镜像使用者看的|EXPOSE 8080|
|**ENTRYPOINT**|镜像中应用的启动命令，容器运行时调用|ENTRYPOINT java -jar xx.jar|

‍

例如，要基于Ubuntu镜像来构建一个Java应用，其Dockerfile内容如下：

```Dockerfile
# 指定基础镜像
FROM ubuntu:16.04
# 配置环境变量，JDK的安装目录、容器内时区
ENV JAVA_DIR=/usr/local
ENV TZ=Asia/Shanghai
# 拷贝jdk和java项目的包
COPY ./jdk8.tar.gz $JAVA_DIR/
COPY ./docker-demo.jar /tmp/app.jar
# 设定时区
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
# 安装JDK
RUN cd $JAVA_DIR \
 && tar -xf ./jdk8.tar.gz \
 && mv ./jdk1.8.0_144 ./java8
# 配置环境变量
ENV JAVA_HOME=$JAVA_DIR/java8
ENV PATH=$PATH:$JAVA_HOME/bin
# 指定项目监听的端口
EXPOSE 8080
# 入口，java项目的启动命令
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

‍

提供了基础的系统加JDK环境，我们在此基础上制作java镜像，就可以省去JDK的配置了：

```Dockerfile
# 基础镜像
FROM openjdk:11.0-jre-buster
# 设定时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
# 拷贝jar包
COPY docker-demo.jar /app.jar
# 入口
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

‍

‍

‍

## 项目搭载

‍

### 项目部署实例

‍

黑马商城项目示例

* hmall：商城的后端代码
* hmall-portal：商城用户端的前端代码
* hmall-admin：商城管理端的前端代码

‍

部署的容器及端口说明：

|**项目**|**容器名**|**端口**|**备注**|
| --------------| -------| --------------------| ---------------------|
|hmall|hmall|8080|黑马商城后端API入口|
|hmall-portal|nginx|18080|黑马商城用户端入口|
|hmall-admin|18081|黑马商城管理端入口||
|mysql|mysql|3306|数据库|

‍

‍

#### 部署后端

​`hmall`​项目是一个maven聚合项目

> 我们要部署的就是其中的`hm-service`​，其中的配置文件采用了多环境的方式：
>
> 其中的`application-dev.yaml`​是部署到开发环境的配置，`application-local.yaml`​是本地运行时的配置.
>
> 查看application.yaml，你会发现其中的JDBC地址并未写死，而是读取变量：
>
> 这两个变量在`application-dev.yaml`​和`application-local.yaml`​中并不相同：
>
> 在dev开发环境（也就是Docker部署时）采用了mysql作为地址，刚好是我们的mysql容器名，只要两者在一个网络，就一定能互相访问.
>
> 我们将项目打包：跳过单元测试(闪电标志)直接package

‍

将`hm-service`​目录下的`Dockerfile`​和`hm-service/target`​目录下的`hm-service.jar`​一起上传到虚拟机的`root`​目录：

‍

部署项目：

```Bash
# 1.构建项目镜像，不指定tag，则默认为latest
docker build -t hmall .

# 2.查看镜像
docker images
# 结果
REPOSITORY    TAG       IMAGE ID       CREATED          SIZE
hmall         latest    0bb07b2c34b9   43 seconds ago   362MB
docker-demo   1.0       49743484da68   24 hours ago     327MB
nginx         latest    605c77e624dd   16 months ago    141MB
mysql         latest    3218b38490ce   17 months ago    516MB

# 3.创建并运行容器，并通过--network将其加入hmall网络，这样才能通过容器名访问mysql
docker run -d --name hmall --network hmall -p 8080:8080 hmall
```

测试，通过浏览器访问：http://你的虚拟机地址:8080/search/list

‍

‍

#### 部署前端

​`hmall-portal`​和`hmall-admin`​是前端代码，需要基于nginx部署

* ​`html`​是静态资源目录，我们需要把`hmall-portal`​以及`hmall-admin`​都复制进去
* ​`nginx.conf`​是nginx的配置文件，主要是完成对`html`​下的两个静态资源目录做代理

我们现在要做的就是把整个nginx目录上传到虚拟机的`/root`​目录下：

‍

然后创建nginx容器并完成两个挂载：

* 把`/root/nginx/nginx.conf`​挂载到`/etc/nginx/nginx.conf`​
* 把`/root/nginx/html`​挂载到`/usr/share/nginx/html`​

‍

由于需要让nginx同时代理hmall-portal和hmall-admin两套前端资源，因此我们需要暴露两个端口：

* 18080：对应hmall-portal
* 18081：对应hmall-admin

命令如下：

```Bash
docker run -d \
  --name nginx \
  -p 18080:18080 \
  -p 18081:18081 \
  -v /root/nginx/html:/usr/share/nginx/html \
  -v /root/nginx/nginx.conf:/etc/nginx/nginx.conf \
  --network hmall \
  nginx
```

测试，通过浏览器访问：http://你的虚拟机ip:18080

‍

‍

## 网络

‍

网络创建的东西

主要是网桥问题, Docker网络 Network东西

两个容器加入同一个网桥后就可以互相访问了(需要自定义网络)

‍

‍

容器之间能否互相访问

首先，我们查看下MySQL容器的详细信息，重点关注其中的网络IP地址：

```Bash
# 1.用基本命令，寻找Networks.bridge.IPAddress属性
docker inspect mysql
# 也可以使用format过滤结果
docker inspect --format='{{range .NetworkSettings.Networks}}{{println .IPAddress}}{{end}}' mysql
# 得到IP地址如下：
172.17.0.2

# 2.然后通过命令进入dd容器
docker exec -it dd bash

# 3.在容器内，通过ping命令测试网络
ping 172.17.0.2
# 结果
PING 172.17.0.2 (172.17.0.2) 56(84) bytes of data.
64 bytes from 172.17.0.2: icmp_seq=1 ttl=64 time=0.053 ms
64 bytes from 172.17.0.2: icmp_seq=2 ttl=64 time=0.059 ms
64 bytes from 172.17.0.2: icmp_seq=3 ttl=64 time=0.058 ms
```

发现可以互联，没有问题. 

但是，容器的网络IP其实是一个虚拟的IP，其值并不固定与某一个容器绑定，如果我们在开发时写死某个IP，而在部署时很可能MySQL容器的IP会发生变化，连接会失败. 

‍

‍

所以，我们必须借助于docker的网络功能来解决这个问题，[官方文档](https://docs.docker.com/engine/reference/commandline/network/)

‍

常见命令

|**命令**|**说明**|**文档地址**|
| ---------------------------| --------------------------| --|
|docker network create|创建一个网络|[docker network create](https://docs.docker.com/engine/reference/commandline/network_create/)|
|docker network ls|查看所有网络|[docs.docker.com](https://docs.docker.com/engine/reference/commandline/network_ls/)|
|docker network rm|删除指定网络|[docs.docker.com](https://docs.docker.com/engine/reference/commandline/network_rm/)|
|docker network prune|清除未使用的网络|[docs.docker.com](https://docs.docker.com/engine/reference/commandline/network_prune/)|
|docker network connect|使指定容器连接加入某网络|[docs.docker.com](https://docs.docker.com/engine/reference/commandline/network_connect/)|
|docker network disconnect|使指定容器连接离开某网络|[docker network disconnect](https://docs.docker.com/engine/reference/commandline/network_disconnect/)|
|docker network inspect|查看网络详细信息|[docker network inspect](https://docs.docker.com/engine/reference/commandline/network_inspect/)|

‍

教学演示：自定义网络

```Bash
# 1.首先通过命令创建一个网络
docker network create hmall

# 2.然后查看网络
docker network ls
# 结果：
NETWORK ID     NAME      DRIVER    SCOPE
639bc44d0a87   bridge    bridge    local
403f16ec62a2   hmall     bridge    local
0dc0f72a0fbb   host      host      local
cd8d3e8df47b   none      null      local
# 其中，除了hmall以外，其它都是默认的网络

# 3.让dd和mysql都加入该网络，注意，在加入网络时可以通过--alias给容器起别名
# 这样该网络内的其它容器可以用别名互相访问！
# 3.1.mysql容器，指定别名为db，另外每一个容器都有一个别名是容器名
docker network connect hmall mysql --alias db
# 3.2.db容器，也就是我们的java项目
docker network connect hmall dd

# 4.进入dd容器，尝试利用别名访问db
# 4.1.进入容器
docker exec -it dd bash
# 4.2.用db别名访问
ping db
# 结果
PING db (172.18.0.2) 56(84) bytes of data.
64 bytes from mysql.hmall (172.18.0.2): icmp_seq=1 ttl=64 time=0.070 ms
64 bytes from mysql.hmall (172.18.0.2): icmp_seq=2 ttl=64 time=0.056 ms
# 4.3.用容器名访问
ping mysql
# 结果：
PING mysql (172.18.0.2) 56(84) bytes of data.
64 bytes from mysql.hmall (172.18.0.2): icmp_seq=1 ttl=64 time=0.044 ms
64 bytes from mysql.hmall (172.18.0.2): icmp_seq=2 ttl=64 time=0.054 ms
```

‍

OK，现在无需记住IP地址也可以实现容器互联了. 

‍

**总结**

* 在自定义网络中，可以给容器起多个别名，默认的别名是容器名本身
* 在同一个自定义网络中的容器，可以通过别名互相访问

‍

‍

‍

‍

‍

# 高级

‍

‍

## 移花接木

‍

### 基于java8构建Java项目

虽然我们可以基于Ubuntu基础镜像，添加任意自己需要的安装包，构建镜像，但是却比较麻烦。所以大多数情况下，我们都可以在一些安装了部分软件的基础镜像上做改造。

例如，构建java项目的镜像，可以在已经准备了JDK的基础镜像基础上构建。

‍

‍

需求：基于java:8-alpine镜像，将一个Java项目构建为镜像

‍

实现思路如下：

* ① 新建一个空的目录，然后在目录中新建一个文件，命名为Dockerfile
* ② 拷贝课前资料提供的docker-demo.jar到这个目录中
* ③ 编写Dockerfile文件：

  * a ）基于java:8-alpine作为基础镜像
  * b ）将app.jar拷贝到镜像中
  * c ）暴露端口
  * d ）编写入口ENTRYPOINT  
    内容如下：

    ```dockerfile
    FROM java:8-alpine
    COPY ./app.jar /tmp/app.jar
    EXPOSE 8090
    ENTRYPOINT java -jar /tmp/app.jar
    ```
* ④ 使用docker build命令构建镜像
* ⑤ 使用docker run创建容器并运行

‍

1. Dockerfile的本质是一个文件，通过指令描述镜像的构建过程
2. Dockerfile的第一行必须是FROM，从一个基础镜像来构建
3. 基础镜像可以是基本操作系统，如Ubuntu。也可以是其他人制作好的镜像，例如：java:8-alpine

‍

‍

‍

## DockerCompose

‍

大家可以看到，我们部署一个简单的java项目包含3个容器

* MySQL
* Nginx
* Java项目

而稍微复杂的项目，其中还会有各种各样的其它**中间件**，需要部署的东西远不止3个. 如果还像之前那样手动的逐一部署，就太麻烦了. 

而Docker Compose就可以帮助我们实现**多个相互关联的Docker容器的快速部署**. 它允许用户通过一个单独的 docker-compose.yml 模板文件（YAML 格式）来定义一组相关联的应用容器. 

‍

‍

### 基本语法

docker-compose.yml文件的基本语法可以[参考官方文档](https://docs.docker.com/compose/compose-file/compose-file-v3/)

docker-compose文件中可以定义多个相互关联的应用容器，每一个应用容器被称为一个服务（service）. 由于service就是在定义某个应用的运行时参数，因此与`docker run`​参数非常相似. 

‍

举例 用docker run部署MySQL

```Bash
docker run -d \
  --name mysql \
  -p 3306:3306 \
  -e TZ=Asia/Shanghai \
  -e MYSQL_ROOT_PASSWORD=123 \
  -v ./mysql/data:/var/lib/mysql \
  -v ./mysql/conf:/etc/mysql/conf.d \
  -v ./mysql/init:/docker-entrypoint-initdb.d \
  --network hmall
  mysql
```

‍

如果用`docker-compose.yml`​文件来定义，就是这样：

```YAML
version: "3.8"

services:
  mysql:
    image: mysql
    container_name: mysql
    ports:
      - "3306:3306"
    environment:
      TZ: Asia/Shanghai
      MYSQL_ROOT_PASSWORD: 123
    volumes:
      - "./mysql/conf:/etc/mysql/conf.d"
      - "./mysql/data:/var/lib/mysql"
    networks:
      - new
networks:
  new:
    name: hmall
```

‍

**对比**

|**docker run 参数**|**docker compose 指令**|**说明**|
| -----------| ----------------| ------------|
|--name|container_name|容器名称|
|-p|ports|端口映射|
|-e|environment|环境变量|
|-v|volumes|数据卷配置|
|--network|networks|网络|

明白了其中的对应关系，相信编写`docker-compose`​文件应该难不倒大家. 

‍

‍

‍

### 基础命令

‍

编写好docker-compose.yml文件，就可以部署项目了

[常见的命令](https://docs.docker.com/compose/reference/)

‍

基本语法

```Bash
docker compose [OPTIONS] [COMMAND]
```

‍

其中，OPTIONS和COMMAND都是可选参数，比较常见的有：

|**类型**|**参数或指令**|**说明**|
| ------------| ----------------------------------------------------------------------------------| -----------------------------|
|Options|-f|指定compose文件的路径和名称|
|-p|指定project名称. project就是当前compose文件中设置的多个service的集合，是逻辑概念||
|Commands<br />|up|创建并启动所有service容器|
|down|停止并移除所有容器、网络||
|ps|列出所有启动的容器||
|logs|查看指定容器的日志||
|stop|停止容器||
|start|启动容器||
|restart|重启容器||
|top|查看运行的进程||
|exec|在指定的运行中容器中执行命令||

‍

‍

### 部署示例

‍

黑马商城部署文件

```YAML
version: "3.8"

services:
  mysql:
    image: mysql
    container_name: mysql
    ports:
      - "3306:3306"
    environment:
      TZ: Asia/Shanghai
      MYSQL_ROOT_PASSWORD: 123
    volumes:
      - "./mysql/conf:/etc/mysql/conf.d"
      - "./mysql/data:/var/lib/mysql"
      - "./mysql/init:/docker-entrypoint-initdb.d"
    networks:
      - hm-net
  hmall:
    build: 
      context: .
      dockerfile: Dockerfile
    container_name: hmall
    ports:
      - "8080:8080"
    networks:
      - hm-net
    depends_on:
      - mysql
  nginx:
    image: nginx
    container_name: nginx
    ports:
      - "18080:18080"
      - "18081:18081"
    volumes:
      - "./nginx/nginx.conf:/etc/nginx/nginx.conf"
      - "./nginx/html:/usr/share/nginx/html"
    depends_on:
      - hmall
    networks:
      - hm-net
networks:
  hm-net:
    name: hmall
```

演示结果

```Bash
# 1.进入root目录
cd /root

# 2.删除旧容器
docker rm -f $(docker ps -qa)

# 3.删除hmall镜像
docker rmi hmall

# 4.清空MySQL数据
rm -rf mysql/data

# 5.启动所有, -d 参数是后台启动
docker compose up -d
# 结果：
[+] Building 15.5s (8/8) FINISHED
 => [internal] load build definition from Dockerfile                                    0.0s
 => => transferring dockerfile: 358B                                                    0.0s
 => [internal] load .dockerignore                                                       0.0s
 => => transferring context: 2B                                                         0.0s
 => [internal] load metadata for docker.io/library/openjdk:11.0-jre-buster             15.4s
 => [1/3] FROM docker.io/library/openjdk:11.0-jre-buster@sha256:3546a17e6fb4ff4fa681c3  0.0s
 => [internal] load build context                                                       0.0s
 => => transferring context: 98B                                                        0.0s
 => CACHED [2/3] RUN ln -snf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo   0.0s
 => CACHED [3/3] COPY hm-service.jar /app.jar                                           0.0s
 => exporting to image                                                                  0.0s
 => => exporting layers                                                                 0.0s
 => => writing image sha256:32eebee16acde22550232f2eb80c69d2ce813ed099640e4cfed2193f71  0.0s
 => => naming to docker.io/library/root-hmall                                           0.0s
[+] Running 4/4
 ✔ Network hmall    Created                                                             0.2s
 ✔ Container mysql  Started                                                             0.5s
 ✔ Container hmall  Started                                                             0.9s
 ✔ Container nginx  Started                                                             1.5s

# 6.查看镜像
docker compose images
# 结果
CONTAINER           REPOSITORY          TAG                 IMAGE ID            SIZE
hmall               root-hmall          latest              32eebee16acd        362MB
mysql               mysql               latest              3218b38490ce        516MB
nginx               nginx               latest              605c77e624dd        141MB

# 7.查看容器
docker compose ps
# 结果
NAME                IMAGE               COMMAND                  SERVICE             CREATED             STATUS              PORTS
hmall               root-hmall          "java -jar /app.jar"     hmall               54 seconds ago      Up 52 seconds       0.0.0.0:8080->8080/tcp, :::8080->8080/tcp
mysql               mysql               "docker-entrypoint.s…"   mysql               54 seconds ago      Up 53 seconds       0.0.0.0:3306->3306/tcp, :::3306->3306/tcp, 33060/tcp
nginx               nginx               "/docker-entrypoint.…"   nginx               54 seconds ago      Up 52 seconds       80/tcp, 0.0.0.0:18080-18081->18080-18081/tcp, :::18080-18081->18080-18081/tcp
```

‍

‍

‍

## 镜像原理

‍

‍

### 底层原理

> Docker 镜像本质是什么？  
> Docker 中一个centos镜像为什么只有200MB，而一个centos操作系统的iso文件要几个个G？  
> Docker 中一个tomcat镜像为什么有500MB，而一个tomcat安装包只有70多MB？

操作系统的组成部分：进程调度子系统、进程通信子系统、内存管理子系统、设备管理子系统、文件管理子系统、网络通信子系统、作业控制子系统

Linux文件系统由bootfs和rootfs两部分组成：

* bootfs：包含bootloader（引导加载程序）和 kernel（内核）
* rootfs： root文件系统，包含的就是典型 Linux 系统中的/dev，/proc，/bin，/etc等标准目录和文件
* 不同的linux发行版，bootfs基本一样，而rootfs不同，如ubuntu，centos

‍

Docker镜像原理：

* Docker镜像是一个**分层文件系统**，是由特殊的文件系统叠加而成，最底端是 bootfs，并复用宿主机的bootfs ，第二层是 root文件系统rootfs称为base image，然后再往上可以叠加其他的镜像文件
* 统一文件系统（Union File System）技术能够将不同的层整合成一个文件系统，为这些层提供了一个统一的视角，这样就隐藏了多层的存在，在用户的角度看来，只存在一个文件系统
* 一个镜像可以放在另一个镜像的上面。位于下面的镜像称为父镜像，最底部的镜像成为基础镜像。
* 当从一个镜像启动容器时，Docker会在最顶层加载一个读写文件系统作为容器

问题：

* Docker 中一个Ubuntu镜像为什么只有200MB，而一个Ubuntu操作系统的iso文件要几个个G？  
  Ubuntu的iso镜像文件包含bootfs和rootfs，而docker的Ubuntu镜像复用操作系统的bootfs，只有rootfs和其他镜像层
* Docker 中一个tomcat镜像为什么有500MB，而一个tomcat安装包只有70多MB？  
  由于docker中镜像是分层的，tomcat虽然只有70多MB，但他需要依赖于父镜像和基础镜像，所有整个对外暴露的tomcat镜像大小500多MB

‍

### 镜像制作

‍

1. 容器转镜像
2. dockerfile

‍

### Dockerfile

#### 基本概述

Dockerfile是一个文本文件，包含一条条的指令，每一条指令构建一层，基于基础镜像最终构建出新的镜像

* 对于开发人员：可以为开发团队提供一个完全一致的开发环境
* 对于测试人员：可以直接拿开发时所构建的镜像或者通过Dockerfile文件构建一个新的镜像开始工作了
* 对于运维人员：在部署时，可以实现应用的无缝移植

|关键字|作用|备注|
| -------------| --------------------------| -----------------------------------------------------------------------------------------------------------------------------|
|FROM|指定父镜像|指定dockerfile基于那个image构建|
|MAINTAINER|作者信息|用来标明这个dockerfile谁写的|
|LABEL|标签|用来标明dockerfile的标签 可以使用Label代替Maintainer 最终都是在docker image基本信息中可以查看|
|RUN|执行命令|执行一段命令 默认是/bin/sh 格式: RUN command 或者 RUN ["command" , "param1","param2"]|
|CMD|容器启动命令|提供启动容器时候的默认命令 和ENTRYPOINT配合使用.格式 CMD command param1 param2 或者 CMD ["command" , "param1","param2"]|
|ENTRYPOINT|入口|一般在制作一些执行就关闭的容器中会使用|
|COPY|复制文件|build的时候复制文件到image中|
|ADD|添加文件|build的时候添加文件到image中 不仅仅局限于当前build上下文 可以来源于远程服务|
|ENV|环境变量|指定build时候的环境变量 可以在启动的容器的时候 通过-e覆盖 格式ENV name=value|
|ARG|构建参数|构建参数 只在构建的时候使用的参数 如果有ENV 那么ENV的相同名字的值始终覆盖arg的参数|
|VOLUME|定义外部可以挂载的数据卷|指定build的image那些目录可以启动的时候挂载到文件系统中 启动容器的时候使用 -v 绑定 格式 VOLUME ["目录"]|
|EXPOSE|暴露端口|定义容器运行的时候监听的端口 启动容器的使用-p来绑定暴露端口 格式: EXPOSE 8080 或者 EXPOSE 8080/udp|
|WORKDIR|工作目录|指定容器内部的工作目录 如果没有创建则自动创建 如果指定/ 使用的是绝对地址 如果不是/开头那么是在上一条workdir的路径的相对路径|
|USER|指定执行用户|指定build或者启动的时候 用户 在RUN CMD ENTRYPONT执行的时候的用户|
|HEALTHCHECK|健康检查|指定监测当前容器的健康监测的命令 基本上没用 因为很多时候 应用本身有健康监测机制|
|ONBUILD|触发器|当存在ONBUILD关键字的镜像作为基础镜像的时候 当执行FROM完成之后 会执行 ONBUILD的命令 但是不影响当前镜像 用处也不怎么大|
|STOPSIGNAL|发送信号量到宿主机|该STOPSIGNAL指令设置将发送到容器的系统调用信号以退出。|
|SHELL|指定执行脚本的shell|指定RUN CMD ENTRYPOINT 执行命令的时候 使用的shell|

‍

‍

#### Centos

自定义centos7镜像：

1. 默认登录路径为 /usr
2. 可以使用vim

实现步骤：

1. 定义父镜像：FROM centos:7
2. 定义作者信息：MAINTAINER seazean < [zhyzhyang@sina.com](mailto:zhyzhyang@sina.com)>
3. 执行安装vim命令： RUN yum install -y vim
4. 定义默认的工作目录：WORKDIR /usr
5. 定义容器启动执行的命令：CMD /bin/bash
6. 通过dockerfile构建镜像：docker bulid –f dockerfile文件路径 –t 镜像名称:版本

‍

#### Boot

定义dockerfile，发布springboot项目：

实现步骤：

1. 定义父镜像：FROM java:8
2. 定义作者信息：MAINTAINER seazean < [zhyzhyang@sina.com](mailto:zhyzhyang@sina.com)>
3. 将jar包添加到容器： ADD springboot.jar app.jar
4. 定义容器启动执行的命令：CMD java–jar app.jar
5. 通过dockerfile构建镜像：docker bulid –f dockerfile文件路径 –t 镜像名称:版本

‍

‍

## 服务编排

‍

### 基本介绍

微服务架构的应用系统中一般包含若干个微服务，每个微服务一般都会部署多个实例，如果每个微服务都要手动启停，维护的工作量会很大。

* 从Dockerfile build image 或者去dockerhub拉取image；
* 创建多个container，管理这些container（启动停止删除）

**服务编排**：按照一定的业务规则批量管理容器

Docker Compose是一个编排多容器分布式部署的工具，提供命令集管理容器化应用的完整开发周期，包括服务构建，启动和停止。使用步骤：

1. 利用 Dockerfile 定义运行环境镜像
2. 使用 docker-compose.yml 定义组成应用的各服务
3. 运行 docker-compose up 启动应用

‍

### 功能实现

使用docker compose编排nginx+springboot项目

1. 安装Docker Compose
2. 创建docker-compose目录

    ```bash
    mkdir ~/docker-compose
    cd ~/docker-compose
    ```
3. 编写 docker-compose.yml 文件

    ```bash
    version: '3'
    services:
      nginx:
       image: nginx
       ports:
        - 80:80
       links:
        - app
       volumes:
        - ./nginx/conf.d:/etc/nginx/conf.d
      app:
        image: app
        expose:
          - "8080"
    ```
4. 创建./nginx/conf.d目录

    ```bash
    mkdir -p ./nginx/conf.d
    ```
5. 在./nginx/conf.d目录下编写***.conf文件

    ```bash
    server {
        listen 80;
        access_log off;

        location / {
            proxy_pass http://app:8080;
        }
    }
    ```
6. 在~/docker-compose 目录下使用docker-compose启动容器

    ```bash
    docker-compose up
    ```
7. 测试访问

    ```bash
    http://192.168.0.137/hello
    ```

‍

## 私有仓库

Docker官方的Docker hub（[https://hub.docker.com](https://hub.docker.com/)）是一个用于管理公共镜像的仓库，我们可以从上面拉取镜像 到本地，也可以把我们自己的镜像推送上去。但是当服务器无法访问互联网，或者不希望将自己的镜像放到公网当中，那么我们就需要搭建自己的私有仓库来存储和管理自己的镜像

‍

* 私有仓库搭建

  ```bash
  # 1、拉取私有仓库镜像 
  docker pull registry
  # 2、启动私有仓库容器 
  docker run -id --name=registry -p 5000:5000 registry
  # 3、输入地址http://私有仓库服务器ip:5000/v2/_catalog，显示{"repositories":[]} 
  # 4、修改daemon.json   
  vim /etc/docker/daemon.json  
  # 在上述文件中添加一个key，保存退出。此步用于让 docker 信任私有仓库地址；注意将私有仓库服务器ip修改为自己私有仓库服务器真实ip 
  {"insecure-registries":["192.168.0.137:5000"]} 
  # 5、重启docker 服务 
  systemctl restart docker
  docker start registry
  ```
* 将镜像上传至私有仓库

  ```bash
  # 1、标记镜像为私有仓库的镜像   
  docker tag centos:7 私有仓库服务器IP:5000/centos:7

  # 2、上传标记的镜像   
  docker push 私有仓库服务器IP:5000/centos:7
  ```
* 从私有仓库拉取镜像

  ```bash
  #拉取镜像 
  docker pull 私有仓库服务器ip:5000/centos:7
  ```

‍

‍

## 微服务相关

‍

### Docker部署微服务集群

**需求**：将之前学习的cloud-demo微服务集群利用DockerCompose部署

‍

**实现思路**：

① 查看课前资料提供的cloud-demo文件夹，里面已经编写好了docker-compose文件

② 修改自己的cloud-demo项目，将数据库、nacos地址都命名为docker-compose中的服务名

③ 使用maven打包工具，将项目中的每个微服务都打包为app.jar

④ 将打包好的app.jar拷贝到cloud-demo中的每一个对应的子目录中

⑤ 将cloud-demo上传至虚拟机，利用 docker-compose up -d 来部署

‍

‍

#### compose文件

查看课前资料提供的cloud-demo文件夹，里面已经编写好了docker-compose文件，而且每个微服务都准备了一个独立的目录：

‍

```yaml
version: "3.2"

services:
  nacos:
    image: nacos/nacos-server
    environment:
      MODE: standalone
    ports:
      - "8848:8848"
  mysql:
    image: mysql:5.7.25
    environment:
      MYSQL_ROOT_PASSWORD: 123
    volumes:
      - "$PWD/mysql/data:/var/lib/mysql"
      - "$PWD/mysql/conf:/etc/mysql/conf.d/"
  userservice:
    build: ./user-service
  orderservice:
    build: ./order-service
  gateway:
    build: ./gateway
    ports:
      - "10010:10010"
```

可以看到，其中包含5个service服务：

* ​`nacos`​：作为注册中心和配置中心

  * ​`image: nacos/nacos-server`​： 基于nacos/nacos-server镜像构建
  * ​`environment`​：环境变量

    * ​`MODE: standalone`​：单点模式启动
  * ​`ports`​：端口映射，这里暴露了8848端口
* ​`mysql`​：数据库

  * ​`image: mysql:5.7.25`​：镜像版本是mysql:5.7.25
  * ​`environment`​：环境变量

    * ​`MYSQL_ROOT_PASSWORD: 123`​：设置数据库root账户的密码为123
  * ​`volumes`​：数据卷挂载，这里挂载了mysql的data、conf目录，其中有我提前准备好的数据
* ​`userservice`​、`orderservice`​、`gateway`​：都是基于Dockerfile临时构建的

查看mysql目录，可以看到其中已经准备好了cloud_order、cloud_user表：

‍

查看微服务目录，可以看到都包含Dockerfile文件：

‍

内容如下：

```dockerfile
FROM java:8-alpine
COPY ./app.jar /tmp/app.jar
ENTRYPOINT java -jar /tmp/app.jar
```

‍

‍

#### 修改微服务配置

因为微服务将来要部署为docker容器，而容器之间互联不是通过IP地址，而是通过容器名。这里我们将order-service、user-service、gateway服务的mysql、nacos地址都修改为基于容器名的访问。

如下所示：

```yaml
spring:
  datasource:
    url: jdbc:mysql://mysql:3306/cloud_order?useSSL=false
    username: root
    password: 123
    driver-class-name: com.mysql.jdbc.Driver
  application:
    name: orderservice
  cloud:
    nacos:
      server-addr: nacos:8848 # nacos服务地址
```

‍

#### 打包

接下来需要将我们的每个微服务都打包。因为之前查看到Dockerfile中的jar包名称都是app.jar，因此我们的每个微服务都需要用这个名称。

可以通过修改pom.xml中的打包名称来实现，每个微服务都需要修改：

```xml
<build>
  <!-- 服务打包的最终名称 -->
  <finalName>app</finalName>
  <plugins>
    <plugin>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-maven-plugin</artifactId>
    </plugin>
  </plugins>
</build>
```

‍

#### 拷贝jar包到部署目录

编译打包好的app.jar文件，需要放到Dockerfile的同级目录中

‍

#### 部署

最后，我们需要将文件整个cloud-demo文件夹上传到虚拟机中，理由DockerCompose部署

进入cloud-demo目录，然后运行下面的命令：

```bash
docker-compose up -d
```

‍

‍

‍

‍

‍

# 实操

‍

## Bugfix

‍

### 其他

‍

#### MySQL能不能部署问题

> 国外有类似文章，但是以此类推，绝对不要在虚拟机里安装MySQL了. 性能比Docker差，删了虚拟机数据就丢了，太多这样的人要滚蛋了. 实际情况可能只有特例才必须安装在物理机里面，大部分情况收益很高，首先跑满IO和CPU的数据库就极少，其次追求容器化的系统对运维就是更高要求，容灾备份双机和更严格的管控，数据丢失误删概率也很低.

我的评价是不用管那么多

‍
