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

参考

1. [https://gwb.tencent.com/community/detail/115070](https://gwb.tencent.com/community/detail/115070)
2. [https://docs.unity3d.com/cn/2021.3/Manual/class-TagManager.html](https://docs.unity3d.com/cn/2021.3/Manual/class-TagManager.html)
