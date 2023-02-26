# Struct

## struct和class的区别

struct是值类型，创建一个struct类型的实例被分配在栈上。

class是引用类型，创建一个class类型实例被分配在托管堆上。

## Struct不能赋值

值类型默认是按值传递的，当要返回一个Struct时，返回的是栈上一份临时的、本地的拷贝，我们暂且称之为temp\_rect， 而例如draw.MyRect.Width = 20 这样的赋值语句等同于temp\_rect.Width = 20； 因此即使能对它进行修改也无法反映到draw.MyRect本身(即\_rect)的，即这是一个毫无何意义的操作，因此编译器就从源头上禁止了这样的操作。

struct在foreach迭代的时候不能够单独改其中的某一个变量的值，但class可以。

例如：

```
    foreach (var item in rollInfo)
    {
        if (item.buffId == buffID)
        {
            //rollInfo如果是struct，则此句会报错，但如果为class，则不会报错。
            item.buffCurLevel++;
        }
    }
```

## 参考资料

1. C# struct的陷阱：无法修改“...”的返回值，因为它不是变量 [https://blog.csdn.net/onlyou930/article/details/5568319](https://blog.csdn.net/onlyou930/article/details/5568319)
