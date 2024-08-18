---
title: Java 基础加强（上）
date: 2024-08-14 9:57:05
category:
  - [Java, 语法]
tags: Java
cover: https://daiblog.oss-cn-chengdu.aliyuncs.com/cover/3.jpg
---

# 面向对象高级

## static

叫静态，可以修饰成员变量、成员方法

### 修饰成员变量

成员变量按照有无 static 修饰，分为两种：

- ==类变量==（静态成员变量）：有 static 修饰，属于类，在计算机里只有一份，<font color="red">会被类的全部对象共享</font>

  格式：`类名.类变量`（推荐）、`对象名.类变量`（不推荐）

  适用场景：数据只需要一份，且需要被共享时（访问，修改）

- ==实例变量==（对象变量）：无 static 修饰，<font color="red">属于每个对象的</font>

  格式：`对象名.实例变量`

  适用场景：每个对象都要有一份，数据各不同（如：name、score、age）

**成员变量的执行原理：**

![1663978808670](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/1663978808670.png)

+++success 应用场景

在开发中，如果某个数据只需要一份，且希望能够被共享（访问、修改），则该数据可以定义成类变量来记住

```java
// 🌰：要求用户类记住创建了几个用户对象
public class User {
  public static int number;
  public User() {
    // 写法1：
    User.number++;
    
    // 写法2：访问自己类中的类变量，才可以省略类名
    number++;
  }
}
```

+++

### 修饰成员方法

- ==类方法==（静态方法）：有 static 修饰的成员方法，属于类

  格式：`类名.类方法`（推荐）、`对象名.类方法`（不推荐）

- ==实例方法==（对象的方法）：无 static 修饰的成员方法，属于对象

  格式：`对象名.实例方法`

**成员方法的执行原理：**

![1664005554987](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/1664005554987.png)

> 🔖 补充：main 方法是一个类方法

+++success 应用场景

可以用来设计工具类

- 工具类：工具类中的方法都是类方法，每个方法都是用来完成一个功能的，工具类是给开发人员共同使用的

- 好处：提高了代码复用；方便调用，提高了开发效率

  不用实例方法的原因：实例方法需要创建对象来调用，会浪费内存

```java
// 🔖 多学一招：工具类不需要创建对象，建议将工具类的构造器私有化
public class MyUtil {
 private MyUtil() {
 
 }
}

MyUtil t = new MyUtil(); // 无法创建对象
```

+++

### 注意事项

- 类方法中可以直接访问类成员，不可以直接访问实例成员
- 实例方法中可以直接访问类成员、实例成员
- 实例方法中可以出现 this 关键字，类方法中不可以出现 this 关键字的

### static  应用（代码块）

代码块是类的 5 大成分之一（成员变量、构造器、方法、代码块、内部类）

- 静态代码块

  - 格式：`static {}`

  - 特点：类加载时自动执行，由于类只会加载一次，所以静态代码块也只会执行一次
  - 作用：完成类的初始化，例如：对类变量的初始化赋值

- 实例代码块

  - 格式：`{}`

  - 特点：每次创建对象时，执行实例代码块，并在构造器前执行
  - 作用：和构造器一样，都是用来完成对象的初始化的，例如：对实例变量进行初始化赋值

+++success 案例

;;;id1 静态代码块

```java
public class Student {
  static int number = 80;
  static String schoolName = "呆呆";
  // 静态代码块
  static {
    System.out.println("静态代码块执行了~~");
    schoolName = "呆呆";
  }
}

//------------------------------------------------

public class Test {
  public static void main(String[] args) {
    System.out.println(Student.number);
    System.out.println(Student.number);
    System.out.println(Student.schoolName);
  }
}

运行结果：
  静态代码块执行了~~
  80
  80
  呆呆
```

;;;

;;;id1 实例代码块

```java
public class Student {
	int age;
  {
    System.out.println("实例代码块执行了~~");
    age = 18;
    System.out.println("有人创建了对象：" + this);
  }

  public Student() {
    System.out.println("无参数构造器执行了~~");
  }

  public Student(String name) {
    System.out.println("有参数构造器执行了~~");
  }
}

//------------------------------------------------

public class Test {
  public static void main(String[] args) {
    Student s1 = new Student();
    Student s2 = new Student("呆呆");
    System.out.println(s1.age);
    System.out.println(s2.age);
  }
}

运行结果：
  实例代码块执行了~~
  有人创建了对象：
  无参数构造器执行了~~
  实例代码块执行了~~
  有人创建了对象：
  有参数构造器执行了~~
  18
  18
```

;;;

+++

### static 应用（单例设计模式）

设计模式：设计模式就是具体问题的最优解决方案

单例设计模式：确保一个类只有一个对象

**写法：**

- 把类的构造器私有
- 定义一个类变量存储类的一个对象
- 定义一个类方法返回对象

==应用场景==：任务管理器对象、获取运行时对象，使用单例模式可以避免浪费内存

#### 饿汉式单例

特点：拿对象时，对象早就创建好了

```java
public class A {
  private static A a = new A();

  private A() {

  }

  public static A getInstance() {
    return a;
  }
}

--------------------------------
A a1 = A.getInstance();
A a2 = A.getInstance();
System.out.println(a1); // com.itheima.A@4eec7777
System.out.println(a2); // com.itheima.A@4eec7777
```

#### 懒汉式单例

特点：拿对象时，才开始创建对象

```java
public class B {
  private static B b;

  private B() {

  }

  public static B getInstance() {
    if (b == null) {
      System.out.println("第一次创建对象");
      b = new B();
    }
    return b;
  }
}

--------------------------------
B b1 = B.getInstance();
B b2 = B.getInstance();
System.out.println(b1 == b2); 
// 第一次创建对象
// true
```

## 继承

用 `extends` 关键字，让一个类和另一个类建立起一种父子关系

特点：子类能继承父类的非私有成员（成员变量、成员方法）

好处：减少了重复代码的编写，提高了代码的复用性

继承后对象的创建：子类的对象是由子类、父类共同完成的

对象能直接访问什么成员，是由子父类这多张设计图共同决定的，这多张设计图对外暴露了什么成员，对象就可以访问什么成员

```java
public class B extends A{}

➡ A类：父类（基类或超类）
➡ B类：子类（派生类）
```

**继承的执行原理：**

![1664010590126](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/1664010590126.png)

### 权限修饰符

用来限制类中的成员（成员变量、成员方法、构造器、代码块…）能够被访问的范围

| 修饰符    | 在本类里 | 同一个包下中的其他类里 | 任意包下的子类里 | 任意包下的任意类里 |
| --------- | -------- | ---------------------- | ---------------- | ------------------ |
| private   | 🗸        |                        |                  |                    |
| 缺省      | 🗸        | 🗸                      |                  |                    |
| protected | 🗸        | 🗸                      | 🗸                |                    |
| public    | 🗸        | 🗸                      | 🗸                | 🗸                  |

private ＜ 缺省 ＜ protected ＜ public

### 单继承、Object

Java 是<font color="red">单继承</font>的：一个类只能继承一个直接父类

Java 中的类<font color="red">不支持多继承</font>，但是<font color="red">支持多层继承</font>

object 类是 java 所有类的祖宗类。我们写的任何一个类，其实都是 object 的子类或子孙类

### 方法重写

方法重写：子类可以重写一个方法名称、参数列表一样的方法，去覆盖父类的这个方法

注意事项：

- 重写后，方法的访问，Java 会遵循就近原则
- 建议加上：`@Override` 注解，可以校验重写是否正确，同时可读性好
- 子类重写父类方法时，访问权限必须大于或者等于父类被重写的方法的权限<font color="red">（ public > protected > 缺省 ）</font>
- 重写的方法返回值类型，必须与被重写方法的返回值类型一样，或者范围更小
- 私有方法、静态方法不能被重写

==应用场景==：当子类觉得父类的方法不好用，或者不满足自己的需求时，就可以用方法重写

- 子类重写 Object 类的 toString() 方法，以便返回对象的内容

  +++success 🌰

  ```java
  public class Student extends Object{
    private String name;
    private int age;
  
    public Student() {
    }
  
    public Student(String name, int age) {
      this.name = name;
      this.age = age;
    }
  
    public String getName() {
      return name;
    }
  
    public void setName(String name) {
      this.name = name;
    }
  
    public int getAge() {
      return age;
    }
  
    public void setAge(int age) {
      this.age = age;
    }
    
    @Override
    public String toString() {
      return "Student{name=" + name + ", age=" + age + "}";
    }
    
    // idea 会自动生成：鼠标右键 ➡ 生成（Generate）➡ toString() ➡ 按住 shift 全选中
    @Override
    public String toString() {
      return "Student{" +
              "name='" + name + '\'' +
              ", age=" + age +
              '}';
    }
  }
  ```

  ```java
  public class Test {
    public static void main(String[] args) {
      Student s = new Student("呆呆", 18);
      // System.out.println(s.toString());
      System.out.println(s); // Student{name='呆呆', age=18}
    }
  }
  ```

  +++

### 子类中访问成员的特点

- 在子类方法中访问其他成员（成员变量、成员方法），是依照<font color="red">就近原则</font>的

  - 先子类局部范围找

  - 然后子类成员范围找
  - 然后父类成员范围找，如果父类范围还没有找到则报错

- 如果子父类中，出现了重名的成员，会优先使用子类的

- 可以通过 `super` 关键字，指定访问父类的成员：`super.父类成员变量/父类成员方法`

```java
public class Test {
  public static void main(String[] args) {
      // 目标：掌握子类中访问其他成员的特点：就近原则。
    B b = new B();
    b.showName();
    b.showMethod();
  }
}

public class A {
  String name = "父类成员变量";
  public void print1() {
    System.out.println("父类成员方法");
  }
}

public class B extends A {
  String name = "子类成员变量";

  public void showName() {
    String name = "局部名称";
    System.out.println(name); // 局部名称
    System.out.println(this.name); // 子类成员变量
    System.out.println(super.name); // 父类成员变量
  }

  @Override
  public void print1() {
    System.out.println("子类成员方法");
  }

  public void showMethod() {
    print1(); // 子类成员方法
    super.print1(); // 父类成员方法
  }
}
```

### 子类构造器的特点

子类的全部构造器，都会先调用父类的构造器，再执行自己

子类构造器是如何实现调用父类构造器的：

- 默认情况下，子类全部构造器的第一行代码都是 super() （写不写都有） ，它会调用父类的无参数构造器
- 如果父类没有无参数构造器，则我们必须在子类构造器的第一行手写 super(….)，指定去调用父类的有参数构造器

## 多态

## final 关键字

## 抽象

## 接口

## 内部类

## 枚举

## 泛型

## 常用 API

# 集合进阶（异常、集合）
