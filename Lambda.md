# Lambda 表达式
## 一、认识 Lambda 表达式
### 1. Lambda表达式本质上就是匿名函数
### 2. 通常用于函数式编程和 LINQ 查询,表达式一般如下：
~~~
(parameters) => expression
~~~
- 参数列表 (parameters)：圆括号括起来的一组参数，有一个或多个，如果没有参数，参数列表可以是空的圆括号 ()
- Lambda 操作符 => 用于分隔参数列表和表达式体，读作“goes to”
- 表达式 (expression)： 如果是单个表达式，则不需要花括号 {}；如果是多条语句，则需要用 {} 包裹，并且必须有一个返回值。
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/f0b63b06-ce1f-4158-b66c-a4f496aea3e6)


### 3. 任何 Lambda 表达式都可以转换为委托类型。 Lambda 表达式可以转换的委托类型由其参数和返回值的类型定义。
- 任何 Lambda 表达式都可以转换为委托类型
- 如果 Lambda 表达式不返回值，则可以将其转换为 Action 委托类型之一,Action 委托类型表示不返回值的方法
~~~
// 无参数的 Action 委托
Action greet = () => Console.WriteLine("Hello, world!");
greet(); // 输出 "Hello, world!"

// 单参数的 Action 委托
Action<int> printSquare = x => Console.WriteLine(x * x);
printSquare(5); // 输出 25

// 多参数的 Action 委托
Action<int, int> addAndPrint = (x, y) => Console.WriteLine(x + y);
addAndPrint(3, 4); // 输出 7
~~~
- 如果 Lambda 表达式返回值，则可以转换为 Func 委托类型之一。Func 委托类型表示有返回值的方法。
~~~
// 无参数的 Func 委托
Func<int> getRandomNumber = () => new Random().Next(1, 100);
Console.WriteLine(getRandomNumber()); // 输出随机数

// 单参数的 Func 委托
Func<int, int> square = x => x * x;
Console.WriteLine(square(5)); // 输出 25

// 多参数的 Func 委托
Func<int, int, int> add = (x, y) => x + y;
Console.WriteLine(add(3, 4)); // 输出 7
~~~

## 二、在 LINQ 中使用 Lambda
在LINQ查询中，Lambda表达式常用于定义查询中的行为，如Where、Select、OrderBy等操作。下面我们将通过一些示例来介绍Lambda表达式在LINQ中的应用。

### 1️⃣ 进行 Where 查询
假设有一个学生对象的列表，每个学生有Name和Age属性。
~~~
List<Student> students = new List<Student>
{
    new Student { Name = "张三", Age = 20 },
    new Student { Name = "李四", Age = 22 },
    // ... 其他学生
};

可以用where进行查询，筛选出年龄小于21岁的学生：
var youngStudents = students.Where( s => s.Age <21 );
~~~

### 2️⃣ 进行 Select 查询
可以获取每个学生的名字
~~~
var studentNames = students.Select(s => s.Name);
~~~

### 3️⃣ 排序：进行OrderBy和ThenBy
根据年龄和名字对 students 进行排序
~~~
var sortedStudents = students.OrderBy(s => s.Age).ThenBy(s => s.Name);
~~~

### 4️⃣ 分组：GroupBy
~~~
Console.WriteLine("按照班级进行分组");
var groupClass = students.GroupBy(stu => stu.Class).ToList();
~~~

### 5️⃣ First & Last
查找第一个或最后一个数据
~~~
Student student1 = students.First();
Student student2 = students.First(stu => stu.Class == "一班");
~~~

### 6️⃣ Any & All
数据是否满足条件
~~~
Console.WriteLine("是否存在三班");
bool IsClass = students.Any(stu => { return stu.Class == "三班"; });
Console.WriteLine("是否所有学生都大于19岁");
bool IsAll = students.All(stu => { return stu.Age > 19; }); 
~~~

### 7️⃣ Skip
根据条件忽略
~~~
Console.WriteLine("不保留前三个学生");
List<Student> students3 = students.Skip(3).ToList();
Console.WriteLine("忽略满足条件的数据直至不满足开始");
List<Student> students4 = students.SkipWhile(stu => { return stu.Class == "二班"; }).ToList();
~~~

### 8️⃣ Task
选取几个元素
~~~
Console.WriteLine("选取三个开始");
List<Student> students5 = students.Take(3).ToList();
Console.WriteLine("选择直至满足条件的数据开始");
List<Student> students6 = students.TakeWhile(stu => { return stu.Class == "二班"; }).ToList();
~~~

### 9️⃣ Join
联表查询
~~~
public static void Example()
{
    List<Product> products = new List<Product>
    {
        new Product { Name = "Cola", CategoryId = 0 },
        new Product { Name = "Tea", CategoryId = 0 },
        new Product { Name = "Apple", CategoryId = 1 },
        new Product { Name = "Kiwi", CategoryId = 1 },
        new Product { Name = "Carrot", CategoryId = 2 },
    };

    List<Category> categories = new List<Category>
    {
        new Category { Id = 0, CategoryName = "Beverage" },
        new Category { Id = 1, CategoryName = "Fruit" },
        new Category { Id = 2, CategoryName = "Vegetable" }
    };

    Console.WriteLine("查询表达式");
	// 根据CategoryId加入产品和类别
	var query = from product in products
			join category in categories on product.CategoryId equals category.Id
			select new { product.Name, category.CategoryName };
~~~
