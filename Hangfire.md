# Hangfire
Hangfire 是一个开源库，允许在 .NET 应用程序中简化任务调度和后台处理。它支持将任务添加到队列中并在后台异步执行，具有重试机制、延时任务、定时任务和持久化等功能。

## Hangfire 的主要特点
1. 后台处理：将任务放在后台队列中异步执行，而不阻塞主线程。
2. 任务调度：支持定时任务和延时任务。
3. 重试机制：自动重试失败的任务。
4. 持久化存储：任务和它们的状态存储在数据库中（例如 SQL Server、MySQL 等），确保任务不会因为应用程序重启或崩溃而丢失。
5. 仪表盘：提供一个 Web 界面来查看和管理任务。

## 基本使用步骤
### 1.配置 Hangfire
Hangfire 主要需要下载以下包文件
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/76982fc0-23e8-4b3b-99e7-d187d58fbc33)

### 2.在 Startup.cs 或者 Program.cs 文件中配置 Hangfire。
~~~
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        // 添加 Hangfire 服务，使用内存存储（仅用于开发环境，生产环境请使用持久化存储如 SQL Server）
        services.AddHangfire(config => config.UseMemoryStorage());
        
        // 添加 Hangfire 服务器
        services.AddHangfireServer();
        
        // 其他服务注册
    }

    public void Configure(IApplicationBuilder app, IBackgroundJobClient backgroundJobs, IRecurringJobManager recurringJobs)
    {
        // 使用 Hangfire 仪表盘
        app.UseHangfireDashboard();
        
        // 配置定时任务
        recurringJobs.AddOrUpdate(
            "my-recurring-job",
            () => Console.WriteLine("Recurring job executed!"),
            Cron.Daily);
    }
}
~~~

## Hangfire 的方法基本调用
1. Enqueue
- 入队，用于创建一个立即执行的后台任务。
~~~
IBackgroundJobClient.Enqueue(() => Console.WriteLine("Hello, Hangfire!"));
~~~

2. Schedule
- 用于创建一个延时执行的后台任务。
~~~
IBackgroundJobClient.Schedule(() => Console.WriteLine("This task will be executed after a delay"), TimeSpan.FromMinutes(5));
~~~

3. AddOrUpdate
- 用于创建或更新一个定时任务。
~~~
IRecurringJobManager.AddOrUpdate(
    "my-recurring-job",
    () => Console.WriteLine("Recurring job executed!"),
    Cron.Daily);
~~~

4. ContinueWith
- 用于创建一个后续任务，该任务在前一个任务成功完成后执行。
~~~
var jobId = IBackgroundJobClient.Enqueue(() => Console.WriteLine("Initial job"));
IBackgroundJobClient.ContinueWith(jobId, () => Console.WriteLine("Continuation job"));
~~~

5. Delete
- 用于删除一个已排队但尚未执行的后台任务。
~~~
IBackgroundJobClient.Delete(jobId);
~~~

6. Requeue
- 用于重新排队一个已失败的任务。
~~~
IBackgroundJobClient.Requeue(jobId);
~~~

7. Trigger
- 用于立即触发一个定时任务，而不等待其下一次计划执行时间。
~~~
IRecurringJobManager.Trigger("my-recurring-job");
~~~

8. RemoveIfExists
- 用于删除一个定时任务。
~~~
IRecurringJobManager.RemoveIfExists("my-recurring-job");
~~~

## Hangfire 的定时表达式（ Cron 表达式 ）
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/53712126-43e9-41b3-9bde-c686cba330cf)

- Hangfire 使用 Cron 表达式来定义定时任务的执行时间。以下是一些常见的 Cron 表达式：
  - Cron.Minutely - 每分钟执行一次
  - Cron.Hourly - 每小时执行一次
  - Cron.Daily - 每天执行一次
  - Cron.Weekly - 每周执行一次
  - Cron.Monthly - 每月执行一次
  - Cron.Yearly - 每年执行一次

你也可以使用自定义的 Cron 表达式。例如，每天早上 6 点执行：
~~~
IRecurringJobManager.AddOrUpdate(
    "daily-6am-job",
    () => Console.WriteLine("This job runs every day at 6 AM"),
    "0 6 * * *");
~~~


ps：

https://www.hangfire.io/overview.html hangfire官网

https://bradymholt.github.io/cron-expression-descriptor/?locale=zh-CN&expression=0+*%2F2+*+*+*  查询Corn翻译

https://en.wikipedia.org/wiki/Cron#Non-standard_characters Cron的百科网页
