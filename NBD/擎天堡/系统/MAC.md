‍

[Link](https://imageslr.com/2020/03/19/mac-initialization#%E5%8A%A0%E9%80%9F-zoom-%E5%8A%A8%E7%94%BB)

导出思源Mac工作区配置

‍

## 快捷键

‍

[Mac 键盘快捷键 - 官方 Apple 支持 (中国)](https://support.apple.com/zh-cn/102650)

长按最大化按钮可以拼接窗口到左右分栏

将右手的Command和option分配为了方向键修改调度键 和 显示桌面键

​<kbd>Shift + Command + .</kbd>​    显示隐藏文件

​<kbd>Shift + Cm + C</kbd>​    剪贴板(自定义)

​<kbd>Fn + BK</kbd>​    删除

​<kbd>Con + Alt + A </kbd>​    ~~阿里钉截图~~

​<kbd>Con + Shift + Q </kbd>​    ~~搜狗截图~~

​<kbd>Shift + Cm + 4 / 5 </kbd>​    自带截图(推荐)

​<kbd>Con + Cm + Q</kbd>​     锁屏

​<kbd>Con + 方向↑/↓</kbd>​     打开调度​​

​<kbd>Shift + Op + K </kbd>​    展示苹果标签

​<kbd>Cm + Q </kbd>​    关闭应用(别手贱)

​<kbd>Cm + Op + Esc </kbd>​    强制退出程序

​<kbd>Cm + Shift + [ / ] </kbd>​    左右切换页签

​<kbd>Cm + Shift + 5</kbd>​    截屏, 可以选择编辑后钉在桌面

​<kbd>Cm + Con + Q</kbd>​    锁屏

改键 - 右手Command + JIKL切换方向键

‍

‍

## 操作

**zsh 和 bash 的环境变量**

```java
sudo vim ~/.zshrc 
```

bash 的环境变量是`.bash_profile`​文件。  
zsh 的环境变量是`.zshrc`​文件。

如果从 bash 切换到 zsh，但想保留 bash 所设置的环境变量，可在 `.zshrc`​文件末尾添加 `source ~/.bash_profile`​ 保存退出，并重启终端即可使用 bash 的环境变量

‍

‍

性能监视器崩溃 - 能耗按钮闪退, 重启

```java
rm -rf ~/Library/Preferences/com.apple.ActivityMonitor.plist
```

‍

Homebrew自动更新关闭

```java
vim ~/.zshrc
```

```java
export HOMEBREW_NO_AUTO_UPDATE=true
```

‍

---

Node安装

‍

创建一个用于存储全局安装的文件夹

```java
/usr/local/lib/npm
```

‍

配置npm本地仓库(全局安装路径)

```java
sudo npm config set prefix /usr/local/lib/npm
```

将 npm 的 bin 目录添加到系统的 PATH 环境变量中

```java
echo 'export PATH="/usr/local/lib/npm/bin:$PATH"' >> ~/.zshrc_profile
source ~/.zshrc_profile

```

验证

```java
npm config get prefix
echo $PATH
```

检查全局安装的软件包是否可用

```java
npm list -g
```

‍

建议配合root环境使用, 自动补全缺少的软件包目录结构

```java
npm install jquery -g
```

npm list -g再次验证

‍

‍

## 工作区

‍

需要找破解版的

* WPS文档 - 临时用VSCODE
* Navicat
* Postman
* ...

其余见copy的页面截图
