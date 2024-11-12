---
title: Convert 类
date: 2024-11-12 10:36:24
category:
  - [计算机与科学, C#]
tags: C#
cover: https://daiblog.oss-cn-chengdu.aliyuncs.com/cover/3-6.jpg
---

# 方法

## [FromBase64String(String)](https://learn.microsoft.com/zh-cn/dotnet/api/system.convert.frombase64string?view=netframework-4.7.2)

将指定的字符串（它将二进制数据编码为 Base64 数字）转换为等效的 8 位无符号整数数组

```csharp
public static byte[] FromBase64String (string s);
```

+++success 示例：使用 `ToBase64String(Byte)` 此方法将字节数组转换为 UUencoded (base-64) 字符串，然后调用 `FromBase64String(String)` 该方法来还原原始字节数组

```csharp
class Program
{
    static void Main(string[] args)
    {
        byte[] bytes = { 2, 4, 6, 8, 10, 12 };
        Console.WriteLine($"原始字节数组：{BitConverter.ToString(bytes)}");

        string s = Convert.ToBase64String(bytes);
        Console.WriteLine($"字节数组转换成base64字符串：{s}");

        byte[] newBytes = Convert.FromBase64String(s);
        Console.WriteLine($"还原原始字节数组：{BitConverter.ToString(newBytes)}");
    }
}

// 输出
// 原始字节数组：02-04-06-08-0A-0C
// 字节数组转换成base64字符串：AgQGCAoM
// 还原原始字节数组：02-04-06-08-0A-0C
```

+++
