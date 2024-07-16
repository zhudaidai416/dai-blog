---
title: Java 基础（下）
categories: Java
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
int[] money = {16, 26, 36, 6, 100};
int sum = 0;

for (int i = 0; i < money.length; i++) {
  sum += money[i];
}
System.out.println("销售总额：" + sum);
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

Java 内存分配

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
int[] faceScores = {15, 9000, 10000, 20000, 9500, -5};
int max = faceScores[0];

for (int i = 1; i < faceScores.length; i++) {
  if(faceScores[i] > max ){
    max = faceScores[i];
  }
}
System.out.println("最高颜值是：" + max);
```

;;;

;;;id1 数组反转

:::success

需求：某个数组有 5 个数据[10, 20, 30, 40, 50]，请将这个数组中的数据进行反转

:::

```java
// 1、准备一个数组
int[] arr = {10, 20, 30, 40, 50};  

// 2、定义一个循环，设计2个变量，一个在前，一个在后
for (int i = 0, j = arr.length - 1; i < j; i++, j--) {
  // arr[i]   arr[j]
  // 交换
  // 1、定义一个临时变量记住后一个位置处的值
  int temp = arr[j];
  // 2、把前一个位置处的值赋值给后一个位置了
  arr[j] = arr[i];
  // 3、把临时变量中记住的后一个位置处的值赋值给前一个位置处
  arr[i] = temp;
}

// 3、检查
for (int i = 0; i < arr.length; i++) {
  System.out.print(arr[i] + " ");
}
```

;;;

;;;id1 随机排名

:::success

需求：某公司开发部 5 名开发人员，要进行项目进展汇报演讲，现在采取随机排名后进行汇报。请先依次录入5名员工的工号，然后展示出一组随机的排名顺序

:::

```java
// 1、定义一个动态初始化的数组用于存储5名员工的工号
int[] codes = new int[5];

// 2、提示用户录入5名员工的工号。
Scanner sc = new Scanner(System.in);
for (int i = 0; i < codes.length; i++) {
  // i = 0 1 2 3 4
  System.out.println("请您输入第" + (i + 1) +"个员工的工号：");
  int code = sc.nextInt();
  codes[i] = code;
}

// 3、打乱数组中的元素顺序。
// [12, 33, 54, 26, 8]
//  i       index
Random r =  new Random();
for (int i = 0; i < codes.length; i++) {
  // codes[i]
  // 每遍历到一个数据，都随机一个数组索引范围内的值。
  //然后让当前遍历的数据与该索引位置处的值交换。
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
