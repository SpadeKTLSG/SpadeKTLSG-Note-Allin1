--前端Web服务器

‍

一款轻量级的Web服务器/反向代理服务器及电子邮件代理服务器. 占有内存少，并发能力强

官网：[https://nginx.org/](https://nginx.org/)

‍

‍

### Header

包括PC端和服务器端(Linux)量部分内容, 主要是后端使用就归到后端来了

联动Lua内容

‍

# 知识

‍

## 评价

‍

开箱即用, 在借鉴时不需要自己组装

‍

### 特点

1. 跨平台：Nginx可以在大多数操作系统中运行，而且也有Windows的移植版本
2. 配置异常简单：非常容易上手. 配置风格跟程序开发一样，神一般的配置
3. 非阻塞、高并发：数据复制时，磁盘I/O的第一阶段是非阻塞的. 官方测试能够支撑5万并发连接，在实际生产环境中跑到2-3万并发连接数（这得益于Nginx使用了最新的epoll模型）
4. 事件驱动：通信机制采用epoll模式，支持更大的并发连接数
5. 内存消耗小：处理大并发的请求内存消耗非常小. 在3万并发连接下，开启的10个Nginx进程才消耗150M内存（15M*10=150M）
6. 成本低廉：Nginx作为开源软件，可以免费试用. 而购买F5 BIG-IP、NetScaler等硬件负载均衡交换机则需要十多万至几十万人民币
7. 内置健康检查功能：如果Nginx Proxy后端的某台Web服务器宕机了，不会影响前端访问.
8. 节省带宽：支持GZIP压缩，可以添加浏览器本地缓存的Header头.
9. 稳定性高：用于反向代理，宕机的概率微乎其微.

‍

‍

# 基础

‍

## 构建

‍

打包的前端工程**dist目录**下内容拷贝到nginx的**html目录(静态资源文件目录)** 下, 启动服务器即可

‍

‍

## 组成

‍

### 目录结构

‍

* conf/nginx.conf

  * nginx配置文件
* html

  * 存放静态文件(html、css、Js等)
* logs

  * 日志目录，存放日志文件
* sbin/nginx

  * 二进制文件，用于启动、停止Nginx服务

‍

‍

文件目录树状图

```java
.
├── conf                        <-- Nginx配置文件
│   ├── fastcgi.conf
│   ├── fastcgi.conf.default
│   ├── fastcgi_params
│   ├── fastcgi_params.default
│   ├── koi-utf
│   ├── koi-win
│   ├── mime.types
│   ├── mime.types.default
│   ├── nginx.conf              <-- 这个文件我们经常操作
│   ├── nginx.conf.default
│   ├── scgi_params
│   ├── scgi_params.default
│   ├── uwsgi_params
│   ├── uwsgi_params.default
│   └── win-utf
├── html                        <-- 存放静态文件，我们后期部署项目，就要将静态文件放在这
│   ├── 50x.html  
│   └── index.html              <-- 提供的默认的页面
├── logs                        <-- 日志目录，由于我们新装的Nginx，所以现在还没有日志文件
└── sbin              
└── nginx                       <-- 这个文件我们也经常操作
```

‍

‍

### 配置文件结构

‍

Nginx配置文件(conf/nginx.conf)整体分为三部分

* 全局块 和Nginx运行相关的全局配置
* events块 和网络连接相关的配置
* http块 代理、缓存、日志记录、虚拟主机配置

  * http全局块
  * Server块

    * Server全局块
    * location块

```java
worker_processes  1;                              <-- 全局块
  
events {                                          <-- events块
    worker_connections  1024;  
}  
  
http {                                            <-- http块
    include       mime.types;                     <-- http全局块
    default_type  application/octet-stream;  
    sendfile        on;  
    keepalive_timeout  65;  
  
    server {                                      <-- Server块
        listen       80;                          <-- Server全局块
        server_name  localhost;  
        location / {                              <-- location块
            root   html;
            index  index.html index.htm;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
```

注意：http块中可以配置多个Server块，每个Server块中可以配置多个location块

‍

‍

## 指令

‍

### 端口修改

​​conf\nginx.conf​

修改server {listen 80} 为对应端口

‍

‍

## 部署静态资源

‍

Nginx可以作为静态web服务器来部署静态资源. 静态资源指在服务端真实存在并且能够直接展示的一些文件，比如常见的html页面、css文件、js文件、图片、视频等资源.

相对于Tomcat，Nginx处理静态资源的能力更加高效，所以在生产环境下，一般都会将静态资源部署到Nginx中.

将静态资源部署到Nginx非常简单，只需要将文件复制到Nginx安装目录下的html目录中即可.

‍

‍

‍

# 高级

‍

## 正向代理

‍

* 正向代理是一个位于客户端和原始服务器（origin server)之间的服务器，为了从原始服务器取得内容，客户端向代理发送一个请求并指定目标（原始服务器），然后代理向原始服务器转交请求并将获得的内容返回给客户端.
* 正向代理的典型用途是为在防火墙内的局域网客户端提供访问Internet的途径.
* 正向代理一般是在客户端设置代理服务器，通过代理服务器转发请求，最终访问到目标服务器.

‍

‍

## 反向代理

**nginx 反向代理**，就是将前端发送的动态请求由 nginx 转发到后端服务器

* 反向代理服务器位于用户与目标服务器之间，但是对于用户而言，反向代理服务器就相当于目标服务器，即用户直接访问反向代理服务器就可以获得目标服务器的资源，反向代理服务器负责将请求转发给目标服务器.
* 用户不需要知道目标服务器的地址，也无须在用户端作任何设定.

‍

‍

**好处**

* 提高访问速度  
  因为nginx本身可以进行缓存，如果访问的同一接口，并且做了数据缓存，nginx就直接可把数据返回，不需要真正地访问服务端，从而提高访问速度.
* 进行负载均衡  
  所谓负载均衡,就是把大量的请求按照我们指定的方式均衡的分配给集群中的每台服务器.
* 保证后端服务安全  
  因为一般后台服务地址不会暴露，所以使用浏览器不能直接访问，可以把nginx作为请求访问的入口，请求到达nginx后转发到具体的服务中，从而保证后端服务的安全.

‍

‍

例子

* 正向代理：你让舍友去给你带三楼卖的煎饼（你最终会得到一个三楼的煎饼）
* 反向代理：你让舍友去给你买煎饼（你最终只会得到一个煎饼，但你不知道煎饼是哪儿卖的）
* 和正向代理不同，反向代理相当于是为目标服务器工作的，当你去访问某个网站时，你以为你访问问的是目标服务器，其实不然，当你访问时，其实是由一个代理服务器去接收你的请求，正向代理与反向代理最简单的区别： 正向代理隐藏的是用户（卖煎饼的不知道是你要买），反向代理隐藏的是服务器（你不知道煎饼是谁卖的）.
* 正向代理侧重的是用户，用户知道可以通过代理访问无法访问的资源，而反向代理侧重点在服务器这边，用户压根不知道自己访问的是资源时通过代理人去转发的.

‍

‍

### 配置

‍

#### 示例1

这里是在192.168.238.131上配置的，那么访问流程如下  
 客户端 --> 192.168.238.131:82 --> 192.168.238.132/50x.html  
 客户端访问反向代理服务器的82端口，而82端口又将请求转发给web服务器的50x.html资源  
 注意这里需要开启反向代理服务器的82端口

```java
server {
    listen       82;
    server_name  localhost;

    location / {
        proxy_pass http://http://192.168.238.132/50x.html;
    }
}
```

‍

#### 示例2

**nginx 反向代理的配置方式**

```nginx
server{
    listen 80;
    server_name localhost;
  
    location /api/{
        proxy_pass http://localhost:8080/admin/; #反向代理
    }
}
```

‍

**proxy_pass：** 该指令是用来设置代理服务器的地址，可以是主机名称，IP地址加端口号等形式.

如上代码的含义是：监听80端口号， 然后当我们访问 [http://localhost:80/api/../](http://localhost/)..这样的接口的时候，它会通过 location /api/ {} 这样的反向代理到 [http://localhost:8080/admin/上来. ](http://localhost:8080/admin/%E4%B8%8A%E6%9D%A5%E3%80%82)

接下来，进到nginx-1.20.2\conf，打开nginx配置

```nginx
# 反向代理,处理管理端发送的请求
location /api/ {
	proxy_pass   http://localhost:8080/admin/;
    #proxy_pass   http://webservers/admin/;
}
```

当在访问http://localhost/api/employee/login，nginx接收到请求后转到http://localhost:8080/admin/，故最终的请求地址为http://localhost:8080/admin/employee/login，和后台服务的访问地址一致.

‍

‍

## 负载均衡

‍

‍

早期的网站流量和业务功能都比较简单，单台服务器就可以满足基本需求，但是随着互联网的发展，业务流量越来越大并且业务逻辑也越来越复杂，单台服务器的性能及单点故障问题就凸显出来了，因此需要多台服务器组成应用集群，进行性能的水平扩展以及避免单点故障出现.

‍

应用集群：将同一应用部署到多台机器上，组成应用集群，接收负载均衡器分发的请求，进行业务处理并返回响应数据.

负载均衡器：将用户请求根据对应的负载均衡算法分发到应用集群中的一台服务器进行处理.

[  ](https://pic1.imgdb.cn/item/6350b81c16f2c2beb17db4c8.jpg)

当如果服务以集群的方式进行部署时，那nginx在转发请求到服务器时就需要做相应的负载均衡. 其实，负载均衡从本质上来说也是基于反向代理来实现的，最终都是转发请求.

‍

‍

‍

### 配置

‍

默认是轮询算法，第一次访问是`192.168.238.132`​，第二次访问是`101.XXX.XXX.160`​也可以改用权重方式，权重越大，几率越大，现在的访问三分之二是第一台服务器接收，三分之一是第二台服务器接收

server 192.168.238.132 weight=10  
server 101.XXX.XXX.160 weight=5

```java
upstream targetServer{
    server 192.168.238.132;
    server 101.XXX.XXX.160;
}
server {
    listen       82;
    server_name  localhost;

    location / {
        proxy_pass http://targetServer;
    }
}
```

‍

**nginx 负载均衡的配置方式：**

```nginx
upstream webservers{
    server 192.168.100.128:8080;
    server 192.168.100.129:8080;
}
server{
    listen 80;
    server_name localhost;
  
    location /api/{
        proxy_pass http://webservers/admin;#负载均衡
    }
}
```

‍

**upstream：** 如果代理服务器是一组服务器的话，我们可以使用upstream指令配置后端服务器组.

如上代码的含义是：监听80端口号， 然后当我们访问 [http://localhost:80/api/../](http://localhost/)..这样的接口的时候，它会通过 location /api/ {} 这样的反向代理到 [http://webservers/admin，根据webservers名称找到一组服务器，根据设置的负载均衡策略(默认是轮询)转发到具体的服务器. ](http://webservers/admin%EF%BC%8C%E6%A0%B9%E6%8D%AEwebservers%E5%90%8D%E7%A7%B0%E6%89%BE%E5%88%B0%E4%B8%80%E7%BB%84%E6%9C%8D%E5%8A%A1%E5%99%A8%EF%BC%8C%E6%A0%B9%E6%8D%AE%E8%AE%BE%E7%BD%AE%E7%9A%84%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1%E7%AD%96%E7%95%A5(%E9%BB%98%E8%AE%A4%E6%98%AF%E8%BD%AE%E8%AF%A2)%E8%BD%AC%E5%8F%91%E5%88%B0%E5%85%B7%E4%BD%93%E7%9A%84%E6%9C%8D%E5%8A%A1%E5%99%A8%E3%80%82)

**注：** upstream后面的名称可自定义，但要上下保持一致.

‍

‍

‍

‍

具体配置方式：

‍

**轮询：**

```nginx
upstream webservers{
    server 192.168.100.128:8080;
    server 192.168.100.129:8080;
}
```

**weight:**

```nginx
upstream webservers{
    server 192.168.100.128:8080 weight=90;
    server 192.168.100.129:8080 weight=10;
}
```

**ip_hash:**

```nginx
upstream webservers{
    ip_hash;
    server 192.168.100.128:8080;
    server 192.168.100.129:8080;
}
```

**least_conn:**

```nginx
upstream webservers{
    least_conn;
    server 192.168.100.128:8080;
    server 192.168.100.129:8080;
}
```

**url_hash:**

```nginx
upstream webservers{
    hash &request_uri;
    server 192.168.100.128:8080;
    server 192.168.100.129:8080;
}
```

**fair:**

```nginx
upstream webservers{
    server 192.168.100.128:8080;
    server 192.168.100.129:8080;
    fair;
}
```

‍

‍

‍

### 策略

‍

**nginx 负载均衡策略**

|**名称**|**说明**|
| ------------| --------------------------------------------------------|
|轮询|默认方式|
|weight|权重方式，默认为1，权重越高，被分配的客户端请求就越多|
|ip_hash|依据ip分配方式，这样每个访客可以固定访问一个后端服务|
|least_conn|依据最少连接方式，把请求优先分配给连接数少的后端服务|
|url_hash|依据url分配方式，这样相同的url会被分配到同一个后端服务|
|fair|依据响应时间方式，响应时间短的服务将会被优先分配|

‍

## 动静分离

‍

‍

## 多级缓存

多级缓存的实现离不开Nginx编程，而Nginx编程又离不开OpenResty

见OpenResty

‍

# 服务器

‍

## 管理

‍

### 配置

查看版本

‍

* 进入sbin目录，输入`./nginx -v`​
* ​`检查配置文件正确性`​* 进入sbin目录，输入`./nginx -t`​，如果有错误会报错，而且也会记日志
* ​`启动与停止`​* 进入sbin目录，输入`./nginx`​，启动完成后查看进程 ` ps -ef | grep nginx`​
* 输入`./nginx -s stop`​，停止服务后再次查看进程
* 重新加载配置文件

  当修改Nginx配置文件后，需要重新加载才能生效，可以使用下面命令重新加载配置文件：`./nginx -s reload`​

‍

### 随处访问

所有命令，都需要我们在sbin目录下才能运行，比较麻烦，所以我们可以将Nginx的二进制文件配置到环境变量中，这样无论我们在哪个目录下，都能使用上面的命令

* 使用`vim /etc/profile`​命令打开配置文件，并配置环境变量，保存并退出

  ​`+ PATH=/usr/local/nginx/sbin:$JAVA_HOME/bin:$PATH`​
* 后重新加载配置文件，使用`source /etc/profile`​命令，然后我们在任意位置输入`nginx`​即可启动服务，`nginx -s stop`​即可停止服务

‍

‍

‍

# PC实操

‍

## Bugfix

‍

### 启动方法

主要是Win里面

```PowerShell
# 启动nginx
start nginx.exe
# 停止
nginx.exe -s stop
# 重新加载配置
nginx.exe -s reload
# 重启
nginx.exe -s restart
```

nginx.exe 不要双击启动，而是打开cmd窗口，通过命令行启动。停止的时候也一样要是用命令停止。如果启动失败不要重复启动，而是查看logs目录中的error.log日志，查看是否是端口冲突。如果是端口冲突则自行修改端口解决。

‍

‍

‍

# 服务器实操

‍

## Bugfix
