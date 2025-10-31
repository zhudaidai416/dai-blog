---
title: JSP
date: 2025-08-06 16:11:43
category:
  - [计算机与科学, Java, JavaWeb]
tags: JavaWeb
cover: https://daiblog.oss-cn-chengdu.aliyuncs.com/cover/6-10.jpg
---

# 概述

JSP（全称：Java Server Pages）：Java 服务端页面

一种动态的网页技术，其中既可以定义 HTML、JS、CSS 等静态内容，还可以定义 Java 代码的动态内容

JSP = HTML + Java

作用：简化开发，避免了在 Servlet 中直接输出 HTML 标签

# 快速入门

1、 搭建环境：创建一个 maven 的 web 项目

2、导入 JSP 坐标

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>jsp-demo</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>war</packaging>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>

    <dependencies>
      <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
            <scope>provided</scope>
        </dependency>

        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.2</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.tomcat.maven</groupId>
                <artifactId>tomcat7-maven-plugin</artifactId>
                <version>2.2</version>
            </plugin>
        </plugins>
    </build>
</project>
```

3、在 webapp 目录下，创建 jsp 页面

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <h1>hello jsp</h1>
    <%
        System.out.println("hello,jsp~");
    %>
</body>
</html>
```

4、测试：启动服务器并在浏览器地址栏输入 `http://localhost:8080/jsp-demo/hello.jsp`

# 原理

JSP 本质上就是一个 Servlet

JSP 在被访问时，由 JSP 容器（Tomcat）将其转换为 Java 文件（Servlet），再由 JSP 容器（Tomcat）将其编译，最终对外提供服务的其实就是这个字节码文件

![image-20250807101216051](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250807101216051.png)

> - 浏览器第一次访问 `hello.jsp` 页面
> - tomcat 会将 `hello.jsp` 转换为名为 `hello_jsp.java` 的一个 Servlet
> - tomcat 再将转换的 servlet 编译成字节码文件 `hello_jsp.class`
> - tomcat 会执行该字节码文件，向外提供服务

# JSP 脚本

JSP 脚本用于在 JSP 页面内定义 Java 代码

分类：

```jsp
<%...%>：内容会直接放到_jspService()方法之中
<%=…%>：内容会放到out.print()中，作为out.print()的参数
<%!…%>：内容会放到_jspService()方法之外，被类直接包含
```

+++success 查看转换的 `hello_jsp.java` 文件

```java
<%
    System.out.println("hello,jsp~");
    int i = 3;
%>

<%="hello"%>
<%=i%>

<%!
    void show() {}
    String name = "zhangsan";
%>

// JSP 脚本
public final class hello_jsp extends org.apache.jasper.runtime.HttpJspBase implements org.apache.jasper.runtime.JspSourceDependent {
    void  show(){}
    String name = "zhangsan";
    ...
    public void _jspService(final javax.servlet.http.HttpServletRequest request, final javax.servlet.http.HttpServletResponse response) throws java.io.IOException, javax.servlet.ServletException {

        System.out.println("hello,jsp~");
        int i = 3;
        ...
        out.print("hello");
        out.print(i);
        ...
    }
}
```

+++

+++success 案例：使用 JSP 脚本展示品牌数据

![image-20250807152859496](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250807152859496.png)

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ page import="com.itheima.pojo.Brand" %>
<%@ page import="java.util.List" %>
<%@ page import="java.util.ArrayList" %>

<%
    // 查询数据库
    List<Brand> brands = new ArrayList<Brand>();
    brands.add(new Brand(1,"三只松鼠","三只松鼠",100,"三只松鼠，好吃不上火",1));
    brands.add(new Brand(2,"优衣库","优衣库",200,"优衣库，服适人生",0));
    brands.add(new Brand(3,"小米","小米科技有限公司",1000,"为发烧而生",1));

%>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<input type="button" value="新增"><br>
<hr>
<table border="1" cellspacing="0" width="800">
    <tr>
        <th>序号</th>
        <th>品牌名称</th>
        <th>企业名称</th>
        <th>排序</th>
        <th>品牌介绍</th>
        <th>状态</th>
        <th>操作</th>
    </tr>
    <%
        for (int i = 0; i < brands.size(); i++) {
            Brand brand = brands.get(i);
    %>

    <tr align="center">
        <td><%=brand.getId()%></td>
        <td><%=brand.getBrandName()%></td>
        <td><%=brand.getCompanyName()%></td>
        <td><%=brand.getOrdered()%></td>
        <td><%=brand.getDescription()%></td>
		<td><%=brand.getStatus() == 1 ? "启用" : "禁用"%></td>
        <td><a href="#">修改</a> <a href="#">删除</a></td>
    </tr>

    <%
        }
    %>
</table>
</body>
</html>
```

测试：在浏览器地址栏输入 `http://localhost:8080/jsp-demo/brand.jsp`

+++

**缺点**：

- 书写麻烦、阅读麻烦
- 复杂度高
- 占内存和磁盘
- 调试困难
- 不利于团队协作

JSP 已逐渐退出历史舞台

![image-20250807153218306](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250807153218306.png)

使用 EL 表达式和 JSTL 标签库替换 JSP 中的 Java 代码

# EL 表达式

## 概述

EL（全称 Expression Language ）表达式语言，用于简化 JSP 页面内的 Java 代码

主要功能：获取数据

**语法**：

```jsp
${expression}

例如：
${brands}：获取域中存储的key为brands的数据
```

+++success 代码演示

1、定义 servlet，在 servlet 中封装一些数据并存储到 request 域对象中并转发到 `el-demo.jsp` 页面

```java
@WebServlet("/demo1")
public class ServletDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 数据
        List<Brand> brands = new ArrayList<Brand>();
        brands.add(new Brand(1, "三只松鼠", "三只松鼠", 100, "三只松鼠，好吃不上火", 1));
        brands.add(new Brand(2, "优衣库", "优衣库", 200, "优衣库，服适人生", 0));
        brands.add(new Brand(3, "小米", "小米科技有限公司", 1000, "为发烧而生", 1));

        // 存储到request域
        req.setAttribute("brands", brands);

        // 转发到 el-demo.jsp
        req.getRequestDispatcher("/el-demo.jsp").forward(req, resp);
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp);
    }
}
```

2、在 `el-demo.jsp` 中通过 EL 表达式获取数据

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    ${brands}
</body>
</html>
```

3、在浏览器的地址栏输入 `http://localhost:8080/jsp-demo/demo1`

![image-20250807154531270](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250807154531270.png)

+++

## 域对象

JavaWeb 中的四大域对象

- page：当前页面有效
- request：当前请求有效
- session：当前会话有效
- application：当前应用有效

<font color=red>el 表达式获取数据，会依次从这 4 个域中寻找，直到找到为止</font>

四个域对象的作用范围：

![image-20250807112306312](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250807112306312.png)

# JSTL 标签

JSP 标准标签库(Jsp Standarded Tag Library) ，使用标签取代 JSP 页面上的 Java 代码

## 使用

1、导入坐标

```xml
<dependency>
    <groupId>jstl</groupId>
    <artifactId>jstl</artifactId>
    <version>1.2</version>
</dependency>
<dependency>
    <groupId>taglibs</groupId>
    <artifactId>standard</artifactId>
    <version>1.1.2</version>
</dependency>
```

2、在 JSP 页面上引入 JSTL 标签库

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
```

3、使用标签

## if 标签

`<c:if>`：相当于 if 判断

- 属性：test，用于定义条件表达式

```jsp
<c:if test="${flag == 1}">
    男
</c:if>
<c:if test="${flag == 2}">
    女
</c:if>
```

## forEach 标签

`<c:forEach>`：相当于 for 循环

### 用法一

类似于 Java 中的增强 for 循环，涉及到的 `<c:forEach>` 中的属性如下：

- items：被遍历的容器

- var：遍历产生的临时变量

- varStatus：遍历状态对象

```jsp
<c:forEach items="${brands}" var="brand" varStatus="status">
    <tr align="center">
        <td>${status.index}</td> #从0开始计数
        <td>${status.count}</td> #从1开始计数
        <td>${brand.id}</td>
        <td>${brand.brandName}</td>
        <td>${brand.companyName}</td>
        <td>${brand.description}</td>
    </tr>
</c:forEach>
```

### 用法二

类似于 Java 中的普通 for 循环，涉及到的 `<c:forEach>` 中的属性如下：

- begin：开始数

- end：结束数

- step：步长

```jsp
<!-- 从0循环到10，变量名是i，每次自增1 -->
<c:forEach begin="0" end="10" step="1" var="i">
    ${i}
</c:forEach>
```

## 示例

+++success 示例

1、ServletDemo1 类

```java
@WebServlet("/demo1")
public class ServletDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 数据
        List<Brand> brands = new ArrayList<Brand>();
        brands.add(new Brand(1, "三只松鼠", "三只松鼠", 100, "三只松鼠，好吃不上火", 1));
        brands.add(new Brand(2, "优衣库", "优衣库", 200, "优衣库，服适人生", 0));
        brands.add(new Brand(3, "小米", "小米科技有限公司", 1000, "为发烧而生", 1));

        // 存储到request域
        req.setAttribute("brands", brands);
        req.setAttribute("status", 1);

        // 转发到 jstl.jsp
        req.getRequestDispatcher("/jstl.jsp").forward(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp);
    }
}
```

2、jstl.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1 style="color: lightpink">JSTL标签使用 - if标签</h1>
<c:if test="${status == 1}">
    启用
</c:if>

<c:if test="${status == 0}">
    禁用
</c:if>

<h1 style="color: lightpink">JSTL标签使用 - forEach标签</h1>
<input type="button" value="新增"><br>
<hr>
<table border="1" cellspacing="0" width="800">
    <tr>
        <th>index</th>
        <th>序号</th>
        <th>品牌名称</th>
        <th>企业名称</th>
        <th>排序</th>
        <th>品牌介绍</th>
        <th>状态</th>
        <th>操作</th>
    </tr>
    <c:forEach items="${brands}" var="brand" varStatus="status">
        <tr align="center">
            <%--<td>${brand.id}</td>--%>
            <td>${status.index}</td>
            <td>${status.count}</td>
            <td>${brand.brandName}</td>
            <td>${brand.companyName}</td>
            <td>${brand.ordered}</td>
            <td>${brand.description}</td>
            <%--<td>${brand.status == 1 ? "启用" : "禁用"}</td>--%>

            <c:if test="${brand.status  == 1}">
                <td>启用</td>
            </c:if>
            <c:if test="${brand.status  == 0}">
                <td>禁用</td>
            </c:if>

            <td><a href="#">修改</a> <a href="#">删除</a></td>
        </tr>
    </c:forEach>
</table>

</body>
</html>
```

+++

# MVC 和 三层架构

## MVC 模式

MVC 是一种分层开发的模式

- M（Model）：业务模型，处理业务

- V（View）：视图，界面展示

- C（Controller）：控制器，处理请求，调用模型和视图

![image-20250807113729999](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250807113729999.png)

控制器（serlvlet）用来接收浏览器发送过来的请求，控制器调用模型（JavaBean）来获取数据，比如从数据库查询数据

控制器获取到数据后再交由视图（JSP）进行数据展示

**好处**：

- 职责单一，互不影响

- 有利于分工协作

- 有利于组件重用

## 三层架构

三层架构是将项目分成了三个层面：表现层、业务逻辑层、数据访问层

- 数据访问层：对数据库的 CRUD 基本操作

- 业务逻辑层：对业务逻辑进行封装，组合数据访问层层中基本功能，形成复杂的业务逻辑功能

  例如 ：注册业务功能，我们会先调用数据访问层的 `selectByName()` 方法判断该用户名是否存在，如果不存在再调用 数据访问层的 `insert()` 方法进行数据的添加操作

- 表现层：接收请求，封装数据，调用业务逻辑层，响应数据

![image-20250807113959257](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250807113959257.png)

三层架构的每一层都有特有的包名称：

- 表现层：com.itheima.controller 或 com.itheima.web
- 业务逻辑层：com.itheima.service
- 数据访问层：com.itheima.dao 或 com.itheima.mapper

## 区别联系

![image-20250807114712171](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250807114712171.png)

- MVC 模式中的 C（控制器）和 V（视图）就是三层架构中的表现层
- MVC 模式中的 M（模型）就是三层架构中的业务逻辑层和数据访问层

可以将 MVC 模式理解成是一个大的概念，而三层架构是对 MVC 模式实现架构的思想
