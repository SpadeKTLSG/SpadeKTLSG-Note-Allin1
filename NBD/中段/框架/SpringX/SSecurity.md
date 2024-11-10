‍

> 未来

‍

‍

关于权限细粒度的控制

是高级的内容SpringBoot引入

学习成本高

本质上就是一个过滤器链

‍

1、介绍  
​`Spring Security`​是一个能够为基于`Spring`​的企业应用系统提供声明式的安全访问控制解决方案的安全框架.

2、功能  
​`Authentication`​ 认证，就是用户登录  
​`Authorization`​ 授权，判断用户拥有什么权限，可以访问什么资源  
安全防护，跨站脚本攻击，`session`​攻击等  
非常容易结合`Spring`​进行使用

3、`Spring Security`​与`Shiro`​的区别

> 相同点

1、认证功能  
2、授权功能  
3、加密功能  
4、会话管理  
5、缓存支持  
6、rememberMe功能  
....

> 不同点

‍

优点：

1、Spring Security基于Spring开发，项目如果使用Spring作为基础，配合Spring Security做权限更加方便. 而Shiro需要和Spring进行整合开发  
2、Spring Security功能比Shiro更加丰富，例如安全防护方面  
3、Spring Security社区资源相对比Shiro更加丰富

缺点：

1）Shiro的配置和使用比较简单，Spring Security上手复杂些  
2）Shiro依赖性低，不需要依赖任何框架和容器，可以独立运行. Spring Security依赖Spring容器
