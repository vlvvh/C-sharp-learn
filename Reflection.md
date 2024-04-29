# 反射（Reflection）
C# 中的反射（Reflection）是指 在运行时动态地 获取类型信息、访问对象的属性、方法和构造函数，以及调用对象的方法等操作。反射允许你在编译时不知道具体类型的情况下，对类型进行操作。
- 反射就是我们在只知道一个对象的外部而不了解内部结构的情况下，可以知道这个对象的内部实现‼️
  
### :large_blue_diamond:反射通常用于以下几种情况：

1️⃣ 动态加载程序集：通过反射，可以在运行时加载、查找和实例化程序集中的类型，而无需在编译时知道这些类型的存在。

2️⃣ 动态创建类型：通过反射，可以在运行时创建新的类型、属性和方法，从而实现动态代码生成和扩展。

3️⃣ 访问类型的成员：通过反射，可以在运行时访问对象的属性、字段、方法和事件，从而实现动态数据操作和调用。

4️⃣ 序列化和反序列化：反射常用于序列化和反序列化对象，例如将对象转换为 XML 或 JSON 格式，或者从 XML 或 JSON 中反序列化对象。

### :large_orange_diamond: 在 C# 中，反射主要通过 System.Reflection 命名空间下的类和方法来实现。        
一些常用的反射类包括：      
1. Assembly ： 用于加载和操作程序集。   

2. Type ： 用于表示类型信息。   

3. MethodInfo ： 用于表示方法的信息。    

4. PropertyInfo ： 用于表示属性的信息。  

5. FieldInfo ： 用于表示字段的信息。  

6. ConstructorInfo ： 用于表示构造函数的信息。  

7. ParameterInfo ： 用于表示方法参数的信息。   

## 反射中的运行时类型
反射提供类（ 如 Type 和 MethodInfo ），用于表示类型、成员、参数和其他代码实体。但使用反射时，你并不直接使用这些类，其中大部分类是抽象的。

例如：使用 typeof 运算符 获取 Type 对象时，该对象实际上是 RuntimeType 。RuntimeType 派生自 Type ，并提供所有方法的实现。
~~~
using System;

class Program
{
    static void Main()
    {
        object obj = "hello";
        Type type = obj.GetType();
        Console.WriteLine("运行时类型: " + type);
    }
}
~~~
在这个示例中，obj 是一个字符串对象，但是在编译时我们只知道 obj 是一个 object 类型的对象。然而，在运行时，obj 的实际类型是 string，所以 GetType() 方法返回 System.String 类型。

### Assembly 案例
~~~
public class Program
{
    static void Main (string[]args)
    {
        Assembly assem=Assembly.GetExecutingAssembly();
        Console.WriteLine("程序集名称："+assem.FullName);
        Console.WriteLine("版本："+assem.GetName().Version);
        Console.WriteLine("程序集的位置："+assem.CodeBase);
        Console.WriteLine("程序的完整路径："+assem.Location);
        Console.WriteLine("此程序集的入口点"+assem.EntryPoint);
        Type[] types = assem.GetTypes();
        foreach(var item in types)
        {
             Console.WriteLine("类："+item.Name);
        }
        Console.ReadKey();
    }
}
~~~
- 这个案例可以输出：
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/ba3faab6-d85c-453c-a4dd-54565b588ced)

***
## 🤔反射有什么优缺点呢❓
### ✅优点：
灵活性： 反射允许在运行时动态地获取和操作类型信息，这使得编写更加灵活和动态的代码成为可能。

动态加载： 反射允许在运行时加载和操作程序集，这使得可以动态地加载和使用程序集中的类型和成员，从而实现了插件化的架构和延迟加载的功能。

元数据访问： 反射提供了对类型的元数据（metadata）的访问，包括类的属性、方法、字段等信息。这使得可以在运行时检查和操作类型的结构，从而实现了一些高级的功能，比如序列化、自动映射等。

### :negative_squared_cross_mark: 缺点：
性能开销： 反射操作通常比静态类型访问更加耗费资源，因为它涉及到动态类型解析、方法调用和数据访问等操作

安全性问题： 反射可以访问类型的私有成员和方法，这可能会导致安全性问题。如果不加限制地使用反射，可能会造成程序的安全漏洞

代码可读性和维护性： 反射使得代码变得更加动态和抽象，可能会降低代码的可读性和维护性。
