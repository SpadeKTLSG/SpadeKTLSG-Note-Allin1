--Java牛马一条龙前端服务

‍

‍

全称Java Server Pages，是一种动态网页开发技术. JSP 与 PHP、ASP、ASP.NET 等语言类似，是运行在服务端的语言

就是在HTML中引入了java, 脚本小程序的标记`<% %>`​

‍

JSP是一种基于文本的程序，其特点就是HTML和Java代码共同存在！

JSP是为了简化Servlet的工作出现的替代品，Servlet输出HTML非常困难，JSP就是替代Servlet输出HTML的。

Tomcat访问任何的资源都是在访问Servlet！，当然了，JSP也不例外！JSP本身就是一种Servlet

‍

‍

### Header

‍

‍

# 知识

‍

‍

‍

## 概念

‍

‍

### 是什么

‍

**前端还是后端?**

jsp代码编写属于后端，因为前端开发看不懂JSP代码，他们追求的是网页效果。而JSP其实就是JAVA代码来拼接前端页面而已，本身也是Servlet，因此JAVA

WEB工程师也要学习一些HTML，JS，CSS等，实际开法中前端工程师写好网页，Java  
web开发人员负责填写JSP脚本，也可以反过来，后端定义好标签库，让前端以标签的方式来写代码。

‍

‍

# 基础

‍

‍

## 搭建

‍

‍

### 导入

‍

项目开发中会经常性的使用到其他包中的程序类，此时就可以利用page指令中的import属性实现开发包的导入操作，在使用import时可以单独导入一个包，也可以同时导入多个开发包

‍

​` <%@page import="开发包1,开发包2,开发包3"%>`​

‍

‍

#### 静态导入

<%@ include file="要包含的文件路径"%>

‍

#### 动态导入

‍

使用静态导入进行并不会关心具体的文件类型，而是将程序代码全部合并在一起后再进行处理，而除了此类导入之外，在JSP中又提供了一个动态导入操作，该导入功能最大的特点是可以区分被导入的页面是动态文件还是静态文件，如果是静态文件则功能与静态导入相同，如果是动态文件，则先进行动态页面的处理，然后再将结果包含进来

动态>>静态

‍

**形式一：** 导入页面时不传递任何参数

<jsp:include page="导入文件路径"/>

‍

**形式二：** 导入页面同时传递参数

```java
    String URL= "";

     <jsp:include page="导入文件路径">
	    <jsp:param name="参数名称" value="参数内容"/>
	    <jsp:param name="参数名称" value="参数内容"/>
        <jsp:param name="url" value="<%=URL%>"/> 
		    ... ... ...
	 </jsp:include>

```

把变量传过去就用`<%= %>`​

‍

```java
在使用“<jsp:include>”指令的时候，内部允许定义的仅仅是“<jsp:param>”标签，所有的其他的内容都不允许在其中进行定义，包括注释内容。
```

‍

#### 区别

通过之前的分析可以发现，不管是动态包含还是静态包含，在实现静态文件包含时功能相同，而在实现动态文件包含时，两者的处理机制会有所不同：

静态包含：采用先包含后处理的形式，即将所需要的代码先包含到程序之中，而后一起解析；  
动态包含：采用先处理后包含的形式，即不包含程序的源代码，只是包含程序的执行结果.

‍

因为需要将两个代码合并在一起进行统一的处理，所以这样一来就会造成代码中存在有两个num变量的形式，所以静态导入的处理流程是先进行导入，而后再进行整体处理,一旦被导入的页面和导入页面之中产生一些命名的冲突，程序就会出错

结论:静态包含采用的是先包含后处理，而动态包含采用的是先处理后包含的形式，开发中肯定要推荐使用动态包含，因为其可以有效的避免重名对程序所造成的影响，最关键的是还可以向被包含页面传递所需要的参数内容

‍

‍

## 示例

‍

### IDEA整合

JDK17

空项目导入纯Java模块, 而后~~右键引入Web框架支持~~在(IDEA2023)项目结构中选择Facet添加WEB工件, 注意要把最底下的工件也加上

‍

‍

WEB-INF >>hello.jsp

```html
<%--
  Created by IntelliJ IDEA.
  User: SpadeK
  Date: 2023/11/9
  Time: 16:32
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>
        哈哈哈哈哈
    </title>
</head>
<body>
<%
    out.println("hhhh");
%>
</body>
</html>
```

> 虽然此时有错误，但是这些错误是依赖库造成的，不会影响到程序的执行，因为很多程序的依赖库实际上是保存在了Tomcat下，而后当程序运行时会自动的加载这些依赖库，实现程序的正确执行.

‍

### 动态入门

在进行表单定义的时候有两个最为重要的组成结构  
·表单的提交路径，其规定了本次表单到底该交给谁去处理;  
·表单组件，每一个组件之中都会包含有大量的参数，其中通过name属性定义参数名称.

‍

我电脑是UTF-8, 记得加上

```html
	<meta charset="UTF-8">
```

‍

创建表单input_test.html

```html
<html>
<head>
	<title>CWX www.sk.com</title>
	<meta charset="UTF-8">
</head>
<body>

<form action="input_do.jsp" method="post">
	呼叫, 呼叫!
    <input type="text" name="msg" value="SpadeK是天才!"><br>
	<input type="submit" value="报告">
</form>

</body>
</html>
```

如果要想实现表单数据的接收处理,可以直接使用 "request.getParameter(参数名称)" 方法来完成，该方法返回有一个String对象内容，表示用户输入的数据信息

‍

创建JSP处理input_do.jsp

```html
<html>
<head>
	<title>www.sk.com</title>
	<meta charset="UTF-8">
</head>
<body>
<%
	String paramMsg = request.getParameter("msg"); //msg是表单名称, 获取内容到paramMsg
	out.println("<h1>" + paramMsg + "</h1>"); //直接输出拼接的页面
%>
</body>
</html>
```

‍

开启Tomcat服务后访问html表单页面, 自动跳转到JSP文件

‍

‍

‍

‍

‍

‍

‍

‍

‍

### CRUD

‍

例如广告管理案例

‍

‍

加载广告数据（页面名称：index.jsp）

```html
<div id="carouselMenu" class="carousel slide" data-interval="200" data-wrap="true">
   <%
      String sql = "SELECT title,url,photo FROM ad"; 	// 查询SQL
      PreparedStatement pstmt = DatabaseConnection.getConnection()
            	.prepareStatement(sql);                	// 获取接口实例
      ResultSet rs = pstmt.executeQuery() ;           	// 数据查询
   %>

   <!-- 轮播（Carousel）内容显示，显示内容的个数与索引控制项对应 -->
   <div class="carousel-inner">
   <%      int index = 0;
      while (rs.next()) {
         String title = rs.getString(1); 	// 获取title字段
         String url = rs.getString(2); 			// 获取url字段数据
         String photo = rs.getString(3); 	// 获取photo 字段数据
   %>
         <div class="item <%=index ++ == 0 ? "active": ""%>">
            <a href="<%=url%>" target="_ablank" title="<%=title%>">
            <img src="upload/ad/<%=photo%>"></a></div>
   <%      }      DatabaseConnection.close();		// 关闭数据库连接
   %>
   </div>
   <!-- 轮播（Carousel）导航 -->
   <a class="carousel-control left" href="#carouselMenu"
      data-slide="prev">&lsaquo;</a>     <!-- 显示前一个轮播项内容 -->
   <a class="carousel-control right" href="#carouselMenu"
      data-slide="next">&rsaquo;</a>     <!-- 显示后一个轮播项内容-->
</div>

```

可以发现每一个“<div>”元素里面都包含有一个超链接的地址("href="https : //www . yootk.com"")，就可以直接通  
过一个最基础的查询列出全部的数据项. ·

‍

‍

增加

‍

在进行最终请求参数接收的时候，一定要使用FileUpload组件进行包装，所以肯定是通过ParametterUtil工具类来完成所有请求参数接收处理的.

（页面名称：ad_add_do.jsp）

为了便于路径的维护，可以将路径定义为一个变量的形式，这样直接在表单中进行引用即可:

```html
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page import="com.yootk.dbc.DatabaseConnection" %>
<%@ page import="com.yootk.common.util.ParameterUtil" %>
<%@ page import="java.sql.*" %>
<%@include file="/pages/plugins/include_basepath.jsp" %>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
<head>
<base href="<%=basePath%>">
<jsp:include page="/pages/plugins/include_javascript.jsp" />
<script type="text/javascript" src="js/pages/ad_add.js"></script>
<script type="text/javascript" src="bootstrap/tinymce/tinymce.min.js"></script>
</head>
<body class="hold-transition skin-blue sidebar-mini">
	<div id="navbarDiv" class="row">
		<jsp:include page="/pages/plugins/include_navbar.jsp"/>
	</div>
	<div style="height: 50px;">&nbsp;</div>
	<div class="wrapper">
		<div class="content-wrapper">
			<div class="panel panel-success">
				<div class="panel-heading">
					<strong><i class="fa fa-file-picture-o"></i>&nbsp;增加广告项</strong>
				</div>
				<div class="panel-body">
				<%
					request.setCharacterEncoding("UTF-8"); // 设置请求编码
					ParameterUtil pu = new ParameterUtil(request, "/upload/ad/"); // 广告的图片是有存储路径的
					// 通过ParameterUtil工具组件获取用户所提交的请求参数
					List<String> fileNames = pu.getUUIDName("pic"); // 获取上传的文件内容
					pu.saveUploadFile("pic", fileNames); // 实现了上传文件的存储
					pu.clean(); // 清空临时目录
					String title = pu.getParameter("title");
					String url = pu.getParameter("url");
					String note = pu.getParameter("note");
					String sql = "INSERT INTO ad (title,url,photo,note) VALUES (?,?,?,?)";
					PreparedStatement pstmt = DatabaseConnection.getConnection().prepareStatement(sql);
					pstmt.setString(1, title);
					pstmt.setString(2, url);
					pstmt.setString(3, fileNames.get(0)); // 文件的图片名称是通过程序计算得来的
					pstmt.setString(4, note);
					int result = pstmt.executeUpdate(); // 执行数据更新处理
					DatabaseConnection.close(); // 关闭数据库连接
					if (result > 0) {
				%>
						<h1>广告项增加成功！</h1>
				<%
					} else {
				%>
						<h1>广告项增加失败！</h1>
				<%
					}
				%>
				</div>
				<div class="panel-footer">
					<jsp:include page="plugins/include_foot.jsp"/>
				</div>
			</div>
		</div>
	</div>
</body>
</html>

```

‍

‍

广告列表显示（页面名称：ad_list.jsp）

```html
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page import="com.yootk.dbc.DatabaseConnection" %>
<%@ page import="java.sql.*" %>
<%@include file="/pages/plugins/include_basepath.jsp" %>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
<head>
<base href="<%=basePath%>">
<%
	String addNewsUrl = basePath + "pages/ad_add.jsp" ;
	String editNewsUrl = basePath + "pages/ad_edit.jsp" ;
	String deleteNewsUrl = basePath + "pages/ad_delete.jsp" ;
%>
	<jsp:include page="/pages/plugins/include_javascript.jsp" />
<script type="text/javascript" src="js/pages/add_list.js"></script>
</head>
<body class="hold-transition skin-blue sidebar-mini">
	<div id="navbarDiv" class="row">
		<jsp:include page="/pages/plugins/include_navbar.jsp"/>
	</div>
	<div style="height: 50px;">&nbsp;</div>
	<div class="wrapper">
		<div class="content-wrapper">
			<div class="panel panel-success">
				<div class="panel-heading">
					<strong><i class="fa fa-archive"></i>&nbsp;首页大幅广告列表</strong>
					<a href="<%=addNewsUrl%>" id="addBtn" class="btn btn-xs btn-primary"><span class="glyphicon glyphicon-plus-sign"></span>&nbsp;发布公告</a>
				</div>
				<div class="panel-body">
					<table class="table table-hover">
						<tr>
							<th width="40%" class="text-center">标题</th>
							<th width="20%" class="text-center">链接地址</th>
							<th width="30%" class="text-center">广告图片</th>
							<th width="10%" class="text-center">操作</th>
						</tr>
						<%
							String sql = "SELECT aid,title,url,photo FROM ad";
							PreparedStatement pstmt = DatabaseConnection.getConnection().prepareStatement(sql); // 数据查询
							ResultSet rs = pstmt.executeQuery(); // 执行数据查询
							int index = 0; // 作为首页的“active”的配置
							while (rs.next()) {
								long aid = rs.getLong(1);
								String title = rs.getString(2);
								String url = rs.getString(3);
								String photo = rs.getString(4); // 图片通过了一个文件名称进行描述
						%>
						<tr>
							<td class="text-left"><%=title%></td>
							<td class="text-left"><%=url%></td>
							<td class="text-center"><img src="upload/ad/<%=photo%>" alt="f3Img" style="width: 300px;"></td>
							<td class="text-center">
								<a href="<%=editNewsUrl%>?aid=<%=aid%>" class="btn btn-xs btn-primary"><span class="glyphicon glyphicon-edit"></span>&nbsp;编辑</a>
								<a href="<%=deleteNewsUrl%>?aid=<%=aid%>&photo=<%=photo%>" class="btn btn-xs btn-danger"><span class="glyphicon glyphicon-remove"></span>&nbsp;删除</a></td>
						</tr>
						<%
							}
							DatabaseConnection.close();
						%>
					</table>
				</div>
				<div class="panel-footer">
					<jsp:include page="plugins/include_foot.jsp"/>
				</div>
			</div>
		</div>
	</div>
</body>
</html>

```

‍

‍

编辑

表单回填页（页面名称：ad_edit.jsp）

```html
<%
   ParameterUtil pu = new ParameterUtil(request, "/upload/ad"); 	// 参数工具
   long aid = Long.parseLong(pu.getParameter("aid")); 		// 获取提交参数
   String sql = "SELECT title,url,photo,note FROM ad WHERE aid=?"; 	// 查询SQL
   PreparedStatement pstmt = DatabaseConnection.getConnection()
         		.prepareStatement(sql);                		// 获取接口实例
   pstmt.setLong(1, aid); 						// 设置SQL参数
   ResultSet rs = pstmt.executeQuery() ;           			// 数据查询
   if (rs.next()) {						// 数据存在
      String title = rs.getString(1); 				// 获取title字段数据
      String url = rs.getString(2); 				// 获取url字段数据
      String photo = rs.getString(3); 				// 获取photo字段数据
      String note = rs.getString(4); 				// 获取note字段数据
%>
<% }
   DatabaseConnection.close(); // 关闭数据库连接
%>

```

```html
<form action="pages/ad_edit_do.jsp" id="myform" method="post" 	class="form-horizontal" enctype="multipart/form-data">
   <div class="form-group" id="titleDiv">
      <label class="col-md-2 control-label" for="title">标题：</label>
      <div class="col-md-5">
         <input type="text" name="title" id="title" class="form-control input-sm“ placeholder="请输入标题" value="<%=title%>">
      </div><div class="col-md-4" id="titleMsg">*</div>
   </div>
   <div class="form-group" id="urlDiv">
      <label class="col-md-2 control-label" for="title">链接：</label>
      <div class="col-md-5">
         <input type="text" name="url" id="url" class="form-control input-sm“ placeholder="请输入广告链接路径" value="<%=url%>">
      </div>div class="col-md-4" id="urlMsg">*</div>
   </div>
   <div class="form-group" id="picDiv">
      <label class="col-md-2 control-label" for="pic">图片：</label>
      <div class="col-md-5">
         <input type="file" name="pic" id="pic" class="form-control input-sm“ placeholder="请选择发布的图片">
      </div><div class="col-md-4" id="picMsg">*</div>
   </div>
   <div class="form-group" id="noteDiv">
      <label class="col-md-2 control-label" for="note">备注：</label>
      <div class="col-md-9">
         <textarea id="note" name="note" class="form-control“ rows="10"><%=note%></textarea>
      </div>
   </div>
   <div class="form-group">
      <div class="col-md-offset-2 col-md-5">
         <%-- 将当前数据的ID编号以及原始的photo名称传递到目标页面，这样才可以实现数据更新 --%>
         <input type="hidden" name="aid" value="<%=aid%>">
         <input type="hidden" name="photo" value="<%=photo%>">
         <input type="submit" value="编辑" class="btn btn-sm btn-primary">
         <input type="reset" value="重置" class="btn btn-sm btn-warning">
      </div>
   </div>
</form>


```

‍

广告更新页面（页面名称：ad_edit_do.jsp）

```html
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page import="com.yootk.dbc.DatabaseConnection" %>
<%@ page import="com.yootk.common.util.ParameterUtil" %>
<%@ page import="java.sql.*" %>
<%@include file="/pages/plugins/include_basepath.jsp" %>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
<head>
<base href="<%=basePath%>">
<jsp:include page="/pages/plugins/include_javascript.jsp" />
<script type="text/javascript" src="js/pages/ad_add.js"></script>
<script type="text/javascript" src="bootstrap/tinymce/tinymce.min.js"></script>
</head>
<body class="hold-transition skin-blue sidebar-mini">
	<div id="navbarDiv" class="row">
		<jsp:include page="/pages/plugins/include_navbar.jsp"/>
	</div>
	<div style="height: 50px;">&nbsp;</div>
	<div class="wrapper">
		<div class="content-wrapper">
			<div class="panel panel-success">
				<div class="panel-heading">
					<strong><i class="fa fa-file-picture-o"></i>&nbsp;编辑广告项</strong>
				</div>
				<div class="panel-body">
				<%
					request.setCharacterEncoding("UTF-8"); // 设置请求编码
					ParameterUtil pu = new ParameterUtil(request, "/upload/ad/"); // 广告的图片是有存储路径的
					// 通过ParameterUtil工具组件获取用户所提交的请求参数
					List<String> fileNames = pu.getUUIDName("pic"); // 获取上传的文件内容
					long aid = Long.parseLong(pu.getParameter("aid"));
					String title = pu.getParameter("title");
					String url = pu.getParameter("url");
					String note = pu.getParameter("note");
					String photo = pu.getParameter("photo"); // 原始的图片名称

					if (fileNames.size() > 0) {	// 有上传内容
						pu.saveUploadFile("pic", Arrays.asList(photo));
						pu.clean(); // 清除操作
					}

					String sql = "UPDATE ad SET title=?,url=?,photo=?,note=? WHERE aid=?";
					PreparedStatement pstmt = DatabaseConnection.getConnection().prepareStatement(sql);
					pstmt.setString(1, title);
					pstmt.setString(2, url);
					pstmt.setString(3, photo); // 原始图片名称
					pstmt.setString(4, note);
					pstmt.setLong(5, aid);
					int result = pstmt.executeUpdate(); // 执行数据更新处理
					DatabaseConnection.close(); // 关闭数据库连接
					if (result > 0) {
				%>
						<h1>广告项修改成功！</h1>
				<%
					} else {
				%>
						<h1>广告项修改失败！</h1>
				<%
					}
				%>
				</div>
				<div class="panel-footer">
					<jsp:include page="plugins/include_foot.jsp"/>
				</div>
			</div>
		</div>
	</div>
</body>
</html>

```

‍

删除

‍

```html
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page import="com.yootk.dbc.DatabaseConnection" %>
<%@ page import="com.yootk.common.util.ParameterUtil" %>
<%@ page import="java.sql.*" %>
<%@include file="/pages/plugins/include_basepath.jsp" %>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
<head>
<base href="<%=basePath%>">
<jsp:include page="/pages/plugins/include_javascript.jsp" />
<script type="text/javascript" src="js/pages/ad_add.js"></script>
<script type="text/javascript" src="bootstrap/tinymce/tinymce.min.js"></script>
</head>
<body class="hold-transition skin-blue sidebar-mini">
	<div id="navbarDiv" class="row">
		<jsp:include page="/pages/plugins/include_navbar.jsp"/>
	</div>
	<div style="height: 50px;">&nbsp;</div>
	<div class="wrapper">
		<div class="content-wrapper">
			<div class="panel panel-success">
				<div class="panel-heading">
					<strong><i class="fa fa-file-picture-o"></i>&nbsp;删除广告项</strong>
				</div>
				<div class="panel-body">
				<%
					request.setCharacterEncoding("UTF-8"); // 设置请求编码
					ParameterUtil pu = new ParameterUtil(request, "/upload/ad/"); // 广告的图片是有存储路径的
					long aid = Long.parseLong(pu.getParameter("aid")); // 接收删除id
					String photo = pu.getParameter("photo"); // 原始的图片名称

					String sql = "DELETE FROM ad WHERE aid=?";
					PreparedStatement pstmt = DatabaseConnection.getConnection().prepareStatement(sql);
					pstmt.setLong(1, aid);
					int result = pstmt.executeUpdate(); // 执行数据更新处理
					DatabaseConnection.close(); // 关闭数据库连接
					if (result > 0) {
						java.io.File file = new java.io.File(this.getServletContext().getRealPath("/upload/ad/"),photo);
						if (file.exists()) {
							file.delete(); // 文件删除
						}
				%>
						<h1>广告项删除成功！</h1>
				<%
					} else {
				%>
						<h1>广告项删除失败！</h1>
				<%
					}
				%>
				</div>
				<div class="panel-footer">
					<jsp:include page="plugins/include_foot.jsp"/>
				</div>
			</div>
		</div>
	</div>
</body>
</html>

```

‍

## 执行流程

当用户编写完成了一个“hello.jsp”程序并且成功运行之后会发现一个非常有意思的现象：第一次执行的速度较慢，之后执行的速度较快，之所以会出现这样的问题，是因为在Tomcat内部需要将jsp程序转为java源代码，最后再自动编译为*.class文件.

‍

1、如果现在通过浏览器发送一个访问请求:“http://localhost/yootk.jsp”，该请求属于一个GET请求;

2、Tomcat接收到了用户发送的请求之后，会自动找到与之匹配的资源(yootk.jsp);

3、考虑到执行本质性的问题，所以在Tomcat 内部会自动的将“JSP”文件转为“java”源代码程序，随后再自动进行编译成字节码文件

4、利用字节码文件进行所有的HTML响应输出;

5、如果再一次访问时，由于已经生成了相应的字节码文件，所以执行的速度就会加快.

‍

在“${TOMCAT_HOME)”目录中存在有一个“work”子目录，这个目录保存了所有的临时文件,其中也包括了JSP转为Java以及Java编译为class文件的所有代码.

查看对应名称.java文件

‍

上面的hello.jsp文件生成的.java文件

```java
package org.apache.jsp;

import javax.servlet.*;
import javax.servlet.http.*;
import javax.servlet.jsp.*;

public final class hello_jsp extends org.apache.jasper.runtime.HttpJspBase
    implements org.apache.jasper.runtime.JspSourceDependent,
                 org.apache.jasper.runtime.JspSourceImports {

  private static final javax.servlet.jsp.JspFactory _jspxFactory =
          javax.servlet.jsp.JspFactory.getDefaultFactory();

  private static java.util.Map<java.lang.String,java.lang.Long> _jspx_dependants;

  private static final java.util.Set<java.lang.String> _jspx_imports_packages;

  private static final java.util.Set<java.lang.String> _jspx_imports_classes;

  static {
    _jspx_imports_packages = new java.util.HashSet<>();
    _jspx_imports_packages.add("javax.servlet");
    _jspx_imports_packages.add("javax.servlet.http");
    _jspx_imports_packages.add("javax.servlet.jsp");
    _jspx_imports_classes = null;
  }

  private volatile javax.el.ExpressionFactory _el_expressionfactory;
  private volatile org.apache.tomcat.InstanceManager _jsp_instancemanager;

  public java.util.Map<java.lang.String,java.lang.Long> getDependants() {
    return _jspx_dependants;
  }

  public java.util.Set<java.lang.String> getPackageImports() {
    return _jspx_imports_packages;
  }

  public java.util.Set<java.lang.String> getClassImports() {
    return _jspx_imports_classes;
  }

  public javax.el.ExpressionFactory _jsp_getExpressionFactory() {
    if (_el_expressionfactory == null) {
      synchronized (this) {
        if (_el_expressionfactory == null) {
          _el_expressionfactory = _jspxFactory.getJspApplicationContext(getServletConfig().getServletContext()).getExpressionFactory();
        }
      }
    }
    return _el_expressionfactory;
  }

  public org.apache.tomcat.InstanceManager _jsp_getInstanceManager() {
    if (_jsp_instancemanager == null) {
      synchronized (this) {
        if (_jsp_instancemanager == null) {
          _jsp_instancemanager = org.apache.jasper.runtime.InstanceManagerFactory.getInstanceManager(getServletConfig());
        }
      }
    }
    return _jsp_instancemanager;
  }

  public void _jspInit() {
  }

  public void _jspDestroy() {
  }

  public void _jspService(final javax.servlet.http.HttpServletRequest request, final javax.servlet.http.HttpServletResponse response)
      throws java.io.IOException, javax.servlet.ServletException {

    if (!javax.servlet.DispatcherType.ERROR.equals(request.getDispatcherType())) {
      final java.lang.String _jspx_method = request.getMethod();
      if ("OPTIONS".equals(_jspx_method)) {
        response.setHeader("Allow","GET, HEAD, POST, OPTIONS");
        return;
      }
      if (!"GET".equals(_jspx_method) && !"POST".equals(_jspx_method) && !"HEAD".equals(_jspx_method)) {
        response.setHeader("Allow","GET, HEAD, POST, OPTIONS");
        response.sendError(HttpServletResponse.SC_METHOD_NOT_ALLOWED, "JSP 只允许 GET、POST 或 HEAD。Jasper 还允许 OPTIONS");
        return;
      }
    }

    final javax.servlet.jsp.PageContext pageContext;
    javax.servlet.http.HttpSession session = null;
    final javax.servlet.ServletContext application;
    final javax.servlet.ServletConfig config;
    javax.servlet.jsp.JspWriter out = null;
    final java.lang.Object page = this;
    javax.servlet.jsp.JspWriter _jspx_out = null;
    javax.servlet.jsp.PageContext _jspx_page_context = null;


    try {
      response.setContentType("text/html");
      pageContext = _jspxFactory.getPageContext(this, request, response,
      			null, true, 8192, true);
      _jspx_page_context = pageContext;
      application = pageContext.getServletContext();
      config = pageContext.getServletConfig();
      session = pageContext.getSession();
      out = pageContext.getOut();
      _jspx_out = out;

      out.write("<!DOCTYPE html>\r\n");
      out.write("\r\n");
      out.write("<head>\r\n");
      out.write("	<title>LOSER</title>\r\n");
      out.write("</head>\r\n");
      out.write("\r\n");
      out.write("<body>\r\n");
      out.write("    ");

        out.println("<h1>LOSER</h1>");
        out.println("<script type='text/javascript'>");
        out.println("console.log('07210721')");
        out.println("</script>");
  
      out.write("\r\n");
      out.write("\r\n");
      out.write("</body>\r\n");
      out.write("</html>");
    } catch (java.lang.Throwable t) {
      if (!(t instanceof javax.servlet.jsp.SkipPageException)){
        out = _jspx_out;
        if (out != null && out.getBufferSize() != 0)
          try {
            if (response.isCommitted()) {
              out.flush();
            } else {
              out.clearBuffer();
            }
          } catch (java.io.IOException e) {}
        if (_jspx_page_context != null) _jspx_page_context.handlePageException(t);
        else throw new ServletException(t);
      }
    } finally {
      _jspxFactory.releasePageContext(_jspx_page_context);
    }
  }
}

```

‍

‍

所有的这些文件都是在每次执行的时候动态生成的，所以如果说现在将work目录清空，那么对于Tomcat来说在每次执行  
JSP程序的时候就会自动的重新生成相应的文件.

实际上在早期开发Java的时候，当时的电脑硬件性能可没有这么强大，当年开发的时候如果说电脑上启动的服务过多，那么就会出现JSP程序不编译的问题，每一次修改都需要重新进行代码转换与编译处理，但是由于硬件的性能太差了，所以这种机制就有可能出现不稳定的情况，所以当时的做法就必须手工的进行 work目录的删除.

实际上早期的Tomcat也存在有安全性的问题，因为所有的jsp程序最终都会转为*.class文件执行，但是如果假设你的服务访问量过大，实际上也会出现JSP程序不编译的情况，会直接将JSP的源代码发送到客户端上.

‍

‍

## 表单处理

我们在浏览网页的时候，经常需要向服务器提交信息，并让后台程序处理. 浏览器中使用 GET 和 POST 方法向服务器提交数据.

---

GET 方法

GET方法将请求的编码信息添加在网址后面，网址与编码信息通过"?"号分隔. 如下所示：

```
http://www.runoob.com/hello?key1=value1&key2=value2
```

GET方法是浏览器默认传递参数的方法，一些敏感信息，如密码等建议不使用GET方法.

用get时，传输数据的大小有限制 （注意不是参数的个数有限制），最大为1024字节.

---

### POST 方法

一些敏感信息，如密码等我们可以通过POST方法传递，POST提交数据是隐式的.

POST提交数据是不可见的，GET是通过在url里面传递的（可以看一下你浏览器的地址栏）.

JSP使用getParameter()来获得传递的参数，getInputStream()方法用来处理客户端的二进制数据流的请求.

---

### JSP 读取表单数据

* **getParameter():**  使用 request.getParameter() 方法来获取表单参数的值.
* **getParameterValues():**  获得如checkbox类（名字相同，但值有多个）的数据.  接收数组变量 ，如checkbox类型
* **getParameterNames():** 该方法可以取得所有变量的名称，该方法返回一个 Enumeration.
* **getInputStream():** 调用此方法来读取来自客户端的二进制数据流.

‍

## 生命周期

JSP生命周期中所走过的几个阶段：

* **编译阶段：** servlet容器编译servlet源文件，生成servlet类
* **初始化阶段：** 加载与JSP对应的servlet类，创建其实例，并调用它的初始化方法
* **执行阶段：** 调用与JSP对应的servlet实例的服务方法
* **销毁阶段：** 调用与JSP对应的servlet实例的销毁方法，然后销毁servlet实例

很明显，JSP生命周期的四个主要阶段和servlet生命周期非常相似

‍

‍

### JSP编译

当浏览器请求JSP页面时，JSP引擎会首先去检查是否需要编译这个文件. 如果这个文件没有被编译过，或者在上次编译后被更改过，则编译这个JSP文件.

编译的过程包括三个步骤：

* 解析JSP文件.
* 将JSP文件转为servlet.
* 编译servlet.

‍

### JSP初始化

容器载入JSP文件后，它会在为请求提供任何服务前调用jspInit()方法. 如果您需要执行自定义的JSP初始化任务，复写jspInit()方法就行了，就像下面这样：

```
public void jspInit(){
  // 初始化代码
}
```

‍

### JSP执行

这一阶段描述了JSP生命周期中一切与请求相关的交互行为，直到被销毁.

当JSP网页完成初始化后，JSP引擎将会调用_jspService()方法.

_jspService()方法需要一个HttpServletRequest对象和一个HttpServletResponse对象作为它的参数，就像下面这样：

```
void _jspService(HttpServletRequest request,
                 HttpServletResponse response)
{
   // 服务端处理代码
}
```

_jspService()方法在每个request中被调用一次并且负责产生与之相对应的response，并且它还负责产生所有7个HTTP方法的回应，比如GET、POST、DELETE等等.

‍

### JSP清理

JSP生命周期的销毁阶段描述了当一个JSP网页从容器中被移除时所发生的一切.

jspDestroy()方法在JSP中等价于servlet中的销毁方法. 当您需要执行任何清理工作时复写jspDestroy()方法，比如释放数据库连接或者关闭文件夹等等.

jspDestroy()方法的格式如下：

```
public void jspDestroy()
{
   // 清理代码
}
```

‍

## 指令

JSP指令用来设置整个JSP页面相关的属性，如网页的编码方式和脚本语言.

语法格式如下：

```
<%@ directive attribute="value" %>
```

‍

指令可以有很多个属性，它们以键值对的形式存在，并用逗号隔开.

JSP中的三种指令标签：

|**指令**|**描述**|
| --------------------| ---------------------------------------------------------|
|<%@ page ... %>|定义网页依赖属性，比如脚本语言、error页面、缓存需求等等|
|<%@ include ... %>|包含其他文件|
|<%@ taglib ... %>|引入标签库的定义|

‍

### Page指令

‍

‍

‍

> 所有的JSP程序是以“解释型”的方式进行运行的，并且所有的JSP程序最终都会自动的转为“*.java”源代码，但是在进行

这种转换的过程之中，就需要对转换后的代码(Java源程序，因为有了Java源程序才可以进行程序的自动编译)进行一些相应的属性配置，才可以实现最终所需要的目的，而这样的操作就通过page指令来完成.

如果要是按照严格一点来讲，所谓的page实际上描述的就是当前JSP的对象(就类似于Java中出现的Object是相同的)，  
 相当于做了一些公共的要求.

page指令主要定义了一个JSP页面中的相关属性内容，例如: import导入系统包、MIME配置、错误页、IO缓冲区配置等

<%@ page属性名称1=属性内容1属性名称2=属性内容2...%>

需要注意的是，page指令在使用的时候实际上也需要采用到“Scriptlet”给出的符号  
 结构，但是需要明确说明的是，里面有  
 一个“@”符号，而后才可以编写paae指令  
 最后跟上此指令所需要的一系列环境属性.

页面编码, 重复设置不一样会报错

‍

‍

```html
<%@ page pageEncoding="UTF-8" %>
```

Page指令为容器提供当前页面的使用说明. 一个JSP页面可以包含多个page指令.

Page指令的语法格式：

```
<%@ page attribute="value" %>
```

等价的XML格式：

```
<jsp:directive.page attribute="value" />
```

‍

‍

‍

‍

#### 属性

下表列出与Page指令相关的属性：

|**属性**|**描述**|
| --------------------| -----------------------------------------------------|
|buffer|指定out对象使用缓冲区的大小|
|autoFlush|控制out对象的 缓存区|
|contentType|指定当前JSP页面的MIME类型和字符编码|
|errorPage|指定当JSP页面发生异常时需要转向的错误处理页面|
|isErrorPage|指定当前页面是否可以作为另一个JSP页面的错误处理页面|
|extends|指定servlet从哪一个类继承|
|import|导入要使用的Java类|
|info|定义JSP页面的描述信息|
|isThreadSafe|指定对JSP页面的访问是否为线程安全|
|language|定义JSP页面所用的脚本语言，默认是Java|
|session|指定JSP页面是否使用session|
|isELIgnored|指定是否执行EL表达式|
|isScriptingEnabled|确定脚本元素能否被使用|

---

### Include指令

JSP可以通过include指令来包含其他文件. 被包含的文件可以是JSP文件、HTML文件或文本文件. 包含的文件就好像是该JSP文件的一部分，会被同时编译执行.

Include指令的语法格式如下：

```
<%@ include file="文件相对 url 地址" %>
```

**include** 指令中的文件名实际上是一个相对的 URL 地址.

如果您没有给文件关联一个路径，JSP编译器默认在当前路径下寻找.

等价的XML语法：

```
<jsp:directive.include file="文件相对 url 地址" />
```

---

### Taglib指令

JSP API允许用户自定义标签，一个自定义标签库就是自定义标签的集合.

Taglib指令引入一个自定义标签集合的定义，包括库路径、自定义标签.

Taglib指令的语法：

```
<%@ taglib uri="uri" prefix="prefixOfTag" %>
```

uri属性确定标签库的位置，prefix属性指定标签库的前缀.

等价的XML语法：

```
<jsp:directive.taglib uri="uri" prefix="prefixOfTag" />
```

‍

‍

‍

## 语法

‍

### 注释

‍

·显式注释:是直接使用HTML风格的注释，因为JSP的程序是在 HTML代码之中追加有Java程序代码;  
·隐式注释:指的是使用Java注释内容，所有的Java程序代码都是可以直接使用Java注释语法完成的，例如:Java中的单行注释、多行注释、文档注释等等;

对于两种注释而言，所有的显式注释会发送到客户端浏览器中进行暴露，而所有的隐式注释，是不会发送到客户端浏览器中，

‍

‍

**注释示例**

* ​`<%--吼吼吼, 你看得到吗--%>`​  这个是IDEA提供的<kbd>Ctrl+/</kbd>​的注释形式, 是JSP扩展的注释形式, 也是隐式的
* ​`<!--吼吼吼, 你看得到吗--> `​  这个是原生HTML注释形式

‍

‍

### forward跳转指令

‍

为了便于代码的管理，往往会按照不同的功能创建所需要的JSP页面，而后当某些程序逻辑处理完成后，希望其可以根据结果跳转到不同的页面进行显示，这样就需要使用到forward跳转指令，该指令为标签指令形式，提供有如下两类操作形式

‍

形式一：导入页面时不传递任何参数

```java
<jsp:forward page="目标文件路径"/>
```

‍

‍

形式二：导入页面同时传递参数

```java
<jsp:forward page="目标文件路径">
	    <jsp:param name="参数名称" value="参数内容"/>
	    <jsp:param name="参数名称" value="参数内容"/>
		    ... ... ...
</jsp:forward>
```

‍

body中的示例

```java
if (message.contains("yootk")) {  
%>
        <jsp:forward page="param.jsp">
            <jsp:param name="title" value="好好好"/>
            <jsp:param name="url" value="<%=message%>"/>
        </jsp:forward>
<%
    } else { 
```

‍

使用标签指令的时候，一定是要把代码写在Scriptlet之外，而且通过“<%%>”来进行结构上的区分，此时如果要是跳转，  
则会传递两个参数的内容.

路径并没有随着跳转而发生改变，相当于页面内容变为了param.jsp定义的代码，而访问路径还是之前的forward.jsp    的路径，所以这样的跳转在JavaWEB中属于服务器端跳转.

‍

‍

#### page属性

‍

在每一个JSP文件中都可能会根据需要实例化若干个对象，默认情况下这些对象都只能够在当前的页面中进行范围，但是在实际开发中，还有可能通过其他的途径在Java程序中进行某些对象的定义，为了方便这些对象的传递与状态保持，所以提供了page属性范围

‍

```html
<%@ page pageEncoding="UTF-8"%> 				<%-- 定义页面中文显示编码 --%>
<%  // page属性范围要通过pageContext内置对象来进行设置
    pageContext.setAttribute("message", "www.yootk.com"); 	// 属性设置
    pageContext.setAttribute("score", 150); 		// 属性设置
%>
<%  // 本页面可以获取到设置的page属性，获取时需要进行强制性对象向下转型
    String messageValue = (String) pageContext.getAttribute("message"); 	// 获取属性
    // 为了避免属性获取不到所造成的NullPointerException，此处暂时不进行拆箱操作
    Integer scoreValue = (Integer) pageContext.getAttribute("score"); 	// 获取属性 %>
<h1>【获取page属性】message = <%=messageValue%></h1>  	<%-- 输出属性内容 --%>
<h1>【获取page属性】score = <%=scoreValue%></h1>  		<%-- 输出属性内容 --%>

```

‍

‍

#### request属性

‍

request属性范围是在JavaWEB开发中最为常见的一种操作形式，其最大的特点是可以在一次服务器端跳转后依然可以获取所设置的属性内容(客户端不行)

‍

‍

范例：设置request属性并跳转（页面名称：scope\_a.jsp）

```html
<% 
    request.setAttribute("message", "www.yootk.com");   	// 属性设置
    request.setAttribute("score", 150); 			// 属性设置
%>
<jsp:forward page="scope_b.jsp"/>   			<%-- 服务器端跳转 --%>

```

‍

范例：获取request属性并再次跳转（页面名称：scope\_b.jsp）

```html
<%
    String messageValue = (String) request.getAttribute("message"); 	// 获取属性
    Integer scoreValue = (Integer) request.getAttribute("score"); 	// 获取属性
%>
<h1>【获取request属性】message = <%=messageValue%></h1>  	<%-- 输出属性内容 --%>
<h1>【获取request属性】score = <%=scoreValue%></h1>  	<%-- 输出属性内容 --%>
<h1><a href="scope_c.jsp">显示属性内容</a></h1>   		<%-- 客户端跳转 --%>
```

‍

范例：再次获取request属性（页面名称：scope\_c.jsp）

```html
<%
    String messageValue = (String) request.getAttribute("message"); 	// 获取属性
    Integer scoreValue = (Integer) request.getAttribute("score"); 	// 获取属性
%>
<h1>【获取request属性】message = <%=messageValue%></h1>  	<%-- 输出属性内容 --%>
<h1>【获取request属性】score = <%=scoreValue%></h1>  	<%-- 输出属性内容 --%>
```

‍

"scope_a.jsp”跳转到“scope_b.jsp”采用的是服务器端跳转操作，而“scope_b.jsp"跳转到“scope_c.jsp”采用的是超  
链接的模式(可以发现浏览器的地址已经发生了改变)

‍

‍

#### session属性范围

在JSP中页面的跳转属于常规性的操作，如果现在某些属性希望可以在任意的页面跳转之后还可以继续使用，那么就可以通过session属性来完成，session属性的特点在于，只要设置了属性并且没有关闭页面的情况下，该属性可以持续有效

(全部打通)

request相比较page属性范围，解决了无法在服务器端跳转后的属性传递问题，但是会有客户  
端跳转无法传递的问题.  
session解决了客户端跳转属性范围的传递问题.

‍

‍

‍

设置session属性并跳转（页面名称：scope_a.jsp）

```html
<%@ page pageEncoding="UTF-8"%> 				<%-- 定义页面中文显示编码 --%>
<%
    session.setAttribute("message", "www.yootk.com");   	// 属性设置
    session.setAttribute("score", 150); 			// 属性设置
%>
<jsp:forward page="scope_b.jsp"/>   			<%-- 服务器端跳转 --%>

```

‍

获取session属性并跳转（页面名称：scope_b.jsp）

```html
<%@ page pageEncoding="UTF-8"%> 				<%-- 定义页面中文显示编码 --%>
<%
    String messageValue = (String) session.getAttribute("message"); 	// 获取属性
    Integer scoreValue = (Integer) session.getAttribute("score"); 	// 获取属性
%>
<h1>【获取session属性】message = <%=messageValue%></h1>  	<%-- 输出属性内容 --%>
<h1>【获取session属性】score = <%=scoreValue%></h1>  	<%-- 输出属性内容 --%>
<h1><a href="scope_c.jsp">显示属性内容</a></h1>   		<%-- 客户端跳转 --%>
```

‍

获取session属性（页面名称：scope\_c.jsp）

```html
<%@ page pageEncoding="UTF-8"%> 			<%-- 定义页面中文显示编码 --%>
<%
    String messageValue = (String) session.getAttribute("message"); 	// 获取属性
    Integer scoreValue = (Integer) session.getAttribute("score"); 	// 获取属性
%>
<h1>【获取session属性】message = <%=messageValue%></h1>  	<%-- 输出属性内容 --%>
<h1>【获取session属性】score = <%=scoreValue%></h1>  	<%-- 输出属性内容 --%>

```

‍

‍

‍

#### application属性

application属性可以实现全局属性的配置，即：所有的属性保存在服务器之中，不管用户是否关闭页面，都可以获取到设置的application属性，只有在WEB容器重启后此属性才会消失

‍

设置application属性（页面名称：scope_a.jsp）

‍

```html
<%@ page pageEncoding="UTF-8"%> 			<%-- 定义页面中文显示编码 --%>
<%
    String messageValue = (String) session.getAttribute("message"); 	// 获取属性
    Integer scoreValue = (Integer) session.getAttribute("score"); 	// 获取属性
%>
<h1>【获取session属性】message = <%=messageValue%></h1>  	<%-- 输出属性内容 --%>
<h1>【获取session属性】score = <%=scoreValue%></h1>  	<%-- 输出属性内容 --%>

```

‍

获取application属性（页面名称：scope_b.jsp）

```html
<%@ page pageEncoding="UTF-8"%> 			<%-- 定义页面中文显示编码 --%>
<%
    String messageValue = (String) application.getAttribute("message"); // 获取属性
    Integer scoreValue = (Integer) application.getAttribute("score"); // 获取属性
%>
<h1>【获取application属性】message = <%=messageValue%></h1><%-- 输出属性内容 --%>
<h1>【获取application属性】score = <%=scoreValue%></h1>  <%-- 输出属性内容 --%>

```

但是如果说现在将当前的Tomcat关闭了，那么对应的程序所设置的application属性就无法保存了.

‍

当重新启动之后已经无法获取到application属性设置的信息了，所以application属性是保存在了  
服务器上，但是经过了一  
系列的属性操作之后，也需要注意一个问题:属性范围保存的越久，所占用的内存时间也就会越久，如果

在application属性中保  
了过多的信息，那么这个信息对象是有可能会被持续保留的，那么最终就有可能由于保存的内容过多，而  
j造成服务器的性能下降.

‍

‍

#### pagecontext属性操作

‍

整个的JavaWEB技术核心全部都是围绕着四种属性范围展开的，可以这么说，如果你连属性范围都无法理解透彻(不会使用  
这些属性范围进行开发，以及属性范围的特点)，未来的很多的学习都会出现问题. 在之前学习过了pageContext内置对象可以实现page属性范围的定义，但是实际上如果认真观察pageContext对应的类型(jakarta.servlet.jsp.PageContext)中所提供的方法，你会发现它可以实现全部四种属性范围的操作. “

‍

在pageContext类中提供了更加全面的属性操作支持方法，可以实现全部四种属性范围的控制

|No.|常量与方法|类型|描述|
| -----| -----------------------------------------------------------------| ------| -------------------------|
|1|public static final int  PAGE_SCOPE|常量|page属性操作标记|
|2|public static final int  REQUEST_SCOPE|常量|request属性操作标记|
|3|public static final int  SESSION_SCOPE|常量|session属性操作标记|
|4|public static final int  APPLICATION_SCOPE|常量|application属性操作标记|
|5|public void setAttribute(String  name, Object value, int scope)|方法|设置指定范围的属性|
|6|public Object getAttribute(String  name, int scope)|方法|获取指定范围的属性|
|7|public Object  findAttribute(String name)|方法|发现任意属性范围的属性|
|8|public void  removeAttribute(String name, int scope)|方法|删除指定范围的属性|

‍

‍

设置不同范围的属性（页面名称：scope_a.jsp）

服务器端跳转

```html
<% // 通过pageContext对象实现不同范围的属性设置
    pageContext.setAttribute("message", "www.yootk.com", PageContext.REQUEST_SCOPE);
    pageContext.setAttribute("score", 150, PageContext.SESSION_SCOPE);
%>
<jsp:forward page="scope_b.jsp"/>    
```

‍

获取属性内容（页面名称：scope_b.jsp）

```html
<% // 通过pageContext对象实现不同范围的属性获取
    String messageValue = (String) pageContext.getAttribute("message",                                                   PageContext.REQUEST_SCOPE); 
    Integer scoreValue = (Integer) pageContext.findAttribute("score"); 
%>
<h1>【获取request属性】message = <%=messageValue%></h1>	<%-- 输出属性内容 --%>
<h1>【获取session属性】score = <%=scoreValue%></h1>  	<%-- 输出属性内容 --%>
```

‍

通过当前的执行过程，可以确定的事情就是pageContext是一个非常全能的对象，可以实现四个属  
性范围的全部定义，但是  
考虑到代码的清晰性，建议还是使用各自不同的对象实现属性的设置与获取.

‍

‍

‍

### 脚本程序

脚本程序可以包含任意量的Java语句、变量、方法或表达式，只要它们在脚本语言中是有效的.

脚本程序的语法格式：

```
<% 代码片段 %>
```

或者，您也可以编写与其等价的XML语句，就像下面这样：

```
<jsp:scriptlet>
   代码片段
</jsp:scriptlet>
```

‍

要在页面正常显示中文，我们需要在 JSP 文件头部添加以下代码：

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
```

‍

一个声明语句可以声明一个或多个变量、方法，供后面的Java代码使用. 在JSP文件中，您必须先声明这些变量和方法然后才能使用它们.

JSP声明的语法格式：

```
<%! declaration; [ declaration; ]+ ... %>
```

或者，您也可以编写与其等价的XML语句，就像下面这样：

```
<jsp:declaration>
   代码片段
</jsp:declaration>
```

‍

### JSP表达式

一个JSP表达式中包含的脚本语言表达式，先被转化成String，然后插入到表达式出现的地方.

由于表达式的值会被转化成String，所以您可以在一个文本行中使用表达式而不用去管它是否是HTML标签.

表达式元素中可以包含任何符合Java语言规范的表达式，但是不能使用分号来结束表达式.

JSP表达式的语法格式：

```
<%= 表达式 %>
```

同样，您也可以编写与之等价的XML语句：

```
<jsp:expression>
   表达式
</jsp:expression>
```

‍

### 注释

|**语法**|描述|
| ----------------| ------------------------------------------------------|
|<%-- 注释 --%>|JSP注释，注释内容不会被发送至浏览器甚至不会被编译|
|<!-- 注释 -->|HTML注释，通过浏览器查看网页源代码时可以看见注释内容|
|<\%|代表静态 <%常量|
|%\>|代表静态 %> 常量|
|\'|在属性中使用的单引号|
|\"|在属性中使用的双引号|

‍

### 指令

JSP指令用来设置与整个JSP页面相关的属性.

JSP指令语法格式：

```
<%@ directive attribute="value" %>
```

‍

|**指令**|**描述**|
| --------------------| -----------------------------------------------------------|
|<%@ page ... %>|定义页面的依赖属性，比如脚本语言、error页面、缓存需求等等|
|<%@ include ... %>|包含其他文件|
|<%@ taglib ... %>|引入标签库的定义，可以是自定义标签|

‍

### JSP行为

JSP行为标签使用XML语法结构来控制servlet引擎. 它能够动态插入一个文件，重用JavaBean组件，引导用户去另一个页面，为Java插件产生相关的HTML等等.

行为标签只有一种语法格式，它严格遵守XML标准：

```
<jsp:action_name attribute="value" />
```

‍

‍

|**语法**|**描述**|
| -----------------| ------------------------------------------------------------|
|jsp:include|用于在当前页面中包含静态或动态资源|
|jsp:useBean|寻找和初始化一个JavaBean组件|
|jsp:setProperty|设置 JavaBean组件的值|
|jsp:getProperty|将 JavaBean组件的值插入到 output中|
|jsp:forward|从一个JSP文件向另一个文件传递一个包含用户请求的request对象|
|jsp:plugin|用于在生成的HTML页面中包含Applet和JavaBean对象|
|jsp:element|动态创建一个XML元素|
|jsp:attribute|定义动态创建的XML元素的属性|
|jsp:body|定义动态创建的XML元素的主体|
|jsp:text|用于封装模板数据|

‍

### 隐含对象

JSP 支持九个自动定义的变量，江湖人称隐含对象，它们是在 JSP 页面中自动可用的对象，无需额外的声明或初始化.

‍

‍

|**对象**|**描述**|
| -------------| ----------------------------------------------------------------------------------------------------------------------|
|request|**HttpServletRequest**类的实例，代表 HTTP 请求的对象，包含客户端发送到服务器的信息，如表单数据、URL参数等.|
|response|**HttpServletResponse**类的实例，代表 HTTP 响应的对象，用于向客户端发送数据和响应.|
|out|**JspWriter**类的实例，用于向客户端输出文本内容的对象，通常用于生成HTML.|
|session|**HttpSession**类的实例，代表用户会话的对象，可用于存储和检索用户特定的数据，跨多个页面.|
|application|**ServletContext**类的实例，代表 Web 应用程序的上下文，可以用于存储和检索全局应用程序数据.|
|config|**ServletConfig**类的实例，包含有关当前 JSP 页面的配置信息，例如初始化参数.|
|pageContext|**PageContext**类的实例，提供对JSP页面所有对象以及命名空间的访问|
|page|类似于 Java 类中的 this 关键字，代表当前 JSP 页面的实例，可以用于调用页面的方法.|
|exception|**exception** 类的对象，代表发生错误的 JSP 页面中对应的异常对象，用于处理 JSP 页面中的异常情况，可用于捕获和处理页面中发生的异常.|

‍

‍

### 控制流语句

JSP提供对Java语言的全面支持. 您可以在JSP程序中使用Java API甚至建立Java代码块，包括判断语句和循环语句等等.

‍

#### 判断语句

**if…else** 块

```java
<body>
<h3>IF...ELSE 实例</h3>
<% if (day == 1 || day == 7) { %>
      <p>今天是周末</p>
<% } else { %>
      <p>今天不是周末</p>
<% } %>
</body> 
```

‍

switch…case块

```java
<body>
<h3>SWITCH...CASE 实例</h3>
<% 
switch(day) {
case 0:
   out.println("星期天");
   break;
case 1:
   out.println("星期一");
   break;
case 2:
   out.println("星期二");
   break;
case 3:
   out.println("星期三");
   break;
case 4:
   out.println("星期四");
   break;
case 5:
   out.println("星期五");
   break;
default:
   out.println("星期六");
}
%>
</body>
```

‍

‍

#### 循环语句

‍

for循环

```java
<body>
<h3>For 循环实例</h3>
<%for ( fontSize = 1; fontSize <= 3; fontSize++){ %>
   <font color="green" size="<%= fontSize %>">
    菜鸟教程
   </font><br />
<%}%>
</body> 
```

‍

while 循环

```java
<body>
<h3>While 循环实例</h3>
<%while ( fontSize <= 3){ %>
   <font color="green" size="<%= fontSize %>">
    菜鸟教程
   </font><br />
<%fontSize++;%>
<%}%>
</body> 
```

‍

JSP常见运算符，优先级从高到低

|**类别**|**操作符**|**结合性**|
| -----------| --------------------------------| --------|
|后缀|() [] . (点运算符)|左到右|
|一元|++ - - ! ~|右到左|
|可乘性|* / %|左到右|
|可加性|+ -|左到右|
|移位|>> >>> <<|左到右|
|关系|> >= < <=|左到右|
|相等/不等|== !=|左到右|
|位与|&|左到右|
|位异或|^|左到右|
|位或|||
|逻辑与|&&|左到右|
|逻辑或|||
|条件判断|?:|右到左|
|赋值|= += -= *= /= %= >>= <<= &= ^=|=|
|逗号|,|左到右|

‍

### 字面量

JSP语言定义了以下几个字面量：

* 布尔值(boolean)：true 和 false;
* 整型(int)：与 Java 中的一样;
* 浮点型(float)：与 Java 中的一样;
* 字符串(string)：以单引号或双引号开始和结束;
* Null：null.

‍

‍

‍

## 页面设计

‍

### 错误页面

‍

统一跳转到一个页面中进行错误显示

‍

在JSP页面中错误页的配置主要通过page指令中的“errorPage”和“isErrorPage”两个属性来定义，其中错误显示页中的“isErrorPage”必须设置为“true”，而其他的页面则需要通过“errorPage”指定错误页的路径.

‍

编写产生异常的页面

errorPage="bugone.jsp"

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" errorPage="bugone.jsp" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<%=10 / 0%>
</body>
</html>
```

‍

处理错误页

<%=exception%>

isErrorPage="true"

```html
<%@ page pageEncoding="UTF-8" isErrorPage="true" %> 
<html>
<head>
    <title>错误页</title>
</head>
<body>
<h1>出错了</h1>
<img src="images/wrong.png">
<img src="images/logo.png" style="height: 120px;">
<hr><%=exception%> 
</body>
</html>
```

‍

页面效果

```html
出错了
---
java.lang.ArithmeticException: / by zero
```

‍

如果发现某些静态资源最终无法进行项目的部署，那么就请手工的找到项目的部署路径，而后将资源按照项目中给出的结构转移到指定位置

‍

‍

统一错误处理

有error-code错误代码和exception-type异常类型匹配方式

```html
    <error-page>
        <error-code>404</error-code>
        <location>/errors.jsp</location>
    </error-page>
    <error-page>
        <error-code>500</error-code>
        <location>/errors.jsp</location>
    </error-page>
    <error-page>
        <exception-type>java.lang.Exception</exception-type>
        <location>/errors.jsp</location>
    </error-page>
```

如果此时是进行了异常的匹配，那么在错误页中最终捕获异常就是程序中所产生的异常，而不是所谓的页面异常

‍

‍

‍

### 登录模块

‍

#### 表单模块

‍

**表单**

在表单之中定义的id主要是留给前端的JS开发操作的，而name的信息是留给请求参数接收处理的，需要注意的是，请一定要保证id和name两个属性的内容要一致

```html
<form action="<%=LOGIN_URL%>" class="form-horizontal" id="memberform" method="post">
    <fieldset>
        <legend><img src="images/user-title.png" style="width:50px;">用户登录</legend>
    </fieldset>
    <div class="form-group" id="midDiv">
        <label class="col-md-2 control-label">用户名：</label>
        <div class="col-md-7">
            <input type="text" id="mid" name="mid" class="form-control"  placeholder="请输入用户的注册ID编号。">
        </div>
        <div class="col-md-3">
            <span id="midSpan"></span>
        </div>
    </div>
    <div class="form-group" id="passwordDiv">
        <label class="col-md-2 control-label">密码：</label>
        <div class="col-md-7">
            <input type="password" id="password" name="password" class="form-control"  placeholder="请输入登录密码。">
        </div>
        <div class="col-md-3">
            <span id="passwordSpan"></span>
        </div>
    </div>
    <div class="form-group" id="controlDiv">
        <div class="col-md-push-3 col-md-3">
            <button type="submit" class="btn btn-primary btn-sm">登录</button>
            <button type="reset" class="btn btn-warning btn-sm">重置</button>
        </div>
    </div>
</form>
```

‍

**JSP**

```html
<%! // 为便于路径管理，将表单提交路径定义为全局常量
    public static final String LOGIN_URL = "check.jsp" ;
%>
```

‍

‍

‍

#### 登录检测模块

‍

用户填写完成的表单需要进行提交，而在每次提交后JSP页面都可以实现参数的接收，随后将获取到的参数内容整合到SQL查询语句之中，以实现数据库验证，为了便于方便，本次的操作暂时不进行跳转.

‍

```html
<%@ page pageEncoding="UTF-8" %>    			<%-- 定义页面中文显示编码 --%>
<%@ page import="com.yootk.dbc.DatabaseConnection" %> 	<%-- 导入数据库工具类 --%>
<%@ page import="java.sql.*" %> 				<%-- 导入sql工具包 --%>
<%  // 暂时不考虑服务器端数据的验证问题
    String mid = request.getParameter("mid") ; 		// 接收mid参数
    String password = request.getParameter("password") ; 	// 接收password参数
    Statement stmt = DatabaseConnection.getConnection().createStatement();	// 获取Statement实例
    String sql = "SELECT name FROM member WHERE mid='" + mid +  "' AND password='" + password + "'";	// 拼凑查询SQL语句
    ResultSet rs = stmt.executeQuery(sql) ; 		// 数据查询
    String name = null ; 					// 保存真实姓名信息
    if (rs.next()) {    					// 如果可以查询出数据
        name = rs.getString(1) ; 				// 获取数据库中的查询列
    }
    DatabaseConnection.close();				// 关闭数据库连接
    if (name == null) { 					// 登录失败
%> <h1>用户登录失败，错误的用户名及密码！</h1>
<% } else { %>
        <h1>用户登录成功，欢迎登录沐言科技！</h1>
<% } %>

```

‍

‍

‍

#### 登录信息显示

‍

登录页面

```html
<% String name = request.getParameter("name") ;    // 接收跳转参数 %>
<div class="row">
    <div class="col-md-12">
        <img src="images/muyan.png" style="width:100px;">
        <span class="text-success h2">用户登录成功，欢迎<%=name%>的访问！</span>
    </div>
</div>
```

‍

登录验证页面

```html
<% if (name == null) { %>
        <jsp:forward page="login.jsp">
            <jsp:param name="error" value="emsg"/>
        </jsp:forward>
<% } else { %>
        <jsp:forward page="welcome.jsp">
            <jsp:param name="name" value="<%=name%>"/>
        </jsp:forward>
<% } %>

```

‍

显示错误信息

```html
<%
    String error = request.getParameter("error") ; // 该参数会由check.jsp传递过来
    if (!(error == null || "".equals(error))) { // 判断是否存在有数据
%>

<div class="row">
    <div class="col-md-12">
        <img src="images/error.png" style="width:50px;">
        <span class="text-danger h2">登录失败，错误的用户名或密码！</span>
    </div>
</div>

<% } %>

```

‍

‍

‍

## 请求响应

‍

‍

### 客户端请求

当浏览器请求一个网页时，它会向网络服务器发送一系列不能被直接读取的信息，因为这些信息是作为HTTP信息头的一部分来传送的. 您可以查阅HTTP协议来获得更多的信息.

下表列出了浏览器端信息头的一些重要内容，在以后的网络编程中将会经常见到这些信息：

|**信息**|**描述**|
| ---------------------| ----------------------------------------------------------------------------------------------------------------------|
|Accept|指定浏览器或其他客户端可以处理的MIME类型. 它的值通常为**image/png**或**image/jpeg**|
|Accept-Charset|指定浏览器要使用的字符集. 比如 ISO-8859-1|
|Accept-Encoding|指定编码类型. 它的值通常为**gzip**或**compress**|
|Accept-Language|指定客户端首选语言，servlet会优先返回以当前语言构成的结果集，如果servlet支持这种语言的话. 比如 en，en-us，ru等等|
|Authorization|在访问受密码保护的网页时识别不同的用户|
|Connection|表明客户端是否可以处理HTTP持久连接. 持久连接允许客户端或浏览器在一个请求中获取多个文件. **Keep-Alive**表示启用持久连接|
|Content-Length|仅适用于POST请求，表示 POST 数据的字节数|
|Cookie|返回先前发送给浏览器的cookies至服务器|
|Host|指出原始URL中的主机名和端口号|
|If-Modified-Since|表明只有当网页在指定的日期被修改后客户端才需要这个网页.  服务器发送304码给客户端，表示没有更新的资源|
|If-Unmodified-Since|与If-Modified-Since相反， 只有文档在指定日期后仍未被修改过，操作才会成功|
|Referer|标志着所引用页面的URL. 比如，如果你在页面1，然后点了个链接至页面2，那么页面1的URL就会包含在浏览器请求页面2的信息头中|
|User-Agent|用来区分不同浏览器或客户端发送的请求，并对不同类型的浏览器返回不同的内容|

---

#### HttpServletRequest类

request对象是javax.servlet.http.HttpServletRequest类的实例. 每当客户端请求一个页面时，JSP引擎就会产生一个新的对象来代表这个请求.

request对象提供了一系列方法来获取HTTP信息头，包括表单数据，cookies，HTTP方法等等.

接下来将会介绍一些在JSP编程中常用的获取HTTP信息头的方法. 详细内容请见下表：

|**序号**|**方法&amp;**  **描述**|
| ----| ---------------------------------------------------------------------------------------------|
|1|**Cookie[] getCookies()** 返回客户端所有的Cookie的数组|
|2|**Enumeration getAttributeNames()** 返回request对象的所有属性名称的集合|
|3|**Enumeration getHeaderNames()** 返回所有HTTP头的名称集合|
|4|**Enumeration getParameterNames()** 返回请求中所有参数的集合|
|5|**HttpSession getSession()** 返回request对应的session对象，如果没有，则创建一个|
|6|**HttpSession getSession(boolean create)** 返回request对应的session对象，如果没有并且参数create为true，则返回一个新的session对象|
|7|**Locale getLocale()** 返回当前页的Locale对象，可以在response中设置|
|8|**Object getAttribute(String name)** 返回名称为name的属性值，如果不存在则返回null.|
|9|**ServletInputStream getInputStream()** 返回请求的输入流|
|10|**String getAuthType()** 返回认证方案的名称，用来保护servlet，比如 "BASIC" 或者 "SSL" 或 null 如果 JSP没设置保护措施|
|11|**String getCharacterEncoding()** 返回request的字符编码集名称|
|12|**String getContentType()** 返回request主体的MIME类型，若未知则返回null|
|13|**String getContextPath()** 返回request URI中指明的上下文路径|
|14|**String getHeader(String name)** 返回name指定的信息头|
|15|**String getMethod()** 返回此request中的HTTP方法，比如 GET,，POST，或PUT|
|16|**String getParameter(String name)** 返回此request中name指定的参数，若不存在则返回null|
|17|**String getPathInfo()** 返回任何额外的与此request URL相关的路径|
|18|**String getProtocol()** 返回此request所使用的协议名和版本|
|19|**String getQueryString()** 返回此 request URL包含的查询字符串|
|20|**String getRemoteAddr()** 返回客户端的IP地址|
|21|**String getRemoteHost()** 返回客户端的完整名称|
|22|**String getRemoteUser()** 返回客户端通过登录认证的用户，若用户未认证则返回null|
|23|**String getRequestURI()** 返回request的URI|
|24|**String getRequestedSessionId()** 返回request指定的session ID|
|25|**String getServletPath()** 返回所请求的servlet路径|
|26|**String[] getParameterValues(String name)** 返回指定名称的参数的所有值，若不存在则返回null|
|27|**boolean isSecure()** 返回request是否使用了加密通道，比如HTTPS|
|28|**int getContentLength()** 返回request主体所包含的字节数，若未知的返回-1|
|29|**int getIntHeader(String name)** 返回指定名称的request信息头的值|
|30|**int getServerPort()** 返回服务器端口号|

‍

#### HTTP信息头示例

在这个例子中，我们会使用HttpServletRequest类的getHeaderNames()方法来读取HTTP信息头. 这个方法以枚举的形式返回当前HTTP请求的头信息.

获取Enumeration对象后，用标准的方式来遍历Enumeration对象，用hasMoreElements()方法来确定什么时候停止，用nextElement()方法来获得每个参数的名字.

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.io.*,java.util.*" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>菜鸟教程(runoob.com)</title>
</head>
<body>
<h2>HTTP 头部请求实例</h2>
<table width="100%" border="1" align="center">
<tr bgcolor="#949494">
<th>Header Name</th><th>Header Value(s)</th>
</tr>
<%
   Enumeration headerNames = request.getHeaderNames();
   while(headerNames.hasMoreElements()) {
      String paramName = (String)headerNames.nextElement();
      out.print("<tr><td>" + paramName + "</td>\n");
      String paramValue = request.getHeader(paramName);
      out.println("<td> " + paramValue + "</td></tr>\n");
   }
%>
</table>
</body>
</html>
```

‍

‍

### 服务器响应

Response响应对象主要将JSP容器处理后的结果传回到客户端. 可以通过response变量设置HTTP的状态和向客户端发送数据，如Cookie、HTTP文件头信息等.

一个典型的响应看起来就像下面这样：

```
HTTP/1.1 200 OK
Content-Type: text/html
Header2: ...
...
HeaderN: ...
  (空行)
<!doctype ...>
<html>
<head>...</head>
<body>
...
</body>
</html>
```

状态行包含HTTP版本信息，比如HTTP/1.1，一个状态码，比如200，还有一个非常短的信息对应着状态码，比如OK.

下表摘要出了HTTP1.1响应头中最有用的部分，在网络编程中您将会经常见到它们：

|**响应头**|**描述**|
| ---------------------| ----------------------------------------------------------------------------------------------------------------------------------------------------------------|
|Allow|指定服务器支持的request方法（GET，POST等等）|
|Cache-Control|指定响应文档能够被安全缓存的情况. 通常取值为**public，private**或**no-cache**等等.  Public意味着文档可缓存，Private意味着文档只为单用户服务并且只能使用私有缓存. No-cache 意味着文档不被缓存.|
|Connection|命令浏览器是否要使用持久的HTTP连接. **close值**命令浏览器不使用持久HTTP连接，而keep-alive 意味着使用持久化连接.|
|Content-Disposition|让浏览器要求用户将响应以给定的名称存储在磁盘中|
|Content-Encoding|指定传输时页面的编码规则|
|Content-Language|表述文档所使用的语言，比如en， en-us,，ru等等|
|Content-Length|表明响应的字节数. 只有在浏览器使用持久化 (keep-alive) HTTP 连接时才有用|
|Content-Type|表明文档使用的MIME类型|
|Expires|指明啥时候过期并从缓存中移除|
|Last-Modified|指明文档最后修改时间. 客户端可以 缓存文档并且在后续的请求中提供一个**If-Modified-Since**请求头|
|Location|在300秒内，包含所有的有一个状态码的响应地址，浏览器会自动重连然后检索新文档|
|Refresh|指明浏览器每隔多久请求更新一次页面.|
|Retry-After|与503 (Service Unavailable)一起使用来告诉用户多久后请求将会得到响应|
|Set-Cookie|指明当前页面对应的cookie|

---

#### HttpServletResponse类

response 对象是 javax.servlet.http.HttpServletResponse 类的一个实例. 就像服务器会创建request对象一样，它也会创建一个客户端响应.

response对象定义了处理创建HTTP信息头的接口. 通过使用这个对象，开发者们可以添加新的cookie或时间戳，还有HTTP状态码等等.

下表列出了用来设置HTTP响应头的方法，这些方法由HttpServletResponse 类提供：

|**S.N.**|**方法**  **&amp;**  **描述**|
| ----| --------------------------------------------------------------------------------|
|1|**String encodeRedirectURL(String url)** 对sendRedirect()方法使用的URL进行编码|
|2|**String encodeURL(String url)** 将URL编码，回传包含Session ID的URL|
|3|**boolean containsHeader(String name)** 返回指定的响应头是否存在|
|4|**boolean isCommitted()** 返回响应是否已经提交到客户端|
|5|**void addCookie(Cookie cookie)** 添加指定的cookie至响应中|
|6|**void addDateHeader(String name, long date)** 添加指定名称的响应头和日期值|
|7|**void addHeader(String name, String value)** 添加指定名称的响应头和值|
|8|**void addIntHeader(String name, int value)** 添加指定名称的响应头和int值|
|9|**void flushBuffer()** 将任何缓存中的内容写入客户端|
|10|**void reset()** 清除任何缓存中的任何数据，包括状态码和各种响应头|
|11|**void resetBuffer()** 清除基本的缓存数据，不包括响应头和状态码|
|12|**void sendError(int sc)** 使用指定的状态码向客户端发送一个出错响应，然后清除缓存|
|13|**void sendError(int sc, String msg)** 使用指定的状态码和消息向客户端发送一个出错响应|
|14|**void sendRedirect(String location)** 使用指定的URL向客户端发送一个临时的间接响应|
|15|**void setBufferSize(int size)** 设置响应体的缓存区大小|
|16|**void setCharacterEncoding(String charset)** 指定响应的编码集（MIME字符集），例如UTF-8|
|17|**void setContentLength(int len)** 指定HTTP servlets中响应的内容的长度，此方法用来设置 HTTP Content-Length 信息头|
|18|**void setContentType(String type)** 设置响应的内容的类型，如果响应还未被提交的话|
|19|**void setDateHeader(String name, long date)** 使用指定名称和日期设置响应头的名称和日期|
|20|**void setHeader(String name, String value)** 使用指定名称和值设置响应头的名称和内容|
|21|**void setIntHeader(String name, int value)** 指定 int 类型的值到 name 标头|
|22|**void setLocale(Locale loc)** 设置响应的语言环境，如果响应尚未被提交的话|
|23|**void setStatus(int sc)** 设置响应的状态码|

---

‍

#### HTTP响应头程序示例

接下来的例子使用setIntHeader()方法和setRefreshHeader()方法来模拟一个数字时钟：

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.io.*,java.util.*" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>菜鸟教程(runoob.com)</title>
</head>
<body>
<h2>自动刷新实例</h2>
<%
   // 设置每隔5秒自动刷新
   response.setIntHeader("Refresh", 5);
   // 获取当前时间
   Calendar calendar = new GregorianCalendar();
   String am_pm;
   int hour = calendar.get(Calendar.HOUR);
   int minute = calendar.get(Calendar.MINUTE);
   int second = calendar.get(Calendar.SECOND);
   if(calendar.get(Calendar.AM_PM) == 0)
      am_pm = "AM";
   else
      am_pm = "PM";
   String CT = hour+":"+ minute +":"+ second +" "+ am_pm;
   out.println("当前时间: " + CT + "\n");
%>
</body>
</html>
```

它将会每隔5秒显示一下系统当前时间.

‍

‍

‍

‍

## **隐式对象**

‍

JSP隐式对象是JSP容器为每个页面提供的Java对象，开发者可以直接使用它们而不用显式声明. JSP隐式对象也被称为预定义变量.

JSP所支持的九大隐式对象：

|**对象**|**描述**|
| -------------| ---------------------------------------------------|
|request|**HttpServletRequest** 接口的实例|
|response|**HttpServletResponse** 接口的实例|
|out|**JspWriter**类的实例，用于把结果输出至网页上|
|session|**HttpSession**类的实例|
|application|**ServletContext**类的实例，与应用上下文有关|
|config|**ServletConfig**类的实例|
|pageContext|**PageContext**类的实例，提供对JSP页面所有对象以及命名空间的访问|
|page|类似于Java类中的this关键字|
|Exception|**Exception**类的对象，代表发生错误的JSP页面中对应的异常对象|

---

### request对象

request对象是javax.servlet.http.HttpServletRequest 类的实例. 每当客户端请求一个JSP页面时，JSP引擎就会制造一个新的request对象来代表这个请求.

request对象提供了一系列方法来获取HTTP头信息，cookies，HTTP方法等等.

‍

JSP程序的开发核心为请求（request）与响应（response），用户所需要的所有数据都会封装在HTTP请求之中，在服务器端程序如果要想获取这些请求数据，就可以通过request内置对象完成，request对象对应的类型为javax.servlet.http包中的HttpServletRequest接口

‍

#### 接收单个请求参数

HttpServletRequest接口的核心作用在于接收用户的请求，所有可以接收的用户请求一定都包含有参数名称，而请求参数的来源可以是表单填写或者是通过URL地址重写的方式进行传递，但是不管如何传递只要是参数接收都采用如下的方法进行统一处理：

public String getParameter(String paramName)

‍

定义基础表单（页面名称：input.html）

> 对于当前的这个页面实际上是可以定义为“input.jsp”文件，之所以没有将其定义为JSP程序主要是因为这个页面里面不存在有程序代码，是一个纯粹的由 HTML 代码所组成的文件，但是如果现在将将这样的代码定义为JSP，则在进行页面处理的时候就需要找到 WEB容器，而如果定义的是HTML，那么往很明显执行的速度会快很多. .

表单一般是Post提交

```html
<html>
<head>
 <title>www.yootk.com</title>
<meta charset="UTF-8"/>
</head>
<body>
<form action="input_do.jsp" method="post">			<!-- 表单一般都是post提交 -->
    请输入信息：<input type="text" name="msg" value="沐言科技">	<!-- 文本框 -->
    <input type="hidden" name="url" value="www.yootk.com">	<!-- 隐藏域 -->
    <input type="submit" value="发送">     			<!-- 提交按钮 -->
</form>
</body>
</html>

```

‍

接收表单参数（页面名称：input_do.jsp）

```html
<%
    String msg = request.getParameter("msg") ; 	// 接收请求参数
    String url = request.getParameter("url") ; 	// 接收请求参数
%>
<h1><%=msg%>：<%=url%></h1> 			<%-- 输出参数内容 --%>

```

‍

‍

在实际的信息输入的环节下，一般都可能会存在有数字信息，但是对于getParameter()  
来讲肯定接收到的数据类型为字符串,  
那么此时就需要进行数据的转换处理，下面可以定义一个下拉列表框, 然后编写jsp

‍

（页面名称：input_do.jsp）

使用Java对应包装类的转换方法即可

```html
<body>
<%
    String msg = request.getParameter("msg"); 
    String url = request.getParameter("url");
    int level = Integer.parseInt(request.getParameter("level")); //接受参数并转型
    String levelContent = null;
    switch(level) {
        case 0:
            levelContent = "A";
            break;
        case 1:
            levelContent = "B";
            break;
        case 2:
            levelContent = "C";
            break;
        case 3:
            levelContent = "D";
            break;
    }
%>
<h1><%=msg%>, <%=url%></h1>
<h1>Level<%=levelContent%></h1>
</body>

```

‍

‍

改了表单提交模式为GET，最终在执行的时候可以发现此时的目标路径已经发生了更改

通过这个时候的执行结果可以非常清楚的发现，所有对应的表单中输入的数据信息已经全部的以明文的方式显示在客户端浏览器地址

‍

清楚了以上的操作之后，下面就可以直接总结出GET 与POST提交模式的区别:

·GET模式:只要用户通过浏览器输入了访问地址并且执行，那么就都表示向服务  
器端发送了一个GET请求，如果在表  
单上发出了GET请求则所有的请求数据会直接附加在地址栏之中，但是地址栏不是无限长的  
(—般来讲只能够输入4K~5K的数  
据内容，如果要进行文件上传，那么一定无法使用GET模式)

. POST模式:只能够用于表单提交上，所有提交后的表单为目标路径（请求参数不  
会附加显示上)，理论上可以传递很  
大的提交内容(视频、图片等)，但是如果传递的文件太大，也有可能会因为网络的问题而造  
成上传失败.  
既然已经通过当前的代码发现了GET请求和POST请求的区别，实际上就可以通过GET  
模式采用地址重写的方式进行参数发送

‍

‍

‍

‍

地址重写

‍

在进行参数传递时，除了通过表单之外，也可以采用地址重写的方式进行请求参数的传递，在定义访问目标路径时，就必须采用如下的格式进行定义：

目标路径.jsp?参数名称1=参数内容&参数名称2=参数内容&...

‍

提交地址和参数之间使用了“?”进行分割，而每一个参数之间使用“&”进行分割，下面不再通过表单进行参数的提交，而  
是直接通过地址重写的方式进行表单参数的提交.

‍

**定义超链接传递参数**

​`<a href="input_do.jsp?msg=沐言科技&url=www.yootk.com">参数显示</a>`​

此时实现了同样的参数的接收，但是需要注意的是，如果在使用这种模式的情况下，没有设置参数和设置的参数内容为空字符串两个概念.

如果是一个普通的数据输出，没有接收到数据直接返回null是没有什么问题，也就是说显示难看一点，但是如果涉及数据类型转换处理上，如果没有传递数据，后果很可怕. (格式化异常)

解决: 设置默认值, 加上try-catch判断

```html
int level = 99; //初始值
    try {
        Integer.parseInt(request.getParameter("level")); 
    } catch (Exception e) {}
    String levelContent = null;
```

‍

‍

‍

‍

#### 乱码处理

设置请求编码

在WEB请求中，所有的数据都是以二进制的形式进行传输的，这样就需要对所发送的数据进行编码与解码处理，但是如果说此时传递的是中文数据，并且编码和解码不同，就有可能造成乱码问题，在实际的开发中一般都会使用“UTF-8”编码，而此时就可以利用request对象的如下方法实现请求编码设置：

public void setCharacterEncoding(String env) throws UnsupportedEncodingException

‍

你只要在进行参数接收前设置了正确的编码，那么就可以直接接收到中文数据内容了，而且现在  
在90%的项目之中几乎都是  
使用的UTF-8编码.

‍

##### 接收参数并设置编码

```html
<%@ page pageEncoding="UTF-8"%> 	<%-- 定义页面中文显示编码 --%>
<%
    request.setCharacterEncoding("UTF-8"); 		// 设置编码
    String msg = request.getParameter("msg") ; 	// 接收请求参数
    String url = request.getParameter("url") ; 	// 接收请求参数
%>

```

‍

实际上在最早期开发JavaWEB 的时候乱码问题让我们很头疼（Tomcat版本也是在不断更新完善的)，所以早期如果要想  
传递中文，一般都需要进行地址编码和解码操作.

```java
public class TestCode {
    public static void main(String[] args) {
        String message = "????www.yootk.com";
        String encodeValue = URLEncoder.encode(message, Charset.forName("UTF-8")); // 编码操作
        System.out.println("编码结果" + encodeValue);
        String value = URLDecoder.decode(encodeValue, Charset.forName("UTF-8")); // 解码操作
        System.out.println("解码结果" + value);
    }
}
```

‍

#### 数组请求参数

‍

请求参数除了可以实现单个参数的接收之外，也可以实现一组参数的接收，此时就要求传递的若干个参数内容采用相同的参数名称，而对于数组参数的接收则可以使用以下方法完成：

public String[] getParameterValues(String name)

这个时候可以清楚的发现，此时可以直接返回有一个数组内容，所有的参数都可以通过getValues()方法进行接收处理，但是如果某些参数只有一个，则数组长度就是1，意味着“getParameterValues()”方法可以代替"getParameter()”方法.

‍

复选框表单（页面名称：input.html）

```html
<form action="input_do.jsp" method="post">
    请输入信息：<input type="text" name="msg" value="沐言科技"><br/>
    消息接收者：<input type="checkbox" name="receiver" value="muyan">沐言科技
                <input type="checkbox" name="receiver" value="yootk">沐言优拓
                <input type="checkbox" name="receiver" value="lee">小李老师<br/>
    <input type="submit" value="发送"><input type="reset" value="重置">
</form>
<a href="input_do.jsp?msg=沐言科技&receiver=muyan&receiver=yootk">消息传递</a>

```

‍

接收数组参数（页面名称：input_do.jsp）

```html
<%
    request.setCharacterEncoding("UTF-8"); 			// 设置编码
    String[] msg = request.getParameterValues("msg") ; 		// 接收请求参数
    // 如果使用了getParameter()方法则只会接收第一个参数内容
    String[] receiver = request.getParameterValues("receiver") ; 	// 接收请求参数
%>
<h1>消息内容：<%=java.util.Arrays.toString(msg)%></h1> 		<%-- 输出参数 --%>
<h1>消息用户：<%=java.util.Arrays.toString(receiver)%></h1> 		<%-- 输出参数 --%>

```

‍

‍

#### 动态接受参数

常规的做法都是需要明确的知道请求参数的名称，而后才可以正常接收，但是如果所发送的参数会被经常性的改变，这样固定的模式就无法满足程序的开发要求，为了解决此类问题，在request内置对象中提供了一个动态接收参数的方法：

‍

‍

(老旧接口)

jsp

```html
<body>
<%
    request.setCharacterEncoding("UTF-8"); // 设置请求编码
    response.setCharacterEncoding("UTF-8"); // 设置响应编码
    Enumeration<String> enumeration = request.getParameterNames(); // 实现了全部请求参数的接收
    while (enumeration.hasMoreElements()) { // 直接迭代
        String paramName = enumeration.nextElement(); // 获取参数名称
%>
        <h2><%=paramName%> = <%=java.util.Arrays.toString(request.getParameterValues(paramName))%></h2>
<%
    }
%>
</body>
```

‍

新的推荐方法

```html
public Mapstring,string[] getParameterMap()
```

jsp

```html
<body>

<%
    request.setCharacterEncoding("UTF-8"); // 设置请求编码
    response.setCharacterEncoding("UTF-8"); // 设置响应编码
    Map<String, String[]> paramMap = request.getParameterMap(); // 直接获取全部参数和内容; 动态参数
    for (Map.Entry<String, String[]> entry : paramMap.entrySet()){  // 迭代输出
%>

        <h2><%=entry.getKey()%> = <%=java.util.Arrays.toString(entry.getValue())%></h2>

<%
    }
%>

</body>
```

在之前是先获得了所有请求参数的名称，而后利用迭代的形式再通过参数查找对应的参  
数内容，而现在所有的信息都是通过  
Map 配对完成的，所以获取的时候性能要比重复的查找更高一些.

‍

‍

#### 上下文路径

资源获取

在WEB中如果需要进行某些资源文件（例如：图片、JavaScript程序等）加载时往往都需要编写大量的相对路径，这样一旦所需要加载的资源存储路径过深，则会造成路径匹配的繁琐程度，在动态WEB中为了解决此类问题，可以直接利用request对象中的getContextPath()方法获取当前上下文路径名称，而后从根路径开始实现资源匹配；

‍

加载WEB资源

```html
<img src="<%=request.getContextPath()%>/images/yootk.png" style="width: 400px;">
```

‍

在之前学习过了一个Tomcat虚拟目录的配置，在进行虚拟目录配置的时候，实际上就存在有一个上下文名称，那么此时就存在有了一个虚拟目录的信息:

在进行访问的时候需要通过“/”进行目录的访问，但是随着你配置的虚拟目录的不同，这个名称也需要动态的获取到，所以此时就可以利用HttpServletRequest接口所提供的一个方法来获取此信息，这个方法的具体定义如下:

```java
public String getContextPath()
```

这个方法返回的是一个字符串类型的数据，而这个数据所能够取得就是当前配置的访问路径.

‍

‍

示例 获取信息

```java
<body>
<h1>ContextPath = <%=request.getContextPath()%></h1>
</body>
```

现在可以非常清楚的发现，在进行根路径信息返回的时候返回的是一个空字符串，而在配置了具体的path内容之后，可以返回的内容就是一个所配置的“Application Context”路径信息，这个方法是可以动态的获取所需要的配置项的功能

在整个的WEB开发中除了程序之外，最为头疼的一点就是应用的资源，例如:图片、视频、CSS样式、JS程序，在进  
访问的时候必须准确的设置好对应的路径，否则将无法讲行加载

‍

案例

> 但是如果说现在修改了“context_path.jsp'  
> 保存路径，将其保存在了“WEBROOT/pages,  
> /message”子目录里面，要想再  
> 次进行“images/logo.png”图片的访问，这个时候就需要编写相对路径了，如果要想继续正确  
> 的实现资源的访问，那么就需要修  
> 改资源的引用路径. “

‍

‍

‍

不好

此时的程序代码使用了“../”的形式进行了上级目录的引用，但是由于需要向上两级目录,  
所以就要使用到“../.."，按照这  
样的方式假设说需要保存在五级目录之中，则这个“../”也要不断的出现，但是这样一来在以后t  
进行各种服务器端跳转的时候，就  
有可能产生各种各样的问题了，所以最佳的做法是可以通过根路径开始查找.

```java
<body>
<h1>ContextPath = <%=request.getContextPath()%></h1>
<div><img src="../../images/logo.png"></div>
</body>
```

‍

‍

使用<%=request.getContextPath()%>

```java
<body>
<h1>ContextPath = <%=request.getContextPath()%></h1>
<div><img src="<%=request.getContextPath()%>/images/logo.png"></div>
</body>
```

在以后的开发中用这样的方式实现资源的定位，整体的处理效果是最方便的，因为不管用户如何的移动程序代码，只要根路径的匹配不改变，资源就都可以直接进行完整的定位.

‍

‍

#### base资源定位

利用request内置对象中的getContextPath()方法的确可以非常方便的获取对应的上下文路径，但是如果所有的资源都采用此类的方式进行访问，则会非常的繁琐，为了进一步简化资源加载的问题，可以在项目中通过HTML中的“<base>”元素进行统一资源定位

‍

‍

实际上在真正的JavaWEB项目之中，如果要想解决当前的资源定位问题，最佳的做法是利  
用“<base>”元素来实现，这个  
元素是HTML所提供的，主要的作用是提供了-  
个加载的起始路径，可以简单的理解为，通过“<base>”设置的路径就是所有图  
片或者是其他资源开始进行目录定位的路径，但是在实际的开发中一般都需要动态的获取一个完  
;整的base路径（例如:你现在对  
应的网站地址为:“www.yootk.com”，但是如果要想涉及到路径配置，则应该将base的内容定  
义为“http://www.yootk.com” ,  
后面还应该加上可能存在的虚拟目录的名称)，这个时候就要通过动态获取的机制来完成了. .

‍

|No.|方法名称|类型|描述|
| -----| ------------------------------| ------| ------------------------------------|
|01|public StringgetScheme()|方法|获取当前的路径模式，例如：“http”|
|02|public StringgetServerName()|方法|获取主机名称，例如：“localhost”|
|03|public int getServerPort()|方法|获取当前的主机端口，例如：“80”|

‍

拼接

String basePath = request.getScheme() + "://" + request.getServerName() + ":" + request.getServerPort() + request.getContextPath() + "/";

‍

```java
<%
    String basePath = request.getScheme() + "://" + request.getServerName() + ":" + request.getServerPort() + request.getContextPath() + "/";
    //out.print(basePath);
%>

<html>
<head>  
    <title>沐言科技：www.yootk.com</title>
    <base href="<%=basePath%>">  <%--  资源定位  --%>
</head>

<body>
<div><img src="images/logo.png"></div>
</body> 

</html>
```

‍

‍

#### 获取客户端请求信息

在用户每一次发出请求之后除了所有的数据之外，实际上还会包含有一些附加的信息，包括：请求方法、用户访问地址等内容

‍

|No.|方法名称|类型|描述|
| -----| -----------------------------------| ------| -------------------------------------|
|01|public String getMethod()|方法|获取当前请求的类型，例如：GET、POST|
|02|public StringgetProtocol()|方法|获取协议信息|
|03|public StringgetPathInfo()|方法|获取扩展路径信息|
|04|public String getQueryString()|方法|获取路径附加信息|
|05|public String getRemoteAddr()|方法|获取客户端请求的IP地址|
|06|public String getRequestURI|方法|获取请求的URI|
|07|public StringBuffer getRequestURL|方法|获取请求的URL|

‍

```java
<body>
<h1>HTTP请求模式：<%=request.getMethod()%></h1>
<h1>HTTP协议版本：<%=request.getProtocol()%></h1>
<h1>URL重写信息：<%=request.getQueryString()%></h1>
<h1>客户端IP地址：<%=request.getRemoteAddr()%></h1>
<h1>URI信息：<%=request.getRequestURI()%></h1>
<h1>URL信息：<%=request.getRequestURL()%></h1>
</body>
```

‍

此时可以非常清楚的发现，URI所获得的是抛去协议、主机以及端口之后剩下的访问路径,  
而URL返回的是一个完整的请求  
路径，但是对应的请求参数是不会出现的.

‍

‍

‍

‍

### response对象

response对象是javax.servlet.http.HttpServletResponse类的实例. 当服务器创建request对象时会同时创建用于响应这个客户端的response对象.

response对象也定义了处理HTTP头模块的接口. 通过这个对象，开发者们可以添加新的cookies，时间戳，HTTP状态码等等.

‍

客户端通过浏览器向服务器端发送请求之后，所有的请求可以被服务器端进行处理，而处理  
之前的请求接收就是通过request  
内置对象完成的，但是在服务器端如果要想对用户进行响应，就需要通过response内置对象来  
完成.  
response内置对象对应的类型为: jakarta.servlet.http.HttpServletResponse接口

‍

响应输出

‍

所有的JSP程序在进行响应前都需要经过WEB容器进行处理，实际上所有输出到客户端的HTML代码都是由response响应的结果，在response中可以直接通过IO流进行输出

|No.|方法|类型|描述|
| -----| ----------------------------------------------------------------| ------| --------------------|
|1|publicServletOutputStream getOutputStream()  throwsIOException|方法|返回字节输出流对象|
|2|public PrintWriter getWriter()  throws IOException|方法|返回字符打印流对象|

‍

现在可以非常清楚的发现，HttpServletResponse接口的继承的形式和HttpServletRequest接口继承形式是非常相似的  
都是有一个公共协议的父接口，而后这个父接口在当前只有HTTP协议支持.

‍

往客户端打印(设置请求打印编码解决乱码)

```java
<%@ page import="java.io.PrintWriter" %>
<%@ page pageEncoding="UTF-8" %>    <%-- 设置显示编码 --%>
<%
    request.setCharacterEncoding("UTF-8");
    response.setCharacterEncoding("UTF-8");
    PrintWriter printWriter = response.getWriter(); // 获取客户端打印流
    printWriter.println("<h1>沐言科技：www.yootk.com</h1>"); // 设置响应内容
    printWriter.close(); // 关闭打印流
%>
```

‍

在之前的所有的HTML响应都是直接编写了HTML代码实现的，而你现在所做的处理是通过打印流的方式来完成的，有什么  
区别呢?本质上来讲是没有任何区别的，因为在JSP中所有直接编写的HTML代码最终都是通过IO流响应输出的，这一点可以直接通过生成的Java源程序来观察到.

‍

#### 响应头信息

用户在每一次发出请求以及服务器响应时，除了所需要的核心数据之外，还会包含有许多的附加信息，这些附加信息就属于头信息

‍

如果服务器端要想获得所有的请求头信息，那么就必须通过HttpServletRequest接口所  
提供的方法来完成（只有HTTP协议之中存在有请求头信息的概念).

‍

|No.|方法|所属对象|描述|
| -----| ---------------------------------------------------| ----------| --------------------------------|
|1|public Enumeration<String>getHeaderNames()|request|获取全部请求头信息名称|
|2|public StringgetHeader(String  name)|request|获取指定名称的头信息内容|
|3|public void addHeader(String  name, String value)|response|设置指定头信息|
|4|public boolean  containsHeader(String name)|response|判断是否存在有指定名称的头信息|

通过此时给定的方法可以清楚的发现，对于所有可以获得的请求头信息的内容，是可以通过  
getHeaderNames()方法获取到  
所有的头信息的名称，而后对于头信息的内容，则可以通过“getHeader()”方法来完成.

‍

‍

```java
<%@ page pageEncoding="UTF-8" import="java.util.*" %>    <%-- 设置显示编码 --%>
<%
    Enumeration<String> headerNamesEunm = request.getHeaderNames(); // 获取全部头信息名称
    while (headerNamesEunm.hasMoreElements()) {
        String headerName = headerNamesEunm.nextElement(); // 获得头信息的名称
%>
        <h2><%=headerName%> = <%=request.getHeader(headerName)%></h2>
<%
    }
%>
```

运行获得几次结果, 比对发现多了cookie

下面再尝试获得请求头信息

```html
<body>
<h1>URI信息：<%=request.getRequestURI()%></h1>
<h1>URL信息：<%=request.getRequestURL()%></h1>
<a href="get_headers.jsp">获取请求头信息</a>
</body>
```

和前面的结果比对发现多了referer

‍

在实际的开发中经常需要统计一些页面之间的访问关联性，那么这样就可以通过“referer"  
这个头信息来获取来源页面. 用  
户的请求头信息是在用户发送请求的时候由浏览器自动获取并添加的，服务器端是无法控制其数  
据内容的.

‍

‍

但是对于客户端可以接收到的头信息却可以通过response内置对象来进行控制，例如:设置一个定时刷新的头信息.

```html
<%@ page pageEncoding="UTF-8" import="java.util.*" %>    <%-- 设置显示编码 --%>
<%! int num = 1; // 全局变量%>
<%
    response.addHeader("refresh", "1"); // 设置刷新头信息，每秒刷新1次
%>
<h1>num = <%=num ++%></h1>
```

‍

实际上定时刷新还有一个扩展的功能,就是可以实现定时跳转的操作，当达到了某些时间之后可以实现跳转处理

```html
<%@ page pageEncoding="UTF-8" import="java.util.*" %>    <%-- 设置显示编码 --%>
<%
    response.addHeader("refresh", "2; url=get_headers.jsp"); // 设置定时跳转
%>
```

需要注意的是，此时跳转之后的页面地址栏是发生改变的，所以这样的跳转在开发中称为  
“客户端跳转”(无法传递request属性内容). .

‍

#### 响应状态码

‍

设置状态码

```html
<%@ page pageEncoding="UTF-8" import="java.util.*" %>
<%
    response.setStatus(HttpServletResponse.SC_NOT_FOUND);
%>
```

‍

#### 请求重定向

‍

在WEB开发中页面之间可以通过超链接的方式实现跳转，如果现在希望可以通过程序的方式来实现类似于超链接的跳转操作，则可以通过response对象中提供的请求重定向方法完成

操作方法：public void sendRedirect(String location) throws IOException

```html
<% response.sendRedirect("message.jsp");   // 请求重定向    %>
```

‍

‍

当程序代码执行到此语句的时候就会自动的发生跳转处理操作，同时也可以发现，跳转之后的地址栏发生了改变，所以这种跳转属于客户端跳转.

实际上对于当前的开发来讲，跳转的操作形式还是挺多的，而且在整个的JSP里面就有了两种跳转

‍

1、客户端跳转:跳转之后地址栏会发生改变，所以之前的请求数据会自动消失;

    HTML超链接;

    response.sendRedirect("跳转路径");

    response.setHeader("refresh", "O;url=跳转路径");

‍

2、服务器端跳转∶跳转之后地址栏不会发生改变，但是可以传递request属性范围

     <jsp:forward page="跳转路径"/>;

‍

现在通过执行结果可以清楚的发现，跳转前和跳转之后的语句都执行了，那么就可以得出  
-个结论:当前的客户端跳转是在整个页面的动态代码执行完毕后再进行跳转操作

‍

通过此时的执行结果可以发现，当执行的是服务器端跳转的时候，剩余的后续代码是不会被执行的，属于无条件跳转操作，那么可以想象一下，如果现在你在`<jsp:forward>`​语法之后进行了数据库关闭操作，那么这样的关闭操作是有可能不会执行的，而一旦不执行，你的数据库就再也关不上了(在当前运行的状态下). “

‍

‍

‍

‍

‍

### out对象

out对象是 javax.servlet.jsp.JspWriter 类的实例，用来在response对象中写入内容.

最初的JspWriter类对象根据页面是否有缓存来进行不同的实例化操作. 可以在page指令中使用buffered='false'属性来轻松关闭缓存.

JspWriter类包含了大部分java.io.PrintWriter类中的方法. 不过，JspWriter新增了一些专为处理缓存而设计的方法. 还有就是，JspWriter类会抛出IOExceptions异常，而PrintWriter不会.

下表列出了我们将会用来输出boolean，char，int，double，String，object等类型数据的重要方法：

|**方法**|**描述**|
| --| --------------------------|
|**out.print(dataType dt)**|输出Type类型的值|
|**out.println(dataType dt)**|输出Type类型的值然后换行|
|**out.flush()**|刷新输出流|

---

### session对象

session对象是 javax.servlet.http.HttpSession 类的实例. 和Java Servlets中的session对象有一样的行为.

session对象用来跟踪在各个客户端请求间的会话.

---

### application对象

application对象直接包装了servlet的ServletContext类的对象，是javax.servlet.ServletContext 类的实例.

这个对象在JSP页面的整个生命周期中都代表着这个JSP页面. 这个对象在JSP页面初始化时被创建，随着jspDestroy()方法的调用而被移除.

通过向application中添加属性，则所有组成您web应用的JSP文件都能访问到这些属性.

‍

‍

application是javax.servlet.ServletContext接口实例，明确的描述的是一个WEB应用上下文环境，在Tomcat中可以同时部署多个WEB应用，并且每一个不同的上下文中都存在有各自的application内置对象

‍

application是一个描述Servlet上下文的内置对象，本质上如果要去描述上下文就是指的环境application内置对象对应的完整类型是: jakarta.servlet.ServletContext(此处没有了"jakarta.servlet.http”包)，因为这个操作接口不是一个针对于HTTP设计的，它属于一个更高一级的概念，与ServletRequest、ServletResponse父接口有直接的关联.

‍

录实际上就都属于一各个不同  
需要注意的是，在每一个Tomcat 之中，实际上都可以配置有多个虚拟目录，而不同的虚拟目  
的WEB上下文，每一个WEB上下文(WEB项目）都存在有一个application内置对象信息. .

‍

在讲解四种属性范围的时候一再强调,application属性是保存在服务器上，如果服务器不关闭则application的属性是不会  
消失的，一般不建议保存过多的属性信息. . 项目中可以使用的全部的CLASSPATH上的信息都在application属性中进行了完整的记录

```html
<%
    Enumeration<String> enumeration = application.getAttributeNames(); // 获取全部属性的名称
    while(enumeration.hasMoreElements()) {
        String name = enumeration.nextElement(); // 获取属性名称
%>
        <p><%=name%> = <%=application.getAttribute(name)%></p>
<%
    }
%>
```

需要注意的是，在当前存储的路径前追加有一个“""，实际上这是一种Linux路径描述，这种形  
式实际上是属于cygwin的写  
法（是一种在Windows系统中模拟Linux应用的软件). ·

‍

‍

### config对象

config对象是 javax.servlet.ServletConfig 类的实例，直接包装了servlet的ServletConfig类的对象.

这个对象允许开发者访问Servlet或者JSP引擎的初始化参数，比如文件路径等.

以下是config对象的使用方法，不是很重要，所以不常用：

```
config.getServletName();
```

它返回包含在<servlet-name>元素中的servlet名字，注意，<servlet-name>元素在 WEB-INF\web.xml 文件中定义.

‍

‍

当JSP程序需要通过web.xml进行映射访问的时候，实际上在 web.xml文件里面除了可以定义映射路径之外，最为重要的一点就是可以通过映射配置为程序追加一些初始化的配置参数，这些配置参数名字和内容全部都是String的形式出现. 如果要想在程序中获取这些初始化配置参数的内容，则可以通过config内置对象来实  
现，config内置对象对应的类型jakarta.servlet.ServletConfig(没有与协议绑定的部分). ·

‍

通过web.xml文件实现的程序映射访问，除了可以获得更加安全的保护之外，也可以实现初始化参数的配置，在JSP中所有的初始化参数需要通过config内置对象（对应类型为“javax.servlet.ServletConfig”接口）实现接收

‍

|No.|方法|类型|描述|
| -----| ---------------------------------------------------| ------| ----------------------------|
|1|public StringgetServletName()|方法|获取Servlet程序名称|
|2|publicServletContext getServletContext()|方法|获取ServletContext接口实例|
|3|public String  getInitParameter(String name)|方法|获取指定名称的初始化参数|
|4|public Enumeration<String>getInitParameterNames()|方法|获取所有初始化参数的名称|

‍

修改web.xml文件追加初始化参数配置

```html
 <servlet>
        <servlet-name>HelloJSP</servlet-name>
        <jsp-file>/WEB-INF/hello.jsp</jsp-file>
        <init-param>
            <param-name>databaseName</param-name>
            <param-value>yoootk</param-value>
        </init-param>
        <init-param>
            <param-name>databaseHost</param-name>
            <param-value>www.yootk.com/mysql</param-value>
        </init-param>
    </servlet>
    <servlet-mapping>
        <servlet-name>HelloJSP</servlet-name>
        <url-pattern>/muyan.yootk</url-pattern>
    </servlet-mapping>
```

配置了databaseName, databaseHost两个

‍

获取初始化参数

```html
<%
    Enumeration<String> enu = config.getInitParameterNames(); // 获取全部初始化参数名称
    while (enu.hasMoreElements()) { // 枚举迭代
        String name = enu.nextElement(); // 获取参数名称
%>
        <h3><%=name%> = <%=config.getInitParameter(name)%></h3>
<%
    }
%>
```

‍

在每一个映射路径加载的时候，除了自定义的初始化参数之外，也会包含有一些其他系统内置的参数处理，重点是关注这些初始化参数的获取以及配置操作

‍

‍

### pageContext 对象

pageContext对象是javax.servlet.jsp.PageContext 类的实例，用来代表整个JSP页面.

这个对象主要用来访问页面信息，同时过滤掉大部分实现细节.

这个对象存储了request对象和response对象的引用. application对象，config对象，session对象，out对象可以通过访问这个对象的属性来导出.

pageContext对象也包含了传给JSP页面的指令信息，包括缓存信息，ErrorPage URL,页面scope等.

PageContext类定义了一些字段，包括PAGE_SCOPE，REQUEST_SCOPE，SESSION_SCOPE， APPLICATION_SCOPE. 它也提供了40余种方法，有一半继承自javax.servlet.jsp.JspContext 类.

其中一个重要的方法就是 removeAttribute()，它可接受一个或两个参数. 比如，pageContext.removeAttribute("attrName") 移除四个scope中相关属性，但是下面这种方法只移除特定 scope 中的相关属性：

```
pageContext.removeAttribute("attrName", PAGE_SCOPE);
```

‍

‍

在之前讲解JSP四种属性范围的时候曾经重点分析过了pageContext内置对象的操作，而  
pageContext进行处理的时候最  
为核心的一点是它除了可以实现page属性范围的配置之外，也可以利用其重载的方法实现其他  
所有三个属性范围的配置，当时的  
这种直观感受就是在于: pageContext 很万能，实际上pageContext是一个功能非常全面的内  
置对象，可以这么说，你所能见到  
的内置对象中的处理操作，都可以通过pageContext 一个对象实现.

‍

在JavaWEB开发中每一个JSP页面程序都可以直接使用各个内置对象完成相应的处理，同时在每一个JSP程序文件中都包含有一个完整的pageContext内置对象，表示页面上下文实例

‍

以发现这个类继承了JspContext,pageContext内置对象对应的完整类型是“jakarta.servlet.jsp.PageContext”，同时也可以直观的感受到这种操作和JSP有关，pageContext仅仅只能够出现在JSP程序代码之中

‍

常用方法

‍

|No.|方法名称|类型|描述|
| -----| --------------------------------------------------------------------------------------------| ------| -----------------------------|
|01|public abstract void  forward(StringrelativeUrlPath)  throws ServletException,IOException|方法|服务器端跳转|
|02|public abstract void  include(String relativeUrlPath) throws ServletException, IOException|方法|页面包含|
|03|public abstract ServletRequest  getRequest()|方法|获取ServletRequest接口实例|
|04|public abstract ServletResponse  getResponse()|方法|获取ServletResponse接口实例|
|05|public abstract ServletConfig  getServletConfig()|方法|获取ServletConfig接口实例|
|06|public abstractServletContext getServletContext()|方法|获取ServletContext接口实例|
|07|public abstractHttpSession getSession()|方法|获取HttpSession接口实例|

‍

以上给出了PageContext类对象所提供的几个处理的操作方法，这些提供的“getXxx()”方  
j法对应的全部都是JSP中所给出  
的九个内置对象，那么就可以得出一个结论: PageContext内置对象实在是非常的万能

‍

‍

另外一点就是在 PageContext类中提供有  
forward).include()两个操作，可以使用pag  
eContext提供的跳转方法，避免  
编写过多的Scriptlet代码. ·

‍

设置属性内容（页面名称：scope_set.jsp）

```html
<%
    pageContext.getRequest().setAttribute("r-info","Yootk Request");	// 属性设置
    pageContext.getSession().setAttribute("s-info","Yootk Session");	// 属性设置
    pageContext.forward("scope_get.jsp"); 				// 服务器端跳转
%>
```

如果使用的是传统的jsp:forward操作，则肯定要编写两段不同的“Scriptlet”程序代码  
这样是非常不方便的.

‍

‍

获取属性内容（页面名称：scope_get.jsp）

```html
<h1>Request属性：<%=pageContext.getRequest().getAttribute("r-info")%></h1>
<h1>Session属性：<%=pageContext.getSession().getAttribute("s-info")%></h1>
```

‍

此时通过代码的演示就可以发现，pageContext对象的功能还是足够强大，同时也足够的  
好用，但是有一个最致命的问题,  
就是这个内置对象只能够在JSP程序中去使用  
，所以在实际的项目开发中使用最多的内置对象类型:HttpServletRequest.  
HttpSession、HttpServletResponse、ServletContext、ServletConfig.

‍

‍

### page 对象

这个对象就是页面实例的引用. 它可以被看做是整个JSP页面的代表.

page 对象就是this对象的同义词.

---

### exception 对象

exception 对象包装了从先前页面中抛出的异常信息. 它通常被用来产生对出错条件的适当响应.

‍

‍

### 获得真实路径

每一个WEB应用都对应有一个磁盘的真实存储路径，由于WEB开发环境和部署环境往往不同，所以就需要可以在代码中动态的获取项目所在的真实路径，这样的功能就可以通过ServletContext接口中的“getRealPath()”方法来获得

public String getRealPath(String path)

‍

```html
<%  // 任何情况下，每一个WEB应用都一定会存在有一个根路径
    String diskPathA = application.getRealPath("/"); // 获取根路径对应的真实路径
    String diskPathB = application.getRealPath("/uploads/"); // 获取一个子路径
%>
<h1><%=diskPathA%>、<%=new File(diskPathA).exists()%></h1>
<h1><%=diskPathB%>、<%=new File(diskPathB).exists()%></h1>
```

‍

此时由于是在IDEA下进行的项目开发，所以现在可以获取到的就是IDEA中进行项目部署时所使用  
的程序路径.

在使用getRealPath()方法获取访问路径的时候实际上是不会区分该路径是否真实存在的，因为  
这个地方仅仅是进行了路径的生成，而不作为路经有效性的检查，最重要的一点，如果要想判断这个路径是否存在，可以直接使用File类来完成

‍

```html
<%=new File(diskPathB).exists()%>
```

‍

如果所获取到的路径可以使用，判断存在的结果返回的就是一个true，否则返回的就是一个fal  
lse 数据，需要注意的是，既然已经有了application，实际上在实际的编写  
的时候，很多的开发者不会直接使用application的名称，而是会通过  
getServletContext()方法来代替application. ·

```html
<%  // 任何情况下，每一个WEB应用都一定会存在有一个根路径
    String diskPathA = getServletContext().getRealPath("/"); // 获取根路径对应的真实路径
    String diskPathB = this.getServletContext().getRealPath("/uploads/"); // 获取一个子路径
%>
```

在以后进行JavaWEB开发的时候经常会见到  
"getServletContext()”方法，一旦见到此方法二话不说，它直接描述的就是  
application内置对象信息. ·

‍

‍

### 初始化参数

在WEB应用中会存在有大量的属性内容，这些属性一般分为两种. 一种是Tomcat启动时自动设置的系统属性，另外一种就是用户通过程序设置的应用属性，这些属性被所有的session共享. 除了系统属性的处理之外，在application中还提供了初始化属性的获取操作，而这些初始化的属性必须通过“WEB-INF/web.xml”文件进行配置.

‍

所谓的application属性本质上指的就是一个Map集合，只不过这个Map集合的KEY固定为St  
tring,而Map集合的VALUE  
固定为Object，而对于application所设置的属性来讲，一个是系统属性，另外一个就是应用属性,  
这些属性是由开发者自行定  
义的，而这些属性的配置是可以直接通过web.xml文件进行配置的. “

‍

web.xml配置

```html
    <context-param> <!-- 定义application初始化信息 -->
        <param-name>basePackages</param-name> <!-- 参数名称 -->
        <param-value>com.yootk.service,com.yootk.dao</param-value> <!-- 参数内容 -->
    </context-param>
    <context-param> <!-- 定义application初始化信息 -->
        <param-name>resources</param-name> <!-- 参数名称 -->
        <param-value>/META-INF/config/*.xml</param-value> <!-- 参数内容 -->
    </context-param>
```

此时可以使用的初始化参数(自定义的)一共有两个，但是千万记住了，这种配置方式所设置  
的初始化的内容全部都是字符  
串类型，而要想获取这些初始化的参数，就可以使用如下的方法完成. .

‍

|No.|方法|类型|描述|
| -----| ------------------------------------------------------------| ------| --------------------------|
|1|public String  getInitParameter(String name)|方法|获得指定名称的初始化参数|
|2|public Enumeration<String>  getInitParameterNames()|方法|获得全部初始化参数的名称|
|3|publicboolean setInitParameter(String  name, String value)|方法|设置初始化参数名称与内容|

‍

获取初始化参数

```html
<body>
<%  // 任何情况下，每一个WEB应用都一定会存在有一个根路径
    Enumeration<String> enumeration = this.getServletContext().getInitParameterNames(); // 获取全部初始化参数名称
    while (enumeration.hasMoreElements()) { // 迭代获取名称
        String name = enumeration.nextElement(); // 获取参数名称
%>
        <%=name%> = <%=application.getInitParameter(name)%><br>
<%
    }
%>
</body>
```

‍

‍

## 重定向

‍

最简单的重定向方式就是使用response对象的sendRedirect()方法. 这个方法的签名如下：

```
public void response.sendRedirect(String location)
throws IOException 
```

这个方法将状态码和新的页面位置作为响应发回给浏览器. 您也可以使用setStatus()和setHeader()方法来得到同样的效果：

```
....
String site = "http://www.runoob.com" ;
response.setStatus(response.SC_MOVED_TEMPORARILY);
response.setHeader("Location", site); 
....
```

‍

‍

### 实例

‍

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.io.*,java.util.*" %>
<html>
<html>
<head>
<title>页面重定向</title>
</head>
<body>

<h1>页面重定向</h1>

<%
   // 重定向到新地址
   String site = new String("http://www.runoob.com");
   response.setStatus(response.SC_MOVED_TEMPORARILY);
   response.setHeader("Location", site); 
%>

</body>
</html>
```

‍

‍

‍

‍

## HTTP 状态码

HTTP请求与HTTP响应的格式相近，都有着如下结构：

* 以状态行+CRLF（回车换行）开始
* 零行或多行头模块+CRLF
* 一个空行，比如CRLF
* 可选的消息体比如文件，查询数据，查询输出

下表列出了可能会从服务器返回的HTTP状态码和与之关联的消息：

|**状态码**|**消息**|**描述**|
| -----| -------------------------------| --------------------------------------------------------------------------------------------|
|100|Continue|只有一部分请求被服务器接收，但只要没被服务器拒绝，客户端就会延续这个请求|
|101|Switching Protocols|服务器交换机协议|
|200|OK|请求被确认|
|201|Created|请求时完整的，新的资源被创建|
|202|Accepted|请求被接受，但未处理完|
|203|Non-authoritative Information||
|204|No Content||
|205|Reset Content||
|206|Partial Content||
|300|Multiple Choices|一个超链接表，用户可以选择一个超链接并访问，最大支持5个超链接|
|301|Moved Permanently|被请求的页面已经移动到了新的URL下|
|302|Found|被请求的页面暂时性地移动到了新的URL下|
|303|See Other|被请求的页面可以在一个不同的URL下找到|
|304|Not Modified||
|305|Use Proxy||
|306|*Unused*|已经不再使用此状态码，但状态码被保留|
|307|Temporary Redirect|被请求的页面暂时性地移动到了新的URL下|
|400|Bad Request|服务器无法识别请求|
|401|Unauthorized|被请求的页面需要用户名和密码|
|402|Payment Required|*目前还不能使用此状态码*|
|403|Forbidden|禁止访问所请求的页面|
|404|Not Found|服务器无法找到所请求的页面|
|405|Method Not Allowed|请求中所指定的方法不被允许|
|406|Not Acceptable|服务器只能创建一个客户端无法接受的响应|
|407|Proxy Authentication Required|在请求被服务前必须认证一个代理服务器|
|408|Request Timeout|请求时间超过了服务器所能等待的时间，连接被断开|
|409|Con'flict|请求有矛盾的地方|
|410|Gone|被请求的页面不再可用|
|411|Length Required|"Content-Length"没有被定义，服务器拒绝接受请求|
|412|Precondition Failed|请求的前提条件被服务器评估为false|
|413|Request Entity Too Large|因为请求的实体太大，服务器拒绝接受请求|
|414|Request-url Too Long|服务器拒绝接受请求，因为URL太长. 多出现在把"POST"请求转换为"GET"请求时所附带的大量查询信息|
|415|Unsupported Media Type|服务器拒绝接受请求，因为媒体类型不被支持|
|417|Expectation Failed||
|500|Internal Server Error|请求不完整，服务器遇见了出乎意料的状况|
|501|Not Implemented|请求不完整，服务器不提供所需要的功能|
|502|Bad Gateway|请求不完整，服务器从上游服务器接受了一个无效的响应|
|503|Service Unavailable|请求不完整，服务器暂时重启或关闭|
|504|Gateway Timeout|网关超时|
|505|HTTP Version Not Supported|服务器不支持所指定的HTTP版本|

‍

### 设置HTTP状态码的方法

下表列出了HttpServletResponse 类中用来设置状态码的方法：

|**S.N.**|**方法**  **&amp;**  **描述**|
| ---| ------------------------------------------------------------------------------------------------------------------------------|
|1|**public void setStatus ( int statusCode )** 此方法可以设置任意的状态码. 如果您的响应包含一个特殊的状态码和一个文档，请确保在用PrintWriter返回任何内容前调用setStatus方法|
|2|**public void sendRedirect(String url)** 此方法产生302响应，同时产生一个*Location*头告诉URL 一个新的文档|
|3|**public void sendError(int code, String message)** 此方法将一个状态码(通常为 404)和一个短消息，自动插入HTML文档中并发回给客户端|

‍

‍

‍

‍

‍

# 高级

‍

## **动作元素**

与JSP指令元素不同的是，JSP动作元素在请求处理阶段起作用. JSP动作元素是用XML语法写成的.

利用JSP动作可以动态地插入文件、重用JavaBean组件、把用户重定向到另外的页面、为Java插件生成HTML代码.

动作元素只有一种语法，它符合XML标准：

```
<jsp:action_name attribute="value" />
```

‍

动作元素基本上都是预定义的函数，JSP规范定义了一系列的标准动作，它用JSP作为前缀，可用的标准动作元素如下：

|语法|描述|
| -----------------| ------------------------------------------------|
|jsp:include|在页面被请求的时候引入一个文件.|
|jsp:useBean|寻找或者实例化一个JavaBean.|
|jsp:setProperty|设置JavaBean的属性.|
|jsp:getProperty|输出某个JavaBean的属性.|
|jsp:forward|把请求转到一个新的页面.|
|jsp:plugin|根据浏览器类型为Java插件生成OBJECT或EMBED标记.|
|jsp:element|定义动态XML元素|
|jsp:attribute|设置动态定义的XML元素属性.|
|jsp:body|设置动态定义的XML元素内容.|
|jsp:text|在JSP页面和文档中使用写入文本的模板|

详细略

‍

### 常见的属性

所有的动作要素都有两个属性：id属性和scope属性.

* **id属性：** id属性是动作元素的唯一标识，可以在JSP页面中引用. 动作元素创建的id值可以通过PageContext来调用.
* **scope属性：** 该属性用于识别动作元素的生命周期.  id属性和scope属性有直接关系，scope属性定义了相关联id对象的寿命.  scope属性有四个可能的值： (a) page, (b)request, (c)session, 和 (d) application.

‍

‍

### MIME

‍

MIME (Multipurpose Internet Mail Extensions、多用途互联网邮件扩展类型）是一种根据文件扩展名匹配相关应用程序的标识

例如:当用户获取到了一个pdf文件并打开时，会自动匹配本机的 PDF 阅读器，这依靠的就是 MIME配置实现的

‍

page指令中可以通过“contentType”属性来实现MIME类型的定义.

‍

在传统的WEB项目中，所有的JSP程序执行之后实际上最终所完成的功能就是进行HTML代码的输出，而之所以可以输出  
HTML代码，主要的原因就在于其MIME默认类型为“text/html”

‍

设置MIME类型为HTML, 加不加都行

​`<%@ page contentType="text/html; charset=UTF-8"%>`​

‍

查看支持的类型: TOMCAT-conf-web.xml -> <mime-mapping>标签

例如XML类型指定    application/xml

```html
    <mime-mapping>
        <extension>xml</extension>
        <mime-type>application/xml</mime-type>
    </mime-mapping>
```

‍

‍

## Scriptlet

‍

‍

Scriptlet(中文含义为“脚本小程序")是在JSP页面之中编写Java程序语句的标记

‍

一直在强调，所有的JSP本质上就是通过Java程序代码实现了HTML输出的控制，而为了区分Java代码，实际上在之前都会使用“<%%>”标记编写Java代码，而这样的标记在JSP中就称为Scriptlet

‍

在整个的JSP之中针对于Scriptlet语法提供有三类的操作形式:

* 代码Scriptlet
* 结构定义Scriptlet
* 表达式输出Scriptlet

明确说明的是，由于所有的JSP程序代码在最终执行的时候都需要自动将其转为Java源代码，并且要进行自动的class文件的编译，也就意味着使用Scriptlet定义的所有的结构，本质上就相当于是在定义Java程序.

‍

‍

‍

‍

‍

代码编写(略, 就是Java那一套)所有的JSP可以实现HTML代码的的动态输出，通过逻辑的方式来实现HTML的打印，但是如果使用的是静态WEB，那么就需要你编写大量的HTML代码了.

‍

需要清楚的一个核心概念:所有的JSP程序最终都转为了Java程序类，而在整个的WEB容器之中，每一个JSP程序实际上都只会提供有唯一的一个实例化对象，而现在所讲解的结构定义Scriptlet实际上就属于类结构的定义项.

‍

‍

### 特殊的

```html
<%-- 【注意】表达式输出语句为一个独立的Scriptlet结构，该结构不允许嵌套在其他Scriptlet之中 --%>
<h1><%=MESSAGE%></h1>             <%-- 输出全局常量 --%>
<h1><%="edu.yootk.com"%></h1>      <%-- 输出字符串常量 --%>
```

‍

现在可以清楚的发现，如果使用表达式输出Scriptlet进行的内容输出，实际上可以有效的实现HTML代码与Java 代码之间的分割，而如果直接使用out.println()，这个语句是必须放在代码编写Scriptlet之中定义的，而表达式输出没有这样的限制 (但是提升了上手难度)

‍

‍

表格输出

不断采用<%%>来分隔HTML与JSP代码来完成精细的功能

这是标准的开发模式

```html
<html>
<head>
    <title>www.yootk.com</title>
</head>
<body>
<table border="1">
<%
    for (int x = 1; x <= 9; x ++) {
%>
    <tr>
<%
        for (int y = 1 ; y <= x; y ++) {
%>
        <td> <%=x%> * <%=y%> = <%=x * y%> </td>
<%
        }
        for (int y = 1 ; y <= 9 - x; y ++) {
%>
        <td>&nbsp;</td>
<%
        }
%>
    </tr>
<%
    }
%>
</table>
</body>
</html>
```

此时的JSP真正的实现了HTML代码与Java代码的分离操作，不在Java中输出任何的HTML代码，所以代码的整洁程度是  
比较高的，但是由于利用了Scriptlet进行了过多的分割(前提:必须使用“状”声明)，所以代码阅读起来的感觉是比较混乱的...

‍

在以后的开发过程之中，不建议使用“out.println()"方法进行输出操作  
输出操作建议全部都使用  
表达式输出的Scriptlet来实现. ﹒

‍

‍

### jsp:scriptlet标签

在之前已经编写了大量的Scriptlet程序，这里面也会存在有一个问题，所有的程序代码里面实际上会有大量的“<%%>”结  
构出现，所以造成JSP代码阅读的不方便，于是如果可以将Scriptlet代码的形式按照HTML元素风格的方式来进行定义

通过此时执行的对比可以发现，原来使用“jsp:scriptlet”标签本质上就相当于代表了之前的代码编写  
Scriptlet程序结构, 没什么用

‍

‍

‍

‍

‍

‍

## JavaBean

‍

JavaBean是一个程序类，通过这个自定义的程序类可以实现一些特定的程序功能包装,但是如果要想在一个WEB项目中使  
用JavaBean，那么对于程序的存储是有要求的，所有的JavaBean必须编译为“*.class”文件（没有包的 Bean是不存在)，并且必须保存在“WEB项目/WEB-INF/classes”目录之中(如果你使用的是IDEA工具，则会在最终输出的时候自动保存)

‍

JavaBean 与其它 Java 类相比而言独一无二的特征：

* 提供一个默认的无参构造函数.
* 需要被序列化并且实现了 Serializable 接口.
* 可能有一系列可读写属性.
* 可能有一系列的 getter 或 **setter** 方法.

‍

### 属性

一个 JavaBean 对象的属性应该是可访问的. 这个属性可以是任意合法的 Java 数据类型，包括自定义 Java 类.

一个 JavaBean 对象的属性可以是可读写，或只读，或只写. JavaBean 对象的属性通过 JavaBean 实现类中提供的两个方法来访问：

|**方法**|**描述**|
| -------| -----------------------------------------------------------------------------------------------------------------|
|get**PropertyName**()|举例来说，如果属性的名称为 myName，那么这个方法的名字就要写成 getMyName() 来读取这个属性. 这个方法也称为访问器.|
|set**PropertyName**()|举例来说，如果属性的名称为 myName，那么这个方法的名字就要写成 setMyName()来写入这个属性. 这个方法也称为写入器.|

一个只读的属性只提供 getPropertyName() 方法，一个只写的属性只提供 setPropertyName() 方法.

‍

‍

### 示例

‍

bean配置数据库连接对象

src>>

```java
package com.yootk.dbc;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
public class DatabaseConnection {
    public static final String DBDRIVER = "com.mysql.cj.jdbc.Driver" ;  	// 驱动
    public static final String DBURL = "jdbc:mysql://localhost:3306/yootk" ; // 连接地址
    public static final String USER = "root" ; 				// 用户名
    public static final String PASSWORD = "mysqladmin" ; 			// 密码
    public static final ThreadLocal<Connection> THREAD_LOCAL = new ThreadLocal<>() ;
    private DatabaseConnection(){}			// 构造方法私有化禁止外部实例化本类对象
    private static Connection rebuildConnection() {	// 获取新的数据库连接
        Connection conn = null ; 			// 声明连接接口对象
        try {
            Class.forName(DBDRIVER) ; 		// 加载驱动
            conn = DriverManager.getConnection(DBURL,USER,PASSWORD) ; // 连接数据库
        } catch (Exception e) {
            e.printStackTrace();
        }
        return conn ;
    }
    public static Connection getConnection() { 	// 获取数据库连接
        Connection conn = THREAD_LOCAL.get() ; 	// 获取当前线程连接
        if (conn == null) { 			// 当前没有连接保存
            conn = rebuildConnection() ; 		// 创建新的连接
            THREAD_LOCAL.set(conn); 			// 保存数据库连接
        }
        return conn ; 				// 返回数据库连接
    }
    public static void close() { 			// 关闭数据库连接
        Connection conn = THREAD_LOCAL.get() ; 	// 获取当前线程连接
        if (conn != null) { 			// 如果存在连接对象
            try {
                conn.close();			// 连接关闭
            } catch (SQLException e) {
                e.printStackTrace();
            }
            THREAD_LOCAL.remove();			// 移除对象
        }
    }
}
```

‍

```java
<%@ page pageEncoding="UTF-8"%> 				<%-- 定义页面中文显示编码 --%>
<%@ page import="java.sql.*"%> 				<%-- 导入开发包 --%>
<%@ page import="com.yootk.dbc.DatabaseConnection"%>    	<%-- 自定义JavaBean --%>
<%
    String sql = "SELECT deptno,dname,loc FROM dept" ;   	// 查询SQL语句
    Connection conn = DatabaseConnection.getConnection();	// 获取数据库连接
    PreparedStatement pstmt = conn.prepareStatement(sql) ; 	// 创建操作对象
    ResultSet rs = pstmt.executeQuery() ; 			// 执行数据库查询
%>
<%-- 表格循环输出部分与之前相同，不再重复列出，可以参考视频讲解代码 --%>
<% // 如果不关闭连接就再也关闭不了，除非重新启动Tomcat
    DatabaseConnection.close();				// 关闭数据库
%>
```

‍

‍

‍

‍

## Mysql

‍

```html
<%@ page pageEncoding="UTF-8"%>
<%@ page import="java.sql.*" %> <%-- JDBC --%>
<html>
<head>
    <title>yootk.com</title>
</head>
<body>
<%!
    public static final String DBDRIVER = "com.mysql.cj.jdbc.Driver";
    public static final String DBURL = "jdbc:mysql://localhost:3306/yootk";
    public static final String USER = "root";
    public static final String PASSWORD = "mysqladmin";
%>
<%
    String sql = "SELECT deptno,dname,loc FROM dept";
    Class.forName(DBDRIVER); //加载驱动
    Connection conn = DriverManager.getConnection(DBURL, USER, PASSWORD); //连接
    PreparedStatement pstmt = conn.prepareStatement(sql); // 创建数据库操作对象
    ResultSet rs = pstmt.executeQuery();
%>
<table border="1" width="100%">
    <thead><tr><td>ID</td><td>NAME</td><td>LOCATION</td></tr></thead>
    <tbody>
<%
    while (rs.next()) { // 迭代结果集
        long deptno = rs.getLong(1);
        String dname = rs.getString(2);
        String loc = rs.getString(3);
%>
    <tr>
        <td><%=deptno%></td>
        <td><%=dname%></td>
        <td><%=loc%></td>
    </tr>
<%
    }
    conn.close(); //在WEB中如果没有关闭数据库连接，就再也关不上了，除非重新启动服务器

%>
    </tbody>
</table>
</body>
</html>
```

‍

‍

‍

## 表达式语言

JSP表达式语言（EL）使得访问存储在JavaBean中的数据变得非常简单. JSP EL既可以用来创建算术表达式也可以用来创建逻辑表达式. 在JSP EL表达式内可以使用整型数，浮点数，字符串，常量true、false，还有null.

‍

‍

## Cookie

HTTP属于无状态的网络通讯协议，服务器端为了便于状态保持，可以在客户端保存有部分的数据信息，这样的信息在HTTP协议中被称为Cookie（小甜饼干），所有的Cookie由服务器端进行设置，而在设置后会随着请求头信息发送到服务器之中从而实现应用

‍

‍

如果要想进行Cookie的操作,在JakartaEE之中提供有专属的Cookie的操作类库,可以使用"jakarta.servlet.http.Cookie" ,  
这个类的定义结构如下:  
public class Cookie extends Object implements Cloneable, Serializable

可以发现Cookie是Object的直接子类,同时Cookie类实现了Cloneable接口，那么这个接  
口的作用主要指的就是Cookie  
对象是可以进行对象克隆处理的，同时也可以直接进行序列化的处理操作，在Cookie 类中实际上也  
提供有许多的处理方法. .

‍

‍

在JavaWEB中，Cookie数据操作被封装在javax.servlet.http.Cookie类中. 在进行Cookie操作时必须通过关键字new实例化对象，同时设置Cookie的名称（name）以及对应内容（value）

|No.|方法名称|类型|描述|
| -----| ----------------------------------------------| ------| ----------------------------------------|
|01|public Cookie(String name, String  value)|方法|创建一个Cookie，设置Cookie的名字和内容|
|02|public StringgetName()|方法|获取Cookie的名字|
|03|public StringgetValue()|方法|获取Cookie的内容|
|04|public void setValue(String  newValue)|方法|修改Cookie的内容|
|05|public void setHttpOnly(boolean  isHttpOnly)|方法|该数据只允许HTTP协议访问|
|06|public void setMaxAge(int expiry)|方法|设置Cookie失效时间|
|07|public void setPath(String uri)|方法|设置Cookie的存储路径|

‍

‍

Cookie 是存储在客户机的文本文件，它们保存了大量轨迹信息. 在 Servlet 技术基础上，JSP 显然能够提供对 HTTP cookie 的支持.

通常有三个步骤来识别回头客：

* 服务器脚本发送一系列 cookie 至浏览器. 比如名字，年龄，ID 号码等等.
* 浏览器在本地机中存储这些信息，以备不时之需.
* 当下一次浏览器发送任何请求至服务器时，它会同时将这些 cookie 信息发送给服务器，然后服务器使用这些信息来识别用户或者干些其它事情.

本章节将会传授您如何去设置或重设 cookie 的方法，还有如何访问它们及如何删除它们.

> JSP Cookie 处理需要对中文进行编码与解码，方法如下：
>
> ```
> String   str   =   java.net.URLEncoder.encode("中文", "UTF-8");            //编码
> String   str   =   java.net.URLDecoder.decode("编码后的字符串","UTF-8");   // 解码
> ```

‍

### Servlet Cookie 方法

下表列出了 Cookie 对象中常用的方法：

|**序号**|**方法**  **&amp;**  **描述**|
| ----| ----------------------------------------------------------------------------------|
|1|**public void setDomain(String pattern)** 设置 cookie 的域名，比如 runoob.com|
|2|**public String getDomain()** 获取 cookie 的域名，比如 runoob.com|
|3|**public void setMaxAge(int expiry)** 设置 cookie 有效期，以秒为单位，默认有效期为当前session的存活时间|
|4|**public int getMaxAge()** 获取 cookie 有效期，以秒为单位，默认为-1 ，表明cookie会活到浏览器关闭为止|
|5|**public String getName()** 返回 cookie 的名称，名称创建后将不能被修改|
|6|**public void setValue(String newValue)** 设置 cookie 的值|
|7|**public String getValue()** 获取cookie的值|
|8|**public void setPath(String uri)** 设置 cookie 的路径，默认为当前页面目录下的所有 URL，还有此目录下的所有子目录|
|9|**public String getPath()** 获取 cookie 的路径|
|10|**public void setSecure(boolean flag)** 指明 cookie 是否要加密传输|
|11|**public void setComment(String purpose)** 设置注释描述 cookie 的目的. 当浏览器将 cookie 展现给用户时，注释将会变得非常有用|
|12|**public String getComment()** 返回描述 cookie 目的的注释，若没有则返回 null|

‍

‍

### 使用 JSP 设置 cookie

使用 JSP 设置 cookie 包含三个步骤：

 **(1)创建一个 cookie 对象：**  调用 cookie 的构造函数，使用一个 cookie 名称和值做参数，它们都是字符串.

```
Cookie cookie = new Cookie("key","value");
```

请务必牢记，名称和值中都不能包含空格或者如下的字符：

```
[ ] ( ) = , " / ? @ : ;
```

 **(2) 设置有效期：** 调用 setMaxAge() 函数表明 cookie 在多长时间（以秒为单位）内有效. 下面的操作将有效期设为了 24 小时.

```
cookie.setMaxAge(60*60*24); 
```

 **(3) 将 cookie 发送至 HTTP 响应头中：** 调用 response.addCookie() 函数来向 HTTP 响应头中添加 cookie.

```
response.addCookie(cookie);
```

‍

‍

### session对象

除了以上几种方法外，JSP利用servlet提供的HttpSession接口来识别一个用户，存储这个用户的所有访问信息.

默认情况下，JSP允许会话跟踪，一个新的HttpSession对象将会自动地为新的客户端实例化. 禁止会话跟踪需要显式地关掉它，通过将page指令中session属性值设为false来实现，就像下面这样：

```
<%@ page session="false" %>
```

JSP引擎将隐含的session对象暴露给开发者. 由于提供了session对象，开发者就可以方便地存储或检索数据.

下表列出了session对象的一些重要方法：

|**S.N.**|**方法**  **&amp;**  **描述**|
| ----| ------------------------------------------------------------------------|
|1|**public Object getAttribute(String name)** 返回session对象中与指定名称绑定的对象，如果不存在则返回null|
|2|**public Enumeration getAttributeNames()** 返回session对象中所有的对象名称|
|3|**public long getCreationTime()** 返回session对象被创建的时间， 以毫秒为单位，从1970年1月1号凌晨开始算起|
|4|**public String getId()** 返回session对象的ID|
|5|**public long getLastAccessedTime()** 返回客户端最后访问的时间，以毫秒为单位，从1970年1月1号凌晨开始算起|
|6|**public int getMaxInactiveInterval()** 返回最大时间间隔，以秒为单位，servlet 容器将会在这段时间内保持会话打开|
|7|**public void invalidate()** 将session无效化，解绑任何与该session绑定的对象|
|8|**public boolean isNew()** 返回是否为一个新的客户端，或者客户端是否拒绝加入session|
|9|**public void removeAttribute(String name)** 移除session中指定名称的对象|
|10|**public void setAttribute(String name, Object value)**  使用指定的名称和值来产生一个对象并绑定到session中|
|11|**public void setMaxInactiveInterval(int interval)** 用来指定时间，以秒为单位，servlet容器将会在这段时间内保持会话有效|

‍

‍

‍

‍

‍

‍

‍

‍

‍

‍

### Cooke存储路径

‍

在进行Cookie操作时，必须注意Cookie的保存路径，如果一个Cookie的保存路径被设置为“/”，则表示该Cookie可以在WEB中的任意目录下访问，如果一个Cookie的保存路径设置为“/pages”，则表示只允许“/pages”路径下的所有程序进行访问

‍

操作方法

‍

|方法|所属对象|描述|
| --------------------------------------| ----------| ------------------|
|public Cookie[]getCookies()|request|获取全部Cookie|
|public voidaddCookie(Cookie  cookie)|response|设置客户端Cookie|

‍

设置Cookie

```html
<%
    Cookie cookie = new Cookie("message", "www.yootk.com"); // 实例化了Cookie类对象
    response.addCookie(cookie); // 进行Cookie响应
%>
```

‍

‍

获取cookie

所有在客户端浏览器中所设置的Cookie数据信息，全部都是在每次客户端发出请求的时候自动发送到服务器端上的，所以在  
服务器端上如果要想接收全部的Cookie，那么就必须依靠HttpServletRequest接口提供的方法来实现.

‍

```html
<body>
<%
    Cookie [] cookies = request.getCookies(); // 获取全部的客户端发送的Cookie数据
    for (Cookie cookie : cookies) { // foreach循环结构输出
%>
        <%=cookie.getName()%> = <%=cookie.getValue()%>
<%
    }
%>
</body>
```

‍

通过当前的程序执行的结果来看，已经可以明确的获取到了所有的 Cookie 数据信息，之所以服务器端可以获取到全部的  
Cookie数据内容，主要是在每次发送HTTP请求的时候会自动随着请求头信息一起将Cookie数据发送到服务器端上，这样才可  
以实现接收，而getCookies()方法是针对于这些请求头信息处理之后的简单调用.

关闭浏览器就找不到之前的信息了

‍

‍

设置有效时间

```html
<body>
<%
    Cookie cookieA = new Cookie("message", "www.yootk.com"); // 实例化了Cookie类对象
    Cookie cookieB = new Cookie("information", "edu.yootk.com"); // 实例化了Cookie类对象
    cookieA.setMaxAge(3600);
    cookieB.setMaxAge(3600);

    response.addCookie(cookieA); // 进行Cookie响应
    response.addCookie(cookieB); // 进行Cookie响应
%>
</body>
```

此时设置了Cookie数据的保存时间，而后可以直接观察到浏览器中所保存Cookie失效时间就存在了，  
只要一过这个时间,  
那么所设置的Cookie数据就将彻底的消失掉. ·

‍

‍

设置生效路径

在整个的WEB开发过程之中，如果一个Cookie设置为父路径生效，那么对应的所有子路径都可以获取  
到这些Cookie的内  
容，但是如果你把Cookie设置为子路径生效，那么父路径是无法获取的.

```html
    cookieA.setPath("/"); // 设置生效路径
    cookieB.setPath("/yootk/message/"); // 生效路径
```

如果说现在“get_cookie.jsp”页面处于根路径下  
那么其只能够匹配到存储路径为“”下的全部的Cookie 数据，但是无  
法获取到全部的子路径下的Cookie 内容，而如果将“get_cookie.jsp”页面保存在了“/yootk/message"  
路径之中，那么继续  
观察可以获取到的全部Cookie内容. ·

‍

在以后的开发中如果某些Cookie信息只能够在特定的路径下访问的时候你就配置Path，而如果希望C  
.ookie可以被所有的  
路径所访问，那么就可以将访问路径设置为“/”（默认形式).

‍

‍

## Session

‍

session描述的是一个会话的概念，之所以会存在有session的概念，主要的目的是为了解决HTTP协议的无状态问题，在HTTP协议之中，是基于TCP协议实现的可靠数据传输，所以TCP协议本身是存在有严重的性能问题(未来的HTTP协议有可能会基于UDP重写)，同时为了保证性能服务端是不会保留有用户状态的内容的，即:同样的客户，第一次发送请求和第N次发送请求来讲，服务器都会认为是一个新的用户.

如果要想去考虑WEB访问量，那么一般会存在有UV和PV两个概念，UV指的是用户的访问  
量(真实的访问量)，PV指的是  
页面的打开次数，一个人如果要是打开了100个页面，那么这个PV的指就是100，而UV就只  
是一个1. .

于是为了解决这种HTTP协议没有状态的问题，所以在HTTP协议里面提供了session的  
概念，基于session通过各种的算  
法机制来使得服务器可以清楚的知道不同的访问者（专业描述:有状态). ·

在WEB容器中为了方便的区分不同的用户，每一个用户都有一个对应的session（会话）对象，当用户通过浏览器向WEB服务器发出访问请求时，服务器会自动对该用户的session状态进行维护，不同客户端的访问用户拥有各自的session对象，不同的session之间彼此隔离，不允许直接访问

‍

session继承结构

‍

在JSP的开发中提供有了“session”的内置对象，此对象对应的接口类型为“jarkarta.set  
rvlet.http.HttpSession”，这个接  
口不像HttpServletRequest或者是HttpServletResponse接口那样存在有继承的关系，仅仅是  
一个独立的接口，因为只有HTTP  
协议之中存在有session的概念. .

JSP中为便于用户状态管理提供了session内置对象，该对象对应的接口类型为“javax.servlet.http.HttpSession”，由于session属于http协议定义范畴，所以HttpSession是一个独立的接口没有任何的继承关系

‍

‍

### 工作原理

‍

‍

session在整个的WEB开发中是用来进行用户身份标识的，但是既然HTTP是无状态的操  
作形式，那么它又是如何实现这种  
身份标识的呢?实际上这种标识的实现依靠的是  
Cookie (头信息）以及服务器端的算法机制实现的. “

‍

在之前讲解四种属性范围的时候已经强调过了，session的属性范围会在每次浏览器关闭之  
后自动销毁，之所以会出现这样的  
情况，主要的原因是在于Cookie数据的丢失，在整个的WEB 容器之中，为了便于Session的  
保存，实际上针对于不同的Session  
都会提供有一个SessionIlD 的信息，这个信息是由WEB容器自动生成的，并且不会有任何重复  
的ID产生，在HttpSession接口  
里面并没有提供SessionID的生成操作，但是却提供了一个SessionlD的获取操作方法public String getId()

```html
SessionID = <%=session.getId()%>
```

‍

现在可以发现，原来在Cookie里面自动保存的JSESSIONID的信息就是整个WEB容器为当前用户所分配的SessionlD，实际上在每一次该用户进行请求的时候,这个Cookie的内容都会自动的发送到服务器端上,这个时候在服务器端去找到JSESSIONID的内容，而后与在服务器端保存的SESSIONID进行匹配.

‍

但是需要注意的是，JSESSIONID并不是长期保存在客户端浏览器的Cookie之中的，它是  
在每次发送请求并且有了回应之后  
才会自动的设置在客户端浏览器之中(第一次请求是没有SESSIONID)，所以服务器就会自动的  
生成一个新的SessionlD，而后再  
将其保存在客户端浏览器之中，那么也就可以在项目开发中利用这样的机制来判断某一个  
用户是否为第一次访问，所以在  
HttpSession接口里面提供有一个判断新用户的处理方法: public boolean isNew();

```html
SessionID = <%=session.isNew()%>
```

‍

在传统的开发过程之中，SessionlD一旦分配了，那么一般就都不允许被改变了，但是需要  
注意的是，在Servlet 3.1版本里  
面追加了一个更改SessionID方法: public String changeSessionld()

```html
<h1>【原始】SessionID = <%=session.getId()%></h1>
<h1>【变更】SessionID = <%=request.changeSessionId()%></h1>
```

‍

在后续讲解的时候实际上还会针对于这个  
changeSessionld()方法进行更多的操作分析，现阶段需要掌握的概念就是︰  
Session是依靠Cookie实现的用户状态维持，如果Cookie被禁用了，那么Session也是无法  
的使用，后来有了一个Struts 1.x  
开发框架简化了这些操作.

‍

#### Session数据存储结构

每一个数据都通过一个Map集合的形式表示，其中Map的Key是一个SessionID标志，而Map对应的value则是另外一个Map集合

‍

#### 范例

是不是新用户

```html
<%@ page pageEncoding="UTF-8" %>
<%  // 方法定义：public boolean isNew()
    if (session.isNew()) {  			// 不存在JSESSIONID的Cookie
%><h1>用户是第一次访问，为其分配的SessionID为：<%=session.getId()%></h1>
<% } else {    					// 存在JSESSIONID的Cookie
%> <h1>欢迎再次访问，可以获取Cookie中的JSESSIONID：<%=session.getId()%></h1>
<% } %>

```

‍

### 线程池

‍

```html
<h1>【SessionID与线程池】<%=session.getId()%> = <%=Thread.currentThread().getName()%></h1>
```

在整个的Session处理机制之中，用户所发送的每一个请求，如果要想进行处理，都需要通i过Tomcat进行线程资源的分配，也就是说在整个的Tomcat之中是存在有一个预设的线程池配置的，如果在没有做出任何的修改之前，这个线程池可以实现的线程个数是有限的，但是在实际的开发之中，一般的线程池的大小可以设置为“内核线程数量*2”,在讲解多线程的时候分析过内核数量以及超线程的概念，一个CPU内核最多设置2个物理线程. 如果你要想知道自己的电脑硬件的CPU内核数量，可以直接通过Runtime类的方法来获取.

每一个用户的请求在WEB容器中都会通过一个Session实例化对象进行描述，这样在服务端就需要同时维护若干个Session线程，为了便于Session管理，在Tomcat内部会默认创建一个线程池保存所有的可用资源，这样就可以保证在高并发状态下，每一个请求的正确处理

```html
<h1>【主机CPU内核数量】<%=Runtime.getRuntime().availableProcessors()%></h1>
```

‍

如果要想实现线程池的大小配置，那么可以修改“${TOMCAT_HOME}/conf/server.xml”配置文件实现

```html
    <Connector port="80" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" 
			   maxThreads="16" minSpareThreads="16" acceptCount="32"/>
```

‍

三个末尾参数含义

* 设置并发处理的最大线程个数
* 初始化线程池大小
* 阻塞队列中的线程个数

‍

现在为止整个的Tomcat就可以扩大其处理的并发访问量，这个线程池的大小只是一种预估的状态，如果在服务器的设计之中，要想考虑到服务器的调优操作，那么一般情况下都会使用的线程分配模式都是“物理线程2”，但是最终是否可以这样还是需要各种压力测试工具经过测试之后才能够获取到某一台主机的最佳性能.

而此时Tomcat可以处理的最大的并发访问数量为: maxThreads + acceptCount. .良好的优化=主机硬件支持＋服务软件的调优＋优秀的程序代码编写.

‍

‍

### 登录认证

‍

分析了半天，发现Session就是进行用户信息的描述，不管你最终的session的实现机制是什么  
么，主要的作用就是表示存储  
用户自己的信息，实际上在整个的session内置对象里面，最为重要的处理操作形式就是session属  
性操作，因为session的属性  
范围一旦设置之后，不管进行如何的跳转操作，最终该属性都会被保留下来，只有当浏览器关闭之后  
属性才会彻底的消失. 基于这  
样的特点就可以使用session来实现用户的登录认证处理. ‘

‍

额外的话题:所有的系统几乎都离不开认证授权，而认证主要就是判断用户的身份信息，而授权主要是进行角色与权限的排查，而现在所实现的登录认证仅仅是对用户身份的排查，例如:按照单机版的应用来讲，最为常见的设计框架:SpringSecurity.Shiro，而在前后端分离的设计之中，可以使用OAuth2或者是JWT来完成.

session的主要作用是保存用户的操作状态，在实际项目开发中主要是通过session实现登录认证处理，现在假设项目中存在有一些重要的页面，并且这些页面必须要求用户登录后才可以访问，那么这样的功能就可以通过session的属性操作来实现

‍

#### 认证检测代码清单

|No.|文件名称|类型|描述|
| -----| -------------| ------| -----------------------------------------------------------------------------------------------------------|
|1|login.jsp|JSP|用户登录表单，提供用户名和密码输入组件，同时显示登录失败信息|
|2|check.jsp|JSP|固定信息认证检测，登录成功设置session属性并跳转到welcome.jsp，如果登录认证失败，则跳转到login.jsp重新登录|
|3|welcome.jsp|JSP|程序首页，如果未登录的用户将提示错误信息并跳转到login.jsp页面，如果是已登录的用户则可以直接获取欢迎信息.|
|4|logout.jsp|JSP|用户注销操作，使当前session失效并跳转到login.jsp重新登录|

‍

‍

#### 登录表单页login.jsp

关注uname和upass

```html
<body>
<%
    String error = request.getParameter("error"); // 保存错误信息
    if (!(error == null || "".equals(error))) { // 现在有数据
%>
        <h1>登录失败，错误的用户名或密码！</h1>
<%
    }
%>
<form action="check.jsp" method="post">
    用户名：<input type="text" name="uname" value="muyan"><br>
    密码：<input type="password" name="upass" value="yootk"><br>
    <button type="submit">登录</button><button type="reset">重置</button>
</form>
</body>
```

‍

‍

#### 鉴权页面check.jsp

```html
<body>
<%
    String uname = request.getParameter("uname"); // 获取登录用户名
    String upass = request.getParameter("upass"); // 获取登录密码
    // 当前系统之中的用户认证信息是固定的，用户名为muyan，密码为yootk
    if ("muyan".equals(uname) && "yootk".equals(upass)) {   // 用户登录合法
        response.setHeader("refresh", "2;url=welcome.jsp"); // 2秒后自动跳转到welcome.jsp页面
        // 将当前的用户名保存在session属性范围之中，不管如何跳转，只要不关闭浏览器都可以获取该数据
        session.setAttribute("id", uname);
%>
        <h1>用户登录成功，欢迎您的光临，2秒后跳转到欢迎页！</h1>
<%
    } else {    // 错误的用户名或密码
        response.setHeader("refresh", "2; url=login.jsp?error=yootk.com");
%>
        <h1>用户登录失败，错误的用户名或密码！</h1>
<%
    }
%>
</body>
```

‍

#### 首页welcome.jsp

```html
<body>
<%
    if (session.getAttribute("id") != null) {   // 现在存在有session属性
%>
        <img src="images/logo.png" style="width:200px;">
        <h1>欢迎您的访问，请为自己认真学习编程技术，<a href="logout.jsp">系统注销</a></h1>
<%
    } else {    // 现在没有登录过
        response.setHeader("refresh", "2;url=login.jsp");
%>
        <h1>非法用户，不允许进行程序访问！</h1>
<%
    }
%>
</body>
```

‍

#### 系统注销logout.jsp

理论上如果要进行注销操作，直接进行id的session属性的删除操作即可

但是如果你现在还保留有其他的 session属性，那么这样的注销操作就没有任何意义了，最佳的做法是直  
接让整个的session失效，在  
HttpSession接口里面提供有一个专属的失效操作方法: public void invalidate() .

```html
<body>
<%
    session.invalidate(); // 系统注销
%>
<h1>您已成功注销，下次再见，<a href="login.jsp">重新登录</a></h1>
</body>
```

‍

‍

### 验证码

在项目引入动态的验证码，在每次进行登录验证前先进行验证码的输入校验，如果验证码输入正确，才进行用户名和密码的检验，同时为了保证验证码的安全，往往通过图片的方式进行显示

‍

验证码生成（页面名称：image.jsp）

```html
<%@ page pageEncoding="UTF-8"%> 			<%-- 设置页面编码 --%>
<%@ page contentType="image/jpeg"%>                	<%-- MIME显示风格为图片 --%>
<%@ page import="java.awt.*,java.awt.image.*,java.util.*,javax.imageio.*"%> 
<%!Color getRandColor(int fc, int bc) {    		// 获取随机颜色
      Random random = new Random();
      if (fc > 255) { 				// 设置颜色边界
         fc = 255;
      }
      if (bc > 255) { 				// 设置颜色边界
         bc = 255;
      }
      int r = fc + random.nextInt(bc - fc); 	// 随机生成红色数值
      int g = fc + random.nextInt(bc - fc); 	// 随机生成绿色数值
      int b = fc + random.nextInt(bc - fc); 	// 随机生成蓝色数值
      return new Color(r, g, b); 			// 随机返回颜色对象
   }%>
<%
   response.setHeader("Pragma", "No-cache"); 	// 【HTTP1.0】设置页面不缓存
   response.setHeader("Cache-Control", "no-cache");	// 【HTTP1.1】设置页面不缓存
   response.setDateHeader("Expires", 0); 		// 设置缓存失效时间
   int width = 80; 				// 生成图片宽度
   int height = 25; 				// 生成图片高度
   BufferedImage image = new BufferedImage(width, height,
         BufferedImage.TYPE_INT_RGB); 		// 内存中创建图象
   Graphics g = image.getGraphics();		// 获取图形上下文对象
   Random random = new Random();  			// 实例化随机数类
   g.setColor(getRandColor(200, 250));     		// 设定背景色
   g.fillRect(0, 0, width, height); 		// 绘制矩形
   g.setFont(new Font("宋体", Font.PLAIN, 18)); 	// 设定字体
   g.setColor(getRandColor(160, 200)); 		// 获取新的颜色
   for (int i = 0; i < 155; i++) { 		// 产生干扰线
      int x = random.nextInt(width);
      int y = random.nextInt(height);
      int xl = random.nextInt(12);
      int yl = random.nextInt(12);
      g.drawLine(x, y, x + xl, y + yl); 		// 绘制长线
   }
   StringBuffer sRand = new StringBuffer();	// 保存生成的随机数
   // 如果要使用中文，必须定义字库，可以使用数组进行定义，同时必须将中文转换为unicode编码
   String[] str = { "A", "B", "C", "D", "E", "F", "G", "H", "J", "K",
         "L", "M", "N", "P", "Q", "R", "S", "T", "U", "V", "W", "X",
         "Y", "Z", "a", "b", "c", "d", "e", "f", "g", "h", "i", "j",
         "k", "m", "n", "p", "s", "t", "u", "v", "w", "x", "y", "z",
         "1", "2", "3", "4", "5", "6", "7", "8", "9" };
   for (int i = 0; i < 4; i++) { 			// 生成4位随机数
      String rand = str[random.nextInt(str.length)];// 获取随机数
      sRand.append(rand); 				// 随机数保存
      g.setColor(new Color(20 + random.nextInt(110), 20 + random.nextInt(110),
             20 + random.nextInt(110))); 		// 将认证码显示到图象中
      g.drawString(rand, 16 * i + 6, 19); 		// 图形绘制
   }
   session.setAttribute("rand", sRand.toString());	// 将认证码存入SESSION
   g.dispose();					// 图象生效
   ImageIO.write(image, "JPEG", response.getOutputStream());// 输出图象到页面
   out.clear();					// 避免输出冲突
   out = pageContext.pushBody();			// 避免输出冲突
%>
```

实际上现在的开发者根本就没有必要去过多的关注这些验证码的生成操作，这里面最重要的一点  
就是将生成的验证码的数据  
内容直接保存在了session属性范围之中，而属性的名称就是“rand”，在以后进行登录认证之前，首  
先先进行验证码的检查. .

‍

登录页引入验证码（页面名称：login.jsp）

表单之中追加验证码信息

```html
<body>
<%
    String error = request.getParameter("error"); // 保存错误信息
    if (!(error == null || "".equals(error))) { // 现在有数据
%>
        <h1>登录失败，错误的用户名或密码！</h1>
<%
    }
%>
<form action="check.jsp" method="post">
    用户名：<input type="text" name="uname" value="muyan"><br>
    密码：<input type="password" name="upass" value="yootk"><br>
    验证码：<input type="text" maxlength="4" size="4" name="code"><img src="image.jsp"><br>
    <button type="submit">登录</button><button type="reset">重置</button>
</form>
</body>
```

‍

登录验证页（页面名称：check.jsp）

此时用户会将输入的验证码以code 名称的参数提交到服务器端上，而后服务器端通过“chec  
k.jsp”页面来进行生成验证码  
与输入验证码的匹配，而且这个匹配是不区分大小写的操作. .

```html
<%
    String code = request.getParameter("code"); //获取用户输入的验证码
    String rand = session.getAttribute("rand").toString(); //获取生成的验证码
    if (!rand.equalsIgnoreCase(code)) {//验证码不匹配:
%>
    <jsp : forward page="login.jsp" />
<%
}
%>
```

可以发现在进行用户名和密码的验证之前，都会首先进行验证码的检查，如果验证码输入错误,则会跳转到login.jsp页面(如果你有需要，也可以自己设置一些错误信息的展示)，而如果验证码输入正确的时候才会继续进行用户名和密码的匹配，这样一来就可以极大的保护登录系统的安全

‍

‍

‍

‍

## 过滤器

‍

过滤器是一个实现了 javax.servlet.Filter 接口的 Java 类. javax.servlet.Filter 接口定义了三个方法：

|序号|方法 & 描述|
| ------| -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|1|**public void doFilter (ServletRequest, ServletResponse, FilterChain)** <br />该方法完成实际的过滤操作，当客户端的请求与过滤器设置的 URL 匹配时，Servlet 容器将先调用过滤器的 doFilter 方法. FilterChain 用于访问后续过滤器.|
|2|**public void init(FilterConfig filterConfig)** <br />web 应用程序启动时，web 服务器将创建Filter 的实例对象，并调用其init方法，读取web.xml配置，完成对象的初始化功能，从而为后续的用户请求作好拦截的准备工作（filter对象只会创建一次，init方法也只会执行一次）. 开发人员通过init方法的参数，可获得代表当前filter配置信息的FilterConfig对象.|
|3|**public void destroy()** <br />Servlet容器在销毁过滤器实例前调用该方法，在该方法中释放Servlet过滤器占用的资源.|

‍

‍

### FilterConfig 使用

Filter 的 init 方法中提供了一个 FilterConfig 对象.

如 web.xml 文件配置如下：

```
<filter>
    <filter-name>LogFilter</filter-name>
    <filter-class>com.runoob.test.LogFilter</filter-class>
    <init-param>
        <param-name>Site</param-name>
        <param-value>菜鸟教程</param-value>
    </init-param>
    </filter>
```

在 init 方法使用 FilterConfig 对象获取参数：

```
public void  init(FilterConfig config) throws ServletException {
    // 获取初始化参数
    String site = config.getInitParameter("Site"); 
    // 输出初始化参数
    System.out.println("网站名称: " + site); 
}
```

‍

‍

‍

# 实操

‍

## Bugfix

‍

### 浏览器编码问题

‍

谷歌浏览器的问题, 安装谷歌插件`Set Character Encoding`​后就可以了

再次排查, 电脑已经是UTF8全球支持的电脑不需要再重新构建传参来调整编码集.

‍

指这个

```html
	//String a;
	//a=new String(paramMsg.getBytes("UTF-8"),"UTF-8"); //重构为UTF-8
```

```html
<head>
	<title>www.sk.com</title>
	<!-- <meta charset="UTF-8"> -->
</head>
```

‍

### 警告信息解决

‍

解决这个问题，最佳的做法是将Tomcat之中的"jsp-api.jar"、"servlet-api.jar”配置到当前的项目之中

项目配置添加模块
