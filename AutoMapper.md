# AutoMapper 详解
## 一、概述
### 1.❓什么是AutoMapper❓
 AutoMapper 是一个用于 对象映射的 库 ，它可以帮助简化将一个对象的数据 映射到 另一个对象的过程。在实际开发中，经常会有需要 将数据库实体对象 映射到 DTO（或者反过来）的情况。
 AutoMapper 提供了一种便捷的方式来定义 映射规则，而不需要开发者手动编写大量的映射代码，从而提高了开发效率。

 ### 2.❓为什么要做对象之间的映射❓
 1️⃣ 解耦合：当不同层（例如数据访问层、业务逻辑层和表示层）之间需要交换数据时，使用映射可以帮助解耦合，即降低这些层之间的依赖性。    
 
 2️⃣ 隐藏细节：对象之间的结构可能因为不同的目的而不同。通过映射，可以隐藏这些细节，只暴露需要的信息，从而使代码更加清晰和易于理解。      
 
 3️⃣ 数据转换：不同的组件或系统之间可能使用不同的数据结构来表示相同的实体。通过映射，可以方便地进行数据转换，将一个数据结构转换为另一个数据结构，以满足特定的需求。      
 
 4️⃣ 提高性能：通过映射，可以将数据库实体对象 映射到 较小的数据传输对象（DTO），从而减少数据传输量，提高系统的性能。      


## 二、如何使用？
### 1.nuget 引入 AutoMapper 程序集
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/a879f64c-ca55-4ba2-8a88-0763c74e4732)

### 2.配置映射关系
~~~
public  class MyFirstProfile: Profile     // MyFirstProfile 类继承自 Profile 类，这是AutoMapper库中用于配置映射的基类。
{
    public MyFirstProfile()
    {
        // AutoMapper会自动将 Src01 对象的属性值复制到 Dest 对象的相应属性中，前提是这两个类型的属性名和类型相匹配。
        // 源类型是 Src01，目标类型是 Dest01
        CreateMap<Src01, Dest01>();    
    }
}
~~~

### 3.对象转换
~~~
// 创建配置对象
var configuration = new MapperConfiguration(cfg =>
{ 
    cfg.AddProfile<MyFirstProfile>();
});

// 创建映射对象
// 或者var mapper=new Mapper(configuration);
var mapper = configuration.CreateMapper();

// 映射动作，将对象 Src01 转换成另一个对象 Dest01
var dest = mapper.Map<Dest01>(new Src01() { Name = "zhangsan", Age = 18 });
~~~
