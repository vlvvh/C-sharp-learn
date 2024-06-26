# IoC 控制反转
## 一、IoC 基础概念
- IoC 指的是 "Inversion of Control"，即控制反转。它是一种软件设计原则，用于改变传统的程序控制流程，以便更好地实现 松耦合 和 可测试性。


### 1.传统的软件开发的对象 A B C D 的依赖关系强的话，就会如下图：
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/a1b94f92-2579-488a-a91d-722b285cf7d7)
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/8800f84d-0643-4457-950a-406ec97800bc)
- IOC 容器则像是对象与对象之间的“第三方”，齿轮之间的传动全部依靠“第三方”（ IOC ）使得 ABCD 这4个对象没有了耦合关系
- IOC 容器成了整个系统的关键核心，起到一种类似于“粘合剂”的作用，将系统中的所有对象粘合一起发挥作用。如果没有 IOC ，对象与对象之间会彼此失去联系
  
### 2. IoC 的解耦过程：
![761713439107_ pic](https://github.com/vlvvh/C-sharp-learn/assets/160467935/2f2cd96f-28ec-43cf-a07c-248053bc5fd0)

### 3.如果把中间的IoC拿掉，那么就会如下图：
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/6622f607-9b05-4c7c-a316-5d074a6498b2)
- 可以看到，拿掉 IoC 容器后，各个对象（ABCD）之间基本上没有耦合的。这样的话，当你想去实现 A 时，就不用去考虑 BCD 

## 二、IoC 容器有个别名，叫依赖注入（ DI ）
- 由IoC 容器在运行期间，动态的将某种依赖关系注入到对象之中
- 依赖注入（ DI ）和控制反转（ IoC ）是从不同角度描述同一件事情
- ✅通过引入 IOC容器 利用依赖关系注入的方式，实现对象之间的解耦

### ❓为什么代码中用 new 创建对象会增加代码的耦合度❓
⭕️ 在传统的视线中，由程序内部代码来控制组件之间的关系。我们经常使用 new关键字 来实现两个组件之间关系的组合。这种实现方式会造成组件之间的耦合。

### ❓那如果不用 new 创建对象，怎么创建呢❓
⭕️ 当然是！用注入的方式
### 1️⃣ 方法注入 （ Method Injection ）：通过类的方法来注入依赖对象。
如我们定义了以下的代码，Cow 和 Rabbit 类都实现了 Animal 的接口
~~~
Interface Animal
{
    void eat();}

Class cow implements Animal{
    public void eat(){System.out.println("牛吃草");}

Class rabbit implements Animal{
    public void eat(){System.out.println("兔子吃草");}
~~~
然后使用时，只需要如下：
~~~
Class Myclass
{
   private Animal animal;
   public void test(){animal.eat();}
   public void set Animal(Animal a){animal=a;}
}
~~~
维护一个Animal类型的引用即可，使用 setAnimal（）来对 animal 进行对象的注入。Myclass 的代码逻辑就跟 Cow 和 Rabbit 的代码逻辑分开，耦合程度降低。
- 通常，在类的方法中会接收一个依赖对象作为参数。
  - 因为不管 setAnimal() 中传入的是 Cow 还是 Rabbit 都可以用 Animal来表示。


### 2️⃣ Setter 方法注入（ Setter Injection ）:通过 公开的属性 或 setter方法 注入到对象当中
~~~
public class MyClass
{
    private Animal _animal;                 // _animal 是私有字段，用于储存Animal对象

    public Animal MyAnimal { get; set; }    // 注意属性名称应该与私有字段的名称保持一致
}
~~~
属性"MyAnimal"具有公共的 getter 和 setter 方法，允许在类外部设置和获取 "Animal" 对象
- Setter 方法注入 允许在对象创建后动态地注入依赖关系，灵活性较高


### 3️⃣ 构造函数注入（ Constructor Injection ）:通过 对象的构造函数 来注入依赖
~~~
public class Myclass      
{
   private Animal _animal;            
   public Myclass(Animal animal)      // 构造函数接受 Animal 类型的参数，并将其存储在私有字段 _animal 中
   {
       this._animal=animal;           // 将参数 animal 的值赋给私有字段 _animal
   }
}
~~~
构造函数用于初始化 Myclass 类的实例，并且在初始化时注入了 Animal 对象。
- 构造函数注入可以确保对象在创建时就具备了所需的依赖关系，使得对象的状态是不可变的🙅


### 4️⃣ 接口注入（ Interface Injection ）:通过 对象实现一个 特定的接口 来注入依赖
~~~
public class Myclass
{
    private Animal _animal;         // 构造函数接受 Animal 类型的参数，并将其存储在私有字段 _animal 中
    public Myclass(Animal animal)   
    {
        _animal = animal;
    }
     public MyClass(Animal animal) {
        _animal = animal;
    }
}
~~~
- 较少被使用，通常情况下步入构造函数注入和Setter方法注入灵活


## 三、IOC 的实现原理
✅我们可以把IOC容器的工作模式，看作是工厂模式的升华，可以把 IOC 容器看作是一个工厂，这个工程是要生产的对象都在配置文件中给出定义，然后用编程语言的反射机制，根据配置文件中给出的类生成相应对象。从实现来看，IOC是把工厂和对象分隔开来。

‼️ IOC的目的：提高灵活性和可维护性。   

‼️ IOC的缺点：由于IOC容器生成对象是通过反射方式，在运行效率上有一定的损耗。
