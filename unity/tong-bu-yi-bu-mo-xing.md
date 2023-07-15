# 同步/异步模型

1.Task

2.async/await

被async关键字标记了的方法，只能返回Task,Task\<T>和void，对该Task的Result的获取会阻塞调用线程。

在async方法执行到await关键字时会处于挂起的状态，直到该异步动作完成后才恢复继续执行方法后面的动作。

3.coroutine

4.UniTask

参考资料

1. [https://wudaijun.com/2021/11/c-sharp-unity-async-programing/](https://wudaijun.com/2021/11/c-sharp-unity-async-programing/)
2. [https://zhuanlan.zhihu.com/p/197335532](https://zhuanlan.zhihu.com/p/197335532)&#x20;
3. [https://www.cnblogs.com/CreateMyself/p/5983208.html](https://www.cnblogs.com/CreateMyself/p/5983208.html)
