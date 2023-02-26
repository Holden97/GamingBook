# Canvas Group

整体调整子节点UI的属性。

在 Unity Raycasters 中有三种类型的 Raycasters:

* Graphic Raycaster - 存在于 Canvas 下，用于检测 Canvas 中所有的物体
* Physics 2D Raycaster - 用于检测 2D 物体
* Physics Raycaster - 用于检测 3D 物体

官方文档中

| **Block Raycasts** | 此组件是否作为射线投射的碰撞体？需要在连接到画布的图形射线投射器上调用 RayCast 函数。这\_不\_适用于 **Physics.Raycast**。 |
| ------------------ | ----------------------------------------------------------------------------- |

指的应该是如果在一个3D或2D的场景物体，而不是UI元素上添加Canvas Group的话，那么射线检测时，射线始终会穿过该物体，不受该选项设置的影响。
