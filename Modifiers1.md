# 访问修饰符
在 C# 中，修饰符用于控制类型成员（如类、字段、方法等）的特性和行为。   
以下是常见的修饰符，并按照它们的功能进行分类：  
- 访问修饰符（Access Modifiers）  
- 类型修饰符（Type Modifiers）  
- 成员修饰符（Member Modifiers）    

## （一）访问修饰符（Access Modifiers）    
用于控制成员的访问级别，即哪些部分的代码可以访问该成员。一个访问修饰符定义了一个类成员的范围和可见性。    

>在C#中，默认可访问性规则（即在没有明确指定访问修饰符的情况下）如下：
>>1.类、结构体、枚举、接口：    
 如果没有指定访问修饰符，则默认为 internal，即在同一程序集内可访问。    
>>2.成员（字段、方法、属性等）：     
 如果没有指定访问修饰符，则默认为 private，即只能在声明它们的类内部访问。      
    
C# 支持的访问修饰符如下所示：  
- [public](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/public)     
- [private](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/private)      
- [protected](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/protected)     
- [internal](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/internal)     
- [protected internal](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/protected-internal)    
- [private protected](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/private-protected)    

可访问性级别     
|声明的可访问性|含义|
|---------------|------|
|public |访问不受限制|
|protected|访问限于包含类或派生自包含类的类型|
|internal|访问限于当前程序集|
|protected internal    |访问限于当前程序集或派生自包含类的类型|
|private|访问限于包含类|
|private protected|访问限于包含类或当前程序集中派生自包含类的类型| 

### 1.public访问修饰符     
- 允许一个类将其成员变量和成员函数暴露给其他的函数和对象。任何公有成员可以被外部的类访问。
```

using System;

namespace RectangleApplication
{
    class Rectangle
    {
        //成员变量
        public double length;
        public double width;

        public double GetArea()
        {
            return length * width;
        }
        public void Display()
        {
            Console.WriteLine("长度： {0}", length);
            Console.WriteLine("宽度： {0}", width);
            Console.WriteLine("面积： {0}", GetArea());
        }
    }// Rectangle 结束

    class ExecuteRectangle
    {
        static void Main(string[] args)
        {
            Rectangle r = new Rectangle();
            r.length = 4.5;
            r.width = 3.5;
            r.Display();
            Console.ReadLine();
        }
    }
}
```
在上面的实例中，成员变量 length 和 width 被声明为 public，所以它们可以被函数 Main() 使用 Rectangle 类的实例 r 访问。      

### 2. protected 访问修饰符  
- 只有在通过派生类类型进行访问时，基类的受保护成员在派生类中才是可访问的。 以下面的代码段为例：    
```
class A
{
    protected int x = 123;
}

class B : A
{
    static void Main()
    {
        var a = new A();
        var b = new B();

        b.x = 10;
    }
}
```
在上面的实例中，类‘A’包含一个受保护的整型字段‘x’，而类'B‘继承自类’A‘。在 Main 方法中，我们试图访问这个字段。     
在C#中，受保护（protected）成员只能在声明它们的类或派生类的实例内部访问。因此，Main 方法位于类’B‘中，它可以访问类‘A’中的受保护字段‘x’。这就解释了为什么‘b.x = 10’; 是有效的。     
但当我们尝试使用‘a.x = 10’; 来访问‘x’字段时，会导致错误。这是因为‘Main’方法中的‘a'实例不属于类’A‘的派生类，因此无法访问’A‘中的受保护字段'x'。       

### 3. internal 访问修饰符     
- 只有在同一程序集的文件中，内部类型或成员才可访问。
```
using System;

namespace RectangleApplication
{
    class Rectangle
    {
        //成员变量
        internal double length;
        internal double width;

        //没有写访问修饰符，默认为private，GetArea用于计算矩形的面积
        double GetArea()
        {
            return length * width;
        }
       //定义一个公共public方法 Display()，用于显示矩形的信息，包括length、width、GetArea
       public void Display()
        {
            Console.WriteLine("长度： {0}", length);
            Console.WriteLine("宽度： {0}", width);
            Console.WriteLine("面积： {0}", GetArea());
        }
    }
    class ExecuteRectangle
    {
        //Main入口，从这里开始，先设置length和width的长度
        static void Main(string[] args)
        {
            Rectangle r = new Rectangle();
            r.length = 4.5;
            r.width = 3.5;
            r.Display();
            Console.ReadLine();
        }
    }
}
```
在上面的实例中，Internal 访问修饰符允许一个类将其成员变量‘length和width’和成员函数暴露给当前程序中的其他函数和对象。    

### 4. protected internal 访问修饰符    
- 允许在 本类,派生类 或者 包含该类的程序集 中访问。
- 这也被用于实现继承。
```
// 程序集Assembly1.cs
public class BaseClass
{
// 声明一个受保护内部的整型字段 myValue，初始值为 0
   protected internal int myValue = 0;
}
// 程序集Assembly1.cs
class TestAccess
{
    void Access()// 定义名为 Access 的方法
    {
 // 相同程序集，TestAccess 可以通过new BaseClass访问受保护内部成员myValue
        var baseObject = new BaseClass();
        baseObject.myValue = 5;//设置为5
    }
}
// 程序集Assembly2.cs
class DerivedClass : BaseClass
{
    static void Main()
    {
        var baseObject = new BaseClass();
        var derivedObject = new DerivedClass();

        //baseObject.myValue = 10;
        // 错误, 不同程序集，无法通过new BaseClass的实例去访问受保护内部成员 myValue

        derivedObject.myValue = 10;
        //不同程序集，但是可以通过派生类derivedObject去访问受保护内部成员 myValue
    }
}
```
在上面的实例中，在尝试修改‘baseObject.myValue’的值时会报错，因为‘myValue’只能被从‘BaseClass’继承的类访问。而修改‘derivedObject.myValue’的值是可以的，因为‘DerivedClass’是从’BaseClass‘派生的。    

### 5. private 访问修饰符    
- 私有访问是允许的最低访问级别。 私有成员只有在声明它们的类和结构体中才是可访问的
```
class Employee2
{
    // 设置_name、salary为私有字符串
    private readonly string _name = "FirstName, LastName";
    private readonly double _salary = 100.0;

    // 定义一个名为 GetName 的公共方法，用于获取姓名
    public string GetName()
    {
        return _name;
    }

    // 定义一个名为 Salary 的属性，用于获取工资
    public double Salary
    {
        // 使用 get 访问器返回 _salary 字段的值
        get { return _salary; }
    }
}

class PrivateTest
{
    static void Main()
    {
        // 创建一个名为 e 的 Employee2 类型的对象
        var e = new Employee2();

        // 成员变量是private，所以报错，无法访问
        //    string n = e._name;
        //    double s = e._salary;

        // 通过方法间接访问 '_name':
        string n = e.GetName();

        // 通过属性间接访问 '_salary'
        double s = e.Salary;
    }
}
```
- length 和 width 被声明为私有成员变量，以防止直接在类的外部访问和修改。
- AcceptDetails 方法允许用户输入矩形的长度和宽度，这是通过公有方法来操作私有变量的一个例子。
- GetArea 方法用于计算矩形的面积，而 Display 方法用于显示矩形的属性和面积。

### 6. private protected 访问修饰符
- 仅当变量的静态类型是派生类类型时，才可从派生的类型访问基类的私有受保护成员（在其包含程序集中访问）
```
//程序集 Assembly1.cs
public class BaseClass
{
    private protected int myValue = 0; //设为私有字符串
}
//程序集 Assembly1.cs
public class DerivedClass1 : BaseClass
{
    void Access()  // 定义名为 Access 的方法
    {
        var baseObject = new BaseClass();

        // 报错，因为 myValue 只能被继承自 BaseClass 的类访问。
        // baseObject.myValue = 5;
        myValue = 5;   //可以通过当前派生类实例访问
    }
}
// 程序集 Assembly2.cs
class DerivedClass2 : BaseClass
{
    void Access()
    {
        // 报错, 因为 myValue 只能被 Assembly1 中的类型访问
        // myValue = 10;
    }
}
```
- 此示例包含两个文件，即 Assembly1.cs 和 Assembly2.cs。第一个文件包含公共基类 BaseClass 及其派生的类型 DerivedClass1。
- BaseClass 拥有私有受保护成员 myValue，DerivedClass1 尝试以两种方式访问该成员。
- 通过 BaseClass 的实例第一次尝试访问 myValue 时会报错。 但如果尝试在 DerivedClass1 中将其用作 继承 的成员，则会成功。

### 7. private readonly 修饰符组合     
- 用来声明私有只读字段的修饰符组合。
- 只能在其声明的类内部访问，并且其值只能在构造函数中或字段初始化时被赋值，之后不能再修改。
```
public class MyClass
{
    private readonly int myReadOnlyField;

    public MyClass(int value)
    {
        myReadOnlyField = value; // 可以在构造函数中赋值
    }

    public void SomeMethod()
    {
        // 在类的内部可以访问 myReadOnlyField
        int x = myReadOnlyField;
        // 但无法修改它，下面的代码会引发编译错误
        // myReadOnlyField = 10;
    }
}
```
- 在上面的示例中，’myReadOnlyField‘字段被声明为‘private readonly int’，因此只能在‘MyClass’类内部访问，并且只能在构造函数中进行初始化。
- 在‘SomeMethod()’方法中，可以读取‘myReadOnlyField’的值，但不能修改它。
