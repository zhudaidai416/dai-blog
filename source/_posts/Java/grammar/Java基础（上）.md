---
title: Java 基础（上）
category:
  - [Java, 语法]
tags: Java
# cover: https://img-baofun.zhhainiao.com/fs/2f1e640b85a2b8df12b7216915cea51c.jpg
cover: https://img0.baidu.com/it/u=3953094305,3191906254&fm=253&fmt=auto&app=138&f=JPEG?w=1422&h=800
---

# Java 基础语法

## 注释

写在程序中对代码进行解释说明的文字，方便自己和其他人查看，以便理解程序

```java
单行注释：
  // 注释内容，只能写一行

多行注释：/* + 回车
  /*
    注释内容1
    注释内容2
  */

文档注释：/** + 回车
  /**
    文档注释的内容是可以提取到一个程序说明文档中去的
    注释内容
  */
```

| 快捷键           | 功能                         |
| ---------------- | ---------------------------- |
| Ctrl + /         | 单行注释（对当前行进行注释） |
| Ctrl + Shift + / | 对选中的代码进行多行注释     |

## 字面量

数据在程序中的书写格式

| 常用数据 | 生活中的写法 |        程序中的写法        | 说明                                                            |
| -------- | ------------ | :------------------------: | --------------------------------------------------------------- |
| 整数     | 666，-88     |          666，-88          | 写法一致                                                        |
| 小数     | 13.14，-5.21 |        13.14，-5.21        | 写法一致                                                        |
| 字符     | A，0，我     |       ‘A’，‘0’，‘我’       | 程序中必须使用<font color="red">单引号</font>，有且仅能一个字符 |
| 字符串   | 黑马程序员   | “HelloWorld”，“黑马程序员” | 程序中必须使用<font color="red">双引号</font>，内容可有可无     |
| 布尔值   | 真、假       |        true 、false        | 只有两个值：<br>true：代表真，false：代表假                     |
| 空值     |              |         值是：null         | 一个特殊的值，空值                                              |

> :warning: 注：特殊字符
>
> \n：换行
>
> \t：一个 tab

## 变量

用来记住程序要处理的数据，编写的代码更灵活，管理代码更方便

定义格式： `数据类型 变量名 = 初始值;`

注意事项：

```java
1、变量要先声明才能使用

2、变量是什么类型，就应该用来装什么类型的数据，否则报错

3、变量定义在哪个{}范围内，就只在哪个大括号内有效。变量的有效范围称之为变量的作用域
  {
    int a = 10;
    System.out.println(a); // 这是对的
  }
  System.out.println(a); // 报错

4、在同一个作用域内，不能有两个同名的变量
  {
    int a = 10;
    int a = 20; // 报错
  }

5、变量没有初始化，不能直接使用
  int a; // 仅仅定义了变量，但是没有初始值
  System.out.println(a); // 报错

6、变量可以定义在同一行
  如：int a=10, b=20; // a和b都是int类型
```

## 关键字

关键字是 java 语言中有特殊含义的单词，不能作为类名、变量名

|  abstract  |    assert    |  boolean  |   break    |  byte  |
| :--------: | :----------: | :-------: | :--------: | :----: |
|    case    |    catch     |   char    |   class    | const  |
|  continue  |   default    |    do     |   double   |  else  |
|    enum    |   extends    |   final   |  finally   | float  |
|    for     |     goto     |    if     | implements | import |
| instanceof |     int      | interface |    long    | native |
|    new     |   package    |  private  | protected  | public |
|   return   |   strictfp   |   short   |   static   | super  |
|   switch   | synchronized |   this    |   throw    | throws |
| transient  |     try      |   void    |  volatile  | while  |

## 标志符

标志符就是名字，我们写程序时会起一些名字，如类名、变量名等等都是标识符

**标识符规则：**

- 基本组成：由数字、字母、下划线（\_）和美元符（$）等组成
- 强制要求：不能以数字开头、不能用关键字做为名字、且是区分大小

**标识符的建议规范：**

```java
1、所有的名字要见名知意，便于自己和别人阅读
  class Student {} // 一看这个类就知道表示一个学生
  int age =10;  // 一看这个变量就知道表示年龄

2、类名：首字母大写（大驼峰命名）
  class Student {}

3、变量名：第二个单词开始首字母大写（小驼峰命名）
  double applePrice = 7.5;
```

# 数据详解

数据在计算机底层都是采用二进制来存储的

- 计算机中数据最小的组成单元：使用 8 个二进制位为一组，称之为一个字节（byte，简称 B）

- 字节中的每个二进制位就称为位（bit，简称 b）, `1B = 8b`

- 计算机发展出了 KB、MB、GB、TB、… 这些数据单位

  - 1 B = 8 b

  - 1 KB = 1024 B
  - 1 MB = 1024 KB
  - 1 GB = 1024 MB
  - 1 TB = 1024 GB

![字节](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/字节.png)

## 字符的存储原理

ASCII 编码表：即美国信息交换标准编码，规定了现代英语、数字字符、和其他西欧字符对应的数字编号

![ASCII编码表](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/ASCII编码表.png)

## 图片的存储原理

- 图片就是无数个像素点组成
- 每个像素点的数据：用 0 ~ 255* 255* 255 表示其颜色

## 声音的存储原理

![声音存储](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/声音存储.png)

## 数据的其他表示形式

### 二进制

十进制 ➡ 二进制：除二取余法

![十进制转二进制](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/十进制转二进制.png)

```java
// 二进制 ➡ 十进制

1101 ➡ 1*2^3+1*2^2+1*2^0 = 13
```

### 八进制

每 3 位二进制作为一个单元，最小数是 0，最大数是 7，共 8 个数字

```java
// 二进制 ➡ 八进制

97：01100001
    01、100、001 ➡ 141
```

### 十六进制

每 4 位二进制作为一个单元，最小数是 0，最大数是 15，共 16 个数字，依次用： 0~9 A B C D E F

```java
// 二进制 ➡ 十六进制

97：01100001
    0110、0001 ➡ 61
250：11111010
    1111、1010 ➡ FA
```

Java 程序中支持书写二进制、八进制、十六进制的数据，分别需要以`0B 或者 0b、0、0X 或者 0x 开头`

```java
// 不同进制在 Java 程序中的书写格式

System.out.pirntln('a'+1); // 98
System.out.pirntln(0b01100001); // 97
System.out.pirntln(0141); // 97
System.out.pirntln(0x61); // 97
```

# 数据类型

- 基本数据类型
- 引用数据类型

## 基本数据类型

4 大类 8 种

<table>
  <tr>
    <th colspan=2>数据类型</th>
    <th>内存占用(字节数)</th>
    <th>数据范围</th>
  </tr>
  <tr>
    <td rowspan=4>整型</td>
    <td>byte</td>
    <td>1</td>
    <td>-128~127</td>
  </tr>
  <tr>
    <td>short</td>
    <td>2</td>
    <td>-32768~32767</td>
  </tr>
  <tr>
    <td>int<font color="red">（默认）</font></td>
    <td>4</td>
    <td>-2147483648~2147483647（10位数，大概21亿多）</td>
  </tr>
  <tr>
    <td>long</td>
    <td>8</td>
    <td>-9223372036854775808 ~ 9223372036854775807（19位数）</td>
  </tr>
  <tr>
    <td rowspan=2>浮点型（小数）</td>
    <td>float</td>
    <td>4</td>
    <td>1.401298 E -45 到 3.4028235 E +38</td>
  </tr>
  <tr>
    <td>double<font color="red">（默认）</font></td>
    <td>8</td>
    <td>4.9000000 E -324 到1.797693 E +308</td>
  </tr>
  <tr>
    <td>字符型</td>
    <td>char</td>
    <td>2</td>
    <td>0-65535</td>
  </tr>
  <tr>
    <td>布尔型</td>
    <td>boolean</td>
    <td>1</td>
    <td>true，false</td>
  </tr>
</table>


```java
// long 类型，需要在其后面加上 L/l
long number = 73642422442424L;

// float 类型，需要在其后面加上 F/f
float score = 99.5F;
```

## 自动类型转换（<font color="red">小范围类型变量 ➡ 大范围类型变量</font>）

类型范围小的变量，可以直接赋值给类型范围大的变量

![自动类型转换原理](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/自动类型转换原理.png)

**自动类型转换的其它形式：**

![自动类型转换](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/自动类型转换.png)

```java
int c = 100; // 4位
double d = c; // 8位，发生自动类型转换
System.out.println(d); // 100.0

char ch = 'a'; // 'a' 97 ➡ 00000000 01100001
int i = ch; // 发生自动类型转换 ➡ 00000000 00000000 00000000 01100001
System.out.println(i); // 97
```

### 表达式的自动类型转换

在表达式中，小范围类型的变量，会自动转换成表达式中较大范围的类型，再参与运算

`byte 、short、char ➡ int ➡ long ➡ float ➡ double`

> :warning: **注意事项：**
>
> - 表达式的最终结果类型由表达式中的<font color="red">最高类型</font>决定
> - 在表达式中，<font color="red">byte、short、char</font> 是直接转换成 <font color="red">int</font> 类型参与运算

```java
byte a = 10;
int b = 20;
long c = 30;
➡ long result = a + b + c;


byte b1 = 110;
byte b2 = 80;
➡ int b3 = b1 + b2;
```

## 强制类型转换（<font color="red">大范围类型变量 ➡ 小范围类型变量</font>）

强行将类型范围大的变量、数据赋值给类型范围小的变量

格式：`数据类型 变量2 = (数据类型)变量1、数据`

快捷键：`alt + enter`

```java
int a = 20;
byte b = (byte)a;
```

**强制类型转换的原理**

强行把前面几个字节砍掉，但是有数据丢失的风险

![强制类型转换原理](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/强制类型转换原理.png)

> :warning: ​**注意事项：**
>
> - 强制类型转换<font color="red">**可能**造成数据（丢失）溢出</font>
> - 浮点型强转成整型，<font color="red">直接丢掉小数部分，保留整数部分返回</font>

```java
double d = 99.5;
int m = (int) d;
System.out.println(m); // 99
```

# 运算符

## 算术运算符

| 符号 |                   名称                   |
| ---- | :--------------------------------------: |
| +    |                    加                    |
| -    |                    减                    |
| \*   |                    乘                    |
| /    | 除（在 Java 中两个整数相除结果还是整数） |
| %    |                   取余                   |

```java
System.out.println(5 / 2); // 2
System.out.println(5.0 / 2); // 2.5

// 一定要小数点的写法
int i = 5;
int j = 2;
System.out.println(1.0 * i / j); // 2.5
```

> :warning: 注：
>
> “+”符号除了用于加法运算，还可以作为<font color="red">连接符</font>
>
> “+”符号与字符串运算的时候是用作连接符的，其结果依然是一个字符串
>
> 能算则算，不能算就拼接在一起

```java
int a = 5;
System.out.println("abc" + a); // "abc5"
System.out.println(a + 5); // 10
System.out.println("daidai" + a + 'a'); // "daidai5a"
System.out.println(a + 'a' + "daidai"); // "102daidai"


// 例子（数值拆分）：一个三位数，将其拆分为个位、十位、百位后，打印在控制台
公式总结：
个位 ：数值 % 10
十位 ：数值 / 10 % 10
百位 ：数值 / 10 / 10 % 10
千位 ：数值 / 10 / 10 / 10 % 10
...
```

## 自增自减运算符

| 符号     | 说明                                         |
| -------- | -------------------------------------------- |
| 自增：++ | 放在某个变量前面或者后面，对变量自身的值加 1 |
| 自减：-- | 放在某个变量前面或者后面，对变量自身的值减 1 |

> :warning: 注：
>
> - ++ 、-- 只能操作变量，不能操作字面量
>
> - 如果单独使用放前放后是没有区别的
>
> - 非单独使用（<font color="red">如在表达式中、或者同时有其它操作</font>）
>
>   - 在变量<font color="red">前</font>，**先**进行变量**自增/自减**，**再使用变量**
>
>   - 在变量<font color="red">后</font>，**先使用**变量，**再**进行变量**自增/自减**

```java
int i = 10;
int result = ++i; // 先加后用
System.out.println(result); // 11
System.out.println(i); // 11

int j = 10;
int result2 = j++; // 先用后加
System.out.println(result2); // 10
System.out.println(j); // 11


int c = 10; // 变化过程 ➡ 10 11 12 11
int d = 5; // 变化过程 ➡  5 4 5
int result3 = c++ + ++c - --d - ++d + 1 + c--;
               10 +  12 -  4  -  5  + 1 + 12
System.out.println(result3); // 26
System.out.println(c); // 11
System.out.println(d); // 5
```

## 赋值运算符

<table>
  <tr>
    <th></th>
    <th>符号</th>
    <th>用法</th>
    <th>说明</th>
    <th>底层代码形式</th>
  </tr>
  <tr>
    <td>基本的赋值运算符</td>
    <td>=</td>
    <td>int a = 10;</td>
    <td>从右边往左看</td>
    <td>把数据 10 赋值给左边的变量 a 存储</td>
  </tr>
  <tr>
    <td rowspan=6>扩展的赋值运算符</td>
  </tr>
  <tr>
    <td>+=</td>
    <td>a+=b</td>
    <td>加后赋值</td>
    <td>a = (a的类型)(a + b);</td>
  </tr>
  <tr>
    <td>-=</td>
    <td>a-=b</td>
    <td>减后赋值</td>
    <td>a = (a的类型)(a - b);</td>
  </tr>
  <tr>
    <td>*=</td>
    <td>a*=b</td>
    <td>乘后赋值</td>
    <td>a = (a的类型)(a * b);</td>
  </tr>
  <tr>
    <td>/=</td>
    <td>a/=b</td>
    <td>除后赋值</td>
    <td>a = (a的类型)(a / b);</td>
  </tr>
  <tr>
    <td>%=</td>
    <td>a%=b</td>
    <td>取余后赋值</td>
    <td>a = (a的类型)(a % b);</td>
  </tr>
</table>


> :warning: 注：<font color="red">扩展的赋值运算符隐含了强制类型转换</font>

```java
byte x = 10;
byte y = 30;
x = x + y; // 报错
x += y; // 等价于 byte x = (byte)(x+y); 这里有隐含的强制类型转换
```

## 关系运算符

判断数据是否满足条件，最终会返回一个判断的结果，这个结果是布尔类型的值：true 或 false

<table>
  <tbody align="center"> 
    <tr>
      <th style="text-align:center;">符号</th>
      <th style="text-align:center;">用法</th>
      <th style="text-align:center;">说明</th>
      <th style="text-align:center;">结果</th>
    </tr>
    <tr>
      <td>></td>
      <td>a > b</td>
      <td>判断a是否大于b</td>
      <td rowspan=6>成立返回 true、不成立返回 false</td>
    </tr>
    <tr>
      <td>>=</td>
      <td>a >= b</td>
      <td>判断a是否大于或者等于b</td>
    </tr>
    <tr>
      <td>&lt;</td>
      <td>a &lt; b</td>
      <td>判断a是否小于b</td>
    </tr>
    <tr>
      <td>&lt;=</td>
      <td>a &lt;= b</td>
      <td>判断a是否小于或者等于b</td>
    </tr>
    <tr>
      <td>==</td>
      <td>a == b</td>
      <td>判断a是否等于b</td>
    </tr>
    <tr>
      <td>!=</td>
      <td>a != b</td>
      <td>判断a是否不等于b</td>
    </tr>
  </tbody>
</table>

> :warning: 注：在 java 中判断是否相等一定是“== ” ，千万不要把 “== ”误写成“=”

## 逻辑运算符

把多个条件放在一起运算，最终返回布尔类型的值：true、false

| 符号 | 名称     | 用法              |                                     运算逻辑                                      |
| ---- | -------- | ----------------- | :-------------------------------------------------------------------------------: |
| &    | 逻辑与   | 2 > 1 & 3 > 2     |      多个条件必须都是 true，结果才是 true<br>有一个是 false，结果就是 false       |
| \|   | 逻辑或   | 2 > 1 \| 3 < 5    |                    多个条件中只要有一个是 true，结果就是 true                     |
| !    | 逻辑非   | ! (2 > 1)         |                       取反：!true == false、!false == true                        |
| ^    | 逻辑异或 | 2 > 1 ^ 3 > 1     |      前后条件的结果相同，就直接返回 false<br>前后条件的结果不同，才返回 true      |
| &&   | 短路与   | 2 > 10 && 3 > 2   | 判断结果与“&”一样，过程不同：<font color="red">左边为 false</font>，右边则不执行  |
| \|\| | 短路或   | 2 > 1 \| \| 3 < 5 | 判断结果与“\|”一样，过程不同：<font color="red">左边为 true</font>， 右边则不执行 |

> :warning: 注：
>
> 在 java 中， “&” 、 “|”：无论左边是 false 还是 true，<font color="red">右边都要执行</font>
>
> 由于 &&、|| 运算效率更高，在开发中用的更多

```java
int i = 10;
int j = 20;

System.out.println(i > 100 && ++j > 99); // false
System.out.println(j); // 20

System.out.println(i > 100 & ++j > 99); // false
System.out.println(j); // 21
```

## 三元运算符

格式：`关系表达式? 值1 : 值2;`

执行流程：首先计算关系表达式的值，如果关系表达式的值为 true，则返回值 1；如果关系表达式的值为 false，则返回值 2

```java
int i = 10;
int j = 45;
int k = 34;

// 例子1：找出2个整数中的较大值，并输出
int max = i > j ? i : j;
System.out.println(max);

// 例子2：找3个整数中的较大值
int temp = i > j ? i : j;
int max2 = temp > k ? temp : k;
System.out.println(max2);
i > j && i > k ? i : j < k ? j : k
```

## 运算优先级

| 优先级 | 运算符                     |
| :----: | -------------------------- |
|   1    | ()                         |
|   2    | !、-、++、--               |
|   3    | \*、/、%                   |
|   4    | +、-                       |
|   5    | <<、>>、>>>                |
|   6    | <、<=、>、>=、instanceof   |
|   7    | ==、!=                     |
|   8    | &                          |
|   9    | ^                          |
|   10   | \|                         |
|   11   | &&                         |
|   12   | \|\|                       |
|   13   | ?:                         |
|   14   | =、+=、-=、\*=、/=、%=、&= |

```java
// 这里&&先算，相当于 true || false 结果为 true
System.out.println(10 > 3 || 10 > 3 && 10 < 3); // true


// 在实际开发中，其实我们很少考虑运算优先级，因为如果想让某些数据先运算，其实加()就可以了，这样阅读性更高
System.out.println((10 > 3 || 10 > 3) && 10 < 3); // false
```

## 案例：键盘录入技术

API（Application Programming Interface：应用程序编程接口）

- Java 写好的程序，咱们程序员可以直接拿来调用
- Java 为自己写好的程序提供了相应的程序使用说明书（[API 文档](https://www.oracle.com/java/technologies/javase-jdk17-doc-downloads.html)）

```java
package com.itheima.scanner;
import java.util.Scanner; // 1、导包（自动导包）

public class Test {
  public static void main(String[] args) {
    // 2、创建一个扫描器对象
    Scanner sc = new Scanner(System.in);
    System.out.println("请输入你的年龄：");
    // 3、等待接收用户的数据
    int age = sc.nextInt(); // 等待用户输入一个整数，直到用户按了回车键，才会拿到数据
    System.out.println("你的年龄是：" + age);

    System.out.println("请输入你的名字：");
    String name = sc.next(); // 等待用户输入一个字符串，直到用户按了回车键，才会拿到数据
    System.out.println("欢迎，" + name);
  }
}
```

# 程序流程控制

- 顺序结构：自上而下的执行代码
- 分支结构：根据条件，选择对应代码执行

- 循环结构：控制某段代码重复执行

## 分支结构

### if

根据条件（真或假）来决定执行某段代码

```java
// 形式1
if (条件表达式) {
  代码;
}

// 形式2
if (条件表达式) {
  代码1;
} else {
  代码2;
}

// 形式3
if (条件表达式1) {
  代码1;
} else if (条件表达式2) {
  代码2;
} else if (条件表达式3) {
  代码3;
}
. . .
else {
  代码n;
}
```

> :warning: 注：if 语句中，如果大括号控制的只有一行代码，则<font color="red">大括号可以省略不写</font>（不推荐）

### switch

通过比较值来决定执行哪条分支

```java
switch (表达式) {
  case 值1:
    执行代码...;
    break;
  case 值2:
    执行代码...;
    break;
    ...
  case 值n-1:
    执行代码...;
    break;
  default:
    执行代码n;
}
```

> :warning: 注：
>
> - 表达式类型只能 <font color="red">byte、short、int、char</font>，JDK5 开始支持枚举，JDK7 开始支持 String
>
>   <font color="red">不支持 double、float、long</font>
>
> - case 给出的值不允许重复，且<font color="red">只能是字面量</font>，不能是变量
>
> - 正常使用 switch 的时候，<font color="red">不要忘记写 break</font>，否则会出现穿透现象

+++success if、switch 的比较以及适合的业务场景

- if 在功能上远远强大于 switch
- 当前条件是区间的时候，应该使用 if 分支结构
- 当条件是与一个一个的值比较的时候，switch 分支更合适：格式良好，性能较好，代码优雅

+++

+++primary 利用 switch 穿透性简化代码

当存在多个 case 分支的代码相同时，可以把相同的代码放到一个 case 块中，其他的 case 块都通过穿透性穿透到该 case 块执行代码即可，这样可以简化代码

```java
String week = "周二"
switch (week) {
  case "周一":
  case "周二":
  case "周三":
  case "周四":
  case "周五":
    System.out.println("上班");
    break;
  case "周六":
    System.out.println("逛街");
    break;
  case "周日":
    System.out.println("在家休息");
    break;
  default:
    System.out.println("你输入的星期不存在");
}
```

+++

## 循环结构

### for

控制一段代码反复执行很多次

```java
// 快捷方式：fori+回车
for (初始化语句; 循环条件; 迭代语句) {
  循环体语句(重复执行的代码);
}

* 初始化语句：一般是定义一个变量，并给初始值
* 循环条件：一般是一个关系表达式，结果必须是 true 或者 false
* 迭代语句：用于对条件进行控制，一般是自增或者自减
* 循环语句体：需要重复执行的代码

// 输出3次 Hello World
for (int i = 0; i < 3; i++) {
  System.out.println("Hello World");
}
```

### while

```java
初始化语句;
while () {
  循环体语句（重复执行的代码）;
  迭代语句;
}

// 例子：打印5行 Hello World
int i = 0;
while (i < 5) {
  System.out.println("Hello World");
  i++;
}
```

;;;id1 案例

:::success no-icon

需求：世界最高山峰珠穆朗玛峰高度是：8848.86 米=8848860 毫米，假如我有一张足够大的它的厚度是 0.1 毫米

请问：该纸张折叠多少次，可以折成珠穆朗玛峰的高度？

:::

;;;

;;;id1 代码

```java
// 1、定义变量记住珠穆朗玛峰的高度和纸张的高度
double peakHeight = 8848860;
double paperThickness = 0.1;

// 3、定义一个变量count用于记住纸张折叠了多少次
int count = 0;

// 2、定义while循环控制纸张开始折叠
while (paperThickness < peakHeight) {
    // 把纸张进行折叠，把纸张的厚度变成原来的2倍
  paperThickness = paperThickness * 2;
  count++;
}
System.out.println("需要折叠多少次：" + count);
System.out.println("最终纸张的厚度是：" + paperThickness);
```

;;;

### do-while

```java
初始化语句;
do {
  循环体语句;
  迭代语句;
} while (循环条件);
```

+++danger 三种循环的区别

- for 循环和 while 循环（<font color="red">先判断后执行</font>）； do...while （<font color="red">先执行后判断</font>）

- 使用规范：

  - 如果已知循环次数建议使用 for 循环

  - 如果不清楚要循环次数建议使用 while 循环

- 其他区别：

  - for 循环中，控制循环的变量只在循环中使用
  - while 循环中，控制循环的变量在循环后还可以继续使用

+++

### 死循环

可以一直执行下去的一种循环，如果没有干预不会停下来

```java
for ( ; ; ) {
  System.out.println("Hello World1");
}

while (true) {
  System.out.println("Hello World2");
}

do {
  System.out.println("Hello World3");
} while (true);
```

+++success 死循环的应用场景

最典型的是可以用死循环来做服务器程序， 比如百度的服务器程序就是一直在执行的，你随时都可以通过浏览器去访问百度

如果哪一天百度的服务器停止了运行，有就意味着所有的人都用不了百度提供的服务了

+++

### 循环嵌套

循环中又包含循环，特点：外部循环每循环一次，内部循环会全部执行完一轮

```java
// 例子：在控制台使用*打印出3行4列的矩形
for (int i = 0; i < 3; i++) {
  for (int j = 0; j < 4; j++) {
    System.out.print("*"); // 不换行
  }
  System.out.println(); // 换行
}
```

## 跳转关键字

- break：跳出并结束当前所在循环的执行
- continue：用于跳出当前循环的当次执行，直接进入循环的下一次执行

> :warning: 注：
>
> - break：只能用于结束所在循环，或者结束所在 switch 分支的执行
> - continue：只能在循环中进行使用

## 案例

;;;id2 生成随机数

```java
// 1、导包(idea会自动完成)
import java.util.Random;
public class RandomTest1 {
  public static void main(String[] args) {
    // 2、创建一个Random对象，用于生成随机数
    Random r = new Random();

    for (int i = 1; i <= 20; i++) { // 快捷键：选中要循环的代码，ctrl+alt+t，选择for循环回车
      // 3、调用Random提供的功能：nextInt得到随机数
      int data = r.nextInt(10); // 0-9
      System.out.println(data);
    }
  }
}
```

;;;

> :warning: 注：nextInt(n) 功能只能生成：[0,n) 之间的随机数，即 0 至 n-1

;;;id2 区间随机数

```java
// 生成1-10的随机数 ➡ -1 ➡ [0,9]+1
int data = r.nextInt(10) + 1;

// 生成3-17的随机数 ➡ -3 ➡ [0,14]+3
int data = r.nextInt(15) + 3;

// 生成65-91的随机数 ➡ -65 ➡ [0,26]+65
int data = r.nextInt(27) + 65;

//  java给了功能可以得到指定区间，jdk1.8不支持
int data = r.nextInt(10,31) // 生成10-31的随机数
```

;;;

;;;id2 猜数字

:::success no-icon

需求：随机生成一个 1-100 之间的数据，提示用户猜测，猜大提示过大，猜小提示过小，直到猜中结束游戏

:::

```java
import java.util.Random;
import java.util.Scanner;

public class RandomTest2 {
  public static void main(String[] args) {
    // 1、随机产生一个1-100之间的数据，做为中奖号码
    Random r = new Random();
    int luckNumber = r.nextInt(100) + 1;

    Scanner sc = new Scanner(System.in);
    // 2、定义一个死循环，让用户不断的猜测数据
    while (true) {
      // 提示用户猜测
      System.out.println("请输入你猜测的数字：");
      int guessNumber = sc.nextInt();

      // 3、判断用户猜测的数字与幸运号码的大小情况
      if (guessNumber > luckNumber) {
        System.out.println("你猜测的数字过大~~");
      } else if (guessNumber < luckNumber) {
        System.out.println("你猜测的数字过小~~");
      } else {
        System.out.println("恭喜你猜测成功了~~");
        break; // 结束死循环
      }
    }
  }
}
```

;;;
