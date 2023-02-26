# 关键字

## [c#中@开头的变量名](https://www.cnblogs.com/friedRib/p/7814052.html)

在 C#  规范中, @  可以作为标识符（类名、变量名、方法名等）的第一个字符，以允许C# 中保留关键字作为自己定义的标识符。\
如

![复制代码](https://common.cnblogs.com/images/copycode.gif)

```
class @class
{
   public static void @static(bool @bool) {
      if (@bool)
         System.Console.WriteLine("true");
      else
         System.Console.WriteLine("false");
   }   
}
```

![复制代码](https://common.cnblogs.com/images/copycode.gif)

&#x20;

注意，@ 虽然出现在标识符中，但不作为标识符本身的一部分。\
因此，以上示例，定义了一个名为 class 的类，并包含一个名为 static 的方法，以及一个参数名为了 bool 的形参。\
\
这样，对于跨语言的移植带来了便利。因为，某个单词在 C#  中作为保留关键字，但是在其他语言中也许不是。
