‍

‍

[官网](https://springdoc.org/)

‍

## 搭建

‍

```xml
   <dependency>
      <groupId>org.springdoc</groupId>
      <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
      <version>2.5.0</version>
   </dependency>
```

‍

‍

正确设置依赖后，我们就可以运行应用程序，并在 `/v3/api-docs`​ 路径访问 OpenAPI 描述，这是默认路径：

```sh
http://localhost:8080/v3/api-docs
```

此外，我们还可以使用 `springdoc.api-docs`​ 属性在 `application.properties`​ 中自定义描述的路径。例如，我们可以将路径设置为 `/api-docs`​：

```xml
springdoc:
  api-docs:
    path: /api-docs

```

‍

### 整合 Swagger UI

Springdoc-openapi 依赖项中已经包含了 Swagger UI，因此我们可以通过如下路径访问 API 文档

更改 `application.propertie`​s 文件中的 `springdoc.swagger-ui.path`​ 属性，自定义 API 文档的路径：

```xml
springdoc.swagger-ui.path=/swagger-ui-custom.html
```

‍

|Swagger3注解|注解位置|
| --------------------------------------------------------| ------------------|
|@Tag(name = “接口类描述”)|Controller类上|
|@Operation(summary = “接口方法描述”)|Controller方法上|
|@Parameters|Controller方法上|
|@Parameter(description = “参数描述”)|Controller方法上|
|@Parameter(description = “参数描述”)|方法参数上|
|@Parameter(hidden = true) 或 @Operation(hidden = true)|-|
|@Schema|DTO类上|
|@Schema|DTO属性上|

‍

‍

## Bugfix

[使用SpringDoc时，FastJson作为HttpMessageConverters的问题 - 掘金 (juejin.cn)](https://juejin.cn/post/7264780458738008105)

‍
