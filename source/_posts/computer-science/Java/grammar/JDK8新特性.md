---
title: JDK8新特性
date: 2024-08-28 22:31:27
category:
  - [计算机与科学, Java, Java基础加强]
tags: Java
cover: https://img1.baidu.com/it/u=289468411,697847454&fm=253&fmt=auto&app=120&f=JPEG?w=800&h=500
---

# Lambda 表达式

## 基本用法

JDK8 开始新增的一种语法形式

**作用**：用于简化匿名内部类的代码写法（简化函数式接口的匿名内部类）

```java
(被重写方法的形参列表) -> {
  被重写方法的方法体代码;
}
```

> :warning: 注：Lambda 表达式只能简化函数式接口的匿名内部类！！！

函数式接口：

- 有且仅有一个抽象方法的接口
- 注意：将来我们见到的大部分函数式接口，上面都可能会有一个 `@FunctionalInterface` 的注解，有该注解的接口就必定是函数式接口

## 省略规则

- 参数类型可以省略不写

- 如果只有一个参数，参数类型可以省略，同时 `()` 也可以省略

- 如果 Lambda 表达式中的方法体代码只有一行代码，可以省略大括号不写，同时要省略分号

  此时，如果这行代码是 return 语句，也必须去掉 return 不写

```java
1、Lambda 的标准格式
  (参数类型1 参数名1, 参数类型2 参数名2) -> {
    ...方法体的代码...
    return 返回值;
  }

2、在标准格式的基础上()中的参数类型可以直接省略
  (参数名1, 参数名2) -> {
    ...方法体的代码...
    return 返回值;
  }

3、如果{}总的语句只有一条语句，则{}、return关键字、最后的“;”都可以省略
  (参数名1, 参数名2) -> 结果

4、如果()里面只有一个参数，则()可以省略
  参数名 -> 结果
```

;;;id1 标准格式

+++success Lambda 表达式简化 setAll 方法中匿名内部类

```java
Arrays.setAll(prices, new IntToDoubleFunction() {
  @Override
  public double applyAsDouble(int value) {
    return prices[value] * 0.8;
  }
});

👇🏻 简化后
Arrays.setAll(prices, (int value) -> {
  return prices[value] * 0.8;
});
```

+++

+++success Lambda 表达式简化 Comparator 接口的匿名形式

```java
Arrays.sort(students, new Comparator<Student>() {
  @Override
  public int compare(Student o1, Student o2) {
    return Double.compare(o1.getHeight(), o2.getHeight());
  }
});

👇🏻 简化后
Arrays.sort(students, (Student o1, Student o2) -> {
  return Double.compare(o1.getHeight(), o2.getHeight());
});
```

+++

;;;

;;;id1 简化格式

+++success Lambda 表达式简化 setAll 方法中匿名内部类

```java
// 省略参数类型
Arrays.setAll(prices, (value) -> {
  return prices[value] * 0.8;
});

// 省略()
Arrays.setAll(prices, value -> {
  return prices[value] * 0.8;
});

// 省略{}
Arrays.setAll(prices, value -> prices[value] * 0.8);
```

+++

+++success Lambda 表达式简化 Comparator 接口的匿名形式

```java
// 省略参数类型
Arrays.sort(students, (o1, o2) -> {
  return Double.compare(o1.getHeight(), o2.getHeight());
});

// 省略{}
Arrays.sort(students, (o1, o2) -> Double.compare(o1.getHeight(), o2.getHeight()));
```

+++

;;;

# 方法引用

进一步简化 Lambda 表达式

标志性符号：`:`

## 静态方法

**格式**：`类名::静态方法`

**使用场景**：如果某个 Lambda 表达式里只是调用一个静态方法，并且前后参数的形式一致，就可以使用静态方法引用

+++success 演示

```java
// 原始写法：对数组中的学生对象，按照年龄升序排序
Arrays.sort(students, new Comparator<Student>() {
  @Override
  public int compare(Student o1, Student o2) {
    return o1.getAge() - o2.getAge();
  }
});

// 使用 Lambda 简化
Arrays.sort(students, (o1, o2) -> o1.getAge() - o2.getAge());
```

准备一个 CompareByData 类：封装 Lambda 表达式的方法体

```java
public class CompareByData {
  public static int compareByAge(Student o1, Student o2) {
    return o1.getAge() - o2.getAge();
  }
}
```

简化

```java mark:4
Arrays.sort(students, (o1, o2) -> CompareByData.compareByAge(o1, o2));

// 静态方法引用
Arrays.sort(students, CompareByData::compareByAge);
```

+++

## 实例方法

**格式**：`对象名::实例方法`

**使用场景**：如果某个 Lambda 表达式里只是调用一个实例方法，并且前后参数的形式一致，就可以使用实例方法引用

+++success 演示

在 CompareByData 类中添加一个实例方法：封装 Lambda 表达式的方法体

```java
public class CompareByData {
  // 升序
  public static int compareByAge(Student o1, Student o2) {
    return o1.getAge() - o2.getAge();
  }

  // 降序
  public int compareByAgeDesc(Student o1, Student o2) {
    return o2.getAge() - o1.getAge();
  }
}
```

对象调用 compareByAgeDesc 方法

```java mark:5
CompareByData compare = new CompareByData();
Arrays.sort(students, (o1, o2) -> compare.compareByAgeDesc(o1, o2)); // 降序

// 实例方法引用
Arrays.sort(students, compare::compareByAgeDesc);
```

+++

## 特定类型方法

**格式**：`类型::方法`

**使用场景**：如果某个 Lambda 表达式里只是调用一个实例方法，并且前面参数列表中的第一个参数是作为方法的主调，后面的所有参数都是作为该实例方法的入参的，则此时就可以使用特定类型的方法引用

+++success 演示

```java mark:21
import java.util.Arrays;
import java.util.Comparator;

public class Test {
  public static void main(String[] args) {
    String[] names = {"baby", "angela", "Andy", "Lily", "coco", "Babo", "jack", "Cici"};
    System.out.println(Arrays.toString(names)); // [baby, angela, Andy, Lily, coco, Babo, jack, Cici]

    // 忽略首字符大小写排序
    Arrays.sort(names, new Comparator<String>() {
      @Override
      public int compare(String o1, String o2) {
        return o1.compareToIgnoreCase(o2);
      }
    });

    // Lambda 表达式
    Arrays.sort(names, (o1, o2) -> o1.compareToIgnoreCase(o2));

    // 特定类型方法引用
    Arrays.sort(names, String::compareToIgnoreCase);

    System.out.println(Arrays.toString(names)); // [Andy, angela, Babo, baby, Cici, coco, jack, Lily]
  }
}
```

+++

## 构造器

**格式**：`类名::new`

**使用场景**：如果某个 Lambda 表达式里只是在创建对象，并且前后参数情况一致，就可以使用构造器引用

+++success 演示

定义一个 Car 类

```java
public class Car {
  private String name;
  private double price;

  public Car() {}
  public Car(String name, double price) {
    this.name = name;
    this.price = price;
  }

 // get和set方法...

  @Override
  public String toString() {
    return "Car{" +
            "name='" + name + '\'' +
            ", price=" + price +
            '}';
  }
}
```

定义一个函数式接口，接口中的返回值类型是 Car 类型

```java
interface CreateCar {
  Car create(String name, double price);
}
```

测试类

```java 测试类 https://www.bilibili.com/video/BV1Cv411372m/?p=128&share_source=copy_web&vd_source=2a9e4fd26c8f35df01a9ac444c34eb4b 视频链接 mark:15
public class Test {
  public static void main(String[] args) {
    // 创建这个接口的匿名内部类对象
    CreateCar cc1 = new CreateCar() {
      @Override
      public Car create(String name, double price) {
        return new Car(name, price);
      }
    };

    // Lambda 表达式
    CreateCar cc2 = (name, price) -> new Car(name, price);

    // 构造器引用
    CreateCar cc3 = Car::new;

    // 注意：以上是创建CreateCar接口实现类对象的几种形式而已，语法一步一步简化

    // 对象调用方法
    Car car = cc3.create("奔驰", 49.9);
    System.out.println(car);
  }
}
```

+++
