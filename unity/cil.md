# CIL

为什么是C#？

因为C#的跨平台特性可以解决Unity引擎的跨平台支持问题。

其实确切的说，跨平台的并不是C#这个语言，而是C# 编译后生成的中间语言（CIL，Common Intermediate Language，以前也叫 MSIL）。

小结：

Unity第一推荐开发语言C#，之所以用C#的跨平台特性。而C#之所以跨平台是因为CIL是平台无关的。只要任何一个平台实现了当前平台对应的CLR，就可以在当前平台上解析和运行CIL。从而达到了一次编译，全平台运行的效果。

通用语言运行平台（Common Language Runtime，简称CLR）

<figure><img src="https://pic4.zhimg.com/80/v2-c435b45839b9f16fc5fdae0a85b5a78b_1440w.webp" alt=""><figcaption></figcaption></figure>

从上面可以看出，C#或者CIL跨平台离不开对应平台CLR的支持。如果没有对应平台CLR对CIL的解析和对应机器码的实现，CIL就无法在当前平台运行。

维基百科上CLR包含如下功能：

1.基类库支持 Base Class Library Support

2.内存管理 Memory Management

3.线程管理 Thread Management

4.垃圾回收 Garbage Collection

5.安全性 Security

6.类型检查 Type Checker

7.异常管理 Exception Manager

8.调试管理 Debug Engine

9.中间码(MSIL)到机器代码(Native)编译

10.类别装载 Class Loader

Unity上所使用的的CLR实现

主流的CLR实现有两个版本

一个是[.NET Core](https://link.zhihu.com/?target=https%3A//zh.wikipedia.org/wiki/.NET\_Core) 是微软开发的跨平台 ([Windows](https://link.zhihu.com/?target=https%3A//zh.wikipedia.org/wiki/Windows)、[Mac OSX](https://link.zhihu.com/?target=https%3A//zh.wikipedia.org/wiki/Mac\_OSX)、[Linux](https://link.zhihu.com/?target=https%3A//zh.wikipedia.org/wiki/Linux)) 的应用程序开发框架（Application Framework）。

另外一个就是Unity所使用的[Mono](https://link.zhihu.com/?target=https%3A//zh.wikipedia.org/wiki/Mono)，一个[开源](https://link.zhihu.com/?target=https%3A//zh.wikipedia.org/wiki/%25E9%2596%258B%25E6%2594%25BE%25E6%25BA%2590%25E4%25BB%25A3%25E7%25A2%25BC)的CLR实现版本。

Mono是什么？

Mono 是跨平台的 .Net Framework 的实现。Mono 做了一件很了不起的事情，将 CLR 在所有支持的平台上重新实现了一遍（包含手机上常用的安卓平台和Switch，PS4，这样的游戏机平台），并且mono还将 .Net Framework 提供的基础类库也重新实现了一遍。

Unity当年选用Mono而不是几乎只能在Windows上运行的.Net Framework就是看重了Mono的跨平台支持。并且Mono是一个开源软件，Unity也很方便在Mono的基础上做修改和微调。

好了，上面就是Unity和C#，Mono的各种纠葛。

总结一下就是

Unity因为方便和跨平台选择了C#作为主要的开发语言。而且C#的跨平台是基于.Net Framework框架下的（CIL，通用描述语言）和CLR（通用运行环境的）。

在经过各种考量后，Unity选择了开源，并且平台支持性很好的Mono这一开源的.Net Framework跨平台实现方案。

## 参考资料

1. [https://zhuanlan.zhihu.com/p/266037775](https://zhuanlan.zhihu.com/p/266037775)
