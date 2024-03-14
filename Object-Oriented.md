# 面向对象 Object-Oriented    
## 1、 类 class
- 在面向对象的思想中最核心的就是**对象**，为了在程序中创建对象，首先需要【定义一个类】；
- 类 可以理解为一张设计图，描述了一个对象有什么构成，能做到什么事情；
- 一个类可以创建多个对象。
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/3d3aa58d-e5cf-4ab9-841d-843ba9548b6f)


:star: 例子展示：
~~~
namespace MethodParamDescriptor
{
    class Employee
    {
        //属性字段（成员变量）
        public string companyName = "";
        public string name = "";
        public string id = "";

        //类的行为（成员方法）
        public void IntroduceSelf()
        {
            Console.WriteLine("我是{0}公司的{1}，工号为{2}", companyName, name, id);//设置打印格式
        }

        public void call(string customerName)
        {
            Console.WriteLine("给{0}打电话，我是{1}公司的{2}，工号为{3}",customerName,companyName,name,id);
        }
        
    }

    internal class Program
    {
        static void Main(string[] args)
        {
           //设置填入占位符内容
            Employee e0 = new Employee();
            e0.companyName = "强盛集团";
            e0.name = "阿强";
            e0.id = "2025";

            Employee e1 = new Employee();
            e1.companyName = "强盛集团";
            e1.name = "啊盛";
            e1.id = "112";
            
            e0.IntroduceSelf();//将e0填写的内容运用在IntrodSelf方法中
            e1.IntroduceSelf();

            //分别拨打用户电话
            e0.call("1525644200");//将e0填写内容运用在call方法中
            e1.call("1288353530");
        }
    }
}
~~~

## 2、对象内存分布详解
- 对象变量为**引用类型**，new出来的多个对象，分别布局在了不同的内存上。
- ![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/8a787acf-9270-4208-80c6-357a0577cbfc)
- 成员变量🆚局部变量的区别：


|条目|成员变量|局部变量|
|:-----:|:---:|:----:|
|定义位置|方法外，类内|方法内|
|初始化|初始化为默认值|使用前必须手动初始化|
|内存位置|跟随对象放在堆内存|栈内存|
|生命周期|对象创建时诞生，对象消亡后消失|方法调用时诞生，方法结束后消失|
|作用域|对象内所有方法或通过对象.访问|所在最近大括号内|

## 3、this 引用   
- 当前**对象本身引用**，调用对象函数时，可以使用其引用**类内成员变量或方法**
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/2c9cc9d5-47a5-4a1a-af86-5b7282108999)
- 解决类内方法调用，局部变量与成员变量重名的问题
