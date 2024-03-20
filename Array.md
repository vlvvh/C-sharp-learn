# 数组 Array   
## 1、数组基础应用
- 数组存储多个相同类型的数据集合，对这些数据统一管理。
> 格式
>> 数据类型[]数组变量名=new 数据类型[元素个数]；
>> 
>> 例：int[] arr=new int [5]；
>> 
>> 含义：定义arr是一个能装入**int数据的数组变量**，并且用new在内存中为这个数组开辟5个int数据的空间。
>> 
>> 初始化：
>>> int[] arr=new int[5]{1,2,3,5,7};
>>> 
>>> int[] arr={1,2,3,5,7};
- 数组内元素访问方式
- 数组名字 **[索引号]**
  
:heavy_exclamation_mark: 编程从0开始为第一个

❓ 如何获取数组的动态长度
- 数组名称.Length


## 2、数组初始化
1️⃣ 静态初始化：
- 明确给出需要操作的数值
   
:small_orange_diamond: int[] arr= **new** int[5]{1,2,5,6,7};
   
:small_orange_diamond: int[] arr={1,2,5,6,7};

~~~
静态初始化案例：

//已知成绩{100，99，88，97},求平均分
int[]scores={100，99，88，97};
int sum=0;
for(int i=0；i<scores.Length;i++)
{
    sum+=scores[i];
}
Console.WriteLine("平均分为:"+sum/scores.Length)
~~~

2️⃣ 动态初始化：
- 只能给元素个数，不能给出明确数值

:small_orange_diamond: int[] arr=**new** int[5];
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/0ec30342-7f40-4336-a8df-15230abaf1af)

~~~
动态初始化案例

int[]scores=new int[5];
int sum=0;
for(int i=0;i<scores.Length;i++)
{
     Console.WriteLine("请输入第{0}个成绩：", i);
     scores[i]=Convert.ToInt32(Console.ReadLine());
 } 
for(int i=0;i<scores.Length;i++)
{
     sum+=scores[i];
}
  Console.WriteLine(sum/scores.Length);
~~~


## 3、数组内存分配与堆内存    
- 栈内存：方法运行时，每个方法压入其中，运行完毕即可弹栈销毁。
- 堆内存：用于 **【存储动态】** 分配的对象的一种内存区域。
  - 动态分配和释放内存：允许在运行时动态地分配内存以存储对象，并在对象不再需要时释放该内存 :bangbang:
  - 垃圾回收
  - 不确定性分配时间
  - 需要手动管理非托管资源
- 方法区：加载当前运行程序IL代码，等待执行。
- 常量内存区：存储不可改变的常量。
### 数组与内存
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/00979e37-5c43-49c2-ae73-5e4b79a045a3)

### 数组互相之间赋值
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/f39809fb-6bcd-4c70-9b6e-8432a7ce5a5b)
    
:warning:  **总结**： 数组并不是直接在栈内存开辟空间，而是间接 **“引用”** 一块堆内存!!! 所以称为 **【引用类型】** :exclamation:   

### 方法中的参数
1️⃣  C# 中所有的参数传递都是**值传递**；
2️⃣ 引用类型也是拷贝了当前变量的值（堆内存中的地址）给到形参。    

## 4、数组越界与空引用    

### 数组越界
- 当使用index访问数组时，必须在数组元素范围内访问，如果 index<数组边界 就会引发数组越界。
~~~
namespace ArrayException
{
    internal class Program
    {
         int[] arr = { 1, 2, 3 };
         //int[]arr=null;
         //代表这个还没有被分配堆内存的引用变量，执行会显示NullReferenceException，报空引用错误
            
            Console.WriteLine(arr[0]);//1
            Console.WriteLine(arr[1]);//2
            Console.WriteLine(arr[2]);//3
            Console.WriteLine(arr[3]);//数组越界会显示：IndexOutOfRangeException，超出数组范围
      }
}
~~~


## 5、二维数组   
### ( 1 )概念格式：
- 数据采用行+列的形式存放，可以通过行标、列标**两个index**对数组元素进行访问。
- 格式：
  - 数据类型[,]数组名称={{第一行列数组元素},{第一行列数组元素},{第二行列数组元素}};
  - 数据类型[,]数组名称=new 数据类型[行数,列数]{{第一行列数组元素},{第一行列数组元素},{第二行列数组元素}};
  - 数据类型[，]数组名称=new 数据类型[n,m]；
  - ![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/6a8a14c5-dfe4-4318-8399-24a5b89bc955)


- 访问格式：
  - Console.WriteLine(arr[第几行,第几列]);
  - 已知当前数组arr有 n行 m列，给出 行标i，列标j，访问arr[i,j]时，转换为一维索引的规则为：
    - **index = i*m + j**

### （ 2 )二维数组遍历    
~~~

int[,] arr = {
      { 1, 2, 3 ,4},
      { 4, 5, 6, 4} ,
      { 4, 5, 6 ,4} 
};
      //GetLength可以获取到当前数组到行/列的数量
      //0:代表行
      //1:代表列
      //Console.Write(arr.GetLength(0));

      for (int j = 0; j < arr.GetLength(0); j++) //行
      {
      for(int i=0;i<arr.GetLength(1);i++) //列
      {
          Console.Write(arr[j, i]);
      }
}
~~~
#### 使用 foreach 循环 
- 如果循环体内代码，不需知道当前元素的行标/列标，可以直接使用foreach进行遍历。
  
~~~
foreach(int v in arr)
           {
               Console.WriteLine(v);
           }
~~~
- 【*foreach 工作原理*】:
  - 简化了对集合和数组的遍历，它隐藏了迭代器的实现细节，使得代码更加简洁和易读。
  - 对于实现了 IEnumerable 或 IEnumerable<T> 接口的集合或数组，foreach 循环会自动调用该集合或数组的迭代器，逐个访问其中的元素。
- 【*foreach 内存管理*】:
  - foreach 循环每次只会获取一个元素，并在循环结束后将其释放。
  - 对于实现了 IEnumerable 接口的集合，foreach 循环 使用集合的 GetEnumerator() 方法获取一个迭代器，并使用 MoveNext() 方法逐个访问集合中的元素。
   
## 6、锯齿数组 （JaggedArray）   
- 内部**每个元素是数组**，而且每个元素**数组长度都可以不同**
- 格式：
  - 数据类型[]数组名=new 数据类型[数组元素个数][]；
~~~
 int[][] arr=new int[4][];//分配了一个装有4个数组的 数组

          int[] arr0 = { 1, 2 };
          int[] arr1 = { 1, 2, 3 };
          int[] arr2 = { 4,5,6,7,8 };
          int[] arr3 = { 4,5,6,7,8,9,12 };

          arr[0] = arr0;//将 arr0 放入 arr 的第0个数组 里
          arr[1] = arr1;
          arr[2] = arr2;
          arr[3] = arr3;
          
          Console.WriteLine(arr[0][1]);//结果为arr0的 2
~~~
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/75d1b34c-07c9-48f2-a29a-f8d26f1339bb)


***
 ## ArrayList 、 LinkedList 、 List
 ### 1、ArrayList
 - 是一个动态数组，可以自动调整大小容纳到其中的元素。
 - 存储任何类型的对象，支持通过索引快速访问。因此它不是类型安全的，需要进行类型转换。
~~~
using System.Collections;
ArrayList arrayList = new ArrayList();   //创建ArrayList对象

arrayList.Add("Hello");                  //添加元素，可以添加任何类型 这里添加了string
arrayList.Add(52);                       //int
arrayList.Add(new Myclass());            //创建了一个新的 MyClass 类的实例对象，并存入arrayList里

object element = arrayList[0];           //访问第一个元素

foreach (object obj in arrayList)        //遍历
{
    Console.WriteLine(obj);
}

~~~

### 2、LinkedList 
-  是双向链表的实现，每个元素都包含对前一个和后一个元素的引用。
-  LinkedList 不支持通过索引进行快速访问元素，需要通过遍历链表来查找元素。
-  :small_red_triangle: LinkedList 是在需要频繁插入和删除操作，但不需要随机访问元素的情况下使用的一种数据结构。
~~~
LinkedList<int> linkedList = new LinkedList<int>();  //创建 LinkedList 对象，只能添加<这个括号里面的类型元素>

LinkedList.AddLast(10);                              //因为创建 LinkedList 对象时设定了 int 类型，只可以添加 int 类型
LinkedList.AddFrist(20);

foreach (int i in linkedList)                        //使用 foreach 遍历
{ 
    Console.WriteLine(i);
}

LinkedListNode<int> node = linkedList.Find(10);      // 找到值为 10 的节点
linkedList.AddAfter(node, 15);                       // 在节点 10 后插入值为 15 的节点

linkedList.Remove(20);                               // 使用 Remove 移除值为 20 的节点
~~~


### 3、List
- 一种泛型集合类型。
- 与 ArrayList 类似，但它只能存储指定类型的元素，更安全。
~~~
List<int> numbers = new List<int>();                       // 创建一个存储整数int的 List 实例,需要存什么类型就在<这里写入类型>

numbers.Add(10);                                           //使用 Add 方法向 List 中添加单个元素
numbers.Add(20);

List<int> moreNumbers = new List<int>() { 30, 40, 50 };    //使用 AddRange 方法向 List 中添加一个集合的元素
numbers.AddRange(moreNumbers);

int firstNumber = numbers[0];                              // 访问获取第一个元素

foreach (int num in numbers)                               //使用 foreach 遍历
{
    Console.WriteLine(num);
}
~~~

:warning: 区别总结:    
|特性|ArrayList|LinkedList|List<T>|
|---|---------|----------|-------|
|数据结构|动态数组|双向链表|动态数组|
|元素类型|'object'|泛型类型|泛型类型|
|安全性|较低，可以存储任何类型对象|较高，只能存<T>里类型|较高，只能存<T>里类型|
|随机访问|快速|慢|快|
|使用场景|随机访问需求较多，插入/删除较少的情况|插入/删除需求较多，随机访问较少的情况|随机访问和插入/删除的需求均匀分布的情况|



***
## 堆
- 二叉树，一定要有两个叉，要从左到右上到下填
- 完全树
- 值：最大堆/最小堆才是成熟的堆
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/c44de22c-1267-40dc-afed-0c2ba45f7e30)
### 堆的实现
#### 1、数组
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/9d7bab42-633c-4104-850f-bd9660d504b0)
- 已知数组A和索引i，A[i]的父节点为 A[(i-1)/2]
- 已知数组A和索引i，A[i]的左子节点为 A[2*i+1]
- 已知数组A和索引i，A[i]的右子节点为 A[2*i+2]
- 
  :bangbang:  i 为索引，不是值
