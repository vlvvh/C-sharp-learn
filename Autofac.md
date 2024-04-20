# Autofac
## 一、Autofac 介绍
Autofac 是 .Net 里 IOC(Inversion of Control,控制反转)容器中的一种，同类的框架还有 Unity、Castle 等。可以通过 NuGet 方式添加到项目中使用。   

Autofac 官网：https://autofac.org/

PS：控制反转（ IoC ） 和 依赖注入（ DI ）前往IoC笔记 进行学习 https://github.com/vlvvh/C-sharp-learn/blob/main/IoC.md

## 二、Autofac 服务的生命周期   
Autofac 支持⬆️ 7种生命周期，分别为：

### 1、Instance Per Dependency 瞬时   
- 每次都会返回一个新的实例，并且这是 默认的生命周期
~~~
{
  //瞬时生命周期-每一期获取的对象都是一个全新的实例
  ContainerBuilder builder=new ContainerBuilder();                       // 创建 Autofac 容器构建器
  builder.RegisterType<TestServuceA>().As<ITestServiceA>().InstancePerDependency();     // 注册 TestServiceA 类型为 ITestServiceA 接口的实现，并指定瞬时生命周期
  IContainer con=builder.Build();                                       // 构建容器
  ITestServiceA test1=con.Resolve<ITestServiceA>();                     // 解析 ITestServiceA 接口，获取实例
} 
~~~

### 2、Single Instance 单例
- 所有服务请求都将会返回同一个实例
~~~
builder.RegisterType<TestServuceA>().As<ITestServiceA>().SingleInstance();
~~~

### 3、Instance Per LifetimeScope 作用域
- 在一个嵌套语句块中，只会返回一个实例
~~~
builder.RegisterType<TestServuceA>().As<ITestServiceA>().InstancePerLifetimeScope();
~~~

### 4、Instance Per Matching LifetimeScope 匹配作用域
- 创建一个新的实例，并且该实例的生命周期与当前的匹配生命周期范围相匹配。
~~~
builder.RegisterType<TestService>().As<ITestService>().InstancePerMatchingLifetimeScope();
~~~

### 5、Instance Per Request 请求范围内创建一个实例
- 主要用于 Web 应用程序中，每个 HTTP 请求会创建一个对象实例，并在该请求的处理过程中重复使用该实例。
~~~
builder.RegisterType<TestService>().As<ITestService>().InstancePerRequest();
~~~

### 6、Instance Per Owned 所有者生命周期
- 将依赖注入限定到对应泛型实例
~~~
builder.RegisterType<TestService>().As<ITestService>().InstancePerOwned();
~~~

### 7、Thread Scope 线程（不推荐） 
- 每个线程一个实例，对于多线程场景，必须非常小心，不要在派生线程下释放父作用域
~~~
builder.RegisterType<TestService>().As<ITestService>().ThreadScope();
~~~

## 三、ContainerBuilder
ContainerBuilder 是 Autofac 中的一个重要类，用于构建和配置依赖注入容器。
- 1️⃣ 注册组件：
  - 使用 ContainerBuilder 可以注册应用程序中的组件，这些组件可以是类、接口、委托、Lambda 表达式等。注册组件是通过 RegisterType<T>()、RegisterInstance()、Register() 等方法完成的
- 2️⃣ 指定生命周期：
  - 在注册组件时，可以指定它们的生命周期。
- 3️⃣ 构建容器：
  - 使用 Build() 方法构建 ContainerBuilder，将其转换为 IContainer 实例，这样就可以使用容器来管理组件的生命周期和解析依赖项。
- 4️⃣ 解析依赖：
  - 构建容器后，可以使用 Resolve<T>() 或 Resolve() 方法来解析依赖项，获取注册的组件实例。
~~~
using Autofac;

class Program
{
    static void Main(string[] args)
    {
        // 创建一个 ContainerBuilder 对象
        var builder = new ContainerBuilder();

        // 注册组件
        builder.RegisterType<MyService>().As<IMyService>().InstancePerDependency();
        builder.RegisterType<Logger>().As<ILogger>().SingleInstance();

        // 构建容器
        var container = builder.Build();

        // 解析依赖
        var service = container.Resolve<IMyService>();
        service.DoSomething();
    }
}

// 示例接口和类
public interface IMyService
{
    void DoSomething();
}

public class MyService : IMyService
{
    private readonly ILogger _logger;

    public MyService(ILogger logger)
    {
        _logger = logger;
    }

    public void DoSomething()
    {
        _logger.Log("Doing something...");
    }
}

public interface ILogger
{
    void Log(string message);
}

public class Logger : ILogger
{
    public void Log(string message)
    {
        Console.WriteLine(message);
    }
}
~~~
- 在这个示例中，我们使用 ContainerBuilder 注册了 MyService 和 Logger 类，并指定了它们的生命周期。
- 构建了容器并使用 Resolve<IMyService>() 方法解析了 IMyService 接口，从而获取了 MyService 类的实例。
