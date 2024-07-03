# LINQ 标准查询运算符
 “标准查询运算符”是组成语言集成查询 (LINQ) 模式的方法。 大多数这些方法都在序列上运行。其中的序列其类型实现了 IEnumerable<T> 接口或 IQueryable<T> 接口。标准查询运算符提供了包括筛选、投影、聚合、排序等功能在内的查询功能。
 
 共有两组 LINQ 标准查询运算符：一组在 IEnumerable<T> 类型的对象上运行，另一组在 IQueryable<T> 类型的对象上运行。   
 
 ## IEnumerable<T> 与 IQueryable<T> 的区别
 
 ### IEnumerable<T>
 - 定义: IEnumerable<T> 是一个用于表示数据序列的接口，主要用于内存中的集合，如列表、数组等。
 - 延迟执行: IEnumerable<T> 运算符通常在调用时立即执行并返回结果。
 - 执行位置: 所有操作都在内存中进行。
 - 适用场景: 适用于对内存中集合进行操作，如 List<T>、Array 等。
   
🌰：假设你有一个内存中的列表，并希望对其进行过滤
~~~
List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };
IEnumerable<int> evenNumbers = numbers.Where(n => n % 2 == 0);

foreach (int number in evenNumbers)
{
    Console.WriteLine(number);
}

// 输出: 2, 4
~~~
在这个示例中，numbers 是一个 List<int>，它实现了 IEnumerable<int> 接口。Where 方法在内存中进行过滤操作

### IQueryable<T>
- 定义: IQueryable<T> 是一个继承自 IEnumerable<T> 的接口，用于表示可以进行查询的数据源，主要用于远程数据源，如数据库。
- 延迟执行: IQueryable<T> 运算符在查询被枚举（例如通过 ToList()、ToArray() 等方法）之前不会实际执行查询。
- 执行位置: 查询操作会被翻译成适当的查询语言（如 SQL）并在远程数据源上执行。
- 适用场景: 适用于数据库等远程数据源，通过 Entity Framework 或 LINQ to SQL 等 ORM 工具进行查询。

🌰：假设你有一个数据库上下文，并希望对数据库中的数据进行查询
~~~
using (var context = new MyDbContext())
{
    IQueryable<Customer> customers = context.Customers.Where(c => c.City == "London");

    foreach (var customer in customers)
    {
        Console.WriteLine(customer.Name);
    }
}
~~~
在这个示例中，context.Customers 是一个 DbSet<Customer> 数据库的表，它实现了 IQueryable<Customer> 接口。Where 方法不会立即执行，而是生成一个 SQL 查询，当 customers 被枚举时（例如在 foreach 循环中），查询会在数据库上执行并返回结果。

## 标准查询操作符表
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/e71d1f5b-e069-4ba1-abed-16a38d32cb8a)
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/4ce08dbb-b025-4fe7-b6f4-9b8645d3afe5)
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/73c1b887-f14d-4b56-88ee-00b016de78a8)
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/6a6111a8-309e-429a-bb30-4830b1ad1153)



