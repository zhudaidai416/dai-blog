---
title: oracle 存储过程
date: 2024-11-12 11:04:13
category:
  - [计算机与科学, 数据库]
tags: oracle
cover: https://pic2.zhimg.com/v2-cf44c2fdebc32c350cd4fd1c432401bd_r.jpg
---

# 参考文章

{% links %}

- site: Oracle 存储过程语法详解
  owner: 夜希辰
  url: https://www.jianshu.com/p/4499814ed9ec
  desc: Oracle 存储过程语法详解—及 8 道案例练习
  color: "#e37c5b"
- site: ORACLE 存储过程详解
  owner: 菜鸟程序员
  url: https://www.cnblogs.com/guohu/p/11007350.html
  color: "#c38e9e"

{% endlinks %}

# 存储过程

存储过程（Stored Procedure）是在大型数据库系统中，一组为了完成特定功能的 SQL 语句集，存储在数据库中，经过第一次编译后调用不需要再次编译，用户通过指定存储过程的名字并给出参数（如果该存储过程带有参数）来执行它

存储过程是数据库中的一个重要对象

**特点：**

- 能完成较复杂的判断和运算
- 可编程行强，灵活
- SQL 编程的代码可重复使用
- 执行的速度相对快一些
- 减少网络之间的数据传输，节省开销

# 简单创建

## 简单语法

```sql
create procedure 名称 as
begin
...
end
```

## 示例

```sql
-- 创建一个简单的存储过程
create or replace procedure test as
begin
  dbms_output.put_line('hello world');
end

-- 调用存储过程
call test()
```

# 变量

## 声明赋值

- 在 begin 程序体前声明变量，变量必须先声明后使用
- 变量具有数据类型和长度，与 ORACLE 的<font color=red>数据类型保持一致</font>
- 变量可以通过 `select into` 的方式赋值，也可以通过 `:=` 赋值

```sql
create or replace procedure select_emp as
-- 定义变量my_income
my_income varchar(20);

begin
-- 赋值：将查询到的income赋值给变量my_income
  select income into my_income from emp_test where worker_no = '200010';

-- 打印输出变量my_income
  dbms_output.put_line('工资'||my_income);
end;
```

## 变量分类 👇🏻

- 普通数据类型（char、varchar2、date、number、boolean、long）
- 特殊数据类型（引用型变量、记录型变量）
  - 引用型变量：变量的数据类型取决于表中的数据类型
  - 记录型变量：变量不是接受一个值，是一行值

```sql
-- 声明变量的语法：
变量名 变量类型(变量长度)

-- 普通变量
v_name varchar2(20);
-- 引用型变量
v_income emp_test.income%TYPE;
-- 记录型变量
v_emp emp_test%ROWTYPE  -- 表示变量v_emp存的是表中emp_test一整行的数据
```

## 普通变量

两种变量赋值的方式

- 直接赋值：`:=`
- 语句赋值：`使用 select...into... 赋值`（语法 select 值 into 变量）

```sql
create or replace procedure test as
-- 定义变量
my_number varchar2(20); -- 工号
my_income int := 3000; -- 声明变量直接赋值
my_depart varchar2(50); -- 部门

begin
  -- 通过SELECT语句给变量赋值
  select '5号部门' into my_depart from dual;

  -- 打印输出
  dbms_output.put_line('姓名'||my_number||'工资'||my_income||'部门'||my_depart); 
end;


-- 输出结果：姓名工资3000部门5号部门
```

## 引用变量

引用变量变量的类型和长度取决于表中字段的类型和长度

语法：通过 `表名.列名%TYPE` 指定变量的类型和长度

- 普通变量：需要知道表中列的类型
- 引用变量：不考虑列的类型，适用于数据库定义的更新

```sql
# 定义引用变量：打印工号为200010员工的个人信息，包括：工资、部门

create or replace procedure test as
-- 定义变量
my_income emp_test.income%TYPE; -- 工资：引用型变量
my_depart emp_test.department%TYPE; -- 部门：引用型变量

begin
  select income,department into my_income,my_depart from emp_test where worker_no='200010';
  dbms_output.put_line('工资'||my_income||'部门'||my_depart);  
end;


-- 输出结果：工资5000部门10号部门
```

## 记录型变量

记录型变量接受表中的一整行记录

语法：`变量名称 表名%ROWTYPE`

使用场景：如果有一个表，有100个字段，如果程序要使用这100个字段，使用引用型变量一个个声明，会特别麻烦，记录型变量可以方便解决这个问题

```sql
# 定义记录型变量：打印工号为200010员工的个人信息，包括：工资、部门

create or replace procedure test as
-- 定义变量
v_emp emp_test%ROWTYPE; -- v_emp记录型变量，接受表中的一整行记录

begin
  select * into v_emp from emp_test where worker_no='200010';
  dbms_output.put_line('工资'||v_emp.income||'部门'||v_emp.department);  
end;


-- 输出结果：工资5000部门10号部门
```

# 参数

## 基本语法

```sql
create procedure 名称([IN|OUT|INOUT] 参数名 参数数据类型)
begin
...
end;
```

## 传入参数 IN

- 表示该参数的值必须在调用存储过程时指定，如果不显示指定为 in，那么默认就是 in 类型
- IN 类型参数一般只用于传入，在调用过程中一般不作为修改和返回

```sql
# 需求：传入员工工号，根据工号输出该员工的工资

create or replace procedure test_income(worker_id varchar2) as
my_income varchar2(100);

begin
  select income into my_income from emp_test where worker_no = worker_id;
  dbms_output.put_line(my_income);
end;


-- 调用存储过程
call test_income(200013);
```

## 传出参数 out

- 只能接收赋值，不能给其他变量赋值
- 输出模式的参数，用于输出值，会忽略传入的值，在子程序内部可以对其进行修改
- 调用时，参数需要使用变量

```sql
# 需求：传入worker_id，返回该用户的工资income

create or replace procedure test_out(worker_id in varchar2,my_income out emp_test.income%TYPE) as

begin
  select income into  my_income from emp_test where worker_no = worker_id;
  dbms_output.put_line(my_income);
end;


-- 调用
declare
my_income int;
begin
  test_out(200010, my_income);
end;
```

## 可变参数in out

- 与 out 类型相比，不同是默认初始化参数不为 null，传的是什么就是什么
- 调用时，参数需要使用变量

```sql
create or replace procedure pro_in_out(p_num in out number) as

begin
  dbms_output.put_line(p_num);
  p_num:=10;
end;


-- 调用
declare
test number:=1;
begin
  pro_in_out(test);
  dbms_output.put_line(test);
end;
```

# 条件语句

## 基本语法结构

```sql
-- 基本结构
if() then
  ...
else
  ...
end if;


-- 多条件判断
if() then
  ...
elseif() then
  ...
else
  ...
end if;
```

## 案例

### 简单条件

```sql
# 需求：如果员工工号worker_no是偶数则返回工资income，否则返回部门department

create or replace procedure test_worker(worker_id varchar2) as
my_income varchar(20);
my_department varchar(20);

begin
  if(mod(to_number(worker_id),2)=0)  then
    select income into my_income from emp_test where worker_no = worker_id;
    dbms_output.put_line(my_income);
  else
    select department into my_department from emp_test where worker_no = worker_id;
    dbms_output.put_line(my_department);
  end if;
end;


-- 调用
call test_worker(200013);
call test_worker(200012);
```

### 多条件

```sql
# 需求：以员工号为参数，修改该员工的工资
  -- 若该员工属于10号部门，则工资增加150
  -- 若属于20号部门，则工资增加200
  -- 若属于30号部门，则工资增加250
  -- 若属于其他部门，则工资增加300

create or replace procedure add_income(worker_id varchar2) as
my_department varchar(20);

begin
  select department into my_department from emp_test where worker_no = worker_id;
  if (my_department = '10号部门') then
    update emp_test set income = income+150 where worker_no = worker_id;
    commit;
  elseif (my_department = '20号部门') then
    update emp_test set income = income+200 where worker_no = worker_id;
    commit;
  elseif (my_department = '30号部门') then
    update emp_test set income = income+250 where worker_no = worker_id;
    commit;
  else
    update emp_test set income = income+300 where worker_no = worker_id;
    commit;
  end if;
end;


-- 调用存储过程
call add_income(200010);
call add_income(200015);
-- 执行后结果
select * from emp_test;
```

# 循环语句

## While

```sql
while(条件) loop
  ...
end loop;
```

+++success 示例：向表 emp_test 中插入十条数据，仅给工号字段插入数据，其它字段不插入数据，插入工号从12001至120010

```sql
create or replace procedure test_inset as
my_worker int;

begin
  my_worker := 0;
  while my_worker<10 loop
    my_worker := my_worker + 1;
    insert into emp_test(worker_no) values ('1200'||to_char(my_worker));
    commit;
  end loop;
end;


-- 调用
call test_inset();
select * from emp_test;
```

+++

## Loop

```sql
loop
  exit when(退出条件);
  ...
end loop;
```

+++success 示例：使用LOOP循环，打印输出 0 至 5 的数字

```sql
create or replace procedure loop_test as
i number;

begin
  i := 0;
  LOOP
  Exit When(i > 5);
  dbms_output.put_line(i);
  i := i + 1;
  END LOOP;
end;


-- 调用
call loop_test();
```

+++

## For

```sql
for () in () loop
  ...
end loop;
```

+++success 示例：使用 FOR 循环，打印输出 0 至 5 的数字

```sql
create or replace procedure for_test as
i number;

begin
  i := 0;
  for i in 1..5 loop
    dbms_output.put_line(i);
  end loop;
end;


-- 调用
call for_test();
```

+++

# 游标

## 定义

用于临时存储一个查询返回的多行数据，通过遍历游标，可以逐行访问处理该结果集的数据

<font color=red>游标的使用方式：</font>声明 ➡ 打开 ➡ 读取 ➡ 关闭

## 语法

```sql
-- 游标声明
cursor 游标名[(参数列表)] is 查询语句;

-- 游标打开
open 游标名;

-- 游标取值
fetch 游标名 into 变量列表;

-- 游标关闭
close 游标名;
```

## 案例

需求：使用游标，把 emp_test 表中20号部门的员工工号逐一打印

```sql
create or replace procedure cur_test as
my_workerno varchar(20);

-- 游标声明
cursor cur_worker is select worker_no from emp_test where department = '20号部门';

begin
  -- 游标打开
  open cur_worker;
  loop
    -- 获取游标中的数据
    fetch cur_worker into my_workerno; -- 提取cursor，提取结果集中的记录
  
    -- 退出循环条件
    Exit When cur_worker%notfound;
    dbms_output.put_line('my_workerno:'||my_workerno);
  end loop;
  close cur_worker;
end;


-- 调用存储过程
call cur_test();
```

