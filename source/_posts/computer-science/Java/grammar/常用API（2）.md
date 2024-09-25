---
title: 常用API（2）—— 关于日期时间
date: 2024-08-26 18:13:15
category:
  - [计算机与科学, Java, Java基础加强]
tags: Java
cover: https://daiblog.oss-cn-chengdu.aliyuncs.com/cover/6.jpg
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

时间格式的常见符号：

| 符号 | 说明      |
| ---- | --------- |
| y    | 年        |
| M    | 月        |
| d    | 日        |
| H    | 时        |
| m    | 分        |
| s    | 秒        |
| EEE  | 星期      |
| a    | 上午/下午 |

解析字符串时间成为日期对象：

| 解析方法                                                  | 说明                       |
| --------------------------------------------------------- | -------------------------- |
| public Date <font color="red">parse(String source)</font> | 把字符串时间解析成日期对象 |

+++success 演示

```java
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class Test {
  public static void main(String[] args) throws ParseException {
    Date d1 = new Date();
    long time = d1.getTime();

    SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss EEE a");
    String rs1 = sdf.format(d1);
    System.out.println(rs1);
    String rs2 = sdf.format(time);
    System.out.println(rs2);

    // 指定的时间格式必须和被解析的时间格式一样，否则会出bug
    String dateStr = "2024-08-27 04:16:00";
    SimpleDateFormat sdf2 = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
    Date d2 = sdf2.parse(dateStr);
    System.out.println(d2);
  }
}
```

+++

+++info 案例：秒杀活动

```java
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class Test {
  public static void main(String[] args) throws ParseException {
    String start = "2024年11月11日 0:0:0";
    String end = "2024年11月11日 0:10:0";
    String user1 = "2024年11月11日 0:01:16";
    String user2 = "2024年11月11日 0:10:02";

    // 解析
    SimpleDateFormat d = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
    Date start_date = d.parse(start);
    Date end_date = d.parse(end);
    Date user1_date = d.parse(user1);
    Date user2_date = d.parse(user2);

    // 转换
    long start_time = start_date.getTime();
    long end_time = end_date.getTime();
    long user1_time = user1_date.getTime();
    long user2_time = user2_date.getTime();

    if (user1_time >= start_time && user1_time <= end_time) {
      System.out.println("用户1秒杀成功！");
    } else {
      System.out.println("用户1秒杀失败~~");
    }
    if (user2_time >= start_time && user2_time <= end_time) {
      System.out.println("用户2秒杀成功！");
    } else {
      System.out.println("用户2秒杀失败~~");
    }
  }
}
```

+++

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

+++success 演示

```java
import java.util.Calendar;
import java.util.Date;

public class Test {
  public static void main(String[] args) {
    Calendar now = Calendar.getInstance();
    int year = now.get(Calendar.YEAR);
    System.out.println(year);

    Date date = now.getTime();
    System.out.println("日期对象：" + date);

    long time = now.getTimeInMillis();
    System.out.println("时间毫秒值：" + time);

    // 修改时间
    now.set(Calendar.YEAR, 2026);
    now.set(Calendar.MONTH, 5);
    now.set(Calendar.DAY_OF_YEAR, 6); // 修改成一年的第6天

    // 增加时间
    now.add(Calendar.DAY_OF_YEAR, 6);
    now.add(Calendar.DAY_OF_MONTH, 2);
  }
}
```

+++

# JDK8 日期类新增的原因

- JDK8 之前：传统的时间 API
  - 设计不合理，使用不方便，很多都被淘汰了
  - 都是可变对象，修改后会丢失最开始的时间信息
  - 线程不安全
  - 只能精确到毫秒
- JDK8 开始之后：新增的时间 API
  - 设计更合理，功能丰富，使用更方便
  - 都是不可变对象，修改后会返回新的时间对象，不会丢失最开始的时间
  - 线程安全
  - 能精确到毫秒、纳秒

:::info no-icon

1 秒 = 1000 毫秒

1 毫秒 = 1000 微秒

1 微秒 = 1000 纳秒

1 秒 = 1000 000 000 纳秒

:::

# JDK8 👇🏻

# 日期时间

- LocalDate：代表本地日期（年、月、日、星期）
- LocalTime：代表本地时间（时、分、秒、纳秒）
- LocalDateTime：代表本地日期、时间（年、月、日、星期、时、分、秒、纳秒）

| 方法名                                                 | 示例                                                                                                                                                                               |
| ------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| public static Xxxx now()：获取系统当前时间对应的该对象 | LocaDate ld = LocalDate.now();<br>LocalTime lt = LocalTime.now();<br>LocalDateTime ldt = LocalDateTime.now();                                                                      |
| public static Xxxx of(…)：获取指定时间的对象           | LocalDate localDate1 = LocalDate.of(2099 , 11,11);<br>LocalTime localTime1 = LocalTime.of(9, 8, 59);<br>LocalDateTime localDateTime1 = LocalDateTime.of(2025, 11, 16, 14, 30, 01); |

## LocalDate（年月日）

年、月、日、星期

| 方法名                          | 说明                                   |
| ------------------------------- | -------------------------------------- |
| public int getYear()            | 获取年                                 |
| public int getMonthValue()      | 获取月份（1-12）                       |
| public int getDayOfMonth()      | 获取日                                 |
| public int getDayOfYear()       | 获取当前是一年中的第几天               |
| Public DayOfWeek getDayOfWeek() | 获取星期：ld.getDayOfWeek().getValue() |

🍋

| 方法名                                             | 说明                                     |
| -------------------------------------------------- | ---------------------------------------- |
| withYear、withMonth、withDayOfMonth、withDayOfYear | 直接修改某个信息，返回新日期对象         |
| plusYears、plusMonths、plusDays、plusWeeks         | 把某个信息加多少，返回新日期对象         |
| minusYears、minusMonths、minusDays、minusWeeks     | 把某个信息减多少，返回新日期对象         |
| equals、isBefore、isAfter                          | 判断两个日期对象，是否相等，在前还是在后 |

+++success 演示

```java
import java.time.LocalDate;

public class Test {
  public static void main(String[] args) {
    LocalDate d = LocalDate.now();
    System.out.println(d); // 2024-08-27

    int year = d.getYear(); // 2024
    LocalDate d2 = d.withYear(2026); // 2026-08-27
    LocalDate d3 = d.plusYears(4); // 2028-08-27
    LocalDate d4 = LocalDate.of(1988, 04, 16);

    System.out.println(d4.equals(d)); // false
    System.out.println(d4.isBefore(d)); // true
    System.out.println(d4.isAfter(d)); // false
  }
}
```

+++

## LocalTime（时分秒）

时、分、秒、纳秒

| 方法名                 | 说明     |
| ---------------------- | -------- |
| public int getHour()   | 获取小时 |
| public int getMinute() | 获取分   |
| public int getSecond() | 获取秒   |
| public int getNano()   | 获取纳秒 |

🍋

| 方法名                                             | 说明                                     |
| -------------------------------------------------- | ---------------------------------------- |
| withHour、withMinute、withSecond、withNano         | 修改时间，返回新时间对象                 |
| plusHours、plusMinutes、plusSeconds、plusNanos     | 把某个信息加多少，返回新时间对象         |
| minusHours、minusMinutes、minusSeconds、minusNanos | 把某个信息减多少，返回新时间对象         |
| equals、isBefore、isAfter                          | 判断两个时间对象，是否相等，在前还是在后 |

## LocalDateTime（年月日时分秒）

年、月、日、星期、时、分、秒、纳秒

| 方法名                                                                                                      | 说明                                      |
| ----------------------------------------------------------------------------------------------------------- | ----------------------------------------- |
| getYear、getMonthValue、getDayOfMonth、getDayOfYear<br>getDayOfWeek、getHour、getMinute、getSecond、getNano | 获取年月日、时分秒、纳秒等                |
| withYear、withMonth、withDayOfMonth、withDayOfYear<br>withHour、withMinute、withSecond、withNano            | 修改某个信息，返回新日期时间对象          |
| plusYears、plusMonths、plusDays、plusWeeks<br>plusHours、plusMinutes、plusSeconds、plusNanos                | 把某个信息加多少，返回新日期时间对象      |
| minusYears、minusMonths、minusDays、minusWeeks<br>minusHours、minusMinutes、minusSeconds、minusNanos        | 把某个信息减多少，返回新日期时间对象      |
| equals、isBefore、isAfter                                                                                   | 判断 2 个时间对象，是否相等，在前还是在后 |

## 转换

| 方法名                         | 说明                      |
| ------------------------------ | ------------------------- |
| public LocalDate toLocalDate() | 转换成一个 LocalDate 对象 |
| public LocalTime toLocalTime() | 转换成一个 LocalTime 对象 |

```java
// 转换
LocalDate ld = ldt.toLocalDate();
LocalTime lt = ldt.toLocalTime();
// 合并
LocalDateTime ldt2 = LocalDateTime.of(ld, lt);
```

# 时区

世界标准时间（UTC）

中国标准时间：世界标准时间（UTC）+ 8 小时

![时区](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/%E6%97%B6%E5%8C%BA.png)

## ZoneId（时区）

时区

| 方法名                                          | 说明                       |
| ----------------------------------------------- | -------------------------- |
| public static Set<String> getAvailableZoneIds() | 获取 Java 中支持的所有时区 |
| public static ZoneId systemDefault()            | 获取系统默认时区           |
| public static ZoneId of(String zoneId)          | 获取一个指定时区           |

## ZonedDateTime（带时区的时间）

带时区的时间

| 方法名                                                                                                  | 说明                              |
| ------------------------------------------------------------------------------------------------------- | --------------------------------- |
| public static ZonedDateTime now()                                                                       | 获取当前时区的 ZonedDateTime 对象 |
| public static ZonedDateTime now(ZoneId zone)                                                            | 获取指定时区的 ZonedDateTime 对象 |
| getYear、getMonthValue、getDayOfMonth、getDayOfYeargetDayOfWeek、getHour、getMinute、getSecond、getNano | 获取年月日、时分秒、纳秒等        |
| public ZonedDateTime withXxx(时间)                                                                      | 修改时间系列的方法                |
| public ZonedDateTime minusXxx(时间)                                                                     | 减少时间系列的方法                |
| public ZonedDateTime plusXxx(时间)                                                                      | 增加时间系列的方法                |

+++success 演示

```java
import java.time.Clock;
import java.time.ZoneId;
import java.time.ZonedDateTime;

public class Test {
  public static void main(String[] args) {
    // 获取系统默认的时区
    ZoneId zoneId = ZoneId.systemDefault();
    System.out.println(zoneId.getId()); // Asia/Shanghai

    System.out.println(ZoneId.getAvailableZoneIds()); // 获取全部时区Id
    ZoneId zoneId2 = ZoneId.of("America/New_York");

    ZonedDateTime now = ZonedDateTime.now(zoneId2);
    System.out.println("美国纽约时区的时间：" + now);

    ZonedDateTime utc = ZonedDateTime.now(Clock.systemUTC());
    System.out.println("世界标准时间：" + utc);

    ZonedDateTime now2 = ZonedDateTime.now();
    System.out.println("系统默认时区的时间：" + now2);

    // Calendar instance = Calendar.getInstance(TimeZone.getTimeZone(zoneId));
  }
}
```

+++

# Instant（时间戳/时间线）

时间线上的某个时刻/时间戳

- 通过获取 Instant 的对象可以拿到此刻的时间，该时间由两部分组成：从 1970-01-01 00:00:00 开始走到此刻的<font color="red">总秒数 + 不够 1 秒的纳秒</font>
- **作用**：可以用来记录代码的执行时间，或用于记录用户操作某个事件的时间点
- 传统的 Date 类，只能精确到毫秒，并且是可变对象
- 新增的 Instant 类，可以精确到纳秒，并且是不可变对象，推荐用 Instant 代替 Date

![](https://daiblog.oss-cn-chengdu.aliyuncs.com/img/1667401373923.png)

| 方法名                              | 说明                                        |
| ----------------------------------- | ------------------------------------------- |
| public static Instant now()         | 获取当前时间的 Instant 对象（标准时间）     |
| public long getEpochSecond()        | 获取从 1970-01-01T00：00：00 开始记录的秒数 |
| public int getNano()                | 从时间线开始，获取从第二个开始的纳秒数      |
| plusMillis plusSeconds plusNanos    | 判断系列的方法                              |
| minusMillis minusSeconds minusNanos | 减少时间系列的方法                          |
| equals、isBefore、isAfter           | 增加时间系列的方法                          |

+++success 演示

```java
public class Test {
  public static void main(String[] args) {
    Instant now = Instant.now(); // 不可变对象

    long second = now.getEpochSecond(); // 总秒数
    int nano = now.getNano(); // 纳秒数

    Instant instant = now.plusNanos(111);

    // Instant 对象的作用：做代码的性能分析，或者记录用户的操作时间点
    Instant now1 = Instant.now();
    // 代码执行。。。
    Instant now2 = Instant.now();

  }
}
```

# DateTimeFormatter（格式化）

格式化器，用于时间的格式化、解析

| 方法名                                              | 说明             |
| --------------------------------------------------- | ---------------- |
| public static DateTimeFormatter ofPattern(时间格式) | 获取格式化器对象 |
| public String format(时间对象)                      | 格式化时间       |

LocalDateTime 提供的格式化、解析时间的方法

| 方法名                                                                           | 说明       |
| -------------------------------------------------------------------------------- | ---------- |
| public String format(DateTimeFormatter formatter)                                | 格式化时间 |
| public static LocalDateTime parse(CharSequencetext, DateTimeFormatter formatter) | 解析时间   |

+++success 演示

```java
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

public class Test {
  public static void main(String[] args) {
    // 格式化器对象
    DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy年MM月dd日 HH:mm:ss");
    LocalDateTime now = LocalDateTime.now();
    System.out.println(now); // 2024-08-28T16:43:56.407816900

    // 方式一：正向格式化
    String rs = formatter.format(now); // 2024年08月28日 16:43:56
    // 方式二：反向格式化
    String rs2 = now.format(formatter); // 2024年08月28日 16:43:56

    // 解析
    String dateStr = "1988年04月16日 04:16:00";
    LocalDateTime ldt = LocalDateTime.parse(dateStr, formatter);
    System.out.println(ldt); // 1988-04-16T04:16
  }
}
```

+++

# Period（计算日期间隔）

可以用于计算两个 LocalDate 对象相差的年数、月数、天

| 方法名                                                       | 说明                               |
| ------------------------------------------------------------ | ---------------------------------- |
| public static Period between(LocalDate start, LocalDate end) | 传入两个日期对象，得到 Period 对象 |
| public int getYears()                                        | 计算隔几年，并返回                 |
| public int getMonths()                                       | 计算隔几个月，年返回               |
| public int getDays()                                         | 计算隔多少天，并返回               |

+++success 演示

```java
public class Test {
  public static void main(String[] args) {
    LocalDate start = LocalDate.of(2024, 3, 5);
    LocalDate end = LocalDate.of(2029, 4, 16);
    Period between = Period.between(start, end);

    System.out.println("间隔年数：" + between.getYears()); // 间隔年数：5
    System.out.println("间隔月数：" + between.getMonths()); // 间隔月数：1
    System.out.println("间隔天数：" + between.getDays()); // 间隔天数：11
  }
}
```

+++

# Duration（计算时间间隔）

可以用于计算两个时间对象相差的天数、小时数、分数、秒数、纳秒数

支持 LocalTime、LocalDateTime、Instant 等时间

| 方法名                                                         | 说明                                 |
| -------------------------------------------------------------- | ------------------------------------ |
| public static Duration between(开始时间对象 1, 截止时间对象 2) | 传入两个时间对象，得到 Duration 对象 |
| public long toDays()                                           | 计算隔多少天，并返回                 |
| public long toHours()                                          | 计算隔多少小时，并返回               |
| public long toMinutes()                                        | 计算隔多少分，并返回                 |
| public long toSeconds()                                        | 计算隔多少秒，并返回                 |
| public long toMillis()                                         | 计算隔多少毫秒，并返回               |
| public long toNanos()                                          | 计算隔多少纳秒，并返回               |

+++success 演示

```java
import java.time.Duration;
import java.time.LocalDateTime;

public class Test {
  public static void main(String[] args) {
    LocalDateTime start = LocalDateTime.of(2024, 8, 28, 11, 10, 10);
    LocalDateTime end = LocalDateTime.of(2024, 8, 28, 11, 11, 11);
    Duration between = Duration.between(start, end);

    System.out.println(between.toDays()); // 0
    System.out.println(between.toHours()); // 0
    System.out.println(between.toMinutes()); // 1
    System.out.println(between.toSeconds()); // 61
    System.out.println(between.toMillis()); // 61000
    System.out.println(between.toNanos()); // 61000000000
  }
}
```

+++
