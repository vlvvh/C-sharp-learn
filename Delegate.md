# 委托（ Delegate ）
- 是一种 *引用类型变量*，用于存储某个方法的 *引用地址*。
- 定义格式：
  - public delegate 返回值类型 委托类型名字（参数类型1  参数名字，参数类型2  参数名字...）;

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
- :small_red_triangle: 回调方法（ CallBack Method ）:当某个任务执行完毕后 或 某事件触发后，调用的方法。
- 

