# Queue 队列
## 一、定义
队列（queue）是一种特殊的线性表，特殊之处在于它只允许在表的前端（front）进行删除操作，而在表的后端（rear）进行插入操作，和栈一样，队列是一种操作受限制的线性表。进行插入操作的端称为队尾，进行删除操作的端称为队头。队列中没有元素时，称为空队列。
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/7dcce5f6-1761-4c1c-af0c-2eda9676e72d)
### ⚠️ 先进先出原则:
- 队列的数据元素又称为队列元素。在队列中插入一个队列元素称为入队，从队列中删除一个队列元素称为出队。因为队列只允许在一端插入，在另一端删除，所以只有最早进入队列的元素才能最先从队列中删除，故队列又称为先进先出线性表。
  - 🌰 比如我们去电影院排队买票，第一个进入排队序列的都是第一个买到票离开队列的人，而最后进入排队序列排队的都是最后买到票的。

## 二、示例
这段代码展示了如何在 C# 中使用非泛型的 Queue 类来创建和初始化一个队列，并显示其属性和值。
~~~
using System;
using System.Collections;

public class SamplesQueue
{
    public static void Main()
    {
        // 创建一个新的 Queue 实例 myQ。
        Queue myQ = new Queue();
        myQ.Enqueue("Hello");
        myQ.Enqueue("World");
        myQ.Enqueue("!");

        // 显示队列的属性和值
        Console.WriteLine("myQ");
        Console.WriteLine("\tCount:    {0}", myQ.Count); 
        Console.Write("\tValues:");
        PrintValues(myQ);
    }

    public static void PrintValues(IEnumerable myCollection)
    {
        foreach (Object obj in myCollection)  //该方法接收一个 IEnumerable 参数，遍历集合中的每个对象并打印其值。
            Console.Write("    {0}", obj);
        Console.WriteLine();
    }
}
~~~
当你运行这段代码，将会获得的结果是：
~~~
myQ
    Count:    3
    Values:    Hello    World    !
~~~
#### 关键点：
- Queue 类: Queue 是一个先进先出（FIFO）的集合，表示对象的一个集合，其中最早添加的对象最先被移除。
- Enqueue 方法: 将元素添加到队列的末尾。
- Count 属性: 获取队列中包含的元素数。
- PrintValues 方法: 用于遍历和打印集合中的所有元素。

### 属性  
- Count：获取 Queue 中包含的元素个数 ‼️
- IsSynchronized：获取一个值，该值指示是否同步对 Queue 的访问（线程安全）
- SyncRoot：获取可用于同步对 Queue 的访问的对象。

### 构造函数
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/d1984b89-236e-46f6-a477-91cfbdd6b984)

### 方法
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/a201ad4a-c831-4a59-83bb-b87f8e8b90ae)

⭕️ Queue类：https://learn.microsoft.com/zh-cn/dotnet/api/system.collections.queue?view=net-8.0
