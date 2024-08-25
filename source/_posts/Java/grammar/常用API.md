---
title: 常用API
date: 2024-08-17 23:36:08
category:
  - [Java, Java基础, Java基础加强]
tags: Java
cover: https://img1.baidu.com/it/u=289468411,697847454&fm=253&fmt=auto&app=120&f=JPEG?w=800&h=500
---

# API

API（Application Programming Interface）：应用程序编程接口

就是 Java 帮我们已经写好一些程序，如：类、方法等，我们直接拿过来用就可以解决一些问题

| Object           | LocalTime     | Set             | FileOutputStream    | Properties         |
| ---------------- | ------------- | --------------- | ------------------- | ------------------ |
| objects          | LocalDateTime | HashSet         | Reader              | Thread             |
| Integer          | Duration      | LinkedHashSet   | FileReader          | Runnable           |
| StringBuilder    | Period        | TreeSet         | Writer              | Callable           |
| StringBuffer     | ZoneId        | Map             | FileWriter          | ExecutorService    |
| Math             | ZonedDateTime | HashMap         | BufferdInputStream  | ThreadPoolExecutor |
| System           | Arrays        | LinkedHashMap   | BufferdOutputStream | Socket             |
| Runtime          | Comparable    | TreeMap         | BufferedReader      | ServerSocket       |
| BigDecimal       | Comparator    | Iterator        | BufferedWriter      | Class              |
| Date             | Collection    | Stream          | PrintStream         | Method             |
| SimpleDateFormat | List          | InputStream     | PrintWriter         | Constructor        |
| Calendar         | ArrayList     | FileInputStream | ObjectInputStream   | Field              |
| LocalDate        | LinkedList    | OutputStream    | ObjectOutputStream  | Proxy ...          |

# String

## 创建对象

String 创建对象封装字符串数据的方式

- 方式一： 直接使用双引号 `"..."`

  ```java
  String name = "呆呆";
  ```

- 方式二： 调用 String 类的构造器初始化字符串对象

  | 构造器                         | 说明                                   |
  | ------------------------------ | -------------------------------------- |
  | public String()                | 创建一个空白字符串对象，不含有任何内容 |
  | public String(String original) | 根据传入的字符串内容，来创建字符串对象 |
  | public String(char[] chars)    | 根据字符数组的内容，来创建字符串对象   |
  | public String(byte[] bytes)    | 根据字节数组的内容，来创建字符串对象   |

  ```java
  String rs1 = new String("abc");
  
  char[] chars = {'a', 'b', 'c'};
  String rs2 = new String(chars);
  
  byte[] bytes = {97, 98, 99};
  String rs3 = new String(bytes);
  ```

## 常用方法

| 方法名                                                               | 说明                                                     |
| -------------------------------------------------------------------- | -------------------------------------------------------- |
| public int length()                                                  | 获取字符串的长度返回（就是字符个数）                     |
| public char charAt(int index)                                        | 获取某个索引位置处的字符返回                             |
| public char[] toCharArray()                                          | 将当前字符串转换成字符数组返回                           |
| public boolean equals(Object anObject)                               | 判断当前字符串与另一个字符串的内容一样，一样返回 true    |
| public boolean equalsIgnoreCase(String anotherString)                | 判断当前字符串与另一个字符串的内容是否一样(忽略大小写)   |
| public String substring(int beginIndex, int endIndex)                | 根据开始和结束索引进行截取，得到新的字符串（包前不包后） |
| public String substring(int beginIndex)                              | 从传入的索引处截取，截取到末尾，得到新的字符串返回       |
| public String replace(CharSequence target, CharSequence replacement) | 使用新值，将字符串中的旧值替换，得到新的字符串           |
| public boolean contains(CharSequence s)                              | 判断字符串中是否包含了某个字符串                         |
| public boolean startsWith(String prefix)                             | 判断字符串是否以某个字符串内容开头，开头返回 true，反之  |
| public String[] split(String regex)                                  | 把字符串按照某个字符串内容分割，并返回字符串数组回来     |

- 基本数据类型的变量或者值应该使用 `==` 比较

- 对于字符串对象的比较，`==` 比较的是地址，容易出业务 bug

  使用 String 提供的 equals 方法，它只关心字符串内容一样就返回 true

## 注意事项

1、String 对象的内容不可改变，被称为不可变字符串对象

;;;id1 举个 🌰

name 值发生改变，为何说是不可变字符串对象？

```java
public static void main(String[] args) {
  String name = "黑马";
  name += "程序员";
  name += "波妞";
  System.out.println(name); // 黑马程序员波妞
}
```

;;;

;;;id1 原因

每次试图改变字符串对象，实际上是新产生了新的字符串对象了，变量每次都是指向了新的字符串对象，之前字符串对象的内容确实是没有改变的，因此说 String 的对象是不可变的

![1662610697641](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/1662610697641.png)

![1662610978351](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/1662610978351.png)

;;;

2、字符串字面量和 new 出来字符串的区别

只要是以 `"..."` 方式写出的字符串对象，会存储到字符串常量池，且相同内容的字符串只存储一份

![1662618688215](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/1662618688215.png)

但通过 new 方式创建字符串对象，每 new 一次都会产生一个新的对象放在堆内存中

![1662618651517](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/1662618651517.png)

```java
// 案例1
String s2 = new String("abc"); // 创建2个对象：常量池、堆内存
String s1 = "abc"; // 创建0个对象，因为常量池已经有了abc
System.out.println(s1 == s2); // false

// 案例2
String s1 = "abc";
String s2 = "ab";
String s3 = s2 + "c"; // 在堆内存中运算
System.out.println(s1 == s3); // false

// 案例3
String s1 = "abc";
String s2 = "a" + "b" + "c";
System.out.println(s1 == s2); // true
// Java 存在编译优化机制，程序在编译时："a" + "b" + "c"（确定的值）会直接转成"abc"，以提高程序的执行性能
```

## 案例

+++success 用户登录

```java
import java.util.Scanner;

public class Login {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    for (int i = 0; i < 3; i++) {
      System.out.println("请输入用户名：");
      String user = sc.next();
      System.out.println("请输入密码：");
      String password = sc.next();

      boolean rs = login(user, password);
      if (rs) {
        System.out.println("登录成功！");
        break;
      } else {
        System.out.println("用户名或密码错误，请重新登录！");
      }
    }
  }

  public static boolean login(String user, String password) {
    String okUser = "admin";
    String okPassword = "123456";
    return okUser.equals(user) && okPassword.equals(password);
  }
}
```

+++

+++success 开发验证码

```java
import java.util.Random;

public class Code {
  public static void main(String[] args) {
    System.out.println(createCode(4));
  }

  public static String createCode(int n) {
    String code = "";
    String data = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
    Random r = new Random();
    for (int i = 0; i < n; i++) {
      int index = r.nextInt(data.length());
      code += data.charAt(index);
    }
    return code;
  }
}
```

+++

# ArrayList

- 集合是一种容器，用来存储数据的

- 集合的大小可变
- 集合有很多种，而 ArrayList 只是众多集合中的一种

## 创建

集合中最常用的一种，ArrayList 是泛型类，可以约束存储的数据类型

```java
ArrayList list = new ArrayList(); // 创建一个空的集合对象

// 泛型
ArrayList<String> list = new ArrayList<String>();
// 从jdk1.7开始支持
ArrayList<String> list = new ArrayList<>();
```

- 集合都是支持泛型的

- 集合和泛型都不支持基本数据类型，只能支持引用数据类型

- 定义集合都应该采用泛型

  ```java
  // 要让集合什么都存
  ArrayList list = new ArrayList();
  ArrayList<Object> list = new ArrayList<>(); // ➡ 推荐这样写
  ```

> :warning: 注：ArrayList 可以存储自定义类型的对象，存储的是每个对象在堆内存中的地址

## 常用方法

| 常用方法名                           |                  说明                  |
| ------------------------------------ | :------------------------------------: |
| public boolean add(E e)              |     将指定的元素添加到此集合的末尾     |
| public void add(int index,E element) |   在此集合中的指定位置插入指定的元素   |
| public E get(int index)              |          返回指定索引处的元素          |
| public int size()                    |         返回集合中的元素的个数         |
| public E remove(int index)           | 删除指定索引处的元素，返回被删除的元素 |
| public boolean remove(Object o)      |    删除指定的元素，返回删除是否成功    |
| public E set(int index,E element)    | 修改指定索引处的元素，返回被修改的元素 |

```java
list.add("呆呆");
list.add(1,"daidai");

System.out.println(list.remove(1));
System.out.println(list.remove("daidai"));
// 若删除重复的数据，默认删除第一个出现的数据

System.out.println(list.set(0, "123")); // 呆呆
```

## 案例

+++success 举个 🌰：从容器中找出某些数据并成功删除

```java
import java.util.ArrayList;

public class ArrayListTest {
  public static void main(String[] args) {
    ArrayList<String> list = new ArrayList<>();
    list.add("泡菜");
    list.add("键盘");
    list.add("薯片");
    list.add("旺仔牛奶");
    list.add("柠檬味薯片");
    list.add("大薯片");
    System.out.println(list);

    // 方法一：每次删除一个数据后，索引-1
    for (int i = 0; i < list.size(); i++) {
      String ele =  list.get(i);
      if(ele.contains("薯片")) {
        list.remove(ele);
        i--;
      }
    }

    // 方法二：从集合的后面倒着遍历并删除
    for (int i = list.size() - 1; i >= 0; i--) {
      String ele = list.get(i);
      if (ele.contains("薯片")) {
        list.remove(ele);
      }
    }

    System.out.println(list);
  }
}
```

+++

+++success 举个 🌰：模仿外卖系统中的商家系统

1、菜品类 Food：描述每一个菜品对象要封装的数据

```java
public class Food {
  private String name;
  private double price;
  private String desc;

  public Food() {
  }

  public Food(String name, double price, String desc) {
    this.name = name;
    this.price = price;
    this.desc = desc;
  }

  public String getName() {
    return name;
  }

  public void setName(String name) {
    this.name = name;
  }

  public double getPrice() {
    return price;
  }

  public void setPrice(double price) {
    this.price = price;
  }

  public String getDesc() {
    return desc;
  }

  public void setDesc(String desc) {
    this.desc = desc;
  }
}
```

2、菜品操作类 FoodOperator：提供上架菜品的功能、浏览菜品的功能、展示操作界面的功能

```java
import java.util.ArrayList;
import java.util.Scanner;

public class FoodOperator {
  private ArrayList<Food> foodList = new ArrayList<>();

  public void addFood() {
    Food f = new Food();

    Scanner sc = new Scanner(System.in);
    System.out.println("请输入菜品名称：");
    String name = sc.next();
    f.setName(name);

    System.out.println("请输入菜品价格：");
    double price = sc.nextDouble();
    f.setPrice(price);

    System.out.println("请输入菜品描述：");
    String desc = sc.next();
    f.setDesc(desc);

    foodList.add(f);
    System.out.println("上架成功！");
  }

  public void showFood() {
    if (foodList.size() == 0) {
      System.out.println("暂无菜品，请添加！");
      return;
    }
    for (int i = 0; i < foodList.size(); i++) {
      Food f = foodList.get(i);
      System.out.println("菜品：" + f.getName());
      System.out.println("价格：" + f.getPrice());
      System.out.println("描述：" + f.getDesc());
      System.out.println("===================");
    }
  }

  public void start() {
    while (true) {
      System.out.println("请选择功能：1、上架菜品 2、展示菜品 3、退出");
      Scanner sc = new Scanner(System.in);
      System.out.println("请输入你的操作：");
      String command = sc.next();

      switch (command) {
        case "1":
          addFood();
          break;
        case "2":
          showFood();
          break;
        case "3":
          System.out.println("欢迎下次再来~~");
          return;
        default:
          System.out.println("输入错误，请重新输入！");
      }
    }
  }
}
```

3、测试类 Test：启动程序

```java
public class Test {
  public static void main(String[] args) {
    FoodOperator operator = new FoodOperator();
    operator.start();
  }
}
```

++++

# Object

Object 类是 Java 中所有类的祖宗类，Java 中所有类的对象都可以直接使用 Object 类中提供的一些方法

## 常用方法

| 方法名                          | 说明                                                         |
| ------------------------------- | ------------------------------------------------------------ |
| public String toString()        | 返回对象的字符串表示形式<br>默认的格式：`包名.类名@哈希值16进制` |
| public boolean equals(Object o) | 判断两个对象是否相等（判断地址）                             |
| protected Object clone()        | 对象克隆                                                     |

- toString 存在的意义：让子类重写，以便返回对象具体的内容

- equals 存在的意义：让子类重写，以便用于比较对象的内容是否相同

  直接比较两个对象的地址是否相同完全可以用“==”替代 equals

## toString()

idea 可自动生成

```java
public class Student {
  private String name;
  private int age;
  
  // 重写 toString()方法
  @Override
  public String toString() {
    return "Student{" +
            "name='" + name + '\'' +
            ", age=" + age +
            '}';
  }
  // get和set方法...
}
```

## equals()

idea 可自动生成

```java
public class Student {
  private String name;
  private int age;
  
  @Override
  public boolean equals(Object o) {
    // 1、判断是否是同一个对象比较，如果是返回true
    if (this == o) return true;
    // 2、如果o是null返回false
    // 如果o不是学生类型返回false  ...Student != ...Pig
    if (o == null || getClass() != o.getClass()) return false;
    // 3、说明o一定是学生类型而且不为null
    Student student = (Student) o;
    return age == student.age && Objects.equals(name, student.name);
  }
  // get和set方法...
}
```

## clone()

当某个对象调用这个方法时，这个方法会复制一个一模一样的新对象返回

- 浅克隆：拷贝出的新对象，与原对象中的数据一模一样（引用类型拷贝的只是地址）

  ![1665757187877](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/1665757187877.png)

- 深克隆：对象中基本类型的数据直接拷贝

  对象中的字符串数据拷贝的还是地址

  对象中包含的其他对象，不会拷贝地址，会创建新对象

  ![1665757265609](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/1665757265609.png)

+++success 演示

```java
public class User implements Cloneable{
  private String id;
  private String username;
  private String password;
  private double[] scores;

  public User() {
  }

  public User(String id, String username, String password, double[] scores) {
    this.id = id;
    this.username = username;
    this.password = password;
    this.scores = scores;
  }
  // get和set方法自己加上...
  
  @Override
  protected Object clone() throws CloneNotSupportedException {
    // 浅克隆
    return super.clone();
    
    // 深克隆
    // 先克隆得到一个新对象
    User u = (User) super.clone();
    // 再将新对象中的引用类型数据，再次克隆
    u.scores = u.scores.clone();
    return u;
  }
}
```

```java
// 测试类验证
public class Test {
  public static void main(String[] args) throws CloneNotSupportedException {
    User u1 = new User(1, "daidai", "123", new double[] {99.0, 99.5});
    User u2 = (User) u1.clone();
    System.out.println(u2.getId());
    System.out.println(u2.getUsername());
    System.out.println(u2.getPassword());
    System.out.println(u2.getScores()); 
  }
}
```

+++

# Objects

Objects 是一个工具类，提供了很多操作对象的静态方法给我们使用

## 常用方法

| 方法名                                           | 说明                                               |
| ------------------------------------------------ | -------------------------------------------------- |
| public static boolean equals(Object a, Object b) | 先做非空判断，再比较两个对象                       |
| public static boolean isNull(Object obj)         | 判断对象是否为 null，为 null 返回 true，反之       |
| public static boolean nonNull(Object obj)        | 判断对象是否不为 null，不为 null 则返回 true，反之 |

使用 Objects 类提供的 equals 方法来比较两个对象更安全

```java
// 源码分析
public static boolean equals(Object a, Object b) {
  return (a == b) || (a != null && a.equals(b));
}
```

+++success 演示

```java
public class Test {
  public static void main(String[] args) {
    String s1 = null;
    String s2 = "itheima";

    // 会出现 NullPointerException 异常，调用者不能为 null
    System.out.println(s1.equals(s2));
    // 此时不会有 NullPointerException 异常，底层会自动先判断空
    System.out.println(Objects.equals(s1,s2));

    System.out.println(Objects.isNull(s1)); // true
    System.out.println(s1 == null); // true

    System.out.println(Objects.nonNull(s2)); // true
    System.out.println(s2 != null); // true
  }
}
```

+++

# 包装类

把基本类型的数据包装成对象

| **基本数据类型** | **对应的包装类（引用数据类型）**   |
| ---------------- | ---------------------------------- |
| byte             | Byte                               |
| short            | Short                              |
| int              | <font color="red">Integer</font>   |
| long             | Long                               |
| char             | <font color="red">Character</font> |
| float            | Float                              |
| double           | Double                             |
| boolean          | Boolean                            |

- 自动装箱：基本数据类型可以自动转换为包装类型
- 自动拆箱：包装类型可以自动转换为基本数据类型

```java
Integer a = Integer.valueOf(10);

// 自动装箱 and 自动拆箱
Integer a1 = 10;
int a2 = a1;
```

泛型和集合不支持基本数据类型，因此可以使用包装类进行转换

## 包装类的方法

1、基本类型的数据转换成字符串

| public static String toString(double d) |
| --------------------------------------- |
| public String toString()                |

```java
Integer a = 12;

String s1 = Integer.toString(a);
String s2 = a.toString();
String s3 = a + "";
String s4 = String.valueOf(a);
```

2、字符串类型的数值转换成对应的基本数据类型

| public static int parseInt(String s)    |
| --------------------------------------- |
| public static Integer valueOf(String s) |

```java
String ageStr = "29";
int age = Integer.parseInt(ageStr);
int age = Integer.valueOf(ageStr);

String scoreStr = "3.14";
double score = Double.prarseDouble(scoreStr);
double score = Double.valueOf(scoreStr);
```

# StringBuilder

# StringJoiner

# Math

# System

# Runtime

# BigDecimal

# Date

# SimpleDateFormat

# Calendar

# JDK8 日期、时间、日期时间

# JDK8 日期（时区）

# JDK8 日期（Instant 类）

# JDK8 日期（格式化器）

# JDK8 日期（Period 类）

# JDK8 日期（Duration 类）
