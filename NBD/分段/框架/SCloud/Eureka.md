‍

‍

‍

‍

## 结构和作用

‍

关键词

* 注册服务信息
* 拉取服务
* 负载均衡
* 远程调用
* ...

‍

回答之前的各个问题。

‍

问题1：order-service如何得知user-service实例地址？

获取地址信息的流程如下：

* user-service服务实例启动后，将自己的信息注册到eureka-server（Eureka服务端）。这个叫服务注册
* eureka-server保存服务名称到服务实例地址列表的映射关系
* order-service根据服务名称，拉取实例地址列表。这个叫服务发现或服务拉取

‍

问题2：order-service如何从多个user-service实例中选择具体的实例？

* order-service从实例列表中利用负载均衡算法选中一个实例地址
* 向该实例地址发起远程调用

‍

问题3：order-service如何得知某个user-service实例是否依然健康，是不是已经宕机？

* user-service会每隔一段时间（默认30秒）向eureka-server发起请求，报告自己状态，称为心跳
* 当超过一定时间没有发送心跳时，eureka-server会认为微服务实例故障，将该实例从服务列表中剔除
* order-service拉取服务时，就能将故障实例排除了

> 注意：一个微服务，既可以是服务提供者，又可以是服务消费者，因此eureka将服务注册、服务发现等功能统一封装到了eureka-client端

‍

‍

‍

## 搭建eureka-server

首先注册中心服务端：eureka-server，这必须是一个独立的微服务 (模块)

基础Maven模块即可

‍

starter依赖：

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
```

### 启动类

给eureka-server服务编写一个启动类，一定要添加一个@EnableEurekaServer注解，开启eureka的注册中心功能：

```java
@SpringBootApplication
@EnableEurekaServer
public class EurekaApplication {
    public static void main(String[] args) {
        SpringApplication.run(EurekaApplication.class, args);
    }
}
```

### 配置文件

编写一个application.yml文件，内容如下：

```yaml
server:
  port: 10086
spring:
  application:
    name: eureka-server
eureka:
  client:
    service-url: 
      defaultZone: http://127.0.0.1:10086/eureka
```

‍

‍

### 启动服务

启动微服务，然后在浏览器访问：[http://127.0.0.1:10086](http://127.0.0.1:10086/)

‍

下面可以看到对应的实例

|Application|AMIs|Availability Zones|Status|
| -------------| ------| --------------------| ---------|
|**EUREKA-SERVER**|**n/a** (1)|(1)|**UP** (1) - [SKPCPRO:eureka-server:10086](http://skpcpro:10086/actuator/info)|

‍

‍

## 服务注册

下面，我们将user-service注册到eureka-server中去。

‍

在user-service的pom文件中，引入下面的eureka-client依赖：

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

在user-service中，修改application.yml文件，添加服务名称、eureka地址：

```yaml
spring:
  application:
    name: userservice
eureka:
  client:
    service-url:
      defaultZone: http://127.0.0.1:10086/eureka
```

‍

演示一个服务有多个实例的场景，我们添加一个SpringBoot的启动配置，再启动一个user-service

* VM options: -Dserver.port=8082

现在，SpringBoot窗口会出现两个user-service启动配置, 启动两个user-service实例, 查看eureka-server管理页面

‍

‍

## 服务发现

下面，我们将order-service的逻辑修改：向eureka-server拉取user-service的信息，实现服务发现

‍

服务发现、服务注册统一都封装在eureka-client依赖，因此这一步与服务注册时一致。

在order-service的pom文件中，引入下面的eureka-client依赖：

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

‍

服务发现也需要知道eureka地址，因此第二步与服务注册一致，都是配置eureka信息：

在order-service中，修改application.yml文件，添加服务名称、eureka地址：

```yaml
spring:
  application:
    name: orderservice
eureka:
  client:
    service-url:
      defaultZone: http://127.0.0.1:10086/eureka
```

‍

‍

### 服务拉取和负载均衡

最后，我们要去eureka-server中拉取user-service服务的实例列表，并且实现负载均衡。

‍

在order-service的OrderApplication中，给RestTemplate这个Bean添加一个@LoadBalanced注解

```java
    @Bean
    @LoadBalanced // 开启负载均衡
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }

```

‍

修改order-service服务中的cn.itcast.order.service包下的OrderService类中的queryOrderById方法。修改访问的url路径，用服务名代替ip、端口

userservice -> local...

‍

spring会自动帮助我们从eureka-server端，根据userservice这个服务名称，获取实例列表，而后完成负载均衡

‍

‍

‍
