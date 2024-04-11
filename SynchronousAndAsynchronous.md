# 一、同步和异步的了解
## 同步 ( Synchronous )
- 按照顺序逐步执行，每一步操作都必须等待上一步操作完成后才能进行
- 当一个操作正在执行时，程序会被阻塞，直到这个操作完成
- 关键字 "lock" 用于创建同步块，确保在同一时刻只有一个线程可以访问共享资源
  
## 异步（ Asynchronous )
- 启用了新的线程，主线程和方法线程并行执行。
- 异步指的是程序不会被阻塞等待某个操作完成。相反，它会继续执行其他任务，并且在后台处理那些耗时的操作
- 当异步操作完成时，程序可以通过回调函数、事件或者其他机制来处理结果
- ⭕️关键字 "async" 将方法标记为异步，意味着可以在执行其他代码时在后台运行
- ⭕️关键字 "await"表示在等待异步的结果

# 二、简单示例
用点菜、炒菜和上菜的过程来说明同步和异步之间的区别
## 同步
假设你是一位厨师，餐厅是同步运行的。当服务员接收到顾客的点菜后，他会将菜单递给你。你会立即开始炒菜，并在炒菜完成后将菜上菜。然后服务员才能将菜端给顾客。在这个过程中，每个步骤都是顺序执行的，没有并发操作。   
> 1.顾客点了一份宫保鸡丁
> 
> 2.服务员将菜单递给你，你开始炒宫保鸡丁
> 
> 3.你炒完了宫保鸡丁，将其放在厨房的出菜台上
> 
> 4.服务员拿起宫保鸡丁并上菜给顾客

![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/9a3d4e3c-3224-401b-b814-4167314de3c3)


## 异步
现在假设你是一位厨师，餐厅是异步运行的。当服务员接收到顾客的点菜后，他会将菜单递给你。但与此同时，你可以在炒菜的同时接受其他菜品的订单，并开始炒其他菜品，而不必等待第一份菜炒好。一旦一份菜炒好了，你会将其放在出菜台上，服务员再将其上菜给顾客。   

![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/77d4c8a4-fd17-4a07-950a-b656c90d5c31)
> 1.顾客点了一份宫保鸡丁
> 
> 2.服务员将菜单递给你，你开始炒宫保鸡丁，同时可以接受其他菜品的订单并开始炒其他菜品
> 
> 3.你炒完了宫保鸡丁，将其放在厨房的出菜台上
> 
> 4.服务员拿起宫保鸡丁并上菜给顾客
>

## 代码示例
以下是一个简单的示例，同步和异步之间的区别，假设我们使用 HttpClient 进行网络请求
在这两个示例中，用了 HttpClient 发送了一个 GET 请求到 https://jsonplaceholder.typicode.com/posts/1，并打印了响应内容。    

- 同步示例：
~~~
using System;
using System.Net.Http;

class Program
{
    static void Main()
    {
        Console.WriteLine("Start");

        // 创建一个 HttpClient 实例
        HttpClient client = new HttpClient();

        // 发送同步 GET 请求
        HttpResponseMessage response = client.GetAsync("https://jsonplaceholder.typicode.com/posts/1").Result;

        // 获取响应内容并打印
        string content = response.Content.ReadAsStringAsync().Result;
        Console.WriteLine(content);

        Console.WriteLine("End");
    }
}
~~~
- 异步示例：
~~~
using System;
using System.Net.Http;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        Console.WriteLine("Start");

        // 创建一个 HttpClient 实例
        HttpClient client = new HttpClient();

        // 发送异步 GET 请求，使用 await 关键字等待任务的完成
        HttpResponseMessage response = await client.GetAsync("https://jsonplaceholder.typicode.com/posts/1");

        // 获取响应内容并打印
        string content = await response.Content.ReadAsStringAsync();
        Console.WriteLine(content);

        Console.WriteLine("End");
    }
}
~~~
### 示例🌰总结：
在同步示例中，我们使用了.Result 属性来阻塞主线程直到任务完成，而在异步示例中，我们使用了 await 关键字 来等待任务的完成，但是并不会阻塞主线程，允许其他任务继续执行。
