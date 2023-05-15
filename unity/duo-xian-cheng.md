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

## 参考资料

1. [https://www.jianshu.com/p/854649bc0ce6](https://www.jianshu.com/p/854649bc0ce6)
