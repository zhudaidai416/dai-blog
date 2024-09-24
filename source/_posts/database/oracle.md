---
title: oracle
date: 2024-09-20 14:44:22
categories:
  - [数据库]
tags: oracle
cover: https://pic2.zhimg.com/v2-cf44c2fdebc32c350cd4fd1c432401bd_r.jpg
---

📑 [教程 1](https://blog.csdn.net/lLlLlL__lL/article/details/132261592)

📑 [教程2](https://blog.csdn.net/qq_54525448/article/details/123979746)

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

- 必须是英文符号
- 养成习惯分号结尾
- 表名、字段名不区分大小写，内容区分大小写

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
select ename,substr(ename,3,2) from emp; -- 从第三位开始，截取两位


-- 字符串查找（instr）
select instr(被处理的内容,'截取的内容',截取位数) from emp;

select ename,instr(ename,'I',2) from emp; -- 如果不加最后一个截取个数，返回的值是截取的是第几位
select instr('Hello world!','wo') from dual; -- 截取字符串 Hello world！当中的 wo


-- 返回字符的长度（length、lengthb）：英文的字符等于字节数，中文的1/2
select length(sname) 字符数,lengthb(sname) 字节数 from student;


-- 左右填充函数（lpad，rpad）
➡ lpad：左填充
➡ rpad：右填充

select lpad(ename,10,'*') from emp;
select rpad(ename,8,'*') from emp; -- 给 SMITH 右边填充 3 个星号（填充到8位字符）


-- 去除字符串前后的字符（trim）
select trim('被去除的内容' from 被处理的内容 字段 from emp;

select trim('*' from rpad(ename,8,'*')) 去除星号 from emp;
            
-- 字符串替换（replace）
select replace(需要被替换的内容,'被替换的字符','替换后的字符') from emp;

select replace(concat('Dear',ename),'Dear','-') from emp;
```

### 数学函数

```sql
-- 四舍五入（round）
➡ 截断（trunc）
➡ 求余（mod）：mod(除数，被除数)
➡ 取整（ceil、floor）
    ceil：向上取整，去除小数位整数位+1
    floor：向下取整，去除小数位

select round(123.456) from dual;
select trunc(123.456) from dual; -- 123
select mod(100,3) from dual; -- 3
select ceil(123.123),floor(456.789) from dual; -- 124 456
```

### 日期函数

```sql
-- 日期函数：显示当前日期和时间（sysdate、systimestamp）
select sysdate,systimestamp from dual;


-- 返回当前时间和日期（current_date、current_timestamp、localtimestamp）
select current_date,current_timestamp,localtimestamp from dual;


-- 给指定的日期添加月份（add_months）
add_months(原来时间,增加的月份)

select hiredate,add_months(hiredate,2) from emp;


-- 当前月的最后一天（last_day）
select hiredate,last_day(hiredate) from emp;


-- 抽取日期的单位（extract）
select extract(year from hiredate) year from emp;
select extract(month from hiredate) month from emp;
select extract(day from hiredate) day from emp;
select extract(hour from systimestamp) hour from dual;
select extract(minute from systimestamp) minute from dual;
select extract(second from systimestamp) second from dual;


-- 返回两个日期之间的月份（months_between）
months_between(开始时间,结束时间)
select ceil(months_between(sysdate,hiredate)) months from emp;


-- 返回下一个周几（next_day）：不是下一周是下一个
next_day(开始时间,'结束的那一天')
select next_day(sysdate,'星期五') from dual;
```

## 转换函数

```sql
-- 字符串转日期（to_date）
select to_date('2023-07-25','yyyy-mm-dd') from dual;


-- 日期转字符串（to_char）
select to_char(sysdate,'yyyy') year from dual; -- 23
select to_char(sysdate,'mm') months from dual; -- 07
select to_char(sysdate,'dd') day from dual; -- 25
select to_char(sysdate,'day') week from dual; -- 返回当前星期几 星期二


-- 字符串转数字（to_numbner）
select to_number('20230725') from dual;
select to_number('$123.456','$999.9999') from dual; -- 转有效数字
select to_number('19f','xxx') from dual; -- 16进制转10进制
```

## 通用函数

```sql
-- nvl：nvl(值1，值2) 如果值1为空，返回值2
select nvl(comm,500) from emp; -- 如果奖金为空，给他500奖金


-- nvl2：nvl2(值1，值2，值3) 如果值1为空，返回值3，否则返回值2
select nvl2(comm,comm+500,1500) 涨奖金,comm from emp;


-- nullif：nullif(值1，值2)，如果值1等于值2，返回null，反之返回值1
select nullif(500,null) from dual;


-- coalesce：coalesce(值1，值2，值3，....)返回第一个不为空的值
select coalesce(comm,deptno) from emp;
```

