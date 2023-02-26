# lock

## 什么是lock?

lock语句获取给定对象的互斥 lock，执行语句块，然后释放 lock。 持有 lock 时，持有 lock 的线程可以再次获取并释放 lock。 阻止任何其他线程获取 lock 并等待释放 lock。

lock语句具有以下格式

```csharp
lock (x)
{
    // Your code...
}
```

其中 `x` 是[引用类型](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/reference-types)的表达式。 它完全等同于

C#复制

```csharp
object __lockObj = x;
bool __lockWasTaken = false;
try
{
    System.Threading.Monitor.Enter(__lockObj, ref __lockWasTaken);
    // Your code...
}
finally
{
    if (__lockWasTaken) System.Threading.Monitor.Exit(__lockObj);
}
```

由于该代码使用 [try...finally](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/try-finally) 块，即使在 `lock` 语句的正文中引发异常，也会释放 lock。

在 `lock` 语句的正文中不能使用 [await 运算符](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/operators/await)。

`System.Threading.Monitor.Enter`用于获取特定对象的互斥锁，而`System.Threading.Monitor.Exit`用于释放特定对象的互斥锁。

## 如何使用lock?

lock 关键字可确保当一个线程位于代码的临界区时，另一个线程不会进入该临界区。 如果其他线程尝试进入锁定的代码，则它将一直等待（即被阻止），直到该对象被释放。\
lock 关键字在块的开始处调用 Enter，而在块的结尾处调用 Exit。 ThreadInterruptedException 引发，如果 Interrupt 中断等待输入 lock 语句的线程。\
通常，应避免锁定 public 类型，否则实例将超出代码的控制范围。

常见的结构 lock (this)、lock (typeof (MyType)) 和 lock ("myLock") 违反此准则：\
如果实例可以被公共访问，将出现 lock (this) 问题。\
如果 MyType 可以被公共访问，将出现 lock (typeof (MyType)) 问题。\
由于进程中使用同一字符串的任何其他代码都将共享同一个锁，所以出现 lock("myLock") 问题。\
最佳做法是定义 private 对象来锁定, 或 private static 对象变量来保护所有实例所共有的数据。\
在 lock 语句的正文不能使用 等待 关键字。

#### lock(string)

string也是应用类型，从语法上来说是没有错的。

但是锁定字符串尤其危险，因为字符串被公共语言运行库 (CLR)“暂留”。 这意味着整个程序中任何给定字符串都只有一个实例，就是这同一个对象表示了所有运行的应用程序域的所有线程中的该文本。因此，只要在应用程序进程中的任何位置处具有相同内容的字符串上放置了锁，就将锁定应用程序中该字符串的所有实例。通常，最好避免锁定 public 类型或锁定不受应用程序控制的对象实例。例如，如果该实例可以被公开访问，则 lock(this) 可能会有问题，因为不受控制的代码也可能会锁定该对象。这可能导致死锁，即两个或更多个线程等待释放同一对象。出于同样的原因，锁定公共数据类型（相比于对象）也可能导致问题。而且lock(this)只对当前对象有效，如果多个对象之间就达不到同步的效果。lock(typeof(Class))与锁定字符串一样，范围太广了。

## 总结

1. lock的是引用类型的对象，string类型除外。
2. lock推荐的做法是使用静态的、只读的、私有的对象。
3. 保证lock的对象在外部无法修改才有意义，如果lock的对象在外部改变了，对其他线程就会畅通无阻，失去了lock的意义。
4. 一个lock关键字对应着一对Enter和Exit操作，Enter操作获取锁，而Exit操作释放锁，已经获取了锁的线程可以再次获取锁来释放它，而没有获取锁的线程只能等待拥有锁的线程释放锁之后再进行操作。整个过程就像是一堆人（线程）排队用公用电话（特定对象），在前一个人站在电话亭里拿着公用电话卿卿我我的时候，排在他后面的人再着急也得等着。

## 参考资料

1. lock 语句（C# 参考） [https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/statements/lock](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/statements/lock)
2. wolfy博客园 [https://www.cnblogs.com/wolf-sun/p/4209521.html](https://www.cnblogs.com/wolf-sun/p/4209521.html)
