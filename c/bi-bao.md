# 闭包

闭包的概念：内层函数引用的外层函数的变量的最终值。

一个典型的例子是，当函数中使用匿名函数，匿名函数还需要引用当前函数的遍历变量时，就会出现多次引用都是最终值的问题。

```
        for (int i = 0; i < Buttons.value.Count; i++)
        {
            int index = i;
            Buttons.value[i].onClick.AddListener(() =>
            {
                nowButton = Buttons.value[index];
                OutputLogic.Call(new Flow());
            });
        }
```

i的最终值是Buttons.value.Count-1，而index的最终值是循环到当前的i。



在 C# 中，闭包是一个非常强大的功能，但它有时会导致一些意想不到的问题。闭包问题通常在涉及到匿名方法、lambda 表达式或局部函数时出现，尤其是在循环中使用时。

#### 闭包问题的产生

闭包问题通常是由于变量捕获导致的。在 C# 中，闭包会捕获外部变量的引用，而不是值。因此，如果在循环中使用闭包，闭包会捕获同一个变量，而不是每次迭代的不同值。这导致了在循环结束后，所有闭包都会引用循环的最终值。

#### 示例

以下是一个典型的闭包问题示例：

```csharp
using System;

public class ClosureExample
{
    public static void Main(string[] args)
    {
        Action[] actions = new Action[3];

        for (int i = 0; i < 3; i++)
        {
            actions[i] = () => Console.WriteLine(i);
        }

        foreach (var action in actions)
        {
            action();
        }
    }
}
```

你可能期望输出：

```
0
1
2
```

但实际输出是：

```
3
3
3
```

#### 解释

在上面的代码中，`i` 是在循环外部声明的局部变量。匿名方法（lambda 表达式）捕获的是这个变量的引用，而不是每次循环迭代时的值。因此，当循环结束时，`i` 的值已经是 3，所以所有闭包都会打印 3。

#### 解决方法

解决这个问题的方法是将循环变量的值拷贝到一个新的局部变量中，使得每个闭包捕获的是不同的变量引用。

**方法一：使用局部变量**

```csharp
using System;

public class ClosureExample
{
    public static void Main(string[] args)
    {
        Action[] actions = new Action[3];

        for (int i = 0; i < 3; i++)
        {
            int capturedValue = i;
            actions[i] = () => Console.WriteLine(capturedValue);
        }

        foreach (var action in actions)
        {
            action();
        }
    }
}
```

现在，输出将是：

```
0
1
2
```

**方法二：使用 `foreach` 循环**

另一种解决方法是使用 `foreach` 循环，因为 `foreach` 循环中的迭代变量在每次循环迭代中都是新的变量。

```csharp
using System;

public class ClosureExample
{
    public static void Main(string[] args)
    {
        Action[] actions = new Action[3];
        int[] values = { 0, 1, 2 };

        int index = 0;
        foreach (var value in values)
        {
            actions[index] = () => Console.WriteLine(value);
            index++;
        }

        foreach (var action in actions)
        {
            action();
        }
    }
}
```

#### 总结

闭包问题在 C# 中主要是由于变量捕获的引用而不是值导致的。通过在循环内创建一个新的局部变量，或使用 `foreach` 循环，可以解决这个问题。理解闭包如何捕获变量对于避免意外行为和调试代码是非常重要的。

参考资料

1.[https://gwb.tencent.com/community/detail/127827](https://gwb.tencent.com/community/detail/127827)
