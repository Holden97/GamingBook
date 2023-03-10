# event

[event](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/event)关键字修饰的是一种特殊的多播委托，仅可以从声明事件的类（或派生类）或结构（发布服务器类）中对其进行调用。 如果其他类或结构订阅该事件，则在发布服务器类引发该事件时，将调用其事件处理程序方法。 有关详细信息和代码示例，请参阅[事件](https://learn.microsoft.com/zh-cn/dotnet/csharp/programming-guide/events/)和[委托](https://learn.microsoft.com/zh-cn/dotnet/csharp/programming-guide/delegates/)。

会有这种情况，一个模块A定义了一个委托变量, 外部模块B，要给这个委托变量添加[回掉函数](https://www.zhihu.com/search?q=%E5%9B%9E%E6%8E%89%E5%87%BD%E6%95%B0\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2542588528%7D), 这样，我们一般会把这个变量设置成public，直接可以访问，可是问题来了，触发的时候，A模块可以触发，B模块也可以触发，可是我如果只想要A模块触发，B模块只能添加，不能触发，而public 权限B模块也可以触发，这种如何解决呢？这个时候定义委托变量的时候，在前面加event, 这样外部模块B，只能往委托里面来添加函数，而不能触发[函数调用](https://www.zhihu.com/search?q=%E5%87%BD%E6%95%B0%E8%B0%83%E7%94%A8\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2542588528%7D)，触发函数调用，只有模块A，这个就是event修饰的作用。

事件不是引用类型，他与方法和属性一样，都是成员，这就意味着，事件是可以声明在Interface中的，这样非常便于面向对象开发，而Delegate是引用类型，是无法声明在Interface中的。

另外，在C#的语言内部，提供了很多无返回值的Delegate定义，从无参到多参，还有带返回值的Delegate，从无参到多参的定义，分别是：

```
System.Func
System.Action
```

\
如果提供的这些就能满足使用要求，就没有必要自己定义Delegate。

## 参考

1. [https://www.zhihu.com/question/471323020/answer/1995604167](https://www.zhihu.com/question/471323020/answer/1995604167)
2. [https://gwb.tencent.com/community/detail/128037](https://gwb.tencent.com/community/detail/128037)
