# 方法 Method   
## 1、方法的概念和格式
- 是一段具有独立功能的代码，可以被调取使用。
- 每一个 C# 程序至少有一个带有 Main 方法的类。
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/1389fd51-34d5-44f8-834e-3836cb153065)

:large_orange_diamond: 方法的优势：

:small_orange_diamond:可以将独立功能代码进行统一管理，避免代码臃肿杂乱   

:small_orange_diamond:统一功能代码不需要重复编写，**提高复用性**    


## 2、方法的参数与返回值   
 ![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/3875efac-4d6e-4098-9ddd-109f498a6bb0)
- 形式参数：简称**形参**，表示定义方法的时候，声明参数；不调用无内存分配。
- 实际参数：简称**实参**，表示使用方法的时候，传入的参数。

:red_circle: 返回值   
- 方法运行完毕会产生一个结果，这个结果会被调用者用于**接下来的其他代码逻辑当中**，就需要使用带返回值的方法定义。   
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/a7af47c7-b45a-4756-ae3b-da0fc3abe313)      
### 返回值的注意事项    
- 方法的返回值可以被接收，也可以**不被接收（void）**
- 大部分情况下，推荐接受方法返回值

:diamonds: 方法的三种递进定义    
~~~
public static void 方法名（）
{
    //普通方法
}
~~~
~~~
public static void 方法名（类型 变量1，类型 变量2...）
{
    //带参数方法
}
~~~
~~~
public static 数据类型 方法名（类型 变量1，类型 变量2...）
{
     //带参数带返回值
     return 返回值；
}
~~~
- void :arrow_right: 无返回值

### 方法使用细则
> 1️⃣ 方法与方法之间为平级关系，不得嵌套方法。
> 
> 2️⃣ 方法的定义顺序/位置，与执行顺序无关，按照调用顺序执行。
> 
> 3️⃣ void返回类型的方法，也可以使用return语句【结束当前方法执行】return不加任何数据
>> return的作用： :arrow_heading_down: 
>> - 结束当前方法执行，返回外层代码。
>> - 如需返回数据，结束方法同时，返回数据。

### 方法重载（Overload）
- 在同一个类中，定义多个**同名的方法**，每个方法**有不同的参数或参数个数**，称为方法重载。

 :small_red_triangle:  同一个类，方法名相同，参数不同

 :small_red_triangle:  参数：个数、类型、类型顺序   
 
 :small_red_triangle:  注意：是否重载与返回值无关     

 
### 方法参数修饰符   

|关键字|实参必初始化|方法内可写|方法内必须赋值|
|:---:|:--------:|:------:|:----------:|
|ref|是|是|否|
|in|是|否|否|
|out|否|是|是|

#### 1、ref 关键字（reference）   
- 允许参数以引用传递的方式给到方法中；
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/21a4e9b2-c7a8-498a-afcd-797fa2009397)
- 参数必须先**初始值**

#### 2、in 关键字
- 允许参数以引用传递的方式给到方法中；
- **本参数只能读取，不能赋值**；
- 传入到变量，一定会作为只读变量，在方法内部绝对不会被更改。
~~~
public static void doublelt(in int val)
{
      Console.WriteLine(val);
}
int a=10;
doublelt(in a);
~~~

#### 3、out 关键字
- 允许参数以引用传递的方式给到方法中；
- **进入方法后，参数值被清空，必须重新赋值后使用** ；
- 可以不初始化。
~~~
public static void doublelt(out int val)
{
     val=2;
}
int a;//可以赋值int a=10，也可以不赋值,到方法会被清空参数值
doublelt(out a);
~~~

    
