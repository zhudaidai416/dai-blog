---
title: 常用API（2）—— 关于日期时间
date: 2024-08-26 18:13:15
category:
  - [Java, Java基础, Java基础加强]
tags: Java
cover: https://img1.baidu.com/it/u=289468411,697847454&fm=253&fmt=auto&app=120&f=JPEG?w=800&h=500
---

# Date

代表的是日期和时间

| 构造器                 | 说明                                             |
| ---------------------- | ------------------------------------------------ |
| public Date()          | 创建一个 Date 对象，代表的是系统当前此刻日期时间 |
| public Date(long time) | 把时间毫秒值转换成 Date 日期对象                 |

🍋

| 常见方法                       | 说明                                                   |
| ------------------------------ | ------------------------------------------------------ |
| public long getTime()          | 返回从 1970 年 1 月 1 日 00:00:00 走到此刻的总的毫秒数 |
| public void setTime(long time) | 设置日期对象的时间为当前时间毫秒值对应的时间           |

+++success 演示

```java
public class Test {
  public static void main(String[] args) {
    Date d = new Date();
    System.out.println(d); // 当前时间

    long time = d.getTime();
    System.out.println(time); // 当前时间的毫秒值

    time += 2 * 1000;
    Date d2 = new Date(time);
    System.out.println(d2);

    Date d3 = new Date();
    d3.setTime(time); // 设置当前时间
    System.out.println(d3);
  }
}
```

+++

# SimpleDateFormat

代表简单日期格式化，可以用来把日期对象、时间毫秒值格式化成我们想要的形式

| 常见构造器                              | 说明                                     |
| --------------------------------------- | ---------------------------------------- |
| public SimpleDateFormat(String pattern) | 创建简单日期格式化对象，并封装时间的格式 |

🍋

| 格式化时间的方法                        | 说明                              |
| --------------------------------------- | --------------------------------- |
| public final String format(Date date)   | 将日期格式化成日期/时间字符串     |
| public final String format(Object time) | 将时间毫秒值式化成日期/时间字符串 |

# Calendar

代表的是系统此刻时间对应的日历，通过它可以单独获取、修改时间中的年、月、日、时、分、秒等

| 方法名                                | 说明                        |
| ------------------------------------- | --------------------------- |
| public static Calendar getInstance()  | 获取当前日历对象            |
| public int get(int field)             | 获取日历中的某个信息        |
| public final Date getTime()           | 获取日期对象                |
| public long getTimeInMillis()         | 获取时间毫秒值              |
| public void set(int field,int value)  | 修改日历的某个信息          |
| public void add(int field,int amount) | 为某个信息增加/减少指定的值 |

> :warning: 注：calendar 是可变对象，一旦修改后其对象本身表示的时间将产生变化

# JDK8 日期、时间、日期时间

- LocalDate：代表本地日期（年、月、日、星期）
- LocalTime：代表本地时间（时、分、秒、纳秒）
- LocalDateTime：代表本地日期、时间（年、月、日、星期、时、分、秒、纳秒）

| 方法名                                                 | 示例                                                                                                                                                                               |
| ------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| public static Xxxx now(): 获取系统当前时间对应的该对象 | LocaDate ld = LocalDate.now();<br>LocalTime lt = LocalTime.now();<br>LocalDateTime ldt = LocalDateTime.now();                                                                      |
| public static Xxxx of(…)：获取指定时间的对象           | LocalDate localDate1 = LocalDate.of(2099 , 11,11);<br>LocalTime localTime1 = LocalTime.of(9, 8, 59);<br>LocalDateTime localDateTime1 = LocalDateTime.of(2025, 11, 16, 14, 30, 01); |

## LocalDate

都是处理年、月、日、星期相关的

| 方法名                          | 说明                                     |
| ------------------------------- | ---------------------------------------- |
| public int geYear()             | 获取年                                   |
| public int getMonthValue()      | 获取月份（1-12）                         |
| public int getDayOfMonth()      | 获取日                                   |
| public int getDayOfYear()       | 获取当前是一年中的第几天                 |
| Public DayOfWeek getDayOfWeek() | 获取星期几：ld.getDayOfWeek().getValue() |

🍋

| 方法名                                             | 说明                                     |
| -------------------------------------------------- | ---------------------------------------- |
| withYear、withMonth、withDayOfMonth、withDayOfYear | 直接修改某个信息，返回新日期对象         |
| plusYears、plusMonths、plusDays、plusWeeks         | 把某个信息加多少，返回新日期对象         |
| minusYears、minusMonths、minusDays，minusWeeks     | 把某个信息减多少，返回新日期对象         |
| equals isBefore isAfter                            | 判断两个日期对象，是否相等，在前还是在后 |

## LocalTime

都是处理时、分、秒、纳秒相关的

| 方法名                 | 说明     |
| ---------------------- | -------- |
| public int getHour()   | 获取小时 |
| public int getMinute() | 获取分   |
| public int getSecond() | 获取秒   |
| public int getNano()   | 获取纳秒 |

🍋

| 方法名                                             | 说明                                      |
| -------------------------------------------------- | ----------------------------------------- |
| withHour、withMinute、withSecond、withNano         | 修改时间，返回新时间对象                  |
| plusHours、plusMinutes、plusSeconds、plusNanos     | 把某个信息加多少，返回新时间对象          |
| minusHours、minusMinutes、minusSeconds、minusNanos | 把某个信息减多少，返回新时间对象          |
| equals isBefore isAfter                            | 判断 2 个时间对象，是否相等，在前还是在后 |

## LocalDateTime

可以处理年、月、日、星期、时、分、秒、纳秒等信息

| 方法名                                                                                                  | 说明                                      |
| ------------------------------------------------------------------------------------------------------- | ----------------------------------------- |
| getYear、getMonthValue、getDayOfMonth、getDayOfYeargetDayOfWeek、getHour、getMinute、getSecond、getNano | 获取年月日、时分秒、纳秒等                |
| withYear、withMonth、withDayOfMonth、withDayOfYearwithHour、withMinute、withSecond、withNano            | 修改某个信息，返回新日期时间对象          |
| plusYears、plusMonths、plusDays、plusWeeksplusHours、plusMinutes、plusSeconds、plusNanos                | 把某个信息加多少，返回新日期时间对象      |
| minusYears、minusMonths、minusDays、minusWeeksminusHours、minusMinutes、minusSeconds、minusNanos        | 把某个信息减多少，返回新日期时间对象      |
| equals isBefore isAfter                                                                                 | 判断 2 个时间对象，是否相等，在前还是在后 |

# JDK8 日期（时区）

# JDK8 日期（Instant 类）

# JDK8 日期（格式化器）

# JDK8 日期（Period 类）

# JDK8 日期（Duration 类）
