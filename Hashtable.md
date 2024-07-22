# 哈希表 Hash Table
![image](https://github.com/user-attachments/assets/3db8eefe-c390-48b8-a5df-5ea58c6f88dc)
- 哈希表（Hash Table）：也叫做散列表。是根据关键码值（Key Value）直接进行访问的数据结构。
- 哈希表通过「键 key 」和「映射函数 Hash(key) 」计算出对应的「值 value」，把关键码值映射到表中一个位置来访问记录，以加快查找的速度。
- 这个映射函数叫做「哈希函数（散列函数）」，存放记录的数组叫做「哈希表（散列表）」。

## 一、常用属性
|属性|说明|
|------|------|
|Count|获取包含在Hashtable中键值对的数目|
|IsFixedSize|获取一个值，该值指示Hashtable中是否具有固定的大小|
|IsSyncronized|获取一个值，该值指示是否同步对Hashtable的访问|
|Item|获取或设置与指定键相关联的值|
|Keys|获取包含在Hashtable中键的ICollection|
|SyncRoot|获取可用于同步Hashtable访问的对象|
|Values|获取包含在Hashtable中值的ICollection|

## 二、方法
### （ 1 ）Hashtable元素的添加
- 1️⃣ Add()方法
  
用来将带有指定键和值的元素添加到Hashtable中，其语法格式：  
~~~
    public virtual void Add(Object key,Object value)
    ☑ key：要添加的元素的键。
    ☑ value：要添加的元素的值，该值可以为空引用。
~~~

### （ 2 ）Hashtable元素的删除
- 1️⃣ Clear()方法
  
用来从Hashtable中移除所有元素，其语法格式：
~~~
public virtual void Clear()
~~~

- 2️⃣ Remove()方法

用来从Hashtable中移除带有指定键的元素，其语法格式：
~~~
public virtual void Remove(Object key)
key：要移除的元素的键。
~~~

### ( 3 ) Hashtable的遍历
- 1️⃣ DictionaryEntry结构

 Hashtable的遍历与数组类似，都可以使用foreach语句。这里需要注意的是，由于Hashtable中的元素是一个键/值对，因此需要使用DictionaryEntry结构来进行遍历。DictionaryEntry结构表示一个键/值对的集合。
~~~
class Program
    {
        static void Main(string[] args)
        {
            if (args is null)                               //解除IDE0060
            {
                throw new ArgumentNullException(nameof(args));
            }
            Hashtable hashtable = new /*Hashtable*/()       //实例化Hashtable对象
            {
                { "id", "BH0001" },                         //向Hashtable哈希表中添加元素
                { "name", "TM" },
                { "sex", "男" }
            };			
            Console.WriteLine("\t 键\t 值");                //格式化输出表头           
            foreach (DictionaryEntry dicEntry in hashtable) //遍历Hashtable哈希表中的元素并输出其键值对
            {
                Console.WriteLine("\t " + dicEntry.Key + "\t " + dicEntry.Value);
            }
            Console.WriteLine();
            Console.Read();
        }
    }
~~~
运行结果：
~~~
         键      值
         id      BH0001
         name    TM
         sex     男 
~~~
### ( 4 ) Hashtable元素的查找

可以使用Hashtable类提供的Contains()方法、ContainsKey()方法和 ContainsValue()方法

- 1️⃣ Contains()方法

用来确定Hashtable中是否包含特定键，其语法格式：
~~~
    public virtual bool Contains(Object key)
    ☑ key：要在Hashtable中定位的键。
    ☑ 返回值：如果Hashtable包含具有指定键的元素，则为true；否则为false。
~~~

- 2️⃣ ContainsKey()方法
  
  ContainsKey()方法和Contains()方法实现的功能、语法都相同。

- 3️⃣ ContainsValue()方法
  
用来确定Hashtable中是否包含特定值，其语法格式:
~~~
    public virtual bool ContainsValue(Object key)
    ☑ value：要在Hashtable中定位的值，该值可以为空引用。
    ☑ 返回值：如果Hashtable包含带有指定的value的元素，则为true；否则为false。
~~~


⚠️ps: 
Hashtable类： https://learn.microsoft.com/zh-cn/dotnet/api/system.collections.hashtable?view=net-8.0
