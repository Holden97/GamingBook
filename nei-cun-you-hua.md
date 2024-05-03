# 内存优化

## Unity3d 性能优化(GC篇)

[HOME](https://peakcoder.com/)[DAILY NOTES](https://peakcoder.com/daily/)[ABOUT](https://peakcoder.com/about/)[INDIE GAMES](https://peakcoder.com/indie%20games/)[GUESTBOOK](https://peakcoder.com/guestbook/)[CATEGORIES](https://peakcoder.com/categories/)[TAGS](https://peakcoder.com/tags/)[SUBSCRIBE](https://peakcoder.com/feed/)

**性能允许的情况下，应该打开deep profile。**

使用Profiler的时候经常会看到一些GC Alloc，GC Alloc多了会影响帧数。

产生GC的本质是在heap上频繁申请内存，比如在Update()中使用new 关键字实例化reference type 对象，而像Vector3 struct 这类 value type在栈上分配内存，则不会产生GC。 但有时候在一个脚本中并没有使用任何一个 new 关键字，也会产生GC。以下几条guidelines能够帮助消除游戏中产生的不必要的GC。

**1.避免使用 foreach**

使用foreach的时候会隐式调用 GetEnumerator()，在heap上生成一个enumerator，导致GC。 建议使用 for 和 while来做循环迭代或者显式调用GetEnumerator()，代码如下：

```csharp
var etor = mDictionary.GetEnumerator();
while(etor.MoveNext())
{
    // ...
}
```

foreach的使用起来比较明晰和简便，可以在非 Update、LateUpdate、FixedUpate 调用频率高的场合使用。

**2.避免使用Update中使用string**

C# 里的string是immutable的，会在heap上分配内存。在Update里操作string 会造成很大的GC。所以不要做在Update里给一个Label赋值这种事。

**3.使用struct或者enum作为dictionary key会分配内存**

使用struct或enum做key的dictionary，在使用TryGetValue()或者键值访问时，会分配内存。 以下方法可以避免这种情况：

1\) 自定义的struct作key，struct需要实现 IEquatable接口

2\) enum 作key麻烦一些 申明一个Comparer的struct

```csharp
public struct BuffStatusComparer : IEqualityComparer<BuffStatus>
{
    public bool Equals(BuffStatus x, BuffStatus y)
    {
        return x == y;
    }

    public int GetHashCode(BuffStatus obj)
    {
        // you need to do some thinking here,
        return (int)obj;
    }
}
```

初始化字典

```csharp
HitShipBuffDict = new Dictionary<BuffStatus, List<BuffStatusInfo>>(new BuffStatusComparer());
```

**4.避免Update中new 引用对象**

Update里不能new 引用类型。Vector3、Quaternion、Struct这种值类型可以。堆上分配内存的都不行。

**5.避免高频率使用playerprefs的set get**

Playerprefs set 和get 是文件读写操作，高频操作会影响性能。这个可能不会产生GC，但很耗CPU.

**6.不要使用NGUI的Property Binding**

不要使用NGUI的property binding，她的内部使用反射机制实现的，效率极差。

**7.不要忘记取消注册的事件**

基本上忘记卸载注册的事件会导致内存泄漏，使用NotificationCenter注册的事件也要记得remove

**8.不使用LINQ命令**

因为它们一般会分配中间缓存，而这很容易生成垃圾内存。

**9.不要直接访问gameobject的tag属性**

比如if (go.tag == “human”)最好换成if (go.CompareTag (“human”))。因为访问物体的tag属性会在堆上额外的分配空间。

**10.留意get属性器内部的内存分配情况。**



bool -> System.Boolean (布尔型，其值为 true 或者 false)，1字节[^1]

byte -> System.Byte (字节型， 1 字节，表示 8 位正整数，范围 0 \~ 255)

sbyte -> System.SByte (带符号字节型， 1 字节，表示 8 位整数，范围 -128 \~ 127)

char -> System.Char (字符型，2字节，表示 1 个 Unicode 字符)

short -> System.Int16 (短整型，2 字节，表示 16 位整数，范围 -32,768 \~ 32,767)

ushort -> System.UInt16 (无符号短整型，2 字节，表示 16 位正整数，范围 0 \~ 65,535)

uint -> System.UInt32 (无符号整型，4 字节，表示 32 位正整数，范围 0 \~ 4,294,967,295)

int -> System.Int32 (整型，4 字节， 32 位整数，范围 -2,147,483,648 到 2,147,483,647)

float -> System.Single (单精度浮点型，4 个字节)

ulong -> System.UInt64 (无符号长整型，8 字节，表示 64 位正整数，范围 0 \~ 大约 10 的 20 次方)

long -> System.Int64 (长整型，8 字节，表示 64 位整数，范围大约 -(10 的 19) 次方 到 10 的 19 次方)

double -> System.Double (双精度浮点型，8 个字节)





参考

1. [https://peakcoder.com/unity3d/2016/12/16/unity3d-gc](https://peakcoder.com/unity3d/2016/12/16/unity3d-gc)
2. [https://stackoverflow.com/questions/28514373/what-is-the-size-of-a-boolean-in-c-does-it-really-take-4-bytes](https://stackoverflow.com/questions/28514373/what-is-the-size-of-a-boolean-in-c-does-it-really-take-4-bytes)

[^1]: [https://stackoverflow.com/questions/28514373/what-is-the-size-of-a-boolean-in-c-does-it-really-take-4-bytes](https://stackoverflow.com/questions/28514373/what-is-the-size-of-a-boolean-in-c-does-it-really-take-4-bytes)
