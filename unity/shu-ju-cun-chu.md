# 数据存储

Unity的Vector3数据类型无法序列化保存数据，所以如果要序列化一个三维向量，需要自己定义可序列化的三维向量数据类型。

目前LittleWorld中采用三级数据结构存储跨场景信息

GameSave-GameObjectSave-SceneSave

GameSave对应单个游戏存档，GameObjectSave对应了在游戏存档中应用了ISaveable接口的游戏物体，SceneSave对应了GameObjectSave在单个场景中存储的信息。

所有需要存储的类都必须应用ISaveable接口。

每个物体对应的ID使用`System.Guid.NewGuid().ToString()`在编辑器中生成。

目前的存储结构是多个存档物体用同一种数据存储结构，每一个存档物体都有一份自己的数据实例，在该实例中每个物体只读取/存储该结构与自己相关的部分，造成了大量的空间浪费，后期应修改。
