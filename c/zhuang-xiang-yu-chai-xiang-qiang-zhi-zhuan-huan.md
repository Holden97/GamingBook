# 装箱与拆箱/强制转换

**装箱**是将**值**类型转换为**引用**类型 ；

**拆箱**是将**引用**类型转换为**值**类型。

## [C#基础：简述装箱和拆箱原理](https://www.cnblogs.com/dotnet261010/p/12326344.html)

我们知道，值类型的变量是在堆栈上分配内存的，而引用类型包括System.Object的对象是在堆上分配内存的，基于这一特点，当值类型被类型转换时，会在堆栈和堆上进行一系列的操作，这就是装箱和拆箱的来源。充分理解装箱和拆箱，有助于程序员编写高效率的代码。

### 1、装箱和拆箱的基本概念

我们知道，所有的值类型都继承自System.ValueType，而System.ValueType继承自System.Object。所有的值类型对象都分配在堆栈上，而所有的引用类型包括System.Object对象都分配在堆上。问题随之而来，既然System.Object是所有值类型的基类，那所有的值类型必然都可以隐式的转换成System.Object类型，此时这个对象会被放在哪里呢，堆栈上面还是堆上面？实际上，当这个转换发生时，CLR需要做额外的工作把堆栈上的值类型移动到堆上，这个操作就被称为装箱。来看一个装箱所需要的详细步骤。

1. 在堆上分配一个内存空间，大小等于需要装箱的值类型对象的大小加上两个引用类型对象都拥有的成员：类型对象指针和同步块引用。
2. 把堆栈上的值类型对象复制到堆上新分配的对象。
3. 返回一个指向堆上新对象的引用，并且存储到堆栈上被装箱的那个值类型的对象里。

这些步骤都不需要程序员自己编写，在任何出现装箱的地方，编译器会自动地加上执行以上功能的中间代码。下图展示了装箱前后堆和堆栈的变化。



理解了装箱之后，就可以很方便地理解拆箱操作了。所谓的拆箱，就是装箱操作的反操作，把堆中的对象复制到堆栈中，并且返回其值。需要注意的是，拆箱操作将判断被拆箱的对象类型和将要被复制的值类型引用是否一致，如果不一致，将会抛出一个InvalidCastException的异常。这里的类型匹配并不采用任何显示的类型转换。下面的代码展示了这一特性。



```
static void Main(string[] args)
{
//1.第一种
    try
    {
        Int32 i = 3;
        // 装箱
        Object o = i;
        // 拆箱，类型转换失败
        Int16 j = (Int16)o;
    }
    catch (Exception ex)
    {
        Console.WriteLine(ex.Message);
    }
    
//2.第二种
    Int32 ii = 3;
    // 装箱
    Object obj = ii;
    // 拆箱
    Int16 jj = (Int16)(Int32)obj;
    Console.WriteLine("拆箱成功!");
    Console.ReadKey();
}
```

![](https://img2018.cnblogs.com/i-beta/1033738/202002/1033738-20200218102532921-31071087.png)

分析上面的代码，在第一组装箱拆箱操作中，代码试图把一个原来类型为Int32的值类型装箱后的对象拆箱成Int16的变量，这样的拆箱是非法的，运行时会抛出一个InvalidCastException的异常。而在第二组装箱拆箱操作中，就进行了正确的类型匹配，拆箱顺利完成。

### 2、装箱和拆箱对性能的影响，以及如何避免装箱拆箱

装箱和拆箱都意味着堆和堆栈空间的一系列操作，毫无疑问，这些操作的性能代价是很大的，尤其对于堆上空间的操作，速度相对于堆栈的操作慢的多，并且可能引发垃圾回收，这些都将大规模地影响系统的性能。如何避免装箱拆箱操作，是程序员在编写代码时需要时刻考虑的一个问题。装箱和拆箱操作常发生在以下两个场合：

1. 值类型的格式化输出。
2. System.Object类型的容器。

第一种情况，值类型的格式化输出往往会涉及一次装箱操作。例如下面的两行代码：

```
int i = 10;
Console.WriteLine("i的值是:" + i);
```

代码完全能够通过编译并且正确执行，但却引发了一次不必要的装箱操作。在第2行代码上，值类型i被作为一个System.Object对象传入方法之中，这样的操作完全可以通过下面的改动来避免：

```
int i = 10;
Console.WriteLine("i的值是:" + i.ToString());
```

改动后的代码调用了i的ToString()方法来得到一个字符串对象。由于字符串是引用类型，所以改动后的代码就不在涉及装箱操作。

第二种情况更为常见一些。例如常用的容器类ArrayList，就是一个典型的System.Object容器。任何值类型被放入ArrayList的对象中，都会引发一次装箱操作。而对应的，取出值类型对象就会引发一次拆箱操作。在.NET 1.1之前，这样的操作很难避免，但在.NET 2.0推出了泛型的概念后，这些问题得到了有效的解决。泛型允许定义针对某个特定类型（包括值类型）的容器，并且有效的避免装箱和拆箱。

## 三、总结

装箱和拆箱本质上是值类型在转换到System.Object时引发的堆栈和堆的一系列移动操作。装箱时值类型从堆栈上被复制到堆上，而拆箱时从堆上复制到堆栈上。装箱和拆箱对性能有比较大的影响，应该避免任何没有必要的装箱和拆箱操作。

在可以确定类型的情况下应该使用泛型技术而避免使用针对System.Object类型的容器，这样可以有效避免大规模地使用装箱和拆箱操作。

## 强制转换

尽量不要使用强制转换，而改用as和is来假设。as过程中不产生新的对象[^1]。

强制转换尽量用在明确知道目标对象类型的情况下。更多范例参见参考4.

## (int)和Convert.ToInt32的区别

(int)foo只是转换为Int32 (c#中的int)类型。这是内置于CLR中的，**并且要求foo是一个数值变量**(例如float, long等)。从这个意义上讲，它非常类似于C中的强制类型转换。

Convert.ToInt32被设计成一个通用的转换函数。它的作用比强制转换大得多;也就是说，它可以从任何基本类型转换为int(最明显的是解析字符串)。你可以在MSDN上看到这个方法的重载的完整列表。

参考

1. [https://stackoverflow.com/questions/1608801/what-is-the-difference-between-convert-toint32-and-int](https://stackoverflow.com/questions/1608801/what-is-the-difference-between-convert-toint32-and-int)
2. [https://learn.microsoft.com/en-us/dotnet/api/system.convert.toint32?view=net-8.0\&redirectedfrom=MSDN#overloads](https://learn.microsoft.com/en-us/dotnet/api/system.convert.toint32?view=net-8.0\&redirectedfrom=MSDN#overloads)
3. [https://www.cnblogs.com/dotnet261010/p/12326344.html](https://www.cnblogs.com/dotnet261010/p/12326344.html)
4. 强制转换 [https://learn.microsoft.com/zh-cn/dotnet/csharp/programming-guide/types/casting-and-type-conversions](https://learn.microsoft.com/zh-cn/dotnet/csharp/programming-guide/types/casting-and-type-conversions)

[^1]: [https://blog.csdn.net/shanniuliqingming/article/details/122395677](https://blog.csdn.net/shanniuliqingming/article/details/122395677)
