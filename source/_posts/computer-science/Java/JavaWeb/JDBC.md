---
title: JDBC
date: 2025-06-04 14:14:11
category:
  - [计算机与科学, Java, JavaWeb]
tags: JavaWeb
cover: https://daiblog.oss-cn-chengdu.aliyuncs.com/cover/1-6.jpg

---

# 简介

JDBC：使用 Java 语言操作关系型数据库的一套 API

全称：（Java DataBase Connectivity）Java 数据库连接

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/JDBC.png)

本质：

* 官方（sun公司）定义的一套操作所有关系型数据库的规则，即接口
* 各个数据库厂商去实现这套接口，提供数据库驱动jar包
* 我们可以使用这套接口（JDBC）编程，真正执行的代码是驱动 jar 包中的实现类

# 快速入门

## 下载

MySQL 驱动包：[下载地址](https://downloads.mysql.com/archives/c-j)

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/MySQL驱动包.png)

## 流程

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/Java操作数据库流程.png)

- 编写Java 代码

- Java 代码将 SQL 发送到 MySQL 服务端

- MySQL 服务端接收到 SQL 语句并执行该 SQL 语句

- 将 SQL 语句执行的结果返回给 Java 代码

## 操作

**步骤1**：创建新的空项目

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/jdbc-步骤1.png)

**步骤2**：对项目进行设置，JDK版本、编译版本

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/jdbc-步骤2.png)

**步骤3**：创建模块，指定模块的名称及位置

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/jdbc-步骤3.png)

**步骤4**：导入驱动包，将 mysql 的驱动包放在模块下的 lib 目录（任意命名）下

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/jdbc-步骤4.png)

**步骤5**：并将该 jar 包添加为库文件，级别选择为“模块库”

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/jdbc-步骤5.png)

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/jdbc-步骤6.png)

**步骤6**：在 src 下创建类

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/jdbc-步骤7.png)

**步骤7**：编写代码

```java
package com.itheima.jdbc;

import java.sql.Statement;
import java.sql.Connection;
import java.sql.DriverManager;

/**
 * JDBC 快速入门
 */
public class JDBCDemo {
    public static void main(String[] args) throws Exception {
        // 1、注册驱动
        Class.forName("com.mysql.jdbc.Driver");

        // 2、获取连接
        String url = "jdbc:mysql://127.0.0.1:3306/student_system?useSSL=false";
        String user = "root";
        String password = "123456";
        Connection conn =  DriverManager.getConnection(url, user, password);

        // 3、定义sql
        String sql = "update account set money = 2000 where id = 2";

        // 4、获取执行sql的对象: Statement
        Statement stmt = conn.createStatement();

        // 5、执行sql
        int count = stmt.executeUpdate(sql);

        // 6、处理结果
        System.out.println(count);

        // 7、释放资源
        stmt.close();
        conn.close();
    }
}
```

# API 详解

## DriverManager

作用：

- 注册驱动
- 获取数据库连接

### 注册驱动

| static void | registerDriver (Driver driver) | 使用 Driver 注册给定的驱动程序 |
| ----------- | ------------------------------ | ------------------------------ |

```java
Class.forName("com.mysql.jdbc.Driver");


// 查询Driver类源码
// 只需要加载Driver类，该静态代码块就会执行，从而进行驱动的注册
static {
    try {
        DriverManager.registerDriver(new Driver());
    } catch (SQLException var1) {
        throw new RuntimeException("Can't register driver!");
    }
}
```

> :warning: 注：
>
> * MySQL 5 之后的驱动包，可以省略注册驱动的步骤
> * 自动加载 jar 包中 `META-INF/services/java.sql.Driver` 文件中的驱动类

### 获取连接

| static Connection | getConnection(String url, String user, String password) | 尝试建立与给定数据库 URL 的连接 |
| ----------------- | ------------------------------------------------------- | ------------------------------- |

```java
String url = "jdbc:mysql://127.0.0.1:3306/student_system?useSSL=false";
// String url = "jdbc:mysql:///student_system?useSSL=false";
String user = "root";
String password = "123456";
Connection conn =  DriverManager.getConnection(url, user, password);


// url语法：
jdbc:mysql://ip地址(域名):端口号/数据库名称?参数键值对1&参数键值对2…
// 如果连接的是本机mysql服务器，并且mysql服务默认端口是3306
// 则url可以简写为：jdbc:mysql:///数据库名称?参数键值对
配置useSSL=false参数，禁用安全连接方式，解决警告提示
```

## Connection

作用：

* 获取执行 SQL 的对象
* 管理事务

### 获取执行对象

```java
// 普通执行SQL对象
Statement createStatement()
    
// 预编译SQL的执行SQL对象：防止SQL注入
PreparedStatement prepareStatement(sql)
    
// 执行存储过程的对象
CallableStatement prepareCall(sql)
```

### 事务管理

| void | setAutoCommit(boolean autoCommit) | 将此连接的自动提交模式设置为给定状态                         |
| ---- | --------------------------------- | ------------------------------------------------------------ |
| void | commit()                          | 使自上次提交/回滚以来所做的所有更改成为永久更改，并释放此 Connection 对象当前持有的所有数据库锁 |
| void | rollback()                        | 撤销当前事务中所做的所有更改，并释放此 Connection 对象当前持有的所有数据库锁 |

```java
String sql1 = "update account set money = 3000 where id = 1";
String sql2 = "update account set money = 3000 where id = 2";
// 4、获取执行sql的对象: Statement
Statement stmt = conn.createStatement();
try
{
    // 开启事务
    conn.setAutoCommit(false);

    // 5、执行sql
    int count1 = stmt.executeUpdate(sql1);
    System.out.println(count1);
    int i = 3/0;
    // 5、执行sql
    int count2 = stmt.executeUpdate(sql2);
    System.out.println(count2);

    // 提交事务
    conn.commit();
} catch (Exception e) {
    // 回滚事务
    conn.rollback();
    e.printStackTrace();
}
// 7、释放资源
stmt.close();
conn.close();
```

## Statement

作用：用来执行 SQL 语句

| int       | executeUpdate(String sql) | 执行给定的 SQL 语句，这可能是 INSERT，UPDATE 或者 DELETE 语句，或者不返回任何内容，如 SQL DDL语句的 SQL 语句 |
| --------- | ------------------------- | ------------------------------------------------------------ |
| ResultSet | executeQuery(String sql)  | 执行给定的 SQL 语句，该语句返回单个 ResultSet 对象           |

```java
int   executeUpdate(sql): 执行DML、DDL语句
返回值：1、DML语句影响的行数2、DDL语句执行后，执行成功也可能返回0

ResultSet   executeQuery(sql): 执行DQL语句
返回值：ResultSet 结果集对象
```

## ResultSet

作用：封装了 DQL 查询语句的结果

```java
ResultSet   stmt.executeQuery(sql): 执行DQL语句，返回ResultSet对象
```

获取查询结果

```java
boolean  next()   1、将光标从当前位置向前移动一行 2、判断当前行是否为有效行
➡︎ 返回值：
   true：有效航，当前行有数据
   false：无效行，当前行没有数据

xxx  getXxx(参数)：获取数据
xxx: 数据类型；如：int getInt(参数)  String getString(参数)
➡︎ 参数：
   int：列的编号，从1开始
   String：列的名称 
```

```java
String sql = "select * from account";
// 4、获取执行sql的对象: Statement
Statement stmt = conn.createStatement();
// 5、执行sql
ResultSet res = stmt.executeQuery(sql);

// 创建集合
List<Account> list = new ArrayList<>();

// 6、处理结果：遍历res中的所有数据
while (res.next()) {
    // 创建对象
    Account account = new Account();
    // 获取数据
    int id = res.getInt(1);
    String name = res.getString(2);
    double money = res.getDouble(3);
    // int id = res.getInt("id");
    // String name = res.getString("name");
    // double money = res.getDouble("money");
    // System.out.println(id + "-" + name + "-" + money);

    // 赋值
    account.setId(id);
    account.setName(name);
    account.setMoney(money);
    // 存入集合
    list.add(account);
}
System.out.println(list);
// 7、释放资源
res.close();
stmt.close();
conn.close();
```

## PreparedStatement

作用：预编译 SQL 语句并执行，预防 SQL 注入问题

### SQL 注入

通过操作输入来修改事先定义好的 SQL 语句，用以达到执行代码对服务器进行攻击的方法



# 数据库连接池