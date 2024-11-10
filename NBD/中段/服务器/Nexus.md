--强大的maven仓库管理器, Maven私服实现​

‍

‍

‍

‍

Maven私服其中一种使用量比较大的实现方式 [控制台](https://repo.sonatype.com/)

Sonatype公司的一款maven私服产品, [Link](https://help.sonatype.com/repomanager3/download)

> 联动Maven私服搭建内容, 解耦不完全, 这里主要介绍安装启动信息

‍

### Header

‍

私服概念

> 一种特殊的远程仓库，它是架设在局域网内的仓库服务，用来代理位于外部的中央仓库，用于解决团队内部的资源共享与资源同步问题.

‍

‍

‍

# 知识

介绍

‍

‍

# 基础

‍

## 搭建

‍

### 基础搭建示例

nexus安装包

使用cmd进入到解压目录下的`nexus-3.30.1-01\bin`​,执行如下命令:`nexus.exe /run nexus`​

‍

访问`http://localhost:8081`​

登录设置密码, 我的是2333

‍

至此私服就已经安装成功

如果要想修改一些基础配置信息，可以使用

1. 修改基础配置信息

    * 安装路径下etc目录中nexus-default.properties文件保存有nexus基础配置信息，例如默认访问端口.
2. 修改服务器运行配置信息

    * 安装路径下bin目录中nexus.vmoptions文件保存有nexus服务器启动对应的配置信息，例如默认占用内存空间.

‍

‍

# 高级

‍

‍

# 实操

‍

## Header
