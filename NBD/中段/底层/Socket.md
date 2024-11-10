--网络通信底层

‍

套接字（socket）是一个抽象层，应用程序可以通过它发送或接收数据，可对其进行像对文件一样的打开、读写和关闭等操作. 套接字允许应用程序将I/O插入到网络中，并与网络中的其他应用程序进行通信. 网络套接字是IP地址与端口的组合. 

‍

### Header

> 这里主要是用Java实现Socket编程

‍

# 知识

‍

## 原理

‍

将TCP协议简化一下，就只有三个核心功能：**建立连接、发送数据以及接收数据**

Java中提供的Socket类中的核心功能：建立数据, 发送数据, 接收数据

和TCP协议一样，所以可以把Socket编程理解为对TCP协议的具体实现.

‍

‍

# 基础

‍

## ​​InetAddress类

InetAddress类一个实例就代表一个具体的ip地址

‍

### 实例化

‍

获取指定ip对应的InetAddress的实例

```java
getByName(String host)
```

‍

获取本地ip对应的InetAddress的实例

```java
InetAddress getLocalHost()
```

‍

  

‍

### 常用方法

  

```java
getHostName()
```

‍

```java
getHostAddress()
```

‍

# 高级

‍

## ​UDP编程

了解即可

```java
 
    @Test//发送端
    public void sender() throws IOException {
        DatagramSocket ds =new DatagramSocket();//创建DatagramSocket实例
  
        //将数据和IP地址、端口号封装到DatagramPacket实例中
        InetAddress inetAddress=InetAddress.getLocalHost();
  
        int port=9090;
        byte[] bytes = "发送端".getBytes(Charset.defaultCharset());
        DatagramPacket packet =new DatagramPacket(bytes,0,bytes.length,inetAddress,port);

        ds.send(packet);
        ds.close();
    }
  
  
    @Test//接收端
    public void receiver() throws IOException {
        int port=9090;
        DatagramSocket ds =new DatagramSocket(port);
  
  
        byte[] bytes =new byte[1024*64]; //创建DatagramPacket实例，用于接收数据
        DatagramPacket packet =new DatagramPacket(bytes,0,bytes.length);

        ds.receive(packet);//接收数据

        System.out.println(new String(packet.getData(),0,packet.getLength()));
        ds.close();
    }
```

‍

‍

# 实操

‍

‍

## ​Socket连接

> (方法书:HeadFirstJava)

以 Socket 为工具完成初级网络编程

Socket 是个两台机器网络连接的对象(java.net.Socket)

‍

流程

1. 建立对服务器的 Socket 连接

    ```java
    Socker chatSocket = new Socket("127.0.0.1",5000);  //前面一个参数为JP地址, 后面为端口(对应地址的处理窗口)
    ```

    127.0.0.1 代表本机(指明可以自己的服务端测试客户端),0~1023 是保留给已知的服务的
2. 从服务器读取  
    建立输入串流 InputStreamReader, 从 Socket 获取; 同时建立读取串流BufferReader,从输入串流获取;最后用字符串获取 BufferReader.
3. 向 Socket 服务器输出  
    建立输出串流 PrintWriter, 向 Socket 输出

‍

‍

‍

### 完整通信示例

```java
  
    @Test
    public void client() {//客户端
        Socket socket = null;//创建socket对象
        OutputStream os = null;//获取字节输出流
        InputStream is = null;//获取字节输入流

        try {//创建一个Socket
            InetAddress inet = InetAddress.getByName("10.81.33.226");//获取ip地址
            int port = 9090;//声明端口号
            socket = new Socket(inet, port);

        
            os = socket.getOutputStream();//发送数据
            os.write("你好，我是客户端mm".getBytes());//写入数据,getBytes将字符串转换为字节数组

            socket.shutdownOutput(); 
            //表示客户端已经发送完数据，后面只能接受数据,如果不写这个方法，会造成双方阻塞
  
            is = socket.getInputStream();//接受返回信息
            byte[] buffer = new byte[1024];//创建字节数组
            ByteOutputStream bos = new ByteOutputStream(); //利用字节数出流读取数据转换为字符串
            int len;//记录每次读取的字节个数
            while ((len = is.read(buffer)) != -1) {//循环读取数据
                bos.write(buffer, 0, len);//将字节数组转换为字符串
            }
            System.out.println(bos);

        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {//关闭资源, 下面全部用try-catch加上!=null组合包饶
            is.close();
            socket.close();
            os.close();
        }
    }


  
    @Test
    public void server() {//服务端
        ServerSocket serverSocket = null;
        Socket socket = null;//阻塞式的方法，等待客户端的连接
        InputStream is = null;//获取字节输入流
        OutputStream os = null; //获取字节输出流

        try { //创建一个ServerSocket
       

            int port = 9090;//声明端口号
            serverSocket = new ServerSocket(port);
            socket = serverSocket.accept();//调用accept,接受客户端的Socket
            System.out.println("一个客户端建立了连接");

       
             //接受数据
            is = socket.getInputStream();
            byte[] buffer = new byte[1024];//创建字节数组
            int len;//记录每次读取的字节个数
            while ((len = is.read(buffer)) != -1) {//循环读取数据
                String str = new String(buffer, 0, len);//将字节数组转换为字符串
                System.out.println(str);
            }

            //返回信息
            os = socket.getOutputStream();
            os.write("很好".getBytes());

        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {//关闭资源, 方法同上
        }
    }
```

‍

‍

### 双端口完整实现示例

‍

服务端

```java
   public void server() throws IOException {
        int port = 9090;//声明端口号
        ServerSocket serverSocket = new ServerSocket(port);

        //调用accept,接受客户端的Socket
        Socket socket = serverSocket.accept();
        System.out.println("一个客户端建立了连接");

        //接受数据
        InputStream is = socket.getInputStream();
        byte[] buffer = new byte[1024];//创建字节数组
        int len;//记录每次读取的字节个数
        while ((len = is.read(buffer)) != -1) {//循环读取数据
            String str = new String(buffer, 0, len);//将字节数组转换为字符串
            System.out.println(str);

            OutputStream os = socket.getOutputStream();//返回信息
            os.write("很好".getBytes());
        }
            //close
    }
```

‍

‍

客户端

```java
    public void client() throws IOException {
        InetAddress inet = InetAddress.getByName("10.81.33.226");//获取ip地址
        int port = 9090;//声明端口号
        Socket socket = new Socket(inet, port);

        //发送数据
        OutputStream os = socket.getOutputStream();
        os.write("你好，我是客户端mm".getBytes());//写入数据,getBytes()方法将字符串转换为字节数组

        //表示客户端已经发送完数据，后面只能接受数据,如果不写这个方法，会造成双方阻塞
        socket.shutdownOutput();

        //接受返回信息
        InputStream is = socket.getInputStream();
        byte[] buffer = new byte[1024];//创建字节数组
        ByteOutputStream bos = new ByteOutputStream(); //利用字节数出流读取数据转换为字符串
        int len;//记录每次读取的字节个数
        while ((len = is.read(buffer)) != -1) {//循环读取数据
            bos.write(buffer, 0, len);//将字节数组转换为字符串
        }
            //关闭资源
    }
```

‍

‍

‍
