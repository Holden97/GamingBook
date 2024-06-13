# Tag与Layer

Tags和Layers分别表示是Unity引擎里面的标签和层，他们都是用来对GameObject进行标识的属性，Tags常用于单个GameObject，Layers常用于一组的GameObject。

## **TAG**

tag可以理解为一类元素的标记，如hero、enemy、apple-tree等。通过设置tag，可以方便地通过GameObject.FindWithTag()来寻找对象。GameObject.FindWithTag()只返回一个对象，要想获取多个tag为某值的对象，GameObject.FindGameObjectsWithTag()。

注意:Camera.main 是被标记为“MainCanmera”的相机

## **Layer ——基于layer的渲染和碰撞**

经常使用层的是相机和灯光，相机用它来**渲染**场景中的一部分物体，而灯光依靠层来仅仅照亮场景中的一部分物体。但是也经常利用层，通过光线投射来选择性地忽略**碰撞器**，或者添加碰撞功能。

通过层你可以在特定的层里投射光线、忽略碰撞器。比如说，你只想在玩家层中投射光线(即光线只跟玩家层中的碰撞体作用)，进而忽略其他所有的碰撞器。

在Unity中每个GameObject都有Layer属性，默认的Layer都是Default。在Unity中可编辑的Layer共有24个（8—31层），官方已使用的是0—7层，默认不可编辑！

LayerMask实际上是一个位码操作，在Unity3D中一共有32个Layer层，并且不可增加。

LayMask为-1时代表所有层级。

## 位运算符

按位运算符：`~、|、&、^`。位运算符主要用来对二进制位进行操作。

逻辑运算符：`&&、||、！。`逻辑运算符把语句连接成更复杂的复杂语句。

按位运算符：左移运算符`<<`，左移表示乘以2，左移多少位表示乘以2的几次幂。

举个栗子：

`var temp = 14 << 2;` 表示十进制数14转化为二进制后向左移动2位。

temp最后计算的值为 14乘以2的平方，temp = 56；

同理，右移运算符`>>`，移动多少位表示除以2的几次幂。

具体可以转到博客：[按位运算符](http://www.cnblogs.com/yyangblog/archive/2011/01/14/1935656.html).

上面是个基础知识的补充。

#### 在Unity中使用LayerMask

&#x20;

[如何编辑Layers](http://docs.unity3d.com/Manual/Layers.html).

在代码中使用时如何开启某个Layers？

`LayerMask mask = 1 << 你需要开启的Layers层。`

`LayerMask mask = 0 << 你需要关闭的Layers层。`

举几个个栗子：

> LayerMask mask = 1 << 2; 表示开启Layer2。
>
> LayerMask mask = 0 << 5;表示关闭Layer5。
>
> LayerMask mask = 1<<2|1<<8;表示开启Layer2和Layer8。
>
> LayerMask mask = 0<<3|0<<7;表示关闭Layer3和Layer7。

上面也可以写成：

> LayerMask mask = \~（1<<3|1<<7）;表示关闭Layer3和Layer7。
>
> LayerMask mask = 1<<2|0<<4;表示开启Layer2并且同时关闭Layer4.

代码：

```
using UnityEngine; using System.Collections; public class example : MonoBehaviour {
    LayerMask mask =  ~（1<<3|1<<7）; void Update() { if (Physics.Raycast(transform.position, transform.forward, 100, mask.value))
            Debug.Log("Hit something");
    }
}
```

&#x20;

&#x20;

Physics.Raycast函数里操作一个位掩码，该位掩码的每一位决定相应的层是否被忽略。如果layerMask中的每一位都为1，则将会与所有的碰撞器作用。相反如果layerMask=0,那么将会光线将不会与任何碰撞器作用

```
// bit shift the index of the layer to get a bit mask
var layerMask = 1 << 8;
// Does the ray intersect any objects which are in the player layer.
if (Physics.Raycast (transform.position, Vector3.forward, Mathf.Infinity, layerMask))
    print ("The ray hit the player"); 
```

在现实生活中，会遇到完全相反的情况。如我们只想排除玩家层(Player layer)，即投射出的光线跟除玩家层中的碰撞器外的所有碰撞器作用。

```
function Update () {
  // Bit shift the index of the layer (8) to get a bit mask
  var layerMask = 1 << 8;
  // This would cast rays only against colliders in layer 8.
  // But instead we want to collide against everything except layer 8. The ~ operator does this, it inverts a bitmask.
  layerMask = ~layerMask;
  var hit : RaycastHit;
  // Does the ray intersect any objects excluding the player layer
  if (Physics.Raycast (transform.position, transform.TransformDirection (Vector3.forward), hit, Mathf.Infinity, layerMask)) {
    Debug.DrawRay (transform.position, transform.TransformDirection (Vector3.forward) * hit.distance, Color.yellow);
    print ("Did Hit");
  } else {
    Debug.DrawRay (transform.position, transform.TransformDirection (Vector3.forward) *1000, Color.white);
    print ("Did not Hit");
  }
}
```

当没有给Raycast函数传layerMask参数时，仅仅只忽略使用IgnoreRaycast层的碰撞器。这是投射光线时忽略某些碰撞器的最简单方法。



**使用Physics.Raycast(ray, out var hitInfo, 100, LayerMask.NameToLayer("Ground"));得到的hitinfo.collider.gameobject的层级不在ground层，为什么？ChatGPT**

在 Unity 中，当使用 `Physics.Raycast` 时，第四个参数是一个 `LayerMask`，而不是一个单一的图层索引。因此，如果你直接传递 `LayerMask.NameToLayer("Ground")` 的返回值给 `Physics.Raycast`，你实际上传递的是一个图层索引，这会导致 Raycast 无法正确地识别应该检测的图层。

正确的用法是构建一个 `LayerMask`，包含你想要检测的图层。可以使用 `LayerMask.GetMask` 方法或者通过位操作来构建。

假设Ground的layer是7，那么**LayerMask.NameToLayer("Ground")**得到的结果是7,但是第四个参数需要的值是2^7=128，即使用**LayerMask.GetMask("Ground")**得到的值

#### 正确的使用方法

1. **使用 `LayerMask.GetMask` 方法**：这个方法可以直接根据图层名称创建一个 `LayerMask`。

```csharp
csharp复制代码using UnityEngine;

public class RaycastExample : MonoBehaviour
{
    void Update()
    {
        if (Input.GetMouseButtonDown(0))
        {
            Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
            RaycastHit hitInfo;

            // Use LayerMask.GetMask to create a mask for the "Ground" layer
            int layerMask = LayerMask.GetMask("Ground");

            if (Physics.Raycast(ray, out hitInfo, 100, layerMask))
            {
                Debug.Log("Hit object: " + hitInfo.collider.gameObject.name);
                Debug.Log("Hit object layer: " + LayerMask.LayerToName(hitInfo.collider.gameObject.layer));
            }
        }
    }
}
```

2. **使用位掩码**：如果你需要指定多个图层，可以使用位掩码来组合多个图层。

```csharp
csharp复制代码using UnityEngine;

public class RaycastExample : MonoBehaviour
{
    void Update()
    {
        if (Input.GetMouseButtonDown(0))
        {
            Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
            RaycastHit hitInfo;

            // Create a bitmask that includes the "Ground" layer
            int layerMask = 1 << LayerMask.NameToLayer("Ground");

            if (Physics.Raycast(ray, out hitInfo, 100, layerMask))
            {
                Debug.Log("Hit object: " + hitInfo.collider.gameObject.name);
                Debug.Log("Hit object layer: " + LayerMask.LayerToName(hitInfo.collider.gameObject.layer));
            }
        }
    }
}
```

#### 详细解释

* **`LayerMask.GetMask` 方法**：这个方法接受一个或多个图层名称，并返回一个包含这些图层的 `LayerMask`。它可以处理多个图层名称，并返回一个适合用于 `Physics.Raycast` 的掩码。
* **位操作**：`1 << LayerMask.NameToLayer("Ground")` 通过位移操作创建一个掩码，该掩码仅包含 "Ground" 图层。

#### 常见错误

* **误用图层索引**：直接使用 `LayerMask.NameToLayer` 返回的图层索引作为 `Physics.Raycast` 的图层掩码会导致 Raycast 无法正确工作，因为 `Physics.Raycast` 需要的是一个位掩码，而不是一个单一的图层索引。
* **图层拼写错误**：确保传递给 `LayerMask.GetMask` 和 `LayerMask.NameToLayer` 的图层名称拼写正确，且与 Unity 编辑器中的图层名称完全匹配（区分大小写）。

#### 总结

使用 `Physics.Raycast` 时，确保正确地创建和传递图层掩码。如果需要检测特定图层，请使用 `LayerMask.GetMask` 或通过位操作创建掩码，而不是直接使用图层索引。这样可以确保 Raycast 正确地识别和检测到指定图层上的碰撞体。

参考

1. [https://gwb.tencent.com/community/detail/115070](https://gwb.tencent.com/community/detail/115070)
2. [https://docs.unity3d.com/cn/2021.3/Manual/class-TagManager.html](https://docs.unity3d.com/cn/2021.3/Manual/class-TagManager.html)
