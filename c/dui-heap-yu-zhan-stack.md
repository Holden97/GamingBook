# 堆(heap)与栈(stack)

## C#中的常见值类型/引用

值类型包括C#的基本类型（用关键字int、char、float等来声明），结构（用struct关键字声明的类型），枚举（用enum关键字声明的类型）。

而引用类型包括类（用class关键字声明的类型），数组（array），字符串（string），和委托（用delegate关键字声明的特殊类）。

## 堆与栈存储的一般规律

1. 方法中的本地变量存储在栈中，包含了引用类型的引用和值类型的值。
2. 引用类型的对象通常在堆中。
3. 引用类型变量中的变量一般也存储在堆中。
4. 值类型变量中的变量会和值类型变量本身一同存储在值类型变量的上下文中。如果值类型变量在方法中声明，那么该变量也会在栈中；如果值类型变量在类中声明，那么它也在堆中。

## 参考资料

1. [https://endjin.com/blog/2022/07/understanding-the-stack-and-heap-in-csharp-dotnet](https://endjin.com/blog/2022/07/understanding-the-stack-and-heap-in-csharp-dotnet)
