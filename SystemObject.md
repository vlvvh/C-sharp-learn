# Object 类
- 对于c#中所有的 class ，默认的父类或最终基类都是Object类（ System 命名空间下，简写为 Object ）
  
|System.Object 提供的方法|是否可以重写|
|:-----------:|:---:|
|string ToString();|:heavy_check_mark:|
|System.Type GetType();|❌|
|int GetHashCode();|:heavy_check_mark:|
|bool Equals();|:heavy_check_mark:|

## 1、string ToString();
- 用于‘打印’当前对象的信息，即将当前对象的字段们都转化为字符串，以一定格式打印输出
- 默认情况：打印输出当前对象类型全名（ namespace + class ）
- 用途：
  - 有特定打印输出需求，则可以 重写 ToString

~~~
namespace SeniorObject
{
    class Student
    {
        public int ID { get; set; }
        public string name { get; set; }
        public int Age { get; set; }
        
        public override string ToString()              // 右键生成重写，系统可以帮助生成 ToString 方法 这里需要用 override 重新生成
        {
            string result = "";
            result += $"学号:{ID}\n";                  //  & 符号：通过{}直接引用变量的值 ⚠️
            result += $"年龄:{Age}\n";
            result += $"名字:{name}\n";
            
            return result;                            // ToString用途： Debug 的时候打印对象信息
        }
    internal class Program
    {
        static void Main(string[] args)
        {
            Student student = new Student();
            student.name = "张三";
            student.ID = 8552;
            student.Age = 18;
            Console.WriteLine(student.ToString());    // 右键点 转到前往声明和用法，可以看到怎么使用，可以看到 ToString 是 virtual 了
        }
    }
}
~~~
## 2、System.Type GetType();
- 用于获取当前对象的类型信息（反射） 如类名、命名空间等
> Type
>> FullName：完整类型名，命名空间+类名
>> 
>> Name：类名
>> 
>> IsValueType：是否是值类型
>> 
>> IsClass：是否是引用类型
>>
~~~
namespace SeniorObject
{
    class Student
    {
        public int ID { get; set; }
        public string name { get; set; }
        public int Age { get; set; }
}
    internal class Program
    {
        static void Main(string[] args)
        {
            Student student = new Student();
            student.name = "张三";
            student.ID = 8552;
            student.Age = 18;
          
            ShowType(student);                     //获取类型信息（运行时）
 
        static public void ShowType(object o)      //从 object 里获取
        {
            Type type = o.GetType();
            Console.WriteLine(type.Name);           //类名
            Console.WriteLine(type.FullName);       //全名 namespace+ class
            Console.WriteLine(type.IsValueType);    //是否为值类型，是为true，否为false
            Console.WriteLine(type.IsClass);        //是否为引用类型，是为true，否为false
        }
    }
}
~~~
## 3、int GetHashCode();
- 用于获取对象的哈希码（ 哈希码 是通过将对象的字段或属性的值，转换为一个整数来生成的 ）
- 重写这个方法时，确保满足以下几点：
  - 对于相等的对象，它们的哈希码应该相同。
  - 如果 Equals() 方法返回 true，则它们的哈希码必须相同。
  - 但是，如果 Equals() 方法返回 false，它们的哈希码不一定不同，尽管大多数情况下应该是不同的，以提高哈希表的性能。

## 4、bool Equals();
- 用于判断两个对象是否相等，与“==”一致
- ==：值类型的 值 相同，引用类型的 地址 相同，字符串的 字符 相同
~~~
namespace SeniorObject
{
    class Student
    {
        public int Id { get; set; }
        public string? Name { get; set; }
        public int Age { get; set; }
        
        public override bool Equals(object? obj)                               // 对Equals方法进行重写
        {
            Student other = obj as Student;                                    // 1 尝试将obj转化为Student类型对象
            if (other == null) return false;                                   // 2 如果转化不成功，二者不具有可比性
            return Name == other.Name && Age == other.Age && Id == other.Id;   // 3 如果转化成功，则二者参数一一对比
        }
        protected bool Equals(Student other)
        {
            return Id == other.Id && Name == other.Name && Age == other.Age;
        }
    }
    internal class Program
    {
        static void Main(string[] args)
        {
            Student student = new Student();
            student.Name = "张三";
            student.Id = 8552;
            student.Age = 18;

            Student student0 = new Student();
            student0.Name= "张三";
            student0.Id = 8552;
            student0.Age = 18;
            Console.WriteLine(student.Equals(student0));                         //student与student0是否相等
        }
    }
}
~~~
## 5、装箱与拆箱
- 装箱：将 值类型 转换为 Object类型 的过程
- 拆箱：从 Object 中提取 值类型 的过程
~~~
//装箱
int i=123;
Object o=i;

//拆箱
int j=（int）o;
~~~
- 案例：请实现将各类数据装入同一个数组的过程，{1，2.5，"abc",cat}
~~~
// 1 装箱&拆箱
// 2 object作为基类
 internal class Program
    {
        static void Main(string[] args)
        {
            // 装箱&拆箱
            int i = 123;
            object o = i;        //装箱：将数字i装入对象o中
            int j = (int)o;      //拆箱：将数字123从 对象 o 当中取出来，给 变量 j
            Cat cat = new Cat();

            // 对于1和2.5都进行了装箱操作 "abc"和cat是子类放到了父类的操作
            // 1装入obj0当中，放到了obj数组的0号位
            // 2.5装入obj1当中，放到了objs的1号位
            // "abc"是引用类型，string类型的对象，string的基类是object
            // cat是引用类型，是Cat类的对象，因为Cat的基类是object

            object[] objs = { 1, 2.5, "abc", cat };   
        }
    }
~~~
- 内存原理
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/db58003e-c345-4eba-81ae-d89418095170)

