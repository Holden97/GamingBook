# 旋转

1、unity用的是左手坐标系，握拳伸出左手，大拇指指向坐标轴正向、四指方向是旋转的正方向，和大部分系统的右手坐标系是有很大区别的。

2、unity的欧拉角和四元数也是很变态的，违反常规的，欧拉角采用的是yxz顺归，从来没有哪个软件和硬件会采用这种顺序。四元数采用的是xyzw顺序，而标准的教材和文献乃至硬件、软件都是遵循wxyz的顺序

## 一、Rotate

**1.public void Rotate (**[**Vector3**](https://docs.unity3d.com/cn/2020.2/ScriptReference/Vector3.html) **eulers,** [**Space**](https://docs.unity3d.com/cn/2020.2/ScriptReference/Space.html) **relativeTo= Space.Self);**

### 参数

| eulers     | The rotation to apply in euler angles. |
| ---------- | -------------------------------------- |
| relativeTo | 确定在游戏对象本地还是相对于世界空间中的场景来旋转游戏对象。         |

### 描述

应用一个围绕 Z 轴旋转 eulerAngles.z 度、围绕 X 轴旋转 eulerAngles.x 度、围绕 Y 轴旋转 eulerAngles.y 度（按此顺序）的旋转。

旋转采用欧拉角形式的 [Vector3](https://docs.unity3d.com/cn/2020.2/ScriptReference/Vector3.html) 参数。第二个参数是旋转轴，可以将其设置为本地轴 ([Space.Self](https://docs.unity3d.com/cn/2020.2/ScriptReference/Space.Self.html)) 或全局轴 ([Space.World](https://docs.unity3d.com/cn/2020.2/ScriptReference/Space.World.html))。旋转以欧拉角度数表示。

***

public void Rotate (float xAngle, float yAngle, float zAngle, [Space](https://docs.unity3d.com/cn/2020.2/ScriptReference/Space.html) relativeTo= Space.Self);

### 参数

| relativeTo | 确定在游戏对象本地还是相对于世界空间中的/场景/来旋转游戏对象。 |
| ---------- | -------------------------------- |
| xAngle     | 围绕 X 轴旋转游戏对象的度数。                 |
| yAngle     | 围绕 Y 轴旋转游戏对象的度数。                 |
| zAngle     | 围绕 Z 轴旋转游戏对象的度数。                 |

### 描述

此方法的实现应用一个围绕 Z 轴旋转 `zAngle` 度、围绕 X 轴旋转 `xAngle` 度、围绕 Y 轴旋转 `yAngle` 度（按此顺序）的旋转。

旋转可以使用 x、y 和 z 的 3 个浮点数指定欧拉角。\
\
该示例显示两个立方体：一个立方体使用 [Space.Self](https://docs.unity3d.com/cn/2020.2/ScriptReference/Space.Self.html)（[GameObject](https://docs.unity3d.com/cn/2020.2/ScriptReference/GameObject.html) 的本地空间和轴），而另一个立方体使用 [Space.World](https://docs.unity3d.com/cn/2020.2/ScriptReference/Space.World.html)（相对于 `Scene` 的空间和轴）。它们都会先在 X 轴上旋转 90 度，因此在默认情况下不与世界轴对齐。使用在检视面板中公开的 xAngle、yAngle 和 zAngle 值可查看不同的旋转值如何应用于两个立方体。可能会注意到，立方体直观旋转的方式取决于使用的当前方向和空间选项。在 Scene 视图中选择立方体时尝试设置不同值，以尝试了解这些值如何交互。

```
using UnityEngine;// Transform.Rotate example
//
// This script creates two different cubes: one red which is rotated using Space.Self; one green which is rotated using Space.World.
// Add it onto any GameObject in a scene and hit play to see it run. The rotation is controlled using xAngle, yAngle and zAngle, modifiable on the inspector.public class ExampleScript : MonoBehaviour
{
    public float xAngle, yAngle, zAngle;    private GameObject cube1, cube2;    void Awake()
    {
        cube1 = GameObject.CreatePrimitive(PrimitiveType.Cube);
        cube1.transform.position = new Vector3(0.75f, 0.0f, 0.0f);
        cube1.transform.Rotate(90.0f, 0.0f, 0.0f, Space.Self);
        cube1.GetComponent<Renderer>().material.color = Color.red;
        cube1.name = "Self";        cube2 = GameObject.CreatePrimitive(PrimitiveType.Cube);
        cube2.transform.position = new Vector3(-0.75f, 0.0f, 0.0f);
        cube2.transform.Rotate(90.0f, 0.0f, 0.0f, Space.World);
        cube2.GetComponent<Renderer>().material.color = Color.green;
        cube2.name = "World";
    }    void Update()
    {
        cube1.transform.Rotate(xAngle, yAngle, zAngle, Space.Self);
        cube2.transform.Rotate(xAngle, yAngle, zAngle, Space.World);
    }
}
```

***

**2.public void Rotate (**[**Vector3**](https://docs.unity3d.com/cn/2020.2/ScriptReference/Vector3.html) **axis, float angle,** [**Space**](https://docs.unity3d.com/cn/2020.2/ScriptReference/Space.html) **relativeTo= Space.Self);**

### 参数

| angle      | 要应用的旋转度数。                      |
| ---------- | ------------------------------ |
| axis       | 要应用旋转的轴。                       |
| relativeTo | 确定在游戏对象本地还是相对于世界空间中的场景来旋转游戏对象。 |

### 描述

用给定角度定义的度数围绕给定轴旋转该对象。

旋转具有轴、角度以及本地或全局参数。旋转轴可以为任何方向。

***

public void Rotate ([Vector3](https://docs.unity3d.com/cn/2020.2/ScriptReference/Vector3.html) eulers);

### 参数

### 描述

应用一个围绕 Z 轴旋转 eulerAngles.z 度、围绕 X 轴旋转 eulerAngles.x 度、围绕 Y 轴旋转 eulerAngles.y 度（按此顺序）的旋转。

The rotation is relative to the GameObject's local space ([Space.Self](https://docs.unity3d.com/cn/2020.2/ScriptReference/Space.Self.html)).



例：

```
this.transform.Rotate(new Vector3(angleX, angleY, angleZ));
```

***

**3.public void Rotate (float xAngle, float yAngle, float zAngle);**

### 参数

| xAngle | 围绕 X 轴旋转游戏对象的度数。 |
| ------ | ---------------- |
| yAngle | 围绕 Y 轴旋转游戏对象的度数。 |
| zAngle | 围绕 Z 轴旋转游戏对象的度数。 |

### 描述

此方法的实现应用一个围绕 Z 轴旋转 `zAngle` 度、围绕 X 轴旋转 `xAngle` 度、围绕 Y 轴旋转 `yAngle` 度（按此顺序）的旋转。

The rotation is relative to the GameObject's local space ([Space.Self](https://docs.unity3d.com/cn/2020.2/ScriptReference/Space.Self.html)).

***

public void Rotate ([Vector3](https://docs.unity3d.com/cn/2020.2/ScriptReference/Vector3.html) axis, float angle);

### 参数

| axis  | 要应用旋转的轴。  |
| ----- | --------- |
| angle | 要应用的旋转度数。 |

### 描述

用给定角度定义的度数围绕给定轴旋转该对象。

Rotate has an axis, angle and the local or global parameters. The rotation axis can be in any direction. The rotation is relative to the GameObject's local space ([Space.Self](https://docs.unity3d.com/cn/2020.2/ScriptReference/Space.Self.html)).

## 二、FromToRotation

例：

```
    //enemyPos是敌人位置，transform.position是自身位置，这段代码产生一个从玩家到敌人的旋转角度。
    Vector3 v = enemyPos - transform.position;
    v.z = 0;
    Quaternion rotation = Quaternion.FromToRotation(Vector3.up, v);
    transform.rotation = rotation;
```

## 在ECS中的旋转范例

```
public partial class AimingAidSystem : SystemBase
{
    protected override void OnUpdate()
    {
        foreach (var (pp, lt, ltw, e) in SystemAPI.Query<RefRW<ProjectilePreview>, RefRW<LocalTransform>, RefRO<LocalToWorld>>().WithEntityAccess())
        {
            DeployController.CheckGroundPosition(Camera.main, out var hitPos);
            //var apexHeight = math.min(10, math.distance(ltw.ValueRO.Position, hitPos) / 2);

            lt.ValueRW.Rotation = Quaternion.LookRotation(((float3)hitPos - ltw.ValueRO.Position), Vector3.up);
        }
    }
}
```

在使用API时，要注意到每个坐标位置是否是统一的坐标系，最好都使用世界坐标。

<pre><code><strong> transform.rotation = Quaternion.LookRotation(targetPos- transform.position, Vector3.up);
</strong></code></pre>

## 旋转物体的方式

2D

`A`

`Vector3 dir = target.position - transform.position;`&#x20;

`float angle = Mathf.Atan2(dir.y,dir.x) * Mathf.Rad2Deg;`&#x20;

`transform.rotation = Quaternion.AngleAxis(angle, Vector3.forward);`

`B`

```
    Vector3 diff = Camera.main.ScreenToWorldPoint(Input.mousePosition) - transform.position;
    diff.Normalize();

    float rot_z = Mathf.Atan2(diff.y, diff.x) * Mathf.Rad2Deg;
    transform.rotation = Quaternion.Euler(0f, 0f, rot_z - 90);
```

不要选错旋转轴

```
using UnityEngine;

public class CubeRotation : MonoBehaviour
{
    public float forwardRotationAngle = -90;
    public float fRotationAngle = 0;
    private Quaternion r1;
    private Quaternion r2;

    private void Update()
    {
        r1 = Quaternion.AngleAxis(forwardRotationAngle, Vector3.right);
        r2 = Quaternion.AngleAxis(fRotationAngle, transform.forward);
        //这样写是错误的，因为r2获得的旋转是基于 transform.forward，而还没有经过r1旋转之前的transform有别于已经经过r1旋转的rotation
        transform.rotation = r1 * r2;
        Debug.Log("transform.forward:" + transform.forward);
    }

///以下是正确写法
    //// 初始X轴旋转角度
    //public float initialXRotation = 10.0f;

    //// 自身Z轴每帧旋转的角度
    //public float zRotationPerFrame = 2.0f;

    //void Start()
    //{
    //    // 初始时绕X轴旋转
    //    Quaternion initialRotation = Quaternion.AngleAxis(initialXRotation, Vector3.right);
    //    transform.rotation = initialRotation;
    //}

    //void Update()
    //{
    //    // 在每帧中绕Z轴旋转
    //    Quaternion zRotation = Quaternion.AngleAxis(zRotationPerFrame, transform.forward);
    //    transform.rotation *= zRotation;
    //}

}

```

## 参考资料

1. 官网 [https://docs.unity3d.com/cn/2020.2/ScriptReference/Transform.Rotate.html](https://docs.unity3d.com/cn/2020.2/ScriptReference/Transform.Rotate.html)
2. [https://discussions.unity.com/t/lookat-2d-equivalent/88118/12](https://discussions.unity.com/t/lookat-2d-equivalent/88118/12)
