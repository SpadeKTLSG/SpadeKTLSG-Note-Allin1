‍

‍

## 搭建

‍

```java
sudo apt-get install mongodb
```

‍

验证

```java
mongo -version
```

默认设置MongoDB是随Ubuntu启动自动启动的。 输入以下命令查看是否启动成功：

```java
pgrep mongo -l
```

‍

修改/etc/mongodb.conf文件

将auth=true前面的#号去掉，开启动用户权限认证

**远程工具连接需要将IP改为0.0.0.0**

‍

进入mongo，添加账号密码

```java
mongo

use admin  //用admin身份

db.createUser({user:"root",pwd:"2333",roles:["root"]})//创建账号

db.auth("root","2333")//就可以进入了
```

‍

#### 配置文件

```java
# Where to store the data.
dbpath=/var/lib/mongodb

#where to log
logpath=/var/log/mongodb/mongodb.log
```

‍

#### 管理

如用 pgrep 命令查到 mongod 的进程 id 为 4811，则使用 kill 4811 命令来停止 mongod 服务

再打开一个新的终端窗口，在终端中输入 mongo 命令，可以打开 MongoDB Shell。可以使用 host 命令行选项来指定 mongod 所监听的主机地址和端口

```java
mongo host 127.0.0.1：27017
```

‍

‍

#### 远程

利用 Robo 3T 链接正常

‍
