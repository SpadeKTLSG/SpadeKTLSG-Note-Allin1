--信息传输令牌

‍

‍

(JSON Web Token) JWT令牌定义了一种简洁的<sup>（jwt就是一个简单的字符串. 可以在请求参数或者是请求头当中直接传递）</sup>、自包含<sup>（可以根据自身的需求在jwt令牌中存储自定义的数据内容. 如：可以直接在jwt令牌中存储用户的相关信息）</sup>的格式，用于在通信双方以json数据格式安全的传输信息. 由于数字签名的存在，这些信息是可靠的. jwt就是将原始的json数据格式进行了安全的封装，这样就可以直接基于jwt在通信双方安全的进行信息传输了[官网](https://jwt.io/). 可以使用 HMAC 算法或者是 RSA 的公钥密钥对进行签名

‍

### Header

‍

# 知识

‍

## 评价

‍

* 优点

  * 生产的token可以包含基本信息，比如id、用户昵称、头像等信息，避免再次查库
  * 存储在客户端，不占用服务端的内存资源
* 缺点

  * token是经过base64编码，所以可以解码，因此token加密前的对象不应该包含敏感信息，如用户权限，密码等
  * 如果没有服务端存储，则不能做登录失效处理，除非服务端改秘钥

‍

# 基础

‍

## 组成

使用.分割

‍

### Header(头部）

记录令牌类型、签名算法等

> 例如：{"alg":"HS256","type":"JWT"}

‍

### Payload(负载)

携带一些自定义信息、默认信息

也可以加些规范里面的东西，如iss签发者，exp 过期时间，sub 面向的用户

> 例如：{"id":"1","username":"Tom"}

组成

1. 标准中注册的生命(建议但不强制使用)
2. 公共声明
3. 私有声明

‍

### Signature(签名）

将header、payload，并加入指定秘钥，通过指定签名算法计算而来. 防止Token被篡改、确保安全性

目的就是为了防jwt令牌被篡改  
一旦jwt令牌当中任何一个部分、任何一个字符被篡改了，整个令牌在校验的时候都会失败

‍

组成

1. Header (Base64编码后)
2. Payload (Base64编码后)
3. Secret(盐，必须保密)

‍

‍

## 原理

‍

原始的JSON格式数据 -> 字符串过程

先对JSON格式的数据进行一次**base64编码**

‍

‍

### **base64编码**

‍

是编码方式，而不是加密方式

一种基于64个可打印字符来表示二进制数据的编码方式. 任何数据经过base64编码之后，最终就会通过这64个字符来表示.既然能编码，那也就意味着也能解码

‍

‍

‍

## 功能流程

‍

‍

典型的应用场景就是登录认证

流程

1. 在浏览器发起请求来执行登录操作，此时会访问登录的接口，如果登录成功之后，我们需要生成一个jwt令牌，将生成的 jwt令牌返回给前端.
2. 前端拿到jwt令牌之后，会将jwt令牌存储起来. 在后续的每一次请求中都会将jwt令牌携带到服务端.
3. 服务端统一拦截请求之后，先来判断一下这次请求有没有把令牌带过来，如果没有带过来，直接拒绝访问，如果带过来了，还要校验一下令牌是否是有效. 如果有效，就直接放行进行请求的处理.

‍

Tips

1. 在登录成功之后，要生成令牌.
2. 每一次请求当中，要接收令牌并对令牌进行校验.
3. JWT校验时使用的签名秘钥，必须和生成JWT令牌时使用的秘钥是配套的.
4. 如果JWT令牌解析校验时报错，则说明 JWT令牌被篡改 或 失效了，令牌非法.

‍

‍

‍

### 生成令牌

‍

‍

调用工具包中提供的API来完成JWT令牌的生成和校验

工具类：Jwts

‍

```java
@Test
public void genJwt(){
    Map<String,Object> claims = new HashMap<>();
    claims.put("id",1);
    claims.put("username","Tom");
  
    String jwt = Jwts.builder()
        .setClaims(claims) //自定义内容(载荷)    
        .signWith(SignatureAlgorithm.HS256, "itheima") //签名算法  
        .setExpiration(new Date(System.currentTimeMillis() + 24*3600*1000)) //有效期   
        .compact();
  
    System.out.println(jwt);
}

```

‍

输出的结果就是生成的JWT令牌,，通过英文的点分割对三个部分进行分割，将生成的令牌复制一下，然后打开JWT的官网，将生成的令牌直接放在Encoded位置，此时就会自动的将令牌解析出来. 

‍

> 第一部分解析出来，看到JSON格式的原始数据，所使用的签名算法为HS256. 
>
> 第二个部分是我们自定义的数据，之前我们自定义的数据就是id，还有一个exp代表的是我们所设置的过期时间. 
>
> 由于前两个部分是base64编码，所以是可以直接解码出来. 但最后一个部分并不是base64编码，是经过签名算法计算出来的，所以最后一个部分是不会解析的.

‍

‍

### 校验令牌

解析生成的令牌

‍

```java
@Test
public void parseJwt(){
    Claims claims = Jwts.parser()
        .setSigningKey("itheima")//指定签名密钥（必须保证和生成令牌时使用相同的签名密钥）  
	    .parseClaimsJws("eyJhbGciOiJIUzI1NiJ9.eyJpZCI6MSwiZXhwIjoxNjcyNzI5NzMwfQ.fHi0Ub8npbyt71UqLXDdLyipptLgxBUg_mSuGJtXtBk")
        .getBody();

    System.out.println(claims);
}
```

‍

### 下发令牌示例

‍

1. 生成令牌

    * 在登录成功之后来生成一个JWT令牌，并且把这个令牌直接返回给前端
2. 校验令牌

    * 拦截前端请求，从请求中获取到令牌，对令牌进行解析校验

‍

‍

创建utils包, 两个方法

```java
//导入的Utils工具,完成认证
public class JwtUtils {

    private static String signKey = "itheima";
    private static Long expire = 43200000L;

    /**
     * 生成JWT令牌
     * @param claims JWT第二部分负载 payload 中存储的内容
     * @return
     */
    public static String generateJwt(Map<String, Object> claims){
        String jwt = Jwts.builder()
                .addClaims(claims)
                .signWith(SignatureAlgorithm.HS256, signKey)
                .setExpiration(new Date(System.currentTimeMillis() + expire))
                .compact();
        return jwt;
    }

    /**
     * 解析JWT令牌
     * @param jwt JWT令牌
     * @return JWT第二部分负载 payload 中存储的内容
     */
    public static Claims parseJWT(String jwt){
        Claims claims = Jwts.parser()
                .setSigningKey(signKey)
                .parseClaimsJws(jwt)
                .getBody();
        return claims;
    }
}

```

‍

登录

```java
//有令牌的登录
    @PostMapping("/login")
    public Result login(@RequestBody Emp emp) {
        //调用业务层：登录功能
        Emp loginEmp = empService.login(emp);

        //判断：登录用户是否存在
        if(loginEmp !=null ){
  
            //自定义信息,封装到Map中,生成令牌
            Map<String , Object> claims = new HashMap<>();
            claims.put("id", loginEmp.getId());
            claims.put("username",loginEmp.getUsername());
            claims.put("name",loginEmp.getName());

            //使用JWT工具类，生成身份令牌
            String token = JwtUtils.generateJwt(claims);
            return Result.success(token); //传递令牌
        }
        return Result.error("用户名或密码错误");
    }
```

‍

令牌存储在浏览器的本地存储空间local storage中了

‍

‍

‍

‍

## 登录校验

‍

### 单机

单机tomcat应用登录检验

* sesssion保存在浏览器和应用服务器会话之间
* 用户登录成功，服务端会保存一个session，当然客户端有一个sessionId
* 客户端会把sessionId保存在cookie中，每次请求都会携带这个sessionId

‍

‍

### 分布式

分布式应用中session共享

* 真实的应用不可能单节点部署，所以就有个多节点登录session共享的问题需要解决
* tomcat支持session共享，但是有广播风暴；用户量大的时候，占用资源就严重，不推荐
* 使用redis存储token:

  * 服务端使用UUID生成随机64位或者128位token，放入redis中，然后返回给客户端并存储在cookie中
  * 用户每次访问都携带此token，服务端去redis中校验是否有此用户即可

‍

‍

### 实操

‍

#### 封装通用方法

‍

**引入相关依赖并开发JWT工具类, 开发生产token和校验token的办法**

‍

加入相关依赖

```xml
    <!-- JWT相关 -->
    <dependency>
      <groupId>io.jsonwebtoken</groupId>
      <artifactId>jjwt</artifactId>
      <version>0.7.0</version>
    </dependency>

```

‍

封装生产token方法

```java
  /**
     * 根据用户信息，生成令牌
     * @param user
     * @return
     */
    public static String geneJsonWebToken(User user){

        String token = Jwts.builder().setSubject(SUBJECT)
                .claim("head_img",user.getHeadImg())
                .claim("id",user.getId())
                .claim("name",user.getName())
                .setIssuedAt(new Date())
                .setExpiration(new Date(System.currentTimeMillis() + EXPIRE))
                .signWith(SignatureAlgorithm.HS256,SECRET).compact();

        token = TOKEN_PREFIX + token;


        return token;
    }
```

‍

封装校验token方法

```java
   /**
     * 校验token的方法
     * @param token
     * @return
     */
    public static Claims checkJWT(String token){

        try{
            final  Claims claims = Jwts.parser().setSigningKey(SECRET)
                    .parseClaimsJws(token.replace(TOKEN_PREFIX,"")).getBody();

            return claims;

        }catch (Exception e){
            return null;
        }

    }
```

‍

‍

#### 整合

**开发登录模块功能，并整合JSON Web Token**

* 开发登录功能
* 修改domain 为model层

  * 增加entity、request包
  * 改application.properties配置文件扫描路径
* 整合JWT工具类

‍

‍

​

# 实操

‍

## Bugfix

‍

‍
