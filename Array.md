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
