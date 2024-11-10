Object Storage Service

对象存储是一种使用HTTP API存储和检索非结构化数据和元数据对象的工具

此类存储库将无法维护传统数据库. 对象存储不允许按片段更改数据. 只能修改整个对象，这会影响性能. 例如，在文件系统中，您可以轻松地在日志末尾添加一行. 在对象存储系统中，为此需要还原对象，添加新行并将整个对象写回. 因此，这种存储不适用于数据经常变化的应用. 操作系统无法像常规磁盘一样安装对象存储

总而言之，OSS非常适合存储静态资源

‍

‍

### Header

‍

‍

# 知识

‍

## 概念

‍

### 存储空间（Bucket）

存储空间是用户用于存储对象（Object，就是文件）的容器，所有的对象都必须隶属于某个存储空间

‍

### 对象/文件（Object）

对象是 OSS 存储数据的基本单元，也被称为OSS的文件. 对象由元信息（Object Meta）、用户数据（Data）和文件名（Key）组成. 对象由存储空间内部唯一的Key来标识. 

‍

### 地域（Region）

地域表示 OSS 的数据中心所在物理位置. 您可以根据费用、请求来源等综合选择数据存储的地域. 

‍

### 访问域名（Endpoint）

Endpoint 表示OSS对外服务的访问域名. OSS以HTTP RESTful API的形式对外提供服务，当访问不同地域的时候，需要不同的域名. 通过内网和外网访问同一个地域所需要的域名也是不同的. 具体的内容请参见各个Region对应的Endpoint. 

‍

### 访问密钥（AccessKey）

AccessKey，简称 AK，指的是访问身份验证中用到的AccessKeyId 和AccessKeySecret. OSS通过使用AccessKeyId 和AccessKeySecret对称加密的方法来验证某个请求的发送者身份. AccessKeyId用于标识用户，AccessKeySecret是用户用于加密签名字符串和OSS用来验证签名字符串的密钥，其中AccessKeySecret 必须保密. 

‍

‍

### 读取方式

OSS中的每一个文件都会分配一个访问的url，通过这个url就可以访问到存储在阿里云上的图片

‍

## 对比

SAN(块存储) 和 NAS(文件存储)都是面向数据中心内访问的设备，而OSS（对象存储）产生的目的根本就不是在数据中心内使用，而是面向互联网、移动互联网(3G、4G、5G)而产生的，为大量使用的网页、视频、图片、音频、文档访问而设计. 

‍

‍

# 基础

‍

‍

## 搭建

以阿里云OSS为例[官网页面](https://www.aliyun.com/product/oss?spm=5176.8465980.unusable.ddetail.4f571450BXr6WR) 看最新文档即可

> 注册开通OSS -> 创建Bucket -> Get AccessKey -> 项目集成oss

‍

### 环境配置

‍

‍

核心替换内容

* accessKeyId：阿里云账号AccessKey
* accessKeySecret：阿里云账号AccessKey对应的秘钥
* bucketName：Bucket名称
* objectName：对象名称，在Bucket中存储的对象的名称
* filePath：文件路径

‍

‍

## 示例

‍

### 阿里云OSS独立配置

> 苍穹外卖配置示例, 分模块

‍

#### **定义OSS表单application-dev**

> 我是管它叫表单了, 就是一些必要的配置信息

‍

application-dev.yml

```yaml
sky:
  alioss:
    endpoint: XXX
    access-key-id: XXX
    access-key-secret: XXX
    bucket-name: XXX
```

‍

application.yml

```yaml
spring:
  profiles:
    active: dev    #设置环境
sky:
  alioss:
    endpoint: ${sky.alioss.endpoint}
    access-key-id: ${sky.alioss.access-key-id}
    access-key-secret: ${sky.alioss.access-key-secret}
    bucket-name: ${sky.alioss.bucket-name}
```

‍

‍

#### 封装**配置对象AliOssProperties**

common模块 Properties

‍

properties类将配置封装为Java对象

(自动转换驼峰)

‍

yml是横线习惯, 这里Java是驼峰习惯

```java

@Component
@ConfigurationProperties(prefix = "sky.alioss")
@Data
public class AliOssProperties {

    // 阿里云API的内或外网域名
    private String endpoint;

    // 阿里云API的密钥Access Key ID
    private String accessKeyId;

    // 阿里云API的密钥Access Key Secret
    private String accessKeySecret;

    // 阿里云API的bucket名称
    private String bucketName;

}
```

注意这里实际上是吧这些外部的配置类抽取放到了yml-dev里面, 后面如果是发布的, 直接改上面的

```java
spring:

  profiles:
    active: dev  #设置环境
```

即可切换为对应生产环境的配置文件了

‍

‍

#### **OSS配置类OssConfiguration**

server模块 config包

‍

配置文件需要配置类来加载

```java
/**
 * 配置类，用于创建AliOssUtil对象
 */
@Configuration
@Slf4j
public class OssConfiguration {

    @Bean
    @ConditionalOnMissingBean
    public AliOssUtil aliOssUtil(AliOssProperties aliOssProperties){ //注入属性
        log.info("开始创建阿里云文件上传工具类对象：{}",aliOssProperties);
        return new AliOssUtil(aliOssProperties.getEndpoint(),
                aliOssProperties.getAccessKeyId(),
                aliOssProperties.getAccessKeySecret(),
                aliOssProperties.getBucketName());
    }
}
```

‍

#### OSS工具类AliOssUtil

common模块 utils包

‍

upload方法

```java

@Data
@AllArgsConstructor
@Slf4j
public class AliOssUtil {

    private String endpoint;
    private String accessKeyId;
    private String accessKeySecret;
    private String bucketName;

    /**
     * 文件上传
     *
     * @param bytes      文件字节数组
     * @param objectName 文件名
     * @return 文件访问路径
     */
    public String upload(byte[] bytes, String objectName) {

        // 创建OSSClient实例。
        OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

        try {
            // 创建PutObject请求。
            ossClient.putObject(bucketName, objectName, new ByteArrayInputStream(bytes));

        } catch (OSSException oe) {

            System.out.println("Caught an OSSException, which means your request made it to OSS, "
                    + "but was rejected with an error response for some reason.");
            System.out.println("Error Message:" + oe.getErrorMessage());
            System.out.println("Error Code:" + oe.getErrorCode());
            System.out.println("Request ID:" + oe.getRequestId());
            System.out.println("Host ID:" + oe.getHostId());

        } catch (ClientException ce) {

            System.out.println("Caught an ClientException, which means the client encountered "
                    + "a serious internal problem while trying to communicate with OSS, "
                    + "such as not being able to access the network.");
            System.out.println("Error Message:" + ce.getMessage());

        } finally {

            if (ossClient != null) {
                ossClient.shutdown();
            }
        }

        //文件访问路径规则 https://BucketName.Endpoint/ObjectName
        StringBuilder stringBuilder = new StringBuilder("https://");
        //拼接图片网址, 便于访问
        stringBuilder
                .append(bucketName)
                .append(".")
                .append(endpoint)
                .append("/")
                .append(objectName);

        log.info("文件上传到:{}", stringBuilder.toString());

        return stringBuilder.toString();
    }
}

```

‍

‍

#### **文件上传接口CommonController**

‍

‍

定义通用接口

```java
/**
 * 通用接口
 */
@RestController
@RequestMapping("/admin/common")
@Api(tags = "通用接口")
@Slf4j
public class CommonController {

    @Autowired
    private AliOssUtil aliOssUtil;

    /**
     * 文件上传
     * @param file
     * @return
     */
    @PostMapping("/upload")
    @ApiOperation("文件上传")
    public Result<String> upload(MultipartFile file){
        log.info("文件上传：{}",file);

        try {
            //原始文件名
            String originalFilename = file.getOriginalFilename();
            //截取原始文件名的后缀   dfdfdf.png
            String extension = originalFilename.substring(originalFilename.lastIndexOf("."));
            //构造新文件名称
            String objectName = UUID.randomUUID().toString() + extension;

            //文件的请求路径
            String filePath = aliOssUtil.upload(file.getBytes(), objectName);
            return Result.success(filePath);
        } catch (IOException e) {
            log.error("文件上传失败：{}", e);
        }

        return Result.error(MessageConstant.UPLOAD_FAILED);
    }
}
```

‍

‍

## 管理

‍

### 服务器存储转本地存储

‍

在服务器本地磁盘上创建目录，用来存储上传的文件

使用MultipartFile类提供的API方法，把临时文件转存到本地磁盘目录下

```java
@Slf4j
@RestController
public class UploadController {

    @PostMapping("/upload")
    public Result upload(String username, Integer age, MultipartFile image) throws IOException {
        log.info("文件上传：{},{},{}",username,age,image);

        //获取原始文件名
        String originalFilename = image.getOriginalFilename();

        //将文件存储在服务器的磁盘目录
        image.transferTo(new File("D:/NewCwximages/"+originalFilename)); //文件名不能写死

        return Result.success();
    }

}
```
