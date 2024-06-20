# Middleware 中间件
由于方法和体系的不成熟，以及企业业务和市场需求的不断变化，一个企业可能同时运行着多个不同的业务系统，这些系统可能基于不同的操作系统、不同的数据库、异构的网络环境。
现在的问题是，如何把这些信息系统结合成一个有机地协同工作的整体，真正实现企业跨平台、分布式应用。Middleware 中间件 用自己的复杂换取了企业应用的简单。


中间件（Middleware）是处于操作系统和应用程序之间的软件，也有人认为它应该属于操作系统中的一部分。
人们在使用中间件时，往往是一组中间件集成在一起，构成一个平台（包括开发平台和运行平台），但在这组中间件中必须要有一个通信中间件，即中间件=平台+通信，这个定义也限定了只有用于分布式系统中才能称为中间件，同时还可以把它与支持软件和实用软件区分开来。

## 简介
### 为什么要使用中间件❓
- 中间件屏蔽了底层操作系统的复杂性，使程序开发人员面对一个简单而统一的开发环境，减少程序设计的复杂性，不必再为程序在不同系统软件上的移植而重复工作，从而大大减少了技术上的负担。
- 中间件带给应用系统的，不只是开发的简便、开发周期的缩短，也减少了系统的维护、运行和管理的工作量，还减少了计算机总体费用的投入。

### 中间件定义
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/a754fa18-7c7b-4cea-9aab-a86d7ea13508)

中间件是位于平台(硬件和操作系统)和应用之间的通用服务，如图所示，这些服务具有标准的程序接口和协议。针对不同的操作系统和硬件平台，它们可以有符合接口和协议规范的多种实现。    

也许很难给中间件一个严格的定义，但中间件应该具有以下特点：
- 满足大量应用的需要
- 运行于多种硬件和OS平台
- 支持分别计算，提供**跨网络、硬件和OS平台的透明性的**应用或服务的交互
- 支持标准的协议
- 支持标准的接口

### 基本概念
1. Request 和 Response : Middleware 可以访问和修改 Http 的请求和响应。
2. Next Delegate : 每个 Middleware 组件都调用下一个 Middleware 组件。最终，调用会进入请求处理管道的终点，如控制器或 Razor 页面。
3. Middleware Order : Middleware 的执行顺序很重要，因为他们按照注册的顺序依次执行。

## 使用 Middleware 的示例 
1. 创建自定义 Middleware：
   通过创建一个类并实现 Invoke 或 InvokeAsync 方法来创建自定义 Middleware。
   ~~~
   public class CustomMiddleware
   {
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        // 处理请求前的逻辑
        Console.WriteLine("Handling request: " + context.Request.Path);

        // 调用下一个 Middleware
        await _next(context);

        // 处理请求后的逻辑
        Console.WriteLine("Finished handling request.");
    }
   }
   ~~~
2. 注册 Middleware：
   在 Startup.cs 中注册自定义 Middleware。
 ~~~
   public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    
    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseRouting();

    // 注册自定义 Middleware
    app.UseMiddleware<CustomMiddleware>();

    app.UseAuthentication();
    app.UseAuthorization();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}
 ~~~
3. 使用内置 Middleware：
   
   ASP.NET Core 提供了许多内置的 Middleware，以下是一些常用的：
   - Static Files Middleware：用于提供静态文件。
     ~~~
     app.UseStaticFiles();
     ~~~
   - Routing Middleware：用于路由请求。
     ~~~
     app.UseRouting();
     ~~~
   - Authentication Middleware：用于处理身份验证。
     ~~~
     app.UseAuthentication();
     ~~~
   - Authorization Middleware：用于处理授权。
     ~~~
     app.UseAuthorization();
     ~~~
     
4. 🌰 : Logging Middleware
以下是一个简单的日志记录 Middleware 示例，用于记录每个请求的 URL 和处理时间。
~~~
   public class LoggingMiddleware
{
    private readonly RequestDelegate _next;
    private readonly ILogger<LoggingMiddleware> _logger;

    public LoggingMiddleware(RequestDelegate next, ILogger<LoggingMiddleware> logger)
    {
        _next = next;
        _logger = logger;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        _logger.LogInformation($"Request URL: {context.Request.Path}");

        var startTime = DateTime.UtcNow;
        await _next(context);
        var duration = DateTime.UtcNow - startTime;

        _logger.LogInformation($"Request handled in {duration.TotalMilliseconds} ms");
    }
}
 ~~~
在 Startup.cs 中注册 LoggingMiddleware：
~~~
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    
    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseRouting();

    // 注册日志记录 Middleware
    app.UseMiddleware<LoggingMiddleware>();

    app.UseAuthentication();
    app.UseAuthorization();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}
~~~
Middleware 是 ASP.NET Core 应用程序处理 HTTP 请求管道的重要部分，通过使用自定义和内置的 Middleware，可以实现很多功能和逻辑处理。
