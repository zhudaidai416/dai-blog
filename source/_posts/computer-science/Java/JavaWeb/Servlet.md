---
title: Servlet
date: 2025-08-03 20:54:10
category:
  - [计算机与科学, Java, JavaWeb]
tags: JavaWeb
cover: https://daiblog.oss-cn-chengdu.aliyuncs.com/cover/6-8.jpg
---

# 概述

Servlet 是 JavaWeb 最为核心的内容，它是 Java 提供的一门<font color=red>动态</font> web 资源开发技术

![image-20250804100002700](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250804100002700.png)

- 使用 Servlet 就可以实现，根据不同的登录用户在页面上动态显示不同内容
- Servlet 是 JavaEE 规范之一，其实就是一个接口，将来我们需要定义 Servlet 类实现 Servlet 接口，并由 web 服务器运行 Servlet

# 快速入门

需求：编写一个 Servlet 类，并使用 IDEA 中 Tomcat 插件进行部署，最终通过浏览器访问所编写的 Servlet 程序

步骤：

1、创建 Web 项目 `web-demo`，导入 Servlet 依赖坐标

```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>

    <!--
      此处为什么需要添加该标签?
      provided指的是在编译和测试过程中有效，最后生成的war包时不会加入
      因为Tomcat的lib目录中已经有servlet-api这个jar包，如果在生成war包的时候生效就会和Tomcat中的jar包冲突，导致报错
    -->
    <scope>provided</scope>
</dependency>
```

2、创建：定义一个类，实现 Servlet 接口，并重写接口中所有方法，并在 service 方法中输入一句话

```java
package com.itheima.web;

import javax.servlet.*;
import javax.servlet.annotation.WebServlet;
import java.io.IOException;

@WebServlet("/demo1")
public class ServletDemo1 implements Servlet {

    @Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("servlet hello world~");
    }

    @Override
    public String getServletInfo() {
        return "";
    }

    @Override
    public void destroy() {

    }
}
```

3、配置：在类上使用 @WebServlet 注解，配置该 Servlet 的访问路径

```java
@WebServlet("/demo1")
```

4、访问：启动 Tomcat，浏览器中输入 URL 地址访问该 Servlet

```
http://localhost:8080/web-demo/demo1
```

5、浏览器访问后，在控制台会打印 `servlet hello world~` 说明 servlet 程序已经成功运行

# 执行流程

> 思考：Servlet 程序已经能正常运行，并没有创建 ServletDemo1 类的对象，也没有调用对象中的 service 方法，为什么在控制台就打印了 `servlet hello world~` 这句话呢？

![image-20250804111646528](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250804111646528.png)

1、浏览器发出 `http://localhost:8080/web-demo/demo1` 请求

从请求中可以解析出三部分内容，分别是 `localhost:8080`、`web-demo`、`demo1`

- localhost:8080：可以找到要访问的 Tomcat Web 服务器
- web-demo：可以找到部署在 Tomcat 服务器上的 web-demo 项目
- demo1：可以找到要访问的是项目中的哪个 Servlet 类，根据 @WebServlet 后面的值进行匹配

2、找到 ServletDemo1 这个类后，Tomcat Web 服务器就会为 ServletDemo1 这个类创建一个对象，然后调用对象中的 service 方法

- ServletDemo1 实现了 Servlet 接口，所以类中必然会重写 service 方法供 Tomcat Web 服务器进行调用

- service 方法中有 ServletRequest 和 ServletResponse 两个参数

  ServletRequest 封装的是请求数据

  ServletResponse 封装的是响应数据

  后期我们可以通过这两个参数实现前后端的数据交互

> 1、Servlet 由谁创建？Servlet 方法由谁调用？
>
> Servlet 由 web 服务器创建，Servlet 方法由 web 服务器调用
>
> 2、服务器怎么知道 Servlet 中一定有 service 方法？
>
> 因为我们自定义的 Servlet，必须实现 Servlet 接口并复写其方法，而 Servlet 接口中有 service 方法

# 生命周期

生命周期: 对象的生命周期指一个对象从被创建到被销毁的整个过程

Servlet 运行在 Servlet 容器（web 服务器）中，其生命周期由容器来管理，分为 4 个阶段：

- <font color=red>加载和实例化</font>：默认情况下，当 Servlet 第一次被访问时，由容器创建 Servlet 对象
- <font color=red>初始化</font>：在 Servlet 实例化之后，容器将调用 Servlet 的 <font color=red>init()</font> 方法初始化这个对象，完成一些如加载配置文件、创建连接等初始化的工作。该方法<font color=red>只调用一次</font>
- <font color=red>请求处理</font>：每次请求 Servlet 时，Servlet 容器都会调用 Servlet 的 <font color=red>service()</font> 方法对请求进行处理
- <font color=red>服务终止</font>：当需要释放内存或者容器关闭时，容器就会调用 Servlet 实例的 <font color=red>destroy()</font> 方法完成资源的释放。在 destroy()方法调用之后，容器会释放这个 Servlet 实例，该实例随后会被 Java 的垃圾收集器所回收

```xml
默认情况，Servlet会在第一次访问被容器创建，但是如果创建Servlet比较耗时的话，那么第一个访问的人等待的时间就比较长，用户的体验就比较差，那么我们能不能把Servlet的创建放到服务器启动的时候来创建，具体如何来配置?

@WebServlet(urlPatterns = "/demo1",loadOnStartup = 1)
loadOnstartup的取值有两类情况
➡︎ 负整数（默认值-1）：第一次访问时创建Servlet对象
➡︎ 0或正整数：服务器启动时创建Servlet对象，数字越小优先级越高
```

**代码演示**：

```java
package com.itheima.web;

import javax.servlet.*;
import javax.servlet.annotation.WebServlet;
import java.io.IOException;

/**
 * Servlet生命周期方法
 */
@WebServlet(urlPatterns = "/demo2", loadOnStartup = 1)
public class ServletDemo2 implements Servlet {
    /**
     * 初始化方法
     * 调用时机：默认情况下，Servlet被第一次访问时，调用
     * * loadOnStartup: 默认为-1，修改为0或者正整数，则会在服务器启动的时候，调用
     * 调用次数：1次
     */
    @Override
    public void init(ServletConfig servletConfig) throws ServletException {
        System.out.println("初始化init...");
    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    /**
     * 提供服务
     * 调用时机：每一次Servlet被访问时，调用
     * 调用次数：多次
     */
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("servlet hello world~");
    }

    @Override
    public String getServletInfo() {
        return "";
    }

    /**
     * 销毁方法
     * 调用时机：内存释放或者服务器关闭的时候，Servlet对象会被销毁，调用
     * 调用次数：1次
     */
    @Override
    public void destroy() {
        System.out.println("销毁destroy...");
    }
}
```

> :warning: 注：如何才能让 Servlet 中的 destroy 方法被执行？
>
> 在 Terminal 命令行中，先使用 `mvn tomcat7:run` 启动，再使用 `ctrl+c` 关闭 tomcat
>
> ![image-20250804134724568](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250804134724568.png)

# 方法介绍

```java
// 初始化方法：在Servlet被创建时执行，只执行一次
void init(ServletConfig config)

// 提供服务方法：每次Servlet被访问，都会调用该方法
void service(ServletRequest req, ServletResponse res)

// 销毁方法：当Servlet被销毁时，调用该方法。在内存释放或服务器关闭时销毁Servlet
void destroy()

// 获取Servlet信息
// 该方法用来返回Servlet的相关信息，没有什么太大的用处，一般我们返回一个空字符串即可
String getServletInfo() {
  return "";
}

// 获取ServletConfig对象
ServletConfig getServletConfig()
```

> ServletConfig 对象，在 init 方法的参数中有，而 Tomcat Web 服务器在创建 Servlet 对象的时候会调用 init 方法，必定会传入一个 ServletConfig 对象，我们只需要将服务器传过来的 ServletConfig 进行返回即可
>
> ```java
> package com.itheima.web;
>
> import javax.servlet.*;
> import javax.servlet.annotation.WebServlet;
> import java.io.IOException;
>
> /**
>  * Servlet方法介绍
>  */
> @WebServlet(urlPatterns = "/demo3", loadOnStartup = 1)
> public class ServletDemo3 implements Servlet {
>     private ServletConfig config;
>
>     @Override
>     public void init(ServletConfig servletConfig) throws ServletException {
>         this.config = servletConfig;
>         System.out.println("初始化init...");
>     }
>
>     @Override
>     public ServletConfig getServletConfig() {
>         return config;
>     }
>
>     @Override
>     public String getServletInfo() {
>         return "";
>     }
>
>     // 省略其他方法...
> }
>
> ```

# 体系结构

![image-20250804140112826](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250804140112826.png)

因为我们将来开发 B/S 架构的 web 项目，都是针对 HTTP 协议，所以我们自定义 Servlet，会通过继承 HttpServlet

## 使用步骤

- 继承 HttpServlet

- 重写 doGet 和 doPost 方法

```java
package com.itheima.web;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/demo4")
public class ServletDemo4 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // TODO GET 请求方式处理逻辑
        System.out.println("继承HttpServlet的get...");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // TODO Post 请求方式处理逻辑
        System.out.println("继承HttpServlet的post...");
    }
}
```

**测试**：

- 发送 GET 请求：通过浏览器发送 `http://localhost:8080/web-demo/demo4`，就能看到 doGet 方法被执行了

- 发送 POST 请求：单单通过浏览器是无法实现的，需要编写一个 form 表单来发送请求，在 webapp 下创建一个 `a.html` 页面

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <title>Title</title>
    </head>
    <body>
      <form action="/web-demo/demo4" method="post">
        <input name="username" /><input type="submit" />
      </form>
    </body>
  </html>
  ```

  启动测试 `http://localhost:8080/web-demo/a.html`，提交表单即可看到 doPost 方法被执行了

## HttpServlet 原理

获取请求方式，并根据不同的请求方式，调用不同的 doXxx 方法

> 思考：
>
> - HttpServlet 中为什么要根据请求方式的不同，调用不同的方法？
> - 如何调用？

前端发送 GET 和 POST 请求的时候，参数的位置不一致，GET 请求参数在请求行中，POST 请求参数在请求体中，为了能处理不同的请求方式，得在 service 方法中进行判断，然后写不同的业务处理，但是每个 Servlet 类中都将有相似的代码，如下：

```java
package com.itheima.web;

import javax.servlet.*;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServletRequest;
import java.io.IOException;

@WebServlet("/demo5")
public class ServletDemo5 implements Servlet {
    @Override
    public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
        // 如何调用?
        // 获取请求方式，根据不同的请求方式进行不同的业务处理
        HttpServletRequest request = (HttpServletRequest) req;
        // 1、获取请求方式
        String method = request.getMethod();
        // 2、判断
        if ("GET".equals(method)) {
            // get方式的处理逻辑
        } else if ("POST".equals(method)) {
            // post方式的处理逻辑
        }
    }

    // 省略其他方法...
}

```

进一步简化：对 Servlet 接口进行继承封装

```java
package com.itheima.web;

import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import java.io.IOException;

public class MyHttpServlet implements Servlet {
    @Override
    public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
        HttpServletRequest request = (HttpServletRequest) req;
        // 1、获取请求方式
        String method = request.getMethod();
        // 2、判断
        if ("GET".equals(method)) {
            // get方式的处理逻辑
            doGet(req, res);
        } else if ("POST".equals(method)) {
            // post方式的处理逻辑
            doPost(req, res);
        }
    }

    protected void doGet(ServletRequest req, ServletResponse res) {

    }

    protected void doPost(ServletRequest req, ServletResponse res) {

    }

    // 省略其他方法...
}
```

有了 MyHttpServlet 这个类，再编写 Servlet 类的时候，只需要继承 MyHttpServlet，重写父类中的 doGet 和 doPost 方法，就可以用来处理 GET 和 POST 请求的业务逻辑

ServletDemo5 代码改造如下：

```java
package com.itheima.web;

import javax.servlet.*;
import javax.servlet.annotation.WebServlet;

@WebServlet("/demo5")
public class ServletDemo5 extends MyHttpServlet {
    @Override
    protected void doGet(ServletRequest req, ServletResponse res) {
        System.out.println("自定义MyHttpServlet的get...");
    }

    @Override
    protected void doPost(ServletRequest req, ServletResponse res) {
        System.out.println("自定义MyHttpServlet的post...");
    }
}
```

查看 HttpServlet 源码：搜索 `service()` 方法，会发现 HttpServlet 做的事更多，不仅可以处理 GET 和 POST 还可以处理其他五种请求方式

# urlPattren 配置

Servlet 类编写好后，要想被访问到，就需要配置其访问路径（<font color=red>urlPattern</font>）

urlPattern 总共有四种配置方式，分别是精确匹配、目录匹配、扩展名匹配、任意匹配

五种配置的优先级：`精确匹配 > 目录匹配 > 扩展名匹配 > /* > /`，以最终运行结果为准

一个 Servlet，可以配置多个 urlPattern

```java
@WebServlet({"/demo6", "/demo7"})
```

在浏览器上输入 `http://localhost:8080/web-demo/demo6`、`http://localhost:8080/web-demo/demo7` 这两个地址都能访问到 ServletDemo7 的 doGet 方法

```java
package com.itheima.web;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
* urlPattern：一个Servlet可以配置多个访问路径
*/
@WebServlet(urlPatterns = {"/demo6", "/demo7"})
public class ServletDemo6 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("demo6 get...");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("demo6 post...");
    }
}
```

## 精确匹配

以 `/` 开头，后面跟上 Servlet 的名称

```java
// 例子
配置路径：@WebServlet("/user/select")
访问路径：http://localhost:8080/web-demo/user/select
```

## 目录匹配

以 `/` 开头，以 `/*` 结尾

```java
// 例子
配置路径：@WebServlet("/user/*")
访问路径：http://localhost:8080/web-demo/user/aaa
        http://localhost:8080/web-demo/user/bbb
```

## 扩展名匹配

以 `*.` 开头，后面跟上扩展名

`*.do` 的前面不能加 `/`，否则会报错

```java
// 例子
配置路径：@WebServlet("*.do")
访问路径：http://localhost:8080/web-demo/aaa.do
        http://localhost:8080/web-demo/bbb.do
```

## 任意匹配

`/` 或 `/*`

```java
// 例子
配置路径：@WebServlet("/")
         @WebServlet("/*")
访问路径：http://localhost:8080/web-demo/hehe
        http://localhost:8080/web-demo/haha
```

> :warning: 注：`/` 和 `/*` 的区别
>
> - 当项目中配置了 `/`，会覆盖掉 tomcat 中的 DefaultServlet
>
>   当其他的 url-pattern 都匹配不上时都会走这个 Servlet
>
> - 当项目中配置了 `/*`，意味着匹配任意访问路径
>
> - DefaultServlet 是用来处理静态资源，如果配置了 `/` 会把默认的覆盖掉，就会引发请求静态资源的时候没有走默认的而是走了自定义的 Servlet 类，最终导致静态资源不能被访问

# XML 配置

- Servlet 从 3.0 版本后开始支持使用注解配置

- 3.0 版本前只支持 XML 配置文件的配置方法

XML 的配置步骤：

1、编写 Servlet 类

```java
package com.itheima.web;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class ServletDemo8 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("demo8 get...");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("demo8 post...");
    }
}
```

2、在 web.xml 中配置该 Servlet

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!-- Servlet全类名 -->
    <servlet>
        <servlet-name>demo8</servlet-name>
        <servlet-class>com.itheima.web.ServletDemo8</servlet-class>
    </servlet>
    <!-- Servlet访问路径 -->
    <servlet-mapping>
        <servlet-name>demo8</servlet-name>
        <url-pattern>/demo8</url-pattern>
    </servlet-mapping>
</web-app>
```
