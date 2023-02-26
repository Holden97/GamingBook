# GameObject

## 一、GameObject不支持?.和??运算符

## 深入 UnityEngine.Object 中重载的几个运算符

2020-05-06

## 前言 <a href="#qian-yan" id="qian-yan"></a>

`UnityEngine.Object` 类作为 Unity 中所有内建对象的基类，可以在 Unity 中被任意引用。继承自 `System.Object`，并且重载了 `==`、`!=` 和 `bool` 几个特殊的操作符。

## 问题 <a href="#wen-ti" id="wen-ti"></a>

问题从一行错误日志开始。在最近的一个线上项目中收到这样一条错误上报:

> MissingReferenceException: The object of type 'GameObject' has been destroyed but you are still trying to access it. Your script should either check if it is null or you should not destroy the object.

定位到具体代码，大致如下:

| <pre><code>1Transform transform = gameObject?.transform;
</code></pre> |
| ---------------------------------------------------------------------- |

当前 GameObject 已经被销毁仍然尝试读取其 transform，但是这里明明已经使用 `?.` 操作符判定了，为什么还会出现这个问题？下面尝试模拟这种情况。

| <pre><code>GameObject gameObject = new GameObject("go");DestroyImmediate(gameObject);Transform transform = gameObject?.transform;
</code></pre> |
| ----------------------------------------------------------------------------------------------------------------------------------------------- |

运行上面的代码，确实抛出了一样的错误异常。难道是 `?.` 操作符的问题？下面我们换一种方式来 check null，如下:

| <pre><code>GameObject gameObject = new GameObject("go");DestroyImmediate(gameObject);Transform transform;if (gameObject != null){    transform = gameObject.transform;}
</code></pre> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

运行这一段代码，没有错误异常抛出，说明 check null 成功。为什么 `?.` 操作符会检验失败？带着疑问来深入看一下编译器为我们生成的部分 CIL 代码。

## 探究 <a href="#tan-jiu" id="tan-jiu"></a>

在探究之前，首先补充一段关于 UnityEngine.Object 知识。在 Unity 中，所有实际的 UnityEngine.Object 的数据均存储在一段本地原生的对象空间中，与 CLR 无关；在 CLR 层中的 UnityEngine.Object 对象实际上是一个指向本地原生对象的指针，这类对象又被称为 "wrapper objects"。

原生对象的生命周期由 Unity 管理，在加载新场景、显式调用 `Destroy` 方法(当前帧的某个时刻执行)或调用 `DestroyImmediate` 方法(立即执行)时原生对象会被销毁。C# 中的 UnityEngine.Object 对象的销毁回收由 CLR 决定。因此就可能出现一种情况: 本地原生对象已经被销毁，但是由于 CLR GC 未发生，导致 C# 中的 UnityEngine.Object 对象未被回收。

了解了这个知识点，下面开始探究问题所在。

看看第一段使用了 `?.` 操作符生成的的 CIL 代码指令:

| <pre><code>.locals init (    [0] class [UnityEngine.CoreModule]UnityEngine.GameObject gameObject,    [1] class [UnityEngine.CoreModule]UnityEngine.Transform transform)IL_0001: ldstr "go"IL_0006: newobj instance void [UnityEngine.CoreModule]UnityEngine.GameObject::.ctor(string)IL_000b: stloc.0IL_000c: ldloc.0IL_000d: call void [UnityEngine.CoreModule]UnityEngine.Object::DestroyImmediate(class [UnityEngine.CoreModule]UnityEngine.Object)// check null 部分指令IL_0013: ldloc.0IL_0014: brtrue.s IL_0019IL_0016: ldnullIL_0017: br.s IL_001fIL_0019: ldloc.0IL_001a: call instance class [UnityEngine.CoreModule]UnityEngine.Transform [UnityEngine.CoreModule]UnityEngine.GameObject::get_transform()IL_001f: stloc.1IL_0020: ret
</code></pre> |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

上面生成的 CIL 指令，从 `IL_0001` 到 `IL_000d` 主要是生成新的 GameObject(存在于 Managed Heap 中) 并将其引用存入 Record frame 的第 0 个变量处，然后调用 `DestroyImmediate` 方法销毁这个 GameObject。

接着就是 `?.` 操作符部分的 CIL 指令(从 `IL_0013` 开始)。首先加载当前 Record frame 中的第一个变量值(这里是 GameObject 的引用)到 Evaluation stack 中，使用 `brtrue.s` 判断这个引用的对象是否为空，不为空则跳转到 `IL_0019` 处开始调用 `get_transform` 方法，为空则返回 null。这样看下来 `?.` 操作符就是判断了当前 GameObject 引用对象是否为空，和普通 System.Object 对象 `!= null` 处理类似。

那么绕过空检测出现错误异常的原因也就知道了。当我们调用 `DestroyImmediate` 方法销毁一个 GameObject 对象的时候，此时在底层对应的原生对象是被销毁了，但是其存在于 CLR 层的引用对象(方法结束后对象临时引用变量被释放销毁，存放于 Managed heap 中的 GameObject 对象等待 GC 释放)并未被销毁，若使用 `?.` 操作符在 CLR 层面判断不为空，跳转到 `get_transform` 方法所在指令附近获取 transform，由于底层的 GameObject 已经被销毁所以底层抛出 'MissingReferenceException' 异常。

那么为什么使用常规 `!= null` 形式判空没出现这个问题了，再来看看这种方式生成的 CIL 代码:

| <pre><code>.locals init (    [0] class [UnityEngine.CoreModule]UnityEngine.GameObject gameObject,    [1] class [UnityEngine.CoreModule]UnityEngine.Transform transform,    [2] bool)// 生成 GameObject 以及销毁 GameObject ...// check null 部分指令IL_0013: ldloc.0IL_0014: ldnullIL_0015: call bool [UnityEngine.CoreModule]UnityEngine.Object::op_Inequality(class [UnityEngine.CoreModule]UnityEngine.Object, class [UnityEngine.CoreModule]UnityEngine.Object)IL_001a: stloc.2IL_001b: ldloc.2IL_001c: brfalse.s IL_0027IL_001f: ldloc.0IL_0020: callvirt instance class [UnityEngine.CoreModule]UnityEngine.Transform [UnityEngine.CoreModule]UnityEngine.GameObject::get_transform()IL_0025: stloc.1IL_0027: ret
</code></pre> |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

前面的指令基本都相同，我们主要看看判空部分。首先加载了生成的 GameObject 以及 null，然后在 `IL_0015` 处**调用了 UnityEngine.Object 类重载的 `op_Inequality` 操作符(operator !=)判断 GameObject 是否不为空**，如果判定为空则跳转到 `IL_0027` 指令结束方法，否则从 `IL_001a` 处继续执行指令获取 transform。下面就来重点解读 UnityEngine.Object 重载的 `!=` 操作符的实现部分。

首先看看源码:

| <pre><code>public static bool operator !=(Object x, Object y){        return !Object.CompareBaseObjects(x, y);}
</code></pre> |
| ----------------------------------------------------------------------------------------------------------------------------- |

具体的实现在 `CompareBaseObjects` 方法:

| <pre><code>private static bool CompareBaseObjects(Object lhs, Object rhs){    bool flag1 = (object) lhs == null;    bool flag2 = (object) rhs == null;    if (flag2 &#x26;&#x26; flag1)        return true;    if (flag2)        return !Object.IsNativeObjectAlive(lhs);    if (flag1)        return !Object.IsNativeObjectAlive(rhs);    return lhs.m_InstanceID == rhs.m_InstanceID;}
</code></pre> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

上面的方法用来比较两个 UnityEngine.Object 对象是否相等，lhs 和 rhs 分别代表操作符两边的参数:

* 若两边参数在 CLR 层均为 null，判定相等；
* 若右边参数在 CLR 层为 null，左边不为 null，根据左边参数 CLR 层所在对象对应的底层原生对象是否 Alive 返回比较结果；
* 若左边参数在 CLR 层为 null，右边不为 null，根据右边参数 CLR 层所在对象对应的底层原生对象是否 Alive 返回比较结果；
* 否则左右两边对象在 CLR 层对象均不为 null，比较它们的 `m_InstanceID` 是否相等。

在我们的测试代码中分别是 GameObject 对象和 null。通过前面的分析指导，调用 `DestroyImmediate` 方法销毁一个 GameObject 对象时仅底层对应的原生对象是被销毁了，但是其存在于 CLR 层的引用对象未被销毁；所以上面方法中 `flag1` 为 false，`flag2` 为 true，最终返回结果由 UnityEngine.Object 类的 `IsNativeObjectAlive` 方法决定，这个方法正是判断 CLR 层所在 GameObject 对应的原生对象是否处于 Alive 状态。

在测试代码中，调用了 `DestroyImmediate` 方法销毁了原生的对象，最终 `CompareBaseObjects` 方法返回 true，重载的 `!=` 操作符返回 false。因此 `get_transform` 方法所在的指令不会被调用，从而不会出现错误异常。

## != 和 bool 操作符 <a href="#he-bool-cao-zuo-fu" id="he-bool-cao-zuo-fu"></a>

`!=` 操作符同重载的 `==` 操作符逻辑刚好相反；`bool` 操作符具体实现也是调用 `CompareBaseObjects` 方法:

| <pre><code>public static implicit operator bool(Object exists){    return !Object.CompareBaseObjects(exists, (Object) null);}
</code></pre> |
| ------------------------------------------------------------------------------------------------------------------------------------------- |

从上面源码可以看出 `bool` 操作符内部同样使用了 `CompareBaseObjects` 方法将判定对象与 null 比较，如果判定对象为 null 返回 false，否则返回 true。

## 总结 <a href="#zong-jie" id="zong-jie"></a>

到这里应该清楚某些情况下出现错误异常的原因了，在 [Unity 文档](https://docs.unity3d.com/ScriptReference/Object.html)中关于 Object 有下面这样一句提及:

> This class doesn't support the null-conditional operator (?.) and the null-coalescing operator (??).

UnityEngine.Object 类重载了 `==`、`!=` 以及 `bool` 操作符，对于这几类操作 Unity 会结合 CLR 层的对象以及其底层对应原生对象来得到比较结果。

所以对于 UnityEngine.Object 类(子孙类)类型相关变量使用 `?.` 和 `??` 操作符一定要谨慎，有时显式的使用 `null` 判断或 `bool` 重载符对 UnityEngine.Object 往往更“安全”。

## 关于 MonoBehaviour 可序列化的域 <a href="#guan-yu-monobehaviour-ke-xu-lie-hua-de-yu" id="guan-yu-monobehaviour-ke-xu-lie-hua-de-yu"></a>

对于 MonoBehaviour 可序列化的域，在 Editor 模式下就算这些域没有真正的被“赋值”，Unity 默认也会为其 CLR 层所在的 GameObject 对象默认设置上 “fake null” object (底层原生对象不会被赋值)，通过这样的小 track Unity 能够为开发者调试提供更多的有用信息。

## 二、关于属性

Update当中使用gameObect.transform要注意什么？

~~例如，transform是GameObject对象的一个属性，如果要在update中调用，那么属性的计算会比本地缓存消耗更多的性能，所以在可能的情况下，应该使用缓存来提高性能，用空间换时间。~~

transform缓存无作用，缓存后和gameObject.Transform指向的是两个变量，没有关联。

## 参考资料 <a href="#can-kao" id="can-kao"></a>

* [Possible unintended bypass of lifetime check of underlying Unity engine object](https://github.com/JetBrains/resharper-unity/wiki/Possible-unintended-bypass-of-lifetime-check-of-underlying-Unity-engine-object)
* [Custom == operator, should we keep it?](https://blogs.unity3d.com/2014/05/16/custom-operator-should-we-keep-it/)
* [https://blog.lujun.co/2020/05/06/unity\_object\_operator/](https://blog.lujun.co/2020/05/06/unity\_object\_operator/)
