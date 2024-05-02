# 解读 PractiseForSerena 考核项目———Program.cs 文件
Program.cs 文件通常是 .NET Core 项目的入口点文件，它包含了程序的入口方法 Main    
文件地址：https://gitlab.sjfood.us/solar/practiceforserena/-/blob/master/src/PracticeForSerena.Api/Program.cs   

## 一、代码解读
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/f38ecf20-bb67-4e91-bdf3-23ff16d4228a)
### 1.创建配置对象
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/95b66faf-5ffa-4607-80a3-08a9a62e3814)
- 使用 ConfigurationBuilder 创建了一个配置构建器对象
- 使用 AddJsonFile 方法添加了一个名为 appsettings.json 的 Json 格式 的配置文件
- 使用 AddEnvironmentVariables  方法添加了环境变量的配置
- 使用 Build 方法构建了配置对象

### 2.创建运行主机
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/6d92c78c-e3f3-4dee-ae41-4c11d7772261)
- 调用 CreateHostBuilder 静态方法，返回了一个 IHostBuilder 对象，用于配置应用程序的主机
- 调用 Build 方法构建了主机，再调用 Run 方法运行了主机。表示启动应用程序，开始监听传入的请求

### 3.创建默认主机构建器
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/20edb1a5-5cca-4465-a938-1e5df76b7e17)
- 这个方法创建了一个默认的主机构建器，并使用了默认的配置，包括加载应用程序配置、设置日志记录、配置应用程序的依赖注入容器等

### 4.配置了主机使用 Autofac 作为依赖注入容器
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/ee4babe9-d4ca-4d13-a2ba-6a4052fb3711)
- AutofacServiceProviderFactory 是 Autofac 提供的一个实现了 IServiceProviderFactory 接口的工厂类，用于创建 Autofac 的服务提供程序。

### 5.配置 Web 主机默认选项
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/6c3b13e5-1ee3-4437-8d5d-0151d0a33281)
- 使用了 webBuilder.UseStartup<Startup>() 方法来指定使用 Startup 类来配置 应用程序
- Startup 类负责配置应用程序的 服务、中间件和路由

