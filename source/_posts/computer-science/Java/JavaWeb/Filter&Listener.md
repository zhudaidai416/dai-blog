---
title: Filter&Listener
date: 2025-08-12 14:46:15
category:
  - [计算机与科学, Java, JavaWeb]
tags: JavaWeb
cover: https://daiblog.oss-cn-chengdu.aliyuncs.com/cover/6-12.jpg
---

# Filter

## 概述

Filter 表示过滤器，是 JavaWeb 三大组件（Servlet、Filter、Listener）之一

过滤器可以把对资源的请求<font color=red>拦截</font>下来，从而实现一些特殊的功能

过滤器一般完成一些通用的操作，比如：权限控制、统一编码处理、敏感字符处理等等

![image-20250812150321898](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250812150321898.png)

## 快速入门

1、定义类，实现 Filter 接口，并重写其所有方法

```java
package com.itheima.web.filter;

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import java.io.IOException;

@WebFilter("/*")
public class FilterDemo implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {}

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {}

    @Override
    public void destroy() {}
}
```

2、配置 Filter 拦截资源的路径：在类上定义 `@WebFilter` 注解

```java
// 表示拦截所有的资源
@WebFilter("/*")
```

3、在 doFilter 方法中输出一句话，并放行

```java
public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
    // 放行前，对request数据进行处理
    System.out.println("FilterDemo放行前...");

    // 放行
    filterChain.doFilter(servletRequest, servletResponse);

    // 放行后，对response数据进行处理
    System.out.println("FilterDemo放行后...");
}
```

## 执行流程

![image-20250812165659198](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250812165659198.png)

> 思考：
>
> - 放行后访问对应资源，资源访问完成后，还会回到 Filter 中吗？
>
>   会
>
> - 如果回到 Filter 中，是重头执行还是执行放行后的逻辑呢？
>
>   放行后逻辑

**Filter 的执行流程**：执行放行前逻辑 ➡︎ 放行 ➡︎ 访问资源 ➡︎ 执行放行后逻辑

## 拦截路径配置

Filter 可以根据需求，配置不同的拦截资源路径

四种配置方式：

- 拦截具体的资源：/index.jsp：只有访问 index.jsp 时才会被拦截
- 目录拦截：/user/\*：访问 /user 下的所有资源，都会被拦截
- 后缀名拦截：\*.jsp：访问后缀名为 jsp 的资源，都会被拦截
- 拦截所有：/\*：访问所有资源，都会被拦截

## 过滤器链

一个 Web 应用，可以配置<font color=red>多个</font>过滤器，这多个过滤器称为过滤器链

![image-20250825114509843](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250825114509843.png)

流程执行：

- 执行 Filter1 的放行前逻辑代码
- 执行 Filter1 的放行代码
- 执行 Filter2 的放行前逻辑代码
- 执行 Filter2 的放行代码
- 访问到资源
- 执行 Filter2 的放行后逻辑代码
- 执行 Filter1 的放行后逻辑代码

注解配置 Filter，而这种配置方式的优先级是按照过滤器类名（字符串）的自然排序

# Listener

## 概述

Listener 表示监听器，是 JavaWeb 三大组件（Servlet、Filter、Listener）之一

监听器可以监听就是在 `application`，`session`，`request` 三个对象创建、销毁或者往其中添加修改删除属性时自动执行代码的功能组件

## 分类

JavaWeb 中提供了 8 个监听器

| 监听器分类          | 监听器名称                      | 作用                                             |
| ------------------- | ------------------------------- | ------------------------------------------------ |
| ServletContext 监听 | ServletContextListener          | 用于对 ServletContext 对象进行监听（创建、销毁） |
|                     | ServletContextAttributeListener | 对 ServletContext 对象中属性的监听（增删改属性） |
| Session 监听        | HttpSessionListener             | 对 Session 对象的整体状态的监听（创建、销毁）    |
|                     | HttpSessionAttributeListener    | 对 Session 对象中的属性监听（增删改属性）        |
|                     | HttpSessionBindingListener      | 监听对象于 Session 的绑定和解除                  |
|                     | HttpSessionActivationListener   | 对 Session 数据的钝化和活化的监听                |
| Request 监听        | ServletRequestListener          | 对 Request 对象进行监听（创建、销毁）            |
|                     | ServletRequestAttributeListener | 对 Request 对象中属性的监听（增删改属性）        |

ServletContextListener 接口有以下两个方法

```java
// ServletContext对象被创建了会自动执行的方法
void contextInitialized(ServletContextEvent sce)

// ServletContext对象被销毁时会自动执行的方法
void contextDestroyed(ServletContextEvent sce)
```

## 快速入门

1、定义一个类，实现 ServletContextListener 接口

2、在类上添加 `@WebListener` 注解

```java
package com.itheima.web.listener;

import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import javax.servlet.annotation.WebListener;

@WebListener
public class ContextLoaderListener implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent servletContextEvent) {
        // 加载资源
        System.out.println("ContextLoaderListener...");
    }

    @Override
    public void contextDestroyed(ServletContextEvent servletContextEvent) {
        // 释放资源
    }
}
```
