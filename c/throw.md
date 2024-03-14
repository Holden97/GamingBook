# throw

使用 `throw` 和 `try` 语句来处理异常。 使用 [`throw` 语句](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/statements/exception-handling-statements#the-throw-statement)引发异常。 使用 [`try` 语句](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/statements/exception-handling-statements#the-try-statement)捕获和处理在执行代码块期间可能发生的异常。

### `throw` 语句 <a href="#the-throw-statement" id="the-throw-statement"></a>

`throw` 语句引发异常：

C#复制

```csharp
if (shapeAmount <= 0)
{
    throw new ArgumentOutOfRangeException(nameof(shapeAmount), "Amount of shapes must be positive.");
}
```

在 `throw e;` 语句中，表达式 `e` 的结果必须隐式转换为 [System.Exception](https://learn.microsoft.com/zh-cn/dotnet/api/system.exception)。

可以使用内置异常类，例如 [ArgumentOutOfRangeException](https://learn.microsoft.com/zh-cn/dotnet/api/system.argumentoutofrangeexception) 或 [InvalidOperationException](https://learn.microsoft.com/zh-cn/dotnet/api/system.invalidoperationexception)。 .NET 还提供了在某些情况下引发异常的帮助程序方法：[ArgumentNullException.ThrowIfNull](https://learn.microsoft.com/zh-cn/dotnet/api/system.argumentnullexception.throwifnull) 和 [ArgumentException.ThrowIfNullOrEmpty](https://learn.microsoft.com/zh-cn/dotnet/api/system.argumentexception.throwifnullorempty)。 还可以定义自己的派生自 [System.Exception](https://learn.microsoft.com/zh-cn/dotnet/api/system.exception) 的异常类。 有关详细信息，请参阅[创建和引发异常](https://learn.microsoft.com/zh-cn/dotnet/csharp/fundamentals/exceptions/creating-and-throwing-exceptions)。

在 [`catch` 块](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/statements/exception-handling-statements#the-try-catch-statement)内，可以使用 `throw;` 语句重新引发由 `catch` 块处理的异常：

C#复制

```csharp
try
{
    ProcessShapes(shapeAmount);
}
catch (Exception e)
{
    LogError(e, "Shape processing failed.");
    throw;
}
```

&#x20;备注

`throw;` 保留异常的原始堆栈跟踪，该跟踪存储在 [Exception.StackTrace](https://learn.microsoft.com/zh-cn/dotnet/api/system.exception.stacktrace#system-exception-stacktrace) 属性中。 与此相反，`throw e;` 更新 `e` 的 [StackTrace](https://learn.microsoft.com/zh-cn/dotnet/api/system.exception.stacktrace#system-exception-stacktrace) 属性。

引发异常时，公共语言运行时 (CLR) 将查找可以处理此异常的 [`catch` 块](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/statements/exception-handling-statements#the-try-catch-statement)。 如果当前执行的方法不包含此类 `catch` 块，则 CLR 查看调用了当前方法的方法，并以此类推遍历调用堆栈。 如果未找到 `catch` 块，CLR 将终止正在执行的线程。 有关详细信息，请参阅 [C# 语言规范](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/language-specification/readme)的[如何处理异常](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/language-specification/exceptions#214-how-exceptions-are-handled)部分。

#### `throw` 表达式 <a href="#the-throw-expression" id="the-throw-expression"></a>

还可以将 `throw` 用作表达式。 这在很多情况下可能很方便，包括：

*   [条件运算符](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/operators/conditional-operator)。 以下示例使用 `throw` 表达式在传递的数组 `args` 为空时引发 [ArgumentException](https://learn.microsoft.com/zh-cn/dotnet/api/system.argumentexception)：

    C#复制

    ```csharp
    string first = args.Length >= 1 
        ? args[0]
        : throw new ArgumentException("Please supply at least one argument.");
    ```
*   [null 合并运算符](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/operators/null-coalescing-operator)。 以下示例使用 `throw` 表达式在要分配给属性的字符串为 `null` 时引发 [ArgumentNullException](https://learn.microsoft.com/zh-cn/dotnet/api/system.argumentnullexception)：

    C#复制

    ```csharp
    public string Name
    {
        get => name;
        set => name = value ??
            throw new ArgumentNullException(paramName: nameof(value), message: "Name cannot be null");
    }
    ```
*   expression-bodied [lambda](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/operators/lambda-expressions) 或方法。 以下示例使用 `throw` 表达式引发 [InvalidCastException](https://learn.microsoft.com/zh-cn/dotnet/api/system.invalidcastexception)，以指示不支持转换为 [DateTime](https://learn.microsoft.com/zh-cn/dotnet/api/system.datetime) 值：

    C#复制

    ```csharp
    DateTime ToDateTime(IFormatProvider provider) =>
             throw new InvalidCastException("Conversion to a DateTime is not supported.");
    ```

### `try` 语句 <a href="#the-try-statement" id="the-try-statement"></a>

可以通过以下任何形式使用 `try` 语句：[`try-catch`](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/statements/exception-handling-statements#the-try-catch-statement) - 处理在 `try` 块内执行代码期间可能发生的异常，[`try-finally`](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/statements/exception-handling-statements#the-try-finally-statement) - 指定在控件离开 `try` 块时执行的代码，以及 [`try-catch-finally`](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/statements/exception-handling-statements#the-try-catch-finally-statement) - 作为上述两种形式的组合。

#### `try-catch` 语句 <a href="#the-try-catch-statement" id="the-try-catch-statement"></a>

使用 `try-catch` 语句处理在执行代码块期间可能发生的异常。 将代码置于 `try` 块中可能发生异常的位置。 使用 _catch 子句_指定要在相应的 `catch` 块中处理的异常的基类型：

C#复制

```csharp
try
{
    var result = Process(-3, 4);
    Console.WriteLine($"Processing succeeded: {result}");
}
catch (ArgumentException e)
{
    Console.WriteLine($"Processing failed: {e.Message}");
}
```

可以提供多个 catch 子句：

C#复制

```csharp
try
{
    var result = await ProcessAsync(-3, 4, cancellationToken);
    Console.WriteLine($"Processing succeeded: {result}");
}
catch (ArgumentException e)
{
    Console.WriteLine($"Processing failed: {e.Message}");
}
catch (OperationCanceledException)
{
    Console.WriteLine("Processing is cancelled.");
}
```

发生异常时，将从上到下按指定顺序检查 catch 子句。 对于任何引发的异常，最多只执行一个 `catch` 块。 如前面的示例所示，可以省略异常变量的声明，并在 catch 子句中仅指定异常类型。 没有任何指定异常类型的 catch 子句与任何异常匹配，如果存在，则必须是最后一个 catch 子句。

如果要重新引发捕获的异常，请使用 [`throw` 语句](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/statements/exception-handling-statements#the-throw-statement)，如以下示例所示：

C#复制

```csharp
try
{
    var result = Process(-3, 4);
    Console.WriteLine($"Processing succeeded: {result}");
}
catch (Exception e)
{
    LogError(e, "Processing failed.");
    throw;
}
```

&#x20;备注

`throw;` 保留异常的原始堆栈跟踪，该跟踪存储在 [Exception.StackTrace](https://learn.microsoft.com/zh-cn/dotnet/api/system.exception.stacktrace#system-exception-stacktrace) 属性中。 与此相反，`throw e;` 更新 `e` 的 [StackTrace](https://learn.microsoft.com/zh-cn/dotnet/api/system.exception.stacktrace#system-exception-stacktrace) 属性。

**`when` 异常筛选器**

除了异常类型之外，还可以指定异常筛选器，该筛选器进一步检查异常并确定相应的 `catch` 块是否处理该异常。 异常筛选器是遵循 `when` 关键字的布尔表达式，如以下示例所示：

C#复制

```csharp
try
{
    var result = Process(-3, 4);
    Console.WriteLine($"Processing succeeded: {result}");
}
catch (Exception e) when (e is ArgumentException || e is DivideByZeroException)
{
    Console.WriteLine($"Processing failed: {e.Message}");
}
```

前面的示例使用异常筛选器提供单个 `catch` 块来处理两个指定类型的异常。

可以为相同异常类型提供若干 `catch` 子句，如果它们通过异常筛选器区分。 其中一个子句可能没有异常筛选器。 如果存在此类子句，则它必须是指定该异常类型的最后一个子句。

如果 `catch` 子句具有异常筛选器，则可以指定与 `catch` 子句之后出现的异常类型相同或小于派生的异常类型。 例如，如果存在异常筛选器，则 `catch (Exception e)` 子句不需要是最后一个子句。

**异步和迭代器方法中的异常**

如果[异步函数](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/async)中发生异常，则[等待](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/operators/await)函数的结果时，它会传播到函数的调用方，如以下示例所示：

C#复制

```csharp
public static async Task Run()
{
    try
    {
        Task<int> processing = ProcessAsync(-1);
        Console.WriteLine("Launched processing.");

        int result = await processing;
        Console.WriteLine($"Result: {result}.");
    }
    catch (ArgumentException e)
    {
        Console.WriteLine($"Processing failed: {e.Message}");
    }
    // Output:
    // Launched processing.
    // Processing failed: Input must be non-negative. (Parameter 'input')
}

private static async Task<int> ProcessAsync(int input)
{
    if (input < 0)
    {
        throw new ArgumentOutOfRangeException(nameof(input), "Input must be non-negative.");
    }

    await Task.Delay(500);
    return input;
}
```

如果[迭代器方法](https://learn.microsoft.com/zh-cn/dotnet/csharp/iterators)中发生异常，则仅当迭代器前进到下一个元素时，它才会传播到调用方。

#### `try-finally` 语句 <a href="#the-try-finally-statement" id="the-try-finally-statement"></a>

在 `try-finally` 语句中，当控件离开 `try` 块时，将执行 `finally` 块。 控件可能会离开 `try` 块，因为

* 正常执行，
* 执行 [jump 语句](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/statements/jump-statements)（即 `return`、`break`、`continue` 或 `goto`），或
* 从 `try` 块中传播异常。

以下示例使用 `finally` 块在控件离开方法之前重置对象的状态：

C#复制

```csharp
public async Task HandleRequest(int itemId, CancellationToken ct)
{
    Busy = true;

    try
    {
        await ProcessAsync(itemId, ct);
    }
    finally
    {
        Busy = false;
    }
}
```

还可以使用 `finally` 块来清理 `try` 块中使用的已分配资源。

&#x20;备注

当资源类型实现 [IDisposable](https://learn.microsoft.com/zh-cn/dotnet/api/system.idisposable) 或 [IAsyncDisposable](https://learn.microsoft.com/zh-cn/dotnet/api/system.iasyncdisposable) 接口时，请考虑 [`using` 语句](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/statements/using)。 `using` 语句可确保在控件离开 `using` 语句时释放获取的资源。 编译器将 `using` 语句转换为 `try-finally` 语句。

在几乎所有情况下，都会执行 `finally` 块。 未执行 `finally` 块的唯一情况涉及立即终止程序。 例如，由于 [Environment.FailFast](https://learn.microsoft.com/zh-cn/dotnet/api/system.environment.failfast) 调用或 [OverflowException](https://learn.microsoft.com/zh-cn/dotnet/api/system.overflowexception) 或 [InvalidProgramException](https://learn.microsoft.com/zh-cn/dotnet/api/system.invalidprogramexception) 异常，可能会发生此类终止。 大多数操作系统在停止和卸载进程的过程中执行合理的资源清理。

#### `try-catch-finally` 语句 <a href="#the-try-catch-finally-statement" id="the-try-catch-finally-statement"></a>

使用 `try-catch-finally` 语句来处理在执行 `try` 块期间可能发生的异常，并指定在控件离开 `try` 语句时必须执行的代码：

C#复制

```csharp
public async Task ProcessRequest(int itemId, CancellationToken ct)
{
    Busy = true;

    try
    {
        await ProcessAsync(itemId, ct);
    }
    catch (Exception e) when (e is not OperationCanceledException)
    {
        LogError(e, $"Failed to process request for item ID {itemId}.");
        throw;
    }
    finally
    {
        Busy = false;
    }

}
```

当 `catch` 块处理异常时，`finally` 块在执行该 `catch` 块后执行（即使执行 `catch` 块期间发生另一个异常）。 有关 `catch` 和 `finally` 块的信息，请分别参阅 [`try-catch` 语句](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/statements/exception-handling-statements#the-try-catch-statement)和 [`try-finally` 语句](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/statements/exception-handling-statements#the-try-finally-statement) 部分。



参考

1. [https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/statements/exception-handling-statements#the-throw-statement](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/statements/exception-handling-statements#the-throw-statement)
