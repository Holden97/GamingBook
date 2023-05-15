# Task

在 C# 中，可以使用 `Task` 类型和 `await` 关键字来执行异步操作。以下是一个简单的示例：

```
csharp复制代码async Task<int> MyAsyncMethod()
{
    // 等待一个异步操作完成
    await Task.Delay(1000);

    // 返回一个整数值
    return 42;
}
```

在上面的代码中，`MyAsyncMethod()` 是一个异步方法，它使用 `await` 关键字等待一个 `Task.Delay()` 方法完成，该方法模拟了一个异步操作。当异步操作完成时，方法将会返回一个整数值。

要调用这个异步方法并获取其返回值，可以使用如下代码：

```
csharp复制代码int result = await MyAsyncMethod();
```

在上面的代码中，`await MyAsyncMethod()` 表达式将会等待异步方法完成，并返回其结果。由于 `MyAsyncMethod()` 返回一个整数值，因此可以将其赋给一个整数类型的变量 `result`。

需要注意的是，异步方法必须标记为 `async`，并且返回值类型必须是 `Task` 或者 `Task<TResult>` 才能使用 `await` 关键字。另外，`await` 关键字只能在异步方法中使用。
