--与Mybatis分庭抗礼

‍

Spring Data JPA为Java Persistence API（JPA）提供了实现. 它简化了通过JPA访问数据库的开发工作，提供了很多CRUD的快捷操作，还提供了如分页、排序、复杂查询、自定义查询（JPQL）等功能，Spring Data JPA底层也是依赖于Hibernate来实现的，Spring Data JPA拥有标准化、简单易用、面向对象等优势，并且Spring将EntityManager 的创建与销毁、事务管理等代码抽取出来，并由Spring统一进行管理. 

‍

‍

‍

## Header

‍

# 知识

‍

## 评价

> Mybatis容易上手，现在学编程学数据库基本都学过SQL，Mybatis面向DB基于SQL的模式相对来说就显得特别直观友好
>
> 而**JPA是基于ORM的，把代码和DB分离**，相当于在代码和DB之间增加了一个新的层面，一套新的标准，去间接操作DB，相对SQL的模式来说就显得不够直接和易于控制，增加了学习成本，碰到这样那样问题的时候，很容易让人放弃. 特别是在团队开发的时候，你很难掌控每个人对JPA的深层机制的理解程度，为了避免木桶效应，还不如**干脆都用Mybatis**
>
> 其实这两者就功能的角度来说，肯定是相互学习借鉴的，Spring Data JPA也可以执行原生SQL，存储过程，Mybatis也有一部分ORM特性，选择Spring Data JPA还是选择Mybatis都不会影响到最终业务逻辑的实现

‍

# 基础

‍

‍

# 高级

‍

‍
