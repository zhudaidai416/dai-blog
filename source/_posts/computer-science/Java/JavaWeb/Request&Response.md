---
title: Request&Response
date: 2025-08-04 16:08:39
category:
  - [计算机与科学, Java, JavaWeb]
tags: JavaWeb
cover: https://daiblog.oss-cn-chengdu.aliyuncs.com/cover/6-9.jpg
---

# 概述

- Request 请求对象：获取请求数据
- Response 响应对象：设置响应数据

![image-20250804161657193](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250804161657193.png)

**request 获取请求数据**

- 浏览器会发送 HTTP 请求到后台服务器[Tomcat]
- HTTP 的请求中会包含很多请求数据[请求行+请求头+请求体]
- 后台服务器[Tomcat]会对 HTTP 请求中的数据进行解析并把解析结果存入到一个对象中
- 所存入的对象即为 request 对象，可以从 request 对象中获取请求的相关参数
- 获取到数据后就可以继续后续的业务，比如获取用户名和密码就可以实现登录操作的相关业务

**response 设置响应数据**

- 业务处理完后，后台就需要给前端返回业务处理的结果即响应数据
- 把响应数据封装到 response 对象中
- 后台服务器[Tomcat]会解析 response 对象，按照[响应行+响应头+响应体]格式拼接结果
- 浏览器最终解析结果，把内容展示在浏览器给用户浏览

# Request 对象

## 继承体系

![image-20250804164532784](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250804164532784.png)

ServletRequest 和 HttpServletRequest 是继承关系，并且两个都是接口，接口是无法创建对象

![image-20250805095057031](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250805095057031.png)

- Request 的继承体系为 ServletRequest ➡︎ HttpServletRequest ➡︎ RequestFacade
- Tomcat 需要解析请求数据，封装为 request 对象，并且创建 request 对象传递到 service 方法
- 使用 request 对象，可以查阅 JavaEE API 文档的 HttpServletRequest 接口中方法说明

## 获取请求数据

HTTP 请求数据分为三部分：请求行、请求头、请求体

### 请求行数据

![image-20250805100356596](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250805100356596.png)

```java
// 获取请求方式：GET
String getMethod()

// 动态获取虚拟目录（项目访问路径）
String getContextPath()

// 获取URL（统一资源定位符）
StringBuffer getRequestURL()

// 获取URI（统一资源标识符）
String getRequestURI()

// 获取请求参数（GET方式）
String getQueryString()
```

+++success 示例

```java
package com.itheima.web;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/req1")
public class RequestDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String method = req.getMethod();
        System.out.println("获取请求方式(get): " + method);
        String contextPath = req.getContextPath();
        System.out.println("动态获取虚拟目录(项目访问路径): " + contextPath);
        StringBuffer url = req.getRequestURL();
        System.out.println("获取URL(统一资源定位符): " + url.toString());
        String uri = req.getRequestURI();
        System.out.println("获取URI(统一资源标识符): " + uri);
        String queryString = req.getQueryString();
        System.out.println("获取请求参数(GET方式): " + queryString);
    }
}
```

测试：访问 `http://localhost:8080/request-demo/req1?username=zhangsan&password=123`

运行结果：

![image-20250805105309160](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250805105309160.png)

+++

### 请求头数据

格式为 `key: value`，如下：

![image-20250805103303100](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250805103303100.png)

```java
// 根据请求头名称，获取值
String getHeader(String name)
```

+++success 示例

```java
@WebServlet("/req1")
public class RequestDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 获取请求头: user-agent浏览器的版本信息
        String agent = req.getHeader("user-agent");
        System.out.println("获取请求头user-agent(浏览器的版本信息): " + agent);
    }
}
```

运行结果：

![image-20250805105119402](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250805105119402.png)

+++

### 请求体数据

浏览器在发送 GET 请求的时候是没有请求体的，需要把请求方式变更为 POST，请求体中的数据格式如下：

![image-20250805103659481](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250805103659481.png)

```java
// 获取字节输入流：传递的是文件数据
ServletInputStream getInputStream()

// 获取字符输入流：传递的是纯文本数据
BufferedReader getReader()
```

+++success 示例

```java
@WebServlet("/req1")
public class RequestDemo1 extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 获取post请求体（请求参数）
        BufferedReader br = req.getReader();
        String line = br.readLine();
        System.out.println("获取post请求体:" + line);
    }
}
```

req.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Title</title>
  </head>
  <body>
    <form action="/request-demo/req1" method="post">
      <input type="text" name="username" />
      <input type="password" name="password" />
      <input type="submit" />
    </form>
  </body>
</html>
```

测试：访问 `http://localhost:8080/request-demo/req.html`，并输入提交表单

运行结果：

![image-20250805105738641](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250805105738641.png)

+++

### 通用方式获取请求参数

```java
// GET方式
String getQueryString()

// POST方式
BufferedReader getReader()
```

代码实现：

```java
@WebServlet("/req1")
public class RequestDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String result = req.getQueryString();
        System.out.println(result);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        BufferedReader br = req.getReader();
        String result = br.readLine();
        System.out.println(result);
    }
}
```

> 思考：GET 请求方式和 POST 请求方式区别主要在于获取请求参数的方式不一样，是否可以提供一种统一获取请求参数的方式，从而统一 doGet 和 doPost 方法内的代码？

解决方案一：

```java
@WebServlet("/req2")
public class RequestDemo2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 获取请求方式
        String method = req.getMethod();
        // 获取请求参数
        String params = "";
        if ("GET".equals(method)) {
            params = req.getQueryString();
        } else if ("POST".equals(method)) {
            BufferedReader reader = req.getReader();
            params = reader.readLine();
        }
        // 将请求参数进行打印控制台
        System.out.println(params);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp);
    }
}
```

解决方案二：

request 对象已经将上述获取请求参数的方法进行了封装，并且 request 提供的方法实现的功能更强大，只需要调用 request 提供的方法即可

+++success request 方法中实现的操作

1、根据不同的请求方式获取请求参数，获取的内容如下：

![image-20250805112721511](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250805112721511.png)

2、把获取到的内容进行分割

![image-20250805112740708](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250805112740708.png)

3、把分割后的数据，存入到一个 Map 集合

![image-20250805112821958](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250805112821958.png)

**注意**：因为参数的值可能是一个，也可能有多个，所以 Map 的值的类型为 String 数组

+++

```java
// 获取所有参数Map集合
Map<String,String[]> getParameterMap()

// 根据名称获取参数值（数组）
String[] getParameterValues(String name)

// 根据名称获取参数值（单个值）
String getParameter(String name)
```

+++success 代码演示

1、修改 req.html 页面

```html
<form action="/request-demo/req2" method="get">
  <input type="text" name="username" /><br />
  <input type="password" name="password" /><br />
  <input type="checkbox" name="hobby" value="1" /> 游泳
  <input type="checkbox" name="hobby" value="2" /> 爬山 <br />
  <input type="submit" />
</form>
```

2、Servlet 代码中

```java
@WebServlet("/req2")
public class RequestDemo2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("------ 获取所有参数的Map集合 ------");
        // 1、获取所有参数的Map集合
        Map<String, String[]> map = req.getParameterMap();
        // 快捷：iter
        for (String key : map.keySet()) {
            System.out.print(key + ": ");
            // 获取值
            String[] values = map.get(key);
            for (String value : values) {
                System.out.print(value + " ");
            }
            System.out.println();
        }

        System.out.println("------ 根据key获取参数值（数组）------");
        // 2、根据key获取参数值（数组）
        String[] hobbies = req.getParameterValues("hobby");
        for (String hobby : hobbies) {
            System.out.println(hobby + " ");
        }

        System.out.println("------ 根据名称获取参数值（单个值）------");
        // 3、根据名称获取参数值（单个值）
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        System.out.println("username: " + username);
        System.out.println("password: " + password);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp);
    }
}
```

3、运行结果：

![image-20250805115452423](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250805115452423.png)

+++

## 请求参数中文乱码

解决方案：

- POST：设置输入流的编码

  ```java
  req.setCharacterEncoding("utf-8");
  ```

- 通用方式（GET/POST）：先编码再解码

  ```java
  new String(username.getBytes(StandardCharsets.ISO_8859_1),StandardCharsets.UTF_8);
  ```

Tomcat 8.0 之后，已将 GET 请求乱码问题解决，设置默认的解码方式为 UTF-8

### POST 请求解决方案

POST 请求出现乱码的原因：

- POST 的请求参数是通过 request 的 getReader() 来获取流中的数据
- TOMCAT 在获取流的时候采用的编码是 ISO-8859-1
- ISO-8859-1 编码是不支持中文的，所以会出现乱码

```java
@WebServlet("/req3")
public class RequestDemo3 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 设置字符输入流的编码
        req.setCharacterEncoding("utf-8");
        String username = req.getParameter("username");
        System.out.println(username);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp);
    }
}
```

### GET 请求解决方案

GET 请求出现乱码的原因：

- 浏览器通过 HTTP 协议发送请求和数据给后台服务器（Tomcat）
- 浏览器在发送 HTTP 的过程中会对中文数据进行 URL 编码
- 在进行 URL 编码的时候会采用页面 `<meta>` 标签指定的 UTF-8 的方式进行编码，`张三`编码后的结果为 `%E5%BC%A0%E4%B8%89`
- 后台服务器（Tomcat）接收到 `%E5%BC%A0%E4%B8%89` 后会默认按照 `ISO-8859-1` 进行 URL 解码
- 由于前后编码与解码采用的格式不一样，就会导致后台获取到的数据为乱码

![image-20250805135047537](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250805135047537.png)

**URL 编码**

- 将字符串按照编码方式转为二进制
- 每个字节转为 2 个 16 进制数并在前边加上%

```java
"张三"按照UTF-8的方式转换成二进制
1110 0101 | 1011 1100 | 1010 0000 | 1110 0100 | 1011 1000 | 1000 1001
% E    5  %   B    C  %   A    0  %   E    4  %   B    8  %   8    9
```

在 Java 中提供了编码和解码的 API 工具类可以让我们更快速的进行编码和解码：

```java
// 编码
java.net.URLEncoder.encode("需要被编码的内容","字符集(UTF-8)")
// 解码
java.net.URLDecoder.decode("需要被解码的内容","字符集(UTF-8)")
```

+++success 分析：解决乱码

实现步骤：

- 按照 ISO-8859-1 编码获取乱码对应的字节数组
- 按照 UTF-8 编码获取字节数组对应的字符串

```java
public class URLDemo {
    public static void main(String[] args) throws UnsupportedEncodingException {
        String username = "张三";
        // 1、URL编码
        String encode = URLEncoder.encode(username, "utf-8");
        System.out.println(encode); // %E5%BC%A0%E4%B8%89

        // 2、URL解码
        String decode = URLDecoder.decode(encode, "ISO_8859_1");
        System.out.println(decode); // å¼ ä¸

        // 3、转换为字节数据，编码
        byte[] bytes = decode.getBytes("ISO_8859_1");
        for (byte b : bytes) {
            System.out.print(b + " "); // -27 -68 -96 -28 -72 -119
        }
        // 4、将字节数组转为字符串，解码
        String s = new String(bytes, "utf-8");
        System.out.println(s); // 张三
    }
}
```

+++

通用方式：先编码再解码

```java
@WebServlet("/req3")
public class RequestDemo3 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String username = req.getParameter("username");
        System.out.println("解决乱码前：" + username);

        // GET的请求参数：getQueryString()
        // 乱码原因：tomcat进行URL解码，默认的字符集ISO-8859-1

        // 1、乱码数据进行编码：转为字节数组
        byte[] bytes = username.getBytes(StandardCharsets.ISO_8859_1);
        // 2、字节数组解码
        username = new String(bytes, StandardCharsets.UTF_8);
        System.out.println("解决乱码后：" + username);

        // 一行代码解决
        // username  = new String(username.getBytes(StandardCharsets.ISO_8859_1),StandardCharsets.UTF_8);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp);
    }
}
```

## 请求转发

请求转发（forward）：一种在服务器内部的资源跳转方式

![image-20250806091306335](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250806091306335.png)

- 浏览器发送请求给服务器，服务器中对应的资源 A 接收到请求
- 资源 A 处理完请求后将请求发给资源 B
- 资源 B 处理完后将结果响应给浏览器
- 请求从资源 A 到资源 B 的过程就叫请求转发

实现方式：

```java
req.getRequestDispatcher("资源B路径").forward(req,resp);
```

请求转发资源间共享数据：使用 request 对象

```java
// 存储数据到request域中
// 域：范围，数据是存储在request对象
void setAttribute(String name,Object o);

// 根据key，获取值
Object getAttribute(String name);

// 根据key，删除该键值对
void removeAttribute(String name);
```

特点：

- 浏览器地址栏路径不发生变化
- 只能转发到当前服务器的内部资源
- 一次请求，可以在转发资源间使用 request 共享数据

+++success 示例

RequestDemo4 类

```java
@WebServlet("/req4")
public class RequestDemo4 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("demo4...");

        // 存储数据
        req.setAttribute("msg", "hello");

        // 请求转发
        req.getRequestDispatcher("/req5").forward(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp);
    }
}
```

RequestDemo5 类

```java
@WebServlet("/req5")
public class RequestDemo5 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("demo5...");

        // 获取数据
        Object msg = req.getAttribute("msg");
        System.out.println(msg);
        // req.removeAttribute("msg"); // 删除键值对
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp);
    }
}
```

测试：访问 `http://localhost:8080/request-demo/req4`

运行结果：

```sh
demo4...
demo5...
hello
```

+++

# Response 对象

## 继承体系

![image-20250806094224576](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250806094224576.png)

## 设置响应数据

HTTP 响应数据分为三部分：响应行、响应头、响应体

### 响应行

![image-20250806094526979](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250806094526979.png)

```java
// 设置响应状态码
void setStatus(int sc);
```

### 响应头

![image-20250806094549374](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250806094549374.png)

```java
// 设置响应头键值对
void setHeader(String name,String value);
```

### 响应体

![image-20250806094605925](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250806094605925.png)

```java
// 获取字符输出流
PrintWriter getWriter();

// 获取字节输出流
ServletOutputStream getOutputStream();
```

## 请求重定向

重定向（redirect）：一种资源跳转方式

![image-20250806094848118](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250806094848118.png)

- 浏览器发送请求给服务器，服务器中对应的资源 A 接收到请求

- 资源 A 现在无法处理该请求，就会给浏览器响应一个 302 的状态码+location 的一个访问资源 B 的路径

- 浏览器接收到响应状态码为 302 就会重新发送请求到 location 对应的访问地址去访问资源 B

- 资源 B 接收到请求后进行处理并最终给浏览器响应结果，这整个过程就叫重定向

实现方式：

```java
resp.setStatus(302);
resp.setHeader("location", "资源B的访问路径");
或者
resposne.sendRedirect("资源B的访问路径");
```

特点：

- 浏览器地址栏路径发送变化
- 可以重定向到任何位置的资源（服务内容、外部均可）
- 两次请求，不能在多个资源使用 request 共享数据

+++success 示例

ResponseDemo1 类

```java
@WebServlet("/resp1")
public class ResponseDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("resp1....");

        // 重定向
        // resp.setStatus(302); // 设置响应状态码
        // resp.setHeader("Location", "/request-demo/resp2"); // 设置响应头

        // 简化方式
        resp.sendRedirect("/request-demo/resp2");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp);
    }
}
```

ResponseDemo2 类

```java
@WebServlet("/resp2")
public class ResponseDemo2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("resp2....");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp);
    }
}
```

测试：访问 `http://localhost:8080/request-demo/resp1`

运行结果：

```sh
# 浏览器路径跳转至http://localhost:8080/request-demo/resp2
resp1....
resp2....
```

+++

## 路径问题

- 浏览器使用：需要加虚拟目录（项目访问路径）
- 服务端使用：不需要加虚拟目录

```java
// 请求转发
req.getRequestDispatcher("/req5").forward(req, resp);

// 重定向
resp.setHeader("Location", "/request-demo/resp2");
```

- 转发：在服务端进行的，不需要加虚拟目录
- 重定向：路径最终是由浏览器来发送请求，就需要添加虚拟目录

其它示例：

```java
<a href='路径'> // 从浏览器发送，需要加
<form action='路径'> // 从浏览器发送，需要加
req.getRequestDispatcher("路径"); // 从服务器内部跳转，不需要加
resp.sendRedirect("路径"); // 由浏览器进行跳转，需要加
```

> 思考：在重定向的代码中，`/request-demo` 是固定的，如果后期通过 Tomcat 插件配置了项目的访问路径，那么所有需要重定向的地方都需要重新修改，如何优化？

优化：

```java
@WebServlet("/resp1")
public class ResponseDemo1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("resp1....");
        // 动态获取虚拟目录
        String contextPath = req.getContextPath();
        resp.sendRedirect(contextPath + "/resp2");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp);
    }
}
```

## 响应字符数据

```java
// 通过Response对象获取字符输出流
PrintWriter writer = resp.getWriter();

// 写数据
writer.write("aaa");
```

> :warning: 注：
>
> - 该流不需要关闭，随着响应结束，response 对象销毁，由服务器关闭
>
> - 中文数据乱码：原因通过 Response 获取的字符输出流默认编码：ISO-8859-1
>
>   ```JAVA
>   resp.setContentType("text/html;charset=utf-8");
>   ```

+++success 示例

```java
@WebServlet("/resp3")
public class ResponseDemo3 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 设置响应的数据格式及数据的编码
        resp.setContentType("text/html;charset=utf-8");
        PrintWriter writer = resp.getWriter();
        // resp.setHeader("content-type", "text/html");

        writer.write("你好");
        writer.write("<h1>你好</h1>");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp);
    }
}
```

+++

## 响应字节数据

```java
// 通过Response对象获取字节输出流
ServletOutputStream outputStream = resp.getOutputStream();

// 写数据
outputStream.write(字节数据);
```

+++success 示例

```java
@WebServlet("/resp4")
public class ResponseDemo4 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 1、读取文件
        FileInputStream fis = new FileInputStream("C://Users/zendow_xw/Downloads/cat4.png");
        // 2、获取response字节输出流
        ServletOutputStream os = resp.getOutputStream();
        // 3、完成流的copy
        byte[] buff = new byte[1024];
        int len = 0;
        while ((len = fis.read(buff)) != -1) {
            os.write(buff, 0, len);
        }
        fis.close();
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp);
    }
}
```

+++

IOUtils 工具类使用

1、在 pom.xml 添加依赖

```java
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.6</version>
</dependency>
```

2、调用工具类方法

```java
IOUtils.copy(输入流, 输出流);
```

+++success 示例：使用工具类，优化后的代码

```java
@WebServlet("/resp4")
public class ResponseDemo4 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 1、读取文件
        FileInputStream fis = new FileInputStream("C://Users/zendow_xw/Downloads/cat4.png");
        // 2、获取response字节输出流
        ServletOutputStream os = resp.getOutputStream();
        // 3、完成流的copy
        IOUtils.copy(fis, os);
        fis.close();
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp);
    }
}
```

+++
