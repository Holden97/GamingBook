# Struct

## struct和class的区别

struct是值类型，创建一个struct类型的实例被分配在栈上。

class是引用类型，创建一个class类型实例被分配在托管堆上。

## Struct在某些情况下不能赋值

值类型默认是按值传递的，当要返回一个Struct时，返回的是栈上一份临时的、本地的拷贝，我们暂且称之为temp\_rect， 而例如draw.MyRect.Width = 20 这样的赋值语句等同于temp\_rect.Width = 20； 因此即使能对它进行修改也无法反映到draw.MyRect本身(即\_rect)的，即这是一个毫无何意义的操作，因此编译器就从源头上禁止了这样的操作。

**struct在foreach迭代的时候不能够单独改其中的某一个变量的值，但class可以。**

例如：

```
    foreach (var item in rollInfo)
    {
        if (item.buffId == buffID)
        {
            //当rollInfo是struct，那么它按值传递，在栈中创建一份拷贝，item不会是原先rollinfo
            //所以rollInfo如果是struct，则此句会报错，但如果为class，则传递同一份引用，不会报错。
            item.buffCurLevel++;
        }
    }
```

## 修改结构体

在结构体的无参方法中修改结构体的字段会创建一个新的结构体实例，原始结构体的字段值不会被修改。结构体是值类型，这意味着每次修改结构体的字段时，都会创建一个新的副本，而不会影响原始结构体的值。

如果你希望修改结构体的字段并将更改保存下来，可以考虑使用引用类型，例如类（class），因为类是引用类型，可以在方法中修改字段并保留更改。

示例：

```csharp
csharpCopy codepublic struct MyStruct
{
    public int Value;
    
    public MyStruct(int value)
    {
        Value = value;
    }
    
    public void ModifyValue(int newValue)
    {
        // 这里创建了一个新的结构体实例来保存修改后的值
        Value = newValue;
    }
}

public class MyClass
{
    public int Value;
    
    public MyClass(int value)
    {
        Value = value;
    }
    
    public void ModifyValue(int newValue)
    {
        // 这里直接修改了类的字段值
        Value = newValue;
    }
}
```

在这个示例中，`MyStruct` 结构体的 `ModifyValue` 方法会创建一个新的结构体实例，而 `MyClass` 类的 `ModifyValue` 方法会直接修改类的字段值。所以，在使用结构体时要小心，确保理解它们的值类型特性。

慎用List\<T>返回的结构体

假设

```
public struct SingleWaveEnemiesInfo
{
    public List<EnemySpawnInfo> enemies;
    public int duration;
}

//声明
        public List<SingleWaveEnemiesInfo> detectors;
//那么以下写法是错误的
        detectors[i].duration = 2;
//报错为
//无法修改"List<SingleWaveEnemiesInfo>.this[int]"的返回值，因为它不是变量
```

目前我的理解是，List的get索引器在返回值值类型数据时，按值传递，故返回的是对应位置数据的副本，所以只能用而无法修改数据。

## 参考资料

1. C# struct的陷阱：无法修改“...”的返回值，因为它不是变量 [https://blog.csdn.net/onlyou930/article/details/5568319](https://blog.csdn.net/onlyou930/article/details/5568319)
2. [https://discussions.unity.com/t/where-to-use-structs-and-classes/111008/3](https://discussions.unity.com/t/where-to-use-structs-and-classes/111008/3)
3. [https://www.cnblogs.com/tonney/archive/2011/04/28/2032205.html](https://www.cnblogs.com/tonney/archive/2011/04/28/2032205.html)
4. [https://blog.csdn.net/liulong1567/article/details/50678930](https://blog.csdn.net/liulong1567/article/details/50678930)
