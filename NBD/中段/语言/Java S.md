--爪哇特种章

‍

‍

## 多线程

‍

‍

‍

‍

## 可视化

‍

### 原生Swing窗体

> 仅记录

基本前端页面设计

```java
        JFrame frame = new JFrame("Login Example");

        frame.setSize(350, 200);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        JPanel panel = new JPanel();

        frame.add(panel);
        frame.setVisible(true);
```

‍

动作监听

```java
    @Override
    public void actionPerformed(ActionEvent e) {
        if (e.getSource() == this.button_equal) {
                ......
            }
        }
    }
```

‍

## 多平台

‍

### CMD

控制台里既编译又运行java文件 <kbd>java + 文件名.java</kbd>​

‍

### Linux

‍

‍

## 其他

‍

### 查看系统的环境变量

```java
public static void main(String[] args) throws Exception{
    Map<String, String> env = System.getenv();
    System.out.println(env);
}
```

打印出来的结果中会有一个USERNAME=XXX[自己电脑的用户名称]
