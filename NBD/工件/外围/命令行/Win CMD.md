--控制台相关

‍

### Header

控制台

**cmd**是**command**的缩写.即命令行 (win bash)

‍

‍

## 常用命令

‍

```cmd
help  查看帮助.找到命令之后，使用 命令+ /?来查看该命令下的其他属性

cls clean screen

exit : 退出dos 命令行

d:        //跳转到其他硬盘,直接小写名称冒号
```

‍

### 目录查看

```cmd
cd **..**       //跳转到上一层目录

cd C:\WINDOWS  //跳转到当前硬盘的其他文件

cd \       //跳转到硬盘的根目录

cd /d e:\software    //跳转到其他硬盘的其他文件夹，注意此处必须加/d参数。否则无法跳转。
```

‍

```cmd
dir  //查看当前目录下的文件

dir /?  Help
```

‍

### 目录操作

md 目录名    创建目录

rd 目录名    删除目录

del 文件名    专门删除文件的

‍

copy 路径\文件名 路径\文件名    拷贝    

move 路径\文件名 路径\文件名    移动    

‍

‍

‍

## 特殊指令 

‍

ipconfig  IP

‍

netstat –ano | findStr  XX     查找对应端口有没有被占用

‍

powercfg /batteryreport      电源健康管理

‍

## 操作

运用CMD实现多开

例如,java在对应的目录点击资源管理器后输入cmd,之后输入javac work1.java 进行编译,然后java work1 运行即可多开一个

‍
