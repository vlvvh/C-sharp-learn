# Autofac
## 一、Autofac 介绍
Autofac 是 .Net li IOC(Inversion of Control,控制反转)容器中的一种，同类的框架还有 Unity、Castle 等。可以通过 NuGet 方式添加到项目中使用。   

Autofac 官网：https://autofac.org/

PS：控制反转（ IoC ） 和 依赖注入（ DI ）前往IoC笔记 进行学习

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
