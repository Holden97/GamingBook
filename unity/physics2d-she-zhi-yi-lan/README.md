# Physics2D设置一览

Gravity

重力设置

Default Material

默认2D物理材质

Velocity lterations

设置物理引擎为处理速度影响而进行的迭代次数。数字越大，物理计算越准确，但代价是 CPU 时间增加。默认为8。

Position lterations

设置物理引擎为处理位置变化而进行的迭代次数。数字越大，物理计算越准确，但代价是 CPU 时间增加。默认为3。

Velocity Threshold

弹性碰撞的阈值。Unity 将相对速度低于此值的碰撞视为非弹性碰撞（即，碰撞的游戏对象不会相互反弹）。

Max Linear Correction

设置解算约束时使用的最大线性位置校正（范围从 0.0001 到 1000000）。这有助于防止过冲。

Max Angular Correction

设置解算约束时使用的最大角度校正（范围从 0.0001 到 1000000）。这有助于防止过冲。

Max Translation Speed

设置任何物理更新期间 2D 刚体游戏对象的最大线性速度。

Max Rotation Speed

设置任何物理更新期间 2D 刚体游戏对象的最大旋转速度。

Baumgarte Scale

设置用于确定 Unity 解算碰撞重叠速度的缩放因子。

Baumgarte Time Of lmpact Scale

设置用于确定 Unity 解算撞击时间重叠速度的缩放因子。

Time To Sleep

在 2D 刚体停止移动之后而进入睡眠状态之前必须经过的时间（以秒为单位）。

Linear Sleep Tolerance

设置线性睡眠速度，在低于该速度时，2D 刚体在经过 Time to Sleep 过后进入睡眠状态。

Angular Sleep Tolerance

设置旋转睡眠速度，在低于该速度时，2D 刚体在经过 Time to Sleep 过后进入睡眠状态。

Default Contact Offset

将碰撞体的接近距离值设置为相接触，即使它们实际上没有接触。

Simulation Mode

| Fixed Update | 选择此属性将使 Unity 在调用 [MonoBehaviour.FixedUpdate](https://docs.unity3d.com/ScriptReference/MonoBehaviour.FixedUpdate.html) 后立即执行物理模拟。 |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------- |
| Update       | 选择此属性将使 Unity 在调用 [MonoBehaviour.Update](https://docs.unity3d.com/ScriptReference/MonoBehaviour.Update.html) 后立即执行物理模拟。           |
| Script       | 选择此属性将通过[脚本](https://docs.unity3d.com/ScriptReference/Physics2D.Simulate.html)手动执行物理模拟。                                           |

Queries Hit Triggers

物理查询命中触发器

Queries Start In Colliders

物理查询命中起始碰撞器

Callbacks On Disable

设置Collider2D在被禁用并且与其他未被禁用的Collider2D碰撞时是否触发回调

注意：只有OnCollisionExit2D和OnTriggerExit2D可能被触发。

Reuse Collision Callbacks

启用此设置将使物理引擎对所有碰撞回调重用单个 Collision2D 实例。如果将其禁用，则物理引擎会为每个碰撞回调创建一个新的 Collision2D 实例。

当MonoBehaviour。OnCollisionEnter2D MonoBehaviour。OnCollisionStay2D或MonoBehaviour。OnCollisionExit2D碰撞回调发生时，为每个单独的回调创建传递给它的Collision2D对象。这意味着垃圾收集器必须删除每个对象，这降低了性能。

当此选项为true时，仅为每个回调创建和重用Collision2D类型的单个实例。这减少了垃圾收集器要处理的浪费，并提高了性能。

Auto Sync Transforms

当transform组件更改时，是否自动与物理系统同步转换更改。

当转换组件的变化,任何Rigidbody或对撞机变换或其孩子可能需要重新定位,旋转或缩放根据变化变换。通过将此属性设置为true，您可以控制对Transform所做的更改是否自动应用于正确的组件。当设置为false时，同步只发生在固定更新期间的物理模拟步骤之前。您还可以使用Physics.SyncTransforms手动同步转换更改。

&#x20;

注意:当autoSyncTransforms设置为true时，重复更改Transform然后执行物理查询可能会导致性能损失。为了避免影响性能，如果您想连续执行多个Transform更改和查询，请将autoSyncTransforms设置为false。在Unity 2017.2之前的现有项目中，你应该只将autoSyncTransforms设置为true以实现物理向后兼容性。对于Unity 2017.2以后的项目，关闭这个选项。

Job Options (Experimental)

这个设置短时间无法理解，先贴链接

https://www.youtube.com/watch?v=C56bbgtPr\_w\&t=21s

Gizmos

Gizmos中关于Collider的设置

Always Show Colliders

总是显示碰撞体

Show Collider Sleep

总是睡眠状态中的碰撞体

Collider Awake Color

清醒状态的碰撞体颜色

Collider Asleep Color

睡眠状态的碰撞体颜色

Show Collider Contacts

显示接触到的碰撞体

Contact Arrow Scale 不清楚

Collider Contact Color

接触到的碰撞体颜色

Show Collider AABB

显示碰撞体AABB盒

Collider AABB Color

碰撞体AABB盒颜色

Layer Collision Matrix

选择碰撞矩阵中的哪些层与其他层交互（勾选相应层即可）。

&#x20;

参考

1.https://docs.unity3d.com/cn/2021.2/Manual/class-Physics2DManager.html

2.https://docs.unity3d.com/ScriptReference/Physics-autoSyncTransforms.html

&#x20;

&#x20;

&#x20;
