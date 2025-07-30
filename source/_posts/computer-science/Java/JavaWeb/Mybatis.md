---
title: Mybatis
date: 2025-07-22 17:07:37
category:
  - [计算机与科学, Java, JavaWeb]
tags: JavaWeb
cover: https://daiblog.oss-cn-chengdu.aliyuncs.com/cover/1-4.jpg
---

# [MyBatis](https://mybatis.net.cn/)

# 概述

## 概念

MyBatis 是一款优秀的持久层框架，用于简化 JDBC 开发

官网：https://mybatis.net.cn

MyBatis 本是 Apache 的一个开源项目 iBatis，2010年这个项目由 apache software foundation 迁移到了 google code，并且改名为 MyBatis ，2013年11月迁移到 Github

**持久层：**

* 负责将数据到保存到数据库的那一层代码

  以后开发我们会将操作数据库的Java代码作为持久层。而 Mybatis 就是对 jdbc 代码进行了封装

* JavaEE 三层架构：表现层、业务层、持久层

**框架：**

* 框架就是一个半成品软件，是一套可重用的、通用的、软件基础代码模型
* 在框架的基础之上构建软件编写更加高效、规范、通用、可扩展

## JDBC 缺点

* 硬编码

  * 注册驱动、获取连接

  * SQL 语句

* 操作繁琐

  * 手动设置参数

  * 手动封装结果集


## Mybatis 优化

* 硬编码可以配置到配置文件
* 操作繁琐的地方 mybatis 都自动完成

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/Mybatis.png)

# 快速入门

1、创建 user 表，添加数据

2、创建模块，导入坐标

`pom.xml` 配置文件中添加依赖的坐标

```xml
<dependencies>
    <!-- mybatis 依赖-->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.5</version>
    </dependency>

    <!-- mysql 驱动-->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.46</version>
    </dependency>

    <!-- junit 单元测试-->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.13</version>
        <scope>test</scope>
    </dependency>

    <!-- 添加slf4j日志api -->
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>1.7.20</version>
    </dependency>

    <!-- 添加logback-classic依赖 -->
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
        <version>1.2.3</version>
    </dependency>

    <!-- 添加logback-core依赖 -->
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-core</artifactId>
        <version>1.2.3</version>
    </dependency>
</dependencies>
```

> :warning: 注：在项目的 resources 目录下创建 `logback.xml` 配置文件
>
> ```xml
> <?xml version="1.0" encoding="UTF-8"?>
> <configuration>
>     <!-- CONSOLE ：表示当前的日志信息是可以输出到控制台的。 -->
>     <appender name="Console" class="ch.qos.logback.core.ConsoleAppender">
>         <encoder>
>             <pattern>[%level] %blue(%d{HH:mm:ss.SSS}) %cyan([%thread]) %boldGreen(%logger{15}) - %msg %n</pattern>
>         </encoder>
>     </appender>
> 
>     <logger name="com.itheima" level="DEBUG" additivity="false">
>         <appender-ref ref="Console"/>
>     </logger>
> 
> 
>     <!-- level:用来设置打印级别，大小写无关：TRACE, DEBUG, INFO, WARN, ERROR, ALL 和 OFF， 默认debug
>       <root>可以包含零个或多个<appender-ref>元素，标识这个输出位置将会被本日志级别控制。
>     -->
>     <root level="DEBUG">
>         <appender-ref ref="Console"/>
>     </root>
> </configuration>
> ```

3、编写 MyBatis 核心配置文件 ➡︎ 替换连接信息（解决硬编码问题）

在模块的 resources 目录下创建 `mybatis-config.xml`（可命名） 配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- environments：配置数据库连接环境信息。可以配置多个environment，通过default属性切换不同的environment -->
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <!-- 数据库连接信息 -->
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql:///student_system?useSSL=false"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>
    
    <mappers>
        <!-- 加载sql映射文件 -->
        <mapper resource="UserMapper.xml"/>
    </mappers>
</configuration>
```

4、编写 SQL 映射文件 ➡︎ 统一管理 sql 语句（解决硬编码问题）

在模块的 resources 目录下创建映射配置文件 `UserMapper.xml`

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- namespace：名称空间 -->
<mapper namespace="test">
    <select id="selectAll" resultType="com.itheima.pojo.User">
        select *
        from tb_user;
    </select>
</mapper>
```

5、编码

在 `com.itheima.pojo` 包下，创建 User 类

```java
public class User {
    private Integer id;
    private String username;
    private String password;
    private String gender;
    private String addr;
    
    // ...省略了setter和getter
}
```

编写 MybatisDemo 测试类

```java
package com.itheima;

import com.itheima.pojo.User;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

/**
 * Mybatis 快速入门
 */
public class MybatisDemo {
    public static void main(String[] args) throws IOException {
        // 1、加载mybatis的核心配置文件，获取SqlSessionFactory
        String resource = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        // 2、获取SqlSession对象，用它来执行sql
        SqlSession sqlSession = sqlSessionFactory.openSession();
        // 3、执行sql
        List<User> users = sqlSession.selectList("test.selectAll"); // 参数是一个字符串，该字符串必须是映射配置文件的namespace.id
        System.out.println(users);
        // 4、释放资源
        sqlSession.close();
    }
}
```

**解决 SQL 映射文件的警告提示：**

* 产生的原因：Idea 和数据库没有建立连接，不识别表信息，但它并不影响程序的执行
* 解决方式：在 Idea 中配置 MySQL 数据库连接

![解决SQL映射文件警告1](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/解决SQL映射文件警告1.png)

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/解决SQL映射文件警告2.png)

# Mapper 代理开发

## 开发概述

前面所写的代码是基本使用方式，也存在硬编码的问题

```java
// 这里调用selectList()方法传递的参数是映射配置文件中的namespace.id值，这样写也不便于后期的维护
List<User> users = sqlSession.selectList("test.selectAll");
System.out.println(users);
```

Mapper 代理方式的目的：

* 解决原生方式中的硬编码
* 简化后期执行 SQL

## 使用方式

1、定义与 SQL 映射文件同名的 Mapper 接口，并且将 Mapper 接口和 SQL 映射文件放置在同一目录下

在 `resources` 下创建 `com/itheima/mapper` 目录（这样才能放置在同一目录下），并在该目录下创建 `UserMapper.xml` 映射配置文件

原本路径：resources 根目录下

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/Mapper代理开发使用1.png)

2、设置 SQL 映射文件的 namespace 属性为 Mapper 接口全限定名

```xml
<!-- namespace：名称空间，必须是对应接口的全限定名 -->
<!-- 原本写法 -->
<mapper namespace="test">
    <select id="selectAll" resultType="com.itheima.pojo.User">
        select *
        from tb_user;
    </select>
</mapper>

<!-- 修改 -->
<mapper namespace="com.itheima.mapper.UserMapper">
    <select id="selectAll" resultType="com.itheima.pojo.User">
        select *
        from tb_user;
    </select>
</mapper>
```

3、在 Mapper 接口中定义方法，方法名就是 SQL 映射文件中 sql 语句的 id，并保持参数类型和返回值类型一致

在 `com.itheima.mapper` 包下，创建 UserMapper 接口

```java
package com.itheima.mapper;

import com.itheima.pojo.User;

import java.util.List;

public interface UserMapper {
    List<User> selectAll();
}
```

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/Mapper代理开发使用2.png)

4、在 `com.itheima` 包下，创建 MybatisDemo2 测试类

```java
package com.itheima;

import com.itheima.mapper.UserMapper;
import com.itheima.pojo.User;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

/**
 * Mybatis 代理开发
 */
public class MybatisDemo2 {
    public static void main(String[] args) throws IOException {
        // 1、加载mybatis的核心配置文件，获取SqlSessionFactory
        String resource = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        // 2、获取SqlSession对象，用它来执行sql
        SqlSession sqlSession = sqlSessionFactory.openSession();
        
        // 3、执行sql
        // List<User> users = sqlSession.selectList("test.selectAll"); // 参数是一个字符串，该字符串必须是映射配置文件的namespace.id
        
        // 获取UserMapper接口的代理对象
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        List<User> users = userMapper.selectAll();

        System.out.println(users);
        // 4、释放资源
        sqlSession.close();
    }
}
```

> :warning: 注：如果 Mapper 接口名称和 SQL 映射文件名称相同，并在同一目录下，则可以使用包扫描的方式简化 SQL 映射文件的加载，也就是将核心配置文件的加载映射配置文件的配置修改为
>
> ```xml
> <mappers>
>     <!-- 加载sql映射文件 -->
>     <!-- <mapper resource="UserMapper.xml"/> -->
>     
>     <!-- Mapper代理方式 -->
>     <!-- <mapper resource="com/itheima/mapper/UserMapper.xml"/> -->
> 
>     <!-- Mapper代理方式简化写法 -->
>     <package name="com.itheima.mapper"/>
> </mappers>
> ```

# [Mybatis 核心配置文件](https://mybatis.net.cn/configuration.html)

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/Mapper核心配置文件.png)

## 多环境配置

在核心配置文件的 `environments` 标签中可以配置多个 `environment`，使用 `id` 给每段环境起名

使用 `default='环境id'` 来指定使用哪个配置

```xml
<!-- environments：配置数据库连接环境信息。可以配置多个environment，通过default属性切换不同的environment -->
<environments default="development">
    <environment id="development">
        <transactionManager type="JDBC"/>
        <dataSource type="POOLED">
            <!-- 数据库连接信息 -->
            <property name="driver" value="com.mysql.jdbc.Driver"/>
            <property name="url" value="jdbc:mysql:///student_system?useSSL=false"/>
            <property name="username" value="root"/>
            <property name="password" value="123456"/>
        </dataSource>
    </environment>

    <environment id="test">
        <transactionManager type="JDBC"/>
        <dataSource type="POOLED">
            <!-- 数据库连接信息 -->
            <property name="driver" value="com.mysql.jdbc.Driver"/>
            <property name="url" value="jdbc:mysql:///student_system?useSSL=false"/>
            <property name="username" value="root"/>
            <property name="password" value="123456"/>
        </dataSource>
    </environment>
</environments>
```

## 类型别名

在映射配置文件中的 `resultType` 属性需要配置数据封装的类型（类的全限定名）

Mybatis 提供了 <font color=red>类型别名</font>（typeAliases）可以简化书写

1、首先需要现在核心配置文件中配置类型别名，也就意味着给 pojo 包下所有的类起了别名（别名就是类名），不区分大小写

```xml
<typeAliases>
    <!-- name属性的值是实体类所在包 -->
    <package name="com.itheima.pojo"/> 
</typeAliases>
```

2、通过上述的配置，就可以简化映射配置文件中 `resultType` 属性值的编写

```xml
<!-- 原先 -->
<mapper namespace="com.itheima.mapper.UserMapper">
    <select id="selectAll" resultType="com.itheima.pojo.User">
        select * from tb_user;
    </select>
</mapper>

<!-- 简化后 -->
<mapper namespace="com.itheima.mapper.UserMapper">
    <select id="selectAll" resultType="user">
        select * from tb_user;
    </select>
</mapper>
```

# 安装 MyBatisX 插件

* MybatisX 是一款基于 IDEA 的快速开发插件，为效率而生

* 主要功能

  * XML 映射配置文件和接口方法间相互跳转
  * 根据接口方法生成 statement

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/MyBatisX插件1.png)

- 红色头绳：表示映射配置文件
- 蓝色头绳：表示 mapper 接口

在 mapper 接口点击红色头绳的小鸟图标会自动跳转到对应的映射配置文件

在映射配置文件中点击蓝色头绳的小鸟图标会自动跳转到对应的 mapper 接口

也可以在 mapper 接口中定义方法，自动生成映射配置文件中的 `statement`

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/MyBatisX插件2.png)

# 配置文件实现 CRUD

## 环境准备

1、数据库准备

```sql
-- 删除tb_brand表
drop table if exists tb_brand;
-- 创建tb_brand表
create table tb_brand
(
    id           int primary key auto_increment,
    brand_name   varchar(20),
    company_name varchar(20),
    ordered      int,
    description  varchar(100),
    status       int
);
-- 添加数据
insert into tb_brand (brand_name, company_name, ordered, description, status)
values ('三只松鼠', '三只松鼠股份有限公司', 5, '好吃不上火', 0),
       ('华为', '华为技术有限公司', 100, '华为致力于把数字世界带入每个人、每个家庭、每个组织，构建万物互联的智能世界', 1),
       ('小米', '小米科技有限公司', 50, 'are you ok', 1);
```

2、实体类 Brand

在 `com.itheima.pojo` 包下创建 Brand 实体类

```java
public class Brand {
    // id 主键
    private Integer id;
    // 品牌名称
    private String brandName;
    // 企业名称
    private String companyName;
    // 排序字段
    private Integer ordered;
    // 描述信息
    private String description;
    // 状态：0：禁用  1：启用
    private Integer status;
    
    //...省略 setter and getter
}
```

3、编写测试用例

测试代码需要在 `test/java` 目录下创建包及测试用例：com.itheima.test.MyBatisTest

## 查询所有数据

### 接口方法

- 在 `com.itheima.mapper` 包下，创建 BrandMapper 接口


```java
public interface BrandMapper {
    // 查询所有
    List<Brand> selectAll();
}
```

### SQL 语句

- 在 reources 下创建 `com/itheima/mapper` 目录结构，并创建 `BrandMapper.xml` 的映射配置文件


```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.itheima.mapper.BrandMapper">
    <select id="selectAll" resultType="brand">
        select *
        from tb_brand;
    </select>
</mapper>
```

### 测试方法

- 在 `test\java` 下，创建 `com.itheima.test.MybatisTest` 测试类


```java
@Test
public void testSelectAll() throws IOException {
    // 1、获取SqlSessionFactory
    String resource = "mybatis-config.xml";
    InputStream inputStream = Resources.getResourceAsStream(resource);
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
    
    // 2、获取SqlSession对象
    SqlSession sqlSession = sqlSessionFactory.openSession();
    
    // 3、获取Mapper接口的代理对象
    BrandMapper brandMapper = sqlSession.getMapper(BrandMapper.class);

    // 4、执行方法
    List<Brand> brands = brandMapper.selectAll();
    System.out.println(brands);
    
    // 5、释放资源
    sqlSession.close();
}
```

### 查询结果

![image-20210729172544230](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20210729172544230.png)

数据库的字段名称和实体类的属性名称不一样，则不能自动封装数据，会显示空值

![image-20210729173210433](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20210729173210433.png)

**解决方法**：

1、起别名

```xml
<select id="selectAll" resultType="brand">
    select id, brand_name brandName, company_name companyName, ordered, description, status
    from tb_brand;
</select>
```

2、sql 片段

```xml
<sql id="brand_column">
    id, brand_name brandName, company_name companyName, ordered, description, status
</sql>
<select id="selectAll" resultType="brand">
    select
    <include refid="brand_column"/>
    from tb_brand;
</select>
```

3、resultMap：定义不一致的属性名和列名的映射关系

```xml
<resultMap id="brandResultMap" type="brand">
    <result column="brand_name" property="brandName"/>
    <result column="company_name" property="companyName"/>
</resultMap>
<select id="selectAll" resultMap="brandResultMap">
    select *
    from tb_brand;
</select>
```

## 查询详情

### 接口方法

- BrandMapper 接口


```java
public interface BrandMapper {
    // 查看详情
    Brand selectById(int id);
}
```

### SQL 语句

- BrandMapper.xml


```xml
<select id="selectById" resultMap="brandResultMap">
    select *
    from tb_brand
    where id = #{id};
</select>
```

### 测试方法

- MybatisTest 测试类


```java
@Test
public void testSelectById() throws IOException {
    // 接收参数
    int id = 1;
    
    // 省略...
    
    // 4、执行方法
    Brand brand = brandMapper.selectById(id);
    System.out.println(brand);
    
    // 省略...
}
```

### 参数占位符

- `#{} `：会将 ` #{}` 占位符替换为 `?`，将来自动设置参数值
  - 底层使用的是 PreparedStatement，为了防止 SQL 注入
- `${}` ：拼接 SQL，
  - 底层使用的是 Statement，会存在 SQL 注入

```xml
<select id="selectById" resultMap="brandResultMap">
    select *
    from tb_brand
    where id = #{id};
</select>

<select id="selectById" resultMap="brandResultMap">
    select *
    from tb_brand
    where id = ${id};
</select>
```

使用时机：

- 参数传递：#{}
- 表名或列名不固定，只能进行 sql 拼接：${}

### parameterType 使用

用于设置参数类型，该参数可以省略

```xml
<select id="selectById" parameterType="int" resultMap="brandResultMap">
    select *
    from tb_brand
    where id = #{id};
</select>
```

### 特殊字段处理

```xml
<!-- 方法一：转义字符 -->
<select id="selectById" resultMap="brandResultMap">
    select *
    from tb_brand
    where id &lt; #{id};
</select>

<!-- 方法二：<![CDATA[内容]]> -->
<select id="selectById" resultMap="brandResultMap">
    select *
    from tb_brand
    where id <![CDATA[<]]> #{id};
</select>
```

## 条件查询

### 接口方法

散装参数：使用 `@Param("参数名称")` 标记每一个参数

实体类封装参数：将多个参数封装成一个实体对象

Map 集合：将多个参数封装到 Map 集合中

- BrandMapper 接口

```java
public interface BrandMapper {
    // 条件查询
    // 散装参数
    List<Brand> selectByCondition(@Param("status") int status,
                                  @Param("companyName") String companyName,
                                  @Param("brandName") String brandName);

    // 对象参数：sql的参数名和实体类的属性名一致
    List<Brand> selectByCondition(Brand brand);

    // map集合参数：sql的参数名和map集合的键的名称一致
    List<Brand> selectByCondition(Map map);
}
```

### SQL 语句

- BrandMapper.xml


```xml
<select id="selectByCondition" resultMap="brandResultMap">
    select *
    from tb_brand
    where status = #{status}
      and company_name like #{companyName}
      and brand_name like #{brandName}
</select>
```

### 测试方法

- MybatisTest 测试类


```java
@Test
public void testSelectByCondition() throws IOException {
    // 接收参数
    int status = 1;
    String companyName = "华为";
    String brandName = "华为";
    // 处理参数
    companyName = "%" + companyName + "%";
    brandName = "%" + brandName + "%";

    // Brand对象
    // Brand brand = new Brand();
    // brand.setStatus(status);
    // brand.setCompanyName(companyName);
    // brand.setBrandName(brandName);

    // Map集合
    Map map = new HashMap();
    map.put("status", status);
    map.put("companyName", companyName);
    map.put("brandName", brandName);

    // 省略...
    
    // 4、执行方法
    // 方法一：散装参数
    List<Brand> brands = brandMapper.selectByCondition(status, companyName, brandName);
    // 方法二：对象参数
    List<Brand> brands = brandMapper.selectByCondition(brand);
    // 方法三：Map集合参数
    List<Brand> brands = brandMapper.selectByCondition(map);
    System.out.println(brands);
    
    // 省略...
}
```

### 动态 SQL

* if

* choose (when, otherwise)

* trim (where, set)

* foreach

#### if 标签

用于判断参数是否有值，使用 test 属性进行条件判断

- test 属性：逻辑表达式

```xml
<select id="selectByCondition" resultMap="brandResultMap">
    select *
    from tb_brand
    where
    <if test="status != null">
        status = #{status}
    </if>
    <if test="companyName != null and companyName != '' ">
        and company_name like #{companyName}
    </if>
    <if test="brandName != null and brandName != '' ">
        and brand_name like #{brandName}
    </if>
</select>
```

**存在问题**：第一个条件不需要逻辑运算符

```sql
-- 如果参数值没有status，此时拼接的SQL语句
select * from tb_brand where and company_name like ? and brand_name like ?
```

**解决方法**：

- 方法一：使用恒等式让所有条件格式都一样

```xml
<select id="selectByCondition" resultMap="brandResultMap">
    select *
    from tb_brand
    where 1 = 1
    <if test="status != null">
        and status = #{status}
    </if>
    <if test="companyName != null and companyName != '' ">
        and company_name like #{companyName}
    </if>
    <if test="brandName != null and brandName != '' ">
        and brand_name like #{brandName}
    </if>
</select>
```

- 方法二：where 标签替换 where 关键字

#### where 标签

- 替换 where 关键字
- 会动态的去掉第一个条件前的 and
- 如果所有的参数没有值则不加 where 关键字

> :warning: 注：需要给每个条件前都加上 and 关键字

```xml
<select id="selectByCondition" resultMap="brandResultMap">
    select *
    from tb_brand
    <where>
        <if test="status != null">
            and status = #{status}
        </if>
        <if test="companyName != null and companyName != '' ">
            and company_name like #{companyName}
        </if>
        <if test="brandName != null and brandName != '' ">
            and brand_name like #{brandName}
        </if>
    </where>
</select>
```

## 单个条件（动态SQL）

### 接口方法

- BrandMapper 接口


```java
public interface BrandMapper {
    // 单个条件查询
    List<Brand> selectByConditionSingle(Brand brand);
}
```

### SQL 语句

- BrandMapper.xml


```xml
<select id="selectByConditionSingle" resultMap="brandResultMap">
    select *
    from tb_brand
    <where>
        <choose> <!-- 相当于switch -->
            <when test="status != null"> <!-- 相当于case -->
                and status = #{status}
            </when>
            <when test="companyName != null and companyName != '' ">
                and company_name like #{companyName}
            </when>
            <when test="brandName != null and brandName != '' ">
                and brand_name like #{brandName}
            </when>
            <!-- 相当于default，使用where标签可以不写 -->
            <otherwise>
                1 = 1
            </otherwise>
        </choose>
    </where>
</select>
```

### 测试方法

- MybatisTest 测试类


```java
@Test
public void testselectByConditionSingle() throws IOException {
    // 接收参数
    int status = 1;
    String companyName = "华为";
    String brandName = "华为";
    // 处理参数
    companyName = "%" + companyName + "%";
    brandName = "%" + brandName + "%";

    // Brand对象
    Brand brand = new Brand();
    brand.setStatus(status);
    // brand.setCompanyName(companyName);
    // brand.setBrandName(brandName);

    // 省略...
    
    // 4、执行方法
    // 对象参数
    List<Brand> brands = brandMapper.selectByConditionSingle(brand);
    System.out.println(brands);
    
    // 省略...
}
```

## 添加数据

### 接口方法

- BrandMapper 接口


```java
public interface BrandMapper {
    // 添加数据
    void add(Brand brand);
}
```

### SQL 语句

- BrandMapper.xml


```xml
<insert id="add">
    insert into tb_brand (brand_name, company_name, ordered, description, status)
    values (#{brandName}, #{companyName}, #{ordered}, #{description}, #{status});
</insert>
```

### 测试方法

- MybatisTest 测试类


```java
@Test
public void testAdd() throws IOException {
    // 接收参数
    int status = 1;
    String companyName = "波导手机";
    String brandName = "波导";
    String description = "手机中的战斗机";
    int ordered = 100;

    // Brand对象
    Brand brand = new Brand();
    brand.setStatus(status);
    brand.setCompanyName(companyName);
    brand.setBrandName(brandName);
    brand.setDescription(description);
    brand.setOrdered(ordered);

    // 1、获取SqlSessionFactory
    String resource = "mybatis-config.xml";
    InputStream inputStream = Resources.getResourceAsStream(resource);
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
    // 2、获取SqlSession对象
    SqlSession sqlSession = sqlSessionFactory.openSession();
    // SqlSession sqlSession = sqlSessionFactory.openSession(true); // 设置自动提交事务，这种情况不需要手动提交事务了
    // 3、获取Mapper接口的代理对象
    BrandMapper brandMapper = sqlSession.getMapper(BrandMapper.class);
    // 4、执行方法
    brandMapper.add(brand);
    Integer id = brand.getId(); // 主键返回
    System.out.println(id);
    // 提交事务
    sqlSession.commit();
    // 5、释放资源
    sqlSession.close();
}
```

### 添加-主键返回

在数据添加成功后，有时候需要获取插入数据库数据的主键（主键是自增长）

在 insert 标签上添加属性：

- useGeneratedKeys：是否获取自动增长的主键值，true 表示获取
- keyProperty ：指定将获取到的主键值封装到哪儿个属性里

```xml
<insert id="add" useGeneratedKeys="true" keyProperty="id">
    insert into tb_brand (brand_name, company_name, ordered, description, status)
    values (#{brandName}, #{companyName}, #{ordered}, #{description}, #{status});
</insert>
```

## 修改数据

### 接口方法

- BrandMapper 接口


```java
public interface BrandMapper {
    // 修改数据
    int update(Brand brand);
}
```

### SQL 语句

set 标签可以用于动态包含需要更新的列，忽略其它不更新的列

- BrandMapper.xml

```xml
<update id="update">
    update tb_brand
    <set>
        <if test="brandName != null and brandName != ''">
            brand_name = #{brandName},
        </if>
        <if test="companyName != null and companyName != ''">
            company_name = #{companyName},
        </if>
        <if test="ordered != null">
            ordered = #{ordered},
        </if>
        <if test="description != null and description != ''">
            description = #{description},
        </if>
        <if test="status != null">
            status = #{status}
        </if>
    </set>
    where id = #{id}
</update>
```

### 测试方法

- MybatisTest 测试类


```java
@Test
public void testUpdate() throws IOException {
    // 接收参数
    int status = 0;
    String companyName = "波导手机";
    String brandName = "波导";
    String description = "波导手机";
    int ordered = 200;
    int id = 5;

    // Brand对象
    Brand brand = new Brand();
    brand.setStatus(status);
    // brand.setCompanyName(companyName);
    // brand.setBrandName(brandName);
    brand.setDescription(description);
    brand.setOrdered(ordered);
    brand.setId(id);

    // 1、获取SqlSessionFactory
    String resource = "mybatis-config.xml";
    InputStream inputStream = Resources.getResourceAsStream(resource);
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
    // 2、获取SqlSession对象
    SqlSession sqlSession = sqlSessionFactory.openSession();
    // 3、获取Mapper接口的代理对象
    BrandMapper brandMapper = sqlSession.getMapper(BrandMapper.class);
    // 4、执行方法
    int count = brandMapper.update(brand);
    System.out.println(count);
    // 提交事务
    sqlSession.commit();
    // 5、释放资源
    sqlSession.close();
}
```

## 删除一行数据

### 接口方法

- BrandMapper 接口


```java
public interface BrandMapper {
    // 删除一行数据
    void deleteById(int id);
}
```

### SQL 语句

- BrandMapper.xml


```xml
<delete id="deleteById">
    delete
    from tb_brand
    where id = #{id}
</delete>
```

### 测试方法

- MybatisTest 测试类


```java
@Test
public void testDeleteById() throws IOException {
    // 接收参数
    int id = 4;

    // 1、获取SqlSessionFactory
    String resource = "mybatis-config.xml";
    InputStream inputStream = Resources.getResourceAsStream(resource);
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
    // 2、获取SqlSession对象
    SqlSession sqlSession = sqlSessionFactory.openSession();
    // 3、获取Mapper接口的代理对象
    BrandMapper brandMapper = sqlSession.getMapper(BrandMapper.class);
    // 4、执行方法
    brandMapper.deleteById(id);
    // 提交事务
    sqlSession.commit();
    // 5、释放资源
    sqlSession.close();
}
```

## 批量删除

### 接口方法

- BrandMapper 接口


```java
public interface BrandMapper {
    // 批量删除：参数是一个数组，数组中存储的是多条数据的id
    void deleteByIds(int[] ids);
}
```

### SQL 语句

编写 SQL 时需要遍历数组来拼接 SQL 语句，Mybatis 提供了 `foreach` 标签

**foreach 标签**：用来迭代任何可迭代的对象（如数组，集合）

* collection 属性：
  * mybatis 会将数组参数，封装为一个 Map 集合
    * 默认：array = 数组
    * 使用@Param注解改变 Map 集合的默认 key 的名称
* item 属性：本次迭代获取到的元素
* separator 属性：集合项迭代之间的分隔符。`foreach` 标签不会错误地添加多余的分隔符，也就是最后一次迭代不会加分隔符
* open 属性：拼接 SQL 语句之前拼接的语句，只会拼接一次
* close 属性：拼接 SQL 语句之后拼接的语句，只会拼接一次

🎀

- BrandMapper.xml

```xml
<!-- 🎀写法一：void deleteByIds(int[] ids); -->
<delete id="deleteByIds">
    delete
    from tb_brand
    where id in
    <foreach collection="array" item="id" separator="" open="(" close=")">
        #{id}
    </foreach>
</delete>

或者

<!-- 🎀写法二：void deleteByIds(@Param("ids") int[] ids); -->
<delete id="deleteByIds">
    delete
    from tb_brand
    where id in
    <foreach collection="ids" item="id" separator="" open="(" close=")">
        #{id}
    </foreach>
</delete>
```

### 测试方法

- MybatisTest 测试类


```java
@Test
public void testDeleteByIds() throws IOException {
    // 接收参数
    int[] ids = {6, 9, 10};

    // 1、获取SqlSessionFactory
    String resource = "mybatis-config.xml";
    InputStream inputStream = Resources.getResourceAsStream(resource);
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
    // 2、获取SqlSession对象
    SqlSession sqlSession = sqlSessionFactory.openSession();
    // 3、获取Mapper接口的代理对象
    BrandMapper brandMapper = sqlSession.getMapper(BrandMapper.class);
    // 4、执行方法
    brandMapper.deleteByIds(ids);
    // 提交事务
    sqlSession.commit();
    // 5、释放资源
    sqlSession.close();
}
```

# Mybatis 参数传递

* 多个参数
* 单个参数
  * POJO 类型
  * Map 集合类型
  * Collection 集合类型
  * List 集合类型
  * Array 类型
  * 其他类型

## 多个参数

接收多个参数需要使用 `@Param` 注解，为什么要加该注解？

ParamNameResolver

```java
User selectByCondition(@Param("username") String username,@Param("password") String password);
```

```xml
<select id="selectByCondition" resultType="user">
	select *
    from tb_user
    where username=#{username}
    	and password=#{password}
</select>
```

（Mybatis 底层）Mybatis 会将这些参数封装成 Map 集合对象，值就是参数值，而键在没有使用 `@Param` 注解时，有以下命名规则：

- 以 arg 开头

  ```java
  // 第一个参数就叫 arg0，第二个参数就叫 arg1，依次类推
  map.put("arg0", 参数值1);
  map.put("arg1", 参数值2);
  ```

- 以 param 开头

  ```java
  // 第一个参数就叫 param1，第二个参数就叫 param2，依次类推
  map.put("param1", 参数值1);
  map.put("param2", 参数值2);
  ```

**代码：**

```java
User selectByCondition(String username, String password);
```

```xml
<select id="select" resultType="user">
    select *
    from tb_user
    where username = #{arg0}
      and password = #{arg1}
</select>
或者
<select id="select" resultType="user">
    select *
    from tb_user
    where username = #{param1}
      and password = #{param2}
</select>
```

在接口方法参数上使用 `@Param` 注解，Mybatis 会将 `arg` 开头的键名替换为对应注解的属性值

**代码：**

```java
User selectByCondition(@Param("username") String username, String password);
```

> ```java
> // Mybatis 在封装 Map 集合时，键名就会变成如下：
> map.put("username", 参数值1);
> map.put("arg1", 参数值2);
> map.put("param1", 参数值1);
> map.put("param2", 参数值2);
> ```

```xml
<select id="select" resultType="user">
	select *
    from tb_user
    where username=#{username}
      and password=#{param2}
</select>
```

结论：接口参数是多个时，在每个参数上都使用 `@Param` 注解，这样代码的可读性更高

## 单个参数

* POJO 类型：直接使用，要求<font color=red>属性名</font>和<font color=red>参数占位符名称</font>一致

* Map 集合类型：直接使用，要求 <font color=red>Map 集合的键名</font>和<font color=red>参数占位符名称</font>一致

* Collection 集合类型：Mybatis 会将集合封装到 Map 集合

  ```java
  // 可以使用@Param注解替换Map集合中默认的arg键名
  map.put("arg0"，collection集合);
  map.put("collection"，collection集合;
  ```

* List 集合类型：Mybatis 会将集合封装到 Map 集合中

  ```java
  // 可以使用@Param注解替换Map集合中默认的arg键名
  map.put("arg0", list集合);
  map.put("collection", list集合);
  map.put("list", list集合);
  ```

* Array 类型：Mybatis 会将集合封装到 Map 集合中

  ```java
  // 可以使用@Param注解替换Map集合中默认的arg键名
  map.put("arg0", 数组);
  map.put("array", 数组);
  ```

* 其他类型

  比如 int 类型，参数占位符名称叫什么都可以，尽量做到见名知意

# 注解实现 CRUD

使用注解开发会比配置文件开发更加方便

```java
@Select(value = "select * from tb_user where id = #{id}")
public User select(int id);
```

> :warning: 注：​注解是用来替换映射配置文件方式配置的，所以使用了注解，就不需要再映射配置文件中书写对应的 statement

Mybatis 针对 CURD 操作都提供了对应的注解

- 查询 ：@Select
- 添加 ：@Insert
- 修改 ：@Update
- 删除 ：@Delete

**使用**：

- 注解：完成简单功能
- 配置文件：完成复杂功能

动态 SQL 就是复杂的功能，如果用注解使用的话，就需要使用到 Mybatis 提供的SQL构建器来完成，例子如下：

![image-20210805234842497](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/image-20210805234842497.png)
