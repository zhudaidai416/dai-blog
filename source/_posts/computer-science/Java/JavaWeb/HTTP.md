---
title: HTTP
date: 2025-07-31 09:32:08
category:
  - [计算机与科学, Java, JavaWeb]
tags: JavaWeb
cover: https://daiblog.oss-cn-chengdu.aliyuncs.com/cover/6-6.jpg
---

# Web 概述

- Web：全球广域网，也称为万维网（www），能够通过浏览器访问的网站
- JavaWeb 就是用 Java 技术来解决相关 Web 互联网领域的技术栈

# JavaWeb 技术栈

## B/S 架构

B/S 架构：Browser/Server（浏览器/服务器 架构模式）

- 特点：客户端只需要浏览器，应用程序的逻辑和数据都存储在服务器端。浏览器只需要请求服务器，获取 Web 资源，服务器把 Web 资源发送给浏览器即可
- 好处：易于维护升级（服务器端升级后，客户端无需任何部署就可以使用到新的版本）

## 静态资源

HTML、CSS、JavaScript、图片等，负责页面展现

## 动态资源

Servlet、JSP 等，负责逻辑处理

## 数据库

负责存储数据

**Web 的访问过程**：

![image-20250731105230276](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250731105230276.png)

- 浏览器发送一个请求到服务端，去请求所需要的相关资源
- 资源分为动态资源和静态资源，动态资源可以是使用 Java 代码按照 Servlet 和 JSP 的规范编写的内容
- 在 Java 代码可以进行业务处理也可以从数据库中读取数据
- 拿到数据后，把数据交给 HTML 页面进行展示，再结合 CSS 和 JavaScript 使展示效果更好
- 服务端将静态资源响应给浏览器
- 浏览器将这些资源进行解析
- 解析后将效果展示在浏览器，用户就可以看到最终的结果

## HTTP 协议

定义通信规则

## Web 服务器

负责解析 HTTP 协议，解析请求数据，并发送响应数据

# HTTP

## 概念

HyperText Transfer Protocol，超文本传输协议，规定了浏览器和服务器之间<font color=red>数据传输的规则</font>

数据传输的规则：请求数据和响应数据需要按照指定的格式进行传输

**HTTP 协议特点**：

- 基于 TCP 协议：面向连接，安全

  TCP 是一种面向连接的（建立连接之前是需要经过三次握手）、可靠的、基于字节流的传输层通信协议，在数据传输方面更安全

- 基于请求-响应模型的：一次请求对应一次响应

  请求和响应是一一对应关系

- HTTP 协议是无状态协议：对于事务处理没有记忆能力。每次请求-响应都是独立的

  无状态指的是客户端发送 HTTP 请求给服务端之后，服务端根据请求响应数据，响应完后，不会记录任何信息

  这种特性有优点也有缺点

  - 缺点：多次请求间不能共享数据

    Java 中使用会话技术（Cookie、Session）来解决这个问题

  - 优点：速度快

## 请求数据格式

```json
GET / HTTP/1.1
Host:www.itcast.cn
Connection:keep-alive
Cache-Control:max-age=0 Upgrade-Insecure-Requests:1
User-Agent:Mozilla/5.0 Chrome/91.0.4472.106
...
```

### 请求行

请求数据的第一行数据

分为 3 部分

- GET：请求方式
- /：请求资源路径
- HTTP/1.1：协议版本

请求方式有七种，最常用的是 GET 和 POST

### 请求头

第二行开始，格式为 `key: value` 形式

常见的 HTTP 请求头：

```json
Host：表示请求的主机名
User-Agent：浏览器版本，例如Chrome浏览器的标识类似Mozilla/5.0 ...
	Chrome/79，IE浏览器的标识类似Mozilla/5.0 (Windows NT ...)like Gecko
Accept：表示浏览器能接收的资源类型，例如text/*、image/* 或者 */*表示所有
Accept-Language：表示浏览器偏好的语言，服务器可以据此返回不同语言的网页
Accept-Encoding：表示浏览器可以支持的压缩类型，例如gzip, deflate等
```

### 请求体

POST 请求的最后一部分，存储请求参数

```json
POST / HTTP/1.1
Host:www.itcast.cn
Connection:keep-alive
Cache-Control:max-age=0 Upgrade-Insecure-Requests:1
User-Agent:Mozilla/5.0 Chrome/91.0.4472.106

username=superbaby&password=123456 ➡︎ 请求体
```

GET 请求和 POST 请求区别：

- GET 请求请求参数在请求行中，没有请求体

  POST 请求请求参数在请求体中

- GET 请求请求参数大小有限制，POST 没有

## 响应数据格式

```json
HTTP/1.1 200 OK
Server:Tengine
Content-Type:text/html
Transfer-Encoding:chunked...

<html>
<head>
  <title></title>
</head>
<body></body>
</html>
```

### 响应行

响应数据的第一行

分为 3 部分

- HTTP/1.1：协议版本
- 200：响应状态码
- OK：状态码描述

### 响应头

第二行开始，格式为 `key: value` 形式

常见的 HTTP 响应头：

```json
Content-Type：表示该响应内容的类型，例如text/html，image/jpeg
Content-Length：表示该响应内容的长度（字节数）
Content-Encoding：表示该响应压缩算法，例如gzip
Cache-Control：指示客户端应如何缓存，例如max-age=300表示可以最多缓存300秒
```

### 响应体

最后一部分，存放响应数据

`<html>...</html>` 这部分内容就是响应体，它和响应头之间有一个空行隔开

# 响应状态码

## 分类

状态码大全：https://cloud.tencent.com/developer/chapter/13553

| 状态码分类 | 说明         | 描述                                                                           | 例如                                                     |
| ---------- | ------------ | ------------------------------------------------------------------------------ | -------------------------------------------------------- |
| 1xx        | 响应中       | 临时状态码，表示请求已经接受，告诉客户端应该继续请求或者如果它已经完成则忽略它 |                                                          |
| 2xx        | 成功         | 表示请求已经被成功接收，处理已完成                                             |                                                          |
| 3xx        | 重定向       | 重定向到其它地方：它让客户端再发起一个请求以完成整个处理。                     |                                                          |
| 4xx        | 客户端错误   | 处理发生错误，责任在客户端                                                     | 客户端的请求一个不存在的资源，客户端未被授权，禁止访问等 |
| 5xx        | 服务器端错误 | 处理发生错误，责任在服务端                                                     | 服务端抛出异常，路由出错，HTTP 版本不支持等              |

## 常见的响应状态码

| 状态码 | 英文描述                        | 解释                                                                                                 |
| ------ | ------------------------------- | ---------------------------------------------------------------------------------------------------- |
| 200    | OK                              | 客户端请求成功，即**处理成功**，这是我们最想看到的状态码                                             |
| 302    | Found                           | 指示所请求的资源已移动到由`Location`响应头给定的 URL，浏览器会自动重新访问到这个页面                 |
| 304    | Not Modified                    | 告诉客户端，你请求的资源至上次取得后，服务端并未更改，你直接用你本地缓存吧。隐式重定向               |
| 400    | Bad Request                     | 客户端请求有**语法错误**，不能被服务器所理解                                                         |
| 403    | Forbidden                       | 服务器收到请求，但是**拒绝提供服务**，比如：没有权限访问相关资源                                     |
| 404    | Not Found                       | **请求资源不存在**，一般是 URL 输入有误，或者网站资源被删除了                                        |
| 428    | Precondition Required           | **服务器要求有条件的请求**，告诉客户端要想访问该资源，必须携带特定的请求头                           |
| 429    | Too Many Requests               | **太多请求**，可以限制客户端请求某个资源的数量，配合 Retry-After(多长时间后可以请求)响应头一起使用   |
| 431    | Request Header Fields Too Large | **请求头太大**，服务器不愿意处理请求，因为它的头部字段太大。请求可以在减少请求头域的大小后重新提交。 |
| 405    | Method Not Allowed              | 请求方式有误，比如应该用 GET 请求方式的资源，用了 POST                                               |
| 500    | Internal Server Error           | **服务器发生不可预期的错误**。服务器出异常了，赶紧看日志去吧                                         |
| 503    | Service Unavailable             | **服务器尚未准备好处理请求**，服务器刚刚启动，还未初始化好                                           |
| 511    | Network Authentication Required | **客户端需要进行身份验证才能获得网络访问权限**                                                       |
