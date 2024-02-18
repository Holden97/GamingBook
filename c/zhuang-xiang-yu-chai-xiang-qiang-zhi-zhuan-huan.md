# 装箱与拆箱/强制转换

装箱是将值类型转换为引用类型 ；拆箱是将引用类型转换为值类型。

尽量不要使用强制转换，而改用as和is。强制转换尽量用在明确知道目标对象类型的情况下。

## (int)和Convert.ToInt32的区别

(int)foo只是转换为Int32 (c#中的int)类型。这是内置于CLR中的，**并且要求foo是一个数值变量**(例如float, long等)。从这个意义上讲，它非常类似于C中的强制类型转换。

Convert.ToInt32被设计成一个通用的转换函数。它的作用比强制转换大得多;也就是说，它可以从任何基本类型转换为int(最明显的是解析字符串)。你可以在MSDN上看到这个方法的重载的完整列表。

参考

1. [https://stackoverflow.com/questions/1608801/what-is-the-difference-between-convert-toint32-and-int](https://stackoverflow.com/questions/1608801/what-is-the-difference-between-convert-toint32-and-int)
2. [https://learn.microsoft.com/en-us/dotnet/api/system.convert.toint32?view=net-8.0\&redirectedfrom=MSDN#overloads](https://learn.microsoft.com/en-us/dotnet/api/system.convert.toint32?view=net-8.0\&redirectedfrom=MSDN#overloads)
