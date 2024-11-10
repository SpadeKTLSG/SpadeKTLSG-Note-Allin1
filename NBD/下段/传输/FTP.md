‍

‍

通过Java程序对FTP服务器文件处理：连接，上传，下载，读取，移动文件等

‍

### Header

‍

# 知识

‍

# 基础

‍

## 搭建

‍

### 示例

```java
package com.yootk.common.util.ftp;

import org.apache.commons.net.ftp.FTPClient;

public class FTPConnection { // 实现FTP连接的获取
    private static final String FTP_SERVER = "192.168.190.128"; // 服务地址
    private static final int FTP_PORT = 21;
    private static final String FTP_USER = "ftp";
    private static final String FTP_PASSWORD = "yootk@muyan";
    private static final int TIMEOUT = 5000; // 设置超时时间
    private static final String ENCODING = "UTF-8";
    public static FTPClient getFTPClient() throws Exception {
        FTPClient client = new FTPClient(); // 实例化FTPClient类实例
        client.connect(FTP_SERVER, FTP_PORT); // 设置连接主机信息
        client.login(FTP_USER, FTP_PASSWORD); // 实现服务登录
        client.setConnectTimeout(TIMEOUT); // 超时时间
        client.setControlEncoding(ENCODING); // 编码
        System.out.println("【FTP连接】状态码" + client.getReplyCode()); // 如果可以连接返回230
        return client;
    }
}
```

```java
package com.yootk.test.ftp;

import com.yootk.common.util.ftp.FTPConnection;
import org.apache.commons.net.ftp.FTPClient;

public class FTPConnectTest {
    public static void main(String[] args) throws Exception {
        FTPClient client = FTPConnection.getFTPClient(); // 获取FTP连接
        System.out.println(client.abort());
        client.logout(); // 注销
    }
}

```

‍

## 文件管理

‍

### 上传

‍

#### 示例

```java
public class FTPUploadFile {

    public static void main(String[] args) throws Exception {

        String ftpPath = "/var/ftp/yootk/"; // 此目录为Linux已经存在的路径，但是子路径不存在
        FTPClient client = FTPConnection.getFTPClient(); // 获取FTP连接
        client.enterLocalPassiveMode(); // 采用被动模式
        client.setFileType(FTPClient.BINARY_FILE_TYPE); // 采用二进制传输
        client.setBufferSize(2048); // 设置上传的缓存大小
        if (!client.changeWorkingDirectory(ftpPath)) {  // 切换目录
            client.makeDirectory(ftpPath); // 目录的创建
        }

        File file = new File("h:" + File.separator + "muyan_yootk.png"); // 定义上传文件
        // 所有的文件是以二进制数据流的形式进行传输的，所以一定要通过InputStream获取一个输入流
        InputStream input = new FileInputStream(file); // 文件输入流
        String tempName = ftpPath + file.getName(); // FTP文件保存名称
        String ftpFileName = new String(tempName.getBytes("UTF-8"), "ISO-8859-1");
        // 在进行文件保存的需要明确配置存储文件的名称，同时利用输入流进行数据的传输
        if (client.storeFile(ftpFileName, input)) {
            System.out.println("FTP文件上传成功！");
        } else {
            System.out.println("FTP文件上传失败！");
        }
        input.close(); // 传输完成后关闭输入流
        client.logout(); // 注销
    }
}

```

‍

### 下载

‍

#### 示例

```java
public class FTPDownloadFile {
    public static void main(String[] args) throws Exception {
        String ftpPath = "/var/ftp/yootk/"; // 此目录为Linux已经存在的路径，但是子路径不存在
        String fileName = "muyan_yootk.png"; // FTP文件名称
        FTPClient client = FTPConnection.getFTPClient(); // 获取FTP连接
        client.enterLocalPassiveMode(); // 采用被动模式
        client.setFileType(FTPClient.BINARY_FILE_TYPE); // 采用二进制传输
        client.setBufferSize(2048); // 设置上传的缓存大小
        if (client.changeWorkingDirectory(ftpPath)) { // 切换到要下载的FTP目录
            FTPFile[] files = client.listFiles(); // 获取全部的文件列表
            for (FTPFile file : files) {    // 迭代当前目录中的所有文件
                if (file.getName().trim().equalsIgnoreCase(fileName)) { // 找到了匹配的文件名称
                    // 为了可以区分出下载文件，本次进行一个强制性的命名
                    File downFile = new File("h:" + File.separator + "yootk_download.png");
                    // 要通过FTP下载文件，是需要利用输出流进行输出写入的
                    OutputStream output = new FileOutputStream(downFile); // 获取文件流
                    boolean flag = client.retrieveFile(new String(file.getName().getBytes("UTF-8"), "ISO-8859-1"), output);
                    output.flush();
                    output.close();
                    System.out.println(flag ? "文件下载成功！" : "文件下载失败！");
                }
            }
        }
        client.logout(); // 注销
    }
}
```

‍

### 删除

‍

#### 示例

```java
public class FTPDownloadFile {
    public static void main(String[] args) throws Exception {
        String ftpPath = "/var/ftp/yootk/"; // 此目录为Linux已经存在的路径，但是子路径不存在
        String fileName = "muyan_yootk.png"; // FTP文件名称
        FTPClient client = FTPConnection.getFTPClient(); // 获取FTP连接
        client.enterLocalPassiveMode(); // 采用被动模式
        client.setFileType(FTPClient.BINARY_FILE_TYPE); // 采用二进制传输
        client.setBufferSize(2048); // 设置上传的缓存大小
        if (client.changeWorkingDirectory(ftpPath)) { // 切换到要下载的FTP目录
            FTPFile[] files = client.listFiles(); // 获取全部的文件列表
            for (FTPFile file : files) {    // 迭代当前目录中的所有文件
                if (file.getName().trim().equalsIgnoreCase(fileName)) { // 找到了匹配的文件名称
                    // 为了可以区分出下载文件，本次进行一个强制性的命名
                    File downFile = new File("h:" + File.separator + "yootk_download.png");
                    // 要通过FTP下载文件，是需要利用输出流进行输出写入的
                    OutputStream output = new FileOutputStream(downFile); // 获取文件流
                    boolean flag = client.retrieveFile(new String(file.getName().getBytes("UTF-8"), "ISO-8859-1"), output);
                    output.flush();
                    output.close();
                    System.out.println(flag ? "文件下载成功！" : "文件下载失败！");
                    client.deleteFile(new String(file.getName().getBytes("UTF-8"), "ISO-8859-1"));
                }
            }
        }
        client.logout(); // 注销
    }
}


```

‍

### 移动

‍

#### 示例

```java
public class FTPMoveFile {
    public static void main(String[] args) throws Exception {
        String oldPath = "/var/ftp/yootk/"; // 文件存储的原始目录，该目录存在
        String newPath = "/var/ftp/muyan/"; // 文件存储的目标目录，该目录不存在
        String fileName = "muyan_yootk.png"; // 要移动的文件名称
        FTPClient client = FTPConnection.getFTPClient(); // 获取FTP连接

        client.enterLocalPassiveMode(); // 采用被动模式
        client.setFileType(FTPClient.BINARY_FILE_TYPE); // 采用二进制传输
        client.setBufferSize(2048); // 设置上传的缓存大小
        if (!client.changeWorkingDirectory(newPath)) {  // 目标的路径不存在
            client.makeDirectory(newPath); // 创建目标目录
        }
        // 如果要想移动文件，则一定要切换到移动的原始目录之中
        if (client.changeWorkingDirectory(oldPath)) {   // 移动的源目录存在
            FTPFile[] files = client.listFiles(); // 获取全部的目录文件
            for (FTPFile file : files) {    // 文件的列表
                if (file.getName().trim().equals(fileName)) {  // 可以查找到当前文件
                    client.rename(new String(file.getName().getBytes("UTF-8"), "ISO-8859-1"), newPath + new String(file.getName().getBytes("UTF-8"), "ISO-8859-1"));
                }
            }
        }

        client.logout(); // 注销
    }
}

```

‍

‍

## 配置

‍

### 上传大小

在SpringBoot中，文件上传时默认单个文件最大大小为1M

properties

```properties
# 配置单个文件最大上传大小
spring.servlet.multipart.max-file-size=10MB

# 配置单个请求最大上传大小(一次请求可以上传多个文件)
spring.servlet.multipart.multipart=100MB
```

multipart    max-file-size    multipart

‍

‍

‍

## 上传对象管理

‍

### UUID

保证每次上传文件时文件名都唯一的（使用UUID获取随机文件名）

‍

示例

```java
@Slf4j
@RestController
public class UploadController {

    @PostMapping("/upload")
    public Result upload(String username, Integer age, MultipartFile image) throws IOException {
        log.info("文件上传：{},{},{}",username,age,image);

        //获取原始文件名
        String originalFilename = image.getOriginalFilename();

        //构建新的文件名
        String extname = originalFilename.substring(originalFilename.lastIndexOf("."));//文件扩展名
        String newFileName = UUID.randomUUID().toString()+extname;//随机名+文件扩展名

        //将文件存储在服务器的磁盘目录
        image.transferTo(new File("E:/images/"+newFileName));

        return Result.success();
    }

}
```

‍

‍

# 高级

‍
