# 泛型 Generic
- 使用 占位符<T> 来代表某种类型，编译期间再决定其具体类型(不一定是T，也可以是别的字符）;
-  格式：class MyGeneric<T>;
-  使用：MyGeneric< int >mg=MyGeneric<int>();
-  原理：使用特化的类型替代掉占位符，生成具体的class代码.
  ![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/1f13bb21-4fb2-4359-ae03-682f8645ea57)

~~~
泛型修饰class举例：

class Store<T>                                // T 是类型占位符，还未决定是什么类型（不一定用T表示，其他字符也可以）
{
    private T[] arr = new T[100]; 
    public void Put(T v, T index)
    {
        arr[index] = v;                       // 将数组 arr 中索引为 index 的元素的值设置为 v 
    }
}
class Program
{
    static void Main(string[] args)
    {
        Store<int> store = new Store<int>();  // 对泛型 类Store 的特化，特化为 int类型

    }
}
~~~
- 通过“参数化类型”，实现在同一份代码上操作多种数据类型
- 泛型可以修饰：类、方法、委托等
~~~
泛型修饰方法举例：

class Store<T>                                             // T是类型占位符，还未决定是什么类型（不一定用T表示，其他字符也可以）
{
    private T[] arr = new T[100];                          //声明了一个泛型数组 arr，其中 T 是类的类型，表示数组可以存储任意类型的元素。数组被初始化为长度为 100 的新数组。

    public void Put(T v, T index)                          //这是一个公共方法 Put 的声明，它接受两个参数：一个类型为 T 的值 v，和一个类型为 T index。
    { 
        arr[index] = v;                                    //参数 v 的值存储到数组 arr 的指定索引位置 index 上。
    }
}
class Cat{}

class Program
{
    static void Main(string[] args)
    {
        Store<int> store = new Store<int>();                // 对泛型 类Store 的特化，特化为 int类型
        
        int m=10,n= 20;
        Swap<int>(ref m, ref n);                            // 泛型方法的使用

        Console.WriteLine(Add<int>(2, 4));
        Console.WriteLine(Add<float>(2.2f, 5.5f));

        Cat c0 = new Cat();                                // Cat类 c0c1不可相加，运用dynamic 可以不报错
        Cat c1 = new Cat();
        Add<Cat>(c0, c1);
    }
    static public void Swap<T1>(ref T1 a, ref T1 b)         // 1 交换两个变量的值     //ref:用于声明引用参数,通过将参数声明为 ref，方法可以直接修改原始变量的值，而不需要返回值。
    {
        T1 tmp = a;
        a = b;
        b = tmp;
    }
    static public T1 Add<T1>(T1 a, T1 b)                    // 2 求两个变量的和
    {
        dynamic da = a;                                     // dynamic 是动态的意思，通过 dynamic 将类型校验推迟到运行时⚠️
        dynamic db = b;
        return da + db;
    }
}
~~~
## 一、泛型细节
- 泛型可以同时提供多种数据类型的占位符（ 类/方法均有效 ）
~~~
class Store<T,U>                   //可以提供多个占位符
{
     public T[]arr0{get;set;}
     public U[]arr1{get;set;}
}
class Program
{
    static void Main(string[] args)
    {
        StoreDouble<int, double> storeDouble = new StoreDouble<int, double>();    // T2为 int ，U 为 double类型
    }
}
~~~
- 泛型类 可以被继承！子类可以 指定父类泛型的具体类型（特化）或者 子类也作为泛型类 :bangbang:
~~~
class Person<T>                          // 父类 设T 占位符
{
    public T id { get; set; }
}

class Teacher : Person<int>{}            // 子类直接指定父类泛型的具体类型 int

class Teacher<T,U> : Person<T>           // 子类也作为泛型类，设U占位符。
{
    public U Data { get; set; }
}

class Program
{
    static void Main(string[] args)
    {
        Teacher<int, float> teacher = new Teacher<int, float>();    // T 为 int类型，U 为 float类型
    }
}
~~~
## 二、泛型约束
- 对泛型中传入的类型进行“校验”，规定起必须满足的条件
### （ 1 ）引用类型约束
- 泛型必须是引用类型
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/29736f7c-5b47-4f1a-9048-0a18f8556719)
- int是值类型，T 必须是 引用类型，所以❌不可以用int、float、double、char、bool、struct等值类型
- :small_red_triangle: 引用类型：Class、Interface、Array、Delegate、string、Dynamic
~~~
class MyGenericClass<T> where T : class{}                       // 泛型T 必须是引用类型，class表示引用类型的约束

internal class Program 
{ 
 static void Main(string[] args)
    {
       //MyGenericClass<int> m = new MyGenericClass<int>();     // class表示引用类型的约束，int是值类型，所以报错🙅
       MyGenericClass<string> m = new MyGenericClass<string>(); // string是引用类型，可以✅
     }
}
~~~
### （ 2 ）值类型约束
- 泛型必须是值类型
- 格式：
  - public class AGenericClass< T >where T:struct
~~~
class MyGenericClassForValueType<T> where T : struct{}                       // 泛型T 必须是值类型，class表示值类型的约束

internal class Program 
{ 
 static void Main(string[] args)
    {
       MyGenericClassForValueType<int> m = new MyGenericClassForValueType<int>();     // struct表示值类型的约束，int是值类型可以✅
       MyGenericClassForValueType<string> m = new MyGenericClassForValueType<string>(); // string是引用类型，所以报错🙅
     }
}
~~~

### ( 3 )  new（）约束
- 泛型T 必须包含一个无参构造方法的class
- 如果同时有其他约束，则new（）必须放在最后 :bangbang:
~~~
class TestForNew                                         // 如果不手动写个方法，系统会自动生成一个无参构造函数
{
    public TestForNew(){}                                // 手动生成无参构造函数
    public TestForNew(int a){}                           // 手动生成一个有参数的构造函数,class方法里有一个无参构造方法即可
}
class MyGenericClassForNew<T> where T : class, new() {}  // 当前 T 必须包含无参构造方法,当有多个约束时，new()必须放在最后‼️‼️

internal class Program 
{ 
 static void Main(string[] args) 
      {
          MyGenericClassForNew<TestForNew> m = new MyGenericClassForNew<TestForNew>();
      }
}
~~~

### ( 4 ) 类名约束
- 泛型T 必须是 某类 或 派生自某类
~~~
class Person{}                      // 父类

class Teacher : Person{}           // Teacher继承Person类

class EnglishTeacher : Teacher{}   //EnglishTeacher继承Teacher类

class MyGenericClassForPerson<T> where T : Person{}  // 当前 泛型T 必须是 某类或者派生类

internal class Program 
{ 
 static void Main(string[] args)
 {
     MyGenericClassForPerson<Teacher> teahcer = new MyGenericClassForPerson<Teacher>();
     MyGenericClassForPerson<EnglishTeacher> el = new MyGenericClassForPerson<EnglishTeacher>();
 }
}
~~~

### （ 5 ）接口约束
- 泛型T 必须实现一个或多个接口
~~~
interface IFireable{ void Fire();}            // 设接口IFireable

interface IRunnable{void Run();}              // 设接口IRunnable

class Machine{}                               // 设 Machine类

class Tank : Machine，IFireable, IRunnable    // 需要先继承Machine类，在继承接口
{
    public void Fire(){}
    public void Run(){}
}
class MyGenericClassForInterface<T> where T : MachineI，Fireable,IRunnable{}   // 类与接口共用时，类放在接口前方

internal class Program 
{ 
 static void Main(string[] args)
 {
     MyGenericClassForInterface<Tank> m = new MyGenericClassForInterface<Tank>(); // 选择Tank类时，要注意Tank是否继承了所有需要继承的类与接口
 }
}
~~~

### （ 6 ）多占位符约束
- 使用 where关键字 对多个占位符进行各自的约束 :small_red_triangle:
~~~
class TestForNew               // 如果不手动写个方法，系统会自动生成一个无参构造函数
{
    public TestForNew(){}      // 手动生成无参构造函数
    public TestForNew(int a){} // 手动生成一个有参数的构造函数，class方法里有一个无参构造方法即可
}
interface IFireable{ void Fire();}

interface IRunnable{void Run();}

class Tank : IFireable, IRunnable
{
    public void Fire(){}
    public void Run(){}
}
class MyGenericClassForInterface<T> where T : IFireable,IRunnable{}

class MyGenericClassForMulti<T,B>where T:class,new() where B:IFireable,IRunnable{} 
                                            // T:是一个引用类型和无参构造方法  B:必须实现 IFireable和IRunnable 接口 对 T 和 B 各自约束
internal class Program 
{ 
 static void Main(string[] args)
 {
     MyGenericClassForMulti<TestForNew, Tank> m = new MyGenericClassForMulti<TestForNew, Tank>();
 }                                          // TestForNew是一个class且有无参构造方法   Tank同时继承了 IFireable和IRunnable 
}
~~~

## 三、泛型约束与继承
- 拥有泛型约束的类作为父类，则其子类可以选择特化父类，或者 使用与父类同样/更严格的约束
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/ddee186b-3600-4332-a79e-185669f4d9d8)



