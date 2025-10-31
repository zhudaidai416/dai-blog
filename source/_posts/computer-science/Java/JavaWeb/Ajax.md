---
title: Ajax
date: 2025-08-25 17:20:33
category:
  - [计算机与科学, Java, JavaWeb]
tags: JavaWeb
cover: https://daiblog.oss-cn-chengdu.aliyuncs.com/cover/6-13.jpg
---

# Ajax

## 概述

AJAX（Asynchronous JavaScript And XML）：<font color=red>异步</font>的 JavaScript 和 XML

### 作用

- **与服务器进行数据交换**：通过 AJAX 可以给服务器发送请求，并获取服务器响应的数据

  使用了 AJAX 和服务器进行通信，就可以使用 HTML + AJAX 来<font color=red>替换 JSP </font>页面了

  ![image-20250826091928944](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250826091928944.png)

- **异步交互**：可以在<font color=red>不重新加载整个页面</font>的情况下，与服务器交换数据并<font color=red>更新部分网页</font>的技术，如：搜索联想、用户名是否可用校验，等等...

### 同步和异步

同步发送请求过程：

![image-20250826091055517](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250826091055517.png)

异步发送请求过程：

![image-20250826091107931](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250826091107931.png)

## 快速入门

### 服务端实现

1、编写 AjaxServlet，并使用 response 输出字符串

```java
package com.itheima.web.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/ajaxServlet")
public class AjaxServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 响应数据
        resp.getWriter().write("hello ajax");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp);
    }
}
```

### 客户端实现

在 webapp 下创建 `ajax-demo.html`

2、创建 XMLHttpRequest 对象：用于和服务器交换数据

```js
var xhttp;
if (window.XMLHttpRequest) {
  xhttp = new XMLHttpRequest();
} else {
  // code for IE6, IE5
  xhttp = new ActiveXObject("Microsoft.XMLHTTP");
}
```

3、向服务器发送请求

```js
// 建立连接
xhttp.open("GET", "http://localhost:8080/ajax-demo/ajaxServlet");
// 发送请求
xhttp.send();
```

4、获取服务器响应数据

```js
xhttp.onreadystatechange = function () {
  if (this.readyState == 4 && this.status == 200) {
    // 通过 this.responseText 可以获取到服务端响应的数据
    alert(this.responseText);
  }
};
```

5、测试：在浏览器地址栏输入 `http://localhost:8080/ajax-demo/ajax-demo.html`

![image-20250826164048193](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250826164048193.png)

+++success 完整代码

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Title</title>
  </head>
  <body>
    <script>
      // 1、创建核心对象
      var xhttp;
      if (window.XMLHttpRequest) {
        xhttp = new XMLHttpRequest();
      } else {
        // code for IE6, IE5
        xhttp = new ActiveXObject("Microsoft.XMLHTTP");
      }

      // 2、发送请求
      xhttp.open("GET", "http://localhost:8080/ajax-demo/ajaxServlet");
      xhttp.send();

      // 3、获取响应
      xhttp.onreadystatechange = function () {
        if (this.readyState === 4 && this.status === 200) {
          // 通过 this.responseText 可以获取到服务端响应的数据
          alert(this.responseText);
        }
      };
    </script>
  </body>
</html>
```

+++

# Axios

Axios 对原生的 AJAX 进行封装，简化书写

官网：https://www.axios-http.cn

## 基本使用

1、引入 axios 的 js 文件

```js
<script src="js/axios-0.18.0.js"></script>
```

2、使用 axios 发送请求，并获取响应结果

```js
// 发送get请求
axios({
  method: "get",
  url: "http://localhost:8080/ajax-demo/axiosServlet?username=zhangsan"
}).then(function (resp) {
  alert(resp.data);
});

// 发送post请求
axios({
  method: "post",
  url: "http://localhost:8080/ajax-demo/axiosServlet",
  data: "username=zhangsan"
}).then(function (resp) {
  alert(resp.data);
});
```

- method 属性：用来设置请求方式的
- url 属性：用来书写请求的资源路径
- data 属性：作为请求体被发送的数据
- then：传递一个匿名函数（回调函数），在成功响应后调用的函数

## 快速入门

### 后端实现

定义一个用于接收请求的 servlet

```js
package com.itheima.web.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/axiosServlet")
public class AxiosServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("get...");
        // 接收请求参数
        String username = req.getParameter("username");
        System.out.println(username);
        // 响应数据
        resp.getWriter().write("hello axios");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("post...");
        this.doGet(req, resp);
    }
}
```

### 前端实现

在 webapp 下创建 `axios-demo.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Title</title>
  </head>
  <body>
    <script src="js/axios-0.18.0.js"></script>

    <script>
      axios({
        method: "get",
        url: "http://localhost:8080/ajax-demo/axiosServlet?username=zhangsan"
      }).then(function (resp) {
        alert(resp.data);
      });

      axios({
        method: "post",
        url: "http://localhost:8080/ajax-demo/axiosServlet",
        data: "username=zhangsan"
      }).then(function (resp) {
        alert(resp.data);
      });
    </script>
  </body>
</html>
```

## 请求方法别名

为了方便起见，Axios 已经为所有支持的请求方法提供了别名

```js
axios.get(url[,config]) // get请求
axios.delete(url[,config]) // delete请求
axios.head(url[,config]) // head请求
axios.option(url[,config]) // options请求
axios.post(url[,data[,config]) // post请求
axios.put(url[,data[,config]) // put请求
axios.patch(url[,data[,config]) // patch请求
```

简化

```js
axios
  .get("http://localhost:8080/ajax-demo/axiosServlet?username=zhangsan")
  .then(function (resp) {
    alert(resp.data);
  });

axios
  .post("http://localhost:8080/ajax-demo/axiosServlet", "username=zhangsan")
  .then(function (resp) {
    alert(resp.data);
  });
```

# JSON

## 概述

概念：JavaScript Object Notation，JavaScript 对象表示法

多用于作为数据载体，在网络中进行数据传输

JavaScript 对象：

```js
{
	name: "zhangsan",
	age: 23,
	city: "北京"
}
```

JSON 格式：

```json
{
  "name": "zhangsan",
  "age": 23,
  "city": "北京"
}
```

## JSON 基础语法

### 定义

```js
var 变量名 = {"key1":value1,
			 "key2":value2,
             ...
			};

// 获取数据
变量名.key
```

value 的数据类型：

- 数字（整数或浮点数）
- 字符串（在双引号中）
- 逻辑值（true 或 false）
- 数组（在方括号中）
- 对象（在花括号中）
- null

### 发送异步请求携带参数

axios 是一个很强大的工具，只需要将需要提交的参数封装成 js 对象，并将该 js 对象作为 axios 的 data 属性值进行，它会自动将 js 对象转换为 JSON 串进行提交

```js
var jsObject = { name: "张三" };

axios({
  method: "post",
  url: "http://localhost:8080/ajax-demo/axiosServlet",
  data: jsObject //这里 axios 会将该js对象转换为 json 串的
}).then(function (resp) {
  alert(resp.data);
});
```

## JSON 数据和 Java 对象转换

![image-20250827140715411](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250827140715411.png)

- 请求数据：JSON 字符串转为 Java 对象
- 响应数据：Java 对象转为 JSON 字符串

### Fastjson 概述

Fastjson 是阿里巴巴提供的一个 Java 语言编写的高性能功能完善的 JSON 库，是目前 Java 语言中最快的 JSON 库，可以实现 Java 对象和 JSON 字符串的相互转换

### Fastjson 使用

1、导入坐标

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.62</version>
</dependency>
```

2、Java 对象转 JSON

```java
String jsonStr = JSON.toJSONString(obj);
```

3、JSON 字符串转 Java 对象

```java
User user = JSON.parseObject(jsonStr, User.class);
```

+++success 示例

前提：已创建好 User 类

```java
package com.itheima.json;

import com.alibaba.fastjson.JSON;

public class FastJsonDemo {
    public static void main(String[] args) {
        // 将Java对象转为JSON字符串
        User user = new User();
        user.setId(1);
        user.setUsername("zhangsan");
        user.setPassword("123456");

        String jsonString = JSON.toJSONString(user);
        System.out.println(jsonString);

        // 将JSON字符串转为Java对象
        User u = JSON.parseObject("{\"id\":1,\"password\":\"123456\",\"username\":\"zhangsan\"}", User.class);
        System.out.println(u);
    }
}
```

+++
