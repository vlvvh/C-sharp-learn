# 委托（ Delegate ）
-  概念：是一种 *引用类型变量*，用于存储某个方法的 *引用地址*。
- 定义格式：
  - public delegate 返回值类型 委托类型名字（参数类型1  参数名字，参数类型2  参数名字...）;
  - public 委托类型名字 委托名字；

~~~
namespace SeniorDelegate
{
 internal class Program
 {
  public delegate int Calculate(int x, int y);   // 声明了一种数据类型（委托类型）叫做Calculate，代表某一种方法  // 输入：int int 输出：int

  static void Main(string[] args)
  {
   //int sum = Add(1, 2);                        // 直接调用Add方法

   Calculate cal0 = new Calculate(Add);          // 调用 Calculate 里面的 Add方法
   Calculate cal1 = new Calculate(Multiply);     // 调用 Calculate 里面的 Multiply方法

   int sum = cal0.Invoke(1, 2);                  // 打印结果 Add方法 委托 cal0 帮忙执行
   Console.WriteLine(sum);

   int Multi = cal1(1, 2);                       // 打印结果 Multi方法 委托 cal1 帮忙执行
   Console.WriteLine(Multi);
  }

  static public int Add(int x, int y)
  {
   return x + y;                                 //返回 x+y 的值
  }

  static public int Multiply(int x, int y)
  {
   return x * y;                                 //返回 x*y 的值
  }
 }
}
~~~
## 一、回调方法（ CallBack Method ）
- 概念:当某个任务执行完毕后 或 某事件触发后，调用的方法。
- ![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/418ef0ef-bd1f-4600-8e9a-f24bc10532a7)
### 例子：
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/8f01039b-a5e4-4ceb-9548-ba1a73a1269c)   

> 操作逻辑：
>> 1 场面上所有按钮，都是当前 Button 来实例化的对象
>> 
>>  2 每个按钮被点击后，发生的事情不一样
>> 
>>  3 每个按钮被点击后，发生的事情由外界决定

~~~
namespace SeniorDelegate
{
 class Button
 {
  public delegate void OnClickDelegate();             // 1️⃣ 委托类型 OnClickDelegate，用于声明按钮点击事件的方法签名。
  public OnClickDelegate onclick = null;              // 2️⃣ onclick 的委托实例，用于存储要执行的点击事件处理程序。
  public void Click()                                 // 3️⃣ Click 方法，当按钮被点击时调用委托所引用的方法。
  {
   Console.WriteLine("按钮被点击了");
   if (onclick != null) onclick();                    // onclick不等于null，才可以调用委托onclick的方法
  }
 }
 
 internal class Program
 {
  static void Main(string[] args)
  {
   GameController gameController0 = new GameController();    // 创建了两个 GameController 实例
   gameController0.name = "gc0";
   GameController gameController1 = new GameController();
   gameController1.name = "gc1";
   
   Button gameStartButton = new Button();                    // 创建了一个 Button 实例 gameStartButton
   gameStartButton.onclick = new Button.OnClickDelegate(gameController0.OnGameStart);  // 将 gameController0 的 OnGameStart 方法绑定到 gameStartButton 的 onclick 委托上
   gameStartButton.Click();                                  // 调用 gameStartButton.Click() 触发按钮点击事件

   Button friendButton = new Button();                       // 创建了第二个 Button 实例 friendButton
   friendButton.onclick = new Button.OnClickDelegate(gameController1.OnFriend);
   friendButton.Click();
  }

  static public void OnGameStart()                           // 点击了游戏开始而触发的方法
  {
   Console.WriteLine("游戏开始了");
  }

  static public void OnFriend()
  {
   Console.WriteLine("你已分享给好友");
  }
 }
}
~~~
- 委托delegate 允许您像使用其他任何类型一样使用方法。您可以将委托实例化，然后将其传递给其他方法，从而允许这些方法在需要时调用委托所引用的方法。

## 二、委托的多播（Multicasting of a Delegate）
- 多播：一个委托对象可以引用多个方法，当调用委托时，所有引用的方法都会依次执行。
- 这种行为使得可以将多个方法绑定到同一个委托上，从而在调用委托时同时触发多个方法。
~~~
public delegate int Calculate(int x,int y);

static public int Add(int x,int y)
{
   Console.WriterLine(x+y);
   return x+y;
}
static public int Multiply(int x,int y)
{  Console.writeLine(x*y);
   return x*y;
}
Calculate cal0=new Calculate(Add);
Calculate cal1=new Calculate(Multiply);
Calculate cal2=new Calculate(Add);

// 多播委托有三种方法：
1️⃣ 通过初始化为null ，经过加法运算创建多播委托
Calculate cal=null;                       // 主委托 当前还未分配内存，为空 
cal+=cal0;                                // cal的创建就是在这句代码，创建全新的委托对象
cal+=cal1;        
cal+=cal2;                
cal(1,3);
       
2️⃣通过表达式创建多播委托
Calculate cal=cal0+cal1+cal2
cal-=cal0                                 // 通过“-”删除子委托 cal0

3️⃣通过等于某个子委托来创建多播委托
Calculate cal=cal0；cal+=cal1；cal+=cal2   // ⚠️注意：cal与cal0不是同一个
~~~
- 通过“+”增加子委托，通过“-”删除某个子委托
- 多播调用的返回值是最后一个执行委托的返回值
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/64959325-9f97-4acc-986f-e0ac9a26775d)

### 委托的多播逐个调用
- 如果需要拿到每一个回调方法的执行返回值🔙，可以采用逐个委托调用
~~~
public delegate int Calculate(int x,int y);
static public int Add(int x,int y)
{
   return x+y;
}
static public int Multiply(int x,int y)
{
   return x*y;
}
Calculate cal0=new Calculate(Add);
Calculate cal1=new Calculate(Multiply);
Calculate cal2=new Calculate(Add);

Calculate cal=cal0+cal1+cal2;                       // 通过表达式创建多播委托
foreach(Calculate c in cal.GetInvocationList()      // 循环遍历每一个子委托并且执行，打印返回值
{
    int r=c(1,3);
    Console.WirteLine(r);
}
~~~

