--后端测试工具

‍

后端API开发协作工具, 模拟浏览器向后端服务器发起任何形式(如:get、post)的HTTP请求

替代品可以选择国产Apipost, 但是我还是觉得这个用着舒服

‍

后端测试接口可以直接使用浏览器. 在浏览器中输入地址测试后端程序<sup>（弊端：在浏览器地址栏中输入地址这种方式都是GET请求）</sup>, 但是很明显这太烂了, 使用专业的接口测试工具才是上策

‍

‍

‍

## 使用

    创建工作空间, 在Collection内创建请求post, 然后有手就行

‍

‍

## 注意

‍

post指令需要在Body内进行编辑, key-value常用, JSON使用raw格式

​​

‍

字符编码问题, 需要修改Postman里面的headers属性,新建自定义的Content-Type 和Accept,指定;charset=utf8

‍

Postman发送JSON格式数据一定要使用POST方法,因为数据要**放在请求体内**

‍

‍

‍
