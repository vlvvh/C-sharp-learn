# Async 和 Await 
## 简介
async 和 await 是 C# 语言中的关键字，用于简化异步编程。     
异步编程允许程序在等待耗时操作（如文件读写、网络请求、数据库查询等）时继续处理其他任务，从而提高应用程序的响应性。
- 概念：
  - 任务（Tasks）：表示一个异步操作，可以有返回值（Task<T>）或者没有返回值（Task）。
  - 等待（Awaiting）：使用 await 关键字等待一个异步任务完成，任务完成之前，控制权会返回给调用者，避免阻塞当前线程。
~~~
public async Task<int> GetDataAsync()
{
    await Task.Delay(1000); // 模拟异步操作
    return 42;
}
~~~
~~~
public async Task UseDataAsync()
{
    int data = await GetDataAsync();
    Console.WriteLine(data); // 使用异步方法获取数据
}
~~~
~~~
public static async Task Main(string[] args)
{
    await UseDataAsync();
} // 主程序入口
~~~
在上述示例中:       

GetDataAsync 方法被标记为 async，并返回一个 Task<int>。await Task.Delay(1000) 模拟一个耗时操作。

UseDataAsync 方法调用 GetDataAsync 并使用 await 等待其完成，获取结果.

Main 方法也是异步的，使用 await UseDataAsync 等待 UseDataAsync 方法完成。

## 等待多个任务完成
Task包含两种方法，Task.WhenAll和Task.WhenAny，允许编写异步代码，以非阻塞方式等待多个后台作业。
~~~
private static async Task<User[]> GetUsersAsyncByLINQ(IEnumerable<int> userIds)
{
    var getUserTasks = userIds.Select(id => GetUserAsync(id)).ToArray();
    return await Task.WhenAll(getUserTasks);
}
~~~
- 由于 LINQ 使用延迟执行，因此异步调用不会像在循环中那样立即发生，foreach除非强制生成的序列通过调用.ToList()或进行迭代.ToArray()。
- 上面的示例使用Enumerable.ToArray立即执行查询并将结果存储在数组中。
- 这会强制代码id => GetUserAsync(id)运行并启动任务。

## ⚠️注意事项
- 使用async 必须要在主体中要用到await，否则会有警告❕
- 添加“Async”作为每个异步方法名称的后缀。
- async void 仅应用于事件处理程序。
  - async void是允许异步事件处理程序工作的唯一方法，因为事件没有返回类型（所以不能使用Task和Task<T>）。
- 在 LINQ 表达式中使用异步 lambda 时要谨慎（很容易导致死锁，所以需要在异步方法中使用 ConfigureAwait(false)）
~~~
public async Task UseDataSafelyAsync()
{
    int data = await GetDataWithoutContextAsync().ConfigureAwait(false);
    Console.WriteLine(data);
}
~~~
![image](https://github.com/user-attachments/assets/469d9c59-69bf-4189-b3af-ee66235836b0)
- 取消操作：通过 CancellationToken 支持异步操作的取消。语法：await TaskName(cancellationToken);
~~~
public async Task<int> GetDataWithCancellationAsync(CancellationToken cancellationToken)
{
    await Task.Delay(1000, cancellationToken);
    return 42;
}
~~~


***
## ❓问：async 和 await 是单线程还是多线程的呢❓
async 和 await 本身并不直接控制线程，而是用于异步编程模型。 

作用是简化异步操作的编写和读取，使得异步代码看起来更像同步代码。

### 1️⃣单线程：
async 和 await 并不直接创建新线程。它们允许异步方法在不阻塞调用线程的情况下执行长时间运行的操作。当遇到 await 时，控制权会立即返回给调用方，原来的线程可以继续执行其他任务。
~~~
public async Task SomeMethodAsync()
{
    // 在 UI 线程或主线程上运行
    await Task.Delay(1000); // 异步等待 1 秒，不会阻塞当前线程

    // 这里的代码将在 UI 线程或主线程上继续运行
}
~~~
### 2️⃣多线程：
尽管 async 和 await 本身不创建新线程，但异步方法可以运行在后台线程上。例如，I/O 操作（如文件读写、网络请求）通常会在系统的 I/O 线程池中进行，这些操作在完成后会通过 await 将结果返回给主线程。
~~~
public class Program
{
    public static async Task Main(string[] args)
    {
        // 启动异步任务
        int result = await PerformCpuBoundOperationAsync();

        // 输出结果
        Console.WriteLine($"Result: {result}");
    }

    // 异步方法，用于执行 CPU 绑定任务
    private static async Task<int> PerformCpuBoundOperationAsync()
    {
        Console.WriteLine("Starting CPU-bound operation...");

        // 将 CPU 绑定任务移到后台线程
        int result = await Task.Run(() =>
        {
            // 模拟一个耗时的 CPU 绑定任务
            int sum = 0;
            for (int i = 0; i < 1000000000; i++)
            {
                sum += i;
            }
            return sum;
        });

        Console.WriteLine("Completed CPU-bound operation.");

        // 返回结果
        return result;
    }
}
~~~
使用 async 和 await 结合 Task.Run 来实现多线程操作，使得长时间运行的 CPU 绑定任务在后台线程上执行，从而避免阻塞主线程。
