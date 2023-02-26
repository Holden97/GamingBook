# 报错及解决方式

`$"Scene {path} needs to be opened and resaved, because the scene object {identity.name} has no valid sceneId yet."`

有可能是因为物体创建后没有正确分配NetId，Spawn过程出错。

NetworkIdentity中的netId不正确导致强制退出。

删除不必要的NetworkIdentity。
