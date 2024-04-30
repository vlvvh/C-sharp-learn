# 中介者模式 （ Mediator pattern ）
中介者模式在现实生活中有很多例子，比如：A 和 B 做生意，A 或 B 的想法会经常变化，每次改变时告诉对方，会使对方很反感。如果在 A 和 B 之间增加一个 C 对象，在最终确定之前不要告诉 C 对象，对方就不知道了（隔离了耦合，对方可以更具需求变化），
等一方最终确定想法后，把最后决定告诉 C 对象，C 再转告给对方。这样就简化了 A 和 B 的交易过程，而且双方都很满意。       

在软件构建过程中，因为有了变化，才有增加中介者的需要 ‼️       
变化是模式的前提，无论是什么模式，就因为有变化，我们需要抵御变化，才要使用相应的模式解决问题。   

## 一、模式的详细介绍      
### 1.1 动机
在软件构建过程中，经常会出现多个对象互相关联交互的情况，对象之间常常会维持一种复杂的引用关系，如果遇到一些需求的更改，这种直接的引用关系将面临不断变化。          
在这种情况下，我们可以**使用一个“中介对象”来管理对象间的关联关系，避免相互交互的对象之间的紧耦合引用关系，从而更好的抵御变化。**

### 1.2 意图
定义了一个中介对象来封装一系列对象之间的交互关系。中介者使各个对象之间不需要显式地相互引用，从而使耦合性降低，而且可以独立地改变他们之间的交互行为。   

### 1.3 结构图
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/5c3e6f56-b7da-4985-935e-086dce492790)

### 1.4 模式的组成
( 1 ) 抽象中介者角色 ( Mediator ):在这里定义了各个对象之间交互需要的方法。可以是公众的通信方法，也可以是小范围的交互方法。 

( 2 ) 具体中介者角色 ( ConcreteMediator ):实现了中介者接口，它需要了解并维护各个同事对象，并负责具体的协调各同事对象之间的交互关系。

( 3 ) 抽象同事类( Colleague ): 定义了一个接口或抽象类，通常为抽象类，主要约束同事对象的类型，并实现一些具体同事类之间的公共功能   

      比如:每个具体同事类都应该知道中介者对象，也就是具体同事类都会持有中介者对象，都可以到这个类里面。 

( 4 ) 具体同事类( ConcreteConlleague ):实现自己的业务，**需要与其他同事通信时候，就与持有的中介者通信，中介者会负责与其他同事类交互**

### 1.5 模式的具体实现
#### 举个例子🌰：
在公司管理过程中，就会涉及到各个部门之间的协调和合作，沟通协调就需要一个人来做，假设是总经理，把总经理定义为管理者，各个部门需要向他汇报和发起工作请求。代码如下：
~~~
using Microsoft.VisualBasic;

namespace 中介者模式的实现
{
    // 抽象中介者角色
    public interface IMediator
    {
        void Command(Department department);
    }
    
    // 具体中介者角色：总经理
    public sealed class President : IMediator
    {
        // 总经理有各部门的管理权限
        private Financial _financial;
        private Market _market;
        private Development _development;

        // 财务部
        public void SetFinancial(Financial financial)
        {
            this._financial = financial;
        }
        // 研发部
        public void SetDevelopment(Development development)
        {
            this._development = development;
        }

        // 市场部
        public void SetMarket(Market maket)
        {
            this._market = maket;
        }

        public void Command(Department department)
        {
            if (department.GetType() == typeof(Market))
            {
                _financial.Process();
            }
        }
    }
    // 抽象同事类 同事类的接口
    public abstract class Department
    {
        // 持有中介者（总经理）的引用
        private IMediator mediator;

        protected Department(IMediator mediator)
        {
            this.mediator = mediator;
        }

        public IMediator GetMediator
        {
            get { return mediator; }
            private set { this.mediator = value; }
        }
        // 做本部门的事
        public abstract void Process();
        // 向总经理发出申请
        public abstract void Apply();
    }
    
    //具体同事类1:研发部
    public sealed class Development:Department
    {
        public Development(IMediator m):base(m){ }

        public override void Process()
        {
            Console.WriteLine("研发部门向总经理汇报：我是研发部，需要资金支持");
        }

        public override void Apply()
        {
            Console.WriteLine("研发部门决定：专心科研，开发项目！");
        }
    }
    //具体同事类2:财务部
    public sealed class Financial : Department
    {
        public Financial(IMediator m):base(m){}

        public override void Process()
        {
            Console.WriteLine("财务部门向总经理汇报：钱太多了！怎么花？");
        }

        public override void Apply()
        {
            Console.WriteLine("财务部执行动作：数钱");
        }
    }
    // 具体同事类3:市场部
    public sealed class Market : Department
    {
        public Market(IMediator m):base(m){}

        public override void Process()
        {
            Console.WriteLine("市场部门向总经理汇报：项目承接，需要资金支持");
        }

        public override void Apply()
        {
            Console.WriteLine("市场部决定：接项目");
        }
    }

    class Program
    {
        static void Main(String[] args)
        {
            President mediator = new President();

            Market market = new Market(mediator);
            Development development = new Development(mediator);
            Financial financial = new Financial(mediator);
            
            mediator.SetMarket(market);
            mediator.SetDevelopment(development);
            mediator.SetFinancial(financial);
            
            market.Apply();
            development.Apply();
            financial.Apply();

            Console.Read();
        }
    }
    
~~~
运行结果：
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/cbfa465e-451c-4555-bd7f-cf0c2513d7fd)

## 二、模式实现的要点
将多个对象间复杂的关联关系解耦，Mediator模式将多个对象间的控制逻辑进行集中管理、变"多个对象互相关联”为"多个对象和一个中介者关联”，简化了系统的维护，抵御了可能的变化。随着控制逻辑的复杂化，Mediator具体对象的实现可能相当复杂。这时候可以对Mediator对象进行分解处理，
- Facade模式：是解耦系统外到系统内（单向）的相关联关系
- Mediator模式：是解耦系统内各个对象之间（双向）的关联关系

### 2.1 Mediator的优点✅
#### 1 ）松散耦合
中介者模式通过把多个同事对象之间的交互封装到中介对象里面，从而使得对象之间松散耦合，基本上可以做到互不依赖。
#### 2 ）集中控制交互
多个同事对象的交互，被封装在中介者对象里面集中管理，使得这些交互行为发生变化的时候，只需要修改中介者就可以了
#### 3 ）多对多变一对多
中介者和同事对象的关系通常变为双向的一对，让对象关系更容易理解


### 2.2 Mediator的缺点:negative_squared_cross_mark:
#### 1 ）过多的集中化
使中介者对象变得十分复杂，难于维护管理

⚠️如不使用Mediator会构成如下图的网状结构
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/c112f247-b7e4-4f10-9104-07fc01da2ed9)
使用后会如下图：
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/4e6414d9-e876-463e-96d4-118dc7b9c14a)

