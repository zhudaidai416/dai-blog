---
title: Maven
date: 2025-07-22 16:57:51
category:
  - [计算机与科学, Java, JavaWeb]
tags: JavaWeb
cover: https://daiblog.oss-cn-chengdu.aliyuncs.com/cover/1-4.jpg
---

# 简介

Maven 是专门用于管理和构建 Java 项目的工具

* 提供了一套标准化的项目结构

* 提供了一套标准化的构建流程（编译，测试，打包，发布……）

* 提供了一套依赖管理机制

**Maven 构建的项目结构**：

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/Maven构建的项目结构.png)

**标准化的构建流程**：代码 ➡︎ 编译 ➡︎ 测试 ➡︎ 打包 ➡︎ 发布

[Apache Maven](http://maven.apache.org/) 是一个项目管理和构建工具，它基于项目对象模型（POM）的概念，通过一小段描述信息来管理项目的构建、报告和文档

## Maven 模型

* 项目对象模型（Project Object Model）
* 依赖管理模型（Dependency）
* 插件（Plugin）

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/Maven模型.png)

依赖管理模型：使用坐标来描述当前项目依赖哪儿些第三方 jar 包

## 仓库

依赖 jar 包是存储在我们的本地仓库中，而项目运行时从本地仓库中拿需要的依赖 jar 包

**分类**：

* 本地仓库：自己计算机上的一个目录

* 中央仓库：由Maven团队维护的全球唯一的仓库

  * 地址： https://repo1.maven.org/maven2/

* 远程仓库(私服)：一般由公司团队搭建的私有仓库

当项目中使用坐标引入对应依赖 jar 包后，首先会查找本地仓库中是否有对应的 jar 包

* 如果有，则在项目直接引用

* 如果没有，则去中央仓库中下载对应的 jar 包到本地仓库

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/Maven本地仓库.png)

远程仓库：

jar 包的查找顺序：本地仓库 ➡︎ 远程仓库 ➡︎ 中央仓库

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/Maven远程仓库.png)

# 安装配置

1、解压 apache-maven-3.6.1.rar 即安装完成

解压缩后的目录结构：

- bin 目录 ： 存放的是可执行命令，mvn 命令重点关注
- conf 目录 ：存放 Maven 的配置文件。`settings.xml` 配置文件后期需要修改
- lib 目录 ：存放 Maven 依赖的 jar 包。Maven 也是使用 java 开发的，所以它也依赖其他的 jar 包

2、配置环境变量

在系统变量处新建一个变量 `MAVEN_HOME`

```properties
变量名：MAVEN_HOME
变量值：D:\apache-maven-3.6.1（安装路径）
```

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/Maven安装1.png)

在 `Path` 中进行配置

```properties
%MAVEN_HOME%\bin
```

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/Maven安装2.png)

3、验证

```sh
mvn -version
```

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/Maven安装3.png)

4、配置本地仓库

修改 `conf/settings.xml` 中的 `<localRepository>` 为一个指定目录作为本地仓库，用来存储 jar 包

```

```

5、配置阿里云私服

中央仓库在国外，所以下载 jar 包速度可能比较慢，而阿里公司提供了一个远程仓库，里面基本也都有开源项目的 jar 包

修改 `conf/settings.xml` 中的  `<mirrors>标签`

```xml
<mirror>  
    <id>alimaven</id>  
    <name>aliyun maven</name>  
    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
    <mirrorOf>central</mirrorOf>          
</mirror>
```

# 基本使用

## 常用命令

在磁盘上进入到项目的 `pom.xml` 目录下，打开命令提示符

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/Maven基本使用.png)

### 编译

```sh
mvn compile
```

* 从阿里云下载编译需要的插件的 jar 包，在本地仓库也能看到下载好的插件
* 在项目下会生成一个 `target` 目录，编译后的字节码文件就放在该目录下

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/Maven编译1.png)

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/Maven编译2.png)

### 清理

```sh
mvn clean
```

* 从阿里云下载清理需要的插件 jar 包
* 删除项目下的 `target` 目录

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/Maven清理.png)

### 测试

```sh
mvn test  
```

会执行所有的测试代码

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/Maven测试.png)

### 打包

```sh
mvn package
```

- 从阿里云下载打包需要的插件 jar 包
- 在项目的 `terget` 目录下有一个 jar 包（将当前项目打成的jar包）

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/Maven打包.png)

### 安装

```sh
mvn install
```

会将当前项目打成 jar 包，并安装到本地仓库

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/Maven安装.png)

## 生命周期

Maven 构建项目生命周期描述的是一次构建过程经历经历了多少个事件

Maven 对项目构建的生命周期划分为3套：

* clean ：清理工作
* default ：核心工作，例如编译，测试，打包，安装等
* site ： 产生报告，发布站点等。这套声明周期一般不会使用

同一套生命周期内，执行后边的命令，前面的所有命令会自动执行

例如默认（default）生命周期：compile ➡︎ test ➡︎ package ➡︎ install

# IDEA 使用 Maven

## 配置 Maven 环境



## Maven 坐标详解

- Maven 中的坐标是资源的唯一标识
- 使用坐标来定义项目或引入项目中需要的依赖

**Maven 坐标主要组成**：

* groupId：定义当前 Maven 项目隶属组织名称（通常是域名反写，例如：com.itheima）
* artifactId：定义当前 Maven 项目名称（通常是模块名称，例如 order-service、goods-service）
* version：定义当前项目版本号

```xml
<groupId>com.itheima</groupId>
<artifactId>maven-demo</artifactId>
<version>1.0-SNAPSHOT</version>
```

> :warning: 注：
>
> * 上面所说的资源可以是插件、依赖、当前项目
> * 我们的项目如果被其他的项目依赖时，也是需要坐标来引入的

## 创建 Maven 项目



## 导入 Maven 项目

安装 Maven Helper 插件
