# 一、枚举 Enum
在 C# 中，枚举（enum）是一个值类型，它允许你定义一组命名常量。使用枚举可以提高代码的可读性和可维护性。

## 定义枚举
可以使用 enum 关键字来定义一个枚举类型：
~~~
public enum DaysOfWeek
{
    Sunday,
    Monday,
    Tuesday,
    Wednesday,
    Thursday,
    Friday,
    Saturday
}
~~~
默认情况下，枚举的底层类型是 int，枚举成员的值从 0 开始依次递增。
你也可以显式指定枚举成员的值：
~~~
public enum ErrorCode
{
    None = 0,
    NotFound = 404,
    InternalServerError = 500
}
~~~

## 使用枚举
可以通过名称或值来使用枚举：
~~~
DaysOfWeek today = DaysOfWeek.Wednesday;

int dayValue = (int)today;    // dayValue = 3
~~~

也可以通过枚举的值来获取枚举项：
~~~
DaysOfWeek day = (DaysOfWeek)3;

 // day = DaysOfWeek.Wednesday
~~~

## 枚举的常用操作
遍历枚举值，可以使用 Enum.GetValues 方法来遍历枚举值：
~~~
foreach (DaysOfWeek day in Enum.GetValues(typeof(DaysOfWeek)))
{
    Console.WriteLine(day);
}
~~~

获取枚举的名称和值,可以使用 Enum.GetNames 方法来获取枚举的名称列表：
~~~
string[] names = Enum.GetNames(typeof(DaysOfWeek));

foreach (string name in names)
{
    Console.WriteLine(name);
}
~~~

检查值是否在枚举中，可以使用 Enum.IsDefined 方法来检查一个值是否在枚举中定义：
~~~
bool isDefined = Enum.IsDefined(typeof(DaysOfWeek), 3);  // true

bool isDefined2 = Enum.IsDefined(typeof(DaysOfWeek), 8); // false
~~~

将字符串转换为枚举，可以使用 Enum.Parse 或 Enum.TryParse 方法将字符串转换为枚举值：
~~~
DaysOfWeek day = (DaysOfWeek)Enum.Parse(typeof(DaysOfWeek), "Wednesday");

if (Enum.TryParse("Wednesday", out DaysOfWeek result))
{
    // result now holds DaysOfWeek.Wednesday
}
~~~

## 标志枚举
标志枚举（Flags Enum）用于表示一组可组合的选项。可以使用 [Flags] 特性来定义一个标志枚举：
~~~
[Flags]
public enum FileAccess
{
    Read = 1,
    Write = 2,
    Execute = 4
}
~~~

使用标志枚举时，可以使用位操作符来组合值：
~~~
FileAccess access = FileAccess.Read | FileAccess.Write;

bool canRead = (access & FileAccess.Read) == FileAccess.Read;  // true
bool canWrite = (access & FileAccess.Write) == FileAccess.Write;  // true
bool canExecute = (access & FileAccess.Execute) == FileAccess.Execute;  // false
~~~

## 完整示例
~~~
using System;

public enum DaysOfWeek
{
    Sunday,
    Monday,
    Tuesday,
    Wednesday,
    Thursday,
    Friday,
    Saturday
}

[Flags]
public enum FileAccess
{
    Read = 1,
    Write = 2,
    Execute = 4
}

public class Program
{
    public static void Main()
    {
        // 使用 DaysOfWeek 枚举
        DaysOfWeek today = DaysOfWeek.Wednesday;
        Console.WriteLine($"Today is {today}");
        Console.WriteLine($"The numeric value of {today} is {(int)today}");

        // 遍历 DaysOfWeek 枚举
        foreach (DaysOfWeek day in Enum.GetValues(typeof(DaysOfWeek)))
        {
            Console.WriteLine(day);
        }

        // 使用 FileAccess 枚举
        FileAccess access = FileAccess.Read | FileAccess.Write;
        Console.WriteLine($"Access: {access}");

        bool canRead = (access & FileAccess.Read) == FileAccess.Read;
        bool canWrite = (access & FileAccess.Write) == FileAccess.Write;
        bool canExecute = (access & FileAccess.Execute) == FileAccess.Execute;

        Console.WriteLine($"Can Read: {canRead}");
        Console.WriteLine($"Can Write: {canWrite}");
        Console.WriteLine($"Can Execute: {canExecute}");
    }
}
~~~

ps：
枚举 https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/language-specification/enums
