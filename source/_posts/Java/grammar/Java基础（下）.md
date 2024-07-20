---
title: Java 基础（下）
category:
  - [Java, 语法]
tags: Java
cover: https://pic2.zhimg.com/v2-ffbff74f8591a4a8e78133481588222d_r.jpg
---

# 数组

数组就是一个容器，用来存一批同种类型的数据

## 静态初始化数组

定义数组的时候直接给数组赋值

```java
// 完整格式
数据类型[] 数组名 = new 数据类型[]{元素1, 元素2, 元素3…};

// 简化格式
数据类型[] 数组名 = {元素1, 元素2, 元素3…};


int[] nums = new int[]{12, 24, 36};
int[] nums = {12, 24, 36};
double[] scores = new double[]{19.88, 4.16, 59.5, 88.0};
```

> :warning: 注：
>
> - `数据类型[] 数组名` 也可写成 `数据类型 数组名[]`
> - 什么类型的数组只能存放什么类型的数据

**数组在计算机中的基本原理**

![1661353166416](https://cdn.jsdelivr.net/gh/zhudaidai416/blog-image/1661353166416.png)

### 数组访问

格式：`数组名[索引]`

```java
int[] arr = {12, 24, 36};

// 访问数组
System.out.println(arr[0]); // 12
// 超过最大索引，会出现 ArrayIndexOutOfBoundsException 索引越界异常
// System.out.println(arr[3]);

// 修改数组数据
arr[0] = 66;
System.out.println(arr[0]); // 66

// 数组的元素个数：数组名.length
System.out.println(arr.length);

// 获取数组的最大索引: arr.length - 1(前提是数组中存在数据)
System.out.println(arr.length - 1);
```

### 数组遍历

遍历：就是一个一个数据的访问

```java
int[] ages = {20, 30, 40, 50};

// 快捷键：ages.fori + 回车
for (int i = 0; i < ages.length; i++) {
  System.out.println(ages[i]);
}
```

+++success 案例：数组遍历求和

```java
int[] arr = {16, 26, 36, 6, 100};
int sum = 0;

for (int i = 0; i < arr.length; i++) {
  sum += arr[i];
}
System.out.println("数组总和：" + sum);
```

+++

## 动态初始化数组

定义数组时先不存入具体的元素值，<font color="red">只确定数组存储的数据类型和数组的长度</font>

```java
数据类型[] 数组名 = new 数据类型[长度];

int[] arr = new int[3]; // 初始 arr = [0, 0, 0]
```

![1661356063895](https://cdn.jsdelivr.net/gh/zhudaidai416/blog-image/1661356063895.png)

> :warning: 注：静态初始化和动态初始化数组的写法是独立的，不可以混用
>
> ```java
> int[] arr = new int[3]{30, 40, 50}; // 错误写法
> ```

**动态初始化数组元素默认值规则**

<table>
  <tr>
    <th>数据类型</th>
    <th>明细</th>
    <th>默认值</th>
  </tr>
  <tr>
    <td rowspan=4>基本数据类型</td>
  </tr>
  <tr>
    <td>byte、short、char、int、long</td>
    <td>0</td>
  </tr>
  <tr>
    <td>float、double</td>
    <td>0.0</td>
  </tr>
  <tr>
    <td>boolean</td>
    <td>false</td>
  </tr>
  <tr>
    <td>引用数据类型</td>
    <td>类、接口、数组、String</td>
    <td>null</td>
  </tr>
</table>

+++success 两种方法的业务场景

- 动态初始化：适合开始不确定具体元素值，只知道元素个数的业务场景
- 静态初始化：适合一开始就知道要存入哪些元素值的业务场景

+++

## 执行原理

**Java 内存分配：**

- 方法区：字节码文件先加载到这里
- 栈：方法运行时所进入的内存区域，由于变量在方法中，所以变量也在这一块区域中
- 堆：new 出来的东西会在这块内存中开辟空间并产生地址
- 本地方法栈
- 寄存器

### 数组的执行原理

![1661438278304](https://cdn.jsdelivr.net/gh/zhudaidai416/blog-image/1661438278304.png)

### 多个变量指向同一个数组

```java
int[] arr1 = {11, 22, 33};
int[] arr2 = arr1; // 把arr1记录的地址值，赋值给arr2

System.out.println(arr1); // 两个数组存储相同的地址值
System.out.println(arr2);

arr2[1] = 99;
System.out.println(arr1[1]); // 99

arr2 = null;
System.out.println(arr2); // null
System.out.println(arr2[0]); // 异常
System.out.println(arr2.length); // 异常
```

- 多个数组变量中存储的是同一个数组对象的地址

- 多个变量修改的都是同一个数组对象中的数据

- 如果某个数组变量存储的地址是 null，那么该变量将不再指向任何数组对象

  可以输出这个变量，但是不能用这个数组变量去访问数据或者访问数组长度，会报空指针异常：NullPointerException

## 案例

;;;id1 数组求最值

```java
int[] numArr = {15, 9000, 10000, 20000, 9500, -5};
int max = numArr[0];

for (int i = 1; i < numArr.length; i++) {
  if(numArr[i] > max ){
    max = numArr[i];
  }
}
System.out.println("最大值是：" + max);
```

;;;

;;;id1 数组反转

:::success

需求：某个数组有 5 个数据 [10, 20, 30, 40, 50]，请将这个数组中的数据进行反转

:::

```java
int[] arr = {10, 20, 30, 40, 50};

// 定义一个循环，设计2个变量，一个在前，一个在后
for (int i = 0, j = arr.length - 1; i < j; i++, j--) {
  // arr[i]   arr[j] 交换
  // 1、定义一个临时变量记住后一个位置处的值
  int temp = arr[j];
  // 2、把前一个位置处的值赋值给后一个位置
  arr[j] = arr[i];
  // 3、把临时变量中记住的后一个位置处的值赋值给前一个位置处
  arr[i] = temp;
}

for (int i = 0; i < arr.length; i++) {
  System.out.print(arr[i] + " ");
}
```

;;;

;;;id1 随机排名

:::success

需求：某公司开发部 5 名开发人员，要进行项目进展汇报演讲，现在采取随机排名后进行汇报。请先依次录入 5 名员工的工号，然后展示出一组随机的排名顺序

:::

```java
// 1、定义一个动态初始化的数组用于存储5名员工的工号
int[] codes = new int[5];

// 2、提示用户录入5名员工的工号
Scanner sc = new Scanner(System.in);
for (int i = 0; i < codes.length; i++) {
  System.out.println("请您输入第" + (i + 1) +"个员工的工号：");
  int code = sc.nextInt();
  codes[i] = code;
}

// 3、打乱数组中的元素顺序
// [12, 33, 54, 26, 8]
//  i       index
Random r =  new Random();
for (int i = 0; i < codes.length; i++) {
  // 每遍历到一个数据，都随机一个数组索引范围内的值，然后让当前遍历的数据与该索引位置处的值交换
  int index = r.nextInt(codes.length); // 0 - 4
  // 定义一个临时变量记住index位置处的值
  int temp = codes[index];
  // 把i位置处的值赋值给index位置处
  codes[index] = codes[i];
  // 把index位置原来的值赋值给i位置处
  codes[i] = temp;
}

// 4、遍历数组中的工号输出即可
for (int i = 0; i < codes.length; i++) {
  System.out.print(codes[i] + " ");
}
```

;;;

## Debug 调试工具

- 打断点，如下图的红色小圆点
- 右键 Debug 方式启动程序，如下图右键菜单
- 启动后，代码会停留在打断点的这一行
- 点击箭头按钮，一行一行往下执行

![1661444896100](https://cdn.jsdelivr.net/gh/zhudaidai416/blog-image/1661444896100.png)

# 方法

方法是一种语法结构，它可以把一段代码封装成一个功能，以便重复调用

```java
修饰符 返回值类型 方法名(形参列表) {
  方法体代码(需要执行的功能代码)
  return 返回值;
}
```

- 方法的修饰符：暂时都使用`public static` 修饰

- 方法申明了具体的返回值类型，内部必须使用 return 返回对应类型的数据

- 形参列表可以有多个，甚至可以没有

  如果有多个形参，多个形参必须用 `,` 隔开，且不能给初始化值

## 方法调用

格式：`方法名(...)`

## 其他形式

- 是否需要接收数据处理 ➡ 是否需要定义形参列表
- 是否需要返回数据 ➡ 是否需要声明具体的返回值类型

```java
// 有参数、返回值
public static int sum(int a, int b) {
  int c = a + b;
  return c;
}

// 无参数、无返回值
public static void print(){
  System.out.println("hello！");
  System.out.println("hello！");
  System.out.println("hello！");
}

// 有参数，无返回值
public static void print(int n){
  for(int i = 1; i <= n; i++) {
    System.out.println("hello！");
  }
}
```

## 常见问题

### 定义方法

- 方法在类中的位置放前放后无所谓，但一个方法不能定义在另一个方法里面

  ```java
  // 错误写法
  public static void main(String[] args) {
    public static void add(){

    }
  }
  ```

- 方法没有申明返回值类型（<font color="red">void</font>），内部不能使用 return 返回数据

  方法申明了具体的返回值类型，内部必须使用 return 返回对应类型的数据

- return 语句的下面，不能编写代码，属于无效的代码，执行不到这儿

- 方法不调用就不会执行, 调用方法时，传给方法的数据，必须严格匹配方法的参数情况

### 调用方法

- 有返回值

  - 赋值调用

  - 输出调用

  - 直接调用

    ```java
    // 赋值调用：可以定义变量接收结果
    int rs = sum(5);
    System.out.println("1-5的和是：" + rs);

    // 输出调用
    System.out.println("1-5的和是：" + sum(5));

    // 直接调用
    sum(5);
    ```

- 无返回值：只能直接调用

## 执行原理

方法被调用的时候，是进入到栈内存中运行

栈的特点：先进后出

在栈中运行的原因：保证一个方法调用完另一个方法后，可以回来

![1661692070922](https://cdn.jsdelivr.net/gh/zhudaidai416/blog-image/1661692070922.png)

+++success 案例

;;;id2 有返回值

```java
public class MethodDemo {
  public static void main(String[] args) {
    int rs = sum(10, 20);
    System.out.println(rs);
}
  public static int sum(int a, int b ){
    int c = a + b;
    return c;
  }
}
```

![1661694127049](https://cdn.jsdelivr.net/gh/zhudaidai416/blog-image/1661694127049.png)

;;;

;;;id2 无返回值

```java
public class Demo2Method {
  public static void main(String[] args) {
    study();
  }
  public static void study(){
    eat();
    System.out.println("学习");
    sleep();
  }
  public static void eat(){
    System.out.println("吃饭");
  }
  public static void sleep(){
    System.out.println("睡觉");
  }
}
```

![1661696067585](https://cdn.jsdelivr.net/gh/zhudaidai416/blog-image/1661696067585.png)

;;;

+++

## 参数传递机制

Java 的参数传递机制都是：值传递

值传递：指的是在传输实参给方法的形参的时候，传输的是实参变量中存储的值的副本

- 实参：在方法内部定义的变量
- 形参：定义方法时 `(…)` 中所声明的参数

### 基本类型

![1661725470322](https://cdn.jsdelivr.net/gh/zhudaidai416/blog-image/1661725470322.png)

### 引用类型

![引用类型参数传递机制](https://cdn.jsdelivr.net/gh/zhudaidai416/blog-image/引用类型参数传递机制.png)

+++success 案例

;;;id3 打印数组

:::success

需求：输出一个 int 类型的数组内容，要求输出格式为：[11, 22, 33, 44, 55]

:::

```java
public class Test {
  public static void main(String[] args) {
    int[] arr = {11, 22, 33, 44, 55};
    printArray(arr); // [11, 22, 33, 44, 55]

    int[] arr2 = null;
    printArray(arr2);

    int[] arr3 = {};
    printArray(arr3); // []
  }

  public static void printArray(int[] arr){
    if(arr == null){
      System.out.println(arr); // null
      return; // 跳出当前方法
    }

    System.out.print("[");
    // 直接遍历接到的数组元素
    for (int i = 0; i < arr.length; i++) {
      i == arr.length - 1 ? System.out.print(arr[i]); : System.out.print(arr[i] + ", ");
    }
    System.out.println("]");
  }
}
```

;;;

;;;id3 比较数组

:::success

需求：如果两个 int 类型的数组，元素个数，对应位置的元素内容都是一样的，则认为这两个数组是一模一样的

:::

```java
public class Test {
  public static void main(String[] args) {
    int[] arr1 = {10, 20, 30};
    int[] arr2 = {10, 20, 30};
    System.out.println(equals(arr1, arr2));
  }

  public static boolean equals(int[] arr1, int[] arr2){
    if(arr1 == null && arr2 == null){
      return true; // 相等的
    }

    if(arr1 == null || arr2 == null) {
      return false; // 不相等
    }

    if(arr1.length != arr2.length){
      return false; // 不相等
    }

    for (int i = 0; i < arr1.length; i++) {
      if(arr1[i] != arr2[i]){
        return false; // 不相等的
      }
    }
    return true; // 两个数组是一样的
  }
}
```

;;;

+++

**总结：**

- 基本类型的参数传输存储的数据值
- 引用类型的参数传输存储的地址值

## 方法重载

一个类中，<font color="red">多个方法的名称相同</font>，但它们<font color="red">形参列表不同</font>

```java
public class Test {
  public static void main(String[] args) {
    test();
    test(100);
  }

  public static void test(){
    System.out.println("===test1===");
  }

  public static void test(int a){
    System.out.println("===test2===" + a);
  }

  void test(double a){

  }

  void test(double a, int b){
  }

  void test(int b, double a){
  }

  int test(int a, int b){
    return a + b;
  }
}
```

> :warning: 注：
>
> 一个类中，只要一些方法的名称相同、形参列表不同，那么它们就是方法重载了，其它的都不管（如：修饰符，返回值类型是否一样都无所谓）
>
> 形参列表不同指的是：形参的<font color="red">个数、类型、顺序</font>不同，不关心形参的名称

+++success 方法重载的应用场景

开发中我们经常需要为处理一类业务，提供多种解决方案，此时用方法重载来设计是很专业的

+++

## return

`return;` 可以用在无返回值的方法中，作用是：立即跳出并结束当前方法的执行

- return：跳出并立即结束所在方法的执行
- break：跳出并结束当前所在循环的执行
- continue：结束当前所在循环的当次继续，进入下一次执行

```java
public static void divide(int a , int b){
  if(b == 0){
    System.err.println("被除数不能为0”);
    return; // 直接跳出并结束当前方法
  }
  int c = a / b;
  System.out.println("计算结果是："+c);
}
```

# 面向对象基础

- 面向过程编程：开发一个一个的方法，有数据要处理了，我们就调方法来处理
- 面向对象编程：开发一个一个的对象来处理数据，把数据交给对象，再调用对象的方法来完成对数据的处理

对象本质上是一种特殊的数据结构

class 也就是类，也称为对象的设计图（或者对象的模板）

对象是用类 new 出来的，有了类就可以创建出对象

```java
public class 类名{
  1、变量，用来说明对象可以处理什么数据
  2、方法，描述对象有什么功能，也就是可以对数据进行什么样的处理
  ...
}

类名 对象名 = new 类名();
```

## 多个对象的执行原理

- Student s1 = new Student();
- 每次 new Student()，就是在堆内存中开辟一块内存区域代表一个学生对象
- s1 变量里面记住的是学生对象的地址

![1662213744520](https://cdn.jsdelivr.net/gh/zhudaidai416/blog-image/1662213744520.png)

## 注意事项

- 类名建议用英文单词，首字母大写，满足驼峰模式，且要有意义，比如：Student、Car…

- 类中定义的变量也称为成员变量（对象的属性），类中定义的方法也称为成员方法（对象的行为）

- 成员变量本身存在默认值，同学们在定义成员变量时一般来说不需要赋初始值（没有意义）

  <table>
    <tr>
      <th>数据类型</th>
      <th>明细</th>
      <th>默认值</th>
    </tr>
    <tr>
      <td rowspan=4>基本数据类型</td>
    </tr>
    <tr>
      <td>byte、short、char、int、long</td>
      <td>0</td>
    </tr>
    <tr>
      <td>float、double</td>
      <td>0.0</td>
    </tr>
    <tr>
      <td>boolean</td>
      <td>false</td>
    </tr>
    <tr>
      <td>引用数据类型</td>
      <td>类、接口、数组、String</td>
      <td>null</td>
    </tr>
  </table>

- 一个代码文件中，可以写多个 class 类，但只能一个用 public 修饰，且 public 修饰的类名必须成为代码文件名

- 对象与对象之间的数据不会相互影响，但多个变量指向同一个对象时就会相互影响了

- 如果某个对象没有一个变量引用它，则该对象无法被操作了，该对象会成为所谓的垃圾对象

> :warning: 注：
>
> 当堆内存中的对象，没有被任何变量引用（指向）时，就会被判定为内存中的“垃圾”
>
> Java 存在自动垃圾回收机制，会自动清除掉垃圾对象，程序员不用操心

## this 关键字

## 构造器

## 封装性

## 实体 JavaBean

## 成员变量和局部变量

# 常用 API

## 包

## String 类

## ArrayList 类

### 常用方法
