# Using的用处

## 一、**强制资源清理**

using可以算是.NET中新的语法元素，它清楚地说明一个通常比较占用资源的对象何时开始使用和何时被手动释放。当using可以被使用时，建议尽量使用using语句。至今为止，使用using语句发现它带给程序员的只有优点，而没有任何弊端。

在.NET的环境中，托管的资源都将由.NET的垃圾回收机制来释放，而一些非托管的资源则需要程序员手动地将它们释放。.NET提供了主动和被动两种释放非托管资源的方式，即IDisposable接口的Dispose方法和类型自己的Finalize方法。任何带有非托管资源的类型，都有必要实现IDisposable的Dispose方法，并且在使用完这些类型后需要手动地调用对象的Dispose方法来释放对象中的非托管资源。

> 如果类型正确地实现了Finalize方法，那么即使不调用Dispose方法，非托管资源也最终会被释放，但那时资源已经被很长时间无畏地占据了。

using语句的作用就是提供了一个高效的调用对象Dispose方法的方式。对于任何IDisposable接口的类型，都可以使用using语句，而对于那些没有实现IDisposable接口的类型，使用using语句会导致一个编译错误。

先来看一个using语句的基本语法：

```
using(StreamWriter sw= new StreamWriter())
{
    // 中间处理逻辑
}
```

在上面代码中，using语句一开始定义了一个StreamWriter的对象，之后在整个语句块中都可以使用sw，在using语句块结束的时候，sw的Dispose方法将会被自动调用。using语句不仅免除了程序员输入Dispose调用的代码，它还提供了机制保证Dispose方法被调用，无论using语句块顺利执行结束，还是抛出了一个异常。下面的代码演示了using的这一保护机制。

```
using System;

namespace usingDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            try
            {
                // 使用using
                using (MyDispose md = new MyDispose())
                {
                    md.DoWork();
                    // 抛出一个异常来测试using
                    throw new Exception("抛出一个异常");
                }
            }
            catch
            {

            }
            finally
            {
                Console.ReadKey();
            }
        }
    }

    /// <summary>
    /// 继承自IDisposable接口，仅仅用来做测试，不使用任何非托管资源
    /// </summary>
    public class MyDispose : IDisposable
    {
        public void Dispose()
        {
            Console.WriteLine("Dispose方法被调用");
        }
        public void DoWork()
        {
            Console.WriteLine("做了很多工作");
        }
    }
}
```

在上面的代码中，using语句块抛出了一个异常，而该异常知道using语句结束后才被捕获。由于有了using语句的存在，即使异常被抛出，MyDispose的对象md的DIspose方法仍然被调用。 。程序输出结果如下：

```
做了很多工作
Dispose方法被调用
```

事实上，C#编译器为using语句自动添加了try/finally块，所以Dispose方法能够保证被调用到，所以如下两段代码经过编译后内容将完全一致：

```
using (MyDispose md = new MyDispose())
{
      md.DoWork();
}
```

和

```
MyDispose md;
try
{
    md = new MyDispose();
    md.DoWork();
}
finally
{
    md.Dispose();
}
```

在彻底了解了using的实现原理以后，还应该注意一点使用using时常犯的错误，那就是千万不要试图在using语句块外初始化对象 ，如下面代码所示：

```
MyDispose md = new MyDispose();
using (md)
{
    md.DoWork();
}
```

看上去似乎没有任何问题，但是在多线程的程序中，上述代码就会有隐患。试想当md被初始化后程序突然产生一个异常而中断，那md对象中的非托管资源将没有机会得到释放，这对于系统来说危害是相当大的。所以在任何时候都应该在using语句中初始化需要使用的对象。



using语句为实现了IDisposable的类型对象调用Dispose方法，using语句能够保证使用的对象的Dispose方法在using语句块结束时被调用，无论是否有异常被抛出。C#编译器在编译时自动为using语句加上try/finally块，所以using的本质和异常捕获语句一样，但是语法更为简洁。所有using使用的对象都应该在using语句开始后再初始化，以保证所有的对象都能够被Dispose。

## **二、创建命名空间别名**

using为命名空间创建别名的用法规则为：

using alias = namespace | type;

其中namespace表示创建命名空间的别名；而type表示创建类型别名。

例如，在.NET Office应用中，常常会引入Microsoft.Office.Interop.Word.dll程序集，在引入命名空间时为了避免繁琐的类型输入，我们通常为其创建别名如下：

复制

<pre><code><strong>using MSWord = Microsoft.Office.Interop.Word;  
</strong></code></pre>

这样，就可以在程序中以MSWord来代替Microsoft.Office.Interop.Word前缀，如果要创建Application对象，则可以是这样，而且还有一个好处就是，在一个.CS文件中引入了不同的命名空间但是相同的类名的时候，用别名就可以解决这问题了。

再比如，创建元组类型的别名：

```
using VertexData = System.Tuple<UnityEngine.Vector3, UnityEngine.Vector3, UnityEngine.Vector2>;
```

定义后，即可使用别名

```
        HashSet<VertexData> pointsHash = new HashSet<VertexData>();
```

## **三、引用命名空间**

using作为引入命名空间指令的用法规则为：

using Namespace;

在.NET程序中，最常见的代码莫过于在程序文件的开头引入System命名空间，其原因在于System命名空间中封装了很多最基本最常用的操作，下面的代码对我们来说最为熟悉不过：

using System;

这样，我们在程序中就可以直接使用命名空间中的类型，而不必指定详细的类型名称。using指令可以访问嵌套命名空间。

关于：命名空间

命名空间是.NET程序在逻辑上的组织结构，而并非实际的物理结构，是一种避免类名冲突的方法，用于将不同的数据类型组合划分的方式。例如，在.NET中很多的基本类型都位于System命名空间，数据操作类型位于System.Data命名空间，



## 参考资料

1. using的使用方法 [https://www.cnblogs.com/dotnet261010/p/12329706.html](https://www.cnblogs.com/dotnet261010/p/12329706.html)
2. using的三种用处 [https://www.51cto.com/article/147158.html](https://www.51cto.com/article/147158.html)
