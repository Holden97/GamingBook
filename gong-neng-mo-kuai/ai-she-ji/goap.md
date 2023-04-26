# GOAP

Goap框架是一种以目标为导向，使用A\*算法倒推行为规划的一种思想。

在规划行为路径时，各个Action的Precondition和Effect中的状态并不影响当前认知状态，而在Sensor中修改的memory的世界状态才会实际的更改认知状态。

Sensor是AI对世界状态的认知，可以通过调整刷新间隔来调整AI对世界状态改变的灵敏程度。

## 参考链接

1. Git Regoap库 [https://github.com/luxkun/ReGoap](https://github.com/luxkun/ReGoap)
