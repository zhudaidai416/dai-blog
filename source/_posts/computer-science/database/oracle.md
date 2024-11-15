---
title: oracle
date: 2024-09-20 14:44:22
category:
  - [计算机与科学, 数据库]
tags: oracle
cover: https://pic2.zhimg.com/v2-cf44c2fdebc32c350cd4fd1c432401bd_r.jpg
---

📑 [教程 1](https://blog.csdn.net/lLlLlL__lL/article/details/132261592)

📑 [教程 2](https://blog.csdn.net/qq_54525448/article/details/123979746)

📑 [教程 3：例子多点](https://blog.csdn.net/qq_54525448/article/details/123979746)

# 数据类型

## 数字型

- `number`：长度不限，只要是数字就行
- `number(x)`：为整数，并且长度不超过 x 位
- `number(x,y)`：存在小数部分，总长度不超过 x 位，小数部分为 y 位

## 字符型

- `varchar2(x)`：长度不超过 x 位（为 oracle 特有）
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
select 字段 from 表名 where 字段 between 条件1 and 条件2; -- 等同于 >= and <=

-- 查询工资在1000~2000之间的员工
select ename,sal from emp where sal between 1000 and 2000;
```

# 函数

## 字符串函数

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

## 数学函数

```sql
-- 四舍五入（round）
-- 截断（trunc）
-- 求余（mod）：mod(除数，被除数)
-- 取整（ceil、floor）
    ➡ ceil：向上取整，去除小数位整数位+1
    ➡ floor：向下取整，去除小数位

select round(123.456) from dual;
select trunc(123.456) from dual; -- 123
select mod(100,3) from dual; -- 3
select ceil(123.123),floor(456.789) from dual; -- 124 456
```

## 日期函数

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


-- nvl2：nvl2(值1，值2，值3) —— 如果值1为空，返回值3，否则返回值2
select nvl2(comm,comm+500,1500) 涨奖金,comm from emp;


-- nullif：nullif(值1，值2) —— 如果值1等于值2，返回null，反之返回值1
select nullif(500,null) from dual;


-- coalesce：coalesce(值1，值2，值3，....) —— 返回第一个不为空的值
select coalesce(comm,deptno) from emp;
```

## 条件函数

```sql
case when 值1 then 返回值1
  when 值2 then 返回值2
  ...
else 其他值
end


-- 判断员工的部门名称：如果是10叫财务部；如果是20叫研发部；如果是30叫销售部
select emp.*,
case when deptno = 10 then '财务部'
     when deptno = 20 then '研发部'
     when deptno = 30 then '销售部'
else '其他部门'
end 部门名称
from emp;
```

```sql
decode (条件，值1，返回值1，
        值2，返回值2
        ...
        值n，返回值n，
        其他值)

如果条件=值1，输出返回值1
如果条件=值2，输出返回值2
都不满足，返回其他值


-- 判断员工的部门名称：如果是10叫财务部；如果是20叫研发部；如果是30叫销售部
select emp.*,
decode(deptno,10,'财务部',
       20,'研发部',
       30,'销售部',
       '其他部门') 部门名称
from emp;
```

## 聚合函数（分组函数）

括号里面放字段

```sql
max() -- 最大值
min() -- 最小值
avg() -- 平均值
count() -- 数量
sum() -- 求和


select max(sal),min(sal),avg(sal),count(sal),sum(sal) from emp;
-- count(*)、count(1) 都代表总内容数，多少行数据
select count(*) from emp;
```

## 分组语句 （group by）

```sql
select 字段 from 表名 where 条件 group by 字段 ...;

➡ 出现在 select 后面的字段，如果不是在分组函数中，那么他必须同时出现在 group by 语句当中
➡ 出现在 group by 后面的字段不一定出现在 select 后面
➡ where 语句中不允许出现 group by
➡ 分组语句的条件（having）
  用法与 where 一样，having 条件


-- 求每个部门的平均工资
select deptno,trunc(avg(sal)) 平均工资 from emp group by deptno;
```

**区别**

- having 服务对象是 `group by`，where 服务对象是 `字段`
- where 不能用分组函数，having 通过条件过滤分组函数
- having 是在分组完成后执行，where 是在分组前执行，这也是为什么 where 不能服务 group by 的原因

```sql
-- 分部门和职业统计员工的工资和，并且工资和 > 3000
➡ group by deptno,job
➡ having sum(sal) > 3000
➡ deptno,job,sum(sal)

select deptno,job,sum(sal) from emp group by deptno,job having sum(sal) > 3000;
```

## 查询顺序

- from table
- where example
- group by title
- having
- select
- order by answer

# 连续查询

## 笛卡尔积

两个表的一种关联方式，将第一张表中的每一行都与第二张表中的每一行组合，生成一个新的表

```sql
select * from emp,dept;
```

## 等值连接查询

根据表与表之间的关联性来寻找对应的数据

```sql
-- 只需要部门20，名字是SMITH
select e.ename,d.* from emp e,dept d where e.ename = 'SMITH' and d.deptno = 20; -- 笛卡尔积
select e.ename,d.* from emp e,dept d where e.ename = 'SMITH' and d.deptno = e.deptno; -- 等值连接
```

## 非等值连接查询

连接两个表时使用不等于运算符来比较两个表的列

```sql
-- 查找成绩在60分以上，这门课的学分可以获得，计算每个学生总学分和统计姓名
select sname,sum(cval) from student s,mark m,course c where s.sid = m.sid and m.cid = c.cid and cmark >= 60 group by sname,s.sid;
```

## 自连接

一张表当多张表用

```sql
-- 添加条件
select e1.sal,e1.ename,ceil(avg(e2.sal)) from emp e1,emp e2 where e1.deptno = e2.deptno group by e1.deptno,e1.sal,e1.ename having e1.sal > avg(e2.sal);
```

## 左外连接右外连接（left join/right join）

以左边或者右边的表为基准表查询数据，如果没有数据补 null 值，判断时添加 on 使用等值连接

```sql
-- 左外连接
select * from testA left join testB on testA.tid = testB.tid;

-- 右外连接
select * from testA right join testB on testA.tid = testB.tid;
```

## 内连接（inner join）

当我们想要将两个或者多个表中的数据进行连接时可以使用内连接，通过一个或者多个关联条件，它会返回两个表中匹配到的行，也就是说在连接表中存在匹配的行时才会返回结果

```sql
-- 查询员工的姓名和所在部门的名称
select e.emp_name,d.dept_name from emp_test e,dept_test d where d.dept_no = e.dept_no; -- 笛卡尔积
select e.emp_name,d.dept_name from emp_test e inner join dept_test d on d.dept_no = e.dept_no; -- 内连接


-- 查询每个学生的姓名和平均分
select s.sid,ceil(avg(cmark)) from student s, mark m where s.sid=m.sid group by s.sid -- 笛卡尔积
select s.sid,ceil(avg(cmark)) from student s inner join mark m on m.sid=s.sid group by s.sid -- 内连接
```

**比较笛卡尔积和内连接：**

- 内连接：只返回满足连接条件的结果集，可以过滤数据
- 笛卡尔积：只是单纯的连接两个表的行，不会过滤出数据

**优势：**

- 数据过滤：内连接会根据关联条件来过滤数据，只返回相关的行，减少了数据的查询速度
- 查询效率：内连接的查询效率高于笛卡尔积
- 资源占用：内连接只返回有效数据，所以资源占用小

# 高级查询

## 随机查询

`dbms_random.value()`

```sql
-- 产生0~1之间的随机数，可以为0，不能为1：[0,1)
select dbms_random.value() from dual;

-- 产生1~11之间的随机数：[0,11)
select dbms_random.value(1,11) from dual;

-- 产生1~10之间的随机整数，含有10：[0,10]
select trunc(dbms_random.value(1,11),0) from dual;

-- 生成一个随机小写字母97~122
select chr(trunc(dbms_random.value(97,123),0)) from dual;

-- 随机返回学生表的五条数据，得到一张顺序被打乱的新表
select * from student order by dbms_random.value();

-- 使用 rownum 返回五条数据
select rownum r,s.* from (select * from student order by dbms_random.value()) s where rownum <= 5
```

## 子查询

从一个表中查出来的部分数据当作另一个表的查询条件，分为单行子查询和多行子查询

### 单行子查询

查询出来的结果只有一行，精确的匹配某个值

```sql

```

### 多行子查询

使用 `in、any、all` 三种运算符

#### in

用于判断某个值是否在子查询返回的结果集中，在子查询中返回的结果只要有一个值等于外层查询中的某个值，就会返回结果

```sql
-- 找出所有男生的成绩
select sid,cmark from mark where sid in (select sid from student where ssex = '男');

-- 多行子查询是可以无限嵌套的
-- 找出科目所对应的老师
select * from teacher
where tid in (
select tid from course
where cid in (
select cid from mark
where sid in (select sid from student where ssex = '男')))
```

#### any

用于比较外层查询中的某个值与子查询返回的结果集中的任意一个值是否相等，在子查询中返回的结果只要有一个值与外层查询中的某个值相等，返回结果

```sql
-- 查询年龄大于江苏任意一个学生年龄的其他地区的学生信息
-- 查找江苏学生的年龄
select sage from student where snativeplace = '江苏';

-- 查询比江苏地区最小年龄大的学生的其他地区的学生信息
select sname,snativeplace,sage from student
where sage > any (select sage from student where snativeplace = '江苏')
```

#### all

用于比较外层查询中的某个值与子查询返回的结果集中的全部值是否相等，在子查询中返回的结果集中的所有值都与外层查询中的某个值相等，才会返回结果

```sql
-- 查询年龄大于江苏地区所有学生年龄的其他地区的学生信息
select sname,snativeplace,sage from student
where sage > all (select sage from student where snativeplace = '江苏') and snativeplace != '江苏';
```

# 分页查询

```sql
-- 每页展示m条数据，查询第n页的数据
select * from (
  select rownum r,t1.* from table1 t1(需要分页的表)
  where rownum <= m * n)t2
  where r > m * n - m

-- 查询学生表的10~15行的数据
-- 每页分5条数据，查询第3页的数据
-- m=5，n=3
select * from (
  select rownum r,s.* from student s
  where rownum <= 15)t
  where r > 10;
```

# PLSQL

# 处理空值函数

```sql
-- 如果expr1为空，则返回expr2
NVL(expr1, expr2)

-- 如果expr1为空，则返回expr3，否则返回expr2
NVL2(expr1, expr2, expr3)

-- 如果expr1=expr2,返回空，否则返回expr1，要求两个表达式数据类型一致
NULLIF(expr1, expr2)

-- 返回第一个非空参数，若都为空，则返回NULL
COALESCE(expr1, expr2, expr3, ...)

-- 返回第一个匹配的搜索值，并返回对应的结果
DECODE

-- 返回第一个匹配的搜索值，并返回对应的结果
CASE
```

