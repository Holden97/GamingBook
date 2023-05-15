# 多线程

## 为何Unity可以使用多线程 ，却要避免使用呢?

**一.Unity 为何避免使用多线程?**

实际上大多数游戏引擎也都是单线程的, 因为大多数游戏引擎是主循环结构, 逻辑更新和画面更新的时间点要求有确定性, 如果在逻辑更新和画面更新中引入多线程, 就需要做同步而这加大了游戏的开发难度, 尤其是对编程关卡的程序猿而言. 所以需要异步功能的时候, 游戏引擎总是倾向于使用 Time-Slicing 的策略而不是使用多线程, Unity 中的协程 (coroutine) yield 语法的本质就是 Time-Slicing.

Unity 的函数执行机制是帧序列调用，甚至连 Unity 的协程 Coroutine 的执行机制都是确定的，如果可以使用多线程访问 UnityEngine 的对象和 Api 就得考虑同步问题了，也就是说 Unity 其实根本没有多线程的机制，协程只是达到一个延时或者是当指定条件满足是才继续执行的机制。

Unity 引擎的类都不是线程安全的（Mathf 不是类）；Unity 没有自带的多线程解决方案，协程是假的多线程，本质还是单线程；

**二. 存在使用多线程的时候**

但是多线程也是有好处的, 如果不是画面更新, 也不是常规的逻辑更新 (指包括 AI | 物理碰撞 | 角色控制这些), 而是一些其他后台, 比如网络传输, 则可以将这个独立出来做成一个工作线程, 这需要写 Unity 游戏的 Native 扩展.

也就是说 Unity 开发过程中有使用多线程的时候, Unity 可以使用多线程，但对其有很多限制，也就是 在不使用 UnityEngine API 的情况下，可以使用多线程，提高多核 CPU 的使用率。而提到多线程就要提到 Unity 非常常用的协程，然而协程并非真正的多线程。协程其实是等某个操作完成之后再执行后面的代码，或者说是控制代码在特定的时机执行。而多线程在 Unity 渲染和复杂逻辑运算时可以高效的使用多核 CPU，帮助程序可以更高效的运行。通常可以将需要大量计算的算法内容，放置到多线程中执行，包括逻辑框架也可以放到多线程中执行。

**首先说明unity多线程操作的使用范围**

(1) 大量耗时的数据计算\
(2) 网络请求\
(3) 复杂密集的I/O操作\
(4) Unity3D的NativePlugin中可以新建子线程。通过NativePlugin可以接入移动端iOS与Android中的成熟库，可以是Objective C, Java, C++三种语言交叉混合的方式组成NativePlugin，然后使用Android或者iOS的SDK开辟子线程。

总的来说

对于不是画面更新，也不是常规的逻辑更新（指包括AI、物理碰撞、角色控制这些），而是一些其他后台任务，则可以将这个独立出来开辟一个子线程。

基本关键字：

Start()开始；Abort()终止；Join()阻塞；Sleep()休眠；.\
lock（obj）{}保证数据一致

创建一个多线程

```csharp
using UnityEngine;  
using System.Threading;  
  
  
public class BaseThread{  
      
    private static BaseThread instance;  
  
    object obj = new object();  
    int num = 0;  
    private BaseThread()  
    {  
  
        /*测试线程优先级  
        Thread th1 = new Thread(Th_test1);              //创建一个线程  
        Thread th2 = new Thread(Th_test2);  
        Thread th3 = new Thread(Th_test3);  
        th1.Start();  
        th2.Start();  
        th3.Start();  
        //学习优先级  
        th1.Priority = System.Threading.ThreadPriority.Highest;         //优先级最高  
        th2.Priority = System.Threading.ThreadPriority.Normal;  
        th3.Priority = System.Threading.ThreadPriority.Lowest;  
    }  
  
  
    public static BaseThread GetInstance()    
    {  
        if (instance == null)    
        {  
            instance = new BaseThread();    
        }    
        return instance;   
    }  
      
  
    //测试多线程锁  
    public void Th_lockTest()  
    {  
          
        Debug.Log("测试多线程");  
        while (true)  
        {  
            lock (obj)  
            {                                //线程“锁”           
                num++;  
                Debug.Log(Thread.CurrentThread.Name + "测试多线程" + num);  
            }  
            Thread.Sleep(100);  
            if (num > 300)  
            {  
                Thread.CurrentThread.Abort();  
            }  
        }  
    }  
  
    //测试多线程优先级  
    public void Th_test1()  
    {  
        for (int i = 0; i < 500; i++)  
        {  
             
            Debug.Log("测试多线程1执行的次数:" + i);  
            if(i >200)  
            {  
                Thread.CurrentThread.Abort();  
            }  
        }  
    }  
    public void Th_test2()  
    {  
        for (int i = 0; i < 500; i++)  
        {  
           
            Debug.Log("测试多线程2执行的次数:" + i);  
            if (i > 300)  
            {  
                Thread.CurrentThread.Abort();  
            }  
        }  
    }  
    public void Th_test3()  
    {  
        for (int i = 0; i < 500; i++)  
        {  
        
            Debug.Log("测试多线程3执行的次数:" + i);  
            if (i > 400)  
            {  
                Thread.CurrentThread.Abort();  
            }  
        }  
    }    
}
注意:

确保线程的数据一致需要使用lock关键字，以免数据混乱。

单核线程速度是不如协同程序的。

因为多线程的转化次数要比协程高，若在多核当多核计算速率计算时间会倍率减少，而携程仍是单线程计算时间不变。
```

Unity 使用多线程注意:

1. 变量都是共享的(都能指向相同的内存地址)
2. UnityEngine 的 API 不能在分线程运行
3. UnityEngine 定义的基本结构(int, float, struct 定义的数据类型)可以在分线程计算，如 Vector3(struct)可以, 但 Texture2d(class,根父类为 Object) 不可以。
4. UnityEngine 定义的基本类型的函数可以在分线程运行



### C# Task.Delay() 和 Thread.Sleep() 区别

1、Thread.Sleep 是同步延迟，Task.Delay异步延迟。

2、Thread.Sleep 会阻塞线程，Task.Delay不会。

3、Thread.Sleep不能取消，Task.Delay可以。

4\. Task.Delay() 比 Thread.Sleep() 消耗更多的资源，但是Task.Delay()可用于为方法返回Task类型；或者根据CancellationToken取消标记动态取消等待

**5. Task.Delay() 实质创建一个运行给定时间的任务， Thread.Sleep() 使当前线程休眠给定时间。**

```javascript
using System;
using System.Diagnostics;
using System.Threading;
using System.Threading.Tasks;

namespace ConsoleApp5
{
    class Program
    {
        static void Main(string[] args)

        {
            Task delay = asyncTask();
            syncCode();
            delay.Wait();
            Console.ReadLine();
        }
        static async Task asyncTask()
        {
            var sw = new Stopwatch();
            sw.Start();
            Console.WriteLine("async: Starting *");
            Task delay = Task.Delay(5000);
            Console.WriteLine("async: Running for {0} seconds **", sw.Elapsed.TotalSeconds);
            await delay;
            Console.WriteLine("async: Running for {0} seconds ***", sw.Elapsed.TotalSeconds);
            Console.WriteLine("async: Done ****");
        }
        static void syncCode()
        {
            var sw = new Stopwatch();
            sw.Start();
            Console.WriteLine("sync: Starting *****");
            Thread.Sleep(5000);
            Console.WriteLine("sync: Running for {0} seconds ******", sw.Elapsed.TotalSeconds);
            Console.WriteLine("sync: Done *******");
        }
    }
}
```

复制

_运行结果：_

<figure><img src="https://ask.qcloudimg.com/http-save/yehe-7674702/rluscwa70w.png?imageView2/2/w/1200" alt=""><figcaption></figcaption></figure>

我们可以看到这个代码的执行过程中遇到`await`后就会返回执行了，待await的代码执行完毕后才继续执行接下来的代码的！

\----------------------------------------------------------

_Use `Thread.Sleep` when you want to block the current thread._ **要阻止当前线程时，请使用`Thread.Sleep` 。**

_Use `Task.Delay` when you want a logical delay without blocking the current thread._ **如果需要逻辑延迟而不阻塞当前线程，请使用`Task.Delay` 。**

_Efficiency should not be a paramount concern with these methods._ **对于这些方法，效率不应该是最重要的问题。** _Their primary real-world use is as retry timers for I/O operations, which are on the order of seconds rather than milliseconds._ **它们在现实世界中的主要用途是作为I / O操作的重试计时器，其数量级为秒而不是毫秒**

_Also, it is interesting to notice that `Thread.Sleep` is far more accurate, ms accuracy is not really a problem, while `Task.Delay` can take 15-30ms minimal._ **另外，有趣的是， `Thread.Sleep`准确性要高得多，ms的准确性并不是真正的问题，而`Task.Delay`占用时间最少为15-30ms。** _The overhead on both functions is minimal compared to the ms accuracy they have (use `Stopwatch` Class if you need something more accurate)._ **与它们具有的ms精度相比，这两个函数的开销是最小的（如果您需要更精确的信息，请使用`Stopwatch` Class）。** _`Thread.Sleep` still ties up your Thread, `Task.Delay` release it to do other work while you wait._ **`Thread.Sleep`仍然占用您的线程， `Task.Delay`释放它以便在您等待时进行其他工作。**

## 参考资料

1. [https://www.jianshu.com/p/854649bc0ce6](https://www.jianshu.com/p/854649bc0ce6)
2. [https://cloud.tencent.com/developer/article/1682240](https://cloud.tencent.com/developer/article/1682240)
