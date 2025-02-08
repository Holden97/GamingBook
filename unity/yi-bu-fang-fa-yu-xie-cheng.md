# 异步方法与协程

1.Task

异步是用来转让程序运行权的工具。异步不是专门为多线程而设计的。

如果你写一段这样子的代码

```
Task AddRB2(params GameObject[] objs)
{
    return Task.Run(() =>
    {
        foreach (var o in objs)
            o.AddComponent<Rigidbody2D>();
    });
}
```

那你一定会收到来自Unity的毒打（前提是必须await函数所返回的Task，否则异常将不被捕获）：

```
UnityException: Internal_AddComponentWithType can only be called from the main thread.
Constructors and field initializers will be executed from the loading thread when loading a scene.
Don't use this function in the constructor or field initializers, instead move initialization code to the Awake or Start function.
```

Unity的报错里面有一些很有意思的地方：**要求UnityEngine API必须从主线程调用。**&#x800C;这里的主线程就是指驱动MonoBehaviour的生命周期函数的线程。我之前提到过一点，使用`Task.Run()`会把要计算的内容放置到另外一个线程内进行计算，因此这个问题也不难理解，但是你想在其他线程内调用UnityEngine API的梦想就要破碎了。

2.async/await

被async关键字标记了的方法，只能返回Task,Task\<T>和void类型的结果；对返回的Task的Result的获取会阻塞调用线程。

在async方法执行到await关键字时会处于挂起的状态，直到该异步动作完成后才恢复继续执行方法后面的动作。

3.coroutine

4.UniTask

5.Task测试方法

```
    [Button("测试异步方法")]
    public async void TestAsync()
    {
        Debug.Log("测试异步方法1");
        Debug.Log("测试异步方法2");
        Task<int> returnedTaskTResult = WorkAsync();
        Debug.Log("测试异步方法3");
        Debug.Log("测试异步方法4");
        int result = await returnedTaskTResult;
        Debug.Log("测试异步方法5");
        Debug.Log("测试异步方法6");
    }

    public async Task<int> WorkAsync()
    {
        await Task.Delay(1000);
        Debug.Log("测试异步方法 wait for result");
        return 4;
    }
```

得到的打印结果是

先出现

```
测试异步方法1
测试异步方法2
测试异步方法3
测试异步方法4
```

一秒后，出现

```
测试异步方法 wait for result
测试异步方法5
测试异步方法6
```

由此可见，官方文档中提到的，“遇到await点后，会继续执行不受异步影响的同步方法”的意思是会执行到不受异步结果影响的代码行之前，而不会跳过受影响的代码行继续向后执行。

参考资料

1. [https://wudaijun.com/2021/11/c-sharp-unity-async-programing/](https://wudaijun.com/2021/11/c-sharp-unity-async-programing/)
2. [https://zhuanlan.zhihu.com/p/197335532](https://zhuanlan.zhihu.com/p/197335532)&#x20;
3. [https://www.cnblogs.com/CreateMyself/p/5983208.html](https://www.cnblogs.com/CreateMyself/p/5983208.html)
4. [https://ca2didi.xyz/202204/why-we-need-unitask/](https://ca2didi.xyz/202204/why-we-need-unitask/)
5. [https://suika.blog.csdn.net/article/details/126559085?spm=1001.2101.3001.6650.2\&utm\_medium=distribute.pc\_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-2-126559085-blog-96462356.235%5Ev38%5Epc\_relevant\_sort\_base1\&depth\_1-utm\_source=distribute.pc\_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-2-126559085-blog-96462356.235%5Ev38%5Epc\_relevant\_sort\_base1\&utm\_relevant\_index=5](https://suika.blog.csdn.net/article/details/126559085?spm=1001.2101.3001.6650.2\&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-2-126559085-blog-96462356.235%5Ev38%5Epc_relevant_sort_base1\&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-2-126559085-blog-96462356.235%5Ev38%5Epc_relevant_sort_base1\&utm_relevant_index=5)
6. C#官方文档 [https://learn.microsoft.com/zh-cn/dotnet/csharp/asynchronous-programming/task-asynchronous-programming-model](https://learn.microsoft.com/zh-cn/dotnet/csharp/asynchronous-programming/task-asynchronous-programming-model)
7. 聊聊unity中的协程 [https://juejin.cn/post/6983937577162113060](https://juejin.cn/post/6983937577162113060)
