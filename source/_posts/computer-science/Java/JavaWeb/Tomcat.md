---
title: Tomcat
date: 2025-07-31 15:27:00
category:
  - [计算机与科学, Java, JavaWeb]
tags: JavaWeb
cover: https://daiblog.oss-cn-chengdu.aliyuncs.com/cover/6-7.jpg
---

# 概述

## Web 服务器

Web 服务器是一个应用程序（软件），对 HTTP 协议的操作进行封装，使得程序员不必直接对协议进行操作，让 Web 开发更加便捷。主要功能是“提供网上信息浏览服务”

![image-20250731153625651](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250731153625651.png)

**Web 服务器软件使用步骤**：

- 准备静态资源
- 下载安装 Web 服务器软件
- 将静态资源部署到 Web 服务器上
- 启动 Web 服务器使用浏览器访问对应的资源

**Web 服务器作用**：

- 封装 HTTP 协议操作，简化开发
- 可以将 Web 项目部署到服务器中，对外提供网上浏览服务

## [Tomcat](https://tomcat.apache.org)

官网：https://tomcat.apache.org

Tomcat 是 Apache 软件基金会一个核心项目，是一个开源免费的轻量级 Web 服务器，支持 Servlet/JSP 少量 JavaEE 规范

- JavaEE：Java Enterprise Edition，Java 企业版。指 Java 企业级开发的技术规范总和

- 包含 13 项技术规范：JDBC、JNDI、EJB、RMI、JSP、Servlet、XML、JMS、Java IDL、JTS、JTA、JavaMail、JAF

Tomcat 也称为 Web 容器，Servlet 容器。Servlet 需要依赖于 Tomcat 才能运行

# 基本使用

## 下载安装

官网下载：https://tomcat.apache.org

绿色版，直接解压即可

**目录结构**：

- bin：可执行文件存放目录

  该目录下有两类文件，一种是以 `.bat` 结尾的，是 Windows 系统的可执行文件

  一种是以 `.sh` 结尾的，是 Linux 系统的可执行文件

- conf：配置文件存放目录

- lib：tomcat 依赖的 jar 包

- logs：日志文件

- temp：临时文件

- webapps：应用发布目录

- work：工作目录

## 卸载

直接删除目录即可

## 启动

双击: bin\startup.bat

启动后，通过浏览器访问 `http://localhost:8080` 能看到 Apache Tomcat 的内容就说明 Tomcat 已经启动成功

> :warning: 注：启动的过程中，控制台有中文乱码，需要修改 `conf/logging.prooperties`
>
> ```properties
> # UTF-8改为GBK
> java.util.logging.ConsoleHandler.encoding = UTF-8
> ```

## 关闭

- 直接 x 掉运行窗口：强制关闭（不建议）
- bin\shutdown.bat：正常关闭
- ctrl+c：正常关闭

## 配置

### 修改端口

Tomcat 默认的端口是 8080，要想修改 Tomcat 启动的端口号，需要修改 `conf/server.xml`

```xml
<Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
```

> :warning: 注：HTTP 协议默认端口号为 80，如果将 Tomcat 端口号改为 80，则将来访问 Tomcat 时，将不用输入端口号

### 启动时可能出现的错误

1、端口号冲突：找到对应程序，将其关闭掉

Tomcat 的端口号取值范围是 0-65535

![image-20250801135010533](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250801135010533.png)

2、启动窗口一闪而过：检查 JAVA_HOME 环境变量是否正确配置

## 部署

将项目放置到 webapps 目录下，即部署完成

一般 JavaWeb 项目会被打包称 war 包，然后将 war 包放到 Webapps 目录下，Tomcat 会自动解压缩 war 文件

例子：

- 将静态页面 `hello` 目录拷贝到 Tomcat 的 webapps 目录下
- 通过浏览器访问 `http://localhost:8080/hello/a.html`，能看到内容就说明项目已经部署成功

# Maven 创建 Web 项目

## Web 项目结构

Web 项目的结构分为：开发中的项目和开发完可以部署的 Web 项目

- Maven Web 项目结构：开发中的项目

  ![image-20250801140209914](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250801140209914.png)

- 部署的 JavaWeb 项目结构：开发完成，可以部署的项目

  ![image-20250801140234302](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20250801140234302.png)

开发项目通过执行 Maven 打包命令 package，可以获取到部署的 Web 项目目录

- 编译后的 Java 字节码文件和 resources 的资源文件，会被放到 WEB-INF 下的 classes 目录下
- pom.xml 中依赖坐标对应的 jar 包，会被放入 WEB-INF 下的 lib 目录下

## 创建项目时的报错

### 错误 1：Maven 版本和编译插件版本不匹配

![使用骨架创建报错](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/使用骨架创建报错.png)

{% links %}

- site: 教程
  owner: 阿里云 - 云开发者社区
  url: https://developer.aliyun.com/article/1646608
  image: https://daiblog.oss-cn-chengdu.aliyuncs.com/icon/note1.png
  desc: Maven 编译报错：Failed to execute goal org.apache.maven.plugins:maven-compiler-plugin:3.13.0:compile 解决方案
  color: "#c38e9e"

{% endlinks %}

操作：之前的 maven 版本是 3.6.1，将 maven 版本更新至 3.6.3

1、解压安装（Maven 3.6.3 版本的[下载链接](https://archive.apache.org/dist/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.zip)）

2、修改系统环境变量 MAVEN_HOME 的变量值

输入 `mvn -v`，查看是否配置成功

3、将旧版本 3.6.1 的本地仓库文件夹 repo（自定义的）复制到 3.6.3 中

4、编辑 `conf/settings.xml`

```xml
<!-- 配置自己的本地仓库路径 -->
<localRepository>E:\Tool\apache-maven-3.6.3\repo</localRepository>

<!-- 配置阿里云的Maven私服镜像 -->
<mirrors>
  <mirror>
    <id>alimaven</id>
    <mirrorOf>central</mirrorOf>
    <name>aliyun maven</name>
    <url>http://maven.aliyun.com/repository/public</url>
  </mirror>
</mirrors>
```

5、修改在 IDEA 中 Maven 的配置

### 错误 2：使用骨架创建没有 src 有关的目录

<a id="section1"></a>

![解决Maven创建Web项目报错1](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/解决Maven创建Web项目报错1.png)

![解决Maven创建Web项目报错2](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/解决Maven创建Web项目报错2.png)

![解决Maven创建Web项目报错3](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/解决Maven创建Web项目报错3.png)

## 创建 Maven Web 项目

### 教程

{% links %}

- site: 教程 1
  owner: CSDN
  url: https://blog.csdn.net/weixin_45384457/article/details/128532296
  image: https://daiblog.oss-cn-chengdu.aliyuncs.com/icon/note1.png
  desc: 创建 maven 项目
  color: "#c38e9e"

{% endlinks %}

{% links %}

- site: 教程 2
  owner: CSDN
  url: https://blog.csdn.net/hgnuxc_1993/article/details/125427590
  image: https://daiblog.oss-cn-chengdu.aliyuncs.com/icon/note1.png
  desc: 创建 maven 项目
  color: "#c38e9e"

{% endlinks %}

### 使用骨架

若没有生成 src，跳转至[[创建项目时的报错](#section1)]，再重新创建项目

1、选择 web 项目骨架，创建项目

![使用骨架创建](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/使用骨架创建.png)

2、删除 pom.xml 多余的坐标

3、补齐缺失的目录结构

src/main/java、src/main/resources

### 不使用骨架

1、创建项目

![不使用骨架创建1](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/不使用骨架创建1.png)

2、在 pom.xml 设置打包方式为 war

```xml
<packaging>war</packaging>
```

3、补齐缺失的目录结构：webapp

![不使用骨架创建2](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/不使用骨架创建2.png)

![不使用骨架创建3](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/不使用骨架创建3.png)

# IDEA 使用 Tomcat

## 集成本地 Tomcat

1、打开添加本地 Tomcat 的面板

![idea本地集成Tomcat1](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/idea本地集成Tomcat1.png)

2、指定本地 Tomcat 的具体路径

![idea本地集成Tomcat2](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/idea本地集成Tomcat2.png)

3、修改 Tomcat 的名称（此步骤可以不改，只是让名字看起来更有意义），HTTP port 中的端口也可以进行修改，比如把 8080 改成 80

4、将开发项目部署项目到 Tomcat 中

![idea本地集成Tomcat3](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/idea本地集成Tomcat3.png)

5、部署成功后，就可以启动项目，为了能更好的看到启动的效果，可以在 webapp 目录下添加 a.html 页面

![idea本地集成Tomcat4](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/idea本地集成Tomcat4.png)

6、启动成功后，可以通过浏览器进行访问测试

![idea本地集成Tomcat5](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/idea本地集成Tomcat5.png)

## Tomcat Maven 插件

1、在 pom.xml 中添加 Tomcat 插件

快捷键：alt + insert ➡︎ 插件模板（Plugin Template）

```xml
<build>
    <plugins>
    	<!-- Tomcat插件 -->
        <plugin>
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId>
            <version>2.2</version>
        </plugin>
    </plugins>
</build>
```

2、使用 Maven Helper 插件快速启动项目，选中项目，右键 ➡︎ Run Maven ➡︎ tomcat7:run

![Tomcat的Maven插件](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/Tomcat的Maven插件.png)
