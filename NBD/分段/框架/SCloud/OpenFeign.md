‍

‍

‍

声明式的 http 客户端，官方地址：[https://github.com/OpenFeign/feign](https://github.com/OpenFeign/feign)

‍

‍

## RestTemplate

‍

可以看到，利用RestTemplate发送http请求与前端ajax发送请求非常相似，都包含四部分信息：

* ① 请求方式
* ② 请求路径
* ③ 请求参数
* ④  返回值类型

‍

​`handleCartItems`​方法的完整代码如下：

```Java
private void handleCartItems(List<CartVO> vos) {
    // TODO 1.获取商品id
    Set<Long> itemIds = vos.stream().map(CartVO::getItemId).collect(Collectors.toSet());
    // 2.查询商品
    // List<ItemDTO> items = itemService.queryItemByIds(itemIds);
    // 2.1.利用RestTemplate发起http请求，得到http的响应
    ResponseEntity<List<ItemDTO>> response = restTemplate.exchange(
            "http://localhost:8081/items?ids={ids}",
            HttpMethod.GET,
            null,
            new ParameterizedTypeReference<List<ItemDTO>>() {
            },
            Map.of("ids", CollUtil.join(itemIds, ","))
    );
    // 2.2.解析响应
    if(!response.getStatusCode().is2xxSuccessful()){
        // 查询失败，直接结束
        return;
    }
    List<ItemDTO> items = response.getBody();
    if (CollUtils.isEmpty(items)) {
        return;
    }
    // 3.转为 id 到 item的map
    Map<Long, ItemDTO> itemMap = items.stream().collect(Collectors.toMap(ItemDTO::getId, Function.identity()));
    // 4.写入vo
    for (CartVO v : vos) {
        ItemDTO item = itemMap.get(v.getItemId());
        if (item == null) {
            continue;
        }
        v.setNewPrice(item.getPrice());
        v.setStatus(item.getStatus());
        v.setStock(item.getStock());
    }
}
```

RestTemplate 发起远程调用的代码很不好, 手动拼接字符串什么的

* 代码可读性差，编程体验不统一
* 参数复杂 URL 难以维护

‍

‍

‍

## 快速入门

‍

使用 Feign 的步骤：

① 引入依赖

② 添加@EnableFeignClients 注解

③ 编写 FeignClient 接口

④ 使用 FeignClient 中定义的方法代替 RestTemplate

‍

‍

‍

### 引入依赖

在`cart-service`​服务的pom.xml中引入`OpenFeign`​的依赖和`loadBalancer`​依赖：

```XML
  <!--openFeign-->
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-openfeign</artifactId>
  </dependency>
  <!--负载均衡器-->
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-loadbalancer</artifactId>
  </dependency>
```

‍

### 启用OpenFeign

order-service 的启动类添加注解开启 Feign 的功能

```java
@EnableFeignClients(clients = UserClient.class,defaultConfiguration = DefaultFeignConfiguration.class)
```

‍

‍

### 编写 Feign 客户端

‍

基于 SpringMVC 的注解来声明远程调用的信息，比如：

* 服务名称：userservice
* 请求方式：GET
* 请求路径：/user/{id}
* 请求参数：Long id
* 返回值类型：User

‍

feign的clients包里声明order-service的接口

```java
@FeignClient(value = "userservice")
public interface UserClient {

    @GetMapping("/user/{id}")
    User findById(@PathVariable("id") Long id);
}

```

‍

修改 order-service 中的 OrderService 类中的 queryOrderById 方法，使用 Feign 客户端代替 RestTemplate

```java
    @Autowired
    private OrderMapper orderMapper;

    @Autowired
    private UserClient userClient;

    public Order queryOrderById(Long orderId) {
        // 1.查询订单
        Order order = orderMapper.findById(orderId);
        // 2.用Feign远程调用
        User user = userClient.findById(order.getUserId());
        // 3.封装user到Order
        order.setUser(user);
        // 4.返回
        return order;
    }
```

有了上述信息，OpenFeign就可以利用动态代理帮我们实现这个方法，并且向`http://item-service/items`​发送一个`GET`​请求，携带ids为请求参数，并自动将返回值处理为`List<ItemDTO>`​。

我们只需要直接调用这个方法，即可实现远程调用了

‍

‍

‍

## 自定义配置

Feign 可以支持很多的自定义配置，如下表所示：

|类型|作用|说明|
| ---------------------| ------------------| -------------------------------------------------------------|
|**feign.Logger.Level**|修改日志级别|包含四种不同的级别：NONE、BASIC、HEADERS、FULL|
|feign.codec.Decoder|响应结果的解析器|http 远程调用的结果做解析，例如解析 json 字符串为 java 对象|
|feign.codec.Encoder|请求参数编码|将请求参数编码，便于通过 http 请求发送|
|feign. Contract|支持的注解格式|默认是 SpringMVC 的注解|
|feign. Retryer|失败重试机制|请求失败的重试机制，默认是没有，不过会使用 Ribbon 的重试|

一般情况下，默认值就能满足我们使用，如果要自定义时，只需要创建自定义的@Bean 覆盖默认 Bean 即可。

‍

‍

### 日志配置

OpenFeign只会在FeignClient所在包的日志级别为**DEBUG**时，才会输出日志。而且其日志级别有4级：

* **NONE**：不记录任何日志信息，这是默认值。
* **BASIC**：仅记录请求的方法，URL以及响应状态码和执行时间
* **HEADERS**：在BASIC的基础上，额外记录了请求和响应的头信息
* **FULL**：记录所有请求和响应的明细，包括头信息、请求体、元数据。

Feign默认的日志级别就是NONE，所以默认我们看不到请求日志。

‍

‍

‍

‍

#### 配置文件方式

基于配置文件修改 feign 的日志级别可以针对单个服务：

```yaml
feign:
  client:
    config:
      userservice: # 针对某个微服务的配置
        loggerLevel: FULL #  日志级别
```

‍

也可以针对所有服务：

```yaml
feign:
  client:
    config:
      default: # 这里用default就是全局配置，如果是写服务名称，则是针对某个微服务的配置
        loggerLevel: FULL #  日志级别
```

而日志的级别分为四种：

* NONE：不记录任何日志信息，这是默认值。
* BASIC：仅记录请求的方法，URL 以及响应状态码和执行时间
* HEADERS：在 BASIC 的基础上，额外记录了请求和响应的头信息
* FULL：记录所有请求和响应的明细，包括头信息、请求体、元数据。

‍

‍

#### Java 代码方式

也可以基于 Java 代码来修改日志级别，先声明一个类，然后声明一个 Logger.Level 的对象：

```java
public class DefaultFeignConfiguration  {
    @Bean
    public Logger.Level feignLogLevel(){
        return Logger.Level.BASIC; // 日志级别为BASIC
    }
}
```

‍

如果要**全局生效**，将其放到启动类的@EnableFeignClients 这个注解中：

```java
@EnableFeignClients(defaultConfiguration = DefaultFeignConfiguration .class)
```

‍

如果是**局部生效**，则把它放到对应的@FeignClient 这个注解中：

```java
@FeignClient(value = "userservice", configuration = DefaultFeignConfiguration .class)
```

‍

‍

## 连接池优化

‍

Feign底层发起http请求，依赖于其它的框架。其底层支持的http客户端实现包括：

* HttpURLConnection：默认实现，不支持连接池
* Apache HttpClient ：支持连接池
* OKHttp：支持连接池

因此我们通常会使用带有连接池的客户端来代替默认的HttpURLConnection。比如，我们使用OK Http. 或是 httpClient

‍

‍

### 1）引入依赖

在 order-service 的 pom 文件中引入 Apache 的 HttpClient 依赖：

```xml
<!--httpClient的依赖 -->
<dependency>
    <groupId>io.github.openfeign</groupId>
    <artifactId>feign-httpclient</artifactId>
</dependency>
```

```XML
<!--OK http 的依赖 -->
<dependency>
  <groupId>io.github.openfeign</groupId>
  <artifactId>feign-okhttp</artifactId>
</dependency>
```

‍

‍

### 2）配置连接池

在 order-service 的 application.yml 中添加配置：

‍

httpClient

```yaml
feign:
  client:
    config:
      default: # default全局的配置
        loggerLevel: BASIC # 日志级别，BASIC就是基本的请求和响应信息
  httpclient:
    enabled: true # 开启feign对HttpClient的支持
    max-connections: 200 # 最大的连接数
    max-connections-per-route: 50 # 每个路径的最大连接数
```

‍

OK http

```YAML
feign:
  okhttp:
    enabled: true # 开启OKHttp功能
```

重启服务，连接池就生效了。

‍

‍

### 优化

1.日志级别尽量用 basic

2.使用 HttpClient 或 OKHttp 代替 URLConnection

‍

‍

‍

## 最佳实践

所谓最近实践，就是使用过程中总结的经验，最好的一种使用方式。

自习观察可以发现，Feign 的客户端与服务提供者的 controller 代码非常相似

‍

简化这种重复的代码编写呢？

‍

* 思路1：抽取到微服务之外的公共module
* 思路2：每个微服务自己抽取一个module

方案1抽取更加简单，工程结构也比较清晰，但缺点是整个项目耦合度偏高。

方案2抽取相对麻烦，工程结构相对更复杂，但服务之间耦合度降低。

‍

‍

### 继承方式

一样的代码可以通过继承来共享：

1）定义一个 API 接口，利用定义方法，并基于 SpringMVC 注解做声明。

2）Feign 客户端和 Controller 都集成改接口

‍

优点：

* 简单
* 实现了代码共享

缺点：

* 服务提供方、服务消费方紧耦合
* 参数列表中的注解映射并不会继承，因此 Controller 中必须再次声明方法、参数列表、注解

‍

‍

### 抽取方式

将 Feign 的 Client 抽取为独立模块，并且把接口有关的 POJO、默认的 Feign 配置都放到这个模块中，提供给所有消费者使用。

例如，将 UserClient、User、Feign 的默认配置都抽取到一个 feign-api 包中，所有微服务引用该依赖包，即可直接使用。

‍

‍

### 实现基于抽取的最佳实践

‍

‍

#### 抽取

首先创建一个 module，命名为 feign-api：

‍

在 feign-api 中然后引入 feign 的 starter 依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

‍

然后，order-service 中编写的 UserClient、User、DefaultFeignConfiguration 都复制到 feign-api 项目中

‍

‍

#### 在 order-service 中使用 feign-api

首先，删除 order-service 中的 UserClient、User、DefaultFeignConfiguration 等类或接口。

‍

在 order-service 的 pom 文件中中引入 feign-api 的依赖：

```xml
<dependency>
    <groupId>cn.itcast.demo</groupId>
    <artifactId>feign-api</artifactId>
    <version>1.0</version>
</dependency>
```

修改 order-service 中的所有与上述三个组件有关的导包部分，改成导入 feign-api 中的包

‍

‍

#### 解决扫描包问题

‍

方式一：

指定 Feign 应该扫描的包：

```java
@EnableFeignClients(basePackages = "cn.itcast.feign.clients")
```

‍

方式二：

指定需要加载的 Client 接口：

```java
@EnableFeignClients(clients = {UserClient.class})
```

‍

‍

‍

## 负载均衡原理

在SpringCloud的早期版本中，负载均衡都是有Netflix公司开源的Ribbon组件来实现的，甚至Ribbon被直接集成到了Eureka-client和Nacos-Discovery中。

但是自SpringCloud2020版本开始，已经弃用Ribbon，改用Spring自己开源的Spring Cloud LoadBalancer了，我们使用的OpenFeign的也已经与其整合。

接下来我们就通过源码分析，来看看OpenFeign底层是如何实现负载均衡功能的。

‍

### 源码跟踪

要弄清楚OpenFeign的负载均衡原理，最佳的办法肯定是从FeignClient的请求流程入手。

‍

‍

‍

‍
