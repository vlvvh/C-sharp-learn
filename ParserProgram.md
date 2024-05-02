# 解读 PractiseForSerena 考核项目———Program.cs/Startup.cs 文件
Program.cs 文件通常是 .NET Core 项目的入口点文件，它包含了程序的入口方法 Main    
Startup.cs 文件在 ASP.NET Core 项目负责配置应用程序的行为和特性，并定义了应用程序的启动流程。

文件地址：
- https://gitlab.sjfood.us/solar/practiceforserena/-/blob/master/src/PracticeForSerena.Api/Program.cs
- https://gitlab.sjfood.us/solar/practiceforserena/-/blob/master/src/PracticeForSerena.Api/Startup.cs  

## 一、解读 Program 代码
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

## 二、解读 Startup 代码
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/b6f0c59c-7dd0-436d-9a17-bf170cf634b5)

### 1.向服务集合中添加 MVC 和控制器服务 （ ConfigureServices方法 ）
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/815f9c0c-1377-45ac-926a-45e5517d1395)
- ‼️ ConfigureServices 方法用于 配置应用程序的服务 ，通常用于注册依赖注入服务和中间件
- 1️⃣ services.AddMvc();  这行代码向服务集合中添加 MVC 服务
  - MVC 是 ASP.NET Core 中常用的模式，用于构建 Web 应用程序
  - 通过添加 MVC 服务，可以在应用程序中使用 MVC 模式来组织和处理请求
- 2️⃣ services.AddControllers();  这行代码向服务集合中添加控制器服务
  - 在 ASP.NET Core 中，控制器是用于处理 HTTP 请求的类，通常用于执行特定的操作并生成响应
  - 通过添加控制器服务，你可以在应用程序中定义和使用控制器来处理请求

### 2.配置依赖注入容器 （ ConfigureContainer方法 ）
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/cff81cac-1a5d-4ac9-bdcc-170a50e7a039)
- ‼️ ConfigureContainer 方法 用于 配置依赖注入容器
- builder.RegisterModule(new PractiseForSerenaModule()) 方法 向容器中注册了一个自定义的 Autofac 模块 PractiseForSerenaModule，用于 注册应用程序中的服务

### 3.配置应用程序的请求处理管道 （ Configure方法 ）
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/66c87aeb-918c-4ea2-897a-46d00b3ad74c)
- ‼️ Configure 方法 用于 配置应用程序的请求处理管道
- if(env.IsDevelopment()) 判断检查应用程序 是否处于 开发环境
  - 如果是，则使用 app.UseDeveloperExceptionPage() 方法 添加开发者 异常页面中间件
  - 然后使用 app.UseRouting() 方法启用路由
  - 最后使用 app.UseEndpoints() 方法配置了一个默认的 路由规则，将请求传递给控制器处理

## 三、知识点解读📒
### 1. ConfigureServices（） 方法
用于配置应用程序的服务集合，也就是依赖注入容器
- 注册框架提供的服务，如：MVC、身份验证、授权等。

### 2. services.AddMvc（） 方法
是一种常用的软件架构模式，用于构建 Web 应用程序。
- MVC 提供了处理 HTTP 请求的基础设施，包括路由、控制器、动作方法等。

### 3. services.AddControllers（） 方法
用于注册控制器服务的方法。
- 控制器是用于处理 HTTP 请求的类，通常用于执行特定的操作并生成响应。
- 可以在应用程序中定义和使用控制器来处理请求。

### 4.ConfigureContainer（） 方法
用于配置依赖注入容器，允许你在应用程序启动时注册或配置依赖项。
- 如果🙅不使用 第三方的 依赖注入容器（如 Autofac、Ninject 等），通常不需要实现 ConfigureContainer 方法。
- 如果选择 使用第三方的 依赖注入容器，并且想要在启动时对容器进行特定的配置，可以实现 ConfigureContainer 方法，并在其中对容器进行注册或配置。

### 5.RegisterModule() 方法
是 Autofac 中用于注册模块的方法
- 选择使用 Autofac 作为依赖注入容器，并且想要在应用程序启动时注册一组相关的依赖项，通常会创建一个继承自 Autofac 的 Module 类，并在其中实现依赖项的注册逻辑。
- 可以通过调用 builder.RegisterModule() 方法来注册这个模块。
  - 1️⃣ 注册模块
  - 2️⃣ 执行模块注册逻辑
  - 3️⃣ 管理模块的生命周期

### 6.Configure（） 方法
用于配置应用程序的请求处理管道，即定义请求的处理流程。
- 通常执行以下操作：
  - 1️⃣ 环境检查：通常会使用 IWebHostEnvironment 接口提供的 IsDevelopment()、IsProduction() 等方法来检查环境。
  - 2️⃣ 异常处理：使用 app.UseDeveloperExceptionPage() 方法添加开发者异常页面中间件，用于显示详细的异常信息。
    - 生产环境下，可以使用 app.UseExceptionHandler() 方法添加异常处理中间件，用于统一处理异常并返回友好的错误页面或信息。
  - 3️⃣ 路由配置： 使用 app.UseRouting() 方法启用路由。
  - 4️⃣ 端点路由配置： 使用 app.UseEndpoints() 方法配置端点路由。:small_orange_diamond: 通常会调用 endpoints.MapControllers() 方法配置默认的路由规则

### 6.IsDevelopment（） 方法
用于检查当前的应用程序是否处于开发环境。
- IsDevelopment() 方法会检查应用程序的环境变量（通常是 ASPNETCORE_ENVIRONMENT）是否设置为 "Development"。
- 如果环境变量的值是 "Development"，则返回 true，表示当前的应用程序处于开发环境；否则返回 false，表示当前的应用程序不处于开发环境。

### 7.UseDeveloperExceptionPage() 方法
用于在开发环境下显示详细的异常信息页面。 
- UseDeveloperExceptionPage() 方法会添加一个开发者异常页面中间件到应用程序的请求处理管道中。
- 应用程序在开发环境下出现未处理的异常时，这个中间件会捕获异常并显示一个包含详细异常信息的页面，包括异常堆栈跟踪、异常类型、异常消息等。

### 8.UseRouting()  方法
用于启用请求路由功能。
- UseRouting() 方法会添加一个路由中间件到应用程序的请求处理管道中，路由负责根据请求的 URL 路径将请求路由到相应的处理程序（通常是控制器的动作方法）。
- 使用路由中间件，你可以配置应用程序的路由规则，定义 URL 路径与处理程序之间的映射关系。

### 9.UseEndpoints（） 方法
用于配置端点路由。
- 可以通过调用 endpoints.MapXxx() 方法来配置不同类型的端点路由。
- 常见的 MapControllers() 方法用于配置控制器路由，将请求路由到控制器的动作方法。

