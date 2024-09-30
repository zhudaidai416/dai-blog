---
title: 常用API（3）
date: 2024-08-28 17:22:48
category:
  - [计算机与科学, Java, Java基础加强]
tags: Java
cover: https://daiblog.oss-cn-chengdu.aliyuncs.com/cover/1-7.jpg
---

# Arrays

用来操作数组的一个工具类

## 基本使用

| 方法名                                                              | 说明                             |
| ------------------------------------------------------------------- | -------------------------------- |
| public static String toString(类型[] arr)                           | 返回数组的内容                   |
| public static int[] copyOfRange(类型[] arr, 起始索引, 结束索引)     | 拷贝数组（指定范围）             |
| public static copyOf(类型[] arr, int newLength)                     | 拷贝数组                         |
| public static setAll(double[] array, IntToDoubleFunction generator) | 把数组中的原数据改为新数据       |
| public static void sort(类型[] arr)                                 | 对数组进行排序（默认是升序排序） |

+++success 演示

```java
import java.util.Arrays;
import java.util.function.IntToDoubleFunction;

public class Test {
  public static void main(String[] args) {
    int[] arr = {10, 20, 30, 40, 50, 60};
    System.out.println(Arrays.toString(arr)); // [10, 20, 30, 40, 50, 60]

    int[] newArr1 = Arrays.copyOfRange(arr, 1, 4); // [20, 30, 40]
    int[] newArr2 = Arrays.copyOf(arr, 10); // [10, 20, 30, 40, 50, 60, 0, 0, 0, 0]

    // 修改原数据
    double prices[] = {99.8, 110, 60};
    Arrays.setAll(prices, new IntToDoubleFunction() {
      @Override
      public double applyAsDouble(int value) {
        return prices[value] * 0.8;
      }
    });
    System.out.println(Arrays.toString(prices)); // [79.84, 88.0, 48.0]

    // 排序（升序）
    Arrays.sort(prices);
    System.out.println(Arrays.toString(prices)); // [48.0, 79.84, 88.0]
  }
}
```

+++

## 操作对象数组

数组中存储的是对象，如何排序？

- 方式一：让该对象的类实现 Comparable（比较规则）接口，然后重写 compareTo 方法，自己来制定比较规则

- 方式二：使用下面这个 sort 方法，创建 Comparator 比较器接口的匿名内部类对象，自己制定比较规则

  `public static <T> void sort(T[] arr, Comparator<? super T> c)`：对数组进行排序（支持自定义排序规则）

+++success 方式一：代码演示

```java
// 学生类
import java.util.Objects;

public class Student implements Comparable<Student> {
  private String name;
  private int age;
  private double height;

  // 重写规则
  @Override
  public int compareTo(Student o) {
    // 写法一
    // if (this.age > o.age) {
    //   return 1;
    // } else if (this.age < o.age) {
    //   return -1;
    // }
    // return 0;

    // 写法二
    return this.age - o.age; // 升序
    // return o.age - this.age ; // 降序
  }

  // 重写 toString：可打印对象值
  @Override
  public String toString() {
    return "Student{" +
            "name='" + name + '\'' +
            ", age=" + age +
            ", height=" + height +
            '}';
  }

  // 有参、无参构造器...
  // get和set方法...
}
```

```java
// 测试类
import java.util.Arrays;

public class Test {
  public static void main(String[] args) {
    Student[] students = new Student[4];
    students[0] = new Student("呆呆", 18, 150);
    students[1] = new Student("朱朱", 36, 180);
    students[2] = new Student("朱呆呆", 19, 165);
    students[3] = new Student("小呆", 24, 160);

    Arrays.sort(students);
    System.out.println(Arrays.toString(students)); // 打印验证
  }
}
```

+++

+++success 方式二：代码演示

```java
// 测试类
import java.util.Arrays;
import java.util.Comparator;

public class Test {
  public static void main(String[] args) {
    Student[] students = new Student[4];
    students[0] = new Student("呆呆", 18, 150);
    students[1] = new Student("朱朱", 36, 180);
    students[2] = new Student("朱呆呆", 19, 165);
    students[3] = new Student("小呆", 24, 160);

    Arrays.sort(students, new Comparator<Student>() {
      @Override
      public int compare(Student o1, Student o2) {
        // 写法一
        // if (o1.getHeight() > o2.getHeight()) {
        //   return 1;
        // } else if (o1.getHeight() < o2.getHeight()) {
        //   return -1;
        // }
        // return 0;

        // 写法二
        // return Double.compare(o1.getHeight(), o2.getHeight()); // 升序
        return Double.compare(o2.getHeight(), o1.getHeight()); // 降序
      }
    });
    System.out.println(Arrays.toString(students)); // 打印验证
  }
}
```

+++

自定义排序规则时，需要遵循的官方约定如下：

- 左边对象 > 右边对象 ➡ 返回正整数
- 左边对象 < 右边对象 ➡ 返回负整数
- 左边对象 = 右边对象 ➡ 返回 0
