# HashSet

## 什么是HashSet?

所谓的HashSet，指的就是 `System.Collections.Generic` 命名空间下的 `HashSet<T>` 类，它是一个高性能，无序的集合，因此HashSet它并不能做排序操作，也不能包含任何重复的元素，Hashset 也不能像数组那样使用索引，所以在 HashSet 上你无法使用 for 循环，只能使用 foreach 进行迭代，HashSet 通常用在处理元素的唯一性上有着超高的性能。

`HashSet<T>` 实现了如下几个接口：

```
public class HashSet<T> : 
System.Collections.Generic.ICollection<T>,
System.Collections.Generic.IEnumerable<T>, 
System.Collections.Generic.IReadOnlyCollection<T>,
System.Collections.Generic.ISet<T>,
System.Runtime.Serialization.IDeserializationCallback,
System.Runtime.Serialization.ISerializable
{
}
```

HashSet 只能包含**唯一的元素**，它的内部结构也为此做了专门的优化，值得注意的是，HashSet 也可以存放单个的 null 值，可以得出这么一个结论：如何你想拥有一个具有唯一值的集合，那么 HashSet 就是你最好的选择，何况它还具有超高的检索性能。

## HashSet的常用操作

### 构造

使用包含重复元素的数组构造新HashSet时，重复元素将被剔除。

```
        static void Main(string[] args)
        {
            string[] cities = new string[] {
                "Delhi",
                "Kolkata",
                "New York",
                "London",
                "Tokyo",
                "Washington",
                "Tokyo"
            };
            HashSet<string> hashSet = new HashSet<string>(cities);
            foreach (var city in hashSet)
            {
                Console.WriteLine(city);
            }
        }
```

输出如下

```
                Delhi
                Kolkata
                New York
                London
                Tokyo
                Washington
```

### 添加元素 <a href="#item-4" id="item-4"></a>

&#x20;如果你使用HashSet 的Add方法中插入重复的元素，它会忽视这次操作。

```
        static void Main(string[] args)
        {
            HashSet<string> hashSet = new HashSet<string>();
            hashSet.Add("A");
            hashSet.Add("B");
            hashSet.Add("C");
            hashSet.Add("D");
            hashSet.Add("D");
            Console.WriteLine("The number of elements is: {0}", hashSet.Count);
            Console.ReadKey();
        }
```

当你执行了这个程序，hashSet.Count将为4。

### 查找元素 <a href="#item-3" id="item-3"></a>

如果想判断某一个元素是否在 HashSet 内，建议使用 Contains 进行判断，代码如下：

```
        static void Main(string[] args)
        {
            HashSet<string> hashSet = new HashSet<string>();
            hashSet.Add("A");
            hashSet.Add("B");
            hashSet.Add("C");
            hashSet.Add("D");
            if (hashSet.Contains("D"))
                Console.WriteLine("The required element is available.");
            else
                Console.WriteLine("The required element isn’t available.");
            Console.ReadKey();
        }
```

### 移除元素 <a href="#item-5" id="item-5"></a>

从HashSet 中删除某一个元素可以调用 Remove 方法，它的语法结构如下：

```
public bool Remove (T item);
```

如果在集合中找到了这个元素，Remove方法将会删除这个元素并且返回true，否则返回 false。

下面的代码片段展示了如何使用 Remove 方法删除 HashSet 中的元素

```
string item = "D";
if(hashSet.Contains(item))
{
   hashSet.Remove(item);
}
```

### 移除所有满足特定条件的元素 <a href="#item-5" id="item-5"></a>

```
public int RemoveWhere (Predicate<T> match);
```

返回值返回满足条件的元素个数。

### 删除所有元素

调用 Clear 方法。

### 集合操作 <a href="#item-6" id="item-6"></a>

HashSet提供了非常多的方法用于集合操作上，比如说：`IntersectWith, UnionWith, IsProperSubsetOf, ExceptWith, 和 SymmetricExceptWith`

#### IsProperSubsetOf <a href="#item-6-1" id="item-6-1"></a>

判断 HashSet 是否为某一个集合的完全子集，可以看下面的例子：

```
HashSet<string> setA = new HashSet<string>() { "A", "B", "C", "D" };
HashSet<string> setB = new HashSet<string>() { "A", "B", "C", "X" };
HashSet<string> setC = new HashSet<string>() { "A", "B", "C", "D", "E" };
if (setA.IsProperSubsetOf(setC))
   Console.WriteLine("setC contains all elements of setA.");
if (!setA.IsProperSubsetOf(setB))
   Console.WriteLine("setB does not contains all elements of setA.");
```

如果你执行了上面这个程序，你会在控制台上看到如下的输出：

```
   setC contains all elements of setA.
   Console.WriteLine("setB does not contains all elements of setA.
```

#### UnionWith <a href="#item-6-2" id="item-6-2"></a>

用于合并集合，比如说下面的代码：

```
HashSet<string> setA = new HashSet<string>() { "A", "B", "C", "D", "E" };
HashSet<string> setB = new HashSet<string>() { "A", "B", "C", "X", "Y" };
setA.UnionWith(setB);
foreach(string str in setA)
{
   Console.WriteLine(str);
}
```

当你执行完上面的代码，SetB 集合会被 SetA 集合吞掉，最后 SetA 集合将会是包括：`"A", "B", "C", "D", "E", "X", and "Y"` 。

#### IntersectWith <a href="#item-6-3" id="item-6-3"></a>

用于表示两个 HashSet 的交集，下面的例子或许会让你更加理解：

```
HashSet<string> setA = new HashSet<string>() { "A", "B", "C", "D", "E" };
HashSet<string> setB = new HashSet<string>() { "A", "X", "C", "Y"};
setA.IntersectWith(setB);
foreach (string str in setA)
{
    Console.WriteLine(str);
}
```

当你运行了上面的这段程序，只有两个 HashSet 中都存在的元素才会输出到控制台中，本例中输出A C。

**ExceptWith**

ExceptWith 方法表示数学上的减法操作，这个时间复杂度是 O(N)，假定你有两个HashSet 集合，分别叫 setA 和 setB，并且用了下面的语句。

```
setA.ExceptWith(setB);
```

它返回的元素为： setA中有，setB中没有 的最终结果，如果还不明白的话,使用如下代码辅助理解：

```
HashSet<string> setA = new HashSet<string>() { "A", "B", "C", "D", "E" };
HashSet<string> setB = new HashSet<string>() { "A", "X", "C", "Y" };
setA.ExceptWith(setB);
foreach (string str in setA)
{
   Console.WriteLine(str);
}
```

当你执行了上面这段程序，元素 B,D,E 将会输出到控制台上。

#### SymmetricExceptWith <a href="#item-6-5" id="item-6-5"></a>

SymmetricExceptWith 方法常用于修改一个 HashSet 来存放两个 HashSet 都是唯一的元素，换句话说，我要的就是两个集合都不全有的元素，如果还不明白的话，考虑下面的代码段：

```
HashSet<string> setA = new HashSet<string>() { "A", "B", "C", "D", "E" };
HashSet<string> setB = new HashSet<string>() { "A", "X", "C", "Y" };
setA.SymmetricExceptWith(setB);
foreach (string str in setA)
{
  Console.WriteLine(str);
}
```

当你执行完上面的代码，你会发现，setA中有而setB中没有 和 setB中有而setA中没有的元素将会输出到控制台中。

本例中输出X B Y D E。

## 操作复杂度分析

我们知道数组的平均复杂度是 O(N)，这里的 n 表示数组里的元素数量，而访问 HashSet 中的某一个元素，它的复杂度为 O(1)，这个常量复杂度就决定了 HashSet 在快速检索和执行集合操作上是一个非常好的选择。

### 为什么访问HashSet的某一个元素的复杂度是1？

## 参考资料

1. 使用VS创建一个.NET Core控制台程序 [https://segmentfault.coHm/a/1190000038565893](https://segmentfault.com/a/1190000038565893)
