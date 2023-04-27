# GOAP

Goap框架是一种以目标为导向，使用A\*算法倒推行为规划的一种思想。

在规划行为路径时，各个Action的Precondition和Effect中的状态并不影响当前认知状态，而在Sensor中修改的memory的世界状态才会实际的更改认知状态。

Sensor是AI对世界状态的认知，可以通过调整刷新间隔来调整AI对世界状态改变的灵敏程度。

## 举例

以近战怪物攻击玩家举例。

近战怪物A的**目标**是杀死玩家B，它有行走、攻击两种**行为。**那就可以对这一个目标和两种行为进行抽象。

目标\[Goal]：杀死玩家X

前置条件\[Pre]：无

效果\[Effect]：attacking X



行为\[Action]：行走

设置\[Setting]：从当前目标中找到"atTransform X"词条，将X作为目标点存储->targetTransform X

前置条件\[Pre]：无

运行\[Run]：执行A怪物蓝图的Run方法。

效果\[Effect]：atTransform X



行为\[Action]：攻击任意英雄

设置\[Setting]：以**玩家感知器**为参照，设置一个目标玩家作为攻击目标**X**

前置条件\[Pre]：atTransform **X**

运行\[Run]：执行A怪物蓝图的Run方法。

效果\[Effect]：attacking X

## 参考链接

1. Git Regoap库 [https://github.com/luxkun/ReGoap](https://github.com/luxkun/ReGoap)
