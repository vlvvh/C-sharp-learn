# 面向对象 Object-Oriented    
## 1、 类 class
- 在面向对象的思想中最核心的就是**对象**，为了在程序中创建对象，首先需要【定义一个类】；
- 类 可以理解为一张设计图，描述了一个对象有什么构成，能做到什么事情；
- 一个类可以创建多个对象。
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/3d3aa58d-e5cf-4ab9-841d-843ba9548b6f)


:star: 例子展示：
~~~
namespace MethodParamDescriptor
{
    class Employee
    {
        //属性字段（成员变量）
        public string companyName = "";
        public string name = "";
        public string id = "";

        //类的行为（成员方法）
        public void IntroduceSelf()
        {
            Console.WriteLine("我是{0}公司的{1}，工号为{2}", companyName, name, id);//设置打印格式
        }

        public void call(string customerName)
        {
            Console.WriteLine("给{0}打电话，我是{1}公司的{2}，工号为{3}",customerName,companyName,name,id);
        }
        
    }

    internal class Program
    {
        static void Main(string[] args)
        {
           //设置填入占位符内容
            Employee e0 = new Employee();
            e0.companyName = "强盛集团";
            e0.name = "阿强";
            e0.id = "2025";

            Employee e1 = new Employee();
            e1.companyName = "强盛集团";
            e1.name = "啊盛";
            e1.id = "112";
            
            e0.IntroduceSelf();//将e0填写的内容运用在IntrodSelf方法中
            e1.IntroduceSelf();

            //分别拨打用户电话
            e0.call("1525644200");//将e0填写内容运用在call方法中
            e1.call("1288353530");
        }
    }
}
~~~

## 2、对象内存分布详解
- 对象变量为**引用类型**，new出来的多个对象，分别布局在了不同的内存上。
- ![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/8a787acf-9270-4208-80c6-357a0577cbfc)
- 成员变量🆚局部变量的区别：


|条目|成员变量|局部变量|
|:-----:|:---:|:----:|
|定义位置|方法外，类内|方法内|
|初始化|初始化为默认值|使用前必须手动初始化|
|内存位置|跟随对象放在堆内存|栈内存|
|生命周期|对象创建时诞生，对象消亡后消失|方法调用时诞生，方法结束后消失|
|作用域|对象内所有方法或通过对象.访问|所在最近大括号内|

## 3、this 引用   
- 当前**对象本身引用**，调用对象函数时，可以使用其引用**类内成员变量或方法**
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/2c9cc9d5-47a5-4a1a-af86-5b7282108999)
- 解决类内方法调用，局部变量与成员变量重名的问题


## 4、构造方法（Constructor）    
- 当对象被new创建出来的时间点，会自动调用该方法。
- 没有编写的情况下，C#为你自动生成；
  - 如果定义了构造方法，系统**不再给出**默认构造方法。
### 编写规范    
- 方法名与类名相同，大小写一致
- 无返回值，无void
- 内部不得return
### 构造方法特点  
- 每创建一个对象，调用一次
- 无法手动调用
### 构造方法重载
- 构造方法也满足方法的重载规则
### 构造方法工程规则
- 类的无参/带参构造方法都要手动给出
  -  ![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/7325b7ac-a4e0-4bcf-9b2a-e9dada2135c7)
- 构造方法内存流程
  - ![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/fe693e8f-c1b2-43df-809b-aded514479fb)

## 5、封装    
1️⃣ 将本类型相关的**数据信息**都放入类内统一管理    

2️⃣ 将本类型相关的**方法**都放入类内统一管理    

3️⃣ **将用户需要的内容暴露给用户，细节隐藏起来**
~~~
 class Fridge   //声明了一个名为Fridge的类。
   {
       public void push(string animal)  // 定义了一个公共方法push，接受一个string类型的参数animal，表示要放入冰箱的动物。
       {
           openDoor(); //调用一个名为openDoor的方法
           putln(animal);
           closeDoor();
       }

       public void putln(string animal) //定义了一个公共方法putln
       {
           Console.WriteLine("装入"+animal);
       }

       public void openDoor()
       {
           Console.WriteLine("开冰箱门");
       }

       public void closeDoor()
       {
           Console.WriteLine("关冰箱门");
       }

       public static void Main(string[] args) //定义了一个静态方法Main，是程序的入口点
       {
           Fridge f = new Fridge();  // 创建了一个Fridge类的实例f
           f.push("🐷");   //调用了实例的push方法，将一只🐷放入冰箱
           Console.WriteLine(f);   //打印f
       }
   }
~~~
### 封装——权限控制    
- 用于规定累内部属性/方法，能否背 特定区域访问的规则。
> :large_orange_diamond: 权限修饰符（可以修饰类/成员变量/成员方法）
> 
>> :small_orange_diamond: private(default)   : 只允许**类内**方法访问/修改
>> 
>> :small_orange_diamond: protected
>> 
>> :small_orange_diamond: internal
>> 
>> :small_orange_diamond: protected internal
>> 
>> :small_orange_diamond: public             : 任何方法都可对其访问/修改
>> 
***

### 属性与字段   
~~~
public class Player
{
    private string name;//访问不到
}
~~~
- 字段（Field）：在类内定义的变量，用于确定数据在内存中的储蓄
- 属性（Property）:提供对字段的访问器（Accessor）
~~~
public class Player
{
   private string name;
   public string getName
   {
       get{return name;}  //获取name变量
       set{name=value;}   //对name的值进行设置，传进来参数
   }
}
~~~
#### 属性用途（一）   
- 属性值验证：通过set/get方法，对传入的数据进行参数合法性的验证。
~~~
 public class Student
   {
       private int score=0;
         //情况1:用于对数据进行校验
       public int Score
       {
           get { return score; }
           set    //设置条件
           {
               if (value < 0 || value > 100)
               {
                   Console.WriteLine("学生的分数必须在0-100之间！"); //value为<0||>100时反应
                   return;
               }
               score = value;
           }
       }
   }

   internal class Program
   {
       static void Main(string[] args)
       {
           Student s = new Student();
           s.Score = 80;
           Console.WriteLine(s.Score);

       }
   }
~~~
#### 属性用途（二）
- 字段访问形式：通过get/set方法，可以给到用户合适的数据格式
~~~
需求：设计Clock类（字段：秒seconds）
- 访问方法：
- Hours，设置/获取当前是多少小时
- Minutes，设置/获取当前是多少分钟
   class Clock
    {
        public double seconds;

        public double Hours
        {
            get { return seconds / 3600; }
            set { seconds = value * 3600; }
        }

        public double Minutes
        {
            get { return seconds / 60; }
            set { seconds = value * 60; }
        }
    }

    internal class Program
    {
        static void Main(string[] args)
        {
            Clock c = new Clock();
            c.Hours = 2;
            Console.WriteLine(c.seconds);
            Console.WriteLine(c.Hours);
        }

    }
~~~


#### 属性省略
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/73cbbe9e-7544-470c-b21d-79dad3ce9807)    
:small_blue_diamond: 通过属性省略字段下，不可省略get字段；

:small_blue_diamond: 通过属性省略字段下，可以省略set字段；

:small_blue_diamond: set字段省略后，可以通过**构造方法**赋予初值。
~~~
class Player
{
   public int Money
   {
      get;    //类内其他方法不可对Money赋值，只能读取
   }
   public Player(){Money=100;}  //可以通过构造方法赋予初值
}
~~~
## static
### static 修饰符 
- static 含义为静态，可以修饰**成员变量**，也可以修饰**成员方法**
- 可以满足同一类生成的对象，共享此变量，一处更改，处处感知
- 规则：
  - 1️⃣ 使用方法：类名.变量名  类名.方法名（）
  - 2️⃣ 随着类加载而加载，优先于对象生成
  - 3️⃣ 静态方法内部，只能够访问静态成员变量，不能够访问普通成员变量
  - 4️⃣ 普通方法内部，可以访问静态成员变量/普通成员变量

~~~
 class Student
   {
       //2️⃣ 发现有一个静态变量count，将其分配到静态内存区，并且初始化为0
       //声明初始化
       static public int count = 0;
       
       //静态构造方法
       //3️⃣ 类加载并且扫描完毕后，直接运用此方法
       static Student()
       {
           //在此处可以进行相关逻辑编写
           count = 100;
       }
   }

   internal class Program
   {
       static void Main(string[] args)
       {
           //1️⃣ 碰到了Student类，加载类代码
           Console.WriteLine(Student.count);
       }
   }
~~~
### static 静态类   
- static关键字可以修饰类（class），被其修饰的类，叫静态类（Static Class）。
> 静态类规则：
>> 定义类： static class 名字
>> 
>> 成员： 只允许加入静态成员变量/属性 :white_check_mark:
>>       只允许加入静态成员方法 :white_check_mark:
>> 
>> 实例化：不允许使用 new 进行生成实例 ❎
>>
***
## 继承   
### 1、继承概念
- 描述了class之间的关系，is a的关系，子类可以使用父类的非私有成员
- 父类：
  - 是一个大的类型，封装了本类型通用的成员变量/成员方法，直接被子类继承使用
- 子类：
  - 是一个特殊的类，封装了本子类型独有的成员变量/成员方法
>继承当中的 成员变量/成员方法 访问规则：
>
>> 1️⃣ 子类保存所有父类的成员方法；
>>
>> 2️⃣ 子类成员方法汇“隐藏”父类同签名成员方法；
>>
>> 3️⃣ 子类中通过 base.关键词访问直接父类成员方法；
>>
>> 4️⃣ 子类中通过 new 关键词现实隐藏父类同签名成员方法；(不写new也可以，默认为子类）
>>
>> 5️⃣ 子类中方法与父类方法构成重载，不隐藏父类方法
>>

### 2、继承成员中的访问权限
|权限修饰符|同类|子类|外部累|
|:---:|:---:|:---:|:---:|
|public| :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
|protected| :heavy_check_mark: | :heavy_check_mark: | ❌ |
|private| :heavy_check_mark: |❌|❌|

#### 多层继承
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/e9a96e00-51fa-4708-9819-8672b7d8f091)

### 3、继承中构造方法的使用
- 当实例化一个子类对象的时候，首先调用其父类构造方法，在调用子类构造方法
- 当实例化一个子类对象的时候，如果父类只含有带参构造方法，则需要显式调用其父类的构造方法
~~~
//如果没有手动声明构造方法，系统默认生成无参构造方法

    class Father 
    {
        public int age = 0;
        public Father(int age)
        {
            this.age = age;
            Console.WriteLine("Father Constructor");
        }
    }
    class Son : Father
    {
        public Son():base(30)       //利用：base（填值）调用Father构造方法
        {
            Console.WriteLine("Son Constructor");
        }
    }
    internal class Program
    {
        static void Main(string[] args)
        {
            Son son = new Son();    //如无手动构造方法，则调用默认无参构造函数
            Console.WriteLine(son.age);
        }
    }
}
~~~
> 显示调用父类的构造方法
>> 1、子类构造方法如果 显式调用 父类的某一个构造方法，则遵从其调用选择
>> 
>> 2、子类构造方法 没有显示调用 父类的任何构造方法，则默认寻找其 **无参构造方法**

### 4、继承中的多态（polymorphism）
- 通过父类调用子类对应方法的实现
- 各类方法概念：
  - 1️⃣ 方法重载（overload）：参数个数/类型/顺序
  - 2️⃣ 方法隐藏（hide）：子类继承父类，子类有与父类相同签名的方法，则会隐藏起方法，二者并存
  - 3️⃣ 方法重写（override）：多态，通过父类类型对象，调用子类对象当中对应方法的实现
    - 细节：子类当中的 override方法 会’抹杀‘掉父类当中的对应 virtual方法
~~~
class Pet
{
   virtual public void shout()
   {
       Console.WriteLine("Pet")
   }
}
class Cat:Pet
{
   override public void shout()
   {
        Console.WriteLine("miao")
   }
}
class Dog:Pet
{
   override public void shout()
   {
       Console.WriteLine("wang")
   }
}
class Master
{
    public void play(Pet pet)
    {
        pet.shout();
    }
}
~~~
- 父类方法与子类方法签名一致
- virtual：修饰的方法为父类的 虚拟方法，表示可以被 子类方法override完全抹杀
- override：修饰的方法为子类的 重写方法，表示可以完全抹杀父类的对应方法
- virtual 🆚 override 成对存在，才能构成多态 :bangbang:
- 否则就是单纯子类隐藏父类方法
- :warning:	父类虚拟方法必须在子类 “可见”，不可为 private❌
- 在多层继承的环境中，多态会沿着 继承链条 进行重写方法的寻找锁定🔒

### 5、抽象类与抽象方法
- 使用abstract修饰的类为抽象类，基类❌不可实例化
- 使用abstract修饰的方法为抽象方法
- :warning:	拥有抽象方法的类，必须为抽象类
- 子类中 **【必须重写父类的抽象方法】**
~~~
abstact class Pet                      //抽象方法必须在抽象类里
{
    public string name="";
    abstact public void Shout(){}      //抽象方法 继承子类必须重写父类方法   //基类不可实例化 
}
class Dog:Pet
{
   override public void Shout()        //abstract 🆚 override 也要成对存在
   {
       Console.WriteLine("dog")；      //子类必须重写父类的抽象方法
   }
}
~~~

### 6、接口 interface
- 接口：用于规定一个Class 必须要实现哪些方法 的‘合同’
- 语法： interface I名字 {方法签名；...}    //接口名字都是I开头
> 接口特点：
> 
>> 只定义方法签名，不定义具体方法
>> 
>> 子类必须全部实现其定义的方法
>> 
>> 接口🉑以多继承

~~~
 interface IFireable
    { 
        public void Fire(); 
        public void MultiFire();
    }
    interface IRunnable
    {
        public void Run();
    }
    class MyObject
    {
        public int weight = 0;
    }
    class Tank : MyObject,IFireable,IRunnable   //子类可以同时继承父类以及其他接口，但父类继承需要写在最前方
    {                                           //类只能继承1个，接口可以继承多个
        public int weight = 100;
        public void Fire()
        {
            Console.WriteLine("咚！");
        }
        public void MultiFire()
        {
            Console.WriteLine("咚咚咚！");
        }
        public void Run()
        {
            Console.WriteLine("run");
        }
    }
    internal class OOPInterface
    {
        static void Main(string[] args)
        {
            Tank tank = new Tank();
            tank.MultiFire();       
            IFireable fireable = tank;       //1 使用接口类型可以承接一个子类型的对象
            fireable.Fire();
            IRunnable r = tank;              //坦克可以看作是两个东西
            r.Run();
            //IFireable f = new IFireable;   //2 接口和abstract不能new出对象❌
        }
        static public void Fire(IFireable fireable)
        {
            fireable.Fire();
        }
    }
~~~
:o: Interface 与 class 的区别理解
- 接口规定了用户从什么角度去使用当前对象
- 接口定义“需要做到什么”
- class定义“是什么，还要做到什么”is a的关系
