---
title: oracle
date: 2024-09-20 14:44:22
categories:
  - [数据库]
tags: oracle
cover: https://pic2.zhimg.com/v2-cf44c2fdebc32c350cd4fd1c432401bd_r.jpg
---

📑 [教程 1](https://blog.csdn.net/lLlLlL__lL/article/details/132261592)

📑 [教程 2](https://blog.csdn.net/Smileaway_/article/details/118511529)

📑 [教程 3：例子多点](https://blog.csdn.net/qq_54525448/article/details/123979746)

# 数据类型

## 数字型

- `number`：长度不限，只要是数字就行
- `number(x)`：为整数，并且长度不超过 x 位
- `number(x,y)`：存在小数部分，总长度不超过 x 位，小数部分为 y 位

## 字符型

- `varchar2(x)`：长度不超过 x 位
- `char(x)`：固定长度是 x 位，不够则补空格

## 日期型

- `date`

# 查询

## 基本查询

```sql
-- 全表
select * from 表名;
select * from emp;


-- 指定字段查询
select 字段, 字段 from 表名;
select ename,job,sal from emp;


-- 条件查询（where）
select 字段 from 表名 where 条件;
select ename,job from emp where job = 'SALESMAN';


-- 多个条件查询（and）
select 字段 from 表名 where 条件1 and 条件2;
select ename,sal,job from emp where job = 'CLERK' and sal > 1000; -- 工作是 CLERK 并且工资大于 1000 的人


-- Null 值的判断
➡ is null：值是空值
➡ is not null：不是空值，0不是空值
select 字段 from 表名 where 字段 判断空值;

select comm,ename from emp where comm is not null; -- 查询有奖金的人
select ename,mgr from emp where mgr is null; -- 查询没有上级编号的人


-- 别名，方便记忆，as 可省略
select 字段 as 别名 from 表名 别名 ...;
select e.ename from emp e; -- 给表取了别名，使用字段时要用别名来声明字段


--  去重（distinct）
select distinct(字段) from 表名;
select distinct(deptno) from emp; -- 查询有哪些部门


-- 排序（order by）
➡ asc：升序，一般不加，默认升序
➡ desc：降序
select 字段 from 表名 order by 字段 asc/desc;

select sal,ename from emp order by sal asc; -- 排序工资


-- 面对空值：默认是最大值，可以使用 nulls first 和 nulls last 调整空值的顺序
select 字段 from 表名 order by 字段 asc/desc nulls first/last;
select comm,ename from emp order by comm asc nulls first; -- 排序奖金，空值排在前


-- 模糊查询（like）
➡ _：一个下划线代表一个字符
➡ %：代表全部字符
select 字段 from 表名 where 字段 like '值';

select ename from emp where ename like 'S____'; -- 查询名字是S开头的四个字母
select ename from emp where ename like 'S%'; -- 查询是S后面的全部字母，不管后面几个字母
```

## 运算

### 算数运算符

通常使用在字段中

```sql
+  -  *  /
select 字段+-*/ from 表名;

-- 给每个员工涨 500 块钱工资
select 500 + sal 新工资,sal 旧工资 from emp;
```

### 比较运算符

通常使用在条件里

```sql
>  <  >=  <=  !=

-- 查询不是部门 10 的人
select ename,deptno from emp where deptno != 10;
```

### 逻辑运算符

```sql
➡ and：与的关系，表并列，连接两个表达式，只有二者都成立才返回结果
➡ or：或的关系，连接两个表达式，只有一个成立就可以返回结果
➡ not：非的关系，用在表达式之前，表示取反

-- 查询来自部门 10 或者部门 20 的人的姓名
select ename,deptno from emp where deptno = 10 or deptno = 20;

-- 查询不是来自部门 10 或者部门 20 的人的姓名
select ename,deptno from emp where not(deptno = 10 or deptno = 20);
select ename,deptno from emp where not deptno in (10,20); -- 使用 in + or
```

### 区域条件

```sql
select 字段 from 表名 where 字段 between 条件1 and 条件2;

-- 查询工资在1000~2000之间的员工
select ename,sal from emp where sal between 1000 and 2000;
```

## 函数

### 字符串函数

```sql
-- 大小写控制（upper、lower）
➡ upper：转大写
➡ lower：转小写

select lower(ename) 小写名字 from emp;
select upper('aaaa') 大写字母 from dual;


-- 首字母大写（initcap）
select initcap(lower(ename)) 首字母大写 from emp;


-- 字符串拼接（concat、||）
select concat('Dear',ename) DearName from emp;
select 'Dear' || lower(ename) from emp;


-- 字符串提取（sunstr）
select ename,substr(ename,3,2) from emp; -- 从第3位开始，取2位


-- 字符串查找（instr）
select ename,instr(ename,'I',2) from emp; -- 最后数字表示截取几位
select instr('Hello world!','wo') from dual; -- 截取字符串 Hello world！当中的 wo


-- 返回字符的长度（length、lengthb）
select length(sname) 字符数,lengthb(sname) 字节数 from student;


-- 左右填充函数（lpad、rpad）
➡ lpad：左填充
➡ rpad：右填充

select lpad(ename,10,'*') from emp; -- 给 SMITH 右边填充 3 个星号
select rpad(ename,8,'*') from emp;


-- 去除字符串前后的字符（trim）：使用from连接
select trim('*' from rpad(ename,8,'*')) 去除星号 from emp;


-- 字符串替换（replace）
select replace(concat('Dear',ename),'Dear','-') from emp;
```



# DQL

# DML

# DDL
