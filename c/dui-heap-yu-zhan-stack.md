# 堆(heap)与栈(stack)

## 概念

### 栈(stack)

栈的两个目的

1. 追踪方法调用状态
2. 保存局部变量（local variable）的值。

### 栈帧（stack frame）

存储方法中包含的本地变量，方法结束执行后，对应的栈帧就会从栈中弹出。

## C#中的常见值类型/引用

值类型包括C#的基本类型（用关键字int、char、float等来声明），结构（用struct关键字声明的类型），枚举（用enum关键字声明的类型）。

而引用类型包括类（用class关键字声明的类型），数组（array），字符串（string），和委托（用delegate关键字声明的特殊类）。

## 堆与栈存储的一般规律

1. 方法中的局部变量存储在栈中，包含了引用类型的引用和值类型的值。
2. 引用类型的对象的引用地址在栈中，但实例总是在堆中。
3. **引用类型变量中的变量**也跟随父变量存储在堆中。
4. **值类型变量中的变量A**会和**父变量B**本身一同存储在B的上下文中。\
   如果B在**方法**中声明，那么A也会在栈中；\
   如果B在**类**中声明，那么A也在堆中。

## 例外

1. 静态变量总是在堆中（不一定在“主堆”（the managed heap）中），且堆块（heap block）与静态变量一一对应。
2.  匿名方法与委托\
    匿名函数中，你可以使用包含该匿名函数的方法的局部变量，编译器会生成一个类型，该类型包含匿名方法所需的所有父方法中的局部变量。并在父方法的调用过程中生成该类型的实例，复制所有需要的变量到栈中，匿名方法的局部变量在栈中。\


    下列规则适用于 lambda 表达式中的变量范围：

    * 捕获的变量将不会被作为垃圾回收，直至引用变量的委托符合垃圾回收的条件。
    * 在封闭方法中看不到 Lambda 表达式内引入的变量。
    * Lambda 表达式无法从封闭方法中直接捕获 [in](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/in-parameter-modifier)、[ref](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/ref) 或 [out](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/out-parameter-modifier) 参数。
    * lambda 表达式中的 [return](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/statements/jump-statements#the-return-statement) 语句不会导致封闭方法返回。
    * 如果相应跳转语句的目标位于 Lambda 表达式块之外，Lambda 表达式不得包含 [goto](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/statements/jump-statements#the-goto-statement)、[break](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/statements/jump-statements#the-break-statement) 或 [continue](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/statements/jump-statements#the-continue-statement) 语句。 同样，如果目标在块内部，在 lambda 表达式块外部使用跳转语句也是错误的。
3. 异步方法与迭代器\
   异步方法与迭代器的栈帧在方法返回时不能立刻释放，因为异步方法和迭代器在返回后可能仍在执行。

## 参考资料

1. [https://endjin.com/blog/2022/07/understanding-the-stack-and-heap-in-csharp-dotnet](https://endjin.com/blog/2022/07/understanding-the-stack-and-heap-in-csharp-dotnet)
2. [https://en.citizendium.org/wiki/Stack\_frame](https://en.citizendium.org/wiki/Stack\_frame) stack frame
3. [https://learn.microsoft.com/zh-cn/dotnet/api/system.diagnostics.stackframe?view=net-7.0](https://learn.microsoft.com/zh-cn/dotnet/api/system.diagnostics.stackframe?view=net-7.0)
4. 匿名方法与捕获变量 [https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/operators/lambda-expressions](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/operators/lambda-expressions)
