---
title: JDK8新特性
date: 2024-08-28 22:31:27
category:
  - [Java, Java基础加强]
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

## 静态方法

## 实例方法

## 特定类型方法

## 构造器

