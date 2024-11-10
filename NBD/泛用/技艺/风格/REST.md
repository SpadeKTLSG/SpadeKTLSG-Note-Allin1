--REST软件架构风格

‍

# 概念

‍

REST<sup>(Representational State Transfer)</sup>，表述性状态转换，是一种软件架构风格. 

前后端进行交互的时候，基于当前主流的REST风格的API接口进行交互

REST是风格，是约定方式，约定不是规定，可以打破; 基于REST风格，定义URL，URL将会更加简洁规范优雅

根据REST风格对资源进行访问称为RESTful

‍

‍

## 特点

当我们想表示一个网络资源的时候，可以使用两种方式:

‍

* 传统风格资源描述形式

  * ​`http://localhost/user/getById?id=1`​ 查询id为1的用户信息
  * ​`http://localhost/user/saveUser`​ 保存用户信息
* REST风格描述形式

  通过URL定位要操作的资源，通过HTTP动词(请求方式)来描述具体的操作; 通过四种请求方式，来操作数据的增删改查

  > REST风格URL：
  >
  > http://localhost:8080/users/1  GET：查询id为1的用户  
  > http://localhost:8080/users    POST：新增用户
  >

‍

传统方式一般是一个请求url对应一种操作，这样做不仅麻烦，也不安全，因为会程序的人读取了你的请求url地址，就大概知道该url实现的是一个什么样的操作. 查看REST风格的描述，请求地址变的简单了,光看请求URL并不是很能猜出来该URL的具体功能

‍

**优点**

* 隐藏资源的访问行为，无法通过地址得知对资源是何种操作
* 书写简化

‍

‍

# 实现

‍

‍

## 约定

按照REST风格访问资源时使用==行为动作==区分对资源进行了何种操作

‍

简记

* **GET   查询**
* **POST  新增**
* **PUT  修改**
* **DELETE  删除**

‍

‍

* ​`http://localhost/users`​​ 查询全部用户信息 **GET（查询）**
* ​`http://localhost/users/1`​​ 查询指定用户信息 **GET（查询）**
* ​`http://localhost/users`​​ 添加用户信息 **POST（新增/保存）**
* ​`http://localhost/users`​ 修改用户信息 PUT（修改/更新）
* ​`http://localhost/users/1`​ 删除用户信息 DELETE（删除）

‍

‍

约定方式，约定不是规范，可以打破，所以称REST风格，而不是REST规范; REST提供了对应的架构方式，按照这种架构设计项目可以降低开发的复杂性，提高系统的可伸缩性

> 描述模块的名称通常使用复数，也就是加s的格式描述，表示此类资源，而非单个资源，例如:users、books、accounts......

‍

‍

## **统一响应结果**

‍

使用统一响应结果 Result 类 (R)

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Result {
    private Integer code;//响应码，1 代表成功; 0 代表失败
    private String msg;  //响应信息 描述字符串
    private Object data; //返回的数据

    //增删改 成功响应
    public static Result success(){
        return new Result(1,"success",null);
    }
    //查询 成功响应
    public static Result success(Object data){
        return new Result(1,"success",data);
    }
    //失败响应
    public static Result error(String msg){
        return new Result(0,msg,null);
    }
}
```

‍

‍

# 实操

‍

上传文件前后交互

‍

### 前

‍

**上传文件**的原始form表单，要求表单必须具备三要素

‍

* 表单必须有file域，用于选择要上传的文件

  > ```html
  > <input type="file" name="image"/>
  > ```
  >
* 表单提交方式必须为POST

  > 通常上传的文件会**比较大**，所以需要使用 POST 提交方式
  >
* 表单的编码类型enctype必须要设置为 **multipart/form-data**

  不设置就是仅仅把名字传了上去

  > 普通默认的编码格式是不适合传输大型的二进制数据的，所以在文件上传时，表单的编码格式必须设置为multipart/form-data
  >

‍

示例表单

upload.html

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>上传文件</title>
</head>

<body>

    <form action="/upload" method="post" enctype="multipart/form-data"> 
        姓名: <input type="text" name="username"><br>
        年龄: <input type="text" name="age"><br>
        头像: <input type="file" name="image"><br>
        <input type="submit" value="提交">
    </form>

</body>

</html>
```

‍

‍

### 后

‍

* controller处理`/upload`​
* 在定义的方法中接收提交过来的数据 （方法中的形参名和请求参数的名字保持一致）

  * 用户名：String name
  * 年龄： Integer age
  * 文件： MultipartFile image

  Spring中提供了一个API：MultipartFile，使用这个API就可以来接收到上传的文件

‍

‍

‍

表单项的名字和方法中形参名不一致，使用@RequestParam注解进行参数绑定

```java
public Result upload(String username,
                     Integer age, 
                     @RequestParam("image") MultipartFile file)
```

‍

**UploadController示例**

```java
@Slf4j
@RestController
public class UploadController {

    @PostMapping("/upload")
    public Result upload(String username, Integer age, MultipartFile image)  {
        log.info("文件上传：{},{},{}",username,age,image);
        return Result.success();
    }

}
```

‍

‍
