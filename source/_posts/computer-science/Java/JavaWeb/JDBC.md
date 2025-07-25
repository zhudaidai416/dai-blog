---
title: JDBC
date: 2025-07-17 14:43:14
category:
  - [计算机与科学, Java, JavaWeb]
tags: JavaWeb
cover: https://daiblog.oss-cn-chengdu.aliyuncs.com/cover/6-3.jpg
---

# 简介

JDBC：使用 Java 语言操作关系型数据库的一套 API

全称：（Java DataBase Connectivity）Java 数据库连接

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/JDBC.png)

本质：

- 官方（sun 公司）定义的一套操作所有关系型数据库的规则，即接口
- 各个数据库厂商去实现这套接口，提供数据库驱动 jar 包
- 我们可以使用这套接口（JDBC）编程，真正执行的代码是驱动 jar 包中的实现类

# 快速入门

## 下载

MySQL 驱动包：[下载地址](https://downloads.mysql.com/archives/c-j)

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/MySQL驱动包.png)

## 流程

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/Java操作数据库流程.png)

- 编写 Java 代码

- Java 代码将 SQL 发送到 MySQL 服务端

- MySQL 服务端接收到 SQL 语句并执行该 SQL 语句

- 将 SQL 语句执行的结果返回给 Java 代码

## 操作

**步骤 1**：创建新的空项目

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/jdbc-步骤1.png)

**步骤 2**：对项目进行设置，JDK 版本、编译版本

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/jdbc-步骤2.png)

**步骤 3**：创建模块，指定模块的名称及位置

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/jdbc-步骤3.png)

**步骤 4**：导入驱动包，将 mysql 的驱动包放在模块下的 lib 目录（任意命名）下

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/jdbc-步骤4.png)

**步骤 5**：并将该 jar 包添加为库文件，级别选择为“模块库”

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/jdbc-步骤5.png)

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/jdbc-步骤6.png)

**步骤 6**：在 src 下创建类

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/jdbc-步骤7.png)

**步骤 7**：编写代码

```java
package com.itheima.jdbc;

import java.sql.Statement;
import java.sql.Connection;
import java.sql.DriverManager;

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
> - MySQL 5 之后的驱动包，可以省略注册驱动的步骤
> - 自动加载 jar 包中 `META-INF/services/java.sql.Driver` 文件中的驱动类

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

- 获取执行 SQL 的对象
- 管理事务

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

| void | setAutoCommit(boolean autoCommit) | 将此连接的自动提交模式设置为给定状态                                                            |
| ---- | --------------------------------- | ----------------------------------------------------------------------------------------------- |
| void | commit()                          | 使自上次提交/回滚以来所做的所有更改成为永久更改，并释放此 Connection 对象当前持有的所有数据库锁 |
| void | rollback()                        | 撤销当前事务中所做的所有更改，并释放此 Connection 对象当前持有的所有数据库锁                    |

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
    int i = 3/0; // 设置一个bug
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

| int       | executeUpdate(String sql) | 执行给定的 SQL 语句，这可能是 INSERT，UPDATE 或者 DELETE 语句，或者不返回任何内容，如 SQL DDL 语句的 SQL 语句 |
| --------- | ------------------------- | ------------------------------------------------------------------------------------------------------------- |
| ResultSet | executeQuery(String sql)  | 执行给定的 SQL 语句，该语句返回单个 ResultSet 对象                                                            |

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

### 获取查询结果

```java
boolean  next()   1、将光标从当前位置向前移动一行 2、判断当前行是否为有效行
➡︎ 返回值：
   true：有效行，当前行有数据
   false：无效行，当前行没有数据


xxx  getXxx(参数)：获取数据
➡︎ xxx: 数据类型   // 如：int getInt(参数)  String getString(参数)
➡︎ 参数：
   int：列的编号，从1开始
   String：列的名称
```

示例：

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

```java
// 模拟SQL注入
@Test
public void testLogin() throws Exception {
    String url = "jdbc:mysql:///student_system?useSSL=false";
    String user = "root";
    String password = "123456";
    Connection conn = DriverManager.getConnection(url, user, password);

    // 接收用户输入
    String name = "admin";
    String pwd = "' or '1' = '1";
    String sql = "select * from login where username = '" + name + "' and password = '" + pwd + "'";
    System.out.println(sql);
    // 4、获取执行sql的对象: Statement
    Statement stmt = conn.createStatement();
    // 5、执行sql
    ResultSet res = stmt.executeQuery(sql);
    // 6、处理结果
    if (res.next()) {
        System.out.println("登录成功~~");
    } else {
        System.out.println("登录失败~~");
    }
    // 7、释放资源
    stmt.close();
    conn.close();
}
```

### 使用

1、获取 PreparedStatement 对象

```java
// SQL语句中的参数值，使用？占位符替代
String sql = "select * from user where username = ? and password = ?";
// 通过Connection对象获取，并传入对应的sql语句
PreparedStatement pstmt = conn.prepareStatement(sql);
```

2、设置参数值

```java
// PreparedStatement对象
setXxx(参数1，参数2)：给?赋值
➡︎ Xxx：数据类型   // 如：setInt (参数1，参数2)
➡︎ 参数1：?的位置编号，从1开始
➡︎ 参数2：?的值
```

3、执行 sql

```java
executeUpdate();
executeQuery();
```

示例：

```java
@Test
public void testLogin2() throws Exception {
    String url = "jdbc:mysql:///student_system?useSSL=false";
    String user = "root";
    String password = "123456";
    Connection conn = DriverManager.getConnection(url, user, password);

    // 接收用户输入
    String name = "admin";
    String pwd = "' or '1' = '1";

    String sql = "select * from login where username = ? and password = ?";
    // 获取pstmt对象
    PreparedStatement pstmt = conn.prepareStatement(sql);
    // 设置？的值
    pstmt.setString(1, name);
    pstmt.setString(2, pwd);

    // 5、执行sql
    ResultSet res = pstmt.executeQuery();
    // 6、处理结果
    if (res.next()) {
        System.out.println("登录成功~~");
    } else {
        System.out.println("登录失败~~");
    }
    // 7、释放资源
    res.close();
    pstmt.close();
    conn.close();
}
```

### 原理

PreparedStatement 好处：

- 预编译 SQL，性能更高
- 防止 SQL 注入：将敏感字符进行转义

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/Java操作数据库流程2.png)

Java 代码操作数据库流程

- 将 sql 语句发送到 MySQL 服务器端

- MySQL 服务端会对 sql 语句进行如下操作

  - 检查 SQL 语句：检查语法是否正确

  - 编译 SQL 语句：将 SQL 语句编译成可执行的函数

    检查 SQL 和编译 SQL 花费的时间比执行 SQL 的时间还要长。如果我们只是重新设置参数，那么检查 SQL 语句和编译 SQL 语句将不需要重复执行，这样就提高了性能

  - 执行 SQL 语句

原理：

- 在获取 PreparedStatement 对象时，将 sql 语句发送给 mysql 服务器进行检查，编译（这些步骤很耗时）
- 执行时就不用再进行这些步骤了，速度更快
- 如果 sql 模板一样，则只需要进行一次检查、编译

# 数据库连接池

## 简介

数据库连接池是个容器，负责分配、管理数据库连接(Connection)

它允许应用程序重复使用一个现有的数据库连接，而不是再重新建立一个

释放空闲时间超过最大空闲时间的数据库连接来避免因为没有释放数据库连接而引起的数据库连接遗漏

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/数据库连接池.png)

好处：

- 资源重用
- 提升系统响应速度
- 避免数据库连接遗漏

## 实现

标准接口：DataSource

官方(SUN) 提供的数据库连接池标准接口，由第三方组织实现此接口

功能：获取连接

```java
Connection getConnection()
```

常见的数据库连接池：

- DBCP
- C3P0
- Druid

## Driud 使用

1、导入 jar 包：druid-1.1.12.jar

2、定义配置文件 `druid.properties`

```properties
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql:///student_system?useSSL=false&useServerPrepStmts=true
username=root
password=123456
# 初始化连接数量
initialSize=5
# 最大连接数
maxActive=10
# 最大等待时间
maxWait=3000
```

3、代码

```java
package com.itheima.druid;

import com.alibaba.druid.pool.DruidDataSourceFactory;

import javax.sql.DataSource;
import java.io.FileInputStream;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.Properties;

public class DruidDemo {
    public static void main(String[] args) throws Exception {
        // 1、导入jar包
        // 2、定义配置文件
        // 3、加载配置文件
        Properties prop = new Properties();
        prop.load(new FileInputStream("jdbc-demo/src/druid.properties"));

        // 4、获取连接池对象
        DataSource dataSource = DruidDataSourceFactory.createDataSource(prop);

        // 5、获取数据库连接 Connection
        Connection conn = dataSource.getConnection();
        // System.out.println(conn);
        // System.out.println(System.getProperty("user.dir"));

        // 获取到了连接后就可以继续做其他操作了
    }
}
```
