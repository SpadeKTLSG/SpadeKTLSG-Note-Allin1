--开源的后端 API 设计和文档工具

‍

Swagger 是一个用于生成、描述和调用 RESTful 接口的 Web 服务; 将项目中所有（想要暴露的）接口展现在页面上，并且可以进行接口调用和测试. [官网](https://swagger.io/)

‍

‍

### Header

‍

#### 环境

> 全盘采用3.0依赖形式

‍

新版本和老版本的区别主要体现在以下 4 个方面：

1：**依赖**项的添加不同：新版本只需要添加一项，而老版本需要添加两项；

2：启动 Swagger 的注解不同：新版本使用的是@EnableOpenApi，而老版本是 @EnableSwagger2；

3：Docket（文档摘要信息）的文件类型配置不同：新版本配置的是 **OAS_3**，而老版本是 SWAGGER_2；

4：Swagger UI 访问地址不同：新版本访问地址是“**http://localhost:8080/swagger-ui/** ”，而老版本访问地址是“http://localhost:8080/swagger-ui.html”

Ps：在使用swagger的时候，通常需要**使用【@ApiParam】注解指定接口中参数的名字**，特别是接口是**post请求且参数使用了@RequestParam注解时**

‍

‍

# 知识

‍

## 评价

‍

‍

Swagger 是一个规范且完整的框架，用于生成、描述、调用和可视化 RESTful 风格的 Web 服务. 

Swagger 的目标是对 REST API 定义一个标准且和语言无关的接口，可以让人和计算机拥有无须访问源码、文档或网络流量监测就可以发现和理解服务的能力. 当通过 Swagger 进行正确定义，用户可以理解远程服务并使用最少实现逻辑与远程服务进行交互. 与为底层编程所实现的接口类似，Swagger 消除了调用服务时可能会有的猜测. 

支持 API 自动生成同步的在线文档：使用 Swagger 后可以直接通过代码生成文档，不再需要自己手动编写接口文档了，对程序员来说非常方便，可以节约写文档的时间去学习新技术.   
提供 Web 页面在线测试 API：光有文档还不够，Swagger 生成的文档还支持在线测试. 参数和格式都定好了，直接在界面上输入参数对应的值即可在线测试接口. 

‍

1. 使得前后端分离开发更加方便，有利于团队协作
2. 接口的文档在线自动生成，降低后端开发人员编写接口文档的负担
3. 功能测试  
    Spring已经将Swagger纳入自身的标准，建立了Spring-swagger项目，现在叫Springfox. 通过在项目中引入Springfox ，即可非常简单快捷的使用Swagger.

knife4j是为Java MVC框架集成Swagger生成Api文档的增强解决方案,前身是swagger-bootstrap-ui,取名kni4j是希望它能像一把匕首一样小巧,轻量,并且功能强悍!

目前，一般都使用knife4j框架.

‍

### 优势

**将项目中所有的接口展现在页面上**，这样后端程序员就不需要专门为前端使用者编写专门的接口文档；

当接口更新之后，只需要修改代码中的 Swagger 描述就可以实时生成新的接口文档了，从而**规避了接口文档老旧不能使用的问题**；

通过 Swagger 页面，我们可以**直接进行接口调用，降低了项目开发阶段的调试成本**

‍

‍

# 基础

‍

## 搭建

新版本中，将改成starter的方式，所以我们一来是这样引入的

```pom
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-boot-starter</artifactId>
    <version>3.0.0</version>
</dependency>
```

同时引用主类的注解已经修改了

```bash
# 原来注解
@EnableSwagger2

# 修改后的注解
@EnableOpenApi
```

‍

### 修改配置文件

```java
@Configuration
public class Swagger3Config {
    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.OAS_30)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.withMethodAnnotation(ApiOperation.class))
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("Swagger3接口文档")
                .description("文档描述")
                .contact(new Contact("fanl", "#", "862844083@qq.com"))
                .version("1.0")
                .build();
    }
}
```

然后在原来的SpringSecurity配置文件里面，允许对于网站静态资源的无授权访问

```java
.antMatchers(
    "/swagger-ui.html",
    "/swagger-ui/*",
    "/swagger-resources/**",
    "/v2/api-docs",
    "/v3/api-docs",
    "/webjars/**",
    "/actuator/**",
    "/druid/**"
).permitAll()
```

然后我们运行我们的项目程序，然后输入下面的网址访问swagger-ui页面

```bash
# 注意，新版的swagger页面和2.x版本是有区别的
http://localhost:8601/swagger-ui/index.html
```

‍

但是我们有些接口还需要授权，因此我们还要配置携带token进行访问，因此我们还需要修改一下配置信息

```java
@Configuration
class Swagger3Config {
    @Bean
    public Docket createRestApi() {

        return new Docket(DocumentationType.SWAGGER_2).
                useDefaultResponseMessages(false)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.withMethodAnnotation(ApiOperation.class))
                .paths(PathSelectors.regex("^(?!auth).*$"))
                .build()
                .securitySchemes(securitySchemes())
                .securityContexts(securityContexts());
    }

    private List<SecurityScheme> securitySchemes() {
        return Lists.newArrayList(
                new ApiKey("Authorization", "Authorization", "header"));
    }

    List<SecurityReference> defaultAuth() {
        AuthorizationScope authorizationScope = new AuthorizationScope("global", "accessEverything");
        AuthorizationScope[] authorizationScopes = new AuthorizationScope[1];
        authorizationScopes[0] = authorizationScope;
        return Lists.newArrayList(
                new SecurityReference("Authorization", authorizationScopes));
    }

    private List<SecurityContext> securityContexts() {
        return Lists.newArrayList(
                SecurityContext.builder()
                        .securityReferences(defaultAuth())
                        .forPaths(PathSelectors.regex("^(?!auth).*$"))
                        .build()
        );
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("蘑菇博客Admin接口文档")
                .description("简单优雅的restful风格")
                .version("1.0")
                .build();
    }
}
```

点击按钮，添加对应的token，然后我们在请求接口的时候，就会携带对应的请求头信息了

‍

## 常用注解

|注解|说明|
| --------------------| ----------------------------------------------------------|
|@Api|用在请求的类上，例如Controller，表示对类的说明|
|@ApiModel|用在类上，通常是个实体类，表示一个返回响应数据的信息|
|@ApiModelProperty|用在属性上，描述响应类的属性|
|@ApiOperation|用在请求的方法上，说明方法的用途、作用|
|@ApilmplicitParams|用在请求的方法上，表示一组参数说明|
|@ApilmplicitParam|用在@ApilmplicitParams注解中，指定一个请求参数的各个方面|

‍

‍

# 实操

‍

## Bugfix
