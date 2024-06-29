# Middleware 中间件
Middleware 是 ASP.NET Core 应用程序中用于处理 HTTP 请求和响应的一系列组件。每个中间件组件在管道中依次执行，通常会对请求进行一些预处理，然后将请求传递给管道中的下一个中间件。响应从管道的末端返回时，每个中间件组件可以对响应进行一些后处理。

每个中间件在执行其逻辑后，可以选择调用下一个中间件，也可以直接生成响应并终止请求管道。
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/a61d3de6-9329-4b9c-83d6-dc7d33125cdc)


## 基本概念
1. 请求管道 ：Middleware 组成了一个请求处理管道。每个中间件组件决定是否将请求传递给下一个中间件。这个过程类似于一个洋葱，每个中间件包裹在另一个中间件外面，形成一个层次结构。
2. RequestDelegate ：每个中间件都接收一个 RequestDelegate 参数，这个参数表示管道中下一个中间件的委托。中间件可以选择调用它来将请求传递给下一个中间件。
3. Invoke 方法：中间件通过 Invoke 或 InvokeAsync 方法来实现其功能。这个方法接收一个 HttpContext 参数，用于访问 HTTP 请求和响应。
4. 配置中间件：中间件通过 IApplicationBuilder 接口在 Startup 类的 Configure 方法中进行配置。中间件的顺序非常重要，决定了它们在请求管道中的执行顺序。

## 使用 WebApplication 创建中间件管道
调用单个匿名函数以响应每个 HTTP 请求。
~~~
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.Run(async context =>
{
    await context.Response.WriteAsync("Hello world!");
});

app.Run();
~~~

用 Use 将多个请求委托链接在一起。 next 参数表示管道中的下一个委托。 可通过不调用 next 参数使管道短路。 通常可在 next 委托前后执行操作，如以下示例所示：
~~~
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.Use(async (context, next) =>
{
    // 可以写入响应的工作。
    await next.Invoke();
    // 进行日志记录或其他不写入响应的工作
});
~~~

Run 委托不会收到 next 参数。 第一个 Run 委托始终为终端，用于终止管道。 Run 是一种约定。 某些中间件组件可能会公开在管道末尾运行的 Run[Middleware] 方法：
~~~
app.Run(async context =>
{
    await context.Response.WriteAsync("Hello from 2nd delegate.");
});

app.Run();
~~~

## 中间件顺序
下图显示了 ASP.NET Core MVC 和 Razor Pages 应用的完整请求处理管道。

![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/814f28b4-6131-4092-8998-6736a3eff70a)


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

ps：Middleware中间件： https://learn.microsoft.com/zh-cn/aspnet/core/fundamentals/middleware/?view=aspnetcore-8.0
